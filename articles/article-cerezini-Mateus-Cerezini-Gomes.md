# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Mateus Cerezini Gomes
**Data** 1457277348946

## Qual a diferença entre Autenticação e Autorização?
Autenticação é o processo de validação da identidade de um cliente do banco de dados. Autorização determina o acesso à recursos e operações do usuário autenticado.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Crie o objeto usuário com as propriedades:
- user: \<string> nome do usuário, deve ser único no banco.
- pwd: \<string> senha.
- customData: \<document> Informações opcionais e extras.
- roles: \<array> papéis que o usuário possuirá.

Os papéis podem ser os já existentes no mongodb ou novos definidos pelo usuário. Se o papel será sobre o banco em uso pode ser passado como string `roleName`, caso contrário deve ser passado o nome do banco `{role: 'roleName', db: 'dbName'}`.

Existe dois comandos para criação de usuários: `db.createUser() e db.runCommand()`, a diferença é que no segundo a propriedade *user* deve ser renomeada para *createUser*.

### Usuário administrador 
```js
>use admin
>db.createUser( {
    "user" : "userAdmin",
    "pwd": "senha123",
    "roles" : ["userAdminAnyDatabase"]    
})
```

### Usuário comum
```js
>use ex1
>db.createUser( {
    "user" : "userComum",
    "pwd": "123456",
    "roles" : [ "readWrite"]    
})
```

## Explique cada papel listado em Cluster Administration Roles.

### clusterAdmin

Possui o maior nível de acesso ao gerenciamento do cluster. É a combinação dos privilégios dos outros *roles* de administração de cluster: `clusterManager`, `clusterMonitor` e `hostManager`, além de possuir a ação `dropDatabase`.

### clusterManager

Provê ações de gerenciamento e monitoramento do cluster, pode acessar servidores `config` e `local`, que são usados nos processos de *sharding* e *replication* respectivamente.

### clusterMonitor

Provê acesso apenas leitura (*read-only*) às ferramentas e ações de monitoramento do cluster.

### hostManager

Provê a habilidade de monitorar e gerenciar servidores.


## Explique todas as ações de privilégio listadas em Privilege Actions.

### Query and Write Actions

#### find

Usuário pode executar o método `db.collection.find()`. Aplique esta ação a um banco de dados ou coleção.

#### insert

Usuário pode executar o comando `insert`. Aplique esta ação a um banco de dados ou coleção.

#### remove

Usuário pode executar o método `db.collection.remove()`. Aplique esta ação a um banco de dados ou coleção.

#### update

Usuário pode executar o comando `update`. Aplique esta ação a um banco de dados ou coleção.

#### bypassDocumentValidation

