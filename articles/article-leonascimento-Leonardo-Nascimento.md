# Artigo sobre Autenticação no MongoDb

**Autor**: Leonardo Nascimento de Oliveira

**User**: leonascimento

**Data entregue**: 1450742400000 

## 1) Qual a diferença entre Autenticação e Autorização?

* **Autenticação**: É a ação de verificar e confirmar se algo ou alguem é autêntico, isso requer autoria ou  a veracidade de alguma coisa.

* **Autorização:**: Em segurança da informação, é o mecanismo responsável por garantir que apenas usuários autorizados consumam os recursos (arquivos, rotinas) protegidos de um sistema computacional.

**Autenticação:**

No meu ponto de vista o DBA ou responsável pelos banco de dados possui um usuário admin com todos os privilégios. Logo em seguida
ele é resonsável por cria usuários que desejam conectar ao banco.

Se somos um desses usuários antes de acessar precisamos autenticar, temos um usuário e senha e quando fazemos a autenticação na instância MongoDB, precisamos nos identificar, se somos um usuários podemos conectar a instância do MongoDB como cliente.

**Autorização:**

Se eu sou um cliente e estou autenticado, eu tenho alguma **Autorização**. Ela que gerencia o controle de acesso, determina o acesso de um usuário a recursos e operações. Assim o cliente que fizer a autenticação só possui determinada autorização só deve ser capaz de realizar operações de acordo com sua autorização. Ele possui "privilégios" o que limita o risco potencial de uma aplicação comprometida, alguns podem apenas consultar dados, outros podem ler e gravar dados em determinados bancos de dados ou collections.

Os administradores também podem criar novas funções e privilégios para atender às necessidades operacionais. Os administradores podem atribuir privilégios escopo como granular como o nível da coleção.
Quando concedido uma função, um usuário recebe todos os privilégios dessa função. Um usuário pode ter vários papéis ao mesmo tempo, caso em que o usuário recebe a união de todos os privilégios das respectivas funções.

> Apesar de autenticação e autorização estão intimamente ligados, a autenticação é diferente de autorização.
A **autenticação** verifica a identidade de um usuário; **autorização** determina o acesso do usuário verificada a recursos e operações.

## 2) Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

**Criando um usuário Admin**

* 1- starta o mongo `mongod` em outro terminal digita o comando `mongo`

* 2- Por default todas as informações de autenticação e autorização de usuários fica na collection **system.users** do banco de dados **admin**. Então selecione um database `use admin`

* 3- Faça o uso da função `createUser` dentro dele passe um json com os campos `user, pwd, roles`. Dentor de roles ponha
o valor para o campo role "userAdminAnyDatabase". logo em seguida na propriedade db informe o banco que esse usuário vai ser um admin, lembrando que podemos criar o admin para qualquer banco de dados.

> OBS: o valor "userAdminAnyDatabase" esse papel não restringe permissão que um usuaŕio pode conceder. Os usuários com esse valor pode concender privilégios igual seus privilégios atuais ou todos é o super usuário do sistema.

```JS
db.createUser(
  { 
    user       : "leoadmin" ,
    pwd        : "leo123"   ,
    roles      : [
      { role: "userAdminAnyDatabase", db : "admin" }
    ]
  }
);

/*
Successfully added user: {
  "user": "leoadmin",
  "roles": [
    {
      "role": "userAdminAnyDatabase",
      "db": "admin"
    }
  ]
}
*/
```
* 4- pressione ctrl+c na tela do mongo e na conexão, agora precisamos conectar como u usuário atráves do seguinte comando:

```JS
mongo --port 27017 -u "leoadmin" -p "leo123" --authenticationDatabase "admin"
```


**Criando um usuário Comum**

* 1- starta o mongo `mongod` em outro terminal digita o comando `mongo`

* 2- Por default todas as informações de autenticação e autorização de usuários fica na collection **system.users** do banco de dados **admin**. Então selecione um database `use admin`

* 3- Faça o uso da função `createUser` dentro dele passe um json com os campos `user, pwd, roles`. Dentro do array de roles você pode criar um objeto para cada role. Isso porque podemos atribuir a um usuário várias roles, como "read", "readWrite"; 
```JS
db.createUser(
  { 
    user       : "user3"    ,
    pwd        : "user123" ,
    customData : { "developer" : true } ,
    roles      : [
      { role: "read", db : "admin" }
    ]
  }
);
/* 
Successfully added user: {
  "user": "user3",
  "customData": {
    "developer": true
  },
  "roles": [
    {
      "role": "read",
      "db": "admin"
    }
  ]
}

*/
```

