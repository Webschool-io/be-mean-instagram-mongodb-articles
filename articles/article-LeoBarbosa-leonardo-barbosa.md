# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Leonardo Barbosa de Oliveira
**Data**  15/12/2015 00:10

## Qual a diferença entre Autenticação e Autorização?

Autenticação - deve verificar se um usuário é ele mesmo através de validação de senhas e/ou certificados.
Autorização - com base em um usuário identificado, as regras e os controles de permissão e acesso serão aplicadas na aplicação/produto e seus recursos.


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Diferenca entre usuário comun e administrador são as roles. Um administrador tem role que são suas funções, seus papeis que voce pode tanto selecionar o papel de uma outra database ou so na propria database.


Usuário Administrador:
use dbUsers
{
 createUser:"LeonardoAdmin",
 pwd:"SenhaTeste@",
 roles:[{role:"userAdminAnyDataBase", db:"dbUsers"}]
}

Usuário Comum:
use dbUsers
{
 createUser:"LeonardoUser",
 pwd:"SenhaTesteUser@",
 role:[{db:"dbUsers"}]
}


## Explique cada papel listado em Cluster Administration Roles.

- ClusterAdmin
Fornece o maior acesso de gerenciamento de cluster. Este papel combina os privilégios concedidos pelos papéis clusterManager, clusterMonitor, e hostManager. Além disso, o papel proporciona a ação de dropDatabase.

- ClusterManager
Fornece gerenciamento e ações de monitoramento no cluster. Um usuário com essa função pode acessar a configuração e bancos de dados locais, que são usados em sharding e replicação, respectivamente.

- ClusterMonitor
Fornece acesso somente leitura para ferramentas de monitoramento, como o agente de monitoramento Gerente MongoDB Cloud.

- HostManager
Fornece a capacidade de monitorar e gerenciar servidores.



## Explique todas as ações de privilégio listadas em Privilege Actions.

Ações de privilégio definem as operações que um usuário pode executar em um recurso. 
Um privilégio MongoDB dispõe de um recurso e as ações permitidas.

MongoDB fornece funções integrados com combinação de pré-definidas de recursos e ações permitidas. 

- Ações e Consultas

find
O usuário pode executar o método db.collection.find (). Aplicar esta ação para banco de dados ou de cobrança de recursos.

insert
O usuário pode executar o comando de inserção. Aplicar esta ação para banco de dados ou de cobrança de recursos.

remove
O usuário pode executar o método db.collection.remove (). Aplicar esta ação para banco de dados ou de cobrança de recursos.

update
O usuário pode executar o comando update. Aplicar esta ação para banco de dados ou de cobrança de recursos.

- Ações Gerenciamento Banco de Dados

changeCustomData
O usuário pode alterar as informações de costume de qualquer usuário no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

changeOwnCustomData
Os usuários podem alterar suas próprias informações personalizadas. Aplicar esta ação para recursos de banco de dados. Veja também alterar sua senha e dados personalizados.

changeOwnPassword
Os usuários podem alterar suas próprias senhas. Aplicar esta ação para recursos de banco de dados. Veja também alterar sua senha e dados personalizados.

changePassword
O usuário pode alterar a senha de qualquer usuário no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

createCollection
O usuário pode executar o método db.createCollection (). Aplicar esta ação para banco de dados ou de cobrança de recursos.

CreateIndex
Fornece acesso ao método db.collection.createIndex () eo comando createIndexes. Aplicar esta ação para banco de dados ou de cobrança de recursos.

CREATEROLE
O usuário pode criar novas funções no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

createUser
O usuário pode criar novos usuários no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

dropCollection
O usuário pode executar o método db.collection.drop (). Aplicar esta ação para banco de dados ou de cobrança de recursos.

dropRole
O usuário pode excluir qualquer papel do banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

dropuser
O usuário pode remover qualquer usuário do banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

emptycapped
O usuário pode executar o comando emptycapped. Aplicar esta ação para banco de dados ou de cobrança de recursos.

enableProfiler
O usuário pode executar o método db.setProfilingLevel (). Aplicar esta ação para recursos de banco de dados.

grantRole
O usuário pode conceder qualquer papel no banco de dados para qualquer usuário de qualquer banco de dados no sistema. Aplicar esta ação para recursos de banco de dados.

