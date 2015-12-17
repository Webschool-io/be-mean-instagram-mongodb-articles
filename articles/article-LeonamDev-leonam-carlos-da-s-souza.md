# MongoDb - Artigo sobre Autenticação no MongoDb

**Autor:** Leonam Carlos da S Souza

**Data** 11/12/2015 00:09

## Qual a diferença entre Autenticação e Autorização?

A autenticação é a verificação das credenciais da tentativa de conexão. Esse processo consiste no envio de credenciais do cliente de acesso remoto para o servidor de acesso remoto em um formato de texto sem formatação ou em formato criptografado usando um protocolo de autenticação. 

A autorização é a verificação de que a tentativa de conexão é permitida. A autorização ocorre após a autenticação bem-sucedida. 



## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
```
Para criar um usuário administrador:

use admin
db.createUser(
  {
    user: "LeonamAdmin",
    pwd: "admin123",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)

Para criar um usuário comum:

use admin
db.createUser(
  {
    user: "LeonamComum",
    pwd: "leonam123",
    roles: [{db: 'admin'}]
  }
)

```

## Explique cada papel listado em Cluster Administration Roles.

clusterAdmin - Fornece o maior acesso de gerenciamento de cluster. Este papel combina os privilégios concedidos pelos papéis clusterManager , clusterMonitor , e hostManager . Além disso , o papel proporciona a acção de dropDatabase .

clusterManager - Fornece gerenciamento e ações de monitoramento no cluster. Um usuário com essa função pode acessar a configuração e bancos de dados locais , que são usados ​​em sharding e replicação , respectivamente.

clusterMonitor - Fornece acesso somente leitura para ferramentas de monitoramento , como o agente de monitoramento Gerente MongoDB Cloud.

hostManager - Fornece a capacidade de monitorar e gerenciar servidores.



## Explique todas as ações de privilégio listadas em Privilege Actions.

Privilege Actions¶


Ações de privilégio definem as operações que um usuário pode executar em um recurso. Um privilégio MongoDB dispõe de um recurso e as ações permitidas. Esta página lista as ações disponíveis agrupadas por propósito comum.

MongoDB fornece funções integrados com combinação de pré-definidas de recursos e ações permitidas. Para listas de ações concedidas, consulte Funções Built-In. Para definir funções personalizadas, consulte Criar uma Função Definida pelo usuário.

Query and Write Actions

find
O usuário pode executar o método db.collection.find (). Aplicar esta ação para banco de dados ou de cobrança de recursos.

insert
O usuário pode executar o comando de inserção. Aplicar esta ação para banco de dados ou de cobrança de recursos.

remove
O usuário pode executar o método db.collection.remove (). Aplicar esta ação para banco de dados ou de cobrança de recursos.

update
O usuário pode executar o comando update. Aplicar esta ação para banco de dados ou de cobrança de recursos.
