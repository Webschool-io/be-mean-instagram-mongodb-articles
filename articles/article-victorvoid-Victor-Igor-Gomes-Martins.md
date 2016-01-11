# MongoDb - Artigo sobre Autenticação no MongoDb

**Autor:** Victor Igor Gomes Martins 

**Data:** 1452452512382

## Qual a diferença entre Autenticação e Autorização?
	
Isso é bastante simples, sabe quando você coloca senha no seu computador, e ao iniciar ele pede o login e senha ? Então, ao está logando, você está autenticando, e autorização é esse processo de verificação, caso esteja errado você não está autorizado para entrar. 

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
Primeiro de tudo, como cria um usuário no MongoDB ? Pra isso usamos uma função chamada 'createUser', e nela contém os seguintes campos: 

{
    user : "nome-do-usuario",
    pwd  : "sua-senha",
    roles: [{role : "role", db: "o-banco"}] 
}

###Usuário admin

```js
db.createUser(
    {
        user: "userAdmin",
        pwd: "admin123",
        roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
    }
)
Successfully added user: {
  "user": "userAdmin",
  "roles": [
    {
      "role": "userAdminAnyDatabase",
      "db": "admin"
    }
  ]
}
``` 
###Usuário comum
```js
db.createUser(
    {
        user: "userTest",
        pwd: "123",
        roles: [ "read" ]
    }
)
Successfully added user: {
  "user": "userTest",
  "roles": [
    "read"
  ]
}
``` 
## Explique cada papel listado em Cluster Administration Roles.

**clusterAdmin**: Ele sim é poderoso, possuindo o maior acesso para gerenciamento do Cluster. É a combinação dos privelégio dos que veremos a baixo, `clusterManager`, `clusterMonitor`, e `hostManager`. Além disso, ainda tem o poder de usar a função `dropDatabase` para apagar tudo. 

**clusterManager**: Pode realizar o gerneciamento e monitoração das ações no Cluster. O cluster manager também tem acesso ao config, e os bancos locais, que são usados respectivamente para `sharding` e `replication`.

**clusterMonitor**: Pode realizar apenas a leitura (read-only) nas ferramentas de monitoração, como o MongoDB Cloud Manager e Ops Manager monitoring agent.

**hostManager**: Possui a abilidade de monitorar e gerenciar os servidores.

## Explique todas as ações de privilégio listadas em Privilege Actions.

### Query and Write Actions
**find**: Usuário pode executar o método `db.collection.find()`. Essa ação é aplicada para database ou collection.

**insert**:  Usuário pode executar comandos de `Insert`. Essa ação é aplicada para Database ou collection.

**remove**: usuário pode executar o método `db.collection.remove()`.  Essa ação é aplicada para database ou collection.

**update**: usuário pode executar comandos de Update.  Essa ação é aplicada para Database ou collection.

**bypassDocumentValidation**: Usuário pode ignorar validações em ações que suportam o *bypassDocumentValidation*. Essa ação é aplicada para Database ou collection.

### Database Management Actions

**changeCustomData**: Usuário poderá alterar informações do Custom de qualquer usuário na database. Essa ação é aplicada para Database ou collection.

**changeOwnPassword**: Usuário pode altear suas próprias informações. Essa ação é aplicada para Database ou collection.

**changepassword**: Usuário poderá alterar a senha de qualquer usuário na database. Essa ação é aplicada para Database ou collection.

**createCollection**: Usuário poderá executar o método `db.createCollection()`. Essa ação é aplicada para Database ou collection.

**createIndex**: Prove acesso ao método `db.collection.createindex()` e ao comando createIndexes. Essa ação é aplicada para Database ou collection.

**createRole**: Usuário poderá cirar uma Role na database. Essa ação é aplicada para Database ou collection.

**createuser**: Usuário pode criar novos usuários na database. Essa ação é aplicada para Database ou collection.

**dropCollection**: Usuário pode executar o comando `db.collection.drop()`. Essa ação é aplicada para Database ou collection.

**dropRole**: Usuário pode deletar qualquer papel de uma database. Essa ação é aplicada para Database ou collection.

**dropUser**: Usuário pode remover qualquer usuário de uma database. Essa ação é aplicada para Database ou collection.

