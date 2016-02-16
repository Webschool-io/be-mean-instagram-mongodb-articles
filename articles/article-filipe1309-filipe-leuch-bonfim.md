# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Filipe Leuch Bonfim<br>
**Data** 1454340549030

## Qual a diferença entre Autenticação e Autorização?

Quando é necessário colocar limites de acesso à usuários específicos, em uma página Web, se faz necessário a utilização de **Autenticação** e **Autorização**.

#### Mas o que cada um quer dizer?

##### Autenticação
Se refere a confirmação da identidade do usuário, ou seja, se ele é quem diz ser. Esta verificação é feita geralmente com usuário e senha, mas também pode ser realizada através de certificados digitais, impressões digitais, smart cards, etc.

> representa a forma como o usuário prova quem realmente ele é

##### Autorização
É a parte em que ocorre a verificação se determinado usuário, previamente
autenticado, possui permissão para usar, manipular ou executar o recurso
solicitado.

> Ocorre após a autenticação, e indica quais recursos são permitidos ao usuário autenticado

##### Exemplo
Em um sistema, uma página só é acessível, se estas duas etapas forem bem sucedidas. E existem páginas que só são acessíveis a certos tipos de usuários, como um menu de administração, por exemplo.

#### Outra maneira de entender estes conceitos, é pensar nas perguntas que cada um responde:

##### Autenticação
- Você é quem diz ser?
- Quem é o usuário?

##### Autorização
- Você pode acessar este conteúdo?
- O que um usuário (já autenticado) tem permissão de fazer?


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
O que diferencia os tipos de usuário (comum e administrador), são os papéis chamados de `roles` no mongoDb.

Para criamos um usuário com privilégios de administrador, basta adicionar o papél `` userAdmin`` ou ``userAdminAnyDatabas``.

#### Usuário administrador
```javascript
use admin
db.createUser(
  {
    user: "filipe-admin",
    pwd: "123mudar",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
```

#### Usuário comum
```javascript
use admin
db.createUser(
   {
     user: "filipe-user",
     pwd: "123mudar",
     roles: [ ]
   }
)
```

## Explique cada papel listado em Cluster Administration Roles.
Os seguintes papeis são utilizados para administrar o sistema inteiro, incluindo funções para a adminstração de ``replica set`` e ``sharded cluster``

#### clusterAdmin
Este papel possibilita o maior nível de acesso para a administração de clusters, e inclui todos os outros papéis do Cluster Administration Roles.    

#### clusterManager
Este papel possibilita o gerenciamento e monitoramento de ações de um cluster.
Inclui as seguintes ações:
 - No cluster como um todo:
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
- Em todas databases do cluster:
enableSharding
moveChunk
splitChunk
splitVector
- Na coleção ``settings`` da database config:
insert
remove
update
- Em todas configurações de coleções e nas coleções system.indexes, system.js, e system.namespaces, na database config:
collStats
dbHash
dbStats
find
killCursors

- Na coleção replset, da database local:
collStats
dbHash
dbStats
find
killCursors

#### clusterMonitor

Este papel possibilita o acesso, no modo de somente leitura, as ferramentas de monitoramento.
Inclui as seguintes ações:
 - No cluster como um todo:
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
- Em todas databases do cluster:
collStats
dbStats
getShardVersion
- Em todas coleções ``system.profile`` do cluster:
find
- Em todas configurações de coleções e nas coleções system.indexes, system.js, e system.namespaces, na database config:
collStats
dbHash
dbStats
find
killCursors

#### hostManager
Este papel possibilita monitorar e gerenciar servidores.
Inclui as seguintes ações:
 - No cluster como um todo:
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

- Em todas databases do cluster:
killCursors
repairDatabase




## Explique todas as ações de privilégio listadas em Privilege Actions.
As ações de privilégio definem as operações que um usuário pode efetuar sobre um recurso (database e/ou coleção).

### Ações de Consulta e Escrita
#### find
O usuário pode executar o método db.collection.find(). Esta ação pode ser aplicada para uma ``database`` ou uma ``collection``
#### insert
O usuário pode executar o comando ``insert``. Esta ação pode ser aplicada para uma ``database`` ou uma ``collection``
#### remove
O usuário pode executar o método db.collection.remove(). Esta ação pode ser aplicada para uma ``database`` ou uma ``collection``
#### update
O usuário pode executar o comando ``update``. Esta ação pode ser aplicada para uma ``database`` ou uma ``collection``
#### bypassDocumentValidation
O usuário pode ignorar uma validação de documento nos comandos que suportam o opção ``bypassDocumentValidation``

### Ações para Gerenciamento de databases
#### changeCustomData
É uma ação aplicada a um recurso de database, que o usuário pode alterar a informação customizada (‘custom information’) de qualquer usuário da database informada.

