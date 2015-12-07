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

## Explique cada papel listado em Cluster Administration Roles.
### clusterAdmin
É o papel com o maior nível de acesso de gerenciamento de um cluster. Este papel combina os privilégios dados pelos papeis **clusterManager**, **clusterMonitor** e **hostManager**. Além disso, este papel também permite executar a ação `dropDatabase`.

### clusterManager
Este papel permite executar ações de gerenciamento e monitoramento em um cluster. Um usuário com esse papel pode acessar os bancos de dados `local` e `config`, que são usados para replicação e sharding, respectivamente.

### clusterMonitor


## Explique todas as ações de privilégio listadas em Privilege Actions.
