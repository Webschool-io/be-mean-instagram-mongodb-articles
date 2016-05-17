# Artigo sobre Autenticação no MongoDb

**Autor**: [Jean Martins Rito](http://github.com/rito)
**User**: rito
**Data entregue**: 1461689297706


1) Qual a diferença entre Autenticação e Autorização?

A autenticação é a verificação das credenciais em uma tentativa de conexão. Esse processo consiste no envio de credenciais do cliente (por exemplo, o envio de um nome de usuário e uma senha). Este processo consiste em verificar se este usuário é válido/autêntico.

A autorização é a verificação de que a tentativa de conexão é permitida. A autorização ocorre após a autenticação bem-sucedida e determina ao usuário autenticado o acesso aos recursos e ações, ou seja, suas respectivas permissões.

Para um tentativa de conexão ser aceita, ela deve ser autenticada e autorizada. É possível que uma tentativa de conexão seja autenticada usando credenciais válidas, mas não seja autorizada. Nesse caso, a tentativa de conexão será negada.


2) Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.


Comando para criação de um usuário comum:

```
db.createUser( {
  "user" : "User01Comum",
  "pwd": "pass0123456",
  "roles" : [ "readWrite"]    
})

```
Resultado:
```
Mockingjay(mongod-3.0.7) admin> db.createUser( {
...     "user" : "User01Comum",
...     "pwd": "pass0123456",
...     "roles" : [ "readWrite"]    
... })
Successfully added user: {
  "user": "User01Comum",
  "roles": [
    "readWrite"
  ]
}
Mockingjay(mongod-3.0.7) admin>
```

Para criação de um usuário Admin, primeiramente é necessário conectar na base 'admin':

```
Mockingjay(mongod-3.0.7) test> use admin
switched to db admin
Mockingjay(mongod-3.0.7) admin>

```
Comando:

```
db.createUser(
  {
    user: "UserAdmin",
    pwd: "passwd123",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
```
Resultado:

```
Mockingjay(mongod-3.0.7) teste> db.createUser(
...   {
...     user: "UserAdmin",
...     pwd: "passwd123",
...     roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
...   }
... )
Successfully added user: {
  "user": "UserAdmin",
  "roles": [
    {
      "role": "userAdminAnyDatabase",
      "db": "admin"
    }
  ]
}
Mockingjay(mongod-3.0.7) teste>

```

