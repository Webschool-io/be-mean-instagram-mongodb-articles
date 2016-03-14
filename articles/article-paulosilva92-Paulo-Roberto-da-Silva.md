# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Paulo Roberto da Silva

**Data** 1455330838453

## Qual a diferença entre Autenticação e Autorização?
A autenticação é o processo que verifica a identidade de uma pessoa, no caso de sistemas computacionais, os usuarios. A autorização verifica se esta pessoa possui permissão para executar determinadas operações. Por este motivo a autenticação é o passo que vem sempre antes da autorização.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
Para criar um usuario temos a função `createUser()`. Essa função recebe o nome do usuario, sua senha e suas *roles*, que seriam o equivalente a  privilégios nos bancos relacionais. Abaixo temos um exemplo da criação de um usuario comum, que só poderá ler e escrever em determinado banco:
```
db.createUser(
    {
      user: "User",
      pwd: "12345678",
      roles: [
         { role: "readWrite", db: "products }
      ]
    }
)
```
Para criarmos um usuario que seja administrador temos que usar a role `dbAdmin`. Abaixo temos um exemplo:
```
db.createUser(
    {
      user: "Admin",
      pwd: "12345678",
      roles: [
         { role: "dbAdmin", db: "products" }
      ]
    }
)
```

## Explique cada papel listado em Cluster Administration Roles.
1. ClusterAdmin:
É o usuario que possui as maiores permissões para gerenciar os *clusters*. Esta função combina os privilegios garantidos pelo  *clusterManager*, *clusterMonitor* e o *hostManager*. Também pode usar a a função de *dropDatabase*, que permite excluir banco de dados.

2. clusterManager:
Tem permissões para gerencia e monitorar ações no cluster. Um usuário nesta função pode acessar os bancos da dados *config* e *local*, que são usados no *sharding* e na crição de réplicas, respectivamente.

3. clusterMonitor:
Só possui permissões de leitura das ferramentas de monitoramento dos *clusters*.

## Explique todas as ações de privilégio listadas em Privilege Actions.
#### 1. Ações de buscar e escrever:

  a) find: usuario pode usar `db.collection.find()`.

  b) insert: usuario pode usar o comando de insert e inserir dados na coleção desejada.

  c) remove: usuario pode remover dados da coleção desejada.

  d) update: usuario pode modificar dados existentes na coleção desejada.

#### 2. Ações de gerenciamento de banco de dados:

  a) changeCustomData: o usuário pode alterar as informações de costume de qualquer usuário no banco de dados fornecido.

  b) changeOwnCustomData: os usuários podem alterar suas próprias informações personalizadas.

  c) changeOwnPassword: usuarios podem alterar sua própria senha

  d) changePassword: usuários podem mudar a senha de outros usuários

  e) createCollection: usuários podem criar coleções no banco de dados.

  f) createIndex: Fornece acesso ao método db.collection.createIndex().

  g)createRole: usuario pode criar novas *roles*

  h) createUser: usuário pode criar novos usuários

  l) dropCollection: usuário pode deletar coleções no banco de dados.

  m) dropRole: usuário pode deletar qualquer *role*

  n) dropUser: usuário pode remover qualquer outro usuário do banco de dados

  o) emptycapped: usuário tem acesso ao comando *emptycapped*

  p) enableProfiler:
User can perform the `db.setProfilingLevel()` method. Apply this action to database resources.

  q) grantRole: usuário pode dar qualquer *role* pra outro usuário do banco

  r) killCursors: usuário pode "matar" qualquer cursor no banco

  s) revokeRole: usuário pode remover qualquer *role* de qualquer usuário do banco

  t) unlock: usuário tem acesso ao método `db.fsyncUnlock()`.

  u) viewRole: usuário pode ver qualquer informação de qualquer *role* do banco

  v) viewUser: usuário pode ver qualquer informação de qualquer usuário no banco

#### 3. Ações de gestão de implantação:

  a) authSchemaUpgrade: usuário tem acesso ao comando `authSchemaUpgrade`

  b) cleanupOrphaned: usuário tem acesso ao comando `cleanupOrphaned`

  c) cpuProfiler: usuário pode ativar e usar o *CPU profiler*

  d) inprog: usuário pode usar o método `db.currentOp()` para retornar operações que estejam pendentes e/ou ativas

  e) invalidateUserCache: fornece acesso ao comando `invalidateUserCache`

  f) killop: usuário pode utilizar o método `db.killOp()`

  g) planCacheRead: usuário pode utilizar os comandos `planCacheListPlans` e `planCacheListQueryShapes` e os métodos `PlanCache.getPlansByQuery()` e `PlanCache.listQueryShapes()`

  h) planCacheWrite: usuário pode utilizar o comando `planCacheClear` e os métodos `PlanCache.clearPlansByQuery()` e `PlanCache.clear()`

  i) storageDetails: usuário pode utlizar o comando `storageDetails`

