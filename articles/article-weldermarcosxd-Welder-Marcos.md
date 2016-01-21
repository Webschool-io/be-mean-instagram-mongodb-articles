# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Welder Marcos
**Data** 1453337960646

## Qual a diferença entre Autenticação e Autorização?

Ambas as ações são realizadas pelo método db.auth() apenas divergindo nos parêmetros aceitos por cada um

### Autenticação ###

Processo onde o usuário passa por um processo de avaliação de suas credencias para ser avaliada a sua existncia
como "convidado" da base de dados onde quer acessar.
```
db.auth(<username>, <password>) ou

db.auth( {
   user: <username>,
   pwd: <password>,
   mechanism: <authentication mechanism>,
   digestPassword: <boolean>
} )
```

### Autorização ###

Apoś a identificação é verificada se aquele usuário autenticado possui autorização para executar a operação solicitada
Mesmo que o usuário exista, nada garante que ele venha a ser autorizado em determinada ação.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Usuários administradores são para desenvolvedores e gestores de databases. Usuários comuns são clientes com permissões individuais de acesso.

Conecte-se ao servidor do mongo db com um usuário previamente criado com a role createUser e grantRole

### Usuário Administrador ###
```
use admin
db.createUser(
   {
     user: "King",
     customData {"Usuário com denotação da peça de xadrez"},
     pwd: "Ch3ck-M@te!",
     roles: [{role: "userAdminAnyDatabase", db: "admin"} ]
   }
)
```

### Usuário Comum ###

use admin
db.createUser(
   {
     user: "Pawn",
     customData {"Usuário com denotação da peça de xadrez"},
     pwd: "Check",
     roles: [ ]
   }
)


## Explique cada papel listado em Cluster Administration Roles.

### clusterAdmin ###

	Role que garante poderes supremos ao administrador do cluster. Une os poderes das outras 3 roles e adiciona 
	a ação de dropDatabase.

### clusterManager ###

	Role que garante poderes de administração e monitoramento ao cluster. Usuários com esta role podem gerenciar
	shards e réplicas da base de dados por administrar as bases local e de configuração. O mesmo tem as permissão
	as seguintes ações:
	
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

	Alêm de poder realizar as seguintes ações em qualquer database:
	
    enableSharding
    moveChunk
    splitChunk
    splitVector

	E tomar as seguintes ações na coleção settings dentro da base de configuração:

    insert
    remove
    update

	Ainda na base de configuração, pode realizar as seguintes ações sobre as collections: system.indexes, 
	system.js e system.namespaces

    collStats
    dbHash
    dbStats
    find
    killCursors

	Na base local pode realizar as seguintes ações na coleção de replset:
	
    collStats
    dbHash
    dbStats
    find
    killCursors

### clusterMonitor ###

	Possibilita acesso de leitura e monitoramento com ferramentas como os agentes de monitoramento  
	MongoDB Cloud Manager e Ops Manager.

	Provê as seguintes ações no cluster como um todo:
	
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

	Além disso pode realizar as seguintes ações em qualquer database:
	
    collStats
    dbStats
    getShardVersion
	
	Pode realizar o comando find em qualquer coleção system.profile no cluster. 

	Ainda na base de configuração, pode realizar as seguintes ações sobre as collections: system.indexes, 
	system.js e system.namespaces

    collStats
    dbHash
    dbStats
    find
    killCursors

### hostManager ###

	Garante a habilidade de monitorar e gerenciar servidores.

	Provê as seguintes ações no cluster como um todo:
	
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
	
	Além das seguintes ações em qualquer database:
	
    killCursors
    repairDatabase

## Explique todas as ações de privilégio listadas em Privilege Actions.

### Query and Write Actions ###

find

    Permite que o usuário realize o método find() nas coleções e bases de dados especificadas.

insert

    Permite que o usuário realize o metodo insert() nas coleções e bases de dados especificadas.

remove

    Permite que o usuário realize o metodo remove() nas coleções e bases de dados especificadas.

update

    Permite que o usuário realize o metodo update() nas coleções e bases de dados especificadas.


### Database Management Actions ###

changeCustomData

    Usuário pode alterar a informação custumizada de qualquer usuário na base de dados especificada.

changeOwnCustomData

    Usuário pode alterar a suas informações personalizadas.

changeOwnPassword

    Usuário pode alterar a sua própria senha.

changePassword

    Usuário pode alterar a senha de qualquer outro usuário na database especificada.

createCollection

    Usuário pode criar coleções.

createIndex

    Usuário pode criar índices.

createRole

    Usuário pode criar roles.

createUser

    Usuário pode criar outros usuários do banco de dados especificado.

dropCollection

    Usuário pode excluir coleções.

dropRole

    Usuário pode excluir roles.

dropUser

    Usuário pode excluir outros usuários.

emptycapped

    Usuário pode realizar o comando emptycapped().

enableProfiler

    Usuário pode realizar o comando db.setProfilingLevel().

grantRole

    Usuário pode dar roles a outros usuários.

killCursors

    Usuário pode deletar os cursores na coleção especificada.

revokeRole

    Usuário pode remover roles de outros usuários.

unlock

    Usuário pode realizar o comando db.fsyncUnlock().

viewRole

    Usuário pode ver as informações de determinada role na database especificada.

