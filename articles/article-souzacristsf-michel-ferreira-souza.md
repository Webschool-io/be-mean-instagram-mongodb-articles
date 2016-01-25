# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** [Michel Ferreira Souza](https://github.com/souzacristsf)
**Data** 11/01/2016 18:07

## Qual a diferença entre Autenticação e Autorização?

#### Autenticação
A autenticação nada mais é que as credenciais de acesso ao sistema, que nesse caso as credencias de acesso ao mongoDB, usando nome do usuario e senha ou outros metodos de validação.

#### Autorização
Com a autenticação bem sucedida, uma vez identificada a credencial o mesmo tem a permissão de alguns recurso diposnivel, "Permissão de manipular rercusos especificos", como baseada nas funcões roles.


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
Para criar um usuário Administrador basta digitar e rodar o comando:

```
db.createUser(
  {
    user: "SouzaAdmin",
    pwd: "admin123",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
```

Para criar um usuário comum basta rodar o comando abaixo:

```
db.createUser(
    {
      user: "SouzaUser",
      pwd: "user123",
      roles: []
    }
)
```

## Explique cada papel listado em Cluster Administration Roles.

#### `clusterAdmin`

Fornece o maior acesso de gerenciamento de cluster. Este papel combina os privilégios concedidos pelos clusterManager, clusterMonitor, e hostManager papéis

#### `clusterManager`

Fornece gerenciamento e ações de monitoramento no cluster. Um usuário com essa função pode acessar a configuração e locais bancos de dados, que são usados ​​em sharding e replicação

#### `clusterMonitor`

Fornece acesso somente leitura para ferramentas de monitoramento, como o MongoDB Cloud Manager agente de monitoramento.

#### `hostManager`

Fornece a capacidade de monitorar e gerenciar servidores.

## Explique todas as ações de privilégio listadas em Privilege Actions.

#### `Privilege Actions`

Definem as operações que um usuário pode executar em um recurso.


#### `Query and Write Actions`
**find**
O usuário pode realizar a db.collection.find () método. Aplicar esta ação para banco de dados ou em recursos de coleções.

**insert**
O usuário pode realizar o comando insert. Aplicar esta ação para banco de dados ou em recursos de coleções.

**remove**
O usuário pode realizar a db.collection.remove () método. Aplicar esta ação para banco de dados ou em recursos de coleções.

**update**
O usuário pode realizar o comando update. Aplicar esta ação para banco de dados ou em recursos de coleções.

#### `Database Management Actions`
**changeCustomData**
O usuário pode alterar as informações de costume de qualquer usuário no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**changeOwnCustomData**
Os usuários podem alterar suas próprias informações personalizadas. Aplicar esta ação para recursos de banco de dados.

**changeOwnPassword**
Os usuários podem alterar suas próprias senhas. Aplicar esta ação para recursos de banco de dados.

**changePassword**
O usuário pode alterar a senha de qualquer usuário no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**createCollection**
O usuário pode realizar a db.createCollection () método. Aplicar esta ação para banco de dados ou em recursos de coleções.

**createIndex**
Fornece acesso ao db.collection.createIndex () método e o comando createIndexes. Aplicar esta ação para banco de dados ou em recursos de coleções.

**createRole**
O usuário pode criar novas funções no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**createUser**
O usuário pode criar novos usuários no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**dropCollection**
O usuário pode realizar a db.collection.drop () método. Aplicar esta ação para banco de dados ou em recursos de coleções.

**dropRole**
O usuário pode excluir qualquer papel do banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**dropUser**
O usuário pode remover qualquer usuário do banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**emptycapped**
O usuário pode realizar a emptycapped comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**enableProfiler**
O usuário pode realizar a db.setProfilingLevel () método. Aplicar esta ação para recursos de banco de dados.

**grantRole**
O usuário pode conceder qualquer papel no banco de dados para qualquer usuário de qualquer banco de dados no sistema. Aplicar esta ação para recursos de banco de dados.

**killCursors**
O usuário pode matar os cursores no conjunto de destino.

