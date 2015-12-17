# MongoDb - Artigo sobre Autenticação no MongoDb
**User:** daniofilho

**Autor:** Dânio Aparecido Mendonça Filho

**Data** 1450264382113

## Qual a diferença entre Autenticação e Autorização? ##

Cada usuário registrado possui suas credenciais de acesso, como por exemplo email, senha, chaves de segurança, etc. A Autenticação em um banco é o processo de confirmar se o usuário que está tentando ter acesso a conta é realmente aquele usuário registrado, com base nas informações inseridas.

Este usuário também possui seus níveis de acesso, ou seja, existem ações dentro de um sistema que o usuário pode ou não pode fazer. Autorização é o processo que, após a autenticação, verifica se o usuário possui nível de acesso suficiente para executar uma ação, como por exemplo excluir um registro.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Todas as informações de autenticação do mongo ficam na coleção `system.users` no banco `admin`.

O processo de criação de usuários no MongoDB é bem simples. Usa-se o comando `db.createUser()`.

**Sintaxe**

~~~ js
{
   createUser: "<nome>",
   pwd: "<senha em texto puro>",
   customData: "<demais informações relevantes>",
   roles:[
      { role: "<permissao>", db: "<qual banco?>" | "permissao"
      ...
   ],
   digestPassword: boolean, //opcional, caso queira que o mongo crie a senha automaticamente
   writeConcern: { <write concern> }
}
~~~

### Criando um usuário comum ###

A diferença entre usuários é apenas a `role` que cada um possui, portanto para criar um usuário comum, devemos apenas criar um usuário com a role de `read`.

~~~ js
db.createUser({
   user: 'daniofilho',
   pwd: 'comum123',
   roles: [ { role: 'read', db: 'admin' } ]
})
~~~


### Criando um usuário administrador ###

Agora para se criar um administrador, devemos passar como parâmetro a role `userAdminAnyDatabase`.

~~~ js
db.createUser({
   user: 'daniofilhoAdmin',
   pwd: 'adm123',
   roles: [ { role: 'userAdminAnyDatabase', db: 'admin' } ]
})
~~~

## Explique cada papel listado em Cluster Administration Roles. ##

### Cluster Administration Roles ###

O banco `admin` possui as seguintes roles de administração de todo o sistema ao invés de apenas um banco. Essas roles incluem, mas não se limitam, a replica_set e as funções administrativas do sharded_cluster.

**clusterAdmin**

Dá o maior acesso possível ao gerenciamento de clusters. Esta role combina os acessos concedidos por `clusterManager`, `clusterMonitor` e `hostManager`. Esta role também da acesso a ação de `dropDatabase`.

**clusterManager**

Permite o gerenciamento e monitoramento das ações no cluster. Usuário com esse acesso podem configurar bancos locais que são usados em sharding e replication respectivamente.

Esta role permite as ações gerais em um cluster:

- addShard
- applicationMessage
- cleanupOrphaned
- flushRouterConfig
- listShards
- removeShard
- replSetConfigure
- replSetGetStatus
- replSetStateChange
- resync

Permite as ações em todos os bancos no cluster:

- enableSharding
- moveChunk
- splitChunk
- splitVector

No banco `config`, permite as seguintes ações na collection `settings`:

- insert
- remove
- update

No banco `config`, permite as seguintes ações em todas as configurações de collections e nas collections `system.indexes`, `system.js` e `system.namespaces`:

- collStats
- dbHash
- dbStats
- find
- killCursors

Em bancos locais, permite as seguintes ações na collection `replset`:

- collStats
- dbHash
- dbStats
- find
- killCursors

**clusterMonitor**

Esta role da acesso de leitura apenas as ferramentas de monitoramento, como o agente de monitoramente MongoDB Cloud Manager.

Permite as ações gerais em um cluster:

- connPoolStats
- cursorInfo
- getCmdLineOpts
- getLog
- getParameter
- getShardMap
- hostInfo
- inprog
- listDatabases
- listShards
- netstat
- replSetGetStatus
- serverStatus
- shardingState
- top

Permite as seguintes ações em todos os bancos no cluster:

- collStats
- dbStats
- getShardVersion

Permite a ação de `find` em todas as collections `system.profile` no cluster.

Permite as seguintes ações de configuração de collections no banco `config` e nas colections `system.indexes`, `system.js`, `system.namespaces`:ns:

- collStats
- dbHash
- dbStats
- find
- killCursors

**hostManager**

Garante permissões para monitorar e gerenciar os servidores.

Permite as ações gerais em um cluster:

- applicationMessage
- closeAllDatabases
- connPoolSync
- cpuProfiler
- diagLogging
- flushRouterConfig
- fsync
- invalidateUserCache
- killop
- logRotate
- resync
- setParameter
- shutdown
- touch
- unlock

Permite as seguintes ações em todos os bancos no cluster:

- killCursors
- repairDatabase

## Explique todas as ações de privilégio listadas em Privilege Actions.

TRADUZIR

As Privilege Actions definem operações que um usuário pode realizar em um recurso.

Temos as seguintes operações:

###Query and Write Actions###

*Todos os comandos abaixo podem ser aplicados para um banco ou uma collection.*

`find`

Permite a execução do método `db.collection.find()`.

`insert`

Permite a execução do método `insert`.

`remove`

Permite a execução do método `db.collection.remove()`.

`update`

Permite a execução do método `update`.

### Database Management Actions ###

`changeCustomData`

Um usuário pode mudar uma informação customizada de qualquer usuário em um banco.

`changeOwnCustomData`

Usuários podem mudar suas próprias informações customizadas.

`changeOwnPassword`

Usuários podem mudar suas próprias senhas.

`changePassword`

Usuário pode mudar a senha de qualquer usuário no banco.

`createCollection`

Usuários podem executar o método `db.createCollection()` para criar collections.

`createIndex`

Permite acesso ao método `db.collection.createIndex()` e ao comando `createIndexes`.

`createRole`

Usuário pode criar uma nova role no banco.

`createUser`

Usuário pode criar novos usuários no banco.

`dropCollection`

Permite acesso ao método `db.collection.drop()` para excluir uma collection.

`dropRole`

Usuário pode deletar qualquer role do banco

`dropUser`

Usuário pode excluir qualquer usuário no banco.

`emptycapped`

Permite acesso ao comando `emptycapped`.

`enableProfiler`

Permite acesso ao método `db.setProfilingLevel()`.

`grantRole`

Usuário pode definir qualquer role do banco para qualquer usuário no banco.

`killCursors`

Usuário pode parar um cursor na collection desejada.

`revokeRole`

Usuário pode remover qualquer role do banco.

`unlock`

Permite acesso ao método `db.fsyncUnlock()`.

`viewRole`

Usuário pode ver qualquer informação de qualquer role no banco.

`viewUser`

Usuáiro pode ver qualquer informação de qualquer usuário no banco.

### Deployment Management Actions ###

`authSchemaUpgrade`

Permite acesso ao comando `authSchemaUpgrade`.

`cleanupOrphaned`

Permite acesso ao comando `cleanupOrphaned command`.

`cpuProfiler`

Usuário pode habilitar e desabilitar o `CPU profiler`.

`inprog`

Permite acesso ao método `db.currentOp()` para retornar operações pendentes e ativas.

`invalidateUserCache`

Permite acesso ao comando `invalidateUserCache`.

`killop`

Permite acesso ao método `db.killOp()`.

`planCacheRead`

Permite acesso aos comandos `planCacheListPlans`, `planCacheListQueryShapes` e aos métodos `PlanCache.getPlansByQuery()` e `PlanCache.listQueryByShapes()`.

`planCacheWrite`

Permite acesso ao comando `planCacheClear` e aos métodos` PlanCache.clear()` e `PlanCache.clearPlansByQuery()`.

`storageDetails`

Permite acesso ao comando `storageDetails`.

### Replication Actions ###

`appendOplogNote`

Permite acrescentar notas ao oplog.

`replSetConfigure`

Permite configurar o replica set.

`replSetGetStatus`

Permite acesso ao comando `replSetGetStatus`.

`replSetHeartbeat`

Permite acesso ao comand `replSetHeartbeat`.

`replSetStateChange`

Permite alterar o estado da replica set através com os comandos: `replSetFreeze`, `replSetMaintence`, `replSetStepDown` e `replSetSyncFrom`.

`resync`

Permite acesso ao comando `resync`.

### Sharding Actions ###

`addShard`

Permite acesso ao comando addShard.

`enableSharding`

Permite habilitar sharding no banco com o comando `enableSharding` e permite fazer shard em collections com o comando `shardCollection`.

`flushRouterConfig`

Permite acesso ao comando `flushRouterConfig`.

`getShardMap`

Permite acesso ao comando `getShardMap`.

`getShardVersion`

Permite acesso ao comando `getShardVersion`.

`listShards`

Permite acesso ao comando `listshards`.

`moveChunk`

Permite acesso ao comando `moveChunk`. Além disso, o usuário pode executar o comando `movePrimary` do banco ao qual está com acesso.

`removeShard`

Permite acesso ao comando `removeShard`.

`shardingState`

OPermite acesso ao comando `shardingState`.

`splitChunk`

Permite acesso ao comando `splitChunk`.

`splitVector`

Permite acesso ao comando `splitVector`.

### Server Administration Actions ###

`applicationMessage`

Permite acesso ao comando `logApplicationMessage`.

`closeAllDatabases`

Permite acesso ao comando `closeAllDatabases`.

`collMod`

Permite acesso ao comando `collMod`.

`compact`

Permite acesso ao comando `compact`.

`connPoolSync`

Permite acesso ao comando `connPoolSync`.

`convertToCapped`

Permite acesso ao comando `convertToCapped`.

`dropDatabase`

Permite acesso ao comando `dropDatabase`.

`dropIndex`

Permite acesso ao comando `dropIndexes`.

`fsync`

Permite acesso ao comando `fsync`.

`getParameter`

Permite acesso ao comando `getParameter`.

`hostInfo`

Permite acesso as informações sobre a instância do servidor do Mongo.

`logRotate`

Permite acesso ao comando `logRotate`.

`reIndex`

Permite acesso ao comando `reIndex`.

`renameCollectionSameDB`

Permite que o usuário possa renomear collections da database atual utilizando o comando renameCollection.

`repairDatabase`

Permite acesso ao comando `repairDatabase`.

`setParameter`

Permite acesso ao comando `setParameter`.

`shutdown`

Permite acesso ao comando `shutdown`.

`touch`

Permite acesso ao comando `touch`.

Diagnostic Actions

`collStats`
Permite acesso ao comando `collStats`.

`connPoolStats`

Permite acesso aos comandos `connPoolStats` e `shardconnPoolStats`.

`cursosInfo`

Permite acesso ao comando `cursorInfo`.

`dbHash`

Permite acesso ao comando `dbHash`.

`dbStats`

Permite acesso ao comando `dbStats`.

`diagLogging`

Permite acesso ao comando `diagLogging`.

`getCmdLineOpts`

Permite acesso ao comando `getCmdLineOpts`.

`getLog`

Permite acesso ao comando `getLog`.

`indexStats`

Permite acesso ao comando `indexStats`.  

*removido da versão 3.0 do Mongo*.

`listDatabases`

Permite acesso ao comando `listDatabases`.

`listCollections`

Permite acesso ao comando `listCollections`.

`listIndexes`

Permite acesso ao comando `ListIndexes`.

`netstat`

Permite acesso ao comando `netstat`.

`serverStatus`

Permite acesso ao comando `serverStatus`.

`validate`

Permite acesso ao comando `validate`.

`top`

Permite acesso ao comando `top`.

### Internal Actions ###

`anyAction`

Permite qualquer ação no recurso. Não defina essa ação exceto em circunstancias de exeção.

`internal`

Permite ações internas

Não defina essa ação exceto em circunstancias de exeção.
