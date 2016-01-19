# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Paulo Roberto<br> 
**Data:** 1452363975806
## Qual a diferença entre Autenticação e Autorização?
<b>Autenticação:</b> É o processo de verificação de um cliente(usuário do mongo). O mongo exige autenticação de todos clientes
(mongos), quando a autenticação está habilitada(--auth), autenticação e autorização estão interligadas porém são coisas 
diferentes.
<b>Autorização:</b> É a parte responsável por determina o acesso de um usuário já autenticado.


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

#### Administrador
Primeiramente vamos enteder, o que são usuários administradores. Basicamente no mongoDB há dois tipos de super-usuários, sendo
eles: <i>userAdmin</i> e <i>userAdminAnyDatabase</i>. Esses tipo de usuários devem ser os primeiros criados e sua <b>ÚNICA</b> função
é gerenciar outro usuários, sendo assim não podendo ler nem escrever dados sem adição de outras roles. Sobre a diferença entre
<i>userAdmin</i> e <i>userAdminAnyDatabase</i> é simples, como o próprio nome já diz userAdmin(Usuário administrador), ele
só pode gerenciar usuários da database do qual foi criado, já o userAdminAnyDatabase(Usuário administrador qualquer), pode
gerenciar usuários de qualquer database.

> Há apenas uma execeção quanto a isso que é: Um usuário que tem a role userAdmin, pertecente a database admin
> ele pode obter a role userAdminAnyDatabase portanto, só nesse caso essas roles significam a mesma coisa.

#### Criando um usuário <a target="_blank" style="{color:black}" href="https://docs.mongodb.org/v2.4/tutorial/add-user-administrator/"><i>administrador</i></a>:
```js
  db.createUser({ 
  user: "Paulo",
  pwd: "123",
  roles: [ "userAdminAnyDatabase" ] 
  });
  Successfully added user: {
    "user": "Paulo",
    "roles": [
      "userAdminAnyDatabase"
    ]
  }

  db.createUser({ 
    user: "Jerere",
    pwd: "321",
    roles: [ "userAdmin" ] 
  });
  Successfully added user: {
    "user": "Jerere",
    "roles": [
      "userAdmin"
    ]
  }
```

#### Comum
Não existe necessariamente um tipo de usuario comum no mongoDB, o que existe são <a href="https://docs.mongodb.org/manual/reference/built-in-roles/" target="_blank"><i>User Roles</i></a> e <a href="https://docs.mongodb.org/manual/reference/built-in-roles/" target="_blank"><i>Administration Roles</i></a>, 
vamos partir do ponto que um usuário comum é todo aquele que não possui nenhuma role contida em <i> Administration Roles</i>.

#### Criando usuário comum:
```js
  db.createUser({ 
    user: "PauloRoberto",
    pwd: "123",
    roles: [ "read" ] 
  });
  Successfully added user: {
      "user": "PauloRoberto",
      "roles": [
        "read"
      ]
    }
    
  db.createUser({ 
    user: "javascript",
    pwd: "123",
    roles: [ "readWrite" ] 
  });
  Successfully added user: {
    "user": "javascript",
    "roles": [
      "readWrite"
    ]
  }
```

## Explique cada papel listado em Cluster Administration Roles.
* <a target="_blank" href="https://docs.mongodb.org/manual/reference/built-in-roles/"><i>clusterAdmin:</i></a>
    Essa role forcene o maior acesso para o gerenciamento de cluster, combinando: <b>clusterManager</b>, <b>clusterMonitor</b> e <b>hostManager</b>.
    Também tem como privilégio a ação <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.dropDatabase" target="_blank">dropDatabase</a>.
    