#### changeOwnCustomData
É uma ação aplicada a um recurso de database, que o usuário pode alterar a própria informação customizada (‘custom information’).

#### changeOwnPassword
É uma ação aplicada a um recurso de database, que o usuário pode alterar a própra senha.

#### changePassword
É uma ação aplicada a um recurso de database, que o usuário pode alterar a senha de qualquer usuário da database informada.

#### createCollection
É uma ação aplicada a um recurso de database ou collection, que o usuário pode executar o método db.createCollection().

#### createIndex
É uma ação aplicada a um recurso de database ou collection, que o usuário pode acessar o método db.collection.createIndex()  e o comando createIndexes.

#### createRole
É uma ação aplicada a um recurso de database, que permite ao usuário criar novos papeis (``roles``) da database informada.

#### createUser
É uma ação aplicada a um recurso de database, que permite ao usuário criar novos usuários da database informada.

#### dropCollection
É uma ação aplicada a um recurso de database ou collection, que o usuário pode executar o método db.collection.drop().

#### dropRole
É uma ação aplicada a um recurso de database, que permite ao usuário excluir quaisquer papeis (``roles``) da database informada.

#### dropUser
É uma ação aplicada a um recurso de database, que permite ao usuário remover quaisquer usuários da database informada.

#### emptycapped
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário executar o comando emptycapped.

#### enableProfiler
É uma ação aplicada a um recurso de database, que permite ao usuário executar o método db.setProfilingLevel().

#### grantRole
É uma ação aplicada a um recurso de database, que permite ao usuário conceder quaisquer papéis (``roles``) na database á qualquer usuário de qualquer database no sistema.

#### killCursors
Permite ao usuário eliminar cursores na collection alvo.

#### revokeRole
É uma ação aplicada a um recurso de database, que permite ao usuário remover quaisque papéis (``roles``) de qualquer usuário de qualquer database no sistema.

#### unlock
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o método db.fsyncUnlock() .

#### viewRole
É uma ação aplicada a um recurso de database, que permite ao usuário visualizar informações sobre qualquer papel (``role``) da database informada.

#### viewUser
É uma ação aplicada a um recurso de database, que permite ao usuário visualizar informações sobre qualquer usuário da database informada.

### Ações de Gerenciamento de Implantação
#### authSchemaUpgrade
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o
comando authSchemaUpgrade.

#### cleanupOrphaned
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando cleanupOrphaned.

#### cpuProfiler
É uma ação aplicada a um recurso de cluster, que permite ao usuário ativar e utilizar o CPU Profiler.

#### inprog
É uma ação aplicada a um recurso de cluster, que permite ao usuário utilizar o método db.currentOp() para retornar operações pendentes a ativas.

#### invalidateUserCache
É uma ação aplicada a um recurso de cluster, que permite ao usuário acessar o comando invalidateUserCache.

#### killop
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o método  db.killOp() .

#### planCacheRead
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário executar os comandos planCacheListPlans e planCacheListQueryShapes e os métodos PlanCache.getPlansByQuery() e PlanCache.listQueryShapes().


#### planCacheWrite
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário executar o comando planCacheClear e os métodos PlanCache.clear() e PlanCache.clearPlansByQuery().

#### storageDetails
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário executar o comando storageDetails.

### Ações de Replicação
#### appendOplogNote
É uma ação aplicada a um recurso de cluster, que permite ao usuário adicionar notas ao oplog.

#### replSetConfigure
É uma ação aplicada a um recurso de cluster, que permite ao usuário configurar um conjunto de réplica (``replica set``).

#### replSetGetStatus
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando replSetGetStatus.

#### replSetHeartbeat
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando replSetHeartbeat.

#### replSetStateChange
É uma ação aplicada a um recurso de cluster, que permite ao usuário alterar o estado de um conjunto de réplicas (``replica set``)  através dos comandos replSetFreeze, replSetMaintenance, replSetStepDown, e replSetSyncFrom.

#### resync
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando resync.

### Ações de Sharding
#### addShard
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando addShard.

#### enableSharding
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário ativar ``sharding`` em uma database utilizando o comando enableSharding e pode realizar um ``shard`` em uma collection utilizando o comando shardCollection.

#### flushRouterConfig
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando flushRouterConfig.

#### getShardMap
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando getShardMap.

#### getShardVersion
É uma ação aplicada a um recurso de database, que permite ao usuário executar o comando getShardVersion.

#### listShards
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando listShards.

#### moveChunk
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário executar o moveChunk, além do comando movePrimary desde que o privilégio seja aplicado a um recurso de database apropriado.

#### removeShard
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando removeShard.

#### shardingState
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando shardingState.

