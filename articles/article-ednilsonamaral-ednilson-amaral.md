# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Ednilson Amaral
**Data:** 04/12/2015 às 23:41


## Qual a diferença entre Autenticação e Autorização?  

Tentarei responder essa pergunta dando um exemplo. Quando vamos logar em um determinado sistema com nosso usuário e senha, enviamos ao server uma solicitação de **autenticação**, para que seja feito um processo de verificação da nossa identididade. Assim sendo, nossa identidade identificada, então, receberemos **autorização** ou não para realizarmos determinadas tarefas.  

Mesmo **autenticação** e **autorização** sejam ligadas entre si, elas são diferentes. De uma forma resumida, **autenticação** irá verificar a identidade de um usuário, enquanto a **autorização** vai determinar o acesso desse usuário e quais recursos e operações são concedidas a ele.


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum  

Para criarmos usuários no MongoDB, devemos utilizar a função `db.createUser()`. A sintaxe básica para a criação de um novo usuário no MongoDB é a seguinte:  

```  
{ user: "<name>",  
  pwd: "<cleartext password>",  
  customData: { <any information> },  
  roles: [  
    { role: "<role>", db: "<database>" },  
  ]  
}  
```  

Onde iremos informar o nome, senha, algumas informações adicionas no `customData`, se quisermos ou não. Os campos de nome, senha e roles são obrigatórios, já o campo customData é opcional. Em `roles` iremos informar quais os privilégios para esse usuário e em qual `database` ele irá ter tais privilégios.

### Criando Usuário Comum  

Se passarmos durante o comando `db.createUser()` sem nenhum `role`, informando apenas o nome do database que o usuário vai ter autorização de acesso, então, o MongoDB vai retornar um erro, como visto no trecho a seguir:  

```  
admin> db.createUser(  
		{  
			user: "comum",  
			pwd: "comum123",  
			roles: [{db: "admin"}]  
		}  
	)  

2015-12-03T21:57:07.174-0200 E QUERY    [thread1] Error: couldn't add user: Missing expected field "role" :  
_getErrorWithCode@src/mongo/shell/utils.js:23:13  
DB.prototype.createUser@src/mongo/shell/db.js:1225:11  
@(shell):1:1  
```


O erro acima diz que é necessário passar algum valor no campo `role`. Então, devemos criar um usuário comum, do tipo apenas leitura da seguinte forma:  

```  
admin> db.createUser(  
		{  
			user: "comum",  
			pwd: "comum123",  
			roles: [{role: "read", db: "admin"}]  
		}  
	)  

Successfully added user: {  
  "user": "comum",  
  "roles": [  
    {  
      "role": "read",  
      "db": "admin"  
    }  
  ]  
}  
```


### Criando Usuário Administrador  

Para criarmos um usuário administrador o processo é o mesmo. Só muda o array role. No exemplo a seguir utilizo o `userAdminAnyDatabase` que tem os mesmos privilégios do role `userAdmin`. A diferença entre eles é que o `userAdminAnyDatabase` é aplicável a todos os bancos em um cluster.

```  
admin> db.createUser(  
		{  
			user: "userFoda",  
			pwd: "foda321",  
			roles: [{role: "userAdminAnyDatabase", db: "admin"}]  
		}  
	)  

Successfully added user: {  
  "user": "userFoda",  
  "roles": [  
    {  
      "role": "userAdminAnyDatabase",  
      "db": "admin"  
    }  
  ]  
}  
```  

Então, assim, listamos os usuários criados:  

