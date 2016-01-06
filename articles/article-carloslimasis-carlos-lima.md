# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Nome Completo
**Data** Date.now()

## Qual a diferença entre Autenticação e Autorização?
A autenticação verifica a identidade de um utilizador já a autorização determina o acesso do usuário verificada a recursos e operações.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
Para criar um usuário Administrador basta rodar o comando abaixo:

```
db.createUser(
  {
    user: "UsuarioAdmin",
    pwd: "admin123",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
```

Já para criar um usuário comum basta rodar o comando abaixo:

```
db.createUser(
    {
      user: "reportsUser",
      pwd: "12345678",
      roles: [
         { role: "read", db: "reporting" },
         { role: "read", db: "products" },
         { role: "read", db: "sales" },
         { role: "readWrite", db: "accounts" }
      ]
    }
)
```

No código acima criamos um usuário comum. Este usuário tem permissão de leitura nas coleções 'reporting', 'products' e 'sales' e permissão de leitura e escrita na coleção 'accounts'.


## Explique cada papel listado em Cluster Administration Roles.

O MongoDB nos fornece alguns perfis para administração de clusters. Entre elas estão:

- clusterAdmin

Este perfil fornece o maior nível de acesso a gerenciamento de cluster, englobando todos os privilégios concedidos aos perfis clusterManager, clusterMonitor e hostManager.

- clusterManager

Este perfil fornece acesso às ações de monitoramento de cluster. Quando um usuário é um clusterManager ele possui acesso à configuração e banco de dados locais que são usados em sharding e replicação.

- clusterMonitor

Este perfil fornece acesso somente para a leitura utilizando ferramentas de monitoramento, como por exemplo o agente de monitoramento MongoDB Cloud Manager e Ops Manager monitoring agent.

- hostManager

Este perfil tem a acesso a monitoramento e gerenciamento de servidores.

## Explique todas as ações de privilégio listadas em Privilege Actions.

Quando falamos de Privilege Actions nos referimos às ações que um usuário pode executar em um determinado recurso. 
As Privilege Actions estão divididas em:

- Query and Write Actions
	
	find -> O usuário poderá executar o método db.collection.find(). Esta ação deve ser aplicada para banco de dados ou em recursos de coleções.

	insert -> O usuário poderá executar o comando de inserção. Esta ação deve ser aplicada para banco de dados ou em recursos de coleções.

	remove -> O usuário poderá executar o método db.collection.remove(). Esta ação deve ser aplicada para banco de dados ou em recursos de coleções.

	update -> O usuário poderá executar o comando update. Esta ação deve ser aplicada para banco de dados ou em recursos de coleções.

- Database Management Actions
	
	changeCustomData -> O usuário poderá alterar as informações comuns de qualquer usuário no banco de dados fornecido. Esta ação deve ser aplicada em recursos de banco de dados.

	changeOwnCustomData -> O usuário poderá alterar suas próprias informações personalizadas. Esta ação deve ser aplicada em recursos de banco de dados.

	changeOwnPassword -> O usuário poderá alterar sua própria senha. Esta ação deve ser aplicada em recursos de banco de dados.

	changePassword -> O usuário poderá alterar a senha de qualquer usuário no banco de dados fornecido. Esta ação deve ser aplicada em recursos de banco de dados.

	createCollection -> O usuário poderá executar o método db.createCollection(). Esta ação deve ser aplicada em banco de dados ou em recursos de coleções.

	createIndex -> Fornece acesso ao método db.collection.createIndex() e ao comando createIndexes. Esta ação deve ser aplicada em banco de dados ou em recursos de coleções.

	createRole -> O usuário pode criar novas funções no banco de dados fornecido. Esta ação deve ser aplicada em recursos de banco de dados.

	createUser -> O usuário pode criar novos usuários no banco de dados fornecido. Esta ação deve ser aplicada em recursos de banco de dados.

	dropCollection -> O usuário pode executar o método db.collection.drop(). Esta ação deve ser aplicada em recursos de banco de dados.

	dropRole -> O usuário pode excluir qualquer papel do banco de dados fornecido. Esta ação deve ser aplicada em recursos de banco de dados.

	dropUser -> O usuário pode remover qualquer usuário do banco de dados fornecido. Esta ação deve ser aplicada em recursos de banco de dados.

	emptycapped -> O usuário pode executar o comando emptycapped. O comando emptycapped remove todos os documentos de uma capped collection. Esta ação deve ser aplicada em recursos de banco de dados.

	enableProfiler -> O usuário pode executar o método db.setProfilingLevel (). Esta ação deve ser aplicada em recursos de banco de dados.

	grantRole -> O usuário pode conceder qualquer papel no banco de dados para qualquer usuário de qualquer banco de dados no sistema. Esta ação deve ser aplicada em recursos de banco de dados.

	killCursors -> O usuário pode matar os cursores no conjunto de destino.

	revokeRole -> O usuário pode remover qualquer papel de qualquer usuário a partir de qualquer banco de dados no sistema. Esta ação deve ser aplicada em recursos de banco de dados.

	unlock -> O usuário pode executar o método db.fsyncUnlock (). Esta ação deve ser aplicada para o recurso de cluster.

	viewRole -> O usuário pode visualizar informações sobre qualquer função no banco de dados fornecido. Esta ação deve ser aplicada em recursos de banco de dados.

	viewUser -> O usuário pode visualizar as informações de qualquer usuário no banco de dados fornecido. Esta ação deve ser aplicada em recursos de banco de dados.

