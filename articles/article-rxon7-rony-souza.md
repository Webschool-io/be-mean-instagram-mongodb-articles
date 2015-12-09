# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Rony Souza
**Data:** 08/12/2015

1. Qual a diferença entre Autenticação e Autorização?
  
  Autenticação:
    É realizada quando é enviada dados de usuário para um servidor, ele pode ser criptografado ou não.

  Autorização:
    É realizada após a autenticação ser validada com sucesso, permite ou não o acesso.


2. Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
  
Criação de usuário admin:

db.createUser(
  {
    user: "appAdmin",
    pwd: "admin123",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)

Criação de usuário comum:

db.createUser(
   {
     user: "appUser",
     pwd: "user123",
     roles: [ ]
   }
)

3. Explique cada papel listado aqui em Cluster Administration Roles.
Funcções de Administração de Cluster:

1 - clusterAdmin:
	Esse papel combina os privilégios concedidos pelos clusterManager, clusterMonitor, e hostManager. Além disso, o papel fornece a ação dropDatabase.

2 - clusterManager:
	Fornece gerencimante e ações de monitoramento no cluster. Um usuário com essa função pode acessar as configurações e os banco de dados locais, que são usados em sharding e replicação, respectivamente. 

	Fornece as seguintes ações no cluster como todo:

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

	Fornece as seguintes ações em todos os banco de dados no cluster:

	* enableSharding 
	* moveChunk      
	* splitChunk     
	* splitVector    

	Na configuração do banco de dados, fornece as seguintes ações nas coleções:

	* insert 
	* remove 
	* update 

	Na configuração do banco de dados, fornece as seguintes ações em todas as coleções de configuração sobre as system.indexes, system.js e system.namespaces:

	Também no banco de dados local, fornece as seguintes ações nas coleções replSet:

	* collStats   
	* dbHash      
	* dbStats     
	* find        
	* killCursors 

3 - clusterMonitor

	Fornece acesso somente de leitura para ferramentas de monitoramento, como o MongoDB Cloud Manager monitoring agent.

	Fornece as seguintes ações no cluster todo:

	* connPoolStats
	* cursorInfo
	* getCmdLineOpts
	* getLog
	* getParameter
	* getShardMap
	* hostInfo
	* inprog
	* listDatabases
	* listShard
	* netstat
	* replSetGetStatus
	* serverStatus
	* shardingState
	* top

	Fornece as seguintes ações em todos os banco de dados no cluster:

	* collStats
	* dbStats
	* getShardVersion

	Oferece também a ação find em todos as coleções system.profile no cluster.

	Fornece as seguintes ações nas configurações das coleções do banco de dados system.index, system.js e system.namespaces:

	* collStats
	* dbHash
	* dbStats
	* find
	* killCursors

4 - hostManager

	Fornece a capacidade de monitorar e gerenciar servidores.

	Fornecendo as seguintes ações em todo o cluster:

	* applicationMessage
	* closeAllDatabases
	* connPoolSync
	* cpuProfiler
	* diagLogging
	* flushRouterConfig
	* fsync
	* invalidateUserCache
	* killop
	* logrotate
	* resync
	* setParameter
	* shutdown
	* touch
	* unlock

	Fornece as seguintes ações em todos os banco de dados no cluster:

	* killCursors
	* RepairDatabase


   
4. Explique todas as ações de privilégio listadas aqui Privilege Actions.

* Privilege Actions
      * `find:` o usuário pode realizar uma busca `db.collection.find()` em uma collection.
      * `insert:` realiza a ação de inserir dados em uma collection.
      * `remove:` realiza a ação de remover dados em uma collection.
      * `update:` realiza a ação de atualizar dados em uma collection.

    