```  
admin> db.system.users.find()  
{  
  "_id": "admin.TesteAdmin",  
  "user": "TesteAdmin",  
  "db": "admin",  
  "credentials": {  
    "SCRAM-SHA-1": {  
      "iterationCount": 10000,  
      "salt": "GnomEunpcaUNPDR5X4ztuw==",  
      "storedKey": "h+t/LNSbi621xrZIrVKDCXVhD8c=",  
      "serverKey": "qh4JmtNei6712TTl1xDcTsLI8Iw="  
    }  
  },  
  "roles": [  
    {  
      "role": "userAdminAnyDatabase",  
      "db": "admin"  
    }  
  ]  
}  
{  
  "_id": "admin.comum",  
  "user": "comum",  
  "db": "admin",  
  "credentials": {  
    "SCRAM-SHA-1": {  
      "iterationCount": 10000,  
      "salt": "JDqV1XQcJ9PVKd6rPNKPwQ==",  
      "storedKey": "5UQNVA94E++61ShBzOpXgVc5iHg=",  
      "serverKey": "hgI0LAWXL5jLoY9oGBygbx0kyuE="  
    }  
  },  
  "roles": [  
    {  
      "role": "read",  
      "db": "admin"  
    }  
  ]  
}  
{  
  "_id": "admin.userFoda",  
  "user": "userFoda",  
  "db": "admin",  
  "credentials": {  
    "SCRAM-SHA-1": {  
      "iterationCount": 10000,  
      "salt": "8PSlbiYYVXbe1K2FEBbEZQ==",  
      "storedKey": "QAUumSjFFjakYkgoFV58Z7+P3k8=",  
      "serverKey": "Ev/kr3Pymj+HBqoqap+/KTXUmK4="  
    }  
  },  
  "roles": [  
    {  
      "role": "userAdminAnyDatabase",  
      "db": "admin"  
    }  
  ]  
}  
Fetched 3 record(s) in 7ms  
```


## Explique cada papel listado em Cluster Administration Roles

Caso tenhamos um cluster e queiramos ter um usuário com privilégios de atuar em vários databases, não precisamos ter um usuário para cada servidor. Para resolver isso, temos o **cluster administrator roles**. Eles não são limitados a réplicas e shardings.  

Os roles possíveis para os privilégios em um usuários admin de um cluster são:  

