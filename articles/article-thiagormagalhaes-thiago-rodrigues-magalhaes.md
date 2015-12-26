# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Thiago Rodrigues Magalhães
**Data** 1451095932195

## Qual a diferença entre Autenticação e Autorização?
A autenticação e a autorização são procedimentos utilizados para garantir, ou pelo menos, tentar garantir a segurança de alguma aplicação.
Autenticação se refere ao procedimento que confirma a validade do usuário que tenta acessar algum serviço, em outras palavras, ele tenta confirma se você é você mesmo. Esse procedimento é realizado baseando-se na apresentação de alguma identidade com uma ou mais credenciais, a identidade com a credencial forma uma identidade única usada como forma de distinguir e de reconhecer um usuário. Exemplo bem comum é o email/user e a senha.
A autorização é um procedimento que ocorre depois da autenticação, a autorização é que garante ao usuário autenticado o acesso ao serviço e a funções que são concedidas a ele, funções essas que variam de acordo com o privilégio atribuído ao usuário.


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
Para criar usuário administrador e comum, você deve estar com o ```Server``` levantado (é lógico) e executar os comando no ```Client```. Caso você tenha um cluster, execute na ```Client``` da replica primaria.

###Administrador
Para criar um usuário administrador basta executar...

```js
use database
db.addUser({
  user: "admin",
  pwd: "admin123"",
  roles: [ "userAdminAnyDatabase", "readAnyDatabase", "clusterAdmin" ]
})
```

####Entendeu? Não entendeu? Vamos explicar.

Primeiro nos conectamos a database em que desejamos criar os usuários. Em seguida executamos o comando ```db.addUser()``` responsavel por adicionar um novo usuário ao nosso banco, passando um JSON para ele com informações padrões como o ```user``` que é o nome do usuário, ```pwd``` que é o nosso password e as ```roles``` que é uma array que recebe os papéis, os privilégios, do usuário que estamos criando. No caso dos privilégios consedemos: ```userAdminAnyDatabase``` (a role que torna nosso usuário admin), ```readAnyDatabase``` e ```clusterAdmin```. A função de cada um desses privilégios você verá mais na frente.

###Comum
```js
use database
db.addUser({
  user: "comum",
  pwd: "user123",
  roles: [ "readWrite", "dbAdmin" ]
})
```

O comando para criar um usuário comum é a mesma lógica do administrador, a diferença serão as roles, ou seja, as permissões concedidas.


## Explique cada papel listado em Cluster Administration Roles.

###clusterAdmin
Essa função fornece o maior acesso de gerencialmento de cluster, combinando os privilégios que são concedido pelas roles: ```clusterManager```, ```clusterMonitor```, ```hostManager```. Essa função também permite que o usuário delete a database.

###clusterManager
Essa role permite o gerenciamento e monitoramento do cluster, podendo acessar configurações de databases locais, que são usadas no ```sharding``` e na ```replicação```.

Usuários com essa função dispõem das seguintes funções no cluster:
```
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
```

Para a database no cluster pode ser usada as seguintes funções:
```
    enableSharding
    moveChunk
    splitChunk
    splitVector
```

Na collection o usuário pode fazer as seguintes ações:
```
    insert
    remove
    update
```

As seguintes funções são fornecidas a todas as collections e a ```system.indexes```, ```system.js``` e ```system.collections```.
```
    collStats
    dbHash
    dbStats
    find
    killCursors
```

Na database ```local```, as seguintes ações são fornecidas na coleção ReplSet
```
    collStats
    dbHash
    dbStats
    find
    killCursors
```


###clusterMonitor
Essa role fornece apenas a ação de leitura para ferramentas de monitoramento.

Para o cluster podem ser usadas as funções:
```
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
```

E fornece para todos as databases do cluster, as funções:
```
    collStats
    dbStats
    getShardVersion
```

É fornecida a ação de ```find``` para todas as colections ```system.profile``` do cluster.

Também são fornecidas funções para a configuração das collections da database e das collections ```system.indexes```, ```system.js``` e ```system.namespaces```:
```
    collStats
    dbHash
    dbStats
    find
    killCursors
```


###hostManager
Com está role o usuário terá a capacidade de monitorar e gerenciar servidores.

Para o cluster, são fornecidas as seguintes ações:
```
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
```

E permite as seguintes ações em todas as databases no cluster:
```
    killCursors
    repairDatabase
```

## Explique todas as ações de privilégio listadas em Privilege Actions.

