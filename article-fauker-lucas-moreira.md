# MongoDb - Artigo sobre Autenticação no MongoDb

**Autor:** Lucas da Silva Moreira

**Data** Date.now() //em timestamp

## Qual a diferença entre Autenticação e Autorização?

**Autenticação**: É a verificação das credenciais de um determinado usuário em uma tentativa de conexão. Geralmente envolve um usuário e uma senha, mas pode incluir outros métodos de validação, por exemplo: Certificado Digital etc.

**Autorização**: A autorização serve para verificar se um usuário que já passou pela autenticação e foi bem-sucedido tem acesso a determinadas funcionalidades do sistema.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Para criar um usuário administrador:

```
use admin
db.createUser(
  {
    user: "UsuarioAdmin",
    pwd: "admin123",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
```

Para criar um usuário comum:

```
use admin
db.createUser(
  {
    user: "UsuarioComum",
    pwd: "comum123",
    roles: [{db: 'admin'}]
  }
)
```

## Explique cada papel listado em Cluster Administration Roles.

**Cluster Admin**: Permite acesso total so gerenciamento de cluster. Essa role combina os privilégios das seguintes roles: *clusterManager*, *clusterMonitor* e *hostManager*. E como um adicional, essa função permite o uso de *dropDatabase*.

**Cluster Manager**: Permite gerenciamento e monitoração de ações no cluster. O usuário com essa função pode acessar os bancos de dados *config* e *local*, os quais são usados em sharding e replicações, respectivamente.

**Cluster Monitor**: Permite apenas leitura para monitorar ferramentas como *MongoDB Cloud Manager*.

**Host Manager**: Permite funções de monitoração e administração de servidores de banco de dados.

## Explique todas as ações de privilégio listadas em Privilege Actions.

`Privilege Actions` definem as operações que um usuário pode realizar em determinado recurso. Em nosso caso, no MongoDB.

* Query and Write Actions
	* find: O usuário pode executar o método `db.collection.find()`. Pode ser aplicado para `database` ou `collection`.
	* insert: O usuário pode executar o comando `insert`. Pode ser aplicado para `database` ou `collection`.
	* remove: O usuário pode executar o método `db.collection.remove()`. Pode ser aplicado para `database` ou `collection`.
	* update: O usuário pode executar o comando `update`. Pode ser aplicado para `database` ou `collection`.

* Database Management Actions
	* changeCustomData: O usuário pode alterar uma informação aleatória de qualquer usuário da `database`. Pode ser aplicado para `database`.
	* changeOwnCustomData: Usuários podem alterar as alterações deles próprios. Pode ser aplicado para `database`.
	* changePassword: O usuário poderá alterar o `password` dele na `database`.	Pode ser aplicado para `database`.
	* createCollection: O usuário pode executar o método `db.createCollection()`. Pode ser aplicado para `database` ou `collection`.
	* createIndex: Permite acesso para o método `db.collection.createIndex()` e para o comando `createIndexes`. Pode ser aplicado para `database` ou `collection`.
	* createRole: O usuário pode criar uma nova `role` na `database`. Pode ser aplicado para `database`.
	* createUser: O usuário pode criar novos usuários para a `database`. Pode ser aplicado para `database`.
	* dropCollection: O usuário pode executar o método `db.collection.drop()`. Pode ser aplicado para `database` ou `collection`.
	* dropRole: O usuário pode deletar qualquer `role` na `database`. Pode ser aplicado para `database`.
	* dropUser: Usuário pode remover qualquer usuário da `database`. Pode ser aplicado para `database`.
	* emptycapped: O usuário poderá executar o comando `emptycapped`. Pode ser aplicado para `database` ou `collection`.
	* enableProfiler: O usuário poderá executar o método `db.setProfilingLevel()`. Pode ser aplicado para `database` ou `collection`.
	* grantRole: O usuário poderá conceder qualquer `role` na `database` para qualquer usuário de qualquer `database` no sistema. Pode ser aplicado para `database`.
	* killCursors: O usuário pode matar cursores em determinadas coleções.
	* revokeRole: O usuário pode remover qualquer `role` de qualquer usuário, em qualquer `database` no sistema. Pode ser aplicado para `database` ou `collection`.
	* unlock: O usuário pode executar o método `db.fsyncUnlock()`. Pode ser aplicado para `cluster`.
	* viewRole: O usuário pode visualizar informações sobre qualquer `role` da `database`. Pode ser aplicado para `database`.
	* viewUser: O usuário pode visualizar informações de qualquer usuário da `database`. Pode ser aplicado para `database`.