- Deployment Management Actions
	
	authSchemaUpgrade -> O usuário pode executar o comando authSchemaUpgrade. Aplicar esta ação para o recurso de cluster.

	cleanupOrphaned -> O usuário pode executar o comando cleanupOrphaned. Aplicar esta ação para o recurso de cluster.

	cpuProfiler -> O usuário pode ativar e usar o profiler CPU. Aplicar esta ação para o recurso de cluster.

	inprog -> O usuário pode usar o método db.currentOp() para retornar operações pendentes e ativas. Aplicar esta ação para o recurso de cluster.

	invalidateUserCache -> Fornece acesso ao comando invalidateUserCache. Aplicar esta ação para o recurso de cluster.

	killop -> O usuário pode executar o método db.killOp(). Aplicar esta ação para o recurso de cluster.

	planCacheRead -> O usuário pode executar as planCacheListPlans e comandos planCacheListQueryShapes e os métodos PlanCache.getPlansByQuery() e PlanCache.listQueryShapes(). Aplicar esta ação para banco de dados ou de recursos de coleções.

	planCacheWrite -> O usuário pode executar o comando planCacheClear eo PlanCache.clear() e PlanCache.clearPlansByQuery() métodos. Aplicar esta ação para banco de dados ou de recursos de coleções.

	storageDetails -> O usuário pode executar o comando storageDetails. Aplicar esta ação para banco de dados ou de recursos de coleções.

- Replication Actions
	
	appendOplogNote -> Usuário pode acrescentar notas à oplog. Aplicar esta ação para o recurso de cluster.

	replSetConfigure -> O usuário pode configurar um conjunto de réplicas. Aplicar esta ação para o recurso de cluster.

	replSetGetStatus -> O usuário pode executar o comando replSetGetStatus. Aplicar esta ação para o recurso de cluster.

	replSetHeartbeat -> O usuário pode executar o comando replSetHeartbeat. Aplicar esta ação para o recurso de cluster.

	replSetStateChange -> O usuário pode alterar o estado de um conjunto de réplicas através da replSetFreeze, comandos replSetMaintenance, replSetStepDown, e replSetSyncFrom. Aplicar esta ação para o recurso de cluster.

	resync -> O usuário pode executar o comando de ressincronização. Aplicar esta ação para o recurso de cluster.

- Sharding Actions

	addShard -> O usuário pode executar o comando addShard. Aplicar esta ação para o recurso de cluster.

	enableSharding -> O usuário pode habilitar sharding em um banco de dados usando o comando enableSharding e pode caco uma coleção usando o comando shardCollection. Aplicar esta ação para banco de dados ou de recursos de coleções.

	flushRouterConfig -> O usuário pode executar o comando flushRouterConfig. Aplicar esta ação para o recurso de cluster.

	getShardMap -> O usuário pode executar o comando getShardMap. Aplicar esta ação para o recurso de cluster.

	getShardVersion -> O usuário pode executar o comando getShardVersion. Aplicar esta ação para recursos de banco de dados.

	listShards -> O usuário pode executar o comando listShards. Aplicar esta ação para o recurso de cluster.

	moveChunk -> O usuário pode executar o comando moveChunk. Além disso, o utilizador pode executar o comando movePrimary desde que o privilégio é aplicado a um recurso base de dados apropriada. Aplicar esta ação para banco de dados ou de recursos de coleções.

	removeShard -> O usuário pode executar o comando removeShard. Aplicar esta ação para o recurso de cluster.

	shardingState -> O usuário pode executar o comando shardingState. Aplicar esta ação para o recurso de cluster.

	splitChunk -> O usuário pode executar o comando splitChunk. Aplicar esta ação para banco de dados ou de recursos de coleções.

	splitVector -> O usuário pode executar o comando splitVector. Aplicar esta ação para banco de dados ou de recursos de coleções.

