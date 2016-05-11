# MongoDB - Artigo sobre Autenticação no MongoDB
**Autor:** Abel Neto

**Data:** 1462832546308

## Autenticação versus Autorização

O termo _autenticação_ é utilizado para designar o processo de verificação de credenciais do usuário e permitir ou não o seu acesso ao sistema. Esse processo consiste no envio de credenciais (em texto plano ou criptografadas)  para um serviço de autenticação, que irá determinar se o usuário é realmente quem ele diz que é.

_Autorização_, por outro lado, é algo que acontece somente após uma _autenticação_ bem sucedida, ou seja, quando um usuário foi validado como tendo credenciais válidas. O sistema então verifica se o usuário possui _autorização_ para acessar a funcionalidade do sistema à qual está solicitando acesso. Em geral, esquemas de _autorização_ são definidos através de listas de permissões atribuidas a cada usuário do sistema.

## Como criar um usuário no MongoDb

Para criar um usuário no Mongo, utilizamos o método `db.createUser()`, que recebe como parâmetro obrigatório um documento contendo as informações do usuário, com a seguinte estrutura:

```javascript
{ 
  user: "<nome do usuário>",
  pwd: "<senha em texto plano>",
  customData: { <qualquer informação> },
  roles: [
    { role: "<nome da permissão>", db: "<database>" } | "<nome da permissão>"
  ]
}
```

Onde:

    user        - Nome do usuário;
    pwd         - Senha;
    customData  - Propriedade opcional que recebe um documento e pode ser usada para armazenar dados adicionais de qualquer tipo;
    roles       - As permissões do usuário. 

A propriedade `roles` deve receber um array, que pode ter, em cada posição, uma string ou um documento. 

No caso em que é armazenada uma string nesse array, esta representa o nome da permissão, que terá efeito sobre o novo usuário somente na database selecionada no momento da sua criação. 

Já quando um documento é adicionado ao array, ele especifica o nome da permissão e também a database onde ela terá efeito para o usuário criado, da seguinte forma:

```javascript
{ role: "<nome da permissão>", db: "<database>" }
```

No exemplo abaixo, temos um comando de criação de um usuário comum (sem roles):

```javascript
db.createUser(
{ 
  user : "netoabel",
  pwd: "senha123",
  customData : { twitter: "@_netoabel" }
}, 
{ w: "majority" , wtimeout: 5000 }
)
```