viewUser

    Usuário pode visualizar as informações de determinado usuário presente nas bases de dados especificadas.


### Deployment Management Actions ###


authSchemaUpgrade

    Usuário pode realizar o comando authSchemaUpgrade.

cleanupOrphaned

    Usuário pode realizar o comando cleanupOrphaned.

cpuProfiler

    Usuário pode realizar o CPU profiler.

inprog

    Usuário pode realizar o método db.currentOp() para visualizar operações ativas e pendentes.

invalidateUserCache

    Usuário pode realizar o comando invalidateUserCache.

killop

    Usuário pode realizar o método db.killOp().

planCacheRead

    Usuário pode realizar os comandos planCacheListPlans e planCacheListQueryShapes e os métodos PlanCache.getPlansByQuery() e PlanCache.listQueryShapes().

planCacheWrite

    Usuário pode realizar o comando planCacheClear e os métodos PlanCache.clear() e PlanCache.clearPlansByQuery().

storageDetails

    Usuário pode realizar o comando storageDetails.


### Replication Actions ###


appendOplogNote

    Usuário pode adicionar anotações no  oplog.

replSetConfigure

   usuário pode configurar uma replica set.

replSetGetStatus

    Usuário pode realizar o comando replSetGetStatus.

replSetHeartbeat

    Usuário pode realizar o comando replSetHeartbeat.

replSetStateChange

    usuário pode alterar o estado de uma réplica set através dos comandos replSetFreeze, replSetMaintenance, replSetStepDown e replSetSyncFrom.

resync

    Usuário pode realizar o comando resync.

### Sharding Actions ###


addShard

    Usuário pode realizar o comando addShard.

enableSharding

    Usuário pode habilitar o shardeamento de uma database através do comando enableSharding  
	e pode shardear uma coleção através do shardCollection.

flushRouterConfig

    Usuário pode realizar o comando flushRouterConfig.

getShardMap

    Usuário pode realizar o comando getShardMap.

getShardVersion

    Usuário pode realizar o comando getShardVersion.

listShards

    Usuário pode realizar o comando listShards.

moveChunk

   Usuário pode realizar o comando moveChunk. Além disso o usuário pode realizar o comando movePrimary command se o privilégio foi dado a uma database apropriada.

removeShard

   Usuário pode realizar o comando removeShard.

shardingState

   Usuário pode realizar o comando shardingState.

splitChunk

    UUsuário pode realizar o comando splitChunk.

splitVector

    Usuário pode realizar o comando splitVector.


### Server Administration Actions ###

applicationMessage

	Usuário pode realizar o comando applicationMessage.

closeAllDatabases

    Usuário pode realizar o comando closeAllDatabases.

collMod

    Usuário pode realizar o comando collMod.

compact

    Usuário pode realizar o comando compact.

connPoolSync

    Usuário pode realizar o comando connPoolSync.

convertToCapped

    Usuário pode realizar o comando convertToCapped.

dropDatabase

    Usuário pode realizar o comando dropDatabase.

dropIndex

    Usuário pode realizar o comando dropIndexes.

fsync

    Usuário pode realizar o comando fsync.

getParameter

    Usuário pode realizar o comando getParameter.

hostInfo

    Da informações sobre o servidor em que a instancia do mongoDB esta rodando.

logRotate

    Usuário pode realizar o comando logRotate.

reIndex

    Usuário pode realizar o comando reIndex.

renameCollectionSameDB

    Permite ao usuário renomear as coleções da database atual com o comando renameCollection.

    Além disso o usuário precisa ter permissão find() na colleção fonte e não ter permissão find() na coleção alvo.

    Se uma coleção com o novo nome já existir o usuário precisa da ação dropCollection na coleção de destino.

repairDatabase

    Usuário pode realizar o comando repairDatabase.

setParameter

    Usuário pode realizar o comando setParameter.

shutdown

    Usuário pode realizar o comando shutdown.

touch

    Usuário pode realizar o comando touch.

### Diagnostic Actions ###

collStats

    Usuário pode realizar o comando collStats.

connPoolStats

    Usuário pode realizar os comandos connPoolStats e shardConnPoolStats.

cursorInfo

    Usuário pode realizar o comando cursorInfo.

dbHash

    Usuário pode realizar o comando dbHash.

dbStats

    Usuário pode realizar o comando dbStats.

diagLogging

    Usuário pode realizar o comando diagLogging.

getCmdLineOpts

    Usuário pode realizar o comando getCmdLineOpts.

getLog

    Usuário pode realizar o comando getLog.

indexStats

    Usuário pode realizar o comando indexStats.

    Mudou na versão 3.0: MongoDB 3.0 remove o comando indexStats.

listDatabases

    Usuário pode realizar o comando listDatabases.

listCollections

    Usuário pode realizar o comando listCollections.

listIndexes

    Usuário pode realizar o comando ListIndexes.

netstat

    Usuário pode realizar o comando netstat.
serverStatus

    Usuário pode realizar o comando serverStatus.

validate

   Usuário pode realizar o comando validate.

top

    Usuário pode realizar o comando top command.

### Internal Actions ###


anyAction

    Permite qualquer ação em determinada base.Não garanta esta ação exceto em circunstancias especias.

internal

    Permite ações internas. Não garanta esta ação exceto em circunstancias especias.