**revokeRole**
O usuário pode remover qualquer papel de qualquer usuário a partir de qualquer banco de dados no sistema. Aplicar esta ação para recursos de banco de dados.

**unlock**
O usuário pode realizar a db.fsyncUnlock () método. Aplicar esta ação para o conjunto de recursos.

**viewRole**
O usuário pode visualizar informações sobre qualquer função no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

**viewUser**
O usuário pode visualizar as informações de qualquer usuário no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

#### `Deployment Management Actions`
**authSchemaUpgrade**
O usuário pode realizar a authSchemaUpgrade comando. Aplicar esta ação para o conjunto de recursos.

**cleanupOrphaned**
O usuário pode realizar a cleanupOrphaned comando. Aplicar esta ação para o conjunto de recursos.

**cpuProfiler**
O usuário pode ativar e usar o profiler CPU. Aplicar esta ação para o conjunto de recursos.

**inprog**
O usuário pode usar o db.currentOp () método para retornar pendentes e ativos operações. Aplicar esta ação para o conjunto de recursos.

**invalidateUserCache**
Fornece acesso ao invalidateUserCache comando. Aplicar esta ação para o conjunto de recursos.

**killop**
O usuário pode realizar a db.killOp () método. Aplicar esta ação para o conjunto de recursos.

**planCacheRead**
O usuário pode executar as planCacheListPlans e planCacheListQueryShapes comandos e do PlanCache.getPlansByQuery () e PlanCache.listQueryShapes () métodos. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**planCacheWrite**
O usuário pode realizar a planCacheClear comando eo PlanCache.clear () e PlanCache.clearPlansByQuery () métodos. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**storageDetails**
O usuário pode realizar a storageDetails comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

#### `Replication Actions`
**appendOplogNote**
Usuário pode acrescentar notas à oplog. Aplicar esta ação para o conjunto de recursos.

**replSetConfigure**
O usuário pode configurar um conjunto de réplicas. Aplicar esta ação para o conjunto de recursos.

**replSetGetStatus**
O usuário pode realizar a replSetGetStatus comando. Aplicar esta ação para o conjunto de recursos.

**replSetHeartbeat**
O usuário pode realizar a replSetHeartbeat comando. Aplicar esta ação para o conjunto de recursos.

**replSetStateChange**
O usuário pode alterar o estado de um conjunto de réplicas através da replSetFreeze, replSetMaintenance, replSetStepDown, e replSetSyncFrom comandos. Aplicar esta ação para o conjunto de recursos.

**resync**
O usuário pode realizar a ressincronização de comando. Aplicar esta ação para o conjunto de recursos.

#### `Sharding Actions`
**addShard**
O usuário pode realizar a addShard comando. Aplicar esta ação para o conjunto de recursos.

**enableSharding**
O usuário pode habilitar sharding em um banco de dados usando o enableSharding comando e pode caco uma coleção usando o shardCollection comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**flushRouterConfig**
O usuário pode realizar a flushRouterConfig comando. Aplicar esta ação para o conjunto de recursos.

**getShardMap**
O usuário pode realizar a getShardMap comando. Aplicar esta ação para o conjunto de recursos.

**getShardVersion** 
O usuário pode realizar a getShardVersion comando. Aplicar esta ação para recursos de banco de dados.

**listShards**
O usuário pode realizar a listShards comando. Aplicar esta ação para o conjunto de recursos.

**moveChunk** 
O usuário pode realizar a moveChunk comando. Além disso, o utilizador pode executar movePrimary comando desde que o privilégio é aplicado a um recurso base de dados apropriada. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**removeShard**
O usuário pode realizar a removeShard comando. Aplicar esta ação para o conjunto de recursos.

**shardingState**
O usuário pode realizar a shardingState comando. Aplicar esta ação para o conjunto de recursos.

**splitChunk**
O usuário pode realizar a splitChunk comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**splitVector**
O usuário pode realizar a splitVector comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

#### `Server Administration Actions`
**applicationMessage**
O usuário pode realizar a logApplicationMessage comando. Aplicar esta ação para o conjunto de recursos.

