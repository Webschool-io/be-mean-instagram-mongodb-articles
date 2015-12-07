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
    roles: []
  }
)
```

## Explique cada papel listado em Cluster Administration Roles.

*Cluster Admin*: Permite acesso total so gerenciamento de cluster. Essa role combina os privilégios das seguintes roles: *clusterManager*, *clusterMonitor* e *hostManager*. E como um adicional, essa função permite o uso de *dropDatabase*.

*Cluster Manager*: Permite gerenciamento e monitoração de ações no cluster. O usuário com essa função pode acessar os bancos de dados *config* e *local*, os quais são usados em sharding e replicações, respectivamente.

*Cluster Monitor*: Permite apenas leitura para monitorar ferramentas como *MongoDB Cloud Manager*.

*Host Manager*: Permite funções de monitoração e administração de servidores de banco de dados.

## Explique todas as ações de privilégio listadas em Privilege Actions.