###Query e ações de escrita
Comandos voltados para a ```database```.

####find
Comando utilizado para fazer busca em collections.

####insert
Comando usado para fazer a inserção de documentos nas collections.

####remove
Comando usado para fazer a remoção de documentos.

####update
Comando utilizado para fazer atualização dos documentos.

###Ações de gestão de database
Comandos voltados para a ```database```.

####changeCustomData
O usuário pode alterar as informações personalizadas de qualquer usuário da database fornecida.

####changeOwnCustomData
Os usuários podem alterar suas próprias informações personalizadas.

####changeOwnPassword
Permite que o usuário altere sua própria senha.

####changePassoword
Permite que o usuário altere a senha de qualquer usuário especificado na database

####createCollection
O usuário pode executar o método de criação de collections.

####createIndex
Concede ao usuário o método de criação de índices.

####createRole
Permite o usuário criar novas ```roles``` em uma database especificada.

####createUser
O usuário pode criar um novo usuário para a database especificada.

####dropCollection
O usuário pode executar o método que apaga a collection, assim como seu dados.

####dropRole
O usuário pode deletar qualquer ```role``` de uma database especificada.

####dropUser
O usuário pode deletar usuários de uma database especificada.

####emptycapped
O usuário pode executar o comando emptycapped.

####enableProfiler
O usuário pode executar o método db.setProfilingLevel(), usado para alterar o nível do usuário na database.

####grantRole
O usuário pode concender qualquer ```role``` na database para qualquer usuário de database no sistema.

####killCursors
O usuário pode matar cursores de uma collection selecionada.

####revokeRole
O usuário pode remover qualquer papel de qualquer usuário a partir de qualquer banco de dados no sistema.

####unlock
Permite o uso do método ```db.fsyncUnlock()```. Esse método é utilizado no recurso de ```cluster```.

####viewRole
Permite o usuário ver informações sobre qualquer ```role``` de uma database especifica.

####viewUser
Permite o usuário ver informações sobre qualquer usuário de uma database especifica.

###Ações de gerenciamento de implantação
Comandos voltados para o recurso de ```cluster```.

####authSchemaUpgrade
O usuário pode executar o comando ```authSchemaUpgrade```.

####cleanupOrphaned
O usuário pode executar o comando ```cleanupOrphaned```.

####cpuProfiler
O usuário pode ativar e usar o profiler da CPU.

####inprog
O usuário pode usar o método ```db.currentOp()``` para retornar operações pendentes e ativos.

####invalidateUserCache
Permite acesso ao comando ```invalidateUserCache```.

####killop
Permite acesso ao método ``` db.killOp()```.

####planCacheRead
O usuário pode executar os comandos ```planCacheListPlans``` e ```planCacheListQueryShapes```. Também permite a execução dos métodos ```PlanCache.getPlansByQuery()``` e ```PlanCache.listQueryShapes()```. Essas ações são aplicadas nas databases e collections.

####planCacheWrite
Permite o uso do comando ```planCacheClear``` e métodos ```PlanCache.clear()``` e ```PlanCache.clearPlansByQuery()```. Ações voltadas para databases e collections.

####storageDetails
O usuário pode executar o comando ```storageDetails```. Comando para a database e collection.

###Ações de replicação
Ações voltadas para o ```cluster```.

####appendOplogNote
O usuário pode acrescentar notas ao oplog.

####replSetConfigure
O usuário pode configurar um conjunto de ```ReplicaSet```.

####replSetGetStatus
O usuário pode executar o comando ```replSetGetStatus```.

####replSetHeartbeat
O usuário pode executar o comando ```replSetHeartbeat```.

####replSetStateChange
O usuário pode mudar o estado da ```ReplicaSet``` atrávez dos comandos: ```replSetFreeze```, ```replSetMaintenance```, ```replSetStepDown``` e ```replSetSyncFrom```.

####resync
O usuário pode executar o comando ```resync```.

###Ações de Sharding
A maioria dos funções e comandos são voltadas para o ```cluster```.

####addShard
O usuário pode executar o comando ```addShard```.

####enableSharding
O usuário pode habilitar o comando de sharding na database utilizando o comando ```enableSharding``` e sharding para collections usando o comando ```shardCollection```. Comando aplicados na database e collection.

####flushRouterConfig
O usuário pode executar o comando ```flushRouterConfig```.

####getShardMap
O usuário pode executar o comando ```getShardMap```.