O documento da penúltima linha é o segundo parâmetro, opcional, que o método `db.createUser()` pode receber: o [WriteConcern](https://docs.mongodb.com/manual/reference/write-concern/), que representa o nível de confirmação exigido pelo MongoDB nas operações de escrita e pode ser ajustado visando desempenho ou consistência dos dados.

## Como criar um usuário administrador

Para criar um usuário com permissões de administrador em todas as databases, basta criá-lo com a permissão `userAdminAnyDatabase` na database `admin`:

```javascript
db.createUser({
  user: "netoabel",
  pwd: "senha123",
  roles: [{ role: "userAdminAnyDatabase", db: "admin" }]
})
```

## Permissões de administração de cluster

Usuários com permissões de administração de cluster são capazes de administrar várias instâncias do MongoDB organizadas em cluster, em vez de somente databases específicas em um servidor. 

#### clusterAdmin
Nível mais alto de gerenciamento de cluster. É equivalente a uma combinação das permissões `clusterManager`, `clusterMonitor` e `hostManager`, além de também conceder permissão para a ação `dropDatabase`.

#### clusterManager
Autoriza ações de monitoramento e gerenciamento no cluster. Um usuário com essa permissão pode acessar as databases `config` e `local`, que são usadas em sharding e replicação, respectivamente.

Provê acesso às seguintes ações no cluster como um todo:

* addShard
* applicationMessage
* cleanupOrphaned
* flushRouterConfig
* listShards
* removeShard
* replSetConfigure
* replSetGetStatus
* replSetStateChange
* resync

Além das seguintes ações em todas as databases do cluster:

* enableSharding
* moveChunk
* splitChunk
* splitVector

Na collection `settings` da database `config`, provê acesso às seguintes ações:

* insert
* remove
* update

Ainda na database `config`, nas collections `system.indexes`, `system.js`, `system.namespaces` e em todas as collections de configuração, provê acesso às seguntes ações:

* collStats
* dbHash
* dbStats
* find
* killCursors

Na database `local`, provê acesso às seguintes acões na collection `replset`:

* collStats
* dbHash
* dbStats
* find
* killCursors

#### clusterMonitor

Autoriza acess somente leitura a ferramentas de monitoração, como o [MongoDB Cloud Manager](https://cloud.mongodb.com/?jmp=docs&_ga=1.132013837.389647211.1462567638).

Provê as seguintes permissões no cluster como um todo:

* connPoolStats
* cursorInfo
* getCmdLineOpts
* getLog
* getParameter
* getShardMap
* hostInfo
* inprog
* listDatabases
* listShards
* netstat
* replSetGetStatus
* serverStatus
* shardingState
* top

Provê as seguintes permissões em todas as databases do cluster:

* collStats
* dbStats
* getShardVersion

Provê a ação `find` em todas as collections `system.profile` do cluster. 

Nas collections `system.indexes`, `system.js`, `system.namespaces` e em todas as collections de configuração da database `config`, provê acesso às seguntes ações:

* collStats
* dbHash
* dbStats
* find
* killCursors

#### hostManager

Autoriza o monitoramento e gerenciamento de servidores.

Provê as seguintes ações no cluster como um todo:* 

* applicationMessage
* closeAllDatabases
* connPoolSync
* cpuProfiler
* diagLogging
* flushRouterConfig
* fsync
* invalidateUserCache
* killop
* logRotate
* resync
* setParameter
* shutdown
* touch
* unlock

Disponibiliza as seguintes ações em todas as databases do cluster:

* killCursors
* repairDatabase

## Ações de privilégio

Ações de privilégio definem as operações que um usuário pode realizar em um recurso. Um priviégio compreende um recurso e as ações permitidas. Veja a seguir uma lista das ações de privilégio disponíveis:

## Ações de consulta e escrita

#### find
O usuário pode utilizar o método `db.collection.find()`. Aplicável a databases ou collections.

#### insert
O usuário pode utilizar o comando `insert`. Aplicável a databases ou collections.

#### remove
O usuário pode utilizar o método `db.collection.remove()`. Aplicável a databases ou collections.

#### update
O usuário pode utilizar o comando `update`. Aplicável a databases ou collections.

#### bypassDocumentValidation
O usuário pode bypassar a validação de documentos para comandos que suportam a opção `bypassDocumentValidation`. Aplicável a databases ou collections.

## Ações de gerenciamento de databases

#### changeCustomData
O usuário pode alterar informações customizadas de qualquer usuário na database especificada. Aplicável a databases.

#### changeOwnCustomData
Usuários podem alterar suas próprias informações customizadas. Aplicável a databases.

#### changeOwnPassword
Usuários podem alterar suas próprias senhas. Aplicável a databases.

#### changePassword
O usuário pode alterar a senha de qualquer usuário na database especificada. Aplicável a databases.

#### createCollection
O usuário pode utilizar o método `db.createCollection()`. Aplicável a databases ou collections.

#### createIndex
Provê acesso ao método `db.collection.createIndex()` e ao comando `createIndexes`. Aplicável a databases ou collections.

#### createRole
O usuário pode criar novas permissões na database especificada. Aplicável a databases.

#### createUser
O usuário pode criar novos usuários na database especificada. Aplicável a databases.

#### dropCollection
O usuário pode utilizar o método `db.collection.drop()`. Aplicável a databases ou collections.

#### dropRole
O usuário pode remover qualquer permissão da database especificada. Aplicável a databases.

#### dropUser
O usuário pode remover qualquer usuário da database especificada. Aplicável a databases.

#### emptycapped
O usuário pode utilizar o comando `emptycapped`. Aplicável a databases ou collections.

#### enableProfiler
O usuário pode utilizar o método `db.setProfilingLevel()`. Aplicável a databases.

#### grantRole
O usuário pode atribuir qualquer permissão na database para qualquer usuário de qualquer database no sistema. Aplicável a databases.

#### killCursors
O usuário pode destruir cursores na collection de destino.

#### revokeRole
O usuário pode remover qualquer permissão de qualquer usuário de qualquer database no sistema. Aplicável a databases.

#### unlock
O usuário pode utilizar o método `db.fsyncUnlock()`. Aplicável ao cluster.

#### viewRole
O usuário pode visualizar informações a respeito de qualquer permissão na database especificada. Aplicável a databases.

#### viewUser
O usuário pode visualizar as informações de qualquer usuário na database especificada. Aplicável a databases.

## Ações de gerenciamento de deploy

#### authSchemaUpgrade
O usuário pode utilizar o comando `authSchemaUpgrade`. Aplicável ao cluster.

#### cleanupOrphaned
O usuário pode utilizar o comando `cleanupOrphaned`. Aplicável ao cluster.

#### cpuProfiler
O usuário pode habilitar o CPU profiler. Aplicável ao cluster.

#### inprog
O usuário pode utilizar o método `db.currentOp()` para retornar operações pendentes e ativas. Aplicável ao cluster.

#### invalidateUserCache
Provê acesso ao comando `invalidateUserCache`. Aplicável ao cluster.

#### killop
O usuário pode utilizar o método db.killOp(). Aplicável ao cluster.

#### planCacheRead
O usuário pode utilizar os comandos `planCacheListPlans` e `planCacheListQueryShapes` e os métodos `PlanCache.getPlansByQuery()` e `PlanCache.listQueryShapes()`. Aplicável a databases ou collections.

#### planCacheWrite
O usuário pode utilizar o comando `planCacheClear` e os métodos `PlanCache.clear()` e `PlanCache.clearPlansByQuery()`. Aplicável a databases ou collections.

#### storageDetails
O usuário pode utilizar o comando `storageDetails`. Aplicável a databases ou collections.

## Ações de replicação

#### appendOplogNote
O usuário pode adicionar anotações ao oplog. Aplicável ao cluster.

#### replSetConfigure
O usuário pode configurar um replica set. Aplicável ao cluster.

#### replSetGetStatus
O usuário pode utilizar o comando `replSetGetStatus`. Aplicável ao cluster.

#### replSetHeartbeat
O usuário pode utilizar o comando `replSetHeartbeat`. Aplicável ao cluster.

#### replSetStateChange
O usuário pode modificar o estado de um replica set através dos comandos `replSetFreeze`, `replSetMaintenance`, `replSetStepDown`, and `replSetSyncFrom`. Aplicável ao cluster.

#### resync
O usuário pode utilizar o comando `resync`. Aplicável ao cluster.

## Ações de sharding

#### addShard
O usuário pode utilizar o comando `addShard`. Aplicável ao cluster.

#### enableSharding
O usuário pode habilitar sharding em uma database usando o comando `enableSharding` e pode "shardear" uma collection usando o comando `shardCollection`. Aplicável a databases ou collections.

#### flushRouterConfig
O usuário pode utilizar o comando `flushRouterConfig`. Aplicável ao cluster.

#### getShardMap
O usuário pode utilizar o comando `getShardMap`. Aplicável ao cluster.

#### getShardVersion
O usuário pode utilizar o comando `getShardVersion`. Aplicável a databases.

#### listShards
O usuário pode utilizar o comando `listShards`. Aplicável ao cluster.

#### moveChunk
O usuário pode utilizar o comando `moveChunk`. Também pode utilizar o comando `movePrimary`, desde que o privilégio seja aplicado a uma database apropriada. Aplicável a databases ou collections.

#### removeShard
O usuário pode utilizar o comando `removeShard`. Aplicável ao cluster.

#### shardingState
O usuário pode utilizar o comando `shardingState`. Aplicável ao cluster.

#### splitChunk
O usuário pode utilizar o comando `splitChunk`. Aplicável a databases ou collections.

#### splitVector
O usuário pode utilizar o comando `splitVector`. Aplicável a databases ou collections.

## Ações de administração de servidores

#### applicationMessage
O usuário pode utilizar o comando `logApplicationMessage`. Aplicável ao cluster.

#### closeAllDatabases
O usuário pode utilizar o comando `closeAllDatabases`. Aplicável ao cluster.

#### collMod
O usuário pode utilizar o comando `collMod`. Aplicável a databases ou collections.

#### compact
O usuário pode utilizar o comando `compact`. Aplicável a databases ou collections.

#### connPoolSync
O usuário pode utilizar o comando `connPoolSync`. Aplicável ao cluster.

#### convertToCapped
O usuário pode utilizar o comando `convertToCapped`. Aplicável a databases ou collections.

#### dropDatabase
O usuário pode utilizar o comando `dropDatabase`. Aplicável a databases.

#### dropIndex
O usuário pode utilizar o comando `dropIndexes`. Aplicável a databases ou collections.

#### fsync
O usuário pode utilizar o comando `fsync`. Aplicável ao cluster.

#### getParameter
O usuário pode utilizar o comando `getParameter`. Aplicável ao cluster.

#### hostInfo
Provê informações sobre o servidor onde a instância do MongoDB é executada.Aplicável ao cluster.

#### logRotate
O usuário pode utilizar o comando `logRotate`. Aplicável ao cluster.

#### reIndex
O usuário pode utilizar o comando `reIndex`. Aplicável a databases ou collections.

#### renameCollectionSameDB
Permite que o usuário renomeie collections na database selecionada usando o comando `renameCollection`. Aplicável a databases.

Se uma collection com o novo nome já existir, o usuário deve também possuir a ação `dropCollection` na collection de destino.

#### repairDatabase
O usuário pode utilizar o comando `repairDatabase`. Aplicável a databases.

#### setParameter
O usuário pode utilizar o comando `setParameter`. Aplicável ao cluster.

#### shutdown
O usuário pode utilizar o comando `shutdown`. Aplicável ao cluster.

#### touch
O usuário pode utilizar o comando `touch`. Aplicável ao cluster.

## Ações de diagnóstico

#### collStats
O usuário pode utilizar o comando `collStats`. Aplicável a databases ou collections.

#### connPoolStats
O usuário pode utilizar os comandos `connPoolStats` e `shardConnPoolStats`. Aplicável ao cluster.

#### cursorInfo
O usuário pode utilizar o comando `cursorInfo`. Aplicável ao cluster.

#### dbHash
O usuário pode utilizar o comando `dbHash`. Aplicável a databases ou collections.

#### dbStats
O usuário pode utilizar o comando `dbStats`. Aplicável a databases.

#### diagLogging
O usuário pode utilizar o comando `diagLogging`. Aplicável ao cluster.

#### getCmdLineOpts
O usuário pode utilizar o comando `getCmdLineOpts`. Aplicável ao cluster.

#### getLog
O usuário pode utilizar o comando `getLog`. Aplicável ao cluster.

#### indexStats
O usuário pode utilizar o comando `indexStats`. Aplicável a databases ou collections.

#### listDatabases
O usuário pode utilizar o comando `listDatabases`. Aplicável ao cluster.

#### listCollections
O usuário pode utilizar o comando `listCollections`. Aplicável a databases.

#### listIndexes
O usuário pode utilizar o comando `ListIndexes`. Aplicável a databases ou collections.

#### netstat
O usuário pode utilizar o comando `netstat`. Aplicável ao cluster.

#### serverStatus
O usuário pode utilizar o comando `serverStatus`. Aplicável ao cluster.

#### validate
O usuário pode utilizar o comando `validate`. Aplicável a databases ou collections.

#### top
O usuário pode utilizar o comando `top`. Aplicável ao cluster.

## Ações internas

#### anyAction
Permite qualquer ação em um recurso. Não atribua esta ação, exceto para circunstâncias excepcionais.

#### internal
Permite ações internas. Não atribua esta ação, exceto para circunstâncias excepcionais.

__Referência__: [Documentação oficial do MongoDB](https://docs.mongodb.com/v2.6/reference/).