3) Explique cada papel listado [aqui em Cluster Administration Roles](https://docs.mongodb.org/v2.6/reference/built-in-roles/#cluster-administration-roles).

#### clusterAdmin
  Fornece o maior acesso de administração de *cluster*. Esta regra combina os privilégios concedidos pelas regras **clusterManager**, **clusterMonitor**, e **hostManager**. Além disso ainda fornece a ação `dropDatabase`.

#### clusterManager
  Fornece ações de administração e monitoramento no cluster. Um usuário com essa regra pode acessar os bancos `config` e `local`, que são usadas no *sharding* e na *replication*, respectivamente.

  Fornece as seguintes ações no cluster:

    * addShard
    * applicationMessage
    * cleanupOrphaned
    * flushRouterConfig
    * listShards
    * removeShard
    * replSetConfigure
    * replSetGetStatus
    * replSetStateChange
    * resync

  Fornece as seguintes ações em todos os bancos do cluster:

    * enableSharding
    * moveChunk
    * splitChunk
    * splitVector

  No banco `config`, fornece as seguintes ações na coleção `settings`:

    * insert
    * remove
    * update

  No banco `config`, fornece as seguintes ações em todas as coleções de configuração e nas coleções `system.indexes`, `system.js` e `system.namespaces`:

    * collStats
    * dbHash
    * dbStats
    * find
    * killCursors

  No banco `local`, fornece as seguintes ações na coleção `replSet`

    * collStats
    * dbHash
    * dbStats
    * find
    * killCursors

#### clusterMonitor
  Fornece acesso *read-only* (somente de leitura) as ferramentas de monitoramento, como o *MongoDB Cloud Manager monitoring agent*

  Fornece as seguintes ações no cluster:

    * connPoolStats
    * cursorInfo
    * getCmdLineOpts
    * getLog
    * getParameter
    * getShardMap
    * hostInfo
    * inprog
    * listDatabases
    * listShards
    * netstat
    * replSetGetStatus
    * serverStatus
    * shardingState
    * top

  Fornece as seguintes ações em todas os bancos do cluster:

    * collStats
    * dbStats
    * getShardVersion

  Fornece a ação `find` em todas as coleções `system.profile` do cluster.

  Fornece as seguintes ações nas coleções de configuração `config` do banco e nas coleções `system.indexes`, `system.js` e `system.namespaces`:

    * collStats
    * dbHash
    * dbStats
    * find
    * killCursors

#### hostManager
  Fornece a habilidade para monitorar e administrar servidores.

  Fornece as seguintes ações no cluster:

    * applicationMessage
    * closeAllDatabases
    * connPoolSync
    * cpuProfiler
    * diagLogging
    * flushRouterConfig
    * fsync
    * invalidateUserCache
    * killop
    * logRotate
    * resync
    * setParameter
    * shutdown
    * touch
    * unlock

  Fornece as seguintes ações em todos os bancos do cluster:

    * killCursors
    * repairDatabase


4) Explique todas as ações de privilégio listadas [aqui Privilege Actions](https://docs.mongodb.org/manual/reference/privilege-actions/).

### Query and Write Actions

#### find
O usuário pode executar o método `db.collection.find()`. Aplicar esta ação para banco de dados ou collections.

  Seleciona documentos numa coleção e retorna um cursor dos documentos selecionados.

#### insert
O usuário pode executar o comando de inserção. Aplicar esta ação para banco de dados ou collections.

  O comando `insert` insere um ou mais documentos e retorna um documento contendo o status de todas as inserções. O método `insert` fornecido pelos drives do MongoDB usam este comando internamente.

#### remove
O usuário pode executar o método `db.collection.remove()` . Aplicar esta ação para banco de dados ou collections.

  Remove um documento de uma coleção.

#### update
O usuário pode executar o comando `update`. Aplicar esta ação para banco de dados ou collections.

  O comando `update` modifica documentos numa coleção. Um único comando `update` pode conter múltiplas declarações de atualização.

  #### bypassDocumentValidation
  *Novo na versão 3.2*

Usuário pode ignorar a validação de documentos sobre os comandos que suportam a opção `bypassDocumentValidation`. Para obter uma lista de comandos que suportam a opção `bypassDocumentValidation`, ver [Documento de Validação](https://docs.mongodb.org/manual/release-notes/3.2/#rel-notes-document-validation). Aplicar esta ação para banco de dados ou collections.

### Database Management Actions

#### changeCustomData
O usuário pode alterar as *custom information* de qualquer usuário no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

#### changeOwnCustomData
Os usuários podem alterar suas próprias informações personalizadas. Aplicar esta ação para recursos de banco de dados. Veja também alterar sua senha e dados personalizados.

#### changeOwnPassword
Os usuários podem alterar suas próprias senhas. Aplicar esta ação para recursos de banco de dados. Veja também alterar sua senha e dados personalizados.

#### changePassword
O usuário pode alterar a senha de qualquer usuário no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

#### createCollection
O usuário pode executar o método `db.createCollection()`. Aplicar esta ação para banco de dados ou collections.

  Cria uma nova coleção explicitamente.

#### createIndex
Fornece acesso ao método `db.collection.createIndex()` e o comando `createIndexes`. Aplicar esta ação para banco de dados ou collections.

  Cria índices em coleções.

#### createRole
O usuário pode criar novas funções no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

#### createUser
O usuário pode criar novos usuários no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

#### dropCollection
O usuário pode executar o método `db.collection.drop()`. Aplicar esta ação para banco de dados ou collections.

  Remove uma coleção de uma database. O método também remove todos os índices associados com a coleção deletada.

#### dropRole
O usuário pode excluir qualquer regra do banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

#### dropUser
O usuário pode remover qualquer usuário do banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

#### emptycapped
O usuário pode executar o comando `emptycapped`. Aplicar esta ação para banco de dados ou collections.

  O comando `emptycapped` remove todos os documentos de uma *capped collection*.

#### enableProfiler
O usuário pode executar o método `db.setProfilingLevel()`. Aplicar esta ação para recursos de banco de dados.

  Modifica o *database profiler level* atual usado pela *database profiling system* para obter dados sobre performance.

#### grantRole
O usuário pode conceder qualquer regra no banco de dados para qualquer usuário de qualquer banco de dados no sistema. Aplicar esta ação para recursos de banco de dados.

#### killCursors
O usuário pode matar os cursores numa determinada collection.

### revokeRole
O usuário pode remover qualquer regra de qualquer usuário a partir de qualquer banco de dados no sistema. Aplicar esta ação para recursos de banco de dados.

#### unlock
O usuário pode executar o método `db.fsyncUnlock()`. Aplicar esta ação para o recurso de cluster.

Desbloqueia uma instância `mongod` para permitir escritas e reverte a operação de uma operação `db.fsyncLock()`. Tipicamente você usará `db.fsyncUnlock()` seguindo uma operação de *backup* de database.
`db.fsyncUnlock()` é uma operação administrativa.

#### viewRole
O usuário pode visualizar informações sobre qualquer função no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

#### viewUser
O usuário pode visualizar as informações de qualquer usuário no banco de dados fornecido. Aplicar esta ação para recursos de banco de dados.

### Deployment Management Actions

#### authSchemaUpgrade
O usuário pode executar o comando authSchemaUpgrade. Aplicar esta ação para o recurso de cluster.

  *Lançado na versão 2.6* e *Alterado na versão 3.0*

  `authSchemaUpgrade` suporta o processo de *upgrade* para sistemas existentes que usam autenticação e autorização entre:

    * 2.4 e 2.6 (acesse [Upgrade User Authorization Data to 2.6 Format](https://docs.mongodb.org/manual/release-notes/2.6-upgrade-authorization/))
    * 2.6 e 3.0 (acesse [Upgrade to SCRAM-SHA-1](https://docs.mongodb.org/manual/release-notes/3.0-scram/))


#### cleanupOrphaned
O usuário pode executar o comando cleanupOrphaned. Aplicar esta ação para o recurso de cluster.

  *Lançado na versão 2.6*

  Remove os documentos órfãos de um shard cujos os valores de shard key caem num único, ou numa única faixa contígua que não pertencem ao shard.
  Por exemplo, se duas faixas contíguas não pertencem ao shard, o `cleanupOrphaned` examina ambas as faixas a procura de documentos órfãos.


#### cpuProfiler
O usuário pode ativar e usar o profiler CPU. Aplicar esta ação para o recurso de cluster.

#### inprog
O usuário pode usar o método `db.currentOp()` para retornar pendentes e ativos operações. Aplicar esta ação para o recurso de cluster.

  Retorna um documento que contém informação de operações em progresso da instância da base de dados.

#### invalidateUserCache
Fornece acesso ao comando `invalidateUserCache`. Aplicar esta ação para o recurso de cluster.

  *Lançado na versão 2.6*

  Iguala as informações de usuário do cache em memória, incluindo remoções de *credenciais* e *regras* de cada usuário. Isto permite limpar o cache a qualquer momento, independente do intervalo definido no parâmetro `userCacheInvalidationIntervalSecs`.


#### killop
O usuário pode executar o método `db.killOp()`. Aplicar esta ação para o recurso de cluster.

  Finaliza a execução de uma operação especifica por seu ID. Para ver como encontrar operações e seus correspondentes IDs, acesse [db.currentOp()](https://docs.mongodb.org/manual/reference/method/db.currentOp/#db.currentOp).

#### planCacheRead
O usuário pode executar os comandos `planCacheListPlans` e `planCacheListQueryShapes` e os métodos `PlanCache.getPlansByQuery()` e `PlanCache.listQueryShapes()`. Aplicar esta ação para banco de dados ou collections.

##### planCacheListPlans
    *Lançado na versão 2.6*

    Mostra as *query plans* em cache para uma [*query shape*](https://docs.mongodb.org/manual/reference/glossary/#term-query-shape) especificada.

    O *query optimizer* apenas coloca em cache os planos para essas *query shapes*, que podem ter mais de um plano viável.

    O mongo shell proporciona o *wrapper* `PlanCache.getPlansByQuery()` para este comando.

##### planCacheListQueryShapes
    *Lançado na versão 2.6*

    Mostra as *query shapes* de cada *query plan* em cache existentes para uma coleção.

    O *query optimizer* apenas coloca em cache os planos para essas *query shapes*, que podem ter mais de um plano viável.

    O mongo shell proporciona o *wrapper* `PlanCache.listQueryShapes()` para este comando.


#### planCacheWrite
O usuário pode executar o comando `planCacheClear` e os métodos `PlanCache.clear()` e `PlanCache.clearPlansByQuery()`. Aplicar esta ação para banco de dados ou collections.


##### planCacheClear
  *Lançado na versão 2.6*

  Remove *query plans* em cache de uma coleção. Especifique uma *query shape* para remover *query plans* em cache dessa *shape*. Omita a *query shape* para limpar todas as *query plans* em cache.

##### PlanCache.clear()
  Remove todas as *query plans* em cache de uma coleção. O método é disponível apenas num [*plan cache object*](https://docs.mongodb.org/manual/reference/method/db.collection.getPlanCache/#db.collection.getPlanCache) de uma coleção específica.

##### PlanCache.clearPlansByQuery()
  Limpa as *query plans* em cache especificadas pela *query shape*

  O método é disponível apenas num [*plan cache object*](https://docs.mongodb.org/manual/reference/method/db.collection.getPlanCache/#db.collection.getPlanCache) de uma coleção específica.


#### storageDetails
O usuário pode executar o comando `storageDetails`. Aplicar esta ação para banco de dados ou collections.

### Replication Actions

#### appendOplogNote
O usuário pode acrescentar notas ao `oplog`. Aplicar esta ação para o recurso de cluster.

#### replSetConfigure
O usuário pode configurar um conjunto de réplicas. Aplicar esta ação para o recurso de cluster.

#### replSetGetStatus
O usuário pode executar o comando `replSetGetStatus`. Aplicar esta ação para o recurso de cluster.

#### replSetHeartbeat
O usuário pode executar o comando `replSetHeartbeat`. Aplicar esta ação para o recurso de cluster.

#### replSetStateChange
O usuário pode alterar o estado de um conjunto de réplicas através da `replSetFreeze`, comandos `replSetMaintenance`, `replSetStepDown` e `replSetSyncFrom`. Aplicar esta ação para o recurso de cluster.

#### resync
O usuário pode executar o comando `resync`. Aplicar esta ação para o recurso de cluster.

### ações Sharding

#### addShard
O usuário pode executar o comando `addShard`. Aplicar esta ação para o recurso de cluster.

#### enableSharding
Usuário pode ativar sharding em um banco de dados usando o comando `enableSharding` e pode fragmentar uma coleção usando o comando `shardCollection`. Aplicar esta ação para banco de dados ou collection.

#### flushRouterConfig
O usuário pode executar o comando `flushRouterConfig`. Aplicar esta ação para o recurso de cluster.

#### getShardMap
O usuário pode executar o comando `getShardMap`. Aplicar esta ação para o recurso de cluster.

#### getShardVersion
O usuário pode executar o comando `getShardVersion`. Aplicar esta ação para recursos de banco de dados.

#### listShards
O usuário pode executar o comando `listShards`. Aplicar esta ação para o recurso de cluster.

#### moveChunk
O usuário pode executar o comando `moveChunk`. Além disso, o utilizador pode executar o comando `movePrimary` desde que o privilégio é aplicado a um recurso base de dados apropriada. Aplicar esta ação para banco de dados ou collection.

#### removeShard
O usuário pode executar o comando `removeShard`. Aplicar esta ação para o recurso de cluster.

#### shardingState
O usuário pode executar o comando `shardingState`. Aplicar esta ação para o recurso de cluster.

#### splitChunk
O usuário pode executar o comando `splitChunk`. Aplicar esta ação para banco de dados ou collections.

#### splitVector
O usuário pode executar o comando `splitVector`. Aplicar esta ação para banco de dados ou collections.

### Ações de Administração de Servidor

#### applicationMessage
O usuário pode executar o comando `logApplicationMessage`. Aplicar esta ação para o recurso de cluster.

#### closeAllDatabases
O usuário pode executar o comando `closeAllDatabases`. Aplicar esta ação para o recurso de cluster.

#### collMod
O usuário pode executar o comando `collMod`. Aplicar esta ação para banco de dados ou de cobrança de recursos.

#### compact
O usuário pode executar o comando `compact`. Aplicar esta ação para banco de dados ou de cobrança de recursos.

#### connPoolSync
O usuário pode executar o comando `connPoolSync`. Aplicar esta ação para o recurso de cluster.

#### convertToCapped
O usuário pode executar o comando `convertToCapped`. Aplicar esta ação para banco de dados collections.

#### dropDatabase
O usuário pode executar o comando `dropDatabase`. Aplicar esta ação para recursos de banco de dados.

#### dropIndex
O usuário pode executar o comando `dropIndexes`. Aplicar esta ação para banco de dados ou collections.

#### fsync
O usuário pode executar o comando `fsync`. Aplicar esta ação para o recurso de cluster.

#### getParameter
O usuário pode executar o comando `getParameter`. Aplicar esta ação para o recurso de cluster.

#### hostInfo
Fornece informações sobre o servidor a instância MongoDB é executado. Aplicar esta ação para o recurso de cluster.

#### logrotate
O usuário pode executar o comando `logrotate`. Aplicar esta ação para o recurso de cluster.

#### reIndex
O usuário pode executar o comando `reIndex`. Aplicar esta ação para banco de dados ou collections.

#### renameCollectionSameDB
Permite que o usuário renomear coleções no banco de dados atual usando o comando `renameCollection`. Aplicar esta ação para recursos de banco de dados.

Além disso, o usuário deve/têm que encontrar na coleção de origem ou não têm encontrar na coleção do destino.

Se uma coleção com o novo nome já existe, o usuário também deve ter a ação `dropCollection` na collection do destino.

#### RepairDatabase
O usuário pode executar o comando `RepairDatabase`. Aplicar esta ação para recursos de banco de dados.

#### setParameter
O usuário pode executar o comando `setParameter`. Aplicar esta ação para o recurso de cluster.

#### shutdown
O usuário pode executar o comando de `shutdown`. Aplicar esta ação para o recurso de cluster.

#### touch
O usuário pode executar o comando `touch`. Aplicar esta ação para o recurso de cluster.

### Ações de Diagnóstico

#### collStats
O usuário pode executar o comando `collStats`. Aplicar esta ação para banco de dados ou de cobrança de recursos.

#### connPoolStats
O usuário pode executar as `connPoolStats` e comandos `shardConnPoolStats`. Aplicar esta ação para o recurso de cluster.

#### cursorInfo
O usuário pode executar o comando `cursorInfo`. Aplicar esta ação para o recurso de cluster.

#### dbHash
O usuário pode executar o comando `dbHash`. Aplicar esta ação para banco de dados ou collections.

#### dbStats
O usuário pode executar o comando `dbStats`. Aplicar esta ação para recursos de banco de dados.

#### diagLogging
O usuário pode executar o comando `diagLogging`. Aplicar esta ação para o recurso de cluster.

#### getCmdLineOpts
O usuário pode executar o comando `getCmdLineOpts`. Aplicar esta ação para o recurso de cluster.

#### getLog
O usuário pode executar o comando `getLog`. Aplicar esta ação para o recurso de cluster.

#### indexStats
O usuário pode executar o comando `indexStats`. Aplicar esta ação para banco de dados ou collections.

##### Alterado na versão 3.0: MongoDB 3.0 removeu o comando indexStats.

#### listDatabases
O usuário pode executar o comando `listDatabases`. Aplicar esta ação para o recurso de cluster.

#### listCollections
O usuário pode executar o comando `listCollections`. Aplicar esta ação para recursos de banco de dados.

#### ListIndexes
O usuário pode executar o comando `ListIndexes`. Aplicar esta ação para banco de dados ou collections.

#### netstat
O usuário pode executar o comando `netstat`. Aplicar esta ação para o recurso de cluster.

#### serverStatus
O usuário pode executar o comando `serverStatus`. Aplicar esta ação para o recurso de cluster.

#### validate
O usuário pode executar o comando `validate`. Aplicar esta ação para banco de dados ou collections.

#### top
O usuário pode executar o comando `top`. Aplicar esta ação para o recurso de cluster.

### Ações Internas

#### anyAction
Permite que qualquer ação em um recurso. Não atribua essa ação, exceto em circunstâncias excepcionais.

#### internal
Permite que ações internas. Não atribua essa ação, exceto em circunstâncias excepcionais.