#### 4. Ações de replicação

  a) appendOplogNote: usuário pode adicionar notas ao *oplog*

  b) replSetConfigure: usuário pode configurar uma *replica set*

  c) replSetGetStatus: usuário pode utlizar o comando `replSetGetStatus`

  d) replSetHeartbeat: usuario pode utlizar o comando `replSetHeartbeat`

  e) replSetStateChange: usuário pode alterar o estado de uma *replica set* através dos comandos `replSetStepDown`, `replSetSyncFrom`,`replSetFreeze`e `replSetMaintenance`

  f) resync: usuário pode utlizar o comando `resync`

#### 5. Ações de *Sharding*

  a) addShard: usuário pode utilizar o comando `addShard`

  b) enableSharding: usuário po ativar *sharding* em um banco usando o comando `enableSharding` e pode "*shardear*" uma coleção usando o comando `shardCollection`

  c) flushRouterConfig: usuário pode utilizar o comando `flushRouterConfig`

  d) getShardMap: usuário pode utilizar o comando `getShardMap`

  e) getShardVersion: usuário pode utilizar o comando `getShardVersion`

  f) listShards: usuário pode utilizar o comando `listShards`

  g) moveChunk: usuário pode utilizar o comando `moveChunk`. Além disso, o usuário pode tambem usar o comando `movePrimary`, desde que o privilégio esteja aplicado a um recurso do banco de dados apropriado

  h) removeShard: usuário pode utilizar o comando `removeShard`

  i) shardingState: usuário pode utilizar o comando 'shardingState'

  j) splitChunk: usuário pode utilizar o comando `splitChunk`

  l) splitVector: usuário pode utilizar o comando `splitVector`

#### 6. Ações de administração de Servidores

  a) applicationMessage: usuário pode utilizar o comando `logApplicationMessage`

  b) closeAllDatabases: usuário pode utilizar o comando `closeAllDatabases`

  c) collMod: usuário pode utilizar o comando `collMod`

  d) compact: usuário pode utilizar o comando `compact`

  e) connPoolSync: usuário pode utilizar o comando `connPoolSync`

  f) convertToCapped: usuário pode utilizar o comando `convertToCapped`

  g) dropDatabase: usuário pode utilizar o comando `dropDatabase`

  h) dropIndex: usuário pode utilizar o comando `dropIndexes`

  i) fsync: usuário pode utilizar o comando `fsyn`

  j) getParameter: usuário pode utilizar o comando `getParameter`

  l) hostInfo: fornece informação sobre o servidor que está a instancia do MongoDB

  m) logRotate: usuário pode utilizar o comando `logRotate`

  n) reIndex: usuário pode utilizar o comando `reIndex`

  o) renameCollectionSameDB: permite ao usuário renomear coleções na database atual com o comando `renameCollection`

  p) repairDatabase: usuário pode utilizar o comando `repairDatabase`

  q) setParameter: usuário pode utilizar o comando `setParameter`

  r) shutdown: usuário pode utilizar o comando `shutdown`

  s) touch: usuário pode utilizar o comando `touch`

#### 7. Ações de Diagnóstico

  a) collStats: usuário pode utilizar o comando `collStats`

  b) connPoolStats: usuário pode utilizar o comando `shardConnPoolStats`

  c) cursorInfo: usuário pode utilizar o comando `cursorInfo`

  d) dbHash: usuário pode utilizar o comando `dbHash`

  e) dbStats: usuário pode utilizar o comando `dbStats`

  f) diagLogging: usuário pode utilizar o comando `diagLogging`

  g) getCmdLineOpts: usuário pode utilizar o comando `getCmdLineOpts`

  h) getLog: usuário pode utilizar o comando `getLog`

  i) indexStats: usuário pode utilizar o comando `indexStats`

  j) listDatabases: usuário pode utilizar o comando `listDatabases`

  k) listCollections: usuário pode utilizar o comando `listCollections`

  l) listIndexes: usuário pode utilizar o comando `listIndexes`

  m) netstat: usuário pode utilizar o comando `netstat`

  n) serverStatus: usuário pode utilizar o comando `serverStatus`

  o) validate: usuário pode utilizar o comando `validate`

  p) top: usuário pode utilizar o comando `top`

#### 8. Ações internas

  a) anyAction: permite qualquer ação em um recurso. Não use essa ação, exceto em circunstâncias especiais

  b) internal: permite ações internas. Não use essa ação, exceto em circunstâncias especiais
