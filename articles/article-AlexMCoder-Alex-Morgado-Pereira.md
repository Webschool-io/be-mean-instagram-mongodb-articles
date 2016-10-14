# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Alex Morgado Pereira - [AlexMCoder](https://github.com/AlexMCoder)

## Qual a diferença entre Autenticação e Autorização?

A Autenticação verifica a identidade do usuário, e a autorização determinar o que o usuário pode ter acesso. Imaginando assim, parece que um é ligado ao outro, porém, os dois são diferentes.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Primeiramente devemos saber que, para se criar um usuário dentro do MongoDB, devemos utilizar a função **db.createUser()**.

A sintaxe básica da função **db.createUser()** é a seguinte:

```
{
    user: "<nome>",
    pwd: "<senha>",
    roles: [{role: "<função>", db: "<database>"}]
}
```
Observe que o valor **role**, temos que passar a função e o nome do banco, se passarmos apenas o nome do banco, irá ocorrer um erro, como no exemplo abaixo:

```
Error: couldn't add user: Missing expected field "role" :
_getErrorWithCode@src/mongo/shell/utils.js:23:13
DB.prototype.createUser@src/mongo/shell/db.js:1225:11
@(shell):1:1
```
No erro, diz que é necessário passar um valor no campo **role**. Para criarmos um usuário comum, do tipo leutira, basta fazer da seguinte forma:

```
db.createUser(
{
    user: "alex",
    pwd: "alexMCoder",
    roles: [{role: "read", db: "admin"}]
}
)
```
Agora, para criarmos um usuário administrador é o mesmo processo, porém, mudamos a regra do **role**, nesse caso utilizaremos a palavra chave **userAdminAnyDatabase**, quando aplicamos esse valo dentro do **role**, o usuário terá acesso administrador em todo o Banco. Vejamos um exemplo:

```
db.createUser(
{
    user: "alex",
    pwd: "alexMCoder",
    roles: [{role: "userAdminAnyDatabase", db: "admin"}]
}
)
```

## Explique cada papel listado em [Cluster Administration Roles](https://docs.mongodb.org/v2.6/reference/built-in-roles/#cluster-administration-roles).

##clusterAdmin

Fornece o maior acesso de gerenciamento de cluster. Esta permissão combina os privilégios concedidos pelas permissões **clusterManager**, **clusterMonitor** e **hostManager**. Além disso, a permissão fornece a ação **dropDatabase**.

##clusterManager

Ele fornce ações do gerenciamente e de monitoramento do cluster, quando o usuário obtem este tipo de permissão ele pode acessar os bancos de dados **config** e **local**.

Fornece acesso às seguintes ações no cluster como um todo:
- [addShard](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.addShard)
- [applicationMessage](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.applicationMessage)
- [cleanupOrphaned](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.cleanupOrphaned)
- [flushRouterConfig](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.flushRouterConfig)
- [listShards](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.listShards)
- [removeShards](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.removeShard)
- [replSetConfigure](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.replSetConfigure)
- [replSetGetStatus](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.replSetGetStatus)
- [replSetStateChange](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.replSetStateChange)
- [resync](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.resync)

Também fornece as seguintes ações em todos os bancos de dados do cluster:
- [enableSharding](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.enableSharding)
- [moveChunk](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.moveChunk)
- [splitChunk](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.splitChunk)
- [splitVector](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.splitVector)

Fornece na collection `settings`, no banco de dados `config`:
- [insert](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.insert)
- [remove](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.remove)
- [update](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.update)
- 
No banco de dados **config**, fornece as ações listadas abaixo em todas as configurações das collections e nas collections `system.indexes`, `system.js` e `system.namespaces`, assim como na collection `replset` do banco de dados `local`.
- [collStats](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.collStats)
- [dbHash](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.dbHash)
- [dbStats](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.dbStats)
- [find](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.find)
- [killCursors](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.killCursors)

## clusterMonitor

Fornece acesso somente leitura às ferramentas de monitoramento, como os agentes de monitoramento `MongoDB Cloud Manager` e `Ops Manager`. Fornece as seguintes ações no cluster como um todo:
- [connPoolStats](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.connPoolStats)
- [cursorInfo](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.cursorInfo)
- [getCmdLineOpts](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.getCmdLineOpts)
- [getLog](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.getLog)
- [getParameter](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.getParameter)
- [getShardMap](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.getShardMap)
- [hostInfo](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.hostInfo)
- [inprog](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.inprog)
- [listDatabases](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.listDatabases)
- [listShards](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.listShards)
- [netstat](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.netstat)
- [replSetGetStatus](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.replSetGetStatus)
- [serverStatus](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.serverStatus)
- [shardingState](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.shardingState)
- [top](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.top)