killCursors
O usuário pode matar os cursores no conjunto de destino.

revokeRole
O usuário pode remover qualquer papel de qualquer usuário a partir de qualquer banco de dados no sistema. Aplicar esta ação para recursos de banco de dados.

unlock
O usuário pode executar o método db.fsyncUnlock (). Aplicar esta ação para o recurso de cluster.

viewRole
O usuário pode visualizar informações sobre qualquer função no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

viewUser
O usuário pode visualizar as informações de qualquer usuário no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

Ações de gerenciamento de implantação

authSchemaUpgrade
O usuário pode executar o comando authSchemaUpgrade. Aplicar esta ação para o recurso de cluster.

cleanupOrphaned
O usuário pode executar o comando cleanupOrphaned. Aplicar esta ação para o recurso de cluster.

cpuProfiler
O usuário pode ativar e usar o profiler CPU. Aplicar esta ação para o recurso de cluster.

inprog
O usuário pode usar o método db.currentOp () para retornar pendentes e ativos operações. Aplicar esta ação para o recurso de cluster.

invalidateUserCache
Fornece acesso ao comando invalidateUserCache. Aplicar esta ação para o recurso de cluster.

killop
O usuário pode executar o método db.killOp (). Aplicar esta ação para o recurso de cluster.

planCacheRead
O usuário pode executar as planCacheListPlans e comandos planCacheListQueryShapes e os métodos PlanCache.getPlansByQuery () e PlanCache.listQueryShapes (). Aplicar esta ação para banco de dados ou de cobrança de recursos.

planCacheWrite
O usuário pode executar o comando planCacheClear eo PlanCache.clear () e PlanCache.clearPlansByQuery () métodos. Aplicar esta ação para banco de dados ou de cobrança de recursos.

storageDetails
O usuário pode executar o comando storageDetails. Aplicar esta ação para banco de dados ou de cobrança de recursos.

- Ações de replicação

appendOplogNote
Usuário pode acrescentar notas à oplog. Aplicar esta ação para o recurso de cluster.

replSetConfigure
O usuário pode configurar um conjunto de réplicas. Aplicar esta ação para o recurso de cluster.

replSetGetStatus
O usuário pode executar o comando replSetGetStatus. Aplicar esta ação para o recurso de cluster.

replSetHeartbeat
O usuário pode executar o comando replSetHeartbeat. Aplicar esta ação para o recurso de cluster.

replSetStateChange
O usuário pode alterar o estado de um conjunto de réplicas através da replSetFreeze, comandos replSetMaintenance, replSetStepDown, e replSetSyncFrom. Aplicar esta ação para o recurso de cluster.

resync
O usuário pode executar o comando de ressincronização. Aplicar esta ação para o recurso de cluster.

- Ações Sharding

addShard
O usuário pode executar o comando addShard. Aplicar esta ação para o recurso de cluster.

enableSharding
O usuário pode habilitar sharding em um banco de dados usando o comando enableSharding e pode caco uma coleção usando o comando shardCollection. Aplicar esta ação para banco de dados ou de cobrança de recursos.

flushRouterConfig
O usuário pode executar o comando flushRouterConfig. Aplicar esta ação para o recurso de cluster.

getShardMap
O usuário pode executar o comando getShardMap. Aplicar esta ação para o recurso de cluster.

getShardVersion
O usuário pode executar o comando getShardVersion. Aplicar esta ação para recursos de banco de dados.

listShards
O usuário pode executar o comando listShards. Aplicar esta ação para o recurso de cluster.

moveChunk
O usuário pode executar o comando moveChunk. Além disso, o utilizador pode executar o comando movePrimary desde que o privilégio é aplicado a um recurso base de dados apropriada. Aplicar esta ação para banco de dados ou de cobrança de recursos.

removeShard
O usuário pode executar o comando removeShard. Aplicar esta ação para o recurso de cluster.

shardingState
O usuário pode executar o comando shardingState. Aplicar esta ação para o recurso de cluster.

splitChunk
O usuário pode executar o comando splitChunk. Aplicar esta ação para banco de dados ou de cobrança de recursos.

splitVector
O usuário pode executar o comando splitVector. Aplicar esta ação para banco de dados ou de cobrança de recursos.

