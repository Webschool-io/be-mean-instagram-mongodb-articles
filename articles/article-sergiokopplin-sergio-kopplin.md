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

**find**
O usuário poderá executar o método `db.collection.find()` em uma *collection*.

**insert**
O usuário poderá executar os métodos de inserção em uma *collection*.

**remove**
O usuário poderá executar o método `db.collection.remove()` em uma *collection*.

**update**
O usuário poderá executar os métodos de atualização em uma *collection*.

---

### Database Management Actions

**changeCustomData**
O usuário poderá alterar qualquer informação de qualquer outro usuário do banco informado.

**changeOwnCustomData**
O usuário poderá alterar qualquer informação sua.

**changeOwnPassword**
O usuário poderá alterar sua senha.

**changePassword**
O usuário poderá alterar a senha de qualquer outro usuário do banco informado.

**createCollection**
O usuário poderá executar o método `db.createCollection()`.

**createIndex**
O usuário poderá executar o método `db.collection.createIndex()` e o comando `createIndexes`.

**createRole**
O usuário poderá criar novas `roles` no banco informado.

**createUser**
O usuário poderá cirar novos usuário no banco informado.

**dropCollection**
O usuário poderá executar o método `db.collection.drop()`.

**dropRole**
O usuário poderá deletar qualquer `role` do banco informado.

**dropUser**
O usuário poderá remover qualquer usuário do banco informado.

**emptycapped**
O usuário poderá executar o comando `emptycapped`.

**enableProfiler**
O usuário poderá executar o método `db.setProfilingLevel()`.

**grantRole**
O usuário poderá atribuir qualquer `role` do banco para qualquer usuário do banco.

**killCursors**
O usuário poderá matar/derrubar cursores na `collection` selecionado.

**revokeRole**
O usuário poderá remover qualquer `role` de qualquer usuário do banco.

**unlock**
Oo usuário poderá executar o método `db.fsyncUnlock()`.

**viewRole**
O usuário poderá ver qualquer informação de qualquer `role` no banco informado.

**viewUser**
O usuário poderá ver as informações de qualquer usuário no banco informado.

---

### Deployment Management Actions

**authSchemaUpgrade**
O usuário poderá executar o comando `authSchemaUpgrade`.

**cleanupOrphaned**
O usuário poderá executar o comando `cleanupOrphaned`.

**cpuProfiler**
O usuário poderá abilitar e usar o *CPU profiler*.

**inprog**
O usuário poderá usar método `db.currentOp()` para retornar as operações ativas pendentes.

**invalidateUserCache**
Orove o acesso ao comando `invalidateUserCache`.

**killop**
O usuário poderá executar o método `db.killOp()`.

**planCacheRead**
O usuário poderá executar os comandos `planCacheListPlans` e `planCacheListQueryShapes`, e os métodos `PlanCache.getPlansByQuery()` e `PlanCache.listQueryShaps()`.

**planCacheWrite**
O usuário poderá executar o comando `planCacheClear`, e os métodos `PlanCache.clear()` and `PlanCache.clearPlansByQuery()`.

**storageDetails**
O usuário poderá executar o comando `storageDetails`.

---

### Replication Actions

**appendOplogNote**
O usuário poderá anexar notas ao `oplog`.

**replSetConfigure**
O usuário poderá configurar o `replica set`.

**replSetGetStatus**
O usuário poderá executar o comando `replSetGetStatus`.

**replSetHeartbeat**
O usuário poderá executar o comando `replSetHeartbeat`.

**replSetStateChange**
O usuário poderá mudar o estado do `replica set` atravez dos comandos `replSetFreeze`, `replSetMaintenance`, `replSetStepDown`, e `replSetSyncFrom`.

**resync**
O usuário poderá executar o comando `resync`.

---

### Sharding Actions

**addShard**
O usuário poderá executar o comando `addShard`.

**enableSharding**
O usuário poderá abilitar `shargind` no banco utilizado o comando `enableSharding` e também poderá *shardear* uma `collection` utilizando o comando `shardCollection`.

**flushRouterConfig**
O usuário poderá executar o comando `flushRouterConfig`.

**getShardMap**
O usuário poderá exeutar o comando `getShardMap`.
User can perform the getShardMap command. Apply this action to the cluster resource.

**getShardVersion**
O usuário poderá exeutar o comando `getShardVersion`.

**listShards**
O usuário poderá exeutar o comando `listShards`.

**moveChunk**
O usuário poderá executar o comando `moveChunk`. E também executar o comando `movePrimary`.

**removeShard**
O usuário poderá executar o comando `removeShard`.

**shardingState**
O usuário poderá executar o comando `shardingState`.

**splitChunk**
O usuário poderá executar o comando `splitChunk`.

**splitVector**
O usuário poderá executar o comando `splitVector`.

---

### Server Administration Actions

**applicationMessage**
O usuário poderá executar o comando `logApplicationMessage`.

**closeAllDatabases**
O usuário poderá executar o comando `closeAllDatabases`.

**collMod**
O usuário poderá executar o comando `collMod`.

**compact**
O usuário poderá executar o comando `compact`.

**connPoolSync**
O usuário poderá executar o comando `connPoolSync`.

**convertToCapped**
O usuário poderá executar o comando `convertToCapped`.

**dropDatabase**
O usuário poderá executar o comando `dropDatabase`.

**dropIndex**
O usuário poderá executar o comando `dropIndexes`.

**fsync**
O usuário poderá executar o comando `fsync`.

**getParameter**
O usuário poderá executar o comando `getParameter`.

**hostInfo**
Oornece informações sobre o server do MongoDB que está rodando.

**logRotate**
O usuário poderá executar o comando `logRotate`.

**reIndex**
O usuário poderá executar o comando `reIndex`.
User can perform the reIndex command. Apply this action to database or collection resources.

**renameCollectionSameDB**
Oermite ao usário renomear `collections` no banco atual, utilizando o comando `renameCollection`.

**repairDatabase**
O usuário poderá executar o comando `repairDatabase`.

**setParameter**
O usuário poderá executar o comando `setParameter`.

**shutdown**
O usuário poderá executar o comando `shutdown`.

**touch**
O usuário poderá executar o comando `touch`.

---

### Diagnostic Actions

**collStats**
O usuário poderá executar o comando `collStats`.

**connPoolStats**
O usuário poderá executar os comandos `connPoolStats` e `shardConnPoolStats`.

**cursorInfo**
O usuário poderá executar o comando `cursorInfo`.

**dbHash**
O usuário poderá executar o comando `dbHash`.

**dbStats**
O usuário poderá executar o comando `dbStats`.

**diagLogging**
O usuário poderá executar o comando `diagLogging`.

**getCmdLineOpts**
O usuário poderá executar o comando `getCmdLineOpts`.

**getLog**
O usuário poderá executar o comando `getLog`.

**indexStats**
O usuário poderá executar o comando `indexStats`.

**listDatabases**
O usuário poderá executar o comando `listDatabases`.

**listCollections**
O usuário poderá executar o comando `listCollections`.

**listIndexes**
O usuário poderá executar o comando `ListIndexes`.

**netstat**
O usuário poderá executar o comando `netstat`.

**serverStatus**
O usuário poderá executar o comando `serverStatus`.

**validate**
O usuário poderá executar o comando `validate`.

**top**
O usuário poderá executar o comando `top`.

---

### Internal Actions

**anyAction**
Permite qualquer ação no `resource`, em algunas circunstâncias.

**internal**
Permite ações internas, em algunas circunstâncias.
