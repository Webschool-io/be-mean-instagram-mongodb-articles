# Artigo sobre Autenticação no MongoDb

**Autor**: Mauricio Gravena de Oliveira

**User**: maugravena

**Data**: 03/01/2016 -


----------


## **Qual a diferença entre Autenticação e Autorização?**

### Autenticação

Segundo o dicionário significa:

> s.f. Ação ou efeito de autenticar.

Para melhor entendimento vamos ao significado da palavra **autenticar**:

> v.t.d. Validar; reconhecer algo como verdadeiro; admitir a autenticidade, a veracidade de algo...

### Autorização

Novamente com ajuda do dicionário:

> s.f. Ação ou resultado de autorizar; conceder permissão para que alguém faça alguma coisa; concessão.

Dessa vez já conseguimos identificar o que significa de primeira, não é mesmo?

Assim a diferença fica bem nítida, enquanto a autenticação cuida da veracidade de algo a autorização fica com a parte da permissão de alguma determinada ação.


## **Passo-a-passo como criar um usuário administrador e um usuário comum.**

### Usuário Administrador

1. **Comece MongoDB sem controle de acesso.**

  ```
  mongod --port 27017 --dbpath / data / db1
  ```

2. **Conecte-se à instância.**

  ```
  mongo --port 27017
  ```
3. **Crie o usuário administrador.**

  ```
  use admin
  db. createUser (
    {
      user: "UsuarioAdmin",
      pwd: "abc123",
      roles: [{role: "userAdminAnyDatabase", db: "admin"}]
    }
  )
  ```

4. **Reiniciar a instância MongoDB com controle de acesso.**

  ```
  mongod --auth --port 27017 --dbpath / data / db1
  ```