Ações de Administração de Servidor

applicationMessage
O usuário pode executar o comando logApplicationMessage. Aplicar esta ação para o recurso de cluster.

closeAllDatabases
O usuário pode executar o comando closeAllDatabases. Aplicar esta ação para o recurso de cluster.

collMod
O usuário pode executar o comando collMod. Aplicar esta ação para banco de dados ou de cobrança de recursos.

compactar
O usuário pode executar o comando compacto. Aplicar esta ação para banco de dados ou de cobrança de recursos.

connPoolSync
O usuário pode executar o comando connPoolSync. Aplicar esta ação para o recurso de cluster.

convertToCapped
O usuário pode executar o comando convertToCapped. Aplicar esta ação para banco de dados ou de cobrança de recursos.

dropDatabase
O usuário pode executar o comando dropDatabase. Aplicar esta ação para recursos de banco de dados.

dropIndex
O usuário pode executar o comando dropIndexes. Aplicar esta ação para banco de dados ou de cobrança de recursos.

fsync
O usuário pode executar o comando fsync. Aplicar esta ação para o recurso de cluster.

getParameter
O usuário pode executar o comando getParameter. Aplicar esta ação para o recurso de cluster.

hostInfo
Fornece informações sobre o servidor a instância MongoDB é executado. Aplicar esta ação para o recurso de cluster.

logrotate
O usuário pode executar o comando logrotate. Aplicar esta ação para o recurso de cluster.

reindexar
O usuário pode executar o comando REINDEX. Aplicar esta ação para banco de dados ou de cobrança de recursos.

renameCollectionSameDB
Permite que o usuário renomear coleções no banco de dados atual usando o comando renameCollection. Aplicar esta ação para recursos de banco de dados.

Além disso, o usuário deve ter encontrar na coleção de origem ou não têm encontrar na coleção de destino.

Se uma coleção com o novo nome já existir, o usuário também deve ter a ação dropCollection na coleção de destino.

RepairDatabase
O usuário pode executar o comando RepairDatabase. Aplicar esta ação para recursos de banco de dados.

setParameter
O usuário pode executar o comando setParameter. Aplicar esta ação para o recurso de cluster.

shutdown
O usuário pode executar o comando de desligamento. Aplicar esta ação para o recurso de cluster.

touch
O usuário pode executar o comando touch. Aplicar esta ação para o recurso de cluster.

Ações de diagnóstico

collStats
O usuário pode executar o comando collStats. Aplicar esta ação para banco de dados ou de cobrança de recursos.

connPoolStats
O usuário pode executar as connPoolStats e comandos shardConnPoolStats. Aplicar esta ação para o recurso de cluster.

cursorInfo
O usuário pode executar o comando cursorInfo. Aplicar esta ação para o recurso de cluster.

dbHash
O usuário pode executar o comando dbHash. Aplicar esta ação para banco de dados ou de cobrança de recursos.

dbStats
O usuário pode executar o comando dbStats. Aplicar esta ação para recursos de banco de dados.

diagLogging
O usuário pode executar o comando diagLogging. Aplicar esta ação para o recurso de cluster.

getCmdLineOpts
O usuário pode executar o comando getCmdLineOpts. Aplicar esta ação para o recurso de cluster.

getLog
O usuário pode executar o comando getLog. Aplicar esta ação para o recurso de cluster.

indexStats
O usuário pode executar o comando indexStats. Aplicar esta ação para banco de dados ou de cobrança de recursos.

listDatabases
O usuário pode executar o comando listDatabases. Aplicar esta ação para o recurso de cluster.

netstat
O usuário pode executar o comando netstat. Aplicar esta ação para o recurso de cluster.

serverStatus
O usuário pode executar o comando serverStatus. Aplicar esta ação para o recurso de cluster.

validar
O usuário pode executar o comando validate. Aplicar esta ação para banco de dados ou de cobrança de recursos.

topo
O usuário pode executar o comando top. Aplicar esta ação para o recurso de cluster.

- Ações Internas

anyAction
Permite que qualquer ação em um recurso. Não atribua essa ação, exceto em circunstâncias excepcionais.

internal
Permite ações internas. Não atribua essa ação, exceto em circunstâncias excepcionais.



