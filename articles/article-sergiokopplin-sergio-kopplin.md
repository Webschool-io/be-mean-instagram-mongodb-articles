# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Sergio Kopplin
**Data** 01/28/2016 21:05pm

## Qual a diferença entre Autenticação e Autorização?

Autenticar é enviar dados do usuário para o servidor, com ou sem criptografia.
Autorizar é autenticar o usuário após uma validação, permitindo ou não o acesso do mesmo

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

```
# administrador
use admin

db.createUser({
    user: "kopplin",
    pwd: "admin123",
    roles: [{
        role: "userAdminAnyDatabase",
        db: "admin"
    }]
})

Successfully added user: {
  "user": "kopplin",
  "roles": [
    {
      "role": "userAdminAnyDatabase",
      "db": "admin"
    }
  ]
}

#comum

use admin

db.createUser({
    user: "kopplinuser",
    pwd: "user123",
    roles: [{
        role: "read",
        db: "be-mean"
    },
    "readWrite"
    ]
})

Successfully added user: {
  "user": "kopplinuser",
  "roles": [
    {
      "role": "read",
      "db": "be-mean"
    },
    "readWrite"
  ]
}
```

## Explique cada papel listado em Cluster Administration Roles.

clusterAdmin: Possui o maior acesso para gerenciamento do Cluster. É a combinação dos previlégiso do *clusterManager*, *clusterMonitor*, e *hostManager*. Possuindo ainda a função de poder realizar `dropDatabase`.

clusterManager: Pode realizar o gerenciamento e monitoração das ações no Cluster. Ainda pode acessar o *config*, para `sharding`, e os bancos locais, para `replication`.

clusterMonitor: Pode realizar apenas a leitura (*read-only*) nas ferramentas de monitoração, como o *MongoDB Cloud Manager* e *Ops Manager monitoring agent*.

hostManager: Possui a abilidade de monitorar e gerenciar os servidores.

---

## Explique todas as ações de privilégio listadas em Privilege Actions.

As *Privilege Actions* definem as operações que um usuário pode realizar no seu recurso. Listadas abaixo pelo seu propósito:


### Query and Write Actions

find: o usuário poderá executar o método `db.collection.find()` em uma *collection*.
insert: o usuário poderá executar os métodos de inserção em uma *collection*.
remove: o usuário poderá executar o método `db.collection.remove()` em uma *collection*.
update: o usuário poderá executar os métodos de atualização em uma *collection*.

### Database Management Actions

changeCustomData: o usuário poderá alterar qualquer informação de qualquer outro usuário do banco informado.
changeOwnCustomData: o usuário poderá alterar qualquer informação sua.
changeOwnPassword: o usuário poderá alterar sua senha.
changePassword: o usuário poderá alterar a senha de qualquer outro usuário do banco informado.
createCollection: o usuário poderá executar o método `db.createCollection()`.
createIndex: o usuário poderá executar o método `db.collection.createIndex()` e o comando `createIndexes`.
createRole: o usuário poderá criar novas `roles` no banco informado.
createUser: o usuário poderá cirar novos usuário no banco informado.
dropCollection: o usuário poderá executar o método `db.collection.drop()`.
dropRole: o usuário poderá deletar qualquer `role` do banco informado.
dropUser: o usuário poderá remover qualquer usuário do banco informado.
emptycapped: o usuário poderá executar o comando `emptycapped`.
enableProfiler: o usuário poderá executar o método `db.setProfilingLevel()`.
grantRole: o usuário poderá atribuir qualquer `role` do banco para qualquer usuário do banco.
killCursors: o usuário poderá matar/derrubar cursores na `collection` selecionado.
revokeRole: o usuário poderá remover qualquer `role` de qualquer usuário do banco.
unlock:  o usuário poderá executar o método `db.fsyncUnlock()`.
viewRole: o usuário poderá ver qualquer informação de qualquer `role` no banco informado.
viewUser: o usuário poderá ver as informações de qualquer usuário no banco informado.

### Deployment Management Actions

authSchemaUpgrade: o usuário poderá executar o comando `authSchemaUpgrade`.
cleanupOrphaned: o usuário poderá executar o comando `cleanupOrphaned`.
cpuProfiler: o usuário poderá abilitar e usar o *CPU profiler*.
inprog: o usuário poderá usar método `db.currentOp()` para retornar as operações ativas pendentes.
invalidateUserCache: prove o acesso ao comando `invalidateUserCache`.
killop: o usuário poderá executar o método `db.killOp()`.
planCacheRead: o usuário poderá executar os comandos `planCacheListPlans` e `planCacheListQueryShapes`, e os métodos `PlanCache.getPlansByQuery()` e `PlanCache.listQueryShaps()`.
planCacheWrite: o usuário poderá executar o comando `planCacheClear`, e os métodos `PlanCache.clear()` and `PlanCache.clearPlansByQuery()`.
storageDetails: o usuário poderá executar o comando `storageDetails`.