* <a target="_blank" href="https://docs.mongodb.org/manual/reference/built-in-roles/"><i>clusterManager:</i></a>
    Essa role fornece privilégios de gerenciamento e monitoramento no cluster. Um usuário que possui essa role pode acessar
    a configuração e db's locais, que serão usados em sharding e réplicas respectivamente.<br>
    
    <b>Possui as seguintes ações de cluster:</b>
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.addShard" target="_blank"><i>addShard:</i></a> Cria uma nova shard.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.applicationMessage" target="_blank"><i>applicationMessage:</i></a> Escreve uma mensagem personalizada no audit log.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.cleanupOrphaned" target="_blank"><i>cleanupOrphaned:</i></a> Deleta documentos orfãos de um shard.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.flushRouterConfig" target="_blank"><i>flushRouterConfig:</i></a> Apaga as informações em cache do cluster atual de uma instância mongo.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.listShards" target="_blank"><i>listShards:</i></a> Retorna a lista de shards configurados.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.removeShard" target="_blank"><i>removeShard:</i></a> Remove uma shard de um cluster.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.replSetConfigure" target="_blank"><i>replSetConfigure:</i></a> Configura uma replicaSet.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.replSetGetStatus" target="_blank"><i>replSetGetStatus:</i></a> Retorna o status de uma replicaSet.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.replSetStateChange" target="_blank"><i>replSetStateChange:</i></a> Muda o status de uma réplica usando <a href="https://docs.mongodb.org/manual/reference/command/replSetFreeze/#dbcmd.replSetFreeze" target="_blank">replSetFreeze</a>, <a href="https://docs.mongodb.org/manual/reference/command/replSetMaintenance/#dbcmd.replSetMaintenance" target="_blank">replSetMaintenance</a>, <a href="https://docs.mongodb.org/manual/reference/command/replSetStepDown/#dbcmd.replSetStepDown" target="_blank">replSetStepDown</a>
      e <a href="https://docs.mongodb.org/manual/reference/command/replSetStepDown/#dbcmd.replSetStepDown" target="_blank">replSetStepDown</a>.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.resync" target="_blank"><i>resync:</i></a> Força uma saída de dados.
    
    <b>Possui as seguintes ações em todas database's do cluster:</b>
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.enableSharding" target="_blank"><i>enableSharding:</i></a> Habilita sharding em uma database usando <a href="https://docs.mongodb.org/manual/reference/command/enableSharding/#dbcmd.enableSharding" target="_blank">enableSharding</a>  e <a href="https://docs.mongodb.org/manual/reference/command/shardCollection/#dbcmd.shardCollection" target="_blank">shardCollection</a>.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.moveChunk" target="_blank"><i>moveChunk:</i></a> Move chunks entre shards.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.splitChunk" target="_blank"><i>splitChunk:</i></a> Comando alternativo para dividir chunks.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.splitVector" target="_blank"><i>splitVector:</i></a> Suporta operadores de meta dados nos shards de um cluster.
    
    <b>Possui as seguintes ações nas coleções na database config:</b>
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.insert" target="_blank"><i>insert:</i></a> Insere um ou mais documentos.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.remove" target="_blank"><i>remove:</i></a> Deleta um ou mais documentos.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.update" target="_blank"><i>update:</i></a> Altera um ou mais documentos.
    
    <b>Possui as seguintes ações em todas coleções de configuração na database config: </b>
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.collStats" target="_blank"><i>collStats:</i></a> Retorna estatísticas de armazenamento de uma determina coleção.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.dbHash" target="_blank"><i>dbHash:</i></a> Comando que suporta config servers.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.dbStats" target="_blank"><i>dbStats:</i></a> Retorna estatísticas de armazenamento de uma determinada database.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.find" target="_blank"><i>find:</i></a> Pode executar o find em uma coleção.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.killCursors" target="_blank"><i>killCursors:</i></a> "Mata" os cursores de uma coleção.
    
    <b>Possui as seguintes ações nas database locais na coleção replset: </b>
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.collStats" target="_blank"><i>collStats:</i></a> Retorna estatísticas de armazenamento de uma determina coleção.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.dbHash" target="_blank"><i>dbHash:</i></a> Comando que suporta config servers.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.dbStats" target="_blank"><i>dbStats:</i></a> Retorna estatísticas de armazenamento de uma determinada database.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.find" target="_blank"><i>find:</i></a> Pode executar o find em uma coleção.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.killCursors" target="_blank"><i>killCursors:</i></a> "Mata" os cursores de uma coleção.