####getShardVersion
O usuário pode executar o comando ```getShardVersion```. Ação aplicada na database.

####listShards
O usuário pode executar o comando ```listShards```.

####moveChunk
O usuário pode executar o comando ```moveChunk```. Além de poder executar o comando ```movePrimary``` desde que o privilégio seja aplicado a uma database apropriada. Ação aplicada em databases e collections

####removeShard
O usuário pode executar o comando ```removeShard```.

####shardingState
O usuário pode executar o comando ```shardingState```.

####splitChunk
O usuário pode executar o comando ```splitChunk```. Ação aplicada em databases e collections.

####splitVector
O usuário pode executar o comando ```splitVector```. Ação aplicada em databases e collections.

###Ações de Administração de Servidor

####applicationMessage
O usuário pode executar o comando ```logApplicationMessage```, ação aplicada no ```cluster```.

####closeAllDatabases
O usuário pode executar o comando ```closeAllDatabases```, ação realizada no ```cluster```.

####collMod
O usuário pode executar o comando ```collMod```, em uma database ou collection.

####compact
O usuário pode executar o comando ```compact```, aplicado na database ou collection.

####connPoolSync
O usuário pode executar o comando ```connPoolSync```, voltado para o ```cluster```.

####convertToCapped
O usuário pode executar o comando ```convertToCapped```, ação voltada para collection e database.

####dropDatabase
O usuário pode executar o comando ```dropDatabase```, ação que afeta a database.

####dropIndex
O usuário pode executar o comando ```dropIndex```, ação aplicada na database ou collection.

####fsync
O usuário pode executar o comando ```fsync```, ação aplicada ao ```cluster```.

####getParameter
O usuário pode executar o comando ```getParameter```, ação aplicada ao ```cluster```.

####hostInfo
Prove informações sobre o servidor instanciado do MongoDB que esteja executando, ação aplicada ao ```cluster```.

####logRotate
O usuário pode executar o comando ```logRotate```, ação aplicada ao ```cluster```.

####reIndex
O usuário pode executar o comando ```reIndex```, ação aplicada a database ou collection.

####renameCollectionSameDB
Permite o usuário renomear collections que estejam ativas na database usando o comando ```renameCollection```.

####repairDatabase
O usuário pode executar o comando ```repairDatabase```. Ação voltada para a database.

####setParameter
O usuário pode executar o comando ```setParameter```. Ação aplicada ao ```cluster```.

####shutdown
O usuário pode executar o comando ```shutdown```. Ação aplicada ao ```cluster```.

####touch
O usuário pode executar o comando ```touch```. Ação aplicada ao ```cluster```.

###Ações de diagnóstico

####collStats
O usuário pode executar o comando ```collStats```. Ação aplicada em database ou collection.

####connPoolStats
O usuário pode executar os comandos ```connPoolStats``` e ```shardConnPoolStats```. Ação aplicada ao ```cluster```.

####cursorInfo
O usuário pode executar o comando ```cursorInfo```, aplicado no ```cluster```.

####dbHash
O usuário pode executar o ```dbHash```, na database ou collection.

####dbStats
O usuário pode executar o comando ```dbStats``` na database.

####diagLogging
O comando ```diagLogging``` é concedido ao usuário e executado no ```cluster```.

####getCmdLineOpts
O comando ```getCmdLineOpts``` é concedido ao usuário e executado no ```cluster```.

####getLog
O usuário pode executar o comando ```getLog```, aplicado ao ```cluster```.

####indexStats
O usuário pode executar o comando ```indexStats```, aplicado ao ```cluster```.

####listDatabases
O usuário pode executar o comando ```listDatabases```, aplicado ao ```cluster```.

####listCollections
O usuário pode executar o comando ```listCollections```, aplicado a database.

####listIndexes
O usuário pode executar o comando ```listIndexes```. Ação aplicada a database ou collection.

####netstat
O usuário pode executar o comando ```netstat```, aplicado ao ```cluster```.

####serverStatus
O usuário pode executar o comando ```serverStatus```, aplicado ao ```cluster```.

####validate
O usuário pode executar o comando ```validate```. Ação aplicada a database ou collection.

####top
O usuário pode executar o comando ``` top¶```, aplicado ao ```cluster```.

###Ações internas

####anyAction
Permite qualquer ação em um recurso. Não atribua essa ação, exceto em circunstâncias excepcionais.

####internal
Permite ações internas. Não atribua essa ação, exceto em circunstâncias excepcionais.