Usuário pode ignorar a validação de documentos nos comandos que suportam a opção `bypassDocumentValidation`. Para uma lista de comandos que suportam a opção `bypassDocumentValidation`, veja [Document Validation](https://docs.mongodb.org/manual/release-notes/3.2/#rel-notes-document-validation). Aplique esta ação a um banco de dados ou coleção.

### Database Management Actions

#### changeCustomData

Usuário pode mudar as informações comuns de qualquer usuário de um dado banco de dados. Aplique esta ação a um banco de dados.

#### changeOwnCustomData

Usuários podem mudar suas próprias informações comuns. Aplique esta ação a um banco de dados. Veja também [Change Your Password and Custom Data](https://docs.mongodb.org/manual/tutorial/change-own-password-and-custom-data/).

#### changeOwnPassword

Usuários podem mudar suas próprias senhas. Aplique esta ação a um banco de dados. Veja também [Change Your Password and Custom Data](https://docs.mongodb.org/manual/tutorial/change-own-password-and-custom-data/).

#### changePassword
Usuário pode mudar a senha de qualquer usuário de um dado banco de dados. Aplique esta ação a um banco de dados.

#### createCollection

Usuário pode executar o método `db.createCollection()`. Aplique esta ação a um banco de dados ou coleção.

#### createIndex

Provê acesso ao método `db.collection.createIndex()` e ao comando `createIndexes`. Aplique esta ação a um banco de dados ou coleção.

#### createRole

Usuários podem criar novos papéis em um dado banco de dados. Aplique esta ação a um banco de dados. 

#### createUser

Usuários podem criar novos usuários em um dado banco de dados. Aplique esta ação a um banco de dados.

#### dropCollection

Usuário pode executar o método `db.dropCollection()`. Aplique esta ação a um banco de dados ou coleção.

#### dropRole

Usuário pode deletar qualquer papel de um dado banco de dados. Aplique esta ação a um banco de dados.

#### dropUser

Usuário pode deletar qualquer usuário de um dado banco de dados. Aplique esta ação a um banco de dados.

#### emptycapped

Usuário pode executar o comando `emptycapped`. Aplique esta ação a um banco de dados ou coleção.

#### enableProfiler

Usuário pode executar o método `db.setProfilingLevel()`. Aplique esta ação a um banco de dados.

#### grantRole

Usuário pode atribuir qualquer papel do banco de dados a qualquer usuário de qualquer banco de dados do sistema. Aplique esta ação a um banco de dados.

#### killCursors

Usuário pode finalizar cursores em uma coleção alvo.

#### revokeRole

Usuário pode remover qualquer papel de qualquer usuário de qualquer banco de dados do sistema. Aplique esta ação a um banco de dados.

#### unlock

Usuário pode executar o método `db.fsyncUnlock()`. Aplique esta ação a um cluster.

#### viewRole

Usuário pode visualizar informações sobre qualquer papel em um dado banco de dados. Aplique esta ação a um banco de dados.

#### viewUser

Usuário pode visualizar informações sobre qualquer usuário em um dado banco de dados. Aplique esta ação a um banco de dados.

### Deployment Management Actions

#### authSchemaUpgrade

Usuário pode executar o comando `authSchemaUpgrade`. Aplique esta ação a um cluster.

#### cleanupOrphaned

Usuário pode executar o comando `cleanupOrphaned`. Aplique esta ação a um cluster.

#### cpuProfiler

Usuário pode habilitar e usar o *CPU profiler*. Aplique esta ação a um cluster.

#### inprog

Usuário pode executar o método `db.currentOp()` e retornar operações ativas e pendentes. Aplique esta ação a um cluster.

#### invalidateUserCache

Provê acesso ao comando `invalidateUserCache`. Aplique esta ação a um cluster.

#### killop

Usuário pode executar o método `db.killOp()`. Aplique esta ação a um cluster.

#### planCacheRead

Usuário pode executar os comandos `planCacheListPlans` e `planCacheListQueryShapes` e os métodos `PlanCache.getPlansByQuery()` e `PlanCache.listQueryShapes()`. Aplique esta ação a um banco de dados ou coleção.

#### planCacheWrite

Usuário pode executar o comando `planCacheClear` e os métodos `PlanCache.clear()` e `PlanCache.clearPlansByQuery()`. Aplique esta ação a um banco de dados ou coleção.

#### storageDetails

Usuário pode executar o comando `storageDetails`. Aplique esta ação a um banco de dados ou coleção.

### Replication Actions

#### appendOplogNote

Usuário pode adicionar anotações ao *oplog*. Aplique esta ação a um cluster.

#### replSetConfigure

Usuário pode configurar uma *replica set*. Aplique esta ação a um cluster.

#### replSetGetStatus

Usuário pode executar o comando `replSetGetStatus`. Aplique esta ação a um cluster.

#### replSetHeartbeat

Usuário pode executar o comando `replSetHeartbeat`. Aplique esta ação a um cluster.

#### replSetStateChange

Usuário pode mudar o estado de uma *replica set* através dos comandos: `replSetFreeze`, `replSetMaintenance`, `replSetStepDown`, e `replSetSyncFrom`. Aplique esta ação a um cluster.

#### resync

Usuário pode executar o comando `resync`. Aplique esta ação a um cluster.

### Sharding Actions

#### addShard

Usuário pode executar o comando `addShard`. Aplique esta ação a um cluster.

#### enableSharding

Usuário pode habilitar *sharding* em um banco de dados usando o comando `enableSharding` e pode *shard* uma coleção usando o comando `shardCollection`. Aplique esta ação a um banco de dados ou coleção.

#### flushRouterConfig

Usuário pode executar o comando `flushRouterConfig`. Aplique esta ação a um cluster.

#### getShardMap

Usuário pode executar o comando `getShardMap`. Aplique esta ação a um cluster.

#### getShardVersion

Usuário pode executar o comando `getShardVersion`. Aplique esta ação a um banco de dados.

#### listShards

Usuário pode executar o comando `listShards`. Aplique esta ação a um cluster.

#### moveChunk

Usuário pode executar o comando `moveChunk`. Além disso, usuário pode executar o comando `movePrimary` provido para o privilégio ser aplicado ao banco de dados apropriado. Aplique esta ação a um banco de dados ou coleção.

#### removeShard

Usuário pode executar o comando `removeShard`. Aplique esta ação a um cluster.

#### shardingState

Usuário pode executar o comando `shardingState`. Aplique esta ação a um cluster.

#### splitChunk

Usuário pode executar o comando `splitChunk`. Aplique esta ação a um banco de dados ou coleção.

#### splitVector

Usuário pode executar o comando `splitVector`. Aplique esta ação a um banco de dados ou coleção.

### Server Administration Actions

#### applicationMessage

Usuário pode executar o comando `logApplicationMessage`. Aplique esta ação a um cluster.

#### closeAllDatabases

Usuário pode executar o comando `closeAllDatabases`. Aplique esta ação a um cluster.

#### collMod

Usuário pode executar o comando `collMod`. Aplique esta ação a um banco de dados ou coleção.

#### compact

Usuário pode executar o comando `compact`. Aplique esta ação a um banco de dados ou coleção.

#### connPoolSync

Usuário pode executar o comando `connPoolSync`. Aplique esta ação a um cluster.

#### convertToCapped

Usuário pode executar o comando `convertToCapped`. Aplique esta ação a um banco de dados ou coleção.

#### dropDatabase

Usuário pode executar o comando `dropDatabase`. Aplique esta ação a um banco de dados.

#### dropIndex

Usuário pode executar o comando `dropIndexes`. Aplique esta ação a um banco de dados ou coleção.

#### fsync

Usuário pode executar o comando `fsync`. Aplique esta ação a um cluster.

#### getParameter

Usuário pode executar o comando `getParameter`. Aplique esta ação a um cluster.

#### hostInfo

Provê informações sobre o servidor no qual a instância do MongoDB está sendo executada. Aplique esta ação a um cluster.

#### logRotate

Usuário pode executar o comando `logRotate`. Aplique esta ação a um cluster.

#### reIndex

Usuário pode executar o comando `reIndex`. Aplique esta ação a um banco de dados ou coleção.

#### renameCollectionSameDB

Permite o usuário renomear coleções no banco de dados em uso usando o comando `renameCollection`. Aplique esta ação a um banco de dados.

Além disso, o usuário deve possuir a ação `find` na coleção origem ou não ter `find` na coleção destino.

Se a coleção com o novo nome já existe, o usuário deve também ter a ação `dropCollection` sobre a coleção destino.

#### repairDatabase

Usuário pode executar o comando `repairDatabase`. Aplique esta ação a um banco de dados.

#### setParameter

Usuário pode executar o comando `setParameter`. Aplique esta ação a um cluster.

#### shutdown

Usuário pode executar o comando `shutdown`. Aplique esta ação a um cluster.

#### touch

Usuário pode executar o comando `touch`. Aplique esta ação a um cluster.

### Diagnostic Actions

#### collStats

Usuário pode executar o comando `collStats`. Aplique esta ação a um banco de dados ou coleção.

#### connPoolStats

Usuário pode executar os comandos `connPoolStats` e `shardConnPoolStats`. Aplique esta ação a um cluster.

#### cursorInfo

Usuário pode executar o comando `cursorInfo`. Aplique esta ação a um cluster.

#### dbHash

Usuário pode executar o comando `dbHash`. Aplique esta ação a um banco de dados ou coleção.

#### dbStats

Usuário pode executar o comando `dbStats`. Aplique esta ação a um banco de dados.

#### diagLogging

Usuário pode executar o comando `diagLogging`. Aplique esta ação a um cluster.

#### getCmdLineOpts

Usuário pode executar o comando `getCmdLineOpts`. Aplique esta ação a um cluster.

#### getLog

Usuário pode executar o comando `getLog`. Aplique esta ação a um cluster.

#### indexStats

Usuário pode executar o comando `indexStats`. Aplique esta ação a um banco de dados ou coleção.

#### listDatabases

Usuário pode executar o comando `listDatabases`. Aplique esta ação a um cluster.

#### listCollections

Usuário pode executar o comando `listCollections`. Aplique esta ação a um banco de dados.

#### listIndexes

Usuário pode executar o comando `ListIndexes`. Aplique esta ação a um banco de dados ou coleção.

#### netstat

Usuário pode executar o comando `netstat`. Aplique esta ação a um cluster.

#### serverStatus

Usuário pode executar o comando `serverStatus`. Aplique esta ação a um cluster.

#### validate

Usuário pode executar o comando `validate`. Aplique esta ação a um banco de dados ou coleção.

#### top

Usuário pode executar o comando `top`. Aplique esta ação a um cluster.

### Internal Actions

#### anyAction

Permite qualquer ação sobre um recurso. Não dê esta ação exceto em circunstâncias excepcionais.

#### internal

Permite ações internas. Não dê esta ação exceto em circunstâncias excepcionais.