* Deployment Management Actions
	* authSchemaUpgrade: O usuário poderá executar o comando `authSchemaUpdate`. Pode ser aplicado para `cluster`.
	* cleanupOrphaned: O usuário poderá executar o comando `cleanupOrphaned`. Pode ser aplicado para `cluster`.
	* cpuProfiler: O usuário poderá habilitar e utilizar o profiler da CPU. Pode ser aplicado para `cluster`.
	* inprog: O usuário poderá utilizar o método `db.currentOp()` para retornar as operações ativas e as operações pendentes. Pode ser aplicado para `cluster`.
	* invalidateUserCache: Permite acesso ao comando `invalidateUserCache`. Pode ser aplicado para `cluster`.
	* killop: O usuário poderá executar o método `db.killOp()`. Pode ser aplicado para `cluster`.
	* planCacheRead: O usuário poderá executar os comandos `planCacheListPlans`, `planCacheListQueryShapes` e os seguintes métodos: `PlanCache.getPlansByQuery()`, `PlanCache.listQueryByShapes()`. Pode ser aplicado para `database` ou `collection`.
	* planCacheWrite: O usuário poderá executar o comando `planCacheClear` e os métodos `PlanCache.clear()` `PlanCache.clearPlansByQuery()`. Pode ser aplicado para `database` ou `collection`.
	* storageDetails: O usuário poderá executar o comando `storageDetails`. Pode ser aplicado para `database` ou `collection`.

* Replication Actions
	* appendOplogNote: O usuário poderá acrescentar notas ao oplog.	Pode ser aplicado para `cluster`.
	* replSetConfigure: O usuário poderá configurar o `replica set`. Pode ser aplicado para `cluster`.
	* replSetGetStatus: O usuário poderá executar o comando `replSetGetStatus`. Pode ser aplicado para `cluster`.
	* replSetHeartbeat: O usuário poderá executar o comando `replSetHeartbeat`. Pode ser aplicado para `cluster`.
	* replSetStateChange: O usuário poderá alterar o estado da `replica set` através dos comandos: `replSetFreeze`, `replSetMaintence`, `replSetStepDown` e `replSetSyncFrom`. Pode ser aplicado para `cluster`.
	* resync: O usuário poderá executar o comando `resync`. Pode ser aplicado para `cluster`.

* Sharding Actions
	* addShard: O usuário poderá executar o comando `addShard`. Pode ser aplicado para `cluster`.
	* enableSharding: O usuário poderá habilitar `sharding` na `database` utilizando o comando `enableSharding` e poderá fazer `shard` em `collections` utilizando o comando `shardCollection`. Pode ser aplicado para `database` ou `collection`.	
	* flushRouterConfig: O usuário poderá executar o comando `flushRouterConfig`. Pode ser aplicado para `cluster`.
	* getShardMap: O usuário poderá executar o comando `getShardMap` Pode ser aplicado para `cluster`.
	* getShardVersion: O usuário poderá executar o comando `getShardVersion`. Pode ser aplicado para `database`.
	* listShards: O usuário poderá executar o comando `listshards`. Pode ser aplicado para `cluster`.
	* moveChunk: O usuário pode executar o comando `moveChunk`. Além disso, o usuário também poderá executar o comando `movePrimary` deste que o privilégio esteja aplicado à `database` apropriada.
	* removeShard: O usuário poderá executar o comando `removeShard`. Pode ser aplicado para `cluster`.
	* shardingState: Oo usuário poderá executar o comando `shardingState`. Pode ser aplicado para `cluster`.
	* splitChunk: O usuário poderá executar o comando `splitChunk`. Pode ser aplicado para `database` ou `collection`.
	* splitVector: O usuário poderá executar o comando `splitVector`. Pode ser aplicado para `database` ou `collection`.

