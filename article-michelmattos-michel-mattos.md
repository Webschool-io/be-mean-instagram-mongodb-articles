# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Michel Mattos
**Data** Sun, 06 Dec 2015 15:57:24 GMT

## Qual a diferença entre Autenticação e Autorização?
Autenticação e autorização são dois passos distintos, mas complementares. A *autenticação* é reponsável por verificar se **uma pessoa é quem ela realmente diz que é**, enquanto a *autorização* verifica se **uma pessoa (após ter sua identidade autenticada) pode executar uma determinada ação**.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
Para criar um usuário administrador, você precisa ter o papel `userAdmin` no banco de dados desejado ou `userAdminAnyDatabase`.
Tendo um dos papéis acima, basta executar:
```javascript
> use be-mean
> db.createUser(
	{
		user: 'michel',
		pwd: 'pwd123',
		roles: ['dbOwner']
	},
	{ w: 'majority', wtimeout: 5000 }
)
```
E para criar um usuário com acesso de administrador para **TODOS OS BANCOS DE DADOS** em um cluster:
```javascript
> use admin
> db.createUser(
	{
		user: 'michel',
		pwd: 'pwd123',
		roles: ['userAdminAnyDatabase']
	},
	{ w: 'majority', wtimeout: 5000 }
)
```

Para criar um usuário comun, com acesso apenas para leitura e escrita, basta executar:
```javascript
> use be-mean
> db.createUser(
	{
		user: 'michel',
		pwd: 'pwd123',
		roles: ['readWrite']
	},
	{ w: 'majority', wtimeout: 5000 }
)
```
E para criar um usuário com acesso de leitura e escrita para **TODOS OS BANCOS DE DADOS** em um cluster:
```javascript
> use admin
> db.createUser(
	{
		user: 'michel',
		pwd: 'pwd123',
		roles: ['readWriteAnyDatabase']
	},
	{ w: 'majority', wtimeout: 5000 }
)
```

## Explique cada papel listado em Cluster Administration Roles.
### clusterAdmin
É o papel com o maior nível de acesso de gerenciamento de um cluster. Este papel combina os privilégios dados pelos papeis **clusterManager**, **clusterMonitor** e **hostManager**. Além disso, este papel também permite executar a ação `dropDatabase`.

### clusterManager
Este papel permite executar ações de gerenciamento e monitoramento em um cluster. Um usuário com esse papel pode acessar os bancos de dados `local` e `config`, que são usados para replicação e sharding, respectivamente.

### clusterMonitor
Permite acesso apenas de leitura à ferramentas de monitoração, como o **MongoDB Cloud Manager** e **Ops Manager**.

### hostManager
Permite monitorar e gerenciar servidores.

## Explique todas as ações de privilégio listadas em Privilege Actions.
### Ações de Escrita e Busca
* `find`: usuário pode realizar buscas num banco de dados ou coleção.
* `insert`: usuário pode inserir novos dados num banco de dados ou coleção.
* `remove`: usuário pode remover dados num banco de dados ou coleção.
* `update`: usuário pode atualizar dados num banco de dados ou coleção.

