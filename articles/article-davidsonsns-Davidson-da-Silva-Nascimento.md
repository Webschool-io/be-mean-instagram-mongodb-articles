# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Davidson da Silva Nascimento

**User:** davidsonsns

**Data:** 1449925803365

## Qual a diferença entre Autenticação e Autorização?
#### Autenticação
Verificação das credenciais da tentativa de conexão, o usuário prova quem realmente ele é. 

Geralmente envolve a utilização de um usuário e senha, mas pode incluir outros métodos de validação da identidade.

Esse processo consiste no envio de credenciais do cliente de acesso remoto para o servidor de acesso remoto em um formato de texto sem formatação ou em formato criptografado usando um protocolo de autenticação. 
#### Autorização
Verifica se uma determinada credencial, **uma vez identificada (e com autenticação bem sucedidad)**, tem permissão para manipular recursos específicos. 

Utilizado, por exemplo, para descobrir se uma determinada pessoa previamente autenticado possui permissão para usar, manipular ou executar o recurso em questão.
## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
É possível criar usuários através de dois comandos, o `createUser` e o `runCommand`. Os papeis concedidos aos usuário a serem inseridos são especificados no campo `roles`. 

Para especificar um papel que existe no mesmo banco de dados onde o comando é executado, você pode especificar o papel com o nome da função:

`"readWrite"`

Ou você pode especificar a função com um documento, como em:

`{ role : "<role>", db : "<database>" }`

Para especificar um papel que existe em um banco de dados diferente, especifique o papel com um documento.

> Pode especificar uma matriz vazia `[]` para criar usuários sem papéis, caso contrário será necessário informar seu papel.