* Server Administration Actions
	* applicationMessage: O usuário poderá executar o comando `logApplicationMessage`. Pode ser aplicado para `cluster`.
	* closeAllDatabases: O usuário poderá executar o comando `closeAllDatabases`. Pode ser aplicado para `cluster`.
	* collMod: O usuário poderá executar o comando `collMod`. Pode ser aplicado para `database` ou `collection`.
	* compact: O usuário poderá executar o comando `compact`. Pode ser aplicado para `database` ou `collection`.
	* connPoolSync: O usuário poderá executar o comando `connPoolSync`. Pode ser aplicado para `cluster`.
	* convertToCapped: O usuário poderá executar o comando `convertToCapped`. Pode ser aplicado para `database` ou `collection`.
	* dropDatabase: O usuário poderá executar o comando `dropDatabase`. Pode ser aplicado para `database`.
	* dropIndex: O usuário poderá executar o comando `dropIndexes`. Pode ser aplicado para `cluster`.
	* fsync: O usuário poderá executar o comando `fsync`. Pode ser aplicado para `cluster`.
	* getParameter: O usuário poderá executar o comando `getParameter`. Pode ser aplicado para `cluster`.
	* hostInfo: Provê informações sobre a instância do servidor de MongoDB. Pode ser aplicado para `cluster`.
	* logRotate: O usuário poderá executar o comando `logRotate`. Pode ser aplicado para `cluster`.
	* reIndex: O usuário poderá executar o comando `reIndex`. Pode ser aplicado para `database` ou `collection`.
	* renameCollectionSameDB: Permite que o usuário possa renomear `collections` da `database` atual utilizando o comando `renameCollection`. Pode ser aplicado para `database`. 
	* repairDatabase: O usuário poderá executar o comando `repairDatabase`. Pode ser aplicado para `database`.
	* setParameter: O usuário poderá executar o comando `setParameter`. Pode ser aplicado para `cluster`.
	* shutdown: O usuário poderá executar o comando `shutdown`. Pode ser aplicado para `cluster`.
	* touch: O usuário poderá executar o comando `touch`. Pode ser aplicado para `cluster`.

* Diagnostic Actions
	* collStats: O usuário poderá executar o comando `collStats`. Pode ser aplicado para `database` ou `collection`.
	* connPoolStats: O usuário poderá executar os comandos `connPoolStats` e `shardconnPoolStats`. Pode ser aplicado para `cluster`.
	* cursosInfo: O usuário poderá executar o comando `cursorInfo`. Pode ser aplicado para `cluster`.
	* dbHash: O usuário poderá executar o comando `dbHash`. Pode ser aplicado para `database` ou `collection`.
	* dbStats: O usuário poderá executar o comando `dbStats`. Pode ser aplicado para `database`.
	* diagLogging: O usuário poderá executar o comando `diagLogging`. Pode ser aplicado para `cluster`.
	* getCmdLineOpts: O usuário poderá executar o comando `getCmdLineOpts`. Pode ser aplicado para `cluster`.
	* getLog: O usuário poderá executar o comando `getLog`. Pode ser aplicado para `cluster`.
	* indexStats: 	O usuário poderá executar o comando `indexStats`. Pode ser aplicado para `database` ou `collection`. **Alteração no MongoDB 3.0: MongoDB 3.0 removou o comando `indexStats`.
	* listDatabases: O usuário poderá executar o comando `listDatabases`. Pode ser aplicado para `cluster`.
	* listCollections: O usuário poderá executar o comando `listCollections`. Pode ser aplicado para `database`.
	* listIndexes: O usuário poderá executar o comando `ListIndexes`. Pode ser aplicado para `database` ou `collection`.
	* netstat: O usuário poderá executar o comando `netstat`. Pode ser aplicado para `cluster`.
	* serverStatus: O usuário poderá executar o comando `serverStatus`. Pode ser aplicado para `cluster`.
	* validate: O usuário poderá executar o comando `validate`. Pode ser aplicado para `database` ou `collection`.
	* top: O usuário poderá executar o comando `top`. Pode ser aplicado para `cluster`.

* Internal Actions
	* anyAction: Permite qualquer ação em um recurso. **Não utilize, exceto em circunstâncias excepcionais.**
	* internal: Permite ações internas. **Não utilize, exceto em circunstâncias excepcionais.**