#### splitChunk
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário executar o comando splitChunk.

#### splitVector
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário executar o comando splitVector.

### Ações de administração de servidor
#### applicationMessage
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando logApplicationMessage.

#### closeAllDatabases
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando closeAllDatabases.

#### collMod
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário executar o comando collMod.

#### compact
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário executar o comando compact.

#### connPoolSync
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando connPoolSync.

#### convertToCapped
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário executar o comando convertToCapped.

#### dropDatabase
É uma ação aplicada a um recurso de database, que permite ao usuário executar o comando dropDatabase.

#### dropIndex
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário executar o comando dropIndexes.

#### fsync
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando fsync.

#### getParameter
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando getParameter.

#### hostInfo
É uma ação aplicada a um recurso de cluster, que fornece informação sobre a instancia do MongoDb que está em execução no servidor.

#### logRotate
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando logRotate.

#### reIndex
É uma ação aplicada a um recurso de database ou collection, que permite ao usuário executar o comando reIndex.

#### renameCollectionSameDB
É uma ação aplicada a um recurso de database, que permite ao usuário renomear as coleções na database atual utilizando o comando renameCollection .

O usuário precisa ter find na coleção fonte e não ter find na coleção destino.
Se a coleção com o novo nome existir, o usuário deve ter também a ação dropCollection na coleção de destino.


#### repairDatabase
É uma ação aplicada a um recurso de database, que permite ao usuário executar o comando repairDatabase.

#### setParameter
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando setParameter.

#### shutdown
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando shutdown.

#### touch
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando touch.

### Ações de diagnóstico
#### collStats
É uma ação aplicada a um recurso de database ou coleção, que permite ao usuário executar o comando collStats.

#### connPoolStats
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar os comandos connPoolStats e shardConnPoolStats.

#### cursorInfo
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando cursorInfo.

#### dbHash
É uma ação aplicada a um recurso de database ou coleção, que permite ao usuário executar o comando dbHash.

#### dbStats
É uma ação aplicada a um recurso de database, que permite ao usuário executar o comando dbStats.

#### diagLogging
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando diagLogging.

#### getCmdLineOpts
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando getCmdLineOpts.

#### getLog
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando getLog.

#### indexStats
É uma ação aplicada a um recurso de database ou coleção, que permite ao usuário executar o comando indexStats. (indexStats existe até abaixo da versão 3.0 do MongoDB)

#### listDatabases
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando listDatabases.

#### listCollections
É uma ação aplicada a um recurso de database, que permite ao usuário executar o comando listCollections.

#### listIndexes
É uma ação aplicada a um recurso de database ou coleção, que permite ao usuário executar o comando ListIndexes.

#### netstat
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando netstat.

#### serverStatus
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando serverStatus.

#### validate
É uma ação aplicada a um recurso de database ou coleção, que permite ao usuário executar o comando validate.

#### top
É uma ação aplicada a um recurso de cluster, que permite ao usuário executar o comando top.


### Ações internas
#### anyAction
Permite qualquer ação em um recurso. Indicado somente em circunstancias excepcionais.

#### internal
Permite ações internas. Indicado somente em circunstancias excepcionais.

## Referências
#### Autenticação e Autorização:
[Microsoft Technet ](https://technet.microsoft.com/pt-br/library/cc759647%28v=ws.10%29.aspx)<br>
[Alexandremspmoraes - Autenticação Autorização e Accounting conceitos fundamentais ](https://alexandremspmoraes.wordpress.com/2013/02/15/autenticacao-autorizacao-e-accounting-conceitos-fundamentais/)<br>
[Imasters - Autenticação e Autorização em aplicativos web](http://imasters.com.br/artigo/14152/java/autenticacao-e-autorizacao-em-aplicativos-web)<br>
[Yiiframework - Guide](http://www.yiiframework.com/doc/guide/1.1/pt_br/topics.auth)<br>

#### Criar usuário admin e usuário comum:
[Mongodb Docs - Manage users and roles](https://docs.mongodb.org/manual/tutorial/manage-users-and-roles/)<br>
[Mongodb Docs - db.createUser](https://docs.mongodb.org/manual/reference/method/db.createUser/)<br>
[Mongodb Docs - add user administrator](https://docs.mongodb.org/v2.6/tutorial/add-user-administrator/)<br>
[Github - Gist](https://gist.github.com/tamoyal/10441108)<br>

#### Cluster Administration Roles:
[Mongodb Docs - Cluster administration roles](https://docs.mongodb.org/v2.6/reference/built-in-roles/#cluster-administration-roles)<br>

#### Privilege Actions:
[Mongodb Docs - Privilege actions](https://docs.mongodb.org/manual/reference/privilege-actions/)<br>
