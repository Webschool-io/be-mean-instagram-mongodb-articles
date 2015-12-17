# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Rafael Crispim Ignácio

**Data:** 1450233070833

## Qual a diferença entre Autenticação e Autorização?
Resumidamente podemos dizer que **autenticação** é a validação dos dados de acesso recebidos em um determinada requisição e **autorização** é a verificação dos direitos deste usuário autenticado conforme as ações executadas.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
- Usuário **admin**

```js
be-mean> db.createUser(
    {
        user: "userAdmin",
        pwd: "admin123",
        roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
    }
)
Successfully added user: {
  "user": "userAdmin",
  "roles": [
    {
      "role": "userAdminAnyDatabase",
      "db": "admin"
    }
  ]
}
```

- Usuário **comum*** apenas com permissão de leitura

```js
be-mean> db.createUser(
    {
        user: "userTest",
        pwd: "123",
        roles: [ "read" ]
    }
)
Successfully added user: {
  "user": "userTest",
  "roles": [
    "read"
  ]
}
```

## Explique cada papel listado em Cluster Administration Roles.
- clusterAdmin
  - Fornece total acesso às funções de Clusterização

- clusterManager
  - Fornece acesso de manutenção e monitoramento de clusters

- clusterMonitor
  - Fornece acesso de leitura para monitoramento dos clusters

- hostManager
  - Fornece acesso de manutenção e monitoramento dos servidores.

