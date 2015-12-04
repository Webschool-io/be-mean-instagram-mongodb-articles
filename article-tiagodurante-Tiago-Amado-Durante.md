Autor: Tiago Amado Durante
User: tiagodurante
Data entregue: Date.now()

  1. Qual a diferença entre Autenticação e Autorização?
    A autenticação consiste no processo de envio de credenciais de determinado usuario a um servidor remoto afim de realizar uma conexão.
    A autorização vem após uma autenticação bem sucedida, depois que as credenciais do usuario foi validada, permitindo o acesso dele ao servidor.

  2. Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
    No processo para criação de um usuario, deve ser utilizado a função createUser e, como parâmetro, deve ser enviado um array de informações pré estabelecidas para que a criação seja bem sucedida (user, pwd e roles, entre outras).
    No caso de um adminstrador, a role userAdminAnyDatabase deve ser utilizada e, em seguida, o nome da database em questão:

    ```
    db.createUser(
        {
            user: "tiago",
            pwd: "altonia",
            roles: [{role: "userAdminAnyDatabase", db: "altonia"}]
        }
      )
    ```

    Para um usuário comum, no campo roles, deve ser passado apenas a database que será usada:

    ```
    db.createUser(
        {
            user: "tiago",
            pwd: "altonia",
            roles: [{db: "altonia"}]
        }
      )
    ```

  3. Explique cada papel listado aqui em Cluster Administration Roles.
    * Database User Roles
      * read: habilita a leitura de informações em determinadas coleções;
      * readWrite: permite a escrita e a leitura de informações nas coleções;

    * Database Administration Roles
      * dbAdmin: libera ações (find, createIndex, update, entre outras) em determinada db. Ações como compactar, dropCollection, dropDatabase não estão liberadas;
      * dbOwner: todas as ações administrativas estão liberadas;
      * userAdmin: permite que o usuário possa criar e modificar novos usuários;

    * Cluster Administration Roles
      * clusterAdmin: libera o total gerenciamento de um cluster;
      * clusterManager: libera o total gerenciamento e monitoramento do cluster, podendo acessar configurações e bancos de dados locais;
      * clusterMonitor: acesso somente leitura para a ferramenta de monitoramento do cluster;
      * hostManager: permite que se monitore e gerencie servidores;

    * Backup and Restoration Roles
      * backup: permite o minimo de privilegios para se realizar o backup. Previlégios suficientes para utilizar o MongoDB Cloud Manager backup agent ou o mongodump que permite realizar backup de uma instancia mongod;
      * restore: permite restaurar dados a partir dos backups realizados, utilizando o mongorestore sem o parâmetro --oplogReplay. Para voltar a reproduzir o oplog, é necessário que o usuario defina uma função que tenha anyAction em anyRestore;

    * All-Database Roles
      * readAnyDatabase: oferece as permissões de leitura, além de que ele se aplica a todos os dbs no cluster;
      * readWriteAnyDatabase: oferece as permissões de escrita e leitura, além de que ele se aplica a todos os dbs no cluster;
      * userAdminAnyDatabase: oferece todas as operações de administração do userAdmin, além de que ele se aplica a todos os dbs no cluster;
      * dbAdminAnyDatabase: oferece todas as operações de administração do dbAdmin, além de que ele se aplica a todos os dbs no cluster;

    * Superuser Roles
      * root: permite que seja atribuido as mesmas permissões do readWriteAnyDatabase, dbAdminAnyDatabase, userAdminAnyDatabase e clusterAdmin de forma conjunta;

    * Internal role
      * __ system (não escrevi junto porque formatava): atribui esta regra aos objetos de usuarios que são membros do cluster, replicaSet e instancias de mongos, permitindo que o usuário possa tomar qualquer decisão contra qualquer objeto no db.


  4. Explique todas as ações de privilégio listadas aqui Privilege Actions.

    * Privilege Actions
      * find: permite usar o método find() em uma collection;
      * insert: permite realizar comandos de inserção na collection;
      * remove: permite realizar o método remove() em uma collection;
      * update: permite realizar comandos de atualização em uma collection;

    * Database Management Actions
      * changeCustomData: permite alterar qualquer informação de qualquer usuário no db fornecido;
      * changeOwnCustomData: permite alterar suas próprias informações cadastradas;
      * changeOwnPassword: permite alterar sua própria senha;
      * changePassword: permite alterar a senha de qualquer usuário;
      * createCollection: permite utilizar o método createCollection();
      * createIndex: permite usar o método createIndex() e o comando createIndexes;
      * createRole: permite criar novas funções no db;
      * createUser: permite criar novos usuários ao db;
      * dropCollection: permite limpar a collection a partir do método drop();
      * dropRole: permite remover qualquer usuário no db;
      * emptyCapped: permite remover todos os documentos de uma coleção utilizando o comando emptycapped;
      * grantRole: permmite conceder qualquer regra a qualquer usuário de qualquer db do sistema;
      * killCursors: permite matar todos os cursores no conjunto de destino;
      * revokeRole: permite remover qualquer regra de qualquer usuário de qualquer db do sistema;
      * unlock: permite usar o método fsyncUnlock() em um cluster;
      * viewRole: permite visualizar informações sobre qualquer função no db;
      * viewUser: permite ver informações de qualquer usuário em qualquer db;

    * Deployment Management Actions
      * authSchemaUpgrade: permite utilizar o comando authSchemaUpgrade no cluster;
      * cleanupOrphaned: permite utilizar o comando cleanupOrphaned no cluster;
      * cpuProfiler: permite ativar e usar o perfil CPU no cluster;
      * inprog: permite usar o método currentOp() para retornar operações ativas e pendentes do cluster;
      * invalidateUserCache: fornece acesso ao comando invalidateUserCache no cluster;
      * killop: o usuario pode realizar o método killOp() no cluster;
      * planCacheRead: permite executar os comandos planCacheListPlans e planCacheListQueryShapes e os métodos getPlansByQuery() e listQueryShapes() no db;
      * planCacheWrite: permite executar o comando planCacheClear e os métodos clear() e clearPlansByQuery() no db;
      * storageDetails: permite realizar o comando storageDetails no db;

    * Replication Actions
      * appendOplogNote: permite acrescentar notas ao oplog no cluster;
      * replSetConfigure: permite configurar um cluster de réplicas;
      * replSetGetStatus: permite realizar o comando replSetGetStatus no cluster;
      * replSetHeartbeat: permite realizar o comando replSetHeartbeat no cluster;
      * replSetStateChange: permite alterar o estado de um cluster de replicas através dos comandos prelSetFreeze, replSetMaintenance, replSetStepDown e replSetSyncFrom no cluster;
      * resync: permite realizar o comando resync no cluster;

    * Replication Actions
      * addShard: permite realizar o comando addShard para adicionar um novo cluster;
      * enableSharding: permite habilitar um shard em um db e pode habiltar um shard em uma collection usando o shardCollection comando;
      * flushRouterConfig: permite utilizar o comando flushRouterConfig no cluster, comando responsável por limpar o cache de um cluster pelo mongos;
      * getShardMap: permite realizar o comando getShardMap no cluster;
      * getShardVersion: permite usar o comando getShardVersion no cluster;
      * listShards: permite utilizar o comando listShards no cluster;
      * moveChunk: permite utilizar o moveChunk no cluster, além disso, é permitido utilizar o comando movePrimary assim que o privilégio é aplicado;
      * removeShard: permite utilizar o removeShard no cluster;
      * shardingState: permite utilizar o comando shardingState no cluster;
      * splitChunk: permite utilizar o comando splitChunk no db;
      * splitVector: permite realizar o comando splitVector no db;

    *  Server Administration Actions
      * applicationMessage: permite realizar o comando logApplicationMessage no cluster;
      * closeAllDatabases: permite realizar o comando closeAllDatabases no cluster;
      * collMod: permite realizar o comando collMod no db;
      * compact: permite executar o comando compact no db;
      * connPoolSync: permite executar o comando connPoolSync no cluster;
      * convertToCapped: permite realizar o comando convertToCapped no db;
      * dropDatabase: permite realizar o comando dropDatabase no db;
      * dropIndex: permite realizar o comando dropIndexes no db;
      * fsync: permite realizar o comando fsync no cluster;
      * getParameter: permite realizar o comando getParameter no cluster;
      * hostInfo: fica disponivel informações sobre a instancia db no servidor que está sendo executado, sendo aplicado no cluster;
      * logRotate: permite executar o comando logRotate no cluster;
      * reIndex: permite realizar o comando reIndex no db;
      * renameCollectionSameDB: permite renomear as coleções no db usando o renameCollection;
      * repairDatabase: permite realizar o comando repairDatabase no db;
      * setParameter: permite realizar o comando setParameter no cluster;
      * shutdown: permite executar o comando shutdown no cluster;
      * touch: permite executar o comando toutch no cluster;

    * Diagnostic Actions
      * collStats: permite executar o comando collStats no db;
      * connPoolStats: permite executar os comandos connPoolStats e shardConnPoolStats no cluster;
      * cursorInfo: permite utilizar o comando cursorInfo no cluster;
      * dbHash: permite realizar o comando dbHash no db;
      * diagLogging: permite utilizar o comando diagLogging no cluster;
      * getCmdLineOpts: permite utilizar o comando getCmdLineOpts no cluster;
      * getLog: permite utilizar o comando getLog no cluster;
      * indexStats: permite utilizar o comando indexStats no db (comando removino na versão 3.0);
      * listDatabases: permite listar as dbs pelo comando listDatabases no cluster;
      * listCollections: permite listar as coleções pelo comando listCollections no db;
      * listIndexes: permite listar os indices no db;
      * netstat: permite executar o comando netstat no cluster;
      * serverStatus: permite realizar o comando serverStatus no cluster;
      * validate: permite executar o comando de validação no db;
      * top: permite utilizar o comando top no cluster;

    * Internal Actions
      * anyAction: permite qualquer ação em determinado recurso, apenas em algumas circunstâncias;
      * internal: permite ações internas, apenas em algumas circunstâncias;
