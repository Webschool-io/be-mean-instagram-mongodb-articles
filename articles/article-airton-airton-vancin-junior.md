# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Airton Vancin Junior
**Data** Date.now() //em timestamp

## Qual a diferença entre Autenticação e Autorização?

Embora a autenticação e autorização estão intimamente ligados, autenticação é diferente de autorização. A autenticação verifica a identidade de um utilizador; autorização determina o acesso do usuário verificado a recursos e operações.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

** Criando um usuário administrador **
```
linux(mongod-3.0.7) admin> db.createUser(
...   {
...     user: "TesteAdmin",
...     pwd: "admin123",
...     roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
...   }
... )
Successfully added user: {
  "user": "TesteAdmin",
  "roles": [
    {
      "role": "userAdminAnyDatabase",
      "db": "admin"
    }
  ]
}

```

** Criando um usuário comum **
```
linux(mongod-3.0.7) admin> db.createUser(
...   {
...     user: "TesteComum",
...     pwd: "comum123",
...     roles: []
...   }
... )
Successfully added user: {
  "user": "TesteComum",
  "roles": [ ]
}

```


## Explique cada papel listado em Cluster Administration Roles.

### clusterAdmin 
Fornece o maior acesso de gerenciamento de cluster. Este papel combina os privilégios concedidos pelos **clusterManager**, **clusterMonitor**, e **hostManager** papéis. Além disso, o papel fornece a dropDatabase acção.

### clusterManager 
Fornece gerenciamento e ações de monitoramento no cluster. Um usuário com essa função pode acessar a configuração e locais bancos de dados, que são usados ​​em sharding e replicação, respectivamente.

Fornece as seguintes ações no cluster como um todo:

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

Fornece as seguintes ações em todos os bancos de dados no cluster:

- enableSharding
- moveChunk
- splitChunk
- splitVector

Na configuração de banco de dados, fornece as seguintes ações no configurações de coleção:

- insert
- remove
- update

Na configuração de banco de dados, fornece as seguintes ações em todas as coleções de configuração e sobre as **system.indexes**, **system.js** e **system.namespaces** coleções:

- collStats
- dbHash
- dbStats
- find
- killCursors

No local de banco de dados, fornece as seguintes ações no **replset** coleção:

- collStats
- dbHash
- dbStats
- find
- killCursors

### clusterMonitor  
Fornece acesso somente leitura para ferramentas de monitoramento, como o Gerenciador de MongoDB Nuvem agente de monitoramento.

Fornece as seguintes ações no cluster como um todo:

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

Fornece as seguintes ações em todos os bancos de dados no cluster:

- collStats
- dbStats
- getShardVersion

Oferece a descoberta de ação em todos os system.profile coleções no cluster.

Fornece as seguintes ações na configuração coleções de banco de dados de configuração e **system.indexes**, **system.js** e **system.namespaces** coleções:

- collStats
- dbHash
- dbStats
- find
- killCursors

### hostManager

Fornece a capacidade de monitorar e gerenciar servidores.

Fornece as seguintes ações no cluster como um todo:

- applicationMessage
- closeAllDatabases
- connPoolSync
- cpuProfiler
- diagLogging
- flushRouterConfig
- fsync
- invalidateUserCache
- killop
- logrotate
- resync
- setParameter
- shutdown
- touch
- unlock

Fornece as seguintes ações em todos os bancos de dados no cluster:

- killCursors
- RepairDatabase

## Explique todas as ações de privilégio listadas em Privilege Actions.

### Consultar e gravar ações 

**find**
O usuário pode executar o método db.collection.find (). Aplicar esta ação para banco de dados ou de cobrança de recursos.

**insert**
O usuário pode executar o comando de inserção. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**remove**
O usuário pode executar o método db.collection.remove(). Aplicar esta ação para banco de dados ou de cobrança de recursos.

**update**
O usuário pode executar o comando update. Aplicar esta ação para banco de dados ou de cobrança de recursos.

### Ações de Adminstração de Database

**changeCustomData**
O usuário pode alterar as informações de costume de qualquer usuário no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**changeOwnCustomData**
Os usuários podem alterar suas próprias informações personalizadas. Aplicar esta ação para recursos de banco de dados. Veja também alterar sua senha e dados personalizados.

**changeOwnPassword**
Os usuários podem alterar suas próprias senhas. Aplicar esta ação para recursos de banco de dados. Veja também alterar sua senha e dados personalizados.

**changePassword**
O usuário pode alterar a senha de qualquer usuário no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**createCollection**
O usuário pode executar o método db.createCollection(). Aplicar esta ação para banco de dados ou de cobrança de recursos.