**emptycapped**: Usuário pode executar o comando emptycapped. Essa ação é aplicada para Database ou collection.

**enableProfile**: Usuário pode executar o método `db.setProfilingLevel()`. Essa ação é aplicada para Database.

**grantRole**: Usuário pode executar o método `db.setProfilingLevel()`. Essa ação é aplicada para Database.

**killCursors**: Usuário pode matar qualquer cursor na collection.

**revokeRole**: Usuário pode remover qualquer papel de qualquer usuário em qualquer database. Aplica esta ação para Database.

**unlock**: Usuário pode executar o método `db.fsyncUnlock()`. Aplica esta ação em Cluster.

**viewRole**: Usuário pode ver qualquer informação de qualquer papel na database. Aplica esta ação para Database.

**viewUser**: Usuário poderá ver qualquer informação de qualquer usuário em uma database. Aplica esta ação para Database.

### Deployment Management Actions

**authSchemaUpgrade**: Usuário pode executar o comando authSchemaUpgrade.  Esta ação é aplicada em cluster.

**cpuProfile**: Usuário pode habilitar e usar o perfil CPU.  Esta ação é aplicada em cluster.

**inprog**: Usuário pode usar o metodo `db.currentOp()` para retornar ações ativas e pendentes. Esta ação é aplicada em cluster.

**invalidateUsercache**: Fornece acesso ao comando `invalidateuserCache`. Aplica-se esta ação em cluster.

**killop**: Usuário pode executar o metodo `db.killop()`; Aplica-se esta ação em cluster.

**planCacheread**: Usuário pode executar os comandos `planCacheListPlans` e `planCacheListQueryShapes` e os metodos `PlanCache`.`getPlansByQuery()` e `PlanCache.listQueryShapes()`.  Esta ação se aplica a databass e  collections.

**planCacheWrite**: usuário pode executar o comando `planCacheClear()` e os metodos `PlanCache.clear()` e `PlanCache`.clearPlansByQuery(). Esta ação se aplica a databass e  collections.

**storageDetails**: Usuário pode executar o comando `storageDetails`. Esta ação se aplica a databass e  collections.

### Replication Actions

**appendOplogNote**: Usuário pode adicionar notas ao oplog. Aplica-se esta ação em cluster.

**replSetConfigure**: Usuário pode configurar uma replica set. Aplica-se esta ação em cluster.

**replSetGetStatus**: Usuário pode executar o comando `replSetGetStatus`. Aplica-se esta ação em cluster.

**replSetHeartbeat**: Usuário pode executar o comando `replSetHeartbeat`. Aplica-se esta ação em cluster.

**replSetStateChange** : Usuário pode alterar o status d uma replica set por `repSetFreexe`, `replSetMaintenance`, `replSetStepDown` e `replSetSyncFrom`. Aplica-se esta ação em cluster.
**resync**: Usuário pode executar o comando `resync`. Esta ação é aplicada em cluster.

### Sharding Actions

**addShard**: usuário pode executar o comando `addShard`. Esta ação é aplicada em cluster.

**enableSharding**: Usuário poderá executar o comando enableSharding na database e poderá fazer shard na database usando o comando shardCollection. Aplica-se esta ação em cluster.

**flushRouterConfig**: Usuário pode executar o comando `flushRouterConfig`. Esta ação é aplicada em cluster.

**getShardMap**: Usuário pode executar o comando `flushrouterConfig`.  Esta ação é aplicada em cluster.

**getShardVersion** : Usuário poderá executar ocomando `getShardVersion`.  Esta ação é aplicada em cluster.

**listShard**: Usuário pode executar o comando `listShard`.  Esta ação é aplicada em cluster.

**moveChunk**: Usuário pode executar o comando `moveChunk`. também pode executar o comando `movePrimary` para aplicar privilegios apropriados a uma database.

**removeShard**: Usuário pode executar o comando `removeShard`.  Esta ação é aplicada em cluster.

