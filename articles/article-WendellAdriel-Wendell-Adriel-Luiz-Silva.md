# MongoDb - Artigo sobre Autenticação no MongoDb

**Autor**: Wendell Adriel Luiz Silva - [WendellAdriel](https://github.com/WendellAdriel)  
**Data**: 1454783812110

## Qual a diferença entre Autenticação e Autorização?

Esses dois conceitos estão ligados um ao outro, porém são diferentes:
- **Autenticação:** É fazer a verificação da identidade de alguém, confirmando se esse alguém é
realmente quem diz ser.
- **Autorização:** É fazer a verificação se o usuário tem permissão necessária para realizar alguma
ação ou acessar algum recurso (como acesso a rotas, arquivos, rotinas, funções, etc).

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Por padrão o `MongoDB` guarda todas as informações de usuários no Banco de dados `admin`, na
collection `system.users`, portanto o primeiro passo a fazer é se conectar ao `MongoDB` e acessar
esse Banco de Dados:

```js
// Iniciando o MongoDB no terminal
mongod
// Abra outro terminal e execute o comando abaixo para conectar ao MongoDB
mongo
// Acesse o Banco de Dados admin
use admin
```  

Para criar o usuário devemos utilizar a função `createUser`, passando um `JSON` com dados
de nosso usuário:
- **user:** Nome do usuário
- **pwd:** Senha do usuário
- **customData:** Atributos customizados (opcional)
- **roles:** Permissões do usuário

### Criando um usuário comum

Para criar um usuário comum podemos personalizar suas permissões passando `roles` como: `read`,
`readWrite`, etc. Veja como criar um usuário comum com permissão apenas de leitura:

```js
// Objeto com dados de nosso usuário comum
var user = {
  user : 'simpleUser',
  pwd : '123456',
  roles : [{ role : 'read', db : 'admin' }]
}
// Criando o usuário utilizando nosso objeto definido anteriormente
db.createUser(user)
```

### Criando um usuário administrador

Um usuário administrador tem acesso a diversas rotinas, podendo:
- Criar, modificar e deletar usuários
- Dar e retirar permissões de usuários
- etc

Para criar um usuário administrador, precisamos dar a `role` `userAdminAnyDatabase` a ele.
Veja como criar um usuário administrador:

```js
// Objeto com dados de nosso usuário administrador
var user = {
  user : 'adminUser',
  pwd : 'admin',
  roles : [{ role : 'userAdminAnyDatabase', db : 'admin' }]
}
// Criando o usuário utilizando nosso objeto definido anteriormente
db.createUser(user)
```

## Cluster Administration Roles.

O banco de dados `admin` possui diversas `roles` para administrar o sistema como um todo ao
invés de um único banco de dados. As `roles` abaixo podem ser usadas para administrar `shards` e
`replica sets`, porém não são exclusivas para esses casos:

- **`clusterAdmin`:** Acesso de mais alto nível ao gerenciamento de `clusters`. Essa `role` combina os
privilégios dados ao `clusterManager`, `clusterMonitor` e o `hostManager` e ainda dá acesso à ação `dropDatabase`.

- **`clusterManager`:** Provê ações de gerenciamento e monitoramento no `cluster`. Um usuário com essa `role` pode
acessar os banco de dados: `config` e `local` que são usados para fazer `sharding` e `replicação`, respectivamente.

- **`clusterMonitor`:** Provê acesso de leitura a ferramentas de monitoramento como o agente de monitoração
**MongoDB Cloud Manager**.

- **`hostManager`:** Provê a habilidade de gerenciar e monitorar servidores.

Leia um pouco mais sobre essas `roles` [aqui](https://docs.mongodb.org/v3.2/reference/built-in-roles/#cluster-administration-roles).

## Privilege Actions.

As ações de privilégio definem quais operações um usuário pode realizar em algum recurso (banco de dados, collection, cluster, etc).
Veja abaixo a lista com as ações disponíveis e o(s) recurso(s) que cada uma pode trabalhar:

### Ações de pesquisa e escrita

- **`find`:** Usuário pode executar o método `db.collection.find()`. Pode ser aplicado em uma collection ou um banco de dados.
- **`insert`:** Usuário pode executar o comando `insert`. Pode ser aplicado em uma collection ou um banco de dados.
- **`remove`:** Usuário pode executar o método `db.collection.remove()`. Pode ser aplicado em uma collection ou um banco de dados.
- **`update`:** Usuário pode executar o comando `update`. Pode ser aplicado em uma collection ou um banco de dados.
- **`bypassDocumentValidation`:** ***Disponível a partir da versão 3.2***. Usuário pode pular a validação de documentos. Pode ser aplicado em uma collection ou um banco de dados.

### Ações de gerenciamento de banco de dados

- **`changeCustomData`:** Usuário pode alterar o campo `customData` de qualquer usuário em um banco de dados específico. Pode ser aplicado em um banco de dados.
- **`changeOwnCustomData`:** Usuário pode alterar seu próprio campo `customData`. Pode ser aplicado em um banco de dados.
- **`changePassword`:** Usuário pode alterar a senha de qualquer usuário em um banco de dados específico. Pode ser aplicado em um banco de dados.
- **`changeOwnPassword`:** Usuário pode alterar sua própria senha. Pode ser aplicado em um banco de dados.
- **`createCollection`:** Usuário pode executar o método `db.createCollection()`. Pode ser aplicado em uma collection ou um banco de dados.
- **`createIndex`:** Provê acesso ao método `db.collection.createIndex()` e o comando `createIndexes`. Pode ser aplicado em uma collection ou um banco de dados.
- **`createRole`:** Usuário pode criar novas `roles` em um banco de dados específico. Pode ser aplicado em um banco de dados.
- **`createUser`:** Usuário pode criar novos usuários em um banco de dados específico. Pode ser aplicado em um banco de dados.
- **`dropCollection`:** Usuário pode executar o método `db.collection.drop()`. Pode ser aplicado em uma collection ou em um banco de dados.
- **`dropRole`:** Usuário pode deletar qualquer `role` em um banco de dados específico. Pode ser aplicado em um banco de dados.
- **`dropUser`:** Usuário pode remover qualquer usuário em um banco de dados específico. Pode ser aplicado em um banco de dados.
- **`emptycapped`:** Usuário pode executar o comando `emptycapped`. Pode ser aplicado em uma collection ou um banco de dados.
- **`enableProfiler`:** Usuário pode executar o método `db.setProfilingLevel()`. Pode ser aplicado em um banco de dados.
- **`grantRole`:** Usuário pode dar uma `role` em um banco de dados específico para qualquer usuário de qualquer banco de dados do sistema. Pode ser aplicado em um banco de dados.
- **`killCursors`:** Usuário pode matar cursores na em uma coleção.
- **`revokeRole`:** Usuário pode remover qualquer `role` de qualquer usuário em qualquer banco de dados. Pode ser aplicado em um banco de dados.
- **`unlock`:** Usuário pode executar o método `db.fsyncUnlock()`. Pode ser aplicado em um `cluster`.
- **`viewRole`:** Usuário pode ver informações sobre qualquer `role` em um banco de dados específico. Pode ser aplicado em um banco de dados.
- **`viewUser`:** Usuário pode ver informações sobre qualquer usuário em um banco de dados específico. Pode ser aplicado em um banco de dados.

### Ações de gerencimanto de implantação

- **`authSchemaUpgrade`:** Usuário pode executar o comando `authSchemaUpgrade`. Pode ser aplicado em um `cluster`.
- **`cleanupOrphaned`:** Usuário pode executar o comando `cleanupOrphaned`. Pode ser aplicado em um `cluster`.
- **`cpuProfiler`:** Usuário pode habilitar e utilizar o CPU profiler. Pode ser aplicado em um `cluster`.
- **`inprog`:** Usuário pode executar o método `db.currentOp()` para retornar operações ativas e pendentes. Pode ser aplicado em um `cluster`.
- **`invalidateUserCache`:** Provê acesso ao comando `invalidateUserCache`. Pode ser aplicado em um `cluster`.
- **`killop`:** Usuário pode executar o método `db.killOp()`. Pode ser aplicado em um `cluster`.
- **`planCacheRead`:** Usuário pode executar os comandos: `planCacheListPlans` e `planCacheListQueryShapes` e os métodos: `PlanCache.getPlansByQuery()` e `PlanCache.listQueryShapes()`. Pode ser aplicado em uma collection ou um banco de dados.
- **`planCacheWrite`:** Usuário pode executar o comando `planCacheClear` e os métodos: `PlanCache.clear()` e `PlanCache.clearPlansByQuery()`. Pode ser aplicado em uma collection ou um banco de dados.
- **`storageDetails`:** Usuário pode executar o comando `storageDetails`. Pode ser aplicado em uma collection ou um banco de dados.

### Ações de replicação

- **`appendOplogNote`:** Usuários podem adicionar notas ao oplog. Pode ser aplicado em um `cluster`.
- **`replSetConfigure`:** Usuário pode configurar um `replica set`. Pode ser aplicado em um `cluster`.
- **`replSetGetStatus`:** Usuário pode executar o comando `replSetGetStatus`. Pode ser aplicado em um `cluster`.
- **`replSetHeartbeat`:** Usuário pode executar o comando `replSetHeartbeat`. Pode ser aplicado em um `cluster`.
- **`replSetStateChange`:** Usuário pode mudar o estado de um `replica set` pelos comandos: `replSetFreeze`, `replSetMaintenance`, `replSetStepDown` e `replSetSyncFrom`. Pode ser aplicado em um `cluster`.
- **`resync`:** Usuário pode executar o comando `resync`. Pode ser aplicado em um `cluster`.

### Ações de sharding

- **`addShard`:** Usuário pode executar o comando `addShard`. Pode ser aplicado em um `cluster`.
- **`enableSharding`:** Usuário pode habilitar o sharding em um banco de dados utilizando o comando `enableSharding` e em uma collection utilizando o comando `shardCollection`. Pode ser aplicado em uma collection ou um banco de dados.
- **`flushRouterConifg`:** Usuário pode executar o comando `flushRouterConifg`. Pode ser aplicado em um `cluster`.
- **`getShardMap`:** Usuário pode executar o comando `getShardMap`. Pode ser aplicado em um `cluster`.
- **`getShardVersion`:** Usuário pode executar o comando `getShardVersion`. Pode ser aplicado em um banco de dados.
- **`listShards`:** Usuário pode executar o comando `listShards`. Pode ser aplicado em um `cluster`.
- **`moveChunk`:** Usuário pode executar o comando `moveChunk`. Usuário também pode executar o comando `movePrimary` se o privilégio estiver aplicado a um banco de dados apropriado. Pode ser aplicado em uma collection ou um banco de dados.
- **`removeShard`:** Usuário pode executar o comando `removeShard`. Pode ser aplicado em um `cluster`.
- **`shardingState`:** Usuário pode executar o comando `shardingState`. Pode ser aplicado em um `cluster`.
- **`splitChunk`:** Usuário pode executar o comando `splitChunk`. Pode ser aplicado em uma collection ou um banco de dados.
- **`splitVector`:** Usuário pode executar o comando `splitVector`. Pode ser aplicado em uma collection ou um banco de dados.

### Ações de gerenciamento de servidor

- **`applicationMessage`:** Usuário pode executar o comando `logApplicationMessage`. Pode ser aplicado em um `cluster`.
- **`closeAllDatabases`:** Usuário pode executar o comando `closeAllDatabases`. Pode ser aplicado em um `cluster`.
- **`collMod`:** Usuário pode executar o comando `collMod`. Pode ser aplicado em uma collection ou um banco de dados.
- **`compact`:** Usuário pode executar o comando `compact`. Pode ser aplicado em uma collection ou um banco de dados.
- **`connPoolSync`:** Usuário pode executar o comando `connPoolSync`. Pode ser aplicado em um `cluster`.
- **`convertToCapped`:** Usuário pode executar o comando `convertToCapped`. Pode ser aplicado em uma collection ou um banco de dados.
- **`dropDatabase`:** Usuário pode executar o comando `dropDatabase`. Pode ser aplicado em um banco de dados.
- **`dropIndex`:** Usuário pode executar o comando `dropIndexes`. Pode ser aplicado em uma collection ou um banco de dados.
- **`fsync`:** Usuário pode executar o comando `fsync`. Pode ser aplicado em um `cluster`.
- **`getParameter`:** Usuário pode executar o comando `getParameter`. Pode ser aplicado em um `cluster`.
- **`hostInfo`:** Provê informações sobre o servidor que a instância do MongoDB está sendo executada. Pode ser aplicado em um `cluster`.
- **`logRotate`:** Usuário pode executar o comando `logRotate`. Pode ser aplicado em um `cluster`.
- **`reIndex`:** Usuário pode executar o comando `reIndex`. Pode ser aplicado em uma collection ou um banco de dados.
- **`renameCollectionSameDB`:** Permite ao usuário renomear collections no banco de dados atual utilizando o comando `renameCollection`. O usuário também **deve ter** a permissão de `find` na collection de origem **ou não ter** a permissão de `find` na collection de destino. Se a collection com o novo nome já existir, o usuário deve também ter a permissão de `dropCollection` na collection de destino. Pode ser aplicado em um banco de dados.
- **`repairDatabase`:** Usuário pode executar o comando `repairDatabase`. Pode ser aplicado em um banco de dados.
- **`setParameter`:** Usuário pode executar o comando `setParameter`. Pode ser aplicado em um `cluster`.
- **`shutdown`:** Usuário pode executar o comando `shutdown`. Pode ser aplicado em um `cluster`.
- **`touch`:** Usuário pode executar o comando `touch`. Pode ser aplicado em um `cluster`.

### Ações de diagnósticos

- **`collStats`:** Usuário pode executar o comando `collStats`. Pode ser aplicado em uma collection ou um banco de dados.
- **`connPoolStats`:** Usuário pode executar os comandos: `connPoolStats` e `shardPoolStats`. Pode ser aplicado em um `cluster`.
- **`cursorInfo`:** Usuário pode executar o comando `cursorInfo`. Pode ser aplicado em um `cluster`.
- **`dbHash`:** Usuário pode executar o comando `dbHash`. Pode ser aplicado em uma collection ou um banco de dados.
- **`dbStats`:** Usuário pode executar o comando `dbStats`. Pode ser aplicado em um banco de dados.
- **`diagLogging`:** Usuário pode executar o comando `diagLogging`. Pode ser aplicado em um `cluster`.
- **`getCmdLineOpts`:** Usuário pode executar o comando `getCmdLineOpts`. Pode ser aplicado em um `cluster`.
- **`getLog`:** Usuário pode executar o comando `getLog`. Pode ser aplicado em um `cluster`.
- **`indexStats`:** Usuário pode executar o comando `indexStats`. Pode ser aplicado em uma collection ou um banco de dados. ***Alterado na versão 3.0 do MongoDB: MongoDB 3.0 removeu o comando*** `indexStats`.
- **`listDatabases`:** Usuário pode executar o comando `listDatabases`. Pode ser aplicado em um `cluster`.
- **`listCollections`:** Usuário pode executar o comando `listCollections`. Pode ser aplicado em um banco de dados.
- **`listIndexes`:** Usuário pode executar o comando `listIndexes`. Pode ser aplicado em uma collection ou um banco de dados.
- **`netstat`:** Usuário pode executar o comando `netstat`. Pode ser aplicado em um `cluster`.
- **`serverStatus`:** Usuário pode executar o comando `serverStatus`. Pode ser aplicado em um `cluster`.
- **`validate`:** Usuário pode executar o comando `validate`. Pode ser aplicado em uma collection ou um banco de dados.
- **`top`:** Usuário pode executar o comando `top`. Pode ser aplicado em um `cluster`.

### Ações internas

- **`anyAction`:** Permite qualquer ação em um recurso. **Não atribua essa permissão, exceto em circustâncias especiais!**
- **`internal`:** Permite ações internas. **Não atribua essa permissão, exceto em circustâncias especiais!**

Leia um pouco mais sobre privilégios [aqui](https://docs.mongodb.org/manual/reference/privilege-actions/).
