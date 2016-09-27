# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Mauriene Firmino do Nascimento Júnior
**Data** 1473601672174 

## Qual a diferença entre Autenticação e Autorização?

Autenticação é o processo de identificar o usuário no sistema, validando se ele é um usuário real.

Autorização são os niveis de acesso que o usuário autenticado possui. 


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

####**Administrador:** 

Ùma vez logado no banco, vamos criar nosso usuário administrador, usando o _db.createUser_ passando como parâmetros o user, password (pwd) e nossas roles: _userAdminAnyDatabase_ e _readAnyDatabase_ para o usuário ter leitura e escrita em todos os bancos, e _clusterAdmin_ caso você tenha um cluster.


```
db.createUser( 
{
user: 'admin',
pwd: '123456',
roles: [ 'userAdminAnyDatabase', 'readAnyDatabase', 'clusterAdmin' ] 
})
```

####**Comum:** 

Uma vez logado no banco, criamos um usuário da mesma maneira que anteriormente, porém dessa vez sem as roles.


```
db.createUser(
   {
     user: 'comum',
     pwd: '123456',
     roles: [ ]
   }
)
```


## Explique cada papel listado em Cluster Administration Roles.

####**clusterAdmin**
	
O maior nivel de acesso no cluster, contém todos os previlégios das roles _clusterManager_ _clusterMonitor_ e _hostManager_, e também a permissão para excluir o cluster.

####**clusterManager**

Permite gerencia e monitoramento de ações no cluster. Um usuário com esse acesso pode acessar configurações e bancos locais. Possui as seguintes ações no cluster:


    addShard
    applicationMessage
    cleanupOrphaned
    flushRouterConfig
    listShards
    removeShard
    replSetConfigure
    replSetGetStatus
    replSetStateChange
    resync

Possui as seguintes ações em todos os bancos:

    enableSharding
    moveChunk
    splitChunk
    splitVector

Na database de configuração, possui as seguintes ações na coleção 'settings':

    insert
    remove
    update

Na database de configuração, possui as seguintes ações nas coleções 'system.indexes', 'system.js', e 'system.namespaces':

    collStats
    dbHash
    dbStats
    find
    killCursors

Na database local, possui as sequites ações na coleção 'replset':

    collStats
    dbHash
    dbStats
    find
    killCursors


####**clusterMonitor**

Permite acesso somente de leitura para ferramentas de monitoramento, como o MongoDB Cloud Manager monitoring agent. Fornece as seguintes ações no cluster como um todo:

    connPoolStats
    cursorInfo
    getCmdLineOpts
    getLog
    getParameter
    getShardMap
    hostInfo
    inprog
    listDatabases
    listShards
    netstat
    replSetGetStatus
    serverStatus
    shardingState
    top

Permite as seguintes ações em todas as databases do cluster:

	collStats
	dbStats
	getShardVersion

Permite a ação 'find' nas coleções 'system.profile' do cluster. 

Permite as seguintes ações nas coleções da database de configuração: system.indexes', 'system.js', e 'system.namespaces':

	collStats
	dbHash
	dbStats
	find
	killCursors	

####**hostManager**

Permite a capacidade de monitorar e gerenciar servidores.

Permite as seguintes ações no cluster como um todo:

	applicationMessage
	closeAllDatabases
	connPoolSync
	cpuProfiler
	diagLogging
	flushRouterConfig
	fsync
	invalidateUserCache
	killop
	logRotate
	resync
	setParameter
	shutdown
	touch
	unlock

Permite as seguintes ações em todas as databases como um todo:

	killCursors
	repairDatabase


## Explique todas as ações de privilégio listadas em Privilege Actions.


#### **Ações de execução e escrita**

##### find

O usuário pode utilizar o método `db.collection.find()`. Aplicável a databases ou coleções.

##### insert

O usuário pode utilizar o comando `insert`. Aplicável a databases ou collections.

##### remove

O usuário pode utilizar o método `db.collection.remove()`. Aplicável a databases ou collections.

##### update

O usuário pode utilizar o comando `update`. Aplicável a databases ou collections.


####Ações de Gerencia de Databases

##### changeCustomData

O usuário pode alterar informações customizadas de qualquer usuário na database especificada. Aplicável a databases.

##### changeOwnCustomData

Usuários podem alterar suas próprias informações customizadas. Aplicável a databases.

##### changeOwnPassword

Usuários podem alterar suas próprias senhas. Aplicável a databases.

##### changePassword

O usuário pode alterar a senha de qualquer usuário na database especificada. Aplicável a databases.