* 4- pressione ctrl+c na tela do mongo, agora precisamos conectar como u usuário atráves do seguinte comando:

```JS
mongo --port 27017 -u "user3" -p "user123" --authenticationDatabase "admin"
```

## 3) Explique cada papel listado [aqui em Cluster Administration Roles](https://docs.mongodb.org/v2.6/reference/built-in-roles/#cluster-administration-roles).

| Papel        |   Descrição  |  
| ------------- |-------------| 
| **Cluster Administration Roles**     | O banco de dados admin inclue os seguintes funções para administração de todo sistema. Em vez de apenas um banco de dados. | 
| **clusterAdmin**     | Fornece o maior acesso de gerenciamento de clusters. Essa função combina os privilégio condedidos pelas funções **clusterManager**, **clusterMonitor**, e **hostManager**. Adicionalmente, a role fornece a ação **dropDatabase**. |   
| **clusterManager** | Fornece o gerenciamento e monitormaento de ações nos cluster. Um usuário com esse papel pode acessar as bases de dados **config** e **local**, que são usadas em "sharding" e "replicação", respecitavamente.       |   
| **clusterMonitor** | Fornece acesso somente de leitura para ferramentas de monitoramento, tais como o agente de monitoramento MongoDB Cloud Manager;      |  
| **hostManager** | Fornece a capacidade de monitorar e gerenciar servidores.|  

## 4) Explique todas as ações de privilégio listadas [aqui Privilege Actions](https://docs.mongodb.org/manual/reference/privilege-actions/).

### Ações de Leitura e Escrita