* <a target="_blank" href="https://docs.mongodb.org/manual/reference/built-in-roles/"><i>clusterMonitor:</i></a> Tem acesso somente
de leitura para ferramentas de monitoramento, como: <a href="https://cloud.mongodb.com/?jmp=docs&_ga=1.202078296.855543045.1452279617" target="_blank">MongoDB Cloud Manager</a> e
<a href="https://docs.opsmanager.mongodb.com/current/?_ga=1.136075512.855543045.1452279617" target="_blank">Ops Manager</a>. <br>

  <b>Possui as seguintes ações de cluster:</b>
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.connPoolStats" target="_blank"><i>connPoolStats:</i></a> Usuário pode executar <a href="https://docs.mongodb.org/manual/reference/command/connPoolStats/#dbcmd.connPoolStats" target="_blank">connPoolStats</a> e <a href="https://docs.mongodb.org/manual/reference/command/shardConnPoolStats/#dbcmd.shardConnPoolStats" target="_blank">shardConnPoolStats</a>.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.cursorInfo" target="_blank"><i>cursorInfo:</i></a> Removido na versão 3.2 use serverStatus no lugar.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.getCmdLineOpts" target="_blank"><i>getCmdLineOpts:</i></a> Retorna um documento com as options usadas para levantar o mongod ou mongo.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.getLog" target="_blank"><i>getLog:</i></a> Retorna um documento com um array com o log recente do mongod.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.getParameter" target="_blank"><i>getParameter:</i></a> Recupera valore setado na linha de comando.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.getShardMap" target="_blank"><i>getShardMap:</i></a> Comando interno que suporta a função de sharding.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.hostInfo" target="_blank"><i>hostInfo:</i></a> Retorna informações sobre o servidor mongoDB que está rodando.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.inprog" target="_blank"><i>inprog:</i></a> Pode usar o <a href="https://docs.mongodb.org/manual/reference/method/db.currentOp/#db.currentOp" target="_blank">db.currentOp()</a> que retorna um documento com as operações em progresso da db.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.listDatabases" target="_blank"><i>listDatabases:</i></a> Retorna uma lista com as db's existentes.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.listShards" target="_blank"><i>listShards:</i></a> Retorna uma lista com os shard's configurados.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.netstat" target="_blank"><i>netstat:</i></a> Comando somente disponível em instâncias mongo.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.replSetGetStatus" target="_blank"><i>replSetGetStatus:</i></a> Retorna o status do conjunto de réplicas do ponto de vista do servidor atual.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.serverStatus" target="_blank"><i>serverStatus:</i></a> Retorna um documento com uma visão geral do estado de processo da db.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.shardingState" target="_blank"><i>shardingState:</i></a> Diz se o mongod faz parte de um cluster de shard.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.top" target="_blank"><i>top:</i></a> Retorna as estatísticas de uso para cada coleção.
  
  <b>Possui as seguintes ações em todas database's do cluster:</b>
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.collStats" target="_blank"><i>collStats:</i></a> Retorna estatísticas de armazenamento de uma determina coleção.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.dbStats" target="_blank"><i>dbStats:</i></a> Retorna estatísticas de armazenamento de uma determinada database.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.getShardVersion" target="_blank"><i>getShardVersion:</i></a> Comando que suporta a fucionalidade de sharding.
  
  <b>Possui find em todas coleções de system.profile do cluster.</b>
  
  <b>Possui as seguintes ações nas coleções de configuração na database config: </b>
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.collStats" target="_blank"><i>collStats:</i></a> Retorna estatísticas de armazenamento de uma determina coleção.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.dbHash" target="_blank"><i>dbHash:</i></a> Comando que suporta config servers.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.dbStats" target="_blank"><i>dbStats:</i></a> Retorna estatísticas de armazenamento de uma determinada database.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.find" target="_blank"><i>find:</i></a> Pode executar o find em uma coleção.
    * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.killCursors" target="_blank"><i>killCursors:</i></a> "Mata" os cursores de uma coleção.