* `clusterAdmin`: É o maior privilégio para usuários admini em clusters. Inclui todos os privilégios que serão listados e explicados abaixo. Também, possui o privilégio da ação de `dropDatabase`.  
* `clusterManager`: Fornece o gerenciamento e monitoramento das ações no cluster. Um usúário com esse `role` pode acessar as configurações de databases locais, que são usadas em réplicas e shardings. Como um todo, são fornecidas ações desde `addShard` até `resync`. Suas ações fornecidas para todos os databases no cluster vão desde `enableSharding` até `splitVector`. Além, é claro, de `insert`, `remove` e `update`. Basicamente, aqui o cara faz de um tudo no cluster.  
* `clusterMonitor`: Fornece **apenas** leitura (`read-only`) para ferramentas de monitoramento no cluster. Um exemplo de ferramenta para monitoramento é o [MongoDB Cloud Manager](https://www.mongodb.com/cloud). Suas ações vão desde `connPoolStats`, `netstat` à `serverStatus` e `shardingState`. De uma forma resumidade, aqui o cara só vai ficar olhando o que está sendo feito no cluster.  
* `hostManager`: Já com este `role` é possível monitorar e gerenciar os servers.


## Explique todas as ações de privilégio listadas em Privilege Actions  

As *privilege actions* ou ações de privilégio definem quais são as operações que determinado usuário pode executar ou não através de algum recurso.  

No MongoDB um privilégio dispõe de um recurso e as ações que são permitidas para esses recursos. Além disso, o MongoDB fornece algumas funções integradas com uma combinação de recursos pré-definidos e ações permitidas, como veremos abaixo.


### Ações de Escrita  

* `find`: para buscar documentos em uma determinada coleção;  
* `insert`: para inserir novos documentos em uma determina coleção;  
* `remove`: para remover documentos em uma determinada coleção;  
* `update`: para atualizar documentos em uma determinada coleção.


### Ações de Gerenciamento de Databases  

* `changeCustomData`: aqui o usuário pode alterar as informações contidas no `custom information` de qualquer usuário da database que ele possui privilégios;  
* `changeOwnCustomData`: aqui o usuário pode alterar suas próprias informações de forma personalizada;  
* `changeOwnPassword`: aqui o usuário pode alterar a sua senha de acesso;  
* `changePassword`: aqui o usuário pode alterar a senha de acesso de qualquer usuário da database que ele possui privilégios;  
* `createCollection`: aqui o usuário pode criar uma nova coleção na database que possui privilégios;  
* `createIndex`: aqui o usuário pode criar uma nova index na database que possui privilégios;  
* `createRole`: aqui o usuário pode criar novas roles na database que possui privilégios;  
* `createUser`: pode criar novos usuários na database que possui privilégios;  
* `dropCollection`: pode deletar uma coleção na database que possui privilégios;  
* `dropRole`: pode deletar uma ou mais roles na database que possui privilégios;  
* `dropUser`: pode deletar um usuário na database que possui privilégios;  
* `emptycapped`: aqui o usuário é capaz de remover todos os documentos de uma determinada coleção utilizando o comando `emptycapped`;  
* `enableProfiler`: é possível utilizar o comando `db.setProfilingLevel()`, que nada mais é que modificar o nível de perfil do usuário atual para capturar dados sobre o seu desempenho;  
* `grantRole`: aqui é possível alterar qualquer role no database para qualquer usuário de qualquer databade do sistema;  
* `killCursors`: o usuário pode matar os cursores em determinadas coleções;  
* `revokeRole`: o usuário pode remover qualquer role de qualquer usuário de qualquer database no sistema;  
* `unlock`: é possível utilizar o comando `db.fsyncUnlock()`, que nada mais é que desbloquear um serviço para que seja possível escrever e/ou inverter o funcionamento de uma operação no MongoDB. Normalmente é utilizada em uma sequência de operações de backups de databases;  
* `viewRole`: o usuário visualiza as informações de qualquer role na database que possui privilégios;  
* `viewUser`: o usuário visualiza as informações de qualquer usuário na database que possui privilégios.


### Ações de Gerenciamento de Deployment (Implantação)  

* `authSchemaUpgrade`: o usuário pode dar um upgrade em processos de sistemas existentes que usam autenticação e autorização entre as versões 2.4 à 2.6 e de 2.6 à 3.0;  
* `cleanupOrphaned`: é possível utilizar o comando `cleanupOphaned` no cluster. Esse comando faz com que seja excluído de um shard documentos órfãos cujo seus valores não pertençam a esse shard;  
* `cpuProfiler`: o usuário pode ativar e utilizar o CPU profiler no cluster;  
* `inprog`: o usuário pode utilizar o comando `db.currentOp()` que irá retornar as operações ativas e pendentes no cluster;  
* `invalidateUserCache`: é possível liberar informações do usuário do cache na memória RAM, incluindo a remoção de credenciais e papéis de cada usuário. Com isso é possível limpar o cache do usuário em qualquer momento;  
* `killop`: é possível finalizar uma determinada operação, sendo que deve ser especificada pelo seu ID;  
* `planCacheRead`: o usuário pode visualizar os planos de list e querys para o cache na database ou coleções;  
* `planCacheWrite`: o usuário pode escrever e/ou limpar e/ou modificar os planos de list e querys para cache nas coleções e databases;  
* `storageDetails`: o usuário pode utilizar o comando `storageDetails` nas coleções ou databases que possui privilégios.


### Ações de Replications  

* `appendOplogNote`: o usuário pode adicionar algumas notas ao `oplog` no cluster;  
* `replSetConfigure`: o usuário pode configurar uma `replica_set` no cluster;  
* `replSetGetStatus`: o usuário pode ver o status atual da replica no cluster;  
* `replSetHeartbeat`: é possível utilizar o comando `replSetHeartbeat`, que serve para determinadas funcionalidades em um conjunto de replicas;  
* `replSetStateChange`: o usuário pode alterar/gerenciar o estado atual da réplica no cluster;  
* `resync`: é possível utilizar o comando `resync` no cluster, para resincronizar a replica primária e secundária (master - slave), nunca replicas em conjunto.


### Ações de Sharding  

* `addShard`: é possível adicionar um shard no cluster;  
* `enableSharding`: o usuário consegue ativar um shard na database ou coleções em uso;  
* `flushRouterConfig`: é possível limpar as informaçoẽs do cluster que estão em cache e recarrega os metadados do cluster;  
* `getShardMap`: é um comando interno que suporta algumas funcionalidades de sharding;  
* `getShardVersion`: é um comando interno que suporta algumas funcionalidades de sharding;  
* `listShards`: retorna a lista de shards configurados;  
* `moveChunk`: é um comando de administração interno, capaz de mover os chunks entre os shards;  
* `removeShard`: remove um shard do cluster;  
* `shardingState`: é um comando de administração que mostra o estado dos shardings no cluster;  
* `splitChunk`: é um comando de administração interno, capaz de dividir os chunks das coleções ou databases;  
* `splitVector`: é um comando interno que suporta operações de metadados nos cluster que possuem shards.


### Ações dos Administração de Servidores  

* `applicationMessage`: é possível postar uma mensagem personalizada para o log de auditoria, através do comando `logApplicationMessage`;  
* `closeAllDatabases`: o usuário pode fechar todas as databases no cluster;  
* `collMod`: é possível adicionar um conjunto de _flags_ para modificar o comportamento do MongoDB;  
* `compact`: é possível regravar e desfragmentar todos os dados e indices em uma coleção do database;  
* `connPoolSync`: é um comando interno aplicável no cluster;  
* `convertToCapped`: é possível converter uma coleção `not-capped` para `capped` dentro da database;  
* `dropDatabase`: é possível apagar um database;  
* `dropIndex`: é possível apagar um indice de uma determinada coleção no database;  
* `fsync`: força o serviço `mongod` para que dê um _flush_ em todas as escritas pendentes na camada de armazenamento em disco, sendo aplicável no cluster;  
* `getParameter`: é um comando de administração interno capaz de recuperar o valor das opções configuradas anteriormente, aplicável no cluster;  
* `hostInfo`: retorna informações sobre o server que o MongoDB está rodando atualmente, aplicável no cluster;  
* `logRotate`: é um comando de administração interno capaz de fazer um "gira gira" nos logs do MongoDB para que o mesmo não concentre os logs em um único arquivo, assim não consumindo muito espaço em disco;  
* `reIndex`: apaga todos os indices de um coleção e as cria novamente;  
* `renameCollectionSameDB`: renomeia uma coleção no database em uso;  
* `repairDatabase`: checa e repara um database que apresente inconsistência no armazenamento dos dados;  
* `setParameter`: é possível alterar alguns parâmetros no cluster;  
* `shutdown`: limpa todos os databases e encerra seus processos;  
* `touch`: carrega os dados da camada de armazenamento de dados em memória.


### Ações de Diagnósticos  


* `collStats`: retorna uma variedade de estatísticas de armazenamento de uma determina coleção;  
* `connPoolSync`: é um comando interno aplicável no cluster;  
* `cursorInfo`: retorna algumas informações a respeito do cursor;  
* `dbHash`: é um comando com suporte há algumas configurações no servidor;  
* `dbStats`: retorna uma variedade de estatísticas sobre o database em uso;  
* `diagLogging`: é um comando adicional capaz de retornar dados adicionais para fins de diagnósticos;  
* `getCmdLineOpts`: é possível utilizar o comando `getCmdLineOpts` no cluster;  
* `getLog`: é possível utilizar o comando `getLog` no cluster;  
* `indexStats`: na versão 3.0 do MongoDB esse comando foi removido, porém, anteriormente, era possível retornar algumas estatísticas sobre os indices de determinadas coleções do database;  
* `listDatabases`:  lista os databases com algumas estatísticas básicas;  
* `listCollections`: lista todas as coleções do database com algumas estatísticas básicas;  
* `listIndexes`: lista todos os indices de determina coleção com algumas estatísticas básicas;  
* `netstat`: é possível utilizar o comando `netstat`, aplicável ao mongos;  
* `serverStatus`: retorna algumas estatísticas sobre o status do servidor;  
* `validate`: verifica as estruturas de nomes das coleções e indices, retorna uma representação disso em disco;  
* `top`: é um comando de administração interno capaz de retornar estatísticas de uso para cada coleção do database.


### Ações Internas  

* `anyAction`: permite qualquer ação em um recurso especifico;  
* `internal`: permite ações internas em circunstâncias excepcionais.