5. **Autenticar como usuário administrador.**

  ```
  mongo --port 27017 -u "UsuarioAdmin" -p "abc123" --authenticationDatabase "admin"
  ```

    Ou, no [mongo](https://docs.mongodb.org/manual/reference/program/mongo/) shell conectado sem autenticação, mude para o banco de dados de autenticação, e use o método **db.auth()** para autenticar.

  ```
  use admin
  db.auth("UsuarioAdmin", "abc123")
  ```

### Usuário Comum

1. **Conectar-se ao MongoDB com os privilégios apropriados.**

  ```
  mongo --port 27017 -u UsuarioAdmin -p abc123 --authenticationDatabase admin
  ```
2. **Criar um novo usuário.**

  Criar usuário em um determindo banco de dados. Utilizar a função **db.createUser()** e passar como parâmetro o usuário.

  A operação a seguir cria um usuário no banco de dados **meu-banco** com o nome, senha e papel(*roles*).

```
use meu-banco
db.createUser(
    {
      user: "mauricio",
      pwd: "123456",
      roles: [{role: "read", db: "meu-banco"}]
    }
)
```

## **Cada papel listado em ***Cluster Administration Roles.*****

### Papéis de banco de dados de usuário

#### **Read**

Oferece a capacidade de ler dados em todas as coleções *non-system* e nas seguintes coleções de sistema: **system.indexes**, **system.js** e **system.namespaces**. O papel fornece acesso de leitura ao conceder as seguintes ações: 

 - collStats 
 - dbHash 
 - dbStats
 -  find 
 - killCursors

#### **readWrite**

Fornece todos os privilégios da função de leitura mais capacidade de modificar dados em todas as coleções *non-system* e na coleção system.js. O papel oferece as seguintes ações nesses coleções:

 - collStats 
 - convertToCapped 
 - createCollection 
 - dbHash 
 - dbStats
 - dropCollection 
 - CreateIndex
 -  dropIndex
 -  emptycapped
 -  find 
 - insert
 - killCursors 
 - remove 
 - renameCollectionSameDB 
 - update

### Papéis de Administração de Cluster

O banco de dados **admin** inclui funções para administrar todo o sistema, em vez de apenas um único banco de dados. Essas funções não estão limitadas apenas a funções administrativas **replica set** e **sharded cluster**.

#### **clusterAdmin**

Fornece o maior acesso de gerenciamento de *cluster*. Este papel combina os privilégios concedidos pelos papéis **clusterManager**, **clusterMonitor**, e hostManager. Além disso, o papel fornece a ação **dropDatabase**.

#### **clusterManager**

Fornece gerenciamento e ações de monitoramento no *cluster*. Um usuário com essa função pode acessar a **config** e **local databases**, que são usados ​​em *sharding* e *replication*, respectivamente.

Fornece as seguintes ações no *cluster* como um todo:

 - addShard 
 - applicationMessage
 -  cleanupOrphaned
 -  flushRouterConfig
 - listShards
 -  removeShard
 -  replSetConfigure 
 - replSetGetStatus
 - replSetStateChange 
 - resync

Fornece as seguintes ações em todos os bancos de dados no cluster:

 - enableSharding
 -  moveChunk
 -  splitChunk
 -  splitVector

No banco de dados **config**, oferece as seguintes ações nas configurações de coleção:

 - insert
 - remove
 - update

No banco de dados de **config**, oferece as seguintes ações em todas as coleções de configuração e sobre as coleções **system.indexes**, **system.js** e **system.namespaces**:

 - collStats 
 - dbHash
 -  find
 -  killCursors

No **local database**, fornece as seguintes ações na coleção **replset**:

 - collStats 
 - dbHash
 -  dbStats
 -  find
 -  killCursors

#### **clusterMonitor**

Fornece acesso *read-only* para ferramentas de monitoramento, como o agente de monitoramento [MongoDB Cloud Manager](https://www.mongodb.com/cloud).

Fornece as seguintes ações no *cluster* como um todo:

 - connPoolStats
 -  cursorInfo
 -  getCmdLineOpts
 -  getLog getParameter
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

Fornece as seguintes ações em todos os bancos de dados no *cluster*:

 - collStats 
 - dbStats 
 - getShardVersion

Oferece a ação *find* em todos as coleções system.profile  no *cluster*.

Fornece as seguintes ações de configuração em coleções de **config database's** e nas coleções **system.indexes**, **system.js** e **system.namespaces**:

 - collStats
 -  dbStats 
 - find 
 - killCursors

#### **hostManager**

Fornece a capacidade de monitorar e gerenciar servidores.

Fornece as seguintes ações no *cluster* como um todo:

 - applicationMessage 
 - closeAllDatabases
 -  connPoolSync 
 - cpuProfiler
 - diagLogging 
 - flushRouterConfig 
 - fsync 
 - invalidateUserCache 
 - killop
 - logrotate
 -  resync
 -  setParameter
 -  shutdown
 -  touch
 -  unlock

Fornece as seguintes ações em todos os bancos de dados no *cluster*:

 - killCursors 
 - RepairDatabase

### Papéis de Backup e restauração

O **admin database** inclui as seguintes funções para backup e restauração de dados:

#### **backup**

Fornece privilégios mínimos necessários para fazer backup de dados. Este papel oferece privilégios suficientes para usar o MongoDB Cloud Manager do [MongoDB Cloud Manager](https://www.mongodb.com/cloud), ou para usar **mongodump** para fazer backup de todo uma instância *mongod*.

Oferece as seguintes ações na coleção **mms.backup** no **admin database**: 

 - insert
 - update

Fornece a ação **listDatabases** no *cluster* como um todo.

Oferece a ação **find** sobre o seguinte:

 - todas as coleções *non-system* no *cluster*
 - todos as seguintes **system collections** no *cluster*: **system.indexes**, **system.namespaces** e **system.js**
 - as coleções **admin.system.users** e **admin.system.roles**
 - coleções legadas **system.users** a partir de versões **MongoDB** anteriores da 2.6

#### **restore**

Fornece privilégios necessários para restaurar os dados a partir de backups. Este papel é suficiente ao restaurar dados com **mongorestore** sem a opção **--oplogReplay**. Se você estiver executando **mongorestore** com **--oplogReplay**, no entanto, o papel é insuficiente para restaurar o **oplog**. Para repetir o **oplog**, cria uma função definida pelo usuário que tem **anyAction** em **anyResource** e conceda apenas aos usuários que devem ser executados **mongorestore** com **--oplogReplay**.

Fornece as seguintes ações em todas as coleções *non-system* e coleções **system.js** no *cluster*; nas coleções **admin.system.users** e **admin.system.roles** no *admin database*; e nas coleções legado **system.users**  de versões do MongoDB anteriores à 2.6:

 - collMod
 -  createCollection
 -  CreateIndex
 -  dropCollection
 -  insert

Fornece as seguintes ações adicionais em **admin.system.users** e  coleções legado **system.users**:

 - find
 - remove
 - update

Fornece a ação find em todos as coloções **system.namespaces** no *cluster*.

Embora, **restore** inclui a capacidade de modificar os documentos na coleção **admin.system.users** usando operações normais de modificação, só modificam esses dados usando os [user management methods](https://docs.mongodb.org/v2.6/reference/method/#user-management-methods).

### Papéis para todos os Banco de Dados

O **admin database** fornece as seguintes funções que se aplicam a todos os bancos de dados em uma instância **mongod**  e são mais ou menos equivalentes para seus equivalentes de um banco de dados específico:
 
#### **readAnyDatabase**

Fornece as mesmas permissões somente leitura como **read**, exceto que ele se aplica a todos os bancos de dados no *cluster*. O papel também fornece a **listDatabases** ação no *cluster* como um todo.

#### **readWriteAnyDatabase** 

Fornece as mesmas permissões de leitura e gravação como **readWrite**, exceto que ele se aplica a todos os bancos de dados no *cluster*. O papel também fornece a ação **listDatabases** no cluster como um todo.

#### **userAdminAnyDatabase**

Fornece o mesmo acesso a operações de administração de usuário como userAdmin, exceto que ele se aplica a todos os bancos de dados no cluster. O papel também fornece as seguintes ações no *cluster* como um todo:

 -  authSchemaUpgrade
 -  invalidateUserCache 
 -  listDatabases

O papel também fornece as seguintes ações nas coleções **admin.system.users** e **admin.system.roles**  no *admin database*, e em coleções legadas **system.users** de versões do MongoDB anteriores à 2.6:

 - collStats 
 - dbHash dbStats 
 - find 
 - killCursors 
 - planCacheRead

*Alterados na versão 2.6.4: **userAdminAnyDatabase** acrescentou as seguintes permissões nas coleções admin.system.users e admin.system.roles:*

 - CreateIndex
 -  dropIndex

O papel **userAdminAnyDatabase** não restringe as permissões que um usuário pode conceder. Como resultado, os usuários **userAdminAnyDatabase** podem conceder-se privilégios a mais de seus privilégios atuais e até mesmo pode conceder-se todos os privilégios, mesmo que o papel não autoriza explicitamente privilégios além da administração de usuário. Este papel é efetivamente um sistema de [superuser](https://docs.mongodb.org/v2.6/reference/built-in-roles/#superuser) MongoDB .

#### **dbAdminAnyDatabase**

Fornece o mesmo acesso a operações de administração de banco de dados como **dbadmin**, exceto que ele se aplica a todos os bancos de dados no *cluster*. O papel também fornece a ação **listDatabases**  no *cluster* como um todo.

### Papéis de *superuser*

Vários papéis fornecer acesso de *superuser* para todo o sistema, quer direta ou indireta.

As funções a seguir fornecem a capacidade de atribuir a qualquer usuário qualquer privilégio em qualquer banco de dados, o que significa que os usuários com uma dessas funções pode atribuir-se qualquer privilégio em qualquer banco de dados:

 - papel **dbOwner**, quando escopo para o **admin** database
 - papel **userAdmin**, quando escopo para o **admin** database
 - papel **userAdminAnyDatabase**

O papel a seguir fornece privilégios completos sobre todos os recursos:

#### **root**

Fornece acesso às operações e todos os recursos do **readWriteAnyDatabase**, **dbAdminAnyDatabase**, **userAdminAnyDatabase** e papéis **clusterAdmin** combinados.

**root** não inclui qualquer acesso a coleções que começam com o prefixo **system.**

Por exemplo, sem a capacidade de inserir os dados diretamente nos :data:system.users *< admin.system.users >* e coleções [system.roles](https://docs.mongodb.org/v2.6/reference/system-collections/#admin.system.roles) no **admin** database. root não é adequado para a escrita ou a restauração dos dados que têm essas coleções (por exemplo, com [mongorestore](https://docs.mongodb.org/v2.6/reference/program/mongorestore/#bin.mongorestore).) Para executar esses tipos de operações de restauração, provisionar usuários com a função de **restore**.

### Papel Interno 

#### **__system** 

MongoDB atribui esse papel a objetos de usuário que representam os membros do *cluster*, como os membros do *replica set* e instâncias [Mongos](https://docs.mongodb.org/v2.6/reference/program/mongos/#bin.mongos). O papel dá direito a tomar qualquer ação contra qualquer objeto no banco de dados.

**Não** atribuir essa função a objetos de usuário que representam aplicações ou administradores humanos, excepto em circunstâncias excepcionais.

Se você precisar de acesso a todas as ações em todos os recursos, por exemplo, para executar os comandos [eval](https://docs.mongodb.org/v2.6/reference/command/eval/#dbcmd.eval) ou [applyOps](https://docs.mongodb.org/v2.6/reference/command/applyOps/#dbcmd.applyOps), não atribua esse papel. Em vez disso, [crie uma função definida pelo usuário](https://docs.mongodb.org/v2.6/tutorial/define-roles/#define-roles) que concede [anyAction](https://docs.mongodb.org/v2.6/reference/privilege-actions/#authr.anyAction) em [anyResource](https://docs.mongodb.org/v2.6/reference/resource-document/#resource-anyresource) e garanta que apenas os usuários que precisa de acesso a essas operações tenham esse acesso.

## **Ações de privilégio listadas em **[Privilege Actions](https://docs.mongodb.org/manual/reference/privilege-actions/).****

### Ações *Query* e *Write*

#### **find**

O usuário pode realizar o método db.collection.find(). Aplicar esta ação para banco de dados ou *collection resources*.

#### **insert**

O usuário pode realizar o método db.collection.remove(). Aplicar esta ação para banco de dados ou *collection resources*.

#### **remove**

O usuário pode realizar o método db.collection.remove (). Aplicar esta ação para banco de dados ou *collection resources*.

#### **update**

O usuário pode realizar o comando *update*. Aplicar esta ação para banco de dados ou *collection resources*.

#### **bypassDocumentValidation**

*Novo na versão 3.2.*

O usuário pode ignorar a validação de documentos sobre os comandos que suportam a opção **bypassDocumentValidation**. Para obter uma lista de comandos que suportem a opção **bypassDocumentValidation**, ver [Document Validation](https://docs.mongodb.org/manual/release-notes/3.2/#rel-notes-document-validation). Aplicar esta ação para banco de dados ou *collection resources*.

### Ações Database Management

#### **changeCustomData** 

O usuário pode alterar as informações de costume de qualquer usuário no banco de dados fornecido. Aplicar esta ação para *database resources*.

#### **changeOwnCustomData**

Os usuários podem alterar suas próprias informações personalizadas. Aplicar esta ação para *database resources*. Veja também como [alterar sua senha e dados personalizados](https://docs.mongodb.org/manual/tutorial/change-own-password-and-custom-data/).

#### **changePassword**

O usuário pode alterar a senha de qualquer usuário no banco de dados fornecido. Aplicar esta ação para *database resources* .

#### **createCollection**

O usuário pode realizar o método db.createCollection(). Aplicar esta ação para banco de dados ou *collection resources*.

#### **CreateIndex**

Fornece acesso ao método db.collection.createIndex() e o comando **createIndexes**. Aplicar esta ação para banco de dados ou *collection resources*.

#### **createRole**

O usuário pode criar novas funções no banco de dados fornecido. Aplicar esta ação para *database resources*.

#### **createUser**

O usuário pode criar novos usuários no banco de dados fornecido. Aplicar esta ação para *database resources*.

#### **dropCollection**

O usuário pode realizar o método db.collection.drop(). Aplicar esta ação para banco de dados ou de *collection resources*.

#### **dropRole**

O usuário pode excluir qualquer papel do banco de dados fornecido. Aplicar esta ação para *database resources*.

#### **dropUser** 

O usuário pode remover qualquer usuário do banco de dados fornecido. Aplicar esta ação para *database resources*.

#### **emptycapped** 

O usuário pode realizar o comando emptycapped. Aplicar esta ação para banco de dados ou *collection resources*.

#### **enableProfiler** 

O usuário pode realizar o método db.setProfilingLevel (). Aplicar esta ação para *database resources*.

#### **grantRole** 

O usuário pode conceder qualquer papel no banco de dados para qualquer usuário de qualquer banco de dados no sistema. Aplicar esta ação para *database resources*.

#### **killCursors** 

O usuário pode matar os cursores no conjunto de destino.

#### **revokeRole** 

O usuário pode remover qualquer papel de qualquer usuário a partir de qualquer banco de dados no sistema. Aplicar esta ação para *database resources*.

#### **unlock** 

O usuário pode realizar o método db.fsyncUnlock (). Aplicar esta ação para o recurso de cluster.

#### **viewRole** 

O usuário pode visualizar informações sobre qualquer função no banco de dados fornecido. Aplicar esta ação para *database resources*.

#### **viewUser** 

O usuário pode visualizar as informações de qualquer usuário no banco de dados fornecido. Aplicar esta ação para *database resources*.


### Ações *Deployment Management*

#### **authSchemaUpgrade** 

O usuário pode realizar o comando authSchemaUpgrade. Aplicar esta ação para o recurso de cluster.

#### **cleanupOrphaned** 

O usuário pode realizar o comando cleanupOrphaned. Aplicar esta ação para o recurso de cluster.

#### **cpuProfiler** 

O usuário pode ativar e usar o *CPU profiler*. Aplicar esta ação para o recurso de cluster.

#### **inprog** 

O usuário pode usar o método db.currentOp() para retornar operações pendentes e ativas . Aplicar esta ação para o recurso de cluster.

#### **invalidateUserCache** 

Fornece acesso o comando invalidateUserCache . Aplicar esta ação para o recurso de cluster.

#### **killop** 

O usuário pode realizar o método db.killOp(). Aplicar esta ação para o recurso de cluster.

#### **planCacheRead** 

O usuário pode executar os comandos planCacheListPlans e planCacheListQueryShapes  e os métodos PlanCache.getPlansByQuery() e PlanCache.listQueryShapes() . Aplicar esta ação para banco de dados ou *collection resources*.

#### **planCacheWrite** 

O usuário pode realizar o comando planCacheClear os métodps PlanCache.clear() e PlanCache.clearPlansByQuery(). Aplicar esta ação para banco de dados ou *collection resources*.

#### **storageDetails** 

O usuário pode executar o comando storageDetails. Aplicar esta ação para banco de dados ou de *collection resources*.

### Ações de replicação

#### **appendOplogNote** 

Usuário pode acrescentar notas à oplog. Aplicar esta ação para o recurso de cluster.

#### **replSetConfigure** 

O usuário pode configurar um conjunto de réplicas. Aplicar esta ação para o recurso de cluster.

#### **replSetGetStatus** 

O usuário pode realizar o comando replSetGetStatus. Aplicar esta ação para o recurso de cluster.

#### **replSetHeartbeat** 

O usuário pode realizar o comando replSetHeartbeat . Aplicar esta ação para o recurso de cluster.

#### **replSetStateChange**

O usuário pode alterar o estado de um conjunto de réplicas através dos comandos replSetFreeze, replSetMaintenance, replSetStepDown, e replSetSyncFrom. Aplicar esta ação para o recurso de cluster.

#### **resync** 

O usuário pode realizar o comando de ressincronização. Aplicar esta ação para o recurso de cluster.

### Ações Sharding 

#### **addShard**

O usuário pode realizar o comando addShard. Aplicar esta ação para o recurso de cluster.

####**enableSharding** 

O usuário pode habilitar sharding em um banco de dados usando o comando enableSharding  e pode fazer um shard de uma coleção usando o comando shardCollection. Aplicar esta ação para banco de dados ou *collection resources*.

####**flushRouterConfig** 

O usuário pode realizar o comando flushRouterConfig . Aplicar esta ação para  *collection resources*.

####**getShardMap** 

O usuário pode realizar o comando getShardMap. Aplicar esta ação para o recurso de cluster.

####**getShardVersion** 

O usuário pode realizar o comando getShardVersion . Aplicar esta ação para *database resources*.

####**listShards** 

O usuário pode realizar o comando listShards. Aplicar esta ação para o recurso de cluster.

####**moveChunk** 

O usuário pode realizar o comando moveChunk. Além disso, o utilizador pode executar o comando movePrimary  desde que o privilégio é aplicado a um *database resource* apropriado. Aplicar esta ação para banco de dados ou *collection resources*.

####**removeShard** 

O usuário pode realizar o comando removeShard. Aplicar esta ação para o recurso de cluster.

####**shardingState** 

O usuário pode realizar o comando shardingState. Aplicar esta ação para o recurso de cluster.

####**splitChunk** 

O usuário pode realizar o comando splitChunk. Aplicar esta ação para banco de dados ou de *collection resources*.

#### **splitVector** 

O usuário pode realizar o comando splitVector. Aplicar esta ação para banco de dados ou *collection resources*.

### Ações de Administração de Servidor

#### **applicationMessage** 

O usuário pode realizar o comando logApplicationMessage. Aplicar esta ação para o recurso de cluster.

#### **closeAllDatabases** 

O usuário pode executar o comando closeAllDatabases. Aplicar esta ação para o recurso de cluster.

#### **collMod** 

O usuário pode realizar o comando collMod. Aplicar esta ação para banco de dados ou *collections resources*.

#### **compact** 

O usuário pode executar o comando compact. Aplicar esta ação para banco de dados ou *collections resources*.

#### **connPoolSync** 

O usuário pode realizar o comando connPoolSync. Aplicar esta ação para o recurso de cluster.

#### **convertToCapped** 

O usuário pode realizar  comando convertToCapped. Aplicar esta ação para banco de dados ou *collections resources*.

#### **dropDatabase** 

O usuário pode realizar o comando dropDatabase. Aplicar esta ação para recursos de banco de dados.

#### **dropIndex** 

O usuário pode realizar o comando dropIndexes. Aplicar esta ação para banco de dados ou *collections resources*.

#### **fsync** 

O usuário pode realizar o comando fsync . Aplicar esta ação para o recurso de cluster.

#### **getParameter** 

O usuário pode realizar o comando getParameter. Aplicar esta ação para o recurso de cluster.

#### **hostInfo** 

Fornece informações sobre o servidor da instância MongoDB que está em execução. Aplicar esta ação para o recurso de cluster.

#### **logrotate** 

O usuário pode executar o comando logrotate. Aplicar esta ação para o recurso de cluster.

#### **reIndex** 

O usuário pode realizar o comando reIndex. Aplicar esta ação para banco de dados ou *collection resources*.

#### **renameCollectionSameDB** 

Permite que o usuário renomeie coleções no banco de dados atual usando o comando renameCollection . Aplicar esta ação para *database resources*.

Além disso, o usuário deve ter **find** na coleção de origem ou não têm **find** na coleção de destino.

Se uma coleção com o novo nome já existir, o usuário também deve fazer ação **dropCollection** na coleção de destino.

#### **RepairDatabase** 

O usuário pode realizar o comando RepairDatabase. Aplicar esta ação para *database resources*.

#### **setParameter** 

O usuário pode realizar o comando setParameter. Aplicar esta ação para o recurso de cluster.

#### **shutdown** 

O usuário pode executar o comando shutdown. Aplicar esta ação para o recurso de cluster.

#### **touch** 

O usuário pode executar o comando touch. Aplicar esta ação para o recurso de cluster.

### Ações de Diagnóstico

#### **collStats** 

O usuário pode realizar comando collStats. Aplicar esta ação para banco de dados ou *collections resources*.

#### **connPoolStats** 

O usuário pode executar os comandos connPoolStats e shardConnPoolStats. Aplicar esta ação para o recurso de cluster.

#### **cursorInfo** 

O usuário pode realizar o comando cursorInfo. Aplicar esta ação para o recurso de cluster.

#### **dbHash** 

O usuário pode realizar o comando dbHash. Aplicar esta ação para banco de dados ou *collections resources*.

#### **dbStats** 

O usuário pode realizar o comando dbStats. Aplicar esta ação para *database resources*.

#### **diagLogging** 

O usuário pode realizar o comando diagLogging. Aplicar esta ação para o recurso de cluster.

#### **getCmdLineOpts** 

O usuário pode realizar o comando getCmdLineOpts. Aplicar esta ação para o recurso de cluster.

#### **getLog** 

O usuário pode realizar o comando getLog. Aplicar esta ação para o recurso de cluster.

#### **indexStats** 

O usuário pode executar o comando indexStats. Aplicar esta ação para banco de dados ou *collections resources*.

Alterado na versão 3.0: MongoDB 3.0 remove o comando indexStats.

#### **listDatabases** 

O usuário pode realizar comando listDatabases. Aplicar esta ação para o recurso de cluster.

#### **listCollections** 

O usuário pode realizar o comando listCollections. Aplicar esta ação para *database resources*.

#### **ListIndexes** 

O usuário pode executar o comando ListIndexes. Aplicar esta ação para banco de dados ou *collection resources*.

#### **netstat** 

O usuário pode executar o comando netstat . Aplicar esta ação para o recurso de cluster.

#### **serverStatus** 

O usuário pode realizar o comando serverStatus. Aplicar esta ação para o recurso de cluster.

#### **validate** 

O usuário pode executar o comando validate. Aplicar esta ação para banco de dados ou *collection resources*.

#### **top** 

O usuário pode executar o comendo top. Aplicar esta ação para o recurso de cluster.

### Ações Internas 

#### **anyAction**
 
Permite que qualquer ação em um recurso. Não atribua essa ação, exceto em circunstâncias excepcionais.

#### **internal** 

Permite ações internas. Não atribua essa ação, exceto em circunstâncias excepcionais.