**closeAllDatabases**
O usuário pode realizar a closeAllDatabases comando. Aplicar esta ação para o conjunto de recursos.

**collMod**
O usuário pode realizar a collMod comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**compact**
O usuário pode executar o compacto de comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**connPoolSync**
O usuário pode realizar a connPoolSync comando. Aplicar esta ação para o conjunto de recursos.

**convertToCapped**
O usuário pode realizar a convertToCapped comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**dropDatabase**
O usuário pode realizar a dropDatabase comando. Aplicar esta ação para recursos de banco de dados.

**dopIndex**
O usuário pode realizar a dropIndexes comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**fsync**
O usuário pode realizar a fsync comando. Aplicar esta ação para o conjunto de recursos.

**getParameter**
O usuário pode realizar a getParameter comando. Aplicar esta ação para o conjunto de recursos.

**hostInfo**
Fornece informações sobre o servidor a instância MongoDB é executado. Aplicar esta ação para o conjunto de recursos.

**logrotate**
O usuário pode executar o logrotate comando. Aplicar esta ação para o conjunto de recursos.

**reindexar**
O usuário pode realizar a reindexar comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**renameCollectionSameDB**
Permite que o usuário renomear coleções no banco de dados atual usando o renameCollection comando. Aplicar esta ação para recursos de banco de dados.

Além disso, o usuário deve ter encontrar na coleção de origem ou não tem encontrar na coleção de destino.

Se uma coleção com o novo nome já existir, o usuário também deve ter o dropCollection ação na coleção de destino.

**RepairDatabase**
O usuário pode realizar a RepairDatabase comando. Aplicar esta ação para recursos de banco de dados.

**setParameter**
O usuário pode realizar a setParameter comando. Aplicar esta ação para o conjunto de recursos.

**shutdown**
O usuário pode executar o desligamento comando. Aplicar esta ação para o conjunto de recursos.

**touch** 
O usuário pode executar o toque de comando. Aplicar esta ação para o conjunto de recursos.

#### `Diagnostic Actions`
**collStats**
O usuário pode realizar a collStats comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**connPoolStats** 
O usuário pode executar as connPoolStats e shardConnPoolStats comandos. Aplicar esta ação para o conjunto de recursos.

**cursorInfo**
O usuário pode realizar a cursorInfo comando. Aplicar esta ação para o conjunto de recursos.

**dbHash**
O usuário pode realizar a dbHash comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**dbStats**
O usuário pode realizar a dbStats comando. Aplicar esta ação para recursos de banco de dados.

**diagLogging** 
O usuário pode realizar a diagLogging comando. Aplicar esta ação para o conjunto de recursos.

**getCmdLineOpts**
O usuário pode realizar a getCmdLineOpts comando. Aplicar esta ação para o conjunto de recursos.

**getLog**
O usuário pode realizar a getLog comando. Aplicar esta ação para o conjunto de recursos.

**indexStats**
O usuário pode realizar a indexStats comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

Alterado na versão 3.0: MongoDB 3.0 remove o indexStats comando.

**listDatabases**
O usuário pode realizar a listDatabases comando. Aplicar esta ação para o conjunto de recursos.

**listCollections**
O usuário pode realizar a listCollections comando. Aplicar esta ação para recursos de banco de dados.

**ListIndexes**
O usuário pode realizar a ListIndexes comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**netstat**
O usuário pode executar o netstat comando. Aplicar esta ação para o conjunto de recursos.

**serverStatus**
O usuário pode realizar a serverStatus comando. Aplicar esta ação para o conjunto de recursos.

**validate**
O usuário pode executar a validar comando. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**top**
O usuário pode executar a parte superior de comando. Aplicar esta ação para o conjunto de recursos.

#### `Internal Actions`
**anyAction**
Permite que qualquer ação em um recurso. Não atribuir essa ação, exceto em circunstâncias excepcionais.

**internal**
Permite ações internas. Não atribuir essa ação, exceto em circunstâncias excepcionais.




