Além de fornecer as seguintes ações em todos os banco de dados do cluster:
- [collStats](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.collStats)
- [dbStats](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.dbStats)
- [getShardVersion](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.getShardVersion)

Também fornece a ação [find](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.find) em todas as collections `system.profile` do cluster e fornece as seguintes ações em todas as collections do banco de dados `config` e nas collections `system.indexes`, `system.js` e `system.namespaces`:
- [collStats](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.collStats)
- [dbHash](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.dbHash)
- [dbStats](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.dbStats)
- [find](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.find)
- [killCursors](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.killCursors)

## hostManager
Possui a habilidade de gerenciamento e monitoramento dos servidores. Fornece as seguintes ações no cluster como um todo:
- [applicationMessage](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.applicationMessage)
- [closeAllDatabases](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.closeAllDatabases)
- [connPoolStats](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.connPoolSync)
- [cpuProfiler](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.cpuProfiler)
- [diagLogging](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.diagLogging)
- [flushRouterConfig](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.flushRouterConfig)
- [fsync](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.fsync)
- [invalidateUserCache](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.invalidateUserCache)
- [killop](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.killop)
- [logRotate](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.logRotate)
- [resync](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.resync)
- [setParameter](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.setParameter)
- [shutdown](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.shutdown)
- [touch](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.touch)
- [unlock](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.unlock)