Seguem abaixo exemplos realizando a inserção de usuário administradores e comuns, utilizando os dois métodos:
#### Usuário administrador
```js
use admin
db.createUser(
  {
    user: "DavidsonAdmin",
    pwd: "admin123",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)

db.runCommand( { createUser: "davidson",
  pwd: "adminRoot",
  customData: { teacher: true },
  roles: [
     { role: "clusterAdmin", db: "admin" },
     { role: "readAnyDatabase", db: "admin" },
     "readWrite"
   ],
  writeConcern: { w: "majority" , wtimeout: 5000 }
} )
```
#### Usuário comum
```js
use admin
db.createUser(
  {
    user: "DavidsonAdminComum",
    pwd: "admin123",
    roles: []
  }
)

db.runCommand( { createUser: "davidsonComum",
  pwd: "adminRoot",
  customData: { teacher: true },
  roles: [],
  writeConcern: { w: "majority" , wtimeout: 5000 }
} )
```
## Explique cada papel listado em Cluster Administration Roles.
#### `clusterAdmin`
Fornece o maior acesso de gerenciamento de cluster. Este papel combina os privilégios concedidos pelos `clusterManager`, `clusterMonitor`, e `hostManager` papéis. Além disso, o papel fornece a [dropDatabase](https://docs.mongodb.org/manual/reference/privilege-actions/#authr.dropDatabase) ação.
#### `clusterManager`
Fornece gerenciamento e ações de monitoramento no cluster. Um usuário com essa função pode acessar a configuração e locais bancos de dados, que são usados em sharding e replicação, respectivamente.
#### `clusterMonitor`
Fornece acesso somente leitura para ferramentas de monitoramento, como o [MongoDB Cloud Manager](https://cloud.mongodb.com/?jmp=docs&_ga=1.6569329.291542856.1448632219) e [Ops Manager](https://docs.opsmanager.mongodb.com/current/?_ga=1.6569329.291542856.1448632219) agente de monitoramento.
#### `hostManager`
Fornece a capacidade de monitorar e gerenciar servidores.
## Explique todas as ações de privilégio listadas em Privilege Actions.
Definem as operações que um usuário pode executar em um recurso.

Abaixo estão listadas as ações disponíveis agrupadas por propósito em comum:
### Consulta e gravação
#### `find`
O usuário pode executar o método [db.collection.find()](https://docs.mongodb.org/manual/reference/method/db.collection.find/#db.collection.find). Aplica-se para `database` ou `collection`.
#### `insert`
O usuário pode executar o comando [insert](https://docs.mongodb.org/manual/reference/command/insert/#dbcmd.insert). Aplica-se para `database` ou `collection`.
#### `remove`
O usuário pode executar o método [db.collection.remove()](https://docs.mongodb.org/manual/reference/method/db.collection.remove/#db.collection.remove). Aplica-se para `database` ou `collection`.
#### `update`
O usuário pode executar o comando [update](https://docs.mongodb.org/manual/reference/command/update/#dbcmd.update). Aplica-se para `database` ou `collection`.
#### `bypassDocumentValidation`
O usuário pode ignorar a validação de documentos sobre os comandos que suportam a opção `bypassDocumentValidation`. Para obter uma lista de comandos que suportam a opção `bypassDocumentValidation`, veja [Document Validation](https://docs.mongodb.org/manual/release-notes/3.2/#rel-notes-document-validation). Aplicar esta ação para banco de dados ou de cobrança de recursos.
### Database Management Actions
#### `changeCustomData`
O usuário pode alterar uma informação aleatória de qualquer usuário da database. Aplica-se para `database`.
#### `changeOwnCustomData`
Usuários podem alterar as alterações deles próprios. Aplica-se para `database`.
#### `changePassword`
O usuário poderá alterar o password dele na database. Aplica-se para `database`.
#### `createCollection`
O usuário pode executar o método [db.createCollection()](https://docs.mongodb.org/manual/reference/method/db.createCollection/#db.createCollection). Aplica-se para `database` ou `collection`.
#### `createIndex`
Permite acesso para o método [db.collection.createIndex()](https://docs.mongodb.org/manual/reference/method/db.collection.createIndex/#db.collection.createIndex) e para o comando [createIndexes](https://docs.mongodb.org/manual/reference/command/createIndexes/#dbcmd.createIndexes). Aplica-se para `database` ou `collection`.
#### `createRole`
O usuário pode criar uma nova role na database. Aplica-se para `database`.
#### `createUser`
O usuário pode criar novos usuários para a database. Aplica-se para `database`.
#### `dropCollection`
O usuário pode executar o método [db.collection.drop()](https://docs.mongodb.org/manual/reference/method/db.collection.drop/#db.collection.drop). Aplica-se para `database` ou `collection`.
#### `dropRole`
O usuário pode deletar qualquer `role` na `database`. Aplica-se para `database`.
#### `dropUser`
Usuário pode remover qualquer usuário da `database`. Aplica-se para `database`.
#### `emptycapped`
O usuário poderá executar o comando [emptycapped](https://docs.mongodb.org/manual/reference/command/emptycapped/#dbcmd.emptycapped). Aplica-se para `database` ou `collection`.
#### `enableProfiler`
O usuário poderá executar o método [db.setProfilingLevel()](https://docs.mongodb.org/manual/reference/method/db.setProfilingLevel/#db.setProfilingLevel). Aplica-se para `database` ou `collection`.
#### `grantRole`
O usuário poderá conceder qualquer `role` na database para qualquer usuário de qualquer `database` no sistema. Aplica-se para `database`.
#### `killCursors`
O usuário pode matar cursores em determinadas coleções.
#### `revokeRole`
O usuário pode remover qualquer `role` de qualquer usuário, em qualquer `database` no sistema. Aplica-se para `database` ou `collection`.
#### `unlock`
O usuário pode executar o método [db.fsyncUnlock()](https://docs.mongodb.org/manual/reference/method/db.fsyncUnlock/#db.fsyncUnlock). Aplica-se para `cluster`.
#### `viewRole`
O usuário pode visualizar informações sobre qualquer `role` da `database`. Aplica-se para `database`.
#### `viewUser`
O usuário pode visualizar informações de qualquer usuário da `database`. Aplica-se para `database`.
### Deployment Management Actions
#### `authSchemaUpgrade`
O usuário poderá executar o comando [authSchemaUpdate](https://docs.mongodb.org/manual/reference/command/authSchemaUpgrade/#dbcmd.authSchemaUpgrade). Aplica-se para `cluster`.
#### `cleanupOrphaned`
O usuário poderá executar o comando [cleanupOrphaned](https://docs.mongodb.org/manual/reference/command/cleanupOrphaned/#dbcmd.cleanupOrphaned). Aplica-se para `cluster`.
#### `cpuProfiler`
O usuário poderá habilitar e utilizar o profiler da CPU. Aplica-se para `cluster`.
#### `inprog`
O usuário poderá utilizar o método [db.currentOp()](https://docs.mongodb.org/manual/reference/method/db.currentOp/#db.currentOp) para retornar as operações ativas e as operações pendentes. Aplica-se para `cluster`.
#### `invalidateUserCache`
Permite acesso ao comando [invalidateUserCache](https://docs.mongodb.org/manual/reference/command/invalidateUserCache/#dbcmd.invalidateUserCache). Aplica-se para `cluster`.
#### `killop`
O usuário poderá executar o método [db.killOp()](https://docs.mongodb.org/manual/reference/method/db.killOp/#db.killOp). Aplica-se para `cluster`.
#### `planCacheRead`
O usuário poderá executar os comandos [planCacheListPlans](https://docs.mongodb.org/manual/reference/command/planCacheListPlans/#dbcmd.planCacheListPlans), [planCacheListQueryShapes](https://docs.mongodb.org/manual/reference/command/planCacheListQueryShapes/#dbcmd.planCacheListQueryShapes) e os seguintes métodos: [PlanCache.getPlansByQuery()](https://docs.mongodb.org/manual/reference/method/PlanCache.getPlansByQuery/#PlanCache.getPlansByQuery), [PlanCache.listQueryByShapes()](https://docs.mongodb.org/manual/reference/method/PlanCache.listQueryShapes/#PlanCache.listQueryShapes). Aplica-se para `database` ou `collection`.
#### `planCacheWrite`
O usuário poderá executar o comando [planCacheClear](https://docs.mongodb.org/manual/reference/command/planCacheClear/#dbcmd.planCacheClear) e os métodos [PlanCache.clear()](https://docs.mongodb.org/manual/reference/method/PlanCache.clear/#PlanCache.clear) [PlanCache.clearPlansByQuery()](https://docs.mongodb.org/manual/reference/method/PlanCache.clearPlansByQuery/#PlanCache.clearPlansByQuery). Aplica-se para `database` ou `collection`.
#### `storageDetails`
O usuário poderá executar o comando `storageDetails`. Aplica-se para `database` ou `collection`.



### Replication Actions
#### `appendOplogNote`
O usuário poderá acrescentar notas ao oplog. Aplica-se para `cluster`.
#### `replSetConfigure`
O usuário poderá configurar o replica set. Aplica-se para `cluster`.
#### `replSetGetStatus`
O usuário poderá executar o comando `replSetGetStatus`. Aplica-se para `cluster`.
#### `replSetHeartbeat`
O usuário poderá executar o comando `replSetHeartbeat`. Aplica-se para `cluster`.
#### `replSetStateChange`
O usuário poderá alterar o estado da replica set através dos comandos: `replSetFreeze`, `replSetMaintence`, `replSetStepDown` e `replSetSyncFrom`. Aplica-se para `cluster`.
#### `resync`
O usuário poderá executar o comando `resync`. Aplica-se para `cluster`.

### Sharding Actions
#### `addShard`
O usuário poderá executar o comando addShard. Aplica-se para `cluster`.
#### `enableSharding`
O usuário poderá habilitar sharding na database utilizando o comando enableSharding e poderá fazer shard em collections utilizando o comando shardCollection. Aplica-se para `database` ou `collection`.
#### `flushRouterConfig`
O usuário poderá executar o comando `flushRouterConfig`. Aplica-se para `cluster`.
#### `getShardMap`
O usuário poderá executar o comando `getShardMap` Aplica-se para `cluster`.
#### `getShardVersion`
O usuário poderá executar o comando `getShardVersion`. Aplica-se para `database`.
#### `listShards`
O usuário poderá executar o comando `listshards`. Aplica-se para `cluster`.
#### `moveChunk`
O usuário pode executar o comando `moveChunk`. Além disso, o usuário também poderá executar o comando movePrimary deste que o privilégio esteja aplicado à database apropriada.
#### `removeShard`
O usuário poderá executar o comando `removeShard`. Aplica-se para `cluster`.
#### `shardingState`
O usuário poderá executar o comando `shardingState`. Aplica-se para `cluster`.
#### `splitChunk`
O usuário poderá executar o comando `splitChunk`. Aplica-se para `database` ou `collection`.
#### `splitVector`
O usuário poderá executar o comando `splitVector`. Aplica-se para `database` ou `collection`.

### Server Administration Actions
#### `applicationMessage`
O usuário poderá executar o comando `logApplicationMessage`. Aplica-se para `cluster`.
#### `closeAllDatabases`
O usuário poderá executar o comando `closeAllDatabases`. Aplica-se para `cluster`.
#### `collMo`
O usuário poderá executar o comando `collMod`. Aplica-se para `database` ou `collection`.
#### `compact`
O usuário poderá executar o comando `compact`. Aplica-se para `database` ou `collection`.
#### `connPoolSync`
O usuário poderá executar o comando `connPoolSync`. Aplica-se para `cluster`.
#### `convertToCapped`
O usuário poderá executar o comando `convertToCapped`. Aplica-se para `database` ou `collection`.
#### `dropDatabase`
O usuário poderá executar o comando `dropDatabase`. Aplica-se para `database`.
#### `dropIndex`
O usuário poderá executar o comando `dropIndexes`. Aplica-se para `cluster`.
#### `fsync`
O usuário poderá executar o comando `fsync`. Aplica-se para `cluster`.
#### `getParameter`
O usuário poderá executar o comando `getParameter`. Aplica-se para `cluster`.
#### `hostInfo`
Provê informações sobre a instância do servidor de MongoDB. Aplica-se para `cluster`.
#### `logRotate`
O usuário poderá executar o comando `logRotate`. Aplica-se para `cluster`.
#### `reIndex`
O usuário poderá executar o comando `reIndex`. Aplica-se para `database` ou `collection`.
#### `renameCollectionSameDB`
Permite que o usuário possa renomear `collections` da `database` atual utilizando o comando `renameCollection`. Aplica-se para `database`.
#### `repairDatabase`
O usuário poderá executar o comando `repairDatabase`. Aplica-se para `database`.
#### `setParameter`
O usuário poderá executar o comando `setParameter`. Aplica-se para `cluster`.
#### `shutdown`
O usuário poderá executar o comando `shutdown`. Aplica-se para `cluster`.
#### `touch`
O usuário poderá executar o comando `touch`. Aplica-se para `cluster`.

### Diagnostic Actions
#### `collStats`
O usuário poderá executar o comando `collStats`. Aplica-se para `database` ou `collection`.
#### `connPoolStats`
O usuário poderá executar os comandos `connPoolStats` e `shardconnPoolStats`. Aplica-se para `cluster`.
#### `cursosInfo`
O usuário poderá executar o comando `cursorInfo`. Aplica-se para `cluster`.
#### `dbHash`
O usuário poderá executar o comando `dbHash`. Aplica-se para `database` ou `collection`.
#### `dbStats`
O usuário poderá executar o comando `dbStats`. Aplica-se para `database`.
#### `diagLogging`
O usuário poderá executar o comando `diagLogging`. Aplica-se para `cluster`.
#### `getCmdLineOpts`
O usuário poderá executar o comando `getCmdLineOpts`. Aplica-se para `cluster`.
#### `getLog`
O usuário poderá executar o comando `getLog`. Aplica-se para `cluster`.
#### `indexStats`
O usuário poderá executar o comando `indexStats`. Aplica-se para `database` ou `collection`. Alteração no MongoDB 3.0: MongoDB 3.0 removou o comando indexStats.
#### `listDatabases`
O usuário poderá executar o comando `listDatabases`. Aplica-se para `cluster`.
#### `listCollections`
O usuário poderá executar o comando `listCollections`. Aplica-se para `database`.
#### `listIndexes`
O usuário poderá executar o comando `ListIndexes`. Aplica-se para `database` ou `collection`.
#### `netstat`
O usuário poderá executar o comando `netstat`. Aplica-se para `cluster`.
#### `serverStatus`
O usuário poderá executar o comando `serverStatus`. Aplica-se para `cluster`.
#### `validate`
O usuário poderá executar o comando `validate`. Aplica-se para `database` ou `collection`.
#### `top`
O usuário poderá executar o comando `top`. Aplica-se para `cluster`.

### Internal Actions
#### `anyAction`
Permite qualquer ação em um recurso. Não utilize, exceto em circunstâncias excepcionais.
#### `internal`
Permite ações internas. Não utilize, exceto em circunstâncias excepcionais.