##### createCollection

O usuário pode utilizar o método `db.createCollection()`. Aplicável a databases ou collections.

##### createIndex

Provê acesso ao método `db.collection.createIndex()` e ao comando `createIndexes`. Aplicável a databases ou collections.

##### createRole

O usuário pode criar novas permissões na database especificada. Aplicável a databases.

##### createUser

O usuário pode criar novos usuários na database especificada. Aplicável a databases.

##### dropCollection

O usuário pode utilizar o método `db.collection.drop()`. Aplicável a databases ou collections.

##### dropRole

O usuário pode remover qualquer permissão da database especificada. Aplicável a databases.

##### dropUser

O usuário pode remover qualquer usuário da database especificada. Aplicável a databases.

##### emptycapped

O usuário pode utilizar o comando `emptycapped`. Aplicável a databases ou collections.

##### enableProfiler

O usuário pode utilizar o método `db.setProfilingLevel()`. Aplicável a databases.

##### grantRole

O usuário pode atribuir qualquer permissão na database para qualquer usuário de qualquer database no sistema. Aplicável a databases.

##### killCursors

O usuário pode destruir cursores na collection de destino.

##### revokeRole

O usuário pode remover qualquer permissão de qualquer usuário de qualquer database no sistema. Aplicável a databases.

##### unlock

O usuário pode utilizar o método `db.fsyncUnlock()`. Aplicável ao cluster.

##### viewRole

O usuário pode visualizar informações a respeito de qualquer permissão na database especificada. Aplicável a databases.

##### viewUser

O usuário pode visualizar as informações de qualquer usuário na database especificada. Aplicável a databases.


#### Ações de gerenciamento de deploy

##### authSchemaUpgrade

O usuário pode utilizar o comando `authSchemaUpgrade`. Aplicável ao cluster.

##### cleanupOrphaned

O usuário pode utilizar o comando `cleanupOrphaned`. Aplicável ao cluster.

##### cpuProfiler

O usuário pode habilitar o CPU profiler. Aplicável ao cluster.

##### inprog

O usuário pode utilizar o método `db.currentOp()` para retornar operações pendentes e ativas. Aplicável ao cluster.

##### invalidateUserCache

Provê acesso ao comando `invalidateUserCache`. Aplicável ao cluster.

##### killop

O usuário pode utilizar o método db.killOp(). Aplicável ao cluster.

##### planCacheRead

O usuário pode utilizar os comandos `planCacheListPlans` e `planCacheListQueryShapes` e os métodos `PlanCache.getPlansByQuery()` e `PlanCache.listQueryShapes()`. Aplicável a databases ou collections.

##### planCacheWrite

O usuário pode utilizar o comando `planCacheClear` e os métodos `PlanCache.clear()` e `PlanCache.clearPlansByQuery()`. Aplicável a databases ou collections.

##### storageDetails

O usuário pode utilizar o comando `storageDetails`. Aplicável a databases ou collections.


#### Ações de replicação

##### appendOplogNote

O usuário pode adicionar anotações ao oplog. Aplicável ao cluster.

##### replSetConfigure

O usuário pode configurar um replica set. Aplicável ao cluster.

##### replSetGetStatus

O usuário pode utilizar o comando `replSetGetStatus`. Aplicável ao cluster.

##### replSetHeartbeat

O usuário pode utilizar o comando `replSetHeartbeat`. Aplicável ao cluster.

##### replSetStateChange

O usuário pode modificar o estado de um replica set através dos comandos 'replSetFreeze', 'replSetMaintenance', 'replSetStepDown', and 'replSetSyncFrom'. Aplicável ao cluster.

##### resync

O usuário pode utilizar o comando 'resync'. Aplicável ao cluster.

#### Ações de sharding

##### addShard

O usuário pode utilizar o comando `addShard`. Aplicável ao cluster.

##### enableSharding

O usuário pode habilitar sharding em uma database usando o comando `enableSharding` e pode "shardear" uma collection usando o comando `shardCollection`. Aplicável a databases ou collections.

##### flushRouterConfig

O usuário pode utilizar o comando `flushRouterConfig`. Aplicável ao cluster.

##### getShardMap

O usuário pode utilizar o comando `getShardMap`. Aplicável ao cluster.

##### getShardVersion

O usuário pode utilizar o comando `getShardVersion`. Aplicável a databases.

##### listShards

O usuário pode utilizar o comando `listShards`. Aplicável ao cluster.

##### moveChunk

