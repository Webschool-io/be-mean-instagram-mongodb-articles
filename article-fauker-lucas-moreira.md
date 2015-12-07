# MongoDb - Artigo sobre Autenticação no MongoDb

**Autor:** Lucas da Silva Moreira

**Data** Date.now() //em timestamp

## Qual a diferença entre Autenticação e Autorização?

**Autenticação**: É a verificação das credenciais de um determinado usuário em uma tentativa de conexão. Geralmente envolve um usuário e uma senha, mas pode incluir outros métodos de validação, por exemplo: Certificado Digital etc.

**Autorização**: A autorização serve para verificar se um usuário que já passou pela autenticação e foi bem-sucedido tem acesso a determinadas funcionalidades do sistema.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Para criar um usuário administrador:

```
use admin
db.createUser(
  {
    user: "UsuarioAdmin",
    pwd: "admin123",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
```

Para criar um usuário comum:

```
use admin
db.createUser(
  {
    user: "UsuarioComum",
    pwd: "comum123",
    roles: [{db: 'admin'}]
  }
)
```

## Explique cada papel listado em Cluster Administration Roles.

**Cluster Admin**: Permite acesso total so gerenciamento de cluster. Essa role combina os privilégios das seguintes roles: *clusterManager*, *clusterMonitor* e *hostManager*. E como um adicional, essa função permite o uso de *dropDatabase*.

**Cluster Manager**: Permite gerenciamento e monitoração de ações no cluster. O usuário com essa função pode acessar os bancos de dados *config* e *local*, os quais são usados em sharding e replicações, respectivamente.

**Cluster Monitor**: Permite apenas leitura para monitorar ferramentas como *MongoDB Cloud Manager*.

**Host Manager**: Permite funções de monitoração e administração de servidores de banco de dados.

## Explique todas as ações de privilégio listadas em Privilege Actions.

`Privilege Actions` definem as operações que um usuário pode realizar em determinado recurso. Em nosso caso, no MongoDB.

* Query and Write Actions
	* find: O usuário pode executar o método `db.collection.find()`. Pode ser aplicado para `database` ou `collection`.
	* insert: O usuário pode executar o comando `insert`. Pode ser aplicado para `database` ou `collection`.
	* remove: O usuário pode executar o método `db.collection.remove()`. Pode ser aplicado para `database` ou `collection`.
	* update: O usuário pode executar o comando `update`. Pode ser aplicado para `database` ou `collection`.

* Database Management Actions
	* changeCustomData: O usuário pode alterar uma informação aleatória de qualquer usuário da `database`. Pode ser aplicado para `database`.
	* changeOwnCustomData: Usuários podem alterar as alterações deles próprios. Pode ser aplicado para `database`.
	* changePassword: O usuário poderá alterar o `password` dele na `database`.	Pode ser aplicado para `database`.
	* createCollection: O usuário pode executar o método `db.createCollection()`. Pode ser aplicado para `database` ou `collection`.
	* createIndex: Permite acesso para o método `db.collection.createIndex()` e para o comando `createIndexes`. Pode ser aplicado para `database` ou `collection`.
	* createRole: O usuário pode criar uma nova `role` na `database`. Pode ser aplicado para `database`.
	* createUser: O usuário pode criar novos usuários para a `database`. Pode ser aplicado para `database`.
	* dropCollection: O usuário pode executar o método `db.collection.drop()`. Pode ser aplicado para `database` ou `collection`.
	* dropRole: O usuário pode deletar qualquer `role` na `database`. Pode ser aplicado para `database`.
	* dropUser: Usuário pode remover qualquer usuário da `database`. Pode ser aplicado para `database`.
	* emptycapped: O usuário poderá executar o comando `emptycapped`. Pode ser aplicado para `database` ou `collection`.
	* enableProfiler: O usuário poderá executar o método `db.setProfilingLevel()`. Pode ser aplicado para `database` ou `collection`.
	* grantRole: O usuário poderá conceder qualquer `role` na `database` para qualquer usuário de qualquer `database` no sistema. Pode ser aplicado para `database`.