- Server Administration Actions

	applicationMessage -> O usuário pode executar o comando logApplicationMessage. Aplicar esta ação para o recurso de cluster.

	closeAllDatabases -> O usuário pode executar o comando closeAllDatabases. Aplicar esta ação para o recurso de cluster.

	collMod -> O usuário pode executar o comando collMod. Aplicar esta ação para banco de dados ou de recursos de coleções.

	compact -> O usuário pode executar o comando compacto. Aplicar esta ação para banco de dados ou de recursos de coleções.

	connPoolSync -> O usuário pode executar o comando connPoolSync. Aplicar esta ação para o recurso de cluster.

	convertToCapped -> O usuário pode executar o comando convertToCapped. Aplicar esta ação para banco de dados ou de recursos de coleções.

	dropDatabase -> O usuário pode executar o comando dropDatabase. Aplicar esta ação para recursos de banco de dados.

	dropIndex -> O usuário pode executar o comando dropIndexes. Aplicar esta ação para banco de dados ou de recursos de coleções.

	fsync -> O usuário pode executar o comando fsync. Aplicar esta ação para o recurso de cluster.

	getParameter -> O usuário pode executar o comando getParameter. Aplicar esta ação para o recurso de cluster.

	hostInfo -> Fornece informações sobre o servidor a instância MongoDB é executado. Aplicar esta ação para o recurso de cluster.

	logRotate -> O usuário pode executar o comando logrotate. Aplicar esta ação para o recurso de cluster.

	reIndex -> O usuário pode executar o comando REINDEX. Aplicar esta ação para banco de dados ou de recursos de coleções.

	renameCollectionSameDB -> Permite ao usuário renomear coleções no banco de dados atual usando o comando renameCollection. Aplicar esta ação para recursos de banco de dados. Se uma coleção com o novo nome já existir, o usuário também deve ter a ação dropCollection na coleção de destino.

	repairDatabase -> O usuário pode executar o comando RepairDatabase. Aplicar esta ação para recursos de banco de dados.

	setParameter -> O usuário pode executar o comando setParameter. Aplicar esta ação para o recurso de cluster.

	shutdown -> O usuário pode executar o comando de desligamento. Aplicar esta ação para o recurso de cluster.

	touch -> O usuário pode executar o comando touch. Aplicar esta ação para o recurso de cluster.

- Diagnostic Actions

	collStats -> O usuário pode executar o comando collStats. Aplicar esta ação para banco de dados ou de recursos de coleções.

	connPoolStats -> O usuário pode executar as connPoolStats e comandos shardConnPoolStats. Aplicar esta ação para o recurso de cluster.

	cursorInfo -> O usuário pode executar o comando cursorInfo. Aplicar esta ação para o recurso de cluster.

	dbHash -> O usuário pode executar o comando dbHash. Aplicar esta ação para banco de dados ou de recursos de coleções.

	dbStats -> O usuário pode executar o comando dbStats. Aplicar esta ação para recursos de banco de dados.

	diagLogging -> O usuário pode executar o comando diagLogging. Aplicar esta ação para o recurso de cluster.

	getCmdLineOpts -> O usuário pode executar o comando getCmdLineOpts. Aplicar esta ação para o recurso de cluster.

	getLog -> O usuário pode executar o comando getLog. Aplicar esta ação para o recurso de cluster.

	indexStats -> O usuário pode executar o comando indexStats. Aplicar esta ação para banco de dados ou de recursos de coleções.Alterado na versão 3.0: MongoDB 3.0 remove o comando indexStats.

	listDatabases -> O usuário pode executar o comando listDatabases. Aplicar esta ação para o recurso de cluster.

	listCollections -> O usuário pode executar o comando listCollections. Aplicar esta ação para recursos de banco de dados.

	listIndexes -> O usuário pode executar o comando ListIndexes. Aplicar esta ação para banco de dados ou de recursos de coleções.

	netstat -> O usuário pode executar o comando netstat. Aplicar esta ação para o recurso de cluster.

	serverStatus -> O usuário pode executar o comando serverStatus. Aplicar esta ação para o recurso de cluster.

	validate -> O usuário pode executar o comando validate. Aplicar esta ação para banco de dados ou de recursos de coleções.

	top -> O usuário pode executar o comando top. Aplicar esta ação para o recurso de cluster.

- Internal Actions

	anyAction -> Permite que qualquer ação em um recurso. Não atribua essa ação, exceto em circunstâncias excepcionais.

	internal -> Permite ações internas. Não atribua essa ação, exceto em circunstâncias excepcionais.