| Ações | Descrição  |  
| ------------- |-------------| 
| **find**     | O usuário pode usar para realizar o método db.collection.find(). | 
|  **insert**  | O usuário pode usar para realizar o comando [insert](https://docs.mongodb.org/manual/reference/command/insert/#dbcmd.insert) |   
|  **remove**  | O usuário pode usar para realizar o método [db.collection.remove()](https://docs.mongodb.org/manual/reference/method/db.collection.remove/#db.collection.remove)|   
|  **update**  |  O usuário pode usar para realizar o comando [update](https://docs.mongodb.org/manual/reference/command/update/#dbcmd.update) |  
|  **bypassDocumentValidation** |"Adicionado na versão 3.2" oferece a capacidade de valida dorcumentos durante atualizações e inserções. Mas podemos ignorar a validação usando a opção  `bypassDocumentValidation` |  

### Ações de Gerenciamento de Base de Dados

| Ações | Descrição  |  
| ------------- |-------------| 
| **changeCustomData**  | O usuário pode modificar informações de qualquer usuário em um dado banco de dados  | 
|  **insert**  | O usuário pode modificar suas próprias informações personalizadas. |   
|  **changeOwnCustomData**  |  O usuário pode modificar seu próprio password. |   
|  **changeOwnPassword**  |  O usuário pode modificar o password de qualquer usuário em um banco de dados. |  
|  **changePassword** |  O usuário pode modificar qalquer usuário em um banco de dados.|  
|  **createCollection** | O usuário pode executar o método  [db.createCollection()](https://docs.mongodb.org/manual/reference/method/db.createCollection/#db.createCollection) |  
|  **createIndex** | Fprmece p acessp a [db.collection.createIndex()](https://docs.mongodb.org/manual/reference/method/db.collection.createIndex/#db.collection.createIndex)  método e ao comando [createIndexes](https://docs.mongodb.org/manual/reference/command/createIndexes/#dbcmd.createIndexes) .|  
|  **createRole** | Usuáro pode criar novas roles/papeis em um banco de dados |  
|  **createUser** |Usuário pode criar novos usuário num banco de dados. |  
|  **dropCollection** | Usuáro pode executar o método [db.collection.drop()](https://docs.mongodb.org/manual/reference/method/db.collection.drop/#db.collection.drop)|  
| **dropRole**| O usuário pode deletar qualquer role de um banco de dados.|
|  **dropUser** | O usuário pode deletar qualque usuário de um banco de dados.|  
|  **emptycapped** | O usuário pode executar o comando [emptycapped](https://docs.mongodb.org/manual/reference/command/emptycapped/#dbcmd.emptycapped)|  
|  **enableProfiler** | Usuário pode executar o método [ db.setProfilingLevel()](https://docs.mongodb.org/manual/reference/method/db.setProfilingLevel/#db.setProfilingLevel)|  
|  **grantRole** | O usuŕio pode conceder qualquer role em um banco de dados apenas para usuários de apenas bancos do sistema. |  
|  **killCursors** | Usado para matar cursores na coleção alvo. |  
|  **revokeRole** | usado para remover apenas roles de qualquer usuário de apenas bancos de dados do sistema. | 
|  **unlock** | Usuário pode executar o método [db.fsyncUnlock()](https://docs.mongodb.org/manual/reference/method/db.fsyncUnlock/#db.fsyncUnlock)| 
|  **viewRole** |O usuário pode ver infomações sobre qualquer role num banco de dados.| 
|  **viewUser** |O usuário pode ver infomações sobre qualquer usuário num banco de dados.| 


### Ações de Gerenciamento de Implantação

| Ações | Descrição  |  
| ------------- |-------------| 
|  **authSchemaUpgrade** | Permite o uso do comando [authSchemaUpgrade.](https://docs.mongodb.org/manual/reference/command/authSchemaUpgrade/#dbcmd.authSchemaUpgrade)| 
|  **cleanupOrphaned** | Permite o uso do comando [cleanupOrphaned.](https://docs.mongodb.org/manual/reference/command/cleanupOrphaned/#dbcmd.cleanupOrphaned)| 
|  **cpuProfiler** | Permite a ativação e o uso do CPU profiler.|  
|  **inprog** | Permite a utilização do método [db.currentOp()](https://docs.mongodb.org/manual/reference/method/db.currentOp/#db.currentOp) para retornar a algo que esteja pendente e ativar operações.| 
|  **invalidateUserCache** | Permite a utilização do comando [invalidateUserCache.](https://docs.mongodb.org/manual/reference/command/invalidateUserCache/#dbcmd.invalidateUserCache)| 
|  **killop** | Permite a utilização do método [db.killOp().](https://docs.mongodb.org/manual/reference/method/db.killOp/#db.killOp)| 
|  **planCacheRead** |Permite a utilização dos comandos [planCacheListPlans](https://docs.mongodb.org/manual/reference/command/planCacheListPlans/#dbcmd.planCacheListPlans) e [planCacheListQueryShapes](https://docs.mongodb.org/manual/reference/command/planCacheListQueryShapes/#dbcmd.planCacheListQueryShapes) e dos métodos [PlanCache.getPlansByQuery()](https://docs.mongodb.org/manual/reference/method/PlanCache.getPlansByQuery/#PlanCache.getPlansByQuery) e [PlanCache.listQueryShapes().](https://docs.mongodb.org/manual/reference/method/PlanCache.listQueryShapes/#PlanCache.listQueryShapes)| 
|  **planCacheWrite** |Permite a utilização do comando [planCacheClear](https://docs.mongodb.org/manual/reference/command/planCacheClear/#dbcmd.planCacheClear) e dos métodos [PlanCache.clear()](https://docs.mongodb.org/manual/reference/method/PlanCache.clear/#PlanCache.clear) e [PlanCache.clearPlansByQuery().](https://docs.mongodb.org/manual/reference/method/PlanCache.clearPlansByQuery/#PlanCache.clearPlansByQuery)| 
|  **storageDetails** |Permite a utilização do comando storageDetails.| 


### Ações de Replicação

| Ações | Descrição  |  
| ------------- |-------------| 
|  **appendOplogNote** |Permite a adição de notas ao oplog.|
|  **replSetConfigure** |Permite a configuração de uma replica.|
|  **replSetGetStatus** |Permite a utilização do comando [replSetGetStatus.](https://docs.mongodb.org/manual/reference/command/replSetGetStatus/#dbcmd.replSetGetStatus)|
|  **replSetHeartbeat** |Permite a utilização do comando [replSetHeartbeat.](https://docs.mongodb.org/manual/reference/command/replSetHeartbeat/#dbcmd.replSetHeartbeat)|
|  **replSetStateChange** | Permite alterar o estado de uma replica com os comandos [replSetFreeze](https://docs.mongodb.org/manual/reference/command/replSetFreeze/#dbcmd.replSetFreeze), [replSetMaintenance](https://docs.mongodb.org/manual/reference/command/replSetMaintenance/#dbcmd.replSetMaintenance), [replSetStepDown](https://docs.mongodb.org/manual/reference/command/replSetStepDown/#dbcmd.replSetStepDown), e [replSetSyncFrom.](https://docs.mongodb.org/manual/reference/command/replSetSyncFrom/#dbcmd.replSetSyncFrom)|

### Ações de Fragmentação

| Ações | Descrição  |  
| ------------- |-------------| 
|  **addShard** |Permite a utilização do comando [addShard.](https://docs.mongodb.org/manual/reference/command/addShard/#dbcmd.addShard)|
|  **enableSharding** |Permite habilitar o sharding na base de dados utilizando o comando [enableSharding](https://docs.mongodb.org/manual/reference/command/enableSharding/#dbcmd.enableSharding) e em uma coleção utilizando o comando [shardCollection.](https://docs.mongodb.org/manual/reference/command/shardCollection/#dbcmd.shardCollection)
|
|  **flushRouterConfig** |Permite utilizar o comando [flushRouterConfig.](https://docs.mongodb.org/manual/reference/command/flushRouterConfig/#dbcmd.flushRouterConfig)|
|  **getShardMap** |Permite utilizar o comando [getShardMap.](https://docs.mongodb.org/manual/reference/command/getShardMap/#dbcmd.getShardMap)|
|  **getShardVersion** |Permite utiizar o comando [getShardVersion.](https://docs.mongodb.org/manual/reference/command/getShardVersion/#dbcmd.getShardVersion)|
|  **listShards** |Permite utiizar o comando [listShards.](https://docs.mongodb.org/manual/reference/command/listShards/#dbcmd.listShards)|
|  **moveChunk** |Permite utiizar o comando [moveChunk](https://docs.mongodb.org/manual/reference/command/moveChunk/#dbcmd.moveChunk). Além disso, o usuário também pode utilizar o comando [movePrimary](https://docs.mongodb.org/manual/reference/command/movePrimary/#dbcmd.movePrimary) desde que o privilégio tenha sido aplicado a base de dados.|
|  **removeShard** |Permite utiliar o comando  [removeShard.](https://docs.mongodb.org/manual/reference/command/removeShard/#dbcmd.removeShard)|
|  **shardingState** |Permite utilizar o comando [shardingState.](https://docs.mongodb.org/manual/reference/command/shardingState/#dbcmd.shardingState)|
|  **splitChunk** |Permite utilizar o comando [splitChunk.](https://docs.mongodb.org/manual/reference/command/splitChunk/#dbcmd.splitChunk)|
|  **splitVector** |Permite utilizar o comando [splitVector](https://docs.mongodb.org/manual/reference/command/splitVector/#dbcmd.splitVector)|


### Ações de Gerenciamento de Servidores

| Ações | Descrição  |  
| ------------- |-------------|
|  **applicationMessage** | Permite a utilização do comando [logApplicationMessage.](https://docs.mongodb.org/manual/reference/command/logApplicationMessage/#dbcmd.logApplicationMessage) | 
|  **closeAllDatabases** |  Permite a utilização do comando closeAllDatabases.|
|  **collMod** | Permite a utilização do comando [collMod.](https://docs.mongodb.org/manual/reference/command/collMod/#dbcmd.collMod)| 
| **compact** | Permite a utilização do comando [compact.](https://docs.mongodb.org/manual/reference/command/compact/#dbcmd.compact)|
|**connPoolSync**| Permite a utilização do comando [connPoolSync.](https://docs.mongodb.org/manual/reference/command/connPoolSync/#dbcmd.connPoolSync)|
|**convertToCapped**|Permite a utilização do comando [convertToCapped.](https://docs.mongodb.org/manual/reference/command/convertToCapped/#dbcmd.convertToCapped)|
|**dropDatabase**|Permite a utilização do comando [dropDatabase.](https://docs.mongodb.org/manual/reference/command/dropDatabase/#dbcmd.dropDatabase)|
|**dropIndex**|Permite a utilização do comando [dropIndexes](https://docs.mongodb.org/manual/reference/command/dropIndexes/#dbcmd.dropIndexes).|
|**fsync**|Permite a utilização do comando [fsync](https://docs.mongodb.org/manual/reference/command/dropIndexes/#dbcmd.dropIndexes).|
|**getParameter**|Permite a utilização do comando [getParameter](https://docs.mongodb.org/manual/reference/command/getParameter/#dbcmd.getParameter).|
|**hostInfo**|Permite a consulta de informações da instânca da base que se encontra em execucão.|
|**logRotate**|Permite a utilização do comando [logRotate](https://docs.mongodb.org/manual/reference/command/logRotate/#dbcmd.logRotate).|
|**reIndex**|Permite a utilização do comando [reIndex](https://docs.mongodb.org/manual/reference/command/reIndex/#dbcmd.reIndex).|
|**renameCollectionSameDB**|Permite efetuar alterações no nome das coleções da base atual utilizando o comando [renameCollection](https://docs.mongodb.org/manual/reference/command/renameCollection/#dbcmd.renameCollection). Este processo somente será possível se o novo nome ainda não existir.|
|**repairDatabase**|Permite a utilização do comando [repairDatabase](https://docs.mongodb.org/manual/reference/command/repairDatabase/#dbcmd.repairDatabase).|
|**setParameter**|Permite a utilização do comando [setParameter](https://docs.mongodb.org/manual/reference/command/setParameter/#dbcmd.setParameter).|
|**shutdown**|Permite a utilização do comando [shutdown](https://docs.mongodb.org/manual/reference/command/shutdown/#dbcmd.shutdown).|
|**touch**|Permite a utilização do comando [touch](https://docs.mongodb.org/manual/reference/command/touch/#dbcmd.touch).|

### Ações de Diagnósticos

| Ações | Descrição  | 
| ------------- |-------------|
| **collStats**| Permite a utilização do comando [collStats](https://docs.mongodb.org/manual/reference/command/collStats/#dbcmd.collStats).|
| **connPoolStats**|Permite a utilização dos comandos [connPoolStats](https://docs.mongodb.org/manual/reference/command/connPoolStats/#dbcmd.connPoolStats) e [shardConnPoolStats](https://docs.mongodb.org/manual/reference/command/shardConnPoolStats/#dbcmd.shardConnPoolStats).|
| **cursorInfo**|Permite a utilização do comando [cursorInfo](https://docs.mongodb.org/manual/reference/command/cursorInfo/#dbcmd.cursorInfo).|
| **dbHash**|Permite a utilização do comando [dbHash](https://docs.mongodb.org/manual/reference/command/dbHash/#dbcmd.dbHash).|
| **dbStats**|Permite a utilização do comando [dbStats](https://docs.mongodb.org/manual/reference/command/dbStats/#dbcmd.dbStats).|
| **diagLogging**|Permite a utilização do comando [diagLogging](https://docs.mongodb.org/manual/reference/command/diagLogging/#dbcmd.diagLogging).|
| **getCmdLineOpts**|Permite a utilização do comando [getCmdLineOpts](https://docs.mongodb.org/manual/reference/command/getCmdLineOpts/#dbcmd.getCmdLineOpts).|
| **getLog**|Permite a utilização do comando [getLog](https://docs.mongodb.org/manual/reference/command/getLog/#dbcmd.getLog).|
| **indexStats**|Permite a utilização do comando indexStats. OBS: Não tem mais na versão 3.0|
| **listDatabases**|Permite a utilização do comando [listDatabases](https://docs.mongodb.org/manual/reference/command/listDatabases/#dbcmd.listDatabases)|
| **listCollections**|Permite a utilização do comando [listCollections](https://docs.mongodb.org/manual/reference/command/listCollections/#dbcmd.listCollections).|
| **listIndexes**|Permite a utilização do comando ListIndexes.|
| **netstat**|Permite a utilização do comando [netstat](https://docs.mongodb.org/manual/reference/command/netstat/#dbcmd.netstat).|
| **serverStatus**|Permite a utilização do comando [serverStatus](https://docs.mongodb.org/manual/reference/command/serverStatus/#dbcmd.serverStatus).|
| **validate**|Permite a utilização do comando [validate](https://docs.mongodb.org/manual/reference/command/validate/#dbcmd.validate)|
| **top**|Permite a utilização do comando [top](https://docs.mongodb.org/manual/reference/command/top/#dbcmd.top).|

### Ações Internas

| Ações | Descrição  | 
| ------------- |-------------|
|**anyAction**|Permite qualquer ação no recurso.|
|**internal**|Permite ações internas.|