## Explique todas as ações de privilégio listadas em Privilege Actions.
### Ações de Leitura e Escrita
- find
  - Permite a utilização do método [db.collection.find()](https://docs.mongodb.org/manual/reference/method/db.collection.find/#db.collection.find).

- insert
  - Permite a utilização do comando [insert](https://docs.mongodb.org/manual/reference/command/insert/#dbcmd.insert).

- remove
  - Permite a utilização do método [db.collection.remove()](https://docs.mongodb.org/manual/reference/method/db.collection.remove/#db.collection.remove).

- update
  - Permite a utilização do comando [update](https://docs.mongodb.org/manual/reference/command/update/#dbcmd.update).

- bypassDocumentValidation
  - Objeto de configuração utilizado por alguns parâmetros.
    - applyOps
    - findAndModify
    - mapReduce
    - insert
    - update
    - $out

### Ações de Gestão de Base de Dados
- changeCustomData
  - Permite alterar as informações customizadas de qualquer usuário da base de dados.

- changeOwnCustomData
  - Permite alterar as próprias informações customizadas.

- changeOwnPassword
  - Permite alterar a própria senha.

- changePassword
  - Permite alterar a senha de qualquer usuário da base de dados.

- createCollection
  - Permite a criação de coleções com o método [db.createCollection()](https://docs.mongodb.org/manual/reference/method/db.createCollection/#db.createCollection).

- createIndex
  - Permite a criação de índices com o método [db.collection.createIndex()](https://docs.mongodb.org/manual/reference/method/db.collection.createIndex/#db.collection.createIndex) ou com o comando [createIndexes](https://docs.mongodb.org/manual/reference/command/createIndexes/#dbcmd.createIndexes).

- createRole
  - Permite a criação de regras de permissão.

- createUser
  - Permite a criação de usuários.

- dropCollection
  - Permite a remoção de uma coleção com o comando [db.collection.drop()](https://docs.mongodb.org/manual/reference/method/db.collection.drop/#db.collection.drop).

- dropRole
  - Permite a remoção de uma regra de permissão.

- dropUser
  - Permite a remoção de um usuário.

- emptycapped
  - Permite a utilização do comando [emptycapped](https://docs.mongodb.org/manual/reference/command/emptycapped/#dbcmd.emptycapped).

- enableProfiler
  - Permite que o usuário habilite o profiler da base com o método [db.setProfilingLevel()](https://docs.mongodb.org/manual/reference/method/db.setProfilingLevel/#db.setProfilingLevel).

- grantRole
  - Permite adicinar privilégios a qualquer usuário.

- killCursors
  - Permite a interrupcão de cursores de uma coleção destino.

- revokeRole
  - Permite remover privilégios a qualquer usuário.

- unlock
  - Permite a utilização do método [db.fsyncUnlock()](https://docs.mongodb.org/manual/reference/method/db.fsyncUnlock/#db.fsyncUnlock).

- viewRole
  - Permite a visualização das informações de qualquer regra de permissão.

- viewUser
  - Permite a visualização das informações de qualquer usuário.

### Ações de Gerenciamento de Implantação
- authSchemaUpgrade
  - Permite o uso do comando [authSchemaUpgrade](https://docs.mongodb.org/manual/reference/command/authSchemaUpgrade/#dbcmd.authSchemaUpgrade).

- cleanupOrphaned
  - Permite o uso do comando [cleanupOrphaned](https://docs.mongodb.org/manual/reference/command/cleanupOrphaned/#dbcmd.cleanupOrphaned).

- cpuProfiler
  - Permite a ativação e o uso do CPU profiler.

- inprog
  - Permite a utilização do método [db.currentOp()](https://docs.mongodb.org/manual/reference/method/db.currentOp/#db.currentOp) para retornar a algo que esteja pendente e ativar operações.

- invalidateUserCache
  - Permite a utilização do comando [invalidateUserCache](https://docs.mongodb.org/manual/reference/command/invalidateUserCache/#dbcmd.invalidateUserCache).

- killop
  - Permite a utilização do método [db.killOp()](https://docs.mongodb.org/manual/reference/method/db.killOp/#db.killOp).

- planCacheRead
  - Permite a utilização dos comandos [planCacheListPlans](https://docs.mongodb.org/manual/reference/command/planCacheListPlans/#dbcmd.planCacheListPlans) e [planCacheListQueryShapes](https://docs.mongodb.org/manual/reference/command/planCacheListQueryShapes/#dbcmd.planCacheListQueryShapes) e dos métodos [PlanCache.getPlansByQuery()](https://docs.mongodb.org/manual/reference/method/PlanCache.getPlansByQuery/#PlanCache.getPlansByQuery) e [PlanCache.listQueryShapes()](https://docs.mongodb.org/manual/reference/method/PlanCache.listQueryShapes/#PlanCache.listQueryShapes).

- planCacheWrite
  - Permite a utilização do comando [planCacheClear](https://docs.mongodb.org/manual/reference/command/planCacheClear/#dbcmd.planCacheClear) e dos métodos [PlanCache.clear()](https://docs.mongodb.org/manual/reference/method/PlanCache.clear/#PlanCache.clear) e [PlanCache.clearPlansByQuery()](https://docs.mongodb.org/manual/reference/method/PlanCache.clearPlansByQuery/#PlanCache.clearPlansByQuery).

- storageDetails
  - Permite a utilização do comando storageDetails.

### Ações de Replicas
- appendOplogNote
  - Permite a adição de notas ao oplog.

- replSetConfigure
  - Permite a configuração de uma replica.

- replSetGetStatus
  - Permite a utilização do comando [replSetGetStatus](https://docs.mongodb.org/manual/reference/command/replSetGetStatus/#dbcmd.replSetGetStatus).

- replSetHeartbeat
  - Permite a utilização do comando [replSetHeartbeat](https://docs.mongodb.org/manual/reference/command/replSetHeartbeat/#dbcmd.replSetHeartbeat).

- replSetStateChange
  - Permite alterar o estado de uma replica com os comandos [replSetFreeze](https://docs.mongodb.org/manual/reference/command/replSetFreeze/#dbcmd.replSetFreeze), [replSetMaintenance](https://docs.mongodb.org/manual/reference/command/replSetMaintenance/#dbcmd.replSetMaintenance), [replSetStepDown](https://docs.mongodb.org/manual/reference/command/replSetStepDown/#dbcmd.replSetStepDown), e [replSetSyncFrom](https://docs.mongodb.org/manual/reference/command/replSetSyncFrom/#dbcmd.replSetSyncFrom).

- resync
  - Permite a utilização do comando [resync](https://docs.mongodb.org/manual/reference/command/resync/#dbcmd.resync).

### Ações de Fragmentação
- addShard
  - Permite a utilização do comando [addShard](https://docs.mongodb.org/manual/reference/command/addShard/#dbcmd.addShard).

- enableSharding
  - Permite habilitar o sharding na base de dados utilizando o comando [enableSharding](https://docs.mongodb.org/manual/reference/command/enableSharding/#dbcmd.enableSharding) e em uma coleção utilizando o comando [shardCollection](https://docs.mongodb.org/manual/reference/command/shardCollection/#dbcmd.shardCollection).

- flushRouterConfig
  - Permite utilizar o comando [flushRouterConfig](https://docs.mongodb.org/manual/reference/command/flushRouterConfig/#dbcmd.flushRouterConfig).

- getShardMap
  - Permite utilizar o comando [getShardMap](https://docs.mongodb.org/manual/reference/command/getShardMap/#dbcmd.getShardMap).

- getShardVersion
  - Permite utiizar o comando [getShardVersion](https://docs.mongodb.org/manual/reference/command/getShardVersion/#dbcmd.getShardVersion).

- listShards
  - Permite utiizar o comando [listShards](https://docs.mongodb.org/manual/reference/command/listShards/#dbcmd.listShards).

- moveChunk
  - Permite utiizar o comando [moveChunk](https://docs.mongodb.org/manual/reference/command/moveChunk/#dbcmd.moveChunk). Além disso, o usuário também pode utilizar o comando [movePrimary](https://docs.mongodb.org/manual/reference/command/movePrimary/#dbcmd.movePrimary) desde que o privilégio tenha sido aplicado a base de dados.

- removeShard
  - Permite utiliar o comando [removeShard](https://docs.mongodb.org/manual/reference/command/removeShard/#dbcmd.removeShard).

- shardingState
  - Permite utilizar o comando [shardingState](https://docs.mongodb.org/manual/reference/command/shardingState/#dbcmd.shardingState).

- splitChunk
  - Permite utilizar o comando [splitChunk](https://docs.mongodb.org/manual/reference/command/splitChunk/#dbcmd.splitChunk).

- splitVector
  - Permite utilizar o comando [splitVector](https://docs.mongodb.org/manual/reference/command/splitVector/#dbcmd.splitVector).

### Ações de Administração de Servidores
- applicationMessage
  - Permite a utilização do comando [logApplicationMessage](https://docs.mongodb.org/manual/reference/command/logApplicationMessage/#dbcmd.logApplicationMessage).

- closeAllDatabases
  - Permite a utilização do comando closeAllDatabases.

- collMod
  - Permite a utilização do comando [collMod](https://docs.mongodb.org/manual/reference/command/collMod/#dbcmd.collMod).

- compact
  - Permite a utilização do comando [compact](https://docs.mongodb.org/manual/reference/command/compact/#dbcmd.compact).

- connPoolSync
  - Permite a utilização do comando [connPoolSync](https://docs.mongodb.org/manual/reference/command/connPoolSync/#dbcmd.connPoolSync).

- convertToCapped
  - Permite a utilização do comando [convertToCapped](https://docs.mongodb.org/manual/reference/command/convertToCapped/#dbcmd.convertToCapped).

- dropDatabase
  - Permite a utilização do comando [dropDatabase](https://docs.mongodb.org/manual/reference/command/dropDatabase/#dbcmd.dropDatabase).

- dropIndex
  - Permite a utilização do comando [dropIndexes](https://docs.mongodb.org/manual/reference/command/dropIndexes/#dbcmd.dropIndexes).

- fsync
  - Permite a utilização do comando [fsync](https://docs.mongodb.org/manual/reference/command/fsync/#dbcmd.fsync).

- getParameter
  - Permite a utilização do comando [getParameter](https://docs.mongodb.org/manual/reference/command/getParameter/#dbcmd.getParameter).

- hostInfo
  - Permite a consulta de informações da instânca da base que se encontra em execucão.

- logRotate
  - Permite a utilização do comando [logRotate](https://docs.mongodb.org/manual/reference/command/logRotate/#dbcmd.logRotate).

- reIndex
  - Permite a utilização do comando [reIndex](https://docs.mongodb.org/manual/reference/command/reIndex/#dbcmd.reIndex).

- renameCollectionSameDB
  - Permite efetuar alterações no nome das coleções da base atual utilizando o comando [renameCollection](https://docs.mongodb.org/manual/reference/command/renameCollection/#dbcmd.renameCollection). Este processo somente será possível se o novo nome ainda não existir.

- repairDatabase
  - Permite a utilização do comando [repairDatabase](https://docs.mongodb.org/manual/reference/command/repairDatabase/#dbcmd.repairDatabase).

- setParameter
  - Permite a utilização do comando [setParameter](https://docs.mongodb.org/manual/reference/command/setParameter/#dbcmd.setParameter).

- shutdown
  - Permite a utilização do comando [shutdown](https://docs.mongodb.org/manual/reference/command/shutdown/#dbcmd.shutdown).

- touch
  - Permite a utilização do comando [touch](https://docs.mongodb.org/manual/reference/command/touch/#dbcmd.touch).

### Ações de Diagnósticos
- collStats
  - Permite a utilização do comando [collStats](https://docs.mongodb.org/manual/reference/command/collStats/#dbcmd.collStats).

- connPoolStats
  - Permite a utilização dos comandos [connPoolStats](https://docs.mongodb.org/manual/reference/command/connPoolStats/#dbcmd.connPoolStats) e [shardConnPoolStats](https://docs.mongodb.org/manual/reference/command/shardConnPoolStats/#dbcmd.shardConnPoolStats).

- cursorInfo
  - Permite a utilização do comando [cursorInfo](https://docs.mongodb.org/manual/reference/command/cursorInfo/#dbcmd.cursorInfo).

- dbHash
  - Permite a utilização do comando [dbHash](https://docs.mongodb.org/manual/reference/command/dbHash/#dbcmd.dbHash).

- dbStats
  - Permite a utilização do comando [dbStats](https://docs.mongodb.org/manual/reference/command/dbStats/#dbcmd.dbStats).

- diagLogging
  - Permite a utilização do comando [diagLogging](https://docs.mongodb.org/manual/reference/command/diagLogging/#dbcmd.diagLogging).

- getCmdLineOpts
  - Permite a utilização do comando [getCmdLineOpts](https://docs.mongodb.org/manual/reference/command/getCmdLineOpts/#dbcmd.getCmdLineOpts).

- getLog
  - Permite a utilização do comando [getLog](https://docs.mongodb.org/manual/reference/command/getLog/#dbcmd.getLog).

- indexStats
  - Permite a utilização do comando indexStats. **Removido na versão 3.0**

- listDatabases
  - Permite a utilização do comando [listDatabases](https://docs.mongodb.org/manual/reference/command/listDatabases/#dbcmd.listDatabases).

- listCollections
  - Permite a utilização do comando [listCollections](https://docs.mongodb.org/manual/reference/command/listCollections/#dbcmd.listCollections).

- listIndexes
  - Permite a utilização do comando ListIndexes.

- netstat
  - Permite a utilização do comando [netstat](https://docs.mongodb.org/manual/reference/command/netstat/#dbcmd.netstat).

- serverStatus
  - Permite a utilização do comando [serverStatus](https://docs.mongodb.org/manual/reference/command/serverStatus/#dbcmd.serverStatus).

- validate
  - Permite a utilização do comando [validate](https://docs.mongodb.org/manual/reference/command/validate/#dbcmd.validate).

- top
  - Permite a utilização do comando [top](https://docs.mongodb.org/manual/reference/command/top/#dbcmd.top).

### Ações Internas
- anyAction
  - Permite qualquer ação no recurso.

- internal
  - Permite ações internas.
