# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Gabriel Alves Scavassa
**Data** 1450386300

## Qual a diferença entre Autenticação e Autorização?

	A **Autenticação** é a verficação das credenciais enviadas para o DB para identificar se 
	os dados conferem com os dados de quem ele diz ser.

	A **Autorização** é um processo que se inicia após a **Autenticação**. Este processo verifica
	o nível de acesso ou as permissões que um usuário pode ter no DB.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

	Para a criação de usuários no MongoDB é utilziado o comando ´createUser´. Este comando deverá conter
	os seguinte campos obrigatórios:
	´´´js
	{
		user : "nome_do_usuário",
		pwd  : "senha-em-texto-simples",
		roles: [{db : "db-onde-será-aplicado"}] // array com os roles para os DBs
	}
	´´´
	
	Roles são as permissões concedidas para efetuar determinadas ações em uma base.

	Para criar um usuário administrador:
	O valor da propriedade role, dentro do arry roles, deve ser preenchida com um papel que permita ações de administrador.
	No caso, usamos a role "userAdminAnyDatabase", que permite ao usuário ações de admin em qualquer data base da instância.

	´´´js
	use admin

	db.createUser(
	{
		user   : "admin",
		pwd    : "123eja",
		roles :  [{ role : "userAdminAnyDatabase", db : "admin" }]

	})
	´´´
	Para criar um usuário comum:
	Não é necessário adicionar um role, mas apenas um db. 

	´´´js
	use admin

	db.createUser(
	{
		user   : "comum",
		pwd    : "123eja",
		roles :  [{db : "admin"} ]

	})
	´´´
## Explique cada papel listado em Cluster Administration Roles.

	#### Database user Role
	Esta role pode realziar ações de escrita e leitura em collections de data bases que não são do sistemas.
	1 - **Read** : Possibilidade de leitura dos dados de collections.
	2 - **readWrite** : Permite que um usuário leia os dados e escreva em determinadas collections. A system.js é 
	a única collection do sistema que esta role pode realizar alteração de dados.

	#### Database Administration Roles
	Esta role permite que seja realizadas ações de administração das bases da instância do MongoDB.
	1 - **dbAdmin** : Permite ações nas bases system.index, system.namespaces e system.profile além das bases que não 
	são do sistema.
	2 - **dbOwner** : Possui ações administrativas em todas as bases do MongoDb
	3 - **userAdmin** : Possibilita a criação ou edição de roles em databases com algumas ações como : revokeRole, createRole, createUser...

	#### Cluster Administration Role
	1 - **clusterAdmin** : Tem o maior nível de administração de clusters este role combina todas as ações de clusterManager, clusterMonitor e hostManager.
	2 - **clusterManager** : Esta role tem permissões de monitoramento e gerenciamento nos clusters. 
	3 - **clusterMonitor** : Possui apenas leitura para ações de monitoramento do MongoDB Cloud Manager
	4 - **hostManager** :  Role de monitoramento e gestão de servers

	#### Backup and Restoration Roles
	1 - **backup** : Esta role possui apenas ações de backup 
	2 - **restore** : Possui apenas privilegio de restauração de base.

	#### All-Database Role
	Permissões para todas as bases da instancia do MongoDB.
	1 - **readAnyDatabase** : Leitura de dados de todas as DBs da instância.
	2 - **readWriteAnyDatabase** : Performa a mesma ação de leitura e escrita que readWrite porém para todas as dbs da instância.
	3 - **userAdminAnyDatabase** : Performa as mesmas permissões de userAdmin porém para todas as dbs da instância.
	4 - **dbAdminAnyDatabase** : Performa as mesmas permissões que dbAdmin porém para todas as dbs da instância.

	#### Super Role
	1 - **root** : possui todas as ações de readWriteAnyDatabase, dbAdminAnyDatabase, userAdminAnyDataBase e clusterAdmin combinadas.

	#### Internal role
	1 - **__system** : 	Atribuição do MongoDB para usuários que representam membros de um cluster. É um papel gerencial que pode tomar ações 
	sobre qualquer informações dos DBs da instância.