### Ações de Gerenciamento de Banco de Dados
* `changeCustomData`: usuário pode mudar as informações customizadas de qualquer usuário num banco de dados.
* `changeOwnCustomData`: usuário pode mudar suas próprias informações customizadas num banco de dados.
* `changeOwnPassword`: usuário pode mudar sua senha num banco de dados.
* `changePassword`: usuário pode mudar a senha de qualquer usuário num banco de dados.
* `createCollection`: usuário pode criar uma coleção num banco de dados ou coleção.
* `createIndex`: usuário pode criar índices num banco de dados ou coleção.
* `createRole`: usuário pode criar papéis num banco de dados.
* `createUser`: usuário pode criar usuários num banco de dados.
* `dropCollection`: usuário pode excluir uma coleção num banco de dados ou coleção.
* `dropRole`: usuário pode excluir papéis num banco de dados.
* `dropUser`: usuário pode excluir usuários num banco de dados.
* `emptycapped`: usuário pode excluir todos os dados de uma coleção de tamanho limitado num banco de dados ou coleção.
* `enableProfiler`: usuário pode mudar o nível do profiler num banco de dados.
* `grantRole`: usuário pode conceder um papel para qualquer usuário de qualquer banco de dados.
* `killCursors`: usuário pode derrubar o cursor de uma coleção.
* `revokeRole`: usuário pode remover um papel de qualquer usuário de qualquer banco de dados.
* `unlock`: usuário pode desbloquear uma instância do `mongod` para escrita (geralmente usado após uma operação de backup).
* `viewRole`: usuário pode ver informações de qualquer papel num banco de dados.
* `viewUser`: usuário pode ver informações de qualquer usuário num banco de dados.

### Ações de Gerenciamento de Deployment
* `authSchemaUpgrade`: usuário pode atualizar o esquema de autenticação e autorização de um cluster.
* `cleanupOrphaned`: usuário pode remover dados que não pertencem a um shard.
* `cpuProfiler`: usuário pode habilitar o profile da CPU de um cluster.
* `inprog`: usuário pode verificar operações ativas e pendentes num cluster.
* `invalidateUserCache`: usuário pode 'limpar' da memória (cache) informações de usuário, incluindo credenciais e papéis num cluster.
* `killop`: usuário pode matar operações num cluster.
* `planCacheRead`: usuário pode listar planos de query em cache num banco de dados ou coleção.
* `planCacheWrite`: usuário pode remover os planos de query em cache num banco de dados ou coleção.
* `storageDetails`: usuário pode executar o comando `storageDetails` num banco de dados ou coleção.

### Ações de Replicação
* `appendOplogNote`: usuário pode adicionar notas no oplop num cluster.
* `replSetConfigure`: usuário pode configurar uma replica set num cluster.
* `replSetGetStatus`: usuário pode ver o status de uma replica set num cluster.
* `replSetHeartbeat`: usuário pode executaro comando `replSetHeartbeat` num cluster.
* `replSetStateChange`: usuário pode mudar o estado de uma replica set num cluster.
* `resync`: usuário pode forçar a resincronização de uma instância 'escrava' do `mongod` num cluster.

### Ações de Sharding
* `addShard`: usuário pode adicionar um shard num cluster.
* `enableSharding`: usuário pode habilitar o sharding de um banco de dados e fragmentar (shard) uma coleção.
* `flushRouterConfig`: usuário pode executar o commando `flushRouterConfig` num cluster.
* `getShardMap`: usuário pode executar o commando `getShardMap` num cluster.
* `getShardVersion`: usuário pode executar o commando `getShardVersion` num banco de dados.
* `listShards`: usuário pode executar o commando `listShard` num cluster.
* `moveChunk`: usuário pode executar os comandos ` moveChunk` e `movePrimary` num banco de dados ou coleção.
* `removeShard`: usuário pode remover um shard num cluster.
* `shardingState`: usuário pode executar o commando `shardingState` num cluster.
* `splitChunk`: usuário pode executar o commando `splitChunk` num banco de dados ou coleção.
* `splitVector`: usuário pode executar o commando `splitVector` num banco de dados ou coleção.

### Ações de Administração de Servidor
* `applicationMessage`: 
* `closeAllDatabases`: 
* `collMod`: 
* `compact`: 
* `connPoolSync`: 
* `convertToCapped`: 
* `dropDatabase`: 
* `dropIndex`: 
* `fsync`: 
* `getParameter`: 
* `hostInfo`: 
* `logRotate`: 
* `reIndex`: 
* `renameCollectionSameDB`: 
* `repairDatabase`: 
* `setParameter`: 
* `shutdown`: 
* `touch`: 