### Replication Actions

appendOplogNote: o usuário poderá anexar notas ao `oplog`.
replSetConfigure: o usuário poderá configurar o `replica set`.
replSetGetStatus: o usuário poderá executar o comando `replSetGetStatus`.
replSetHeartbeat: o usuário poderá executar o comando `replSetHeartbeat`.
replSetStateChange: o usuário poderá mudar o estado do `replica set` atravez dos comandos `replSetFreeze`, `replSetMaintenance`, `replSetStepDown`, e `replSetSyncFrom`.
resync: o usuário poderá executar o comando `resync`.

### Sharding Actions

addShard: o usuário poderá executar o comando `addShard`.
enableSharding: o usuário poderá abilitar `shargind` no banco utilizado o comando `enableSharding` e também poderá *shardear* uma `collection` utilizando o comando `shardCollection`.
flushRouterConfig: o usuário poderá executar o comando `flushRouterConfig`.
getShardMap: o usuário poderá exeutar o comando `getShardMap`.
User can perform the getShardMap command. Apply this action to the cluster resource.
getShardVersion: o usuário poderá exeutar o comando `getShardVersion`.
listShards: o usuário poderá exeutar o comando `listShards`.
moveChunk: o usuário poderá executar o comando `moveChunk`. E também executar o comando `movePrimary`.
removeShard: o usuário poderá executar o comando `removeShard`.
shardingState: o usuário poderá executar o comando `shardingState`.
splitChunk: o usuário poderá executar o comando `splitChunk`.
splitVector: o usuário poderá executar o comando `splitVector`.

### Server Administration Actions

applicationMessage: o usuário poderá executar o comando `logApplicationMessage`.
closeAllDatabases: o usuário poderá executar o comando `closeAllDatabases`.
collMod: o usuário poderá executar o comando `collMod`.
compact: o usuário poderá executar o comando `compact`.
connPoolSync: o usuário poderá executar o comando `connPoolSync`.
convertToCapped: o usuário poderá executar o comando `convertToCapped`.
dropDatabase: o usuário poderá executar o comando `dropDatabase`.
dropIndex: o usuário poderá executar o comando `dropIndexes`.
fsync: o usuário poderá executar o comando `fsync`.
getParameter: o usuário poderá executar o comando `getParameter`.
hostInfo: fornece informações sobre o server do MongoDB que está rodando.
logRotate: o usuário poderá executar o comando `logRotate`.
reIndex: o usuário poderá executar o comando `reIndex`.
User can perform the reIndex command. Apply this action to database or collection resources.
renameCollectionSameDB: permite ao usário renomear `collections` no banco atual, utilizando o comando `renameCollection`.
repairDatabase: o usuário poderá executar o comando `repairDatabase`.
setParameter: o usuário poderá executar o comando `setParameter`.
shutdown: o usuário poderá executar o comando `shutdown`.
touch: o usuário poderá executar o comando `touch`.

### Diagnostic Actions

collStats: o usuário poderá executar o comando `collStats`.
connPoolStats: o usuário poderá executar os comandos `connPoolStats` e `shardConnPoolStats`.
cursorInfo: o usuário poderá executar o comando `cursorInfo`.
dbHash: o usuário poderá executar o comando `dbHash`.
dbStats: o usuário poderá executar o comando `dbStats`.
diagLogging: o usuário poderá executar o comando `diagLogging`.
getCmdLineOpts: o usuário poderá executar o comando `getCmdLineOpts`.
getLog: o usuário poderá executar o comando `getLog`.
indexStats: o usuário poderá executar o comando `indexStats`.
Changed in version 3.0: MongoDB 3.0 removes the indexStats command.
listDatabases: o usuário poderá executar o comando `listDatabases`.
listCollections: o usuário poderá executar o comando `listCollections`.
listIndexes: o usuário poderá executar o comando `ListIndexes`.
netstat: o usuário poderá executar o comando `netstat`.
serverStatus: o usuário poderá executar o comando `serverStatus`.
validate: o usuário poderá executar o comando `validate`.
top: o usuário poderá executar o comando `top`.

### Internal Actions

anyAction: permite qualquer ação no `resource`, em algunas circunstâncias.
internal: permite ações internas, em algunas circunstâncias.