## Explique todas as ações de privilégio listadas em Privilege Actions.

	#### Query and Write Actions
	**find** : Usuário pode executar o metodo db.collection.find(). Aplica esta ação para Database ou collection.
	**insert**:  Usuário pode realizar comandos de Isnert.  Aplica esta ação para Database ou collection.
	**remove** : usuário pode realziar o metodo db.collection.remove().  Aplica esta ação para Database ou collection.
	**update** : usuário pode realziar comandos de Update.  Aplica esta ação para Database ou collection.
	**bypassDocumentValidation**  : Usuário pode ignorar validações em ações que suportam o bypassDocumentValidation. Aplica esta ação para Database ou collection.

	#### Database Management Actions
	**changeCustomData** : Usuário poderá alterar informações do Custom de qualquer usuário na database. Aplica esta ação para Database ou collection.
	**changeOwnPassword** : Usuário pode altear suas próprias informações. Aplica esta ação para Database ou collection.
	**changepassword** : Usuário poderá alterar o password de qualquer usuário na database. Aplica esta ação para Database ou collection.
	**createCollection** : Usuário poderá executar o método db.createCollection(). Aplica esta ação para Database ou collection.
	**createIndex** : Prove acesso ao método db.collection.createindex() e ao comando createIndexes. Aplica esta ação para Database ou collection.
	**createRole** : Usuário poderá cirar uma Role na database. Aplica esta ação para Database ou collection.
	**createuser** : Usuário pode criar novos usuários na database. Aplica esta ação para Database ou collection.
	**dropCollection** : Usuário pode executar o comando db.collection.drop(). Aplica esta ação para Database ou collection.
	**dropRole** : Usuário pode deletar qualquer papel de uma database. Aplica esta ação para Database ou collection.
	**dropUser** : Usuário pode remover qualquer usuário de uma database. Aplica esta ação para Database ou collection.
	**emptycapped** : Usuário pode executar o comando emptycapped. Aplica esta ação para Database ou collection.
	**enableProfile** : Usuário pode executar o metodo db.setProfilingLevel(). Aplica esta ação para Database.
	**grantRole** : Usuário pode executar o metodo db.setProfilingLevel(). Aplica esta ação para Database.
	**killCursors** : Usuário pode matar qualquer cursor na collection.
	**revokeRole ** : Usuário pode remover qualquer papel de qualquer usuário em qualquer database. Aplica esta ação para Database.
	**unlock** : Usuário pode executar ometodo db.fsyncUnlock();. Aplica esta ação em Cluster.
	**viewRole** : Usuário pode ver qualquer informação de qualquer papel na database. Aplica esta ação para Database.
	**viewUser** : Usuário poderá ver qualquer informação de qualquer usuário em uma database. Aplica esta ação para Database.

	#### Deployment Management Actions
	**authSchemaUpgrade** : Usuário pode executar o comando authSchemaUpgrade. Aplica-se esta ação em cluster.
	**cpuProfile** : Usuário pode habilitar e usar o perfil CPU. Aplica-se esta ação em cluster.
	**inprog** : Usuário pode usar o metodo db.currentOp() para retornar ações ativas e pendentes. Aplica-se esta ação em cluster.
	**invalidateUsercache** : Fornece acesso ao comando invalidateuserCache. Aplica-se esta ação em cluster.
	**killop** : Usuário pode executar o metodo db.killop(); Aplica-se esta ação em cluster.
	**planCacheread** : Usuário pode executar os comandos planCacheListPlans e planCacheListQueryShapes e os metodos PlanCache.getPlansByQuery() e PlanCache.listQueryShapes().  Esta ação se aplica a databass e  collections.
	**planCacheWrite** : usuário pode executar o comando planCacheClear() e os metodos PlanCache.clear() e PlanCache.clearPlansByQuery(). Esta ação se aplica a databass e  collections.
	**storageDetails** : Usuário pode executar o comando storageDetails. Esta ação se aplica a databass e  collections.

	#### Replication Actions
	**appendOplogNote** : Usuário pode adicionar notas ao oplog. Aplica-se esta ação em cluster.
	**replSetConfigure** : Usuário pode configurar uma replica set. Aplica-se esta ação em cluster.
	**replSetGetStatus** : Usuário pode executar o comando replSetGetStatus. Aplica-se esta ação em cluster.
	**replSetHeartbeat** : Usuário pode executar o comando replSetHeartbeat. Aplica-se esta ação em cluster.
	**replSetStateChange** : Usuário pode alterar o status d uma replica set por repSetFreexe, replSetMaintenance, replSetStepDown e replSetSyncFrom. Aplica-se esta ação em cluster.
	**resync** : Usuário pode executar o comando resync. Aplica-se esta ação em cluster.

	#### Sharding Actions
	**addShard** : usuário pode executar o comando addShard. Aplica-se esta ação em cluster.
	**enableSharding** : Usuário poderá executar o comando enableSharding na database e poderá fazer shard na database usando o comando shardCollection. Aplica-se esta ação em cluster.
	**flushRouterConfig** : Usuário pode executar o comando flushRouterConfig. Aplica-se esta ação em cluster.
	**getShardMap** : Usuário pode executar o comando flushrouterConfig. Aplica-se esta ação em cluster.
	**getShardVersion** : Usuário poderá executar ocomando getShardVersion. Aplica-se esta ação em cluster.
	**listShard** : Usuário pode executar o comando listShard. Aplica-se esta ação em cluster.
	**moveChunk** : Usuário pode executar o comando moveChunk. também pode executar o comando movePrimary para aplicar privilegios apropriados a uma database.
	**removeShard** : Usuário pode executar o comando removeShard. Aplica-se esta ação em cluster.
	**shardingState** : Usuário pode executar o comando removeShard. Aplica-se esta ação em cluster.
	**splitChunk** : Usuário pode executar o comando splitchunk. Aplica-se esta ação em cluster.
	**splitVector** : usuário pode executar o comando splitVector. Aplica-se esta ação em cluster.
	
	#### Server Administration Actions
	**applicationMessage** : Usuário pode executar o comando logapplicationmessage. Aplica-se esta ação em cluster.
	**closeAllDatabase** : Usuário pode executar o comando closeAllDatabases. Aplica-se esta ação em cluster.
	**collMod** : Usuário pode executar o comando collMod. Aplica-se esta ação em cluster.
	**compact** : Usuário pode executar o comando compact. Aplica-se esta ação em cluster.
	**connPoolSync** : Usuário pode executar o comando connPoolSync. Aplica-se esta ação em cluster.
	**convertToCapped** : Usuário pode executar o comando connPoolSync. Aplica-se esta ação em cluster.
	**dropDatabase** : Usuário pode executar o comando dropDatabase. Aplica-se esta ação em cluster.
	**dropIndex** : Usuário pode executar o comando dropIndexs. Aplica-se esta ação em cluster.
	**fsync** : Usuário pode executar o comando fsync. Aplica-se esta ação em cluster.
	**getParameter** : Usuário pode executar o comando getParameter. Aplica-se esta ação em cluster.
	**hostInfo** : Prove informações sobre a instanicia do MongoDB. Aplica-se esta ação em cluster.
	**logRotate** : Usuário pode executar o comando logRotate. Aplica-se esta ação em cluster.
	**reIndex**: Usuário pode executar o comando reIndex; Aplica-se esta ação em cluster.
	**renameCollectionSameDB** : Permite o usuário a renomear uma Collection usando o comando renameCollection. Aplica-se esta ação em uma base de dados.
	Usuário deve ter ação de find na collection e não ter find na collection de destino. se a collection já existir com o mesmo nome o usuário deve ter a ação de dropCollection na database de destino. 
	**repairDatabase**: Usuário pode executar o comando repairDatabase. Aplica-se esta ação em database.
	**setParameter** : Usuário pode executar o comando setparameter. Aplica-se esta ação em cluster.
	**Shutdown** : Usuário pode executar o comando shutdown. Aplica-se esta ação em cluster.
	**touch** : Usuário pode executar o comando touch. Aplica-se esta ação em cluster.

	#### Diagnostic Action
	**collStats** : Usuário pode executar o comando collStats. Aplica-se esta ação em databases ou collections.
	**connPoolStats** : Usuário pode executar os comandos connPoolStats e shardConnPoolStats. Aplica-se esta ação em cluster.
	**cursorInfo** : Usuário pode executar o comando cursorInfo. Aplica-se esta ação em cluster.
	**dbHash** : Usuário pode executr o comando dbhash. Aplica-se esta ação em cluster.
	**dialogLogging** : Usuário pode executar o comando dialogLogging. Aplica-se esta ação em cluster.
	**getCmdLineOpts** : Usuário pode executar o comando getCmdLineOpts. Aplica-se esta ação em cluster.
	**getLog** : Usuário pode executar o comando getLog. Aplica-se esta ação em cluster.
	**IndxStats** : usuário pode executar o comando indexStats. Aplica-se esta ação em cluster. Este comando foi removido na versão 3.0.
	**listDatabases** : usuário pode executar o comando listDatabase. Aplica-se esta ação em cluster.
	**listCollections** : Usuário pode executar o comando listCollections. Aplica-se esta ação em databases.
	**listIndexes** : Usuário pode executar o comando listIndexes.Aplica-se esta ação em databases e collections.
	**netstat** : Usuário pode executar o comando netstat. Aplica-se esta ação em cluster.
	**serverstatus** : Usuário pode executar o comando serverStatus. Aplica-se esta ação em cluster.
	**validate** : Usuário pode executar comando validate. Aplica-se esta ação em Databases e collections.
	**top** : Usuário pode executar o comando top. Aplica-se esta ação em cluster.

	####Internal Actions
	**anyAction** : Permite qualquer ação no recurso. Não aplique esta ação, a não ser em circunstancias excepicionais.
	**internal** : Permite ações internas. Não aplique esta ação , a não ser em circunstancias excepicionais.

 


	

