E fornece as seguintes ações em todos os banco de dados do cluster:
- [killCursors](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.killCursors)
- [repairDatabase](https://docs.mongodb.com/v3.0/reference/privilege-actions/#authr.repairDatabase)

## Explique todas as ações de privilégio listadas em [Privilege Actions](https://docs.mongodb.org/manual/reference/privilege-actions/).

As ações de privilégio (Privilege Actions) definem as operações que um usuário pode realizar em um determinado recurso. As ações de privlégio disponíveis estão listadas abaixo.

### Ações de consulta e escrita
- **find**

O usuário pode realizar o método `db.collection.find()`.

> Exemplo:
```javascript
db.pokemons.find( { _id: 5 } )
```

- **insert**

O usuário pode realizar o comando `insert`.

> Exemplo:
```javascript
{
    insert: "pokemons",
    documents: [ { _id: 1, nome: "Bulbasaur" } ]
}
```

- **remove**

O usuário pode realizar o comando `db.collection.remove()`.

> Exemplo:
```javascript
db.products.remove( {defense: {$gt: 70 } } )
```

- **update**

O usuário pode realizar o comando `update`.

> Exemplo:
```javascript
{
    update: <collection>,
    udpdates:
    [
    { q: <query_document>, u: <update_document>, upsert: <boolean>, multi: <boolean>  },
    ...
    ]
}
```

### Ações de gerenciamento do banco de dados
- **changeCustomData**

O usuário pode modificar a informação personalizada de qualquer usuário do banco de dados.

- **changeOwnCustomData**

Os usuários pode modificar as informações personalizadas deles próprios.

- **changeOwnPassword**

Os usuários podem modificar as próprias senhas.

- **changePassword**

O usuário pode modificar a senha de qualquer usuário do banco de dados fornecido.

- **createCollection**

O usuário pode executar o método `db.createCollection()`.

- **createIndex**

Fornece acesso ao método `db.collection.createIndex()` e ao comando `createIndexes`.

- **createRole**

O usuário pode criar novas permissões no banco de dados fornecido.

- **createUser**

O usuário pode criar novos usuários no banco de dados fornecido.

- **dropCollection**

O usuário pode executar o método `db.collection.drop()`.

- **dropRole**

O usuário pode remover qualquer permissão do banco de dados que foi fornecido.

- **dropUser**

O usuário pode remover qualquer usuário do banco de dados que foi fornecido.

- **emptycapped**

O usuário pode executar o comando [`emptycapped`](https://docs.mongodb.com/v3.0/reference/command/emptycapped/#dbcmd.emptycapped).

- **enableProfiler**

O usuário pode realizar o comando [`db.setProfilingLevel()`](https://docs.mongodb.com/v3.0/reference/method/db.setProfilingLevel/#db.setProfilingLevel).

- **grantRole**

O usuário pode alterar qualquer permissão do banco de dados para qualquer usuário ou banco de dados do sistema.

- **killCursors**

O usuário pode matar os cursores da coleção alvo.

- **revokeRole**

O usuário pode remover qualquer permissão de qualquer usuário a partir de qualquer banco de dados do sistema.

- **unlock**

O usuário pode executar o método `db.fsyncUnlock()`.

- **viewRole**

O usuário pode visualizar qualquer permissão do banco de dados fornecido.

- **viewUser**

O usuário pode visualizar qualquer informação de qualquer usuário do banco de dados fornecido.

### Ações de gerenciamento de deploy

- **authSchemaUpgrade**

O usuário pode executar o comando `authSchemaUpgrade`. Aplique esta ação no cluster.

- **cleanupOrphaned**

O usuário pode executar o comando `cleanupOrphaned`. Aplique esta ação no cluster.

- **cpuProfiler**

O usuário pode habilitar o usar a `CPU profiler`. Aplique esta ação no cluster.

- **inprog**

O usuário pode executar o método `db.currentOp()` para retornar as operações pendentes e ativas. Aplique esta ação no cluster.

- **invalidateUserCache**

O usuário pode executar o comando `invalidateUserCache`. Aplique esta ação no cluster.

- **killop**

O usuário pode executar o método `db.killOp()`. Aplique esta ação no cluster.

- **planCacheRead**

O usuário pode executar os comandos `planCacheListPlans` e `planCacheListQueryShapes` e os métodos `PlanCache.getPlansByQuery()` e `PlanCache.listQueryShapes()`. Aplique esta ação no banco de dados ou collections.

- **planCacheWrite**

O usuário pode executar o comando `planCacheClear` e os métodos `PlanCache.clear()` e `PlanCache.clearPlansByQuery()`. Aplique esta ação no banco de dados ou collections.

- **storageDetails**

O usuário pode executar o comando `storageDetails`. Aplique esta ação no banco de dados ou collections.

### Ações de replicação

- **appendOplogNote**

O usuário pode anexar notas no oplog. Aplique esta ação no cluster.

- **replSetConfigure**

O usuário pode configurar um conjunto replicado. Aplique esta ação no cluster.

- **replSetGetStatus**

O usuário pode executar o comando `replSetGetStatus`. Aplique esta ação no cluster.

- **replSetHeartbeat**

O usuário pode executar o comando `replSetHeartbeat`. Aplique esta ação no cluster.

- **replSetStateChange**

O usuário pode modificar o estado de um conjunto replicado através dos comandos `replSetFreeze`, `replSetMaintenance`, `replSetStepDown` e `replSetSyncFrom`. Aplique esta ação no cluster.

- **resync**

O usuário pode executar o comando `resync`. Aplique esta ação no cluster.

### Ações de sharding

- **addShard**

O usuário pode executar o comando `addShard`. Aplique esta ação no cluster.

- **enableSharding**

O usuário pode habilitar o sharding em um banco de dados usando o comando `enableSharding` e pode shard uma collection usando o comando `shardCollection`. Aplique esta ação no banco de dados ou collections.

- **flushRouterConfig**

O usuário pode executar o comando `flushRouterConfig`. Aplique esta ação no cluster.

- **getShardMap**

O usuário pode executar o comando `getShardMap`. Aplique esta ação no cluster.

- **getShardVersion**

O usuário pode executar o comando `getShardVersion`. Aplique esta ação no banco de dados ou collections.

- **listShards**

O usuário pode executar o comando `listShards`. Aplique esta ação no cluster.

- **moveChunk**

O usuário pode executar o comando `moveChunk`. Adicionalmente, o usuário também pode executar o comando `movePrimary`. Aplique esta ação no banco de dados ou collections.

- **removeShard**

O usuário pode executar o comando `removeShard`. Aplique esta ação no cluster.

- **shardingState**

O usuário pode executar o comando `shardingState`. Aplique esta ação no cluster.

- **splitChunk**

O usuário pode executar o comando `splitChunk`. Aplique esta ação no banco de dados ou collections.

- **splitVector**

O usuário pode executar o comando `splitVector`. Aplique esta ação no banco de dados ou collections.

### Ações de administração do servidor

- **applicationMessage**

O usuário pode executar o comando `logApplicationMessage`. Aplique esta ação no cluster.

- **closeAllDatabases**

O usuário pode executar o comando `closeAllDatabases`. Aplique esta ação no cluster.

- **collMod**

O usuário pode executar o comando `collMod`. Aplique esta ação no banco de dados ou collections.

- **compact**

O usuário pode executar o comando `compact`. Aplique esta ação no banco de dados ou collections.

- **connPoolSync**

O usuário pode executar o comando `connPoolSync`. Aplique esta ação no cluster.

- **convertToCapped**

O usuário pode executar o comando `convertToCapped`. Aplique esta ação no banco de dados ou collections.

- **dropDatabase**

O usuário pode executar o comando `dropDatabase`. Aplique esta ação no banco de dados.

- **dropIndex**

O usuário pode executar o comando `dropIndexes`. Aplique esta ação no banco de dados e collections.

- **fsync**

O usuário pode executar o comando `fsync`. Aplique esta ação no cluster.

- **getParameter**

O usuário pode executar o comando `getParameter`. Aplique esta ação no cluster.

- **hostInfo**

Fornece informação sobre o servidor que está executando o MongoDB. Aplique esta ação no cluster.

- **logRotate**

O usuário pode executar o comando `logRotate`. Aplique esta ação no cluster.

- **reIndex**

O usuário pode executar o comando `reIndex`. Aplique esta ação no banco de dados ou collections.

- **renameCollectionSameDB**

Permite ao usuário renomear as collections do banco de dados atual usando o comando `renameCollection`. Aplique esta ação no banco de dados.

- **repairDatabase**

O usuário pode executar o comando `repairDatabase`. Aplique esta ação no banco de dados.

- **setParameter**

O usuário pode executar o comando `setParameter`. Aplique esta ação no cluster.

- **shutdown**

O usuário pode executar o comando `sutdown`. Aplique esta ação no cluster.

- **touch**

O usuário pode executar o comando `touch`. Aplique esta ação no cluster.

### Ações de diagnóstico

- **collStats**

O usuário pode executar o comando `collStats`. Aplique esta ação no banco de dados ou collections.

- **connPoolStats**

O usuário pode executar os comandos `connPoolStats` e `shardConnPoolStats`. Aplique esta ação no cluster.

- **cursorInfo**

O usuário pode executar o comando `cursorInfo`. Aplique esta ação no cluster.

- **dbHash**

O usuário pode executar o comando `dbHash`. Aplique esta ação no banco de dados ou collections.

- **dbStats**

O usuário pode executar o comando `dbStats`. Aplique esta ação no banco de dados.

- **diagLogging**

O usuário pode executar o comando `diagLogging`. Aplique esta ação no cluster.

- **getCmdLineOpts**

O usuário pode executar o comando `getCmdLineOpts`. Aplique esta ação no cluster.

- **getLog**

O usuário pode executar o comando `getLog`. Aplique esta ação no cluster.

- **indexStats**

O usuário pode executar o comando `indexStats`. Aplique esta ação no banco de dados ou collections.

> Atenção!

> Mudança da versão 3.0: MongoDB 3.0 removeu o comando `indexStats`.

- **listDatabases**

O usuário pode executar o comando `listDatabases`. Aplique esta ação no cluster.

- **listCollections**

O usuário pode executar o comando `listCollections`. Aplique esta ação no banco de dados.

- **listIndexes**

O usuário pode executar o comando `ListIndexes`. Aplique esta ação no banco de dados ou collections.

- **netstat**

O usuário pode executar o comando `netstat`. Aplique esta ação no cluster.

- **serverStatus**

O usuário pode executar o comando `serverStatus`. Aplique esta ação no cluster.

- **validate**

O usuário pode executar o comando `validate`. Aplique esta ação no banco de dados ou nas collections.

- **top**

O usuário poder executar o comando `top`. Aplique esta ação no cluster.

### Ações internas

- **anyAction**

Permite qualquer ação em um determinado recurso. Não atribua esta ação exceto em circunstâncias exceptionais.

- **internal**

Permite ações internas. Não atribua esta ação exceto em circunstâncias exceptionais.