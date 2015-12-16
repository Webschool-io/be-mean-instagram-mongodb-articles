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
  - Permite a criação de coleções com o método [https://docs.mongodb.org/manual/reference/method/db.createCollection/#db.createCollection](db.createCollection()).

- createIndex
  - Permite a criação de índices com o método [https://docs.mongodb.org/manual/reference/method/db.collection.createIndex/#db.collection.createIndex](db.collection.createIndex()) ou com o comando [https://docs.mongodb.org/manual/reference/command/createIndexes/#dbcmd.createIndexes](createIndexs).

- createRole
  - Permite a criação de regras de permissão.

- createUser
  - Permite a criação de usuários.

- dropCollection
  - Permite a remoção de uma coleção com o comando [https://docs.mongodb.org/manual/reference/method/db.collection.drop/#db.collection.drop](db.collection.drop()).

- dropRole
  - Permite a remoção de uma regra de permissão.

- dropUser
  - Permite a remoção de um usuário.

- emptycapped
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/emptycapped/#dbcmd.emptycapped](emptycapped).

- enableProfiler
  - Permite que o usuário habilite o profiler da base com o método [https://docs.mongodb.org/manual/reference/method/db.setProfilingLevel/#db.setProfilingLevel](db.setProfilingLevel()).

- grantRole
  - Permite adicinar privilégios a qualquer usuário.

- killCursors
  - Permite a interrupcão de cursores de uma coleção destino.

- revokeRole
  - Permite remover privilégios a qualquer usuário.

- unlock
  - Permite a utilização do método [https://docs.mongodb.org/manual/reference/method/db.fsyncUnlock/#db.fsyncUnlock](db.fsyncUnlock()).

- viewRole
  - Permite a visualização das informações de qualquer regra de permissão.

- viewUser
  - Permite a visualização das informações de qualquer usuário.

### Ações de Gerenciamento de Implantação
- authSchemaUpgrade
  - Permite o uso do comando [https://docs.mongodb.org/manual/reference/command/authSchemaUpgrade/#dbcmd.authSchemaUpgrade](authSchemaUpgrade).

- cleanupOrphaned
  - Permite o uso do comando [https://docs.mongodb.org/manual/reference/command/cleanupOrphaned/#dbcmd.cleanupOrphaned](cleanupOrphaned).

- cpuProfiler
  - Permite a ativação e o uso do CPU profiler.

- inprog
  - Permite a utilização do método [https://docs.mongodb.org/manual/reference/method/db.currentOp/#db.currentOp](db.currentOp()) para retornar a algo que esteja pendente e ativar operações.

- invalidateUserCache
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/invalidateUserCache/#dbcmd.invalidateUserCache](invalidateUserCache).

- killop
  - Permite a utilização do método [https://docs.mongodb.org/manual/reference/method/db.killOp/#db.killOp](db.killOp()).

- planCacheRead
  - Permite a utilização dos comandos [https://docs.mongodb.org/manual/reference/command/planCacheListPlans/#dbcmd.planCacheListPlans](planCacheListPlans) e [https://docs.mongodb.org/manual/reference/command/planCacheListQueryShapes/#dbcmd.planCacheListQueryShapes](planCacheListQueryShapes) e dos métodos [https://docs.mongodb.org/manual/reference/method/PlanCache.getPlansByQuery/#PlanCache.getPlansByQuery](PlanCache.getPlansByQuery()) e [https://docs.mongodb.org/manual/reference/method/PlanCache.listQueryShapes/#PlanCache.listQueryShapes](PlanCache.listQueryShapes()).

- planCacheWrite
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/planCacheClear/#dbcmd.planCacheClear](planCacheClear) e dos métodos [https://docs.mongodb.org/manual/reference/method/PlanCache.clear/#PlanCache.clear](PlanCache.clear()) e [https://docs.mongodb.org/manual/reference/method/PlanCache.clearPlansByQuery/#PlanCache.clearPlansByQuery](PlanCache.clearPlansByQuery()).

- storageDetails
  - Permite a utilização do comando storageDetails.

### Ações de Replicas
- appendOplogNote
  - Permite a adição de notas ao oplog.

- replSetConfigure
  - Permite a configuração de uma replica.

- replSetGetStatus
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/replSetGetStatus/#dbcmd.replSetGetStatus](replSetGetStatus).

- replSetHeartbeat
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/replSetHeartbeat/#dbcmd.replSetHeartbeat](replSetHeartbeat).

- replSetStateChange
  - Permite alterar o estado de uma replica com os comandos [https://docs.mongodb.org/manual/reference/command/replSetFreeze/#dbcmd.replSetFreeze](replSetFreeze), [https://docs.mongodb.org/manual/reference/command/replSetMaintenance/#dbcmd.replSetMaintenance](replSetMaintenance), [https://docs.mongodb.org/manual/reference/command/replSetStepDown/#dbcmd.replSetStepDown](replSetStepDown), e [https://docs.mongodb.org/manual/reference/command/replSetSyncFrom/#dbcmd.replSetSyncFrom](replSetSyncFrom).

- resync
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/resync/#dbcmd.resync](resync).

### Ações de Fragmentação
- addShard
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/addShard/#dbcmd.addShard](addShard).

- enableSharding
  - Permite habilitar o sharding na base de dados utilizando o comando [https://docs.mongodb.org/manual/reference/command/enableSharding/#dbcmd.enableSharding](enableSharding) e em uma coleção utilizando o comando [https://docs.mongodb.org/manual/reference/command/shardCollection/#dbcmd.shardCollection](shardCollection).

- flushRouterConfig
  - Permite utilizar o comando [https://docs.mongodb.org/manual/reference/command/flushRouterConfig/#dbcmd.flushRouterConfig](flushRouterConfig).

- getShardMap
  - Permite utilizar o comando [https://docs.mongodb.org/manual/reference/command/getShardMap/#dbcmd.getShardMap](getShardMap).

- getShardVersion
  - Permite utiizar o comando [https://docs.mongodb.org/manual/reference/command/getShardVersion/#dbcmd.getShardVersion](getShardVersion).

- listShards
  - Permite utiizar o comando [https://docs.mongodb.org/manual/reference/command/listShards/#dbcmd.listShards](listShards).

- moveChunk
  - Permite utiizar o comando [https://docs.mongodb.org/manual/reference/command/moveChunk/#dbcmd.moveChunk](moveChunk). Além disso, o usuário também pode utilizar o comando [https://docs.mongodb.org/manual/reference/command/movePrimary/#dbcmd.movePrimary](movePrimary) desde que o privilégio tenha sido aplicado a base de dados.

- removeShard
  - Permite utiliar o comando [https://docs.mongodb.org/manual/reference/command/removeShard/#dbcmd.removeShard](removeShard).

- shardingState
  - Permite utilizar o comando [https://docs.mongodb.org/manual/reference/command/shardingState/#dbcmd.shardingState](shardingState).

- splitChunk
  - Permite utilizar o comando [https://docs.mongodb.org/manual/reference/command/splitChunk/#dbcmd.splitChunk](splitChunk).

- splitVector
  - Permite utilizar o comando [https://docs.mongodb.org/manual/reference/command/splitVector/#dbcmd.splitVector](splitVector).

### Ações de Administração de Servidores
- applicationMessage
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/logApplicationMessage/#dbcmd.logApplicationMessage](logApplicationMessage).

- closeAllDatabases
  - Permite a utilização do comando closeAllDatabases.

- collMod
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/collMod/#dbcmd.collMod](collMod).

- compact
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/compact/#dbcmd.compact](compact).

- connPoolSync
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/connPoolSync/#dbcmd.connPoolSync](connPoolSync).

- convertToCapped
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/convertToCapped/#dbcmd.convertToCapped](convertToCapped).

- dropDatabase
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/dropDatabase/#dbcmd.dropDatabase](dropDatabase).

- dropIndex
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/dropIndexes/#dbcmd.dropIndexes](dropIndexes).

- fsync
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/fsync/#dbcmd.fsync](fsync).

- getParameter
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/getParameter/#dbcmd.getParameter](getParameter).

- hostInfo
  - Permite a consulta de informações da instânca da base que se encontra em execucão.

- logRotate
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/logRotate/#dbcmd.logRotate](logRotate).

- reIndex
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/reIndex/#dbcmd.reIndex](reIndex).

- renameCollectionSameDB
  - Permite efetuar alterações no nome das coleções da base atual utilizando o comando [https://docs.mongodb.org/manual/reference/command/renameCollection/#dbcmd.renameCollection](renameCollection). Este processo somente será possível se o novo nome ainda não existir.

- repairDatabase
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/repairDatabase/#dbcmd.repairDatabase](repairDatabase).

- setParameter
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/setParameter/#dbcmd.setParameter](setParameter).

- shutdown
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/shutdown/#dbcmd.shutdown](shutdown).

- touch
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/touch/#dbcmd.touch](touch).

### Ações de Diagnósticos
- collStats
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/collStats/#dbcmd.collStats](collStats).

- connPoolStats
  - Permite a utilização dos comandos [https://docs.mongodb.org/manual/reference/command/connPoolStats/#dbcmd.connPoolStats](connPoolStats) e [https://docs.mongodb.org/manual/reference/command/shardConnPoolStats/#dbcmd.shardConnPoolStats](shardConnPoolStats).

- cursorInfo
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/cursorInfo/#dbcmd.cursorInfo](cursorInfo).

- dbHash
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/dbHash/#dbcmd.dbHash](dbHash).

- dbStats
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/dbStats/#dbcmd.dbStats](dbStats).

- diagLogging
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/diagLogging/#dbcmd.diagLogging](diagLogging).

- getCmdLineOpts
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/getCmdLineOpts/#dbcmd.getCmdLineOpts](getCmdLineOpts).

- getLog
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/getLog/#dbcmd.getLog](getLog).

- indexStats
  - Permite a utilização do comando indexStats. **Removido na versão 3.0**

- listDatabases
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/listDatabases/#dbcmd.listDatabases](listDatabases).

- listCollections
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/listCollections/#dbcmd.listCollections](listCollections).

- listIndexes
  - Permite a utilização do comando ListIndexes.

- netstat
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/netstat/#dbcmd.netstat](netstat).

- serverStatus
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/serverStatus/#dbcmd.serverStatus](serverStatus).

- validate
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/validate/#dbcmd.validate](validate).

- top
  - Permite a utilização do comando [https://docs.mongodb.org/manual/reference/command/top/#dbcmd.top](top).

### Ações Internas
- anyAction
  - Permite qualquer ação no recurso.

- internal
  - Permite ações internas.