O usuário pode utilizar o comando `moveChunk`. Também pode utilizar o comando `movePrimary`, desde que o privilégio seja aplicado a uma database apropriada. Aplicável a databases ou collections.

##### removeShard

O usuário pode utilizar o comando `removeShard`. Aplicável ao cluster.

##### shardingState

O usuário pode utilizar o comando `shardingState`. Aplicável ao cluster.

##### splitChunk

O usuário pode utilizar o comando `splitChunk`. Aplicável a databases ou collections.

##### splitVector

O usuário pode utilizar o comando `splitVector`. Aplicável a databases ou collections.


#### Ações de administração de servidores

##### applicationMessage

O usuário pode utilizar o comando `logApplicationMessage`. Aplicável ao cluster.

##### closeAllDatabases

O usuário pode utilizar o comando `closeAllDatabases`. Aplicável ao cluster.

##### collMod

O usuário pode utilizar o comando `collMod`. Aplicável a databases ou collections.

##### compact

O usuário pode utilizar o comando `compact`. Aplicável a databases ou collections.

##### connPoolSync

O usuário pode utilizar o comando `connPoolSync`. Aplicável ao cluster.

##### convertToCapped

O usuário pode utilizar o comando `convertToCapped`. Aplicável a databases ou collections.

##### dropDatabase

O usuário pode utilizar o comando `dropDatabase`. Aplicável a databases.

##### dropIndex

O usuário pode utilizar o comando `dropIndexes`. Aplicável a databases ou collections.

#### fsync

O usuário pode utilizar o comando `fsync`. Aplicável ao cluster.

##### getParameter

O usuário pode utilizar o comando `getParameter`. Aplicável ao cluster.

##### hostInfo

Provê informações sobre o servidor onde a instância do MongoDB é executada.Aplicável ao cluster.

##### logRotate

O usuário pode utilizar o comando `logRotate`. Aplicável ao cluster.

##### reIndex

O usuário pode utilizar o comando `reIndex`. Aplicável a databases ou collections.

##### renameCollectionSameDB

Permite que o usuário renomeie collections na database selecionada usando o comando `renameCollection`. Aplicável a databases. Se uma collection com o novo nome já existir, o usuário deve também possuir a ação `dropCollection` na collection de destino.

##### repairDatabase

O usuário pode utilizar o comando `repairDatabase`. Aplicável a databases.

##### setParameter

O usuário pode utilizar o comando `setParameter`. Aplicável ao cluster.

##### shutdown

O usuário pode utilizar o comando `shutdown`. Aplicável ao cluster.

##### touch

O usuário pode utilizar o comando `touch`. Aplicável ao cluster.


#### Ações de diagnóstico

##### collStats

O usuário pode utilizar o comando `collStats`. Aplicável a databases ou collections.

##### connPoolStats

O usuário pode utilizar os comandos `connPoolStats` e `shardConnPoolStats`. Aplicável ao cluster.

##### cursorInfo

O usuário pode utilizar o comando `cursorInfo`. Aplicável ao cluster.

##### dbHash

O usuário pode utilizar o comando `dbHash`. Aplicável a databases ou collections.

##### dbStats

O usuário pode utilizar o comando `dbStats`. Aplicável a databases.

##### diagLogging

O usuário pode utilizar o comando `diagLogging`. Aplicável ao cluster.

##### getCmdLineOpts

O usuário pode utilizar o comando `getCmdLineOpts`. Aplicável ao cluster.

##### getLog

O usuário pode utilizar o comando `getLog`. Aplicável ao cluster.

##### indexStats

O usuário pode utilizar o comando `indexStats`. Aplicável a databases ou collections.

##### listDatabases

O usuário pode utilizar o comando `listDatabases`. Aplicável ao cluster.

##### listCollections

O usuário pode utilizar o comando `listCollections`. Aplicável a databases.

##### listIndexes

O usuário pode utilizar o comando `ListIndexes`. Aplicável a databases ou collections.

##### netstat

O usuário pode utilizar o comando `netstat`. Aplicável ao cluster.

##### serverStatus

O usuário pode utilizar o comando `serverStatus`. Aplicável ao cluster.

##### validate

O usuário pode utilizar o comando `validate`. Aplicável a databases ou collections.

##### top

O usuário pode utilizar o comando `top`. Aplicável ao cluster.


#### Ações internas

##### anyAction

Permite qualquer ação em um recurso. Não atribua esta ação, exceto para circunstâncias excepcionais.

##### internal

Permite ações internas. Não atribua esta ação, exceto para circunstâncias excepcionais.





