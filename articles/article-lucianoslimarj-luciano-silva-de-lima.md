# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Luciano Silva de Lima
**User:** lucianoslimarj
**Data:** 18/12/2015 

## Qual a diferença entre Autenticação e Autorização?

		Autenticação é o processo no qual se verifica a identidade de um usuário  ( ou cliente ). Existe várias formas de autenticar um usuário, porém a forma
	mais comum é através de um par de credenciais: usuário e senha.
	Autorização é o processo que vai determinar quais as operações em quais recursos um determinadao usuário, autenticado, tem acesso. Basicamente, existem 2 tipos
	principais de autorização: por roles ( RBAC ) e por atributos (ABAC). O RBAC é mais fácil de administrar, porém pode apresentar uma explosão no número das roles (
	o que torna a administração ruim). O ABAC é mais flexível e consegue-se atender situações onde o RBAC não atinge. O problema do ABAC é seu controle e ratreabilidade dos acessos.


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

		A criação de usuários no mongoDB é feita pelo comando 'db.createUser' diretamente ou passando o createUser dentro do comando 'db.runCommand(...)'
	A estrutura dos dados do usuário a ser criado, é bem parecida nas duas maneiras. Uma das informações passadas na criação de usuário são as roles que 
	novo usuário terá. Essas roles, que podem ser nativas ( do MongoDB ) ou criadas, concedem privilégios, que são os comandos ou operações em recursos ( 
	banco de dados, coleção, sistema com um todo, ... ). Assim, para criar um usuário administrador é necessário conceder alguma(s) role(s) que darão o poder de
	administrador ao usuário. Por outro lado, um usuário comum, poderia ser criado sem qualquer role. A  exercício, não sei o que um usuário criado
	sem role poderia fazer no sistema, uma vez que ele não teria a permissão de ver nada. Assim, vou encarar usuário comum tendo apenas a role READ para que possa
	"ver" alguma coisa no banco de dados em que foi criado.
  
  **Usuário Administrador**
  
	  db.createUser(
	  {
		user: "UAdmin",
		pwd: "admin123",
		roles: [ { role: "dbOwner", db: "admin" } ]
	  }
	  
    **OU**
	
	db.runCommand( { createUser: "UAdmin",
					 pwd: "admin123",
					 roles: [ { role: "dbOwner", db: "admin" } ]
	} )
	
   **Usuário Comum**
   
    db.createUser(
	  {
		user: "UComum",
		pwd: "comum123",
		roles: []
	 }
	 
	 **OU**
	 
	 db.runCommand( { createUser: "UComum",
					 pwd: "comum123",
					 roles: ["read"]  --ou roles: []
	} )
	

## Explique cada papel listado em Cluster Administration Roles.

		Uma role define um conjunto de privilégios, que são ações que podem ser executadas em recursos ( banco de dados, coleções, sistema como um todo - que inclui réplicas e sharding).
	Temos alguns roles que se aplicam a qualquer banco de dados e temos um conjunto que são específicas ao banco 'admin'. As roles de administração de cluster são deste último tipo e não 
	dizem respeito a apenas réplica e cluster.

  clusterMonitor
  
	Possibilita acesso de leitura aos dados a partir de algum ferramenta de monitoramento, tipo o MongoDB Cloud Manager.
  
	1. Ações a  cluster como um todo:
	
		connPoolStats			- Possibilita a execução dos comandos connPoolStats e shardConnPoolStats. connPoolStats retorna informações de todas as conexões
								abertas no servidor da banco de dados corrente. shardConnPoolStats retorna informações das conexões de banco de dados que estão no pool
								de conexões a serem utilizadas na relação mongos->shard ou quando um mongod precisa fazer um map-reduce em que precisa buscar dados em outro servidor.
								
		cursorInfo				- Retorna informações de cursores.
		getCmdLineOpts			- Retorna as opções de linha de comando utilizada no mongod ou mongos.
		getLog					- Retorna as últimas entradas do log do processo mongod.
		getParameter			- Recupera os valores de algumas opções passadas na linha de comando.
		getShardMap				- Comando interno de suporte a funcionalidade de shard.
		hostInfo				- Disponibiliza informações do servidor onde o mongoDB está rodando.
		inprog					- Invoca db.currentOp() que retorna informações das operações correntes no servidor do mongoDB.
		listDatabases			- Lista os banco de dados existentes juntamente com uma breve estatistica de cada um.
		listShards				- Retorna a lista de shards configurados. Disponível apenas em instancias de mongos.
		netstat					- Comando interno, disponível apenas em instância de mongos.
		replSetGetStatus		- Retorna o status da replica set na perspectiva do servidor corrente, participante de uma replica set.
		serverStatus			- Retorna um visão geral do estado do processamento do servidor. Estas informações são úteis para diagnósticos e avaliar o desempenho do servidor.
		shardingState			- Detecta se o servidor é um componente de shard em cluster. Caso o servidor seja uma instancia primária de uma replica set ou participa de shard, o comando retorna verdadeira juntamente com algumas outras informações.
		top						- Retorno estatistica de uso de coleção, como: inserções, atualizações, consultas, etc...
		
	2. Ações em todos os bancos de dados do cluster:
	
		collStats				- Retorna uma variedade de estatisticas de uma coleção.
		dbStats					- Retorna estatisticas do banco de dados corrente.
		getShardVersion			- Comando interno para suporte de sharding. Não está disponível no shell mongo.
		
	3. Possibilita o 'find' na coleção 'system.profile' a nivel de cluster.
	
	4. Possibilita as seguintes ações no banco de dados 'config' ( banco dos shards) e nas coleções 'system.indexes', 'system.js' e 'system.namespaces'.
	
		collStats				- Retorna uma variedade de estatisticas das coleções citadas
		dbHash					- Comando de suporte ao servidor de configurações ( config servers) e não disponibilizado no shell mongo.
		dbStats					- Retorna estatisticas do banco de dados. Neste caso, 'config'.
		find					- Possibilita o 'find' nas coleções citadas.
		killCursors				- Possibilita o 'fechamento' de cursores nas coleções citadas.
	
	
  hostManager
  
	Possibilita a monitoramento e gerenciamento de servidores.
	
	1. Ações a  cluster como um todo:
  
		applicationMessage - Disponível apenas na versão Enterprise. Possibilita o usuário colocar alguma mensagem personalizada no log de auditoria.
		closeAllDatabases  - Invalida todos os cursores e fecha todos os bancos de dados.
		connPoolSync       - Trata-se de um comando interno.
		cpuProfiler        - Possibilita o uso do profiler de CPU. Principalmente para visualizar a alocação da memória de cada servidor do cluster.
		diagLogging        - Captura algum dado adicional para efeitos de diagnósticos. Descontinuado a partir versão 3.0.
		flushRouterConfig  - O router ( mongos) mantém os metadados da distribuição dos chunks em cache. Este comando limpa esse cache e o 
		                     recarrega a partir do banco de dados 'config'.
		fsync			   - O mongoDB não vai ao disco a cada escrita no banco de dados. As atualizações ficam por algum tempo no
		                     na camada de armazenamento ( storage) e de tempos em tempos vai ao disco para concretizar as alterações nos arquivos de dados.
							 fsync força que todas as alterações no camada de storage sejam enviadas para os arquivos.
		
		invalidateUserCache	- Invalida os dados de cache ( em memória ) dos usuários. Dados estes que compreendem as credencias e roles dos usuários.
		killop				- Força o término de uma operação a partir do seu ID. Para buscar os IDs de operações, utilizar o comando: db.currentOp().
		logRotate			- Faz um rotacionamento nos logs do mongoDB para evitar que o arquivo fique muito grande e ocupe muito espaço em disco.
		resync				- Força a resincronização de um slave desatualizado com o mongod master. Só se aplica quando temos master->slave e não quando temos vários nodos em replica sets.
		setParameter		- Permite alterar algumas opções utilizadas na linha de comando do mongod e/ou mongos.
		shutdown			- Libera todos os recursos do banco de dados e finaliza o processo.
		touch				- Carrega dados da camada de storage para memória. Dados podem ser documentos e/ou indexes.
		unlock				- Executa o camando db.fsyncUnlock que libera operações de escrita no banco de dados. Geralmente utiliza-se esse comando após uma operação de backup.
		
		db.fsyncUnlock()

	2. Ações em todos os bancos de dados do cluster:
	
		killCursors			- Permitir encerrar cursores em coleções.
		repairDatabase      - Checa e acerta eventuais inconsistências na camada de storage após um erro inesperado no processo do mongo.
		

  clusterManager
	
	Permite a execução de ações de gerenciamento e monitoramento no cluster. Também é possível ter acesso aos bancos de dados de shard, 'config' e de replica set, 'local'.

    1. Ações a  cluster como um todo:
	
		addShard			- Adicionar novos servidores de mongo em um shard ou replica set.
		applicationMessage	- Disponível apenas na versão Enterprise. Possibilita o usuário colocar alguma mensagem personalizada no log de auditoria.
		cleanupOrphaned		- Possibilita a limpeza de documentos que estão em mais de um shard. Essa situação ocorre devido a uma má execução de migração de chunks.
		flushRouterConfig   - O router ( mongos) mantém os metadados da distribuição dos chunks em cache. Este comando limpa esse cache e o 
		                      recarrega a partir do banco de dados 'config'.
		listShards			- Retorna uma lista de shards configurados. Executado a partir de um router de shard ( mongos).
		removeShard			- Remove um shard da lista de shards. O mongoDB migra os chunks de dados deste shard para outro no cluster.
		replSetConfigure	- Retorna um documento que descreve a configuração atual da replica set. È equivalente a invocar o método rs.conf() no mongo shell.
		replSetGetStatus	- Retorna o status da replica set na perspectiva do servidor corrente, participante de uma replica set.
		replSetStateChange  - Muda o status de um replica set através de um dos comandos: replSetFreeze, replSetMaintenance, replSetStepDown ou replSetSyncFrom.
		resync				- Força a resincronização de um slave desatualizado com o mongod master. Só se aplica quando temos master->slave e não quando temos vários nodos em replica sets.
		
	2. Ações em todos os bancos de dados do cluster:
  
		enableSharding		- Permite colocar um banco de dados para ser 'shardeado'.
		moveChunk			- Possibilita a execução dos comandos: moveChunk e movePrimary. moveChunk: move chunks de dados, das coleções 'shardeadas', entre os shards de um cluster. O comando
							  movePrimary move os chunks de dados das coleções 'não shardedas' para um outro shard. Lembrando que todas as coleções 'não shardeadas' residem em shard 
							  que é chamado shard primária ( que não a ver com o conceito de primário de réplica sets)
		splitChunk			- Cria novos chunks de uma coleção já 'shardeada'. Habilita a execução dos comandos sh.splitFind() e sh.splitAt().
		splitVector			- Utilizado para determinar pontos de distribuição quando uma coleção sofre inserções ou atualizações.

	3. Operações na coleção 'config.settings'
	
		Possibilita inserções, deleções e atualizações na coleção 'config.settings'.

	4. Ações no banco 'config', suas coleções e nas coleções 'system.indexes', 'system.js' e 'system.namespaces'
	
		Possibilita as ações, no banco 'config', e suas coleções, e nas coleções 'system.indexes', 'system.js' e 'system.namespaces'

		collStats			- Retorna uma variedade de estatisticas das coleções citadas.
		dbHash				- Comando de suporte ao servidor de configurações ( config servers) e não disponibilizado no shell mongo.
		dbStats				- Retorna estatisticas do banco de dados. Neste caso, 'config'.
		find				- Possibilita o 'find' nas coleções citadas.
		killCursors			- Permitir encerrar cursores nas coleções citadas.
	
	5. Ações no banco 'local' e na coleção 'local.repset'
	
		collStats			- Retorna uma variedade de estatisticas das coleções citadas.
		dbHash				- Comando de suporte ao servidor de configurações ( config servers) e não disponibilizado no shell mongo.
		dbStats				- Retorna estatisticas do banco de dados. Neste caso, 'local'.
		find				- Possibilita o 'find' nas coleções citadas.
		killCursor			- Permitir encerrar cursores nas coleções citadas.
		
  clusterAdmin
	
	É a role que tem os maiores privilégios num gerenciamento de cluster. Engloba todos os privilégios das roles clusterManager, clusterMonitor e hostManager.
	Além disso, possibilita a exclusão de bancos de dados ( em todos os servidores do shard ), através do comando dropDatabase.
   

## Explique todas as ações de privilégio listadas em Privilege Actions.

	Ações de busca e Escrita
	
		find
			Realiza uma busca em uma coleção através do comando db.collect.find(). O retorno é um cursor e não uma coleção de documentos.
		   
		insert
			Insere um ou mais documentos em uma coleção através do comando 'insert'. Retorna um documento com o status da inserção de todos os documentos.
		   
		remove
			Remove um ou mais documentos em uma coleção através do comando db.collection.remove(). Retorna um documento com o status da operação e informando o total de
			documentos deletados. Caso ocorra algum erro, a mensagem de erro é retornada.
		
		update
			Atualiza documentos em uma coleção. Um simples comando 'update' pode conter multiplas seções de atualizações.
	
	
	Ações de gerenciamento de Banco de dados
		
		chanceCustomData
			Possibilita a alteração de informações customizadas de qualquer usuário no banco de dados. Aplica-se a  banco de dados.
		
		changeOwnCustomData
			Possibilita que o usuário altera suas próprias informações customizadas dentro de um banco de dados.
			
		changeOwnPassword
			Possibilita que o usuário altera sua própria senha dentro de um banco de dados.
			
		changePassword
			Possibilita a alteração da senha de qualquer usuário no banco de dados. Aplica-se a  banco de dados.
			
		createCollection
			Possibilita que um usuário crie coleções, através do comando 'db.createCollection(...)', em um banco de dados. Aplica-se a nivel de banco de dados.
		
		createIndex
			Permite que usuário crie índices em uma coleção de um banco de dados, através dos comandos 'db.collection.createIndex(...)' ou 'createIndexes'. Se aplicado
			a  banco dados, o usuário poderá criar indices em todas as coleções de usuários, neste banco de dados.
			
		createRole
			Permite que o usuário crie roles em um banco de dados. Aplica-se a  banco de dados.
			
		createUser
			Permite que um usuário crie novos usuários dentro de um banco de dados. Aplica-se a nivel de banco de dados.
		
		dropCollection
		    Permite que um usuaário delete coleções, através do comando 'db.collection.dropCollection()', em um banco de dados. Aplica-se a  banco de dados.
			
		dropRole
		    Permite que um usuário delete uma role em um banco de dados. Aplica-se a nivel de banco de dados.
		
		dropUser
		    Permite que um usuário delete um outro usuário de um banco de dados. Aplica-se a nivel de banco de dados.
			
		emptyCapped
			Permite que usuário exclua todos os documentos de uma 'capped' collection. O comando utilizado é o 'emptyCapped'.
		
		enableProfiler
			Permite que usuário altere o  profiler de um banco de dados. Aplica-se a banco de dados e o comando utilizado é o 'db.setProfilingLevel()'.
		
		grantRole
		    Permite que usuário conceda qualquer role a qualuer usuario dentro de um banco de dados. Aplica-se a nivel de banco de dados.
			
		killCursors
			Permite que usuário exclua cursores de uma coleção.
		
		revokeRole
		    Permite que usuário remova qualquer role a qualuer usuario dentro de um banco de dados. Aplica-se a nivel de banco de dados.
			
		unlock
			Permite que um usuário execute o comando 'db.fsyncUnlock()', que é o inverso do 'db.fsyncLock()', geralmente utilizando em backup. Aplica-se a nivel de cluster.
		
		viewRole
			Permite que um usuário visualize as informações das roles em um banco de dados. Aplica-se a nivel de banco de dados.
		
		viewUser
		    Permite que um usuário visualize informações de outros usuários em um banco de dados. Aplica-se a nivel de banco de dados.
		
		
	Ações de gerenciamento de instâncias
	
		authSchemaUpgrade
			Permite a atualização dos processos de autenticação e autorização para versões mais novas. Exemplo: 2.4 para 2.6 e 2.6 para 3.0. Aplica-se a nivel de cluster.
			
		cleanupOrphaned
			Possibilita a limpeza de documentos que estão em mais de um shard. Essa situação ocorre devido a uma má execução de migração de chunks.
		
		cpuProfiler
			Possibilita o uso do profiler de CPU. Principalmente para visualizar a alocação da memória de cada servidor do cluster.
			
		inprog
			Possibilita a execução do comando 'db.currentOp()' que retorna informações das operações correntes no servidor do mongoDB.
			
		invalidateUserCache
			Invalida os dados de cache ( em memória ) dos usuários. Dados estes que compreendem as credencias e roles dos usuários. Utiliza-se o comando 'invalidateUserCache'.
			
		killop
			Força o término de uma operação a partir do seu ID. Para buscar os IDs de operações, utilizar o comando: db.currentOp().
		
		planCacheRead
			Permite a leitura de planos de acessos que estão em cache ( utilizado pelo otimizador de query). Aplica-se a nivel de coleção ou banco de dados.
			
		planCacheWrite
			Permite a limpeza do cache de planos de acessos de uma coleção. Aplica-se a nivel de coleção ou banco de dados. Aplica-se a  banco de dados ou coleção.
		
		storageDetails
			Permite, através do comando 'storageDetais', a visualização de dados de armazenazem. Aplica-se a nivel de coleção ou banco de dados.
			
	
	Ações de Réplicas
			
		appendOplogNote
			Permite o usuário colocar anotações oplog. Aplica-se a cluster.
			
		replSetConfigure
			Permite ao usuário realizar configurações em um réplica set. Aplica-se a cluster.
		
		replSetGetStatus
			Permite ao usuário executar o comando 'replSetGetStatus', que retorna o status da replica set na perspectiva do servidor corrente, participante de uma replica set.
			
		replSetHeartbeat
			Permite ao usuário executar o comando 'replSetHeartbeat', comando interno de suporte a réplicas set.
		
		replSetStateChange
			Permite ao usuário mudar o status da replica set através dos comandos 'replSetFreeze','replSetMaintenance','replSetStepDown' e 'replSetSyncFrom'.
			O comando 'replSetSyncFrom' possibilita que um membro de um replica set seja escolhido com um primário. o comando 'replSetMaintenance' informa que um
			membro de uma réplica se encontra em manutenção ( ou seja, não operacional).O camando 'replSetStepDown' faz com que membro primário se torne secundário, 
			possibilitando assim, a escolha de um novo primário. O comando 'replSetSyncFrom' informa de servidor o processo mongod buscará o oplog.
		
		resync
			Força a resincronização de um slave desatualizado com o mongod master. Só se aplica quando temos master->slave e não quando temos vários nodos em replica sets.
		
	
	Ações de Sharding	
	
		addShard
			Possibilita a execução do comando 'addShard', que adiciona um novo servidor de banco de dados, ou uma replica set, sejam incluída no cluster. É executado em mongos e aplica-se a nivel de cluster.
			
		enableSharding
			Habilita a execução do comando 'enableSharding' e 'shardCollection'. O comando 'enableSharding' faz com que um banco de dados tenha alguma de suas coleções
			distribuídas pelos participantes do cluster. 'shardCollection' possibilita que usúario informe uma coleção em particular a ser distribuída pelo cluster.
			
		flushRouterConfig
			O router ( mongos) mantém os metadados da distribuição dos chunks em cache. O comando 'flushRouterConfig' limpa esse cache e o recarrega a partir do banco de dados 'config'.
		
		getShardMap
			Possibilita a execução do comando 'getShardMap' qeue é um Comando interno de suporte a funcionalidade de shard.
					
		getShardVersion
			Possibilita a execução do comando 'getShardVersion' que é um comando interno para suporte de sharding. Não está disponível no shell mongo.
			
		listShards
			Possibilita a execução do comando 'listShards' que retorna uma lista de shards configurados. Executado a partir de um router de shard ( mongos).
		
		moveChunk
			Possibilita a execução dos comandos: moveChunk e movePrimary. moveChunk: move chunks de dados, das coleções 'shardeadas', entre os shards de um cluster. O comando
			movePrimary move os chunks de dados das coleções 'não shardedas' para um outro shard. Lembrando que todas as coleções 'não shardeadas' residem em shard 
			que é chamado shard primária ( que não a ver com o conceito de primário de réplica sets)
			
		removeShard
			Possibilita a execução do comando 'removeShard' que remove um shard da lista de shards. O mongoDB migra os chunks de dados deste shard para outro no cluster.
		
		shardingState
			Possibilita a execução do comando 'shardingState' que detecta se o servidor é um componente de shard em cluster. 
			Caso o servidor seja uma instancia primária de replica set ou participa de shard, o comando retorna verdadeira juntamente com algumas outras informações.
		
		splitChunk
			Possibilita a execução do comando 'splitChunk' que cria novos chunks de uma coleção já 'shardeada'. Também habilita a execução dos comandos sh.splitFind() e sh.splitAt().
		
		splitVector
			Possibilita a execução do comando 'splitVector' que é utilizado para determinar pontos de distribuição quando uma coleção sofre inserções ou atualizações.
		
		
	Ações de Administração do servidor
	
		applicationMessage
			Disponível apenas na versão Enterprise. Possibilita o usuário colocar alguma mensagem personalizada no log de auditoria. Aplica-se a  cluster.
		
		closeAllDatabases
		    Possibilita a execução do comando 'closeAllDatabases' que invalida todos os cursores e fecha todos os bancos de dados. Aplica-se a  cluster.
		
		collMod
			Possibilita a execução do comando 'collMod' que viabiliza a inclusão de flags em coleções afim de mudar o comportamento do mongoDB. Aplica-se a 
			banco de dados ou coleções.
			
		compact
			Possibilita a execução do comando 'compact' que realiza uma desfragmentação nos dados e indices de uma coleção. Aplica-se a banco de dados e coleções.
		
		connPoolSync
			Possibilita a execução do comando 'connPoolSync' que é um comando interno.
			
		convertToCapped
			Possibilita a execução do comando 'convertToCapped' que converte uma coleção não limitada em uma limitada ( dentro do mesmo banco de dados). Aplica-se a banco de dados e coleções.
			
		dropDatabase
			Possibilita a execução do comando 'dropDatabase' que exclui um banco de dados e seus arquivos associados ( no sistema de arquivos).
			
		dropIndex
			Possibilita a execução do comando 'dropIndexes' que exclui um mais indices em um banco de dados. Aplica-se a banco de dados ou coleções.
			
		fsync
			Possibilita a execução do comando 'fsync' que força que todas as alterações no camada de storage sejam enviadas para os arquivos.
			
		getParameter
			Possibilita a execução do comando 'getParameter' que recupera os valores de algumas opções passadas na linha de comando. Aplica-se a cluster.
			
		hostInfo
			Disponibilidade informações do servidor em que é executado. Aplica-se a cluster.
		
		logRotate
			Possibilita a execução do comando 'logRotate' que faz um rotacionamento nos logs do mongoDB para evitar que o arquivo fique muito grande e ocupe muito espaço em disco.
			
		reIndex
			Possibilita a execução do comando 'reIndex' que exclui e recria todos os indices de uma coleção. Aplica-se a banco de dados ou coleções.
			
		renameCollectionSameDB
			Possibilita a execução do comando 'renameCollection' que renomeia uma coleção existente. Aplica-se a banco de dados.
			
		repairDatabase
			Possibilita a execução do comando 'repairDatabase' que checa e acertar inconsistência de dados na camada de armazenamento. Geralmente, executado após
			uma finalização inesperado do processo do mongo.

		setParameter
			Possibilita a execução do comando 'setParameter' que muda algum parâmetro informado na linha de comando do processo do mongo. Obrigatório uso do admin. Aplica-se a cluster.
		
		shutdown
			Possibilita a execução do comando 'shutdown' que libera todos os recursos do banco de dados e finaliza o processo.
		
		touch
			Possibilita a execução do comando 'touch' que carrega dados da camada de storage para memória. Dados podem ser documentos e/ou indexes.
		
		
	Ações de Diagnósticos
	
		collStats
			Possibilita a execução do comando Retorna uma variedade de estatisticas das coleções citadas
			
		connPoolStats
			Possibilita a execução dos comandos connPoolStats e shardConnPoolStats. connPoolStats retorna informações de todas as conexões
			abertas no servidor da banco de dados corrente. shardConnPoolStats retorna informações das conexões de banco de dados que estão no pool
			de conexões a serem utilizadas na relação mongos->shard ou quando um mongod precisa fazer um map-reduce em que precisa buscar dados em outro servidor.
			
		cursorInfo
			Possibilita a execução do 'cursorInfo' que retorna informações de cursores.
			
		dbHash
			Possibilita a execução do comando 'dbHash', comando de suporte ao servidor de configurações ( config servers) e não disponibilizado no shell mongo.
			
		dbStats
			Possibilita a execução do comando 'dbStats' que retorna estatisticas do banco de dados. Aplica-se a banco de dados.
			
		diagLogging
			Possibilita a execução do comando 'diagLogging' que captura algum dado adicional para efeitos de diagnósticos. Descontinuado a partir versão 3.0.
			
		getCmdLineOpts
			Possibilita a execução do comando 'getCmdLineOpts' que retorna as opções de linha de comando utilizada no mongod ou mongos.
			
		getLog
			Possibilita a execução do comando 'getLog' que retorna as últimas entradas do log do processo mongod.
			
		indexStats
			Possibilita a execução do comando 'indexStats'. Este comando foi removido da versão 3.0.
			
		listDatabases
			Possibilita a execução do comando 'listDatabases' que lista os banco de dados existentes juntamente com uma breve estatistica de cada um.
			
		listCollection
			Possibilita a execução do comando 'listCollection' que busca informações, especificamente nome e opções, de coleção em um banco de dados. Aplica-se
			a banco de dados. Na linha de comando de mongo, os substitutos são os comandos 'db.getCollectionInfos()' e 'db.getCollectionNames()'.
			
		listIndexes
			Possibilita a execução do comando 'ListIndexes' que exibe informações de todos os indices associados as coleções. Aplica-se a banco de dados e coleções.
			
		netstat
			Possibilita a execução do comando 'netstat'. Comando interno, disponível apenas em instância de mongos.
			
		serverStatus
			Possibilita a execução do comando 'serverStatus' que retorna um visão geral do estado do processamento do servidor. 
			Estas informações são úteis para diagnósticos e avaliar o desempenho do servidor. Aplica-se a cluster.
			
		validate
			Possibilita a execução do comando 'validate' que checa a estrutura de uma coleção fazendo uma varredura nos seus dados e indexes. Aplica-se a banco de dados e a coleções.
			
		top
			Possibilita a execução do comando 'top' que retorna estatísticas de uso para cada coleção. Fornece o total de tempo usado, em microsegundos, e um total de
			algumas operações, tais como: readLock,writeLock,queries,getmore,insert,update,remove, etc...
		
	Ações Internas
	
		anyAction
			Possibilita qualquer ação em um recurso. Esta ação só deve ser utilizada em circunstâncias excepcionais.
						
		internal
			Possibilita a execução de ações internas. Esta ação só deve ser utilizada em circunstâncias excepcionais.
		
	
		