**createIndex**
Fornece acesso ao método db.collection.createIndex () eo comando createIndexes. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**createRole**
O usuário pode criar novas funções no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**createUser**
O usuário pode criar novos usuários no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**dropCollection**
O usuário pode executar o método db.collection.drop (). Aplicar esta ação para banco de dados ou de cobrança de recursos.

**dropRole**
O usuário pode excluir qualquer papel do banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**dropUser**
O usuário pode remover qualquer usuário do banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**emptycapped**
O usuário pode executar o comando emptycapped. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**enableProfiler**
O usuário pode executar o método db.setProfilingLevel (). Aplicar esta ação para recursos de banco de dados.

**grantRole**
O usuário pode conceder qualquer papel no banco de dados para qualquer usuário de qualquer banco de dados no sistema. Aplicar esta ação para recursos de banco de dados.

**killCursors**
O usuário pode matar os cursores no conjunto de destino.

**revokeRole**
O usuário pode remover qualquer papel de qualquer usuário a partir de qualquer banco de dados no sistema. Aplicar esta ação para recursos de banco de dados.

**unlock**
O usuário pode executar o método db.fsyncUnlock (). Aplicar esta ação para o recurso de cluster.

**viewRole**
O usuário pode visualizar informações sobre qualquer função no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**viewUser**
O usuário pode visualizar as informações de qualquer usuário no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

### Ações de gerenciamento de implantação

**authSchemaUpgrade**
O usuário pode executar o comando authSchemaUpgrade. Aplicar esta ação para o recurso de cluster.

**cleanupOrphaned**
O usuário pode executar o comando cleanupOrphaned. Aplicar esta ação para o recurso de cluster.

**cpuProfiler**
O usuário pode ativar e usar o profiler CPU. Aplicar esta ação para o recurso de cluster.

**inprog**
O usuário pode usar o método db.currentOp() para retornar pendentes e ativos operações. Aplicar esta ação para o recurso de cluster.

**invalidateUserCache**
Fornece acesso ao comando invalidateUserCache. Aplicar esta ação para o recurso de cluster.

**killop**
O usuário pode executar o método db.killOp (). Aplicar esta ação para o recurso de cluster.

**planCacheRead**
O usuário pode executar as planCacheListPlans e comandos planCacheListQueryShapes e os métodos PlanCache.getPlansByQuery() e PlanCache.listQueryShapes(). Aplicar esta ação para banco de dados ou de cobrança de recursos.

**planCacheWrite**
O usuário pode executar o comando planCacheClear eo PlanCache.clear() e PlanCache.clearPlansByQuery() métodos. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**storageDetails**
O usuário pode executar o comando storageDetails. Aplicar esta ação para banco de dados ou de cobrança de recursos.

### Ações de replicação

**appendOplogNote**
Usuário pode acrescentar notas à oplog. Aplicar esta ação para o recurso de cluster.

**replSetConfigure**
O usuário pode configurar um conjunto de réplicas. Aplicar esta ação para o recurso de cluster.

**replSetGetStatus**
O usuário pode executar o comando replSetGetStatus. Aplicar esta ação para o recurso de cluster.

**replSetHeartbeat**
O usuário pode executar o comando replSetHeartbeat. Aplicar esta ação para o recurso de cluster.

**replSetStateChange**
O usuário pode alterar o estado de um conjunto de réplicas através da replSetFreeze, comandos replSetMaintenance, replSetStepDown, e replSetSyncFrom. Aplicar esta ação para o recurso de cluster.

**resync**
O usuário pode executar o comando de ressincronização. Aplicar esta ação para o recurso de cluster.

### Ações Sharding

**addShard**
O usuário pode executar o comando addShard. Aplicar esta ação para o recurso de cluster.

**enableSharding**
O usuário pode habilitar sharding em um banco de dados usando o comando enableSharding e pode caco uma coleção usando o comando shardCollection. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**flushRouterConfig**
O usuário pode executar o comando flushRouterConfig. Aplicar esta ação para o recurso de cluster.

**getShardMap**
O usuário pode executar o comando getShardMap. Aplicar esta ação para o recurso de cluster.

**getShardVersion**
O usuário pode executar o comando getShardVersion. Aplicar esta ação para recursos de banco de dados.

**listShards**
O usuário pode executar o comando listShards. Aplicar esta ação para o recurso de cluster.

