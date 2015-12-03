# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Francisco Henrique Ruiz Valério
**Data** 03/12/2015 20:06

## Qual a diferença entre Autenticação e Autorização?

Autenticação:
	É utilizado para autenticar uma conexão, ou seja, validar determinado usuário que esta tentando realizar uma conexão no data base.
	
Autorização:
	Concede ao usuário controle de acesso a dados e comandos através de autorização baseada em funções(roles) em determinada data base.
	
## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

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

## Explique cada papel listado em Cluster Administration Roles.

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


## Explique todas as ações de privilégio listadas em Privilege Actions.

Ações de privilégios definem as operações que um usuário pode executar em um recurso. Um privilégio MongoDB dispõe de recurso e as ações permitidas. 

	1 - find
		O usuário pode realizar o comando db.colecao.find(). Em uma coleção ou banco de dados. Esse comando irá retornar todos os objetos da coleção ou data base se não for passado nenhum filtro.

	2 - insert
		O usuário pode realizar o comando insert. Em uma coleção ou banco de dados. Esse comando irá inserir um novo registro em sua coleção ou banco de dados selecionado.

	3 - remove
		O usuário pode realizar o comando db.colecao.remove(). Em uma coleção ou banco de dados. Esse comando irá remover o registro desejado dependendo dos filtros passados como parâmetro, caso não seja passado irá remover todos os registros.

	4 - update
		O usuário pode realizar o comando update, Em uma coleção ou banco de dados. Esse comando irá alterar os registros dependendo do objeto que for passado como parâmetro.