**shardingState**: Usuário pode executar o comando removeShard`.  Esta ação é aplicada em cluster.

**splitChunk**: Usuário pode executar o comando `splitchunk`.  Esta ação é aplicada em cluster.

**splitVector**: usuário pode executar o comando `splitVector`.  Esta ação é aplicada em cluster.

### Server Administration Actions

**applicationMessage**: Usuário pode executar o comando `logapplicationmessage`.  Esta ação é aplicada em cluster.

**closeAllDatabase**: Usuário pode executar o comando `closeAllDatabases`.  Esta ação é aplicada esta ação em cluster.

**collMod**: Usuário pode executar o comando `collMod`.  Esta ação é aplicada em cluster.

**compact**: Usuário pode executar o comando c`ompact`. Esta ação é aplicada Esta ação é aplicada em cluster.

**connPoolSync**: Usuário pode executar o comando `connPoolSync`.  Esta ação é aplicada em cluster.

**convertToCapped**: Usuário pode executar o comando `connPoolSync`.  Esta ação é aplicada em cluster.

**dropDatabase**: Usuário pode executar o comando `dropDatabase`.  Esta ação é aplicada em cluster.

**dropIndex**: Usuário pode executar o comando `dropIndexs`.  Esta ação é aplicada em cluster.

**fsync**: Usuário pode executar o comando fsync.  Esta ação é aplicada em cluster.

**getParameter**: Usuário pode executar o comando `getParameter`.  Esta ação é aplicada em cluster.

**hostInfo**: Prove informações sobre a instanicia do MongoDB.  Esta ação é aplicada em cluster.

**logRotate**: Usuário pode executar o comando `logRotate`.  Esta ação é aplicada em cluster.

**reIndex**: Usuário pode executar o comando `reIndex`.  Esta ação é aplicada em cluster.

**renameCollectionSameDB**: Permite o usuário a renomear uma Collection usando o comando `renameCollection`. Esta ação é aplicada em uma base de dados.
Usuário deve ter ação de find na collection e não ter find na collection de destino. se a collection já existir com o mesmo nome o usuário deve ter a ação de dropCollection na database de destino. 

**repairDatabase**: Usuário pode executar o comando `repairDatabase`. Esta ação é aplicada em uma database.

**setParameter**: Usuário pode executar o comando `setparameter`. Esta ação é aplicada em cluster.

**Shutdown**: Usuário pode executar o comando `shutdown`.Esta ação é aplicada em cluster.

**touch**: Usuário pode executar o comando `touch`. Esta ação é aplicada em cluster.

### Diagnostic Action

**collStats**: Usuário pode executar o comando `collStats`. Esta ação é aplicada em uma databases ou collections.

**connPoolStats**: Usuário pode executar os comandos `connPoolStats` e `shardConnPoolStats`. Esta ação é aplicada em cluster.

**cursorInfo**: Usuário pode executar o comando `cursorInfo`. Esta ação é aplicada em cluster.

**dbHash**: Usuário pode executr o comando dbhash. Esta ação é aplicada em cluster.

**dialogLogging**: Usuário pode executar o comando `dialogLogging`. Esta ação é aplicada em cluster.

**getCmdLineOpts**: Usuário pode executar o comando `getCmdLineOpts`. Esta ação é aplicada em cluster.

**getLog**: Usuário pode executar o comando getLog. Esta ação é aplicada em cluster.

**IndxStats**: usuário pode executar o comando `indexStats`. Esta ação é aplicada em cluster. Este comando foi removido na versão 3.0.

**listDatabases**: usuário pode executar o comando `listDatabase`.  Esta ação é aplicada em cluster.

**listCollections**: Usuário pode executar o comando `listCollections`.  Esta ação é aplicada em databases.

**listIndexes**: Usuário pode executar o comando `listIndexes`.  Esta ação é aplicada em uma databases e collections.

**netstat**: Usuário pode executar o comando `netstat.  Esta ação é aplicada em cluster.

**serverstatus**: Usuário pode executar o comando `serverStatus`. Esta ação é aplicada em cluster.

**validate**: Usuário pode executar comando `validate`.  Esta ação é aplicada em Databases e collections.

**top**: Usuário pode executar o comando top.  Esta ação é aplicada em cluster.

###Internal Actions

**anyAction**: Permite qualquer ação no recurso. Não aplique esta ação, a não ser em circunstancias excepicionais.

**internal**: Permite ações internas. Não aplique esta ação , a não ser em circunstancias excepicionais.