* <a target="_blank" href="https://docs.mongodb.org/manual/reference/built-in-roles/"><i>hostManager:</i></a> Possui privilégios
para monitorar e gerenciar servidores.

  <b>Possui as seguintes ações de cluster:</b>
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.applicationMessage" target="_blank"><i>applicationMessage:</i></a> Escreve uma mensagem personalizada no audit log.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.closeAllDatabases" target="_blank"><i>closeAllDatabases:</i></a> Fecha todas db's do cluster.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.connPoolSync" target="_blank"><i>connPoolSync:</i></a> É um comando interno.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.cpuProfiler" target="_blank"><i>cpuProfiler:</i></a> Usuário pode habilitar e usar o perfil da CPU.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.diagLogging" target="_blank"><i>diagLogging:</i></a> Captura dados adcionais para fazer diagnóstico.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.flushRouterConfig" target="_blank"><i>flushRouterConfig:</i></a> Apaga as informações em cache do cluster atual de uma instância mongo.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.fsync" target="_blank"><i>fsync:</i></a> Força o mongod a gravar todas as escritas pedentes da storage layer em disco.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.invalidateUserCache" target="_blank"><i>invalidateUserCache:</i></a> Limpa o cache removendo todas as informações de usuário.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.killop" target="_blank"><i>killop:</i></a> Termina uma operação como especificado pelo operation ID. Para encontrar operações e seus ID's.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.logRotate" target="_blank"><i>logRotate:</i></a> Permiti uma rotação nos arquivos de log's, impedindo que um só arquivo consuma muito espaço.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.resync" target="_blank"><i>resync:</i></a> Força uma saída de dados.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.setParameter" target="_blank"><i>setParameter:</i></a> Modifica options setadas na linha de comando, deve ser usado na admin database.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.shutdown" target="_blank"><i>shutdown:</i></a> Limpa resources de todas db's e encerra o processo.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.touch" target="_blank"><i>touch:</i></a> Carrega dados da camada data stroge para memória.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.unlock" target="_blank"><i>unlock:</i></a> Executa <a href="https://docs.mongodb.org/manual/reference/method/db.fsyncUnlock/#db.fsyncUnlock" target="_blank">db.fsyncLock()</a> que pertime um mongod escrever e revereter operações de um <a href="https://docs.mongodb.org/manual/reference/method/db.fsyncUnlock/#db.fsyncUnlock" target="_blank">db.fsyncLock()</a>.
  
  <b>Possui as seguintes ações em todas database's do cluster:</b>
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.killCursors" target="_blank"><i>killCursors:</i></a> "Mata" os cursores de uma coleção.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.repairDatabase" target="_blank"><i>repairDatabase:</i></a> Checa e repara erros e inconsistências na data storage.
  
## Explique todas as ações de privilégio listadas em Privilege Actions.
 * <b>Ações de Query e Write(escrita)</b><br>
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.collection.find/#db.collection.find" target="_blank"><i>find:</i></a> Encontra um ou mais documentos em uma coleção.
   * <a href="https://docs.mongodb.org/v3.0/reference/command/insert/#dbcmd.insert" target="_blank"><i>insert:</i></a> Insere um ou mais documentos em uma coleção.
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.collection.remove/#db.collection.remove" target="_blank"><i>remove:</i></a> Remove um ou mais documentos em uma coleção.
   * <a href="https://docs.mongodb.org/v3.0/reference/command/update/#dbcmd.update" target="_blank"><i>update:</i></a> Altera um ou mais documentos em uma coleção.
 
 * <b>Ações de gerenciamento de database</b><br>
   * <a href="https://docs.mongodb.org/manual/reference/method/db.updateUser/" target="_blank"><i>changeCustomData:</i></a> Pode mudar o custom data de qualquer usuário.
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.createCollection/#db.createCollection" target="_blank"><i>changeOwnCustomData:</i></a> Usuário pode mudar o seu próprio custom data.
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.createCollection/#db.createCollection" target="_blank"><i>changeOwnPassword:</i></a> Usuário pode mudar a sua própria senha.
   * <a href="https://docs.mongodb.org/manual/reference/method/db.updateUser/" target="_blank"><i>changePassword:</i></a> Usuário pode mudar a senha de qualquer usuário.
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.createCollection/#db.createCollection" target="_blank"><i>createCollection:</i></a> Cria uma nova coleção.
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.collection.createIndex/#db.collection.createIndex" target="_blank"><i>createIndex:</i></a> Cria indexes em coleções.
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.createRole/" target="_blank"><i>createRole:</i></a> Usuário pode criar roles nas db's.
   * <a href="https://docs.mongodb.org/manual/reference/method/db.createUser/" target="_blank"><i>createUser:</i></a> Usuário pode crias novos usuários na database.
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.collection.drop/#db.collection.drop" target="_blank"><i>dropCollection:</i></a> Usuário pode apagar uma coleção.
   * <a href="https://docs.mongodb.org/v3.0/reference/command/dropRole/#dbcmd.dropRole" target="_blank"><i>dropRole:</i></a> Usuário pode apagar qualquer role da database.
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.dropUser/" target="_blank"><i>dropUser:</i></a> Usuário pode apagar qualquer usuário da database.
   * <a href="https://docs.mongodb.org/v3.0/reference/command/emptycapped/#dbcmd.emptycapped" target="_blank"><i>emptycapped:</i></a> Remove todos documentos de uma coleção capped.
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.setProfilingLevel/#db.setProfilingLevel" target="_blank"><i>enableProfiler:</i></a> Modifica o nível de perfil do banco de dados atual usando db.setProfilingLevel().
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.grantRolesToUser/" target="_blank"><i>grantRole:</i></a> Usuário pode dar qualquer role para qualquer usuário em todas db's do sistema.
   * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.killCursors" target="_blank"><i>killCursors:</i></a> "Mata" os cursores de uma coleção.
   * <a href="https://docs.mongodb.org/v3.0/reference/command/revokeRolesFromUser/" target="_blank"><i>revokeRole:</i></a> Usuário pode remover qualquer role de qualquer usuário em todas db's do sistema.
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.fsyncUnlock/#db.fsyncUnlock" target="_blank"><i>unlock:</i></a> Executa db.fsyncLock() que pertime um mongod escrever e revereter operações de um db.fsyncLock().
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.getRole/" target="_blank"><i>viewRole:</i></a> Usuário pode ver as informações de qualquer role do banco de dados.
   * <a href="https://docs.mongodb.org/v3.0/reference/method/db.getUser/" target="_blank"><i>viewUser:</i></a> Usuário pode obter as informações de qualquer usuário do banco de dados.
  
* <b>Ações de gerenciamento de deploy</b><br>
  * <a href="https://docs.mongodb.org/v3.0/reference/command/authSchemaUpgrade/#dbcmd.authSchemaUpgrade" target="_blank"><i>authSchemaUpgrade:</i></a> Dar suporte ao processo de upgrade para os sistemas que usam autenticação e autorização.
  * <a href="https://docs.mongodb.org/v3.0/reference/command/cleanupOrphaned/#dbcmd.cleanupOrphaned" target="_blank"><i>cleanupOrphaned:</i></a> Deleta documentos orfãos de um shard.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.cpuProfiler" target="_blank"><i>cpuProfiler:</i></a> Usuário pode habilitar e usar o perfil da CPU.
  * <a href="https://docs.mongodb.org/v3.0/reference/method/db.currentOp/#db.currentOp" target="_blank"><i>inprog:</i></a>  Pode usar o <a href="https://docs.mongodb.org/manual/reference/method/db.currentOp/#db.currentOp" target="_blank">db.currentOp()</a> que retorna um documento com as operações em progresso da db.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.invalidateUserCache" target="_blank"><i>invalidateUserCache:</i></a> Limpa o cache removendo todas as informações de usuário.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.killop" target="_blank"><i>killop:</i></a> Termina uma operação como especificado pelo operation ID. Para encontrar operações e seus ID's.
  * <i>planCacheRead:</i> Usuário pode executar os comandos <a href="https://docs.mongodb.org/v3.0/reference/command/planCacheListPlans/#dbcmd.planCacheListPlans" target="_blank">planCacheListPlans</a> e <a href="https://docs.mongodb.org/v3.0/reference/command/planCacheListQueryShapes/#dbcmd.planCacheListQueryShapes" target="_blank">planCacheListQueryShapes</a> e os métodos <a href="https://docs.mongodb.org/v3.0/reference/method/PlanCache.getPlansByQuery/#PlanCache.getPlansByQuery" target="_blank">PlanCache.getPlansByQuery()</a> e <a href="https://docs.mongodb.org/v3.0/reference/method/PlanCache.listQueryShapes/#PlanCache.listQueryShapes" target="_blank">PlanCache.listQueryShapes()</a>.
  * <i>planCacheWrite:</i> Usuário pode executar o comando <a href="https://docs.mongodb.org/v3.0/reference/command/planCacheClear/#dbcmd.planCacheClear" target="_blank">planCacheClear</a> e os métodos <a href="https://docs.mongodb.org/v3.0/reference/method/PlanCache.clear/#PlanCache.clear" target="_blank">PlanCache.clear()</a> e <a href="https://docs.mongodb.org/v3.0/reference/method/PlanCache.clearPlansByQuery/#PlanCache.clearPlansByQuery" target="_blank">PlanCache.clearPlansByQuery()</a>.
  * <i>storageDetails:</i> Usuário pode executar o comando <b>storageDetails</b>.
  
* <b>Ações de réplica</b><br>
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.appendOplogNote" target="_blank"><i>appendOplogNote:</i></a> Usuário pode acrescentar notas no oplog.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.replSetConfigure" target="_blank"><i>replSetConfigure:</i></a> Configura uma replicaSet.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.replSetGetStatus" target="_blank"><i>replSetGetStatus:</i></a> Retorna o status de uma replicaSet.
  * <a href="https://docs.mongodb.org/v3.0/reference/command/replSetHeartbeat/#dbcmd.replSetHeartbeat" target="_blank"><i>replSetHeartbeat:</i></a> Comando interno que suporta uma função de replica set.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.replSetStateChange" target="_blank"><i>replSetStateChange:</i></a> Muda o status de uma réplica usando <a href="https://docs.mongodb.org/manual/reference/command/replSetFreeze/#dbcmd.replSetFreeze" target="_blank">replSetFreeze</a>, <a href="https://docs.mongodb.org/manual/reference/command/replSetMaintenance/#dbcmd.replSetMaintenance" target="_blank">replSetMaintenance</a>, <a href="https://docs.mongodb.org/manual/reference/command/replSetStepDown/#dbcmd.replSetStepDown" target="_blank">replSetStepDown</a>
 e <a href="https://docs.mongodb.org/manual/reference/command/replSetStepDown/#dbcmd.replSetStepDown" target="_blank">replSetStepDown</a>.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.resync" target="_blank"><i>resync:</i></a> Força uma saída de dados.
 
* <b>Ações de sharding</b><br>
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.addShard" target="_blank"><i>addShard:</i></a> Cria uma nova shard.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.enableSharding" target="_blank"><i>enableSharding:</i></a> Habilita sharding em uma database usando <a href="https://docs.mongodb.org/manual/reference/command/enableSharding/#dbcmd.enableSharding" target="_blank">enableSharding</a>  e <a href="https://docs.mongodb.org/manual/reference/command/shardCollection/#dbcmd.shardCollection" target="_blank">shardCollection</a>.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.flushRouterConfig" target="_blank"><i>flushRouterConfig:</i></a> Apaga as informações em cache do cluster atual de uma instância mongo.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.getShardMap" target="_blank"><i>getShardMap:</i></a> Comando interno que suporta a função de sharding.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.getShardVersion" target="_blank"><i>getShardVersion:</i></a> Comando que suporta a fucionalidade de sharding.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.listShards" target="_blank"><i>listShards:</i></a> Retorna a lista de shards configurados.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.moveChunk" target="_blank"><i>moveChunk:</i></a> Move chunks entre shards.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.removeShard" target="_blank"><i>removeShard:</i></a> Remove uma shard de um cluster.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.shardingState" target="_blank"><i>shardingState:</i></a> Diz se o mongod faz parte de um cluster de shard.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.splitChunk" target="_blank"><i>splitChunk:</i></a> Comando alternativo para dividir chunks.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.splitVector" target="_blank"><i>splitVector:</i></a> Suporta operadores de meta dados nos shards de um cluster.
  
* <b>Ações de administração de servidor</b><br>
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.applicationMessage" target="_blank"><i>applicationMessage:</i></a> Escreve uma mensagem personalizada no audit log.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.closeAllDatabases" target="_blank"><i>closeAllDatabases:</i></a> Fecha todas db's do cluster.
  * <a href="" target="_blank"><i>collMod:</i></a> Possibilita o usuário a adcionar flags na coleção para modificar o comportamento do mongoDB.
  * <a href="https://docs.mongodb.org/manual/reference/command/compact/#dbcmd.compact" target="_blank"><i>Compact:</i></a> Usuário pode reescrever e desfragmenta todos dados e indexes de uma coleção.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.connPoolSync" target="_blank"><i>connPoolSync:</i></a> É um comando interno.
  * <a href="https://docs.mongodb.org/manual/reference/command/convertToCapped/#dbcmd.convertToCapped" target="_blank"><i>convertToCapped:</i></a> Usuário pode transforma uma coleção não capped em uma capped dentro da mesma db.
  * <a href="https://docs.mongodb.org/manual/reference/command/dropDatabase/#dbcmd.dropDatabase" target="_blank"><i>dropDatabase:</i></a>Usuário pode apagar  a database atual junto com todos dados associados a ela.
  * <a href="https://docs.mongodb.org/manual/reference/command/dropIndexes/#dbcmd.dropIndexes" target="_blank"><i>dropIndex:</i></a> Usuário pode apagar um ou todos índices da coleção atual.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.fsync" target="_blank"><i>fsync:</i></a> Força o mongod a gravar todas as escritas pedentes da storage layer em disco.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.getParameter" target="_blank"><i>getParameter:</i></a> Recupera valore setado na linha de comando.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.hostInfo" target="_blank"><i>hostInfo:</i></a> Retorna informações sobre o servidor mongoDB que está rodando.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.logRotate" target="_blank"><i>logRotate:</i></a> Permiti uma rotação nos arquivos de log's, impedindo que um só arquivo consuma muito espaço.
  * <a href="https://docs.mongodb.org/manual/reference/command/reIndex/#dbcmd.reIndex" target="_blank"><i>reIndex:</i></a> Apaga todos índices de uma coleção e recria eles.
  * <a href="https://docs.mongodb.org/manual/reference/command/renameCollection/#dbcmd.renameCollection" target="_blank"><i>renameCollectionSameDB:</i></a> Altera o nome de uma coleção existente. O usuário deve ter find na coleção de origem ou não ter find na coleção de destino, se uma coleção com o novo nome existe ele deve ter também o dropCollection na coleção de destino.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.repairDatabase" target="_blank"><i>repairDatabase:</i></a> Checa e repara erros e inconsistências na data storage.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.setParameter" target="_blank"><i>setParameter:</i></a> Modifica options setadas na linha de comando, deve ser usado na admin database.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.shutdown" target="_blank"><i>shutdown:</i></a> Limpa resources de todas db's e encerra o processo.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.touch" target="_blank"><i>touch:</i></a> Carrega dados da camada data stroge para memória.
  
* <b>Ações de diagnóstico</b><br>
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.collStats" target="_blank"><i>collStats:</i></a> Retorna estatísticas de armazenamento de uma determina coleção.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.connPoolStats" target="_blank"><i>connPoolStats:</i></a> Usuário pode executar <a href="https://docs.mongodb.org/manual/reference/command/connPoolStats/#dbcmd.connPoolStats" target="_blank">connPoolStats</a> e <a href="https://docs.mongodb.org/manual/reference/command/shardConnPoolStats/#dbcmd.shardConnPoolStats" target="_blank">shardConnPoolStats</a>.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.cursorInfo" target="_blank"><i>cursorInfo:</i></a> Removido na versão 3.2 use serverStatus no lugar.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.dbHash" target="_blank"><i>dbHash:</i></a> Comando que suporta config servers.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.dbStats" target="_blank"><i>dbStats:</i></a> Retorna estatísticas de armazenamento de uma determinada database.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.diagLogging" target="_blank"><i>diagLogging:</i></a> Captura dados adcionais para fazer diagnóstico.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.getCmdLineOpts" target="_blank"><i>getCmdLineOpts:</i></a> Retorna um documento com as options usadas para levantar o mongod ou mongo.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.getLog" target="_blank"><i>getLog:</i></a> Retorna um documento com um array com o log recente do mongod.
  * <a href="https://docs.mongodb.org/v2.6/reference/command/indexStats/" target="_blank"><i>indexStats:</i></a> Usuário pode executar comando indexStats(<b>Comando removido na versão 3.0</b>).
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.listDatabases" target="_blank"><i>listDatabases:</i></a> Retorna uma lista com as db's existentes.
  * <a href="https://docs.mongodb.org/manual/reference/command/listCollections/#dbcmd.listCollections" target="_blank"><i>listCollections:</i></a> Usuário pode recuperar informações(nome, opções) sobre as coleções em uma database.
  * <a href="https://docs.mongodb.org/v3.0/reference/command/listIndexes/" target="_blank"><i>listIndexes:</i></a> Usuário pode pega informações sobre os índices de uma determina coleção.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.netstat" target="_blank"><i>netstat:</i></a> Comando somente disponível em instâncias mongo.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.serverStatus" target="_blank"><i>serverStatus:</i></a> Retorna um documento com uma visão geral do estado de processo da db.
  * <a href="https://docs.mongodb.org/manual/reference/command/validate/#dbcmd.validate" target="_blank"><i>validate:</i></a> Verifica a estrutura dentro de um namespace para correção escaneando os dados das coleções e índices.
  * <a href="https://docs.mongodb.org/manual/reference/privilege-actions/#authr.top" target="_blank"><i>top:</i></a> Retorna as estatísticas de uso para cada coleção.
  
* <b>Ações internas</b><br>
  * <i>anyAction:</i> Permiti qualquer ação em um resource<b>(Não atribua essa ação, somente em exeções)</b>.
  * <i>internal:</i> Permiti ações internas<b>(Não atribua essa ação, somente em exeções)</b>.