**moveChunk**
O usuário pode executar o comando moveChunk. Além disso, o utilizador pode executar o comando movePrimary desde que o privilégio é aplicado a um recurso base de dados apropriada. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**removeShard**
O usuário pode executar o comando removeShard. Aplicar esta ação para o recurso de cluster.

**shardingState**
O usuário pode executar o comando shardingState. Aplicar esta ação para o recurso de cluster.

**splitChunk**
O usuário pode executar o comando splitChunk. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**splitVector**
O usuário pode executar o comando splitVector. Aplicar esta ação para banco de dados ou de cobrança de recursos.


### Ações de administração de servidor

**applicationMessage**
O usuário pode executar o comando logApplicationMessage. Aplicar esta ação para o recurso de cluster.

**closeAllDatabases**
O usuário pode executar o comando closeAllDatabases. Aplicar esta ação para o recurso de cluster.

**collMod**
O usuário pode executar o comando collMod. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**compactar**
O usuário pode executar o comando compacto. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**connPoolSync**
O usuário pode executar o comando connPoolSync. Aplicar esta ação para o recurso de cluster.

**convertToCapped**
O usuário pode executar o comando convertToCapped. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**dropDatabase**
O usuário pode executar o comando dropDatabase. Aplicar esta ação para recursos de banco de dados.

**dropIndex**
O usuário pode executar o comando dropIndexes. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**fsync**
O usuário pode executar o comando fsync. Aplicar esta ação para o recurso de cluster.

**getParameter**
O usuário pode executar o comando getParameter. Aplicar esta ação para o recurso de cluster.

**hostInfo**
Fornece informações sobre o servidor a instância MongoDB é executado. Aplicar esta ação para o recurso de cluster.

**logrotate**
O usuário pode executar o comando logrotate. Aplicar esta ação para o recurso de cluster.

**reindexar**
O usuário pode executar o comando REINDEX. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**renameCollectionSameDB**
Permite que o usuário renomear coleções no banco de dados atual usando o comando renameCollection. Aplicar esta ação para recursos de banco de dados.

Além disso, o usuário deve ter encontrar na coleção de origem ou não têm encontrar na coleção de destino.

Se uma coleção com o novo nome já existir, o usuário também deve ter a ação dropCollection na coleção de destino.

**RepairDatabase**
O usuário pode executar o comando RepairDatabase. Aplicar esta ação para recursos de banco de dados.

**setParameter**
O usuário pode executar o comando setParameter. Aplicar esta ação para o recurso de cluster.

**shutdown**
O usuário pode executar o comando de desligamento. Aplicar esta ação para o recurso de cluster.

**touch**
O usuário pode executar o comando touch. Aplicar esta ação para o recurso de cluster.

### Ações de diagnóstico

**collStats**
O usuário pode executar o comando collStats. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**connPoolStats**
O usuário pode executar as connPoolStats e comandos shardConnPoolStats. Aplicar esta ação para o recurso de cluster.

**cursorInfo**
O usuário pode executar o comando cursorInfo. Aplicar esta ação para o recurso de cluster.

**dbHash**
O usuário pode executar o comando dbHash. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**dbStats**
O usuário pode executar o comando dbStats. Aplicar esta ação para recursos de banco de dados.

**diagLogging**
O usuário pode executar o comando diagLogging. Aplicar esta ação para o recurso de cluster.

**getCmdLineOpts**
O usuário pode executar o comando getCmdLineOpts. Aplicar esta ação para o recurso de cluster.

**getLog**
O usuário pode executar o comando getLog. Aplicar esta ação para o recurso de cluster.

**indexStats**
O usuário pode executar o comando indexStats. Aplicar esta ação para banco de dados ou de cobrança de recursos.

Alterado na versão 3.0: MongoDB 3.0 remove o comando indexStats.

**listDatabases**
O usuário pode executar o comando listDatabases. Aplicar esta ação para o recurso de cluster.

**listCollections**
O usuário pode executar o comando listCollections. Aplicar esta ação para recursos de banco de dados.

**ListIndexes**
O usuário pode executar o comando ListIndexes. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**netstat**
O usuário pode executar o comando netstat. Aplicar esta ação para o recurso de cluster.

**serverStatus**
O usuário pode executar o comando serverStatus. Aplicar esta ação para o recurso de cluster.

**validate**
O usuário pode executar o comando validate. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**top**
O usuário pode executar o comando top. Aplicar esta ação para o recurso de cluster.

### Ações Internas

**anyAction**
Permite que qualquer ação em um recurso. Não atribua essa ação, exceto em circunstâncias excepcionais.

**internal**
Permite ações internas. Não atribua essa ação, exceto em circunstâncias excepcionais.