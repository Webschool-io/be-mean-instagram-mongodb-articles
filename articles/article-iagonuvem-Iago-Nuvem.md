# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor**: Iago Nuvem | **Data**: 09/12/2015 às 11:14


## Qual a diferença entre Autenticação e Autorização?

**Autenticação** : é o processo que verifica a identidade de um usuário. 
**Autorização** : é a determinação de acesso do usuário autenticado e quais recursos e operações serão permitidos a ele.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum

Para criarmos usuários no MongoDB, devemos utilizar a função :
- ```db.createUser()``` (https://docs.mongodb.org/manual/reference/method/db.createUser/)
Cuja sintaxe é:

```js
{ user: "<name>",  
  pwd: "<cleartext password>",  
  customData: { <any information> },  // Opcional
  roles: [  
    { 
    role: "<role>", // Define os Privilégios do Usuário
    db: "<database>" // Define em qual database o usuário possuirá estes privilégios
    },  
  ]  
}  
```

### Criando Usuário Administrador

Para Criarmos um usuário administrador, apenas deve-se adicionar a permissão *userAdminAnyDatabase* ao **role**. 
O role *userAdminAnyDatabase* tem os mesmos privilégios do role *userAdmin*, a diferença entre eles é que o *userAdminAnyDatabase* é aplicável a todos os bancos em um cluster.

```js
db.createUser(  
        {  
            user: "admin",  
            pwd: "admin123",  
            roles: [{role: "userAdminAnyDatabase", db: "admin"}]  
        }  
    )  

Successfully added user: {  
  "user": "admin",  
  "roles": [  
    {  
      "role": "userAdminAnyDatabase",  
      "db": "admin"  
    }  
  ]  
}  

```

### Criando Usuário Comum

Para Criarmos um usuário comum, deve-se **APENAS** colocar o **role** como *read* (somente leitura), é importante que o atributo **role** esteja preenchido, caso contrário o MongoDB vai retornar o seguinte erro: 

```js
2015-12-03T21:57:07.174-0200 E QUERY    [thread1] Error: couldn't add user: Missing expected field "role" :  
_getErrorWithCode@src/mongo/shell/utils.js:23:13  
DB.prototype.createUser@src/mongo/shell/db.js:1225:11  
@(shell):1:1  
```

**Error: couldn't add user: Missing expected field "role"** : ou seja, é **OBRIGATÓRIO** o preenchimento do campo role.

Veja abaixo um exemplo de como criar um usuário comum.

```js
db.createUser(  
        {  
            user: "user",  
            pwd: "user123",  
            roles: [{role: "read", db: "admin"}]  
        }  
    )  

Successfully added user: {  
  "user": "user",  
  "roles": [  
    {  
      "role": "read",  
      "db": "admin"  
    }  
  ]  
}  
```
Agora vamos listar os usuários pra ver se deu certo a porra toda: 

```js
db.system.users.find()  
{  
  "_id": "admin.admin",  
  "user": "admin",  
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
  "_id": "admin.user",  
  "user": "user",  
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

Fetched 2 record(s) in 4ms  
```
## Explique cada papel listado em Cluster Administration Roles


Administrator roles são os privilégios de um usuário administrador de um cluster, que não são limitados a réplicas e shardings. que pode atuar em vários databases. Os roles possíveis para os privilégios em um usuários admin de um cluster são:

- ```clusterAdmin```: É o maior privilégio para usuários admin em clusters, e único que possui o privilégio da ação de dropDatabase.
- ```clusterManager```: Fornece o gerenciamento e monitoramento das ações no cluster. Pode acessar as configurações de databases locais, que são usadas em réplicas e shardings. Basicamente, pode gerenciar todo o cluster exceto a ação *dropDatabase*.
- ```clusterMonitor```: Fornece apenas leitura (read-only) para ferramentas de monitoramento no cluster. Fala nada, só observa
- ```hostManager```: Já com este role é possível monitorar e gerenciar os servers.

## Explique todas as ações de privilégio listadas em Privilege Actions

As privilege actions ou ações de privilégio definem quais são as operações que determinado usuário pode executar ou não através de algum recurso.


### Ações de Escrita

- ```find```: para buscar documentos em uma determinada coleção;
- ```insert```: para inserir novos documentos em uma determina coleção;
- ```remove```: para remover documentos em uma determinada coleção;
- ```update```: para atualizar documentos em uma determinada coleção.

### Ações de Gerenciamento de Databases

- ```changeCustomData```: Utilizada para alterar as informações contidas no custom information de qualquer usuário da database que ele possui privilégios;
- ```changeOwnCustomData```: Utilizada para alterar suas próprias informações de forma personalizada;
- ```changeOwnPassword```: Utilizada para alterar a sua senha de acesso;
- ```changePassword```: Utilizada para alterar a senha de acesso de qualquer usuário da database que ele possui privilégios;
- ```createCollection```: Utilizada para criar uma nova coleção na database que possui privilégios;
- ```createIndex```: Permite ao usuário criar uma nova index na database que possui privilégios;
- ```createRole```: Utilizada para criar novas roles na database que possui privilégios;
- ```createUser```: Utilizada para criar novos usuários na database que possui privilégios;
- ```dropCollection```: Utilizada para deletar uma coleção na database que possui privilégios;
- ```dropRole```: Utilizada para deletar uma ou mais roles na database que possui privilégios;
- ```dropUser```: Utilizada para deletar um usuário na database que possui privilégios;
- ```emptycapped```: Utilizada para remover todos os documentos de uma determinada coleção;
- ```enableProfiler```: é possível utilizar o comando db.setProfilingLevel(), que nada mais é que modificar o nível de perfil do usuário atual para capturar dados sobre o seu desempenho;
- ```grantRole```: Utilizada para alterar qualquer role no database para qualquer usuário de qualquer databade do sistema;
- ```killCursors```: Utilizada matar os cursores em determinadas coleções;
- ```revokeRole```: Utilizada para remover qualquer role de qualquer usuário de qualquer database no sistema;
- ```unlock```: é possível utilizar o comando db.fsyncUnlock(), que nada mais é que desbloquear um serviço para que seja possível escrever e/ou inverter o funcionamento de uma operação no MongoDB. Normalmente é utilizada em uma sequência de operações de backups de databases;
- ```viewRole```: Utilizada para visualizar as informações de qualquer role na database que possui privilégios;
- ```viewUser```: Utilizada para visualizar as informações de qualquer usuário na database que possui privilégios.

### Ações de Gerenciamento de Deployment (Implantação)

- ```authSchemaUpgrade```: o usuário pode dar um upgrade em processos de sistemas existentes que usam autenticação e autorização entre as versões 2.4 à 2.6 e de 2.6 à 3.0;
- ```cleanupOrphaned```: é possível utilizar o comando cleanupOphaned no cluster. Esse comando faz com que seja excluído de um shard documentos órfãos cujo seus valores não pertençam a esse shard;
- ```cpuProfiler```: o usuário pode ativar e utilizar o CPU profiler no cluster;
- ```inprog```: o usuário pode utilizar o comando db.currentOp() que irá retornar as operações ativas e pendentes no cluster;
- ```invalidateUserCache```: é possível liberar informações do usuário do cache na memória RAM, incluindo a remoção de credenciais e papéis de cada usuário. Com isso é possível limpar o cache do usuário em qualquer momento;
- ```killop```: Utilizada para finalizar uma determinada operação, sendo que deve ser especificada pelo seu ID;
- ```planCacheRead```: o usuário pode visualizar os planos de list e querys para o cache na database ou coleções;
- ```planCacheWrite```: o usuário pode escrever e/ou limpar e/ou modificar os planos de list e querys para cache nas coleções e databases;
- ```storageDetails```: o usuário pode utilizar o comando storageDetails nas coleções ou databases que possui privilégios.

### Ações de Replications

- ```appendOplogNote```: Utilizada para adicionar algumas notas ao oplog no cluster;
- ```replSetConfigure```: Utilizada para configurar uma replica_set no cluster;
- ```replSetGetStatus```: Utilizada para ver o status atual da replica no cluster;
- ```replSetHeartbeat```: é possível utilizar o comando replSetHeartbeat, que serve para determinadas funcionalidades em um conjunto de replicas;
- ```replSetStateChange```: Utilizada para alterar/gerenciar o estado atual da réplica no cluster;
- ```resync```: é possível utilizar o comando resync no cluster, para resincronizar a replica primária e secundária (master - slave), nunca replicas em conjunto.

### Ações de Sharding

- ```addShard```: Utilizada para adicionar um shard no cluster;
- ```enableSharding```: Utilizada para ativar um shard na database ou coleções em uso;
- ```flushRouterConfig```: Utilizada para limpar as informações do cluster que estão em cache e recarrega os metadados do cluster;
- ```getShardMap```: é um comando interno que suporta algumas funcionalidades de sharding;
- ```getShardVersion```: é um comando interno que suporta algumas funcionalidades de sharding;
- ```listShards```: retorna a lista de shards configurados;
- ```moveChunk```: é um comando de administração interno, capaz de mover os chunks entre os shards;
- ```removeShard```: remove um shard do cluster;
- ```shardingState```: é um comando de administração que mostra o estado dos shardings no cluster;
- ```splitChunk```: é um comando de administração interno, capaz de dividir os chunks das coleções ou databases;
- ```splitVector```: é um comando interno que suporta operações de metadados nos cluster que possuem shards.

### Ações dos Administração de Servidores

- ```applicationMessage```: Utilizado para postar uma mensagem personalizada para o log de auditoria, através do comando logApplicationMessage;
- ```closeAllDatabases```: o usuário pode fechar todas as databases no cluster;
- ```collMod```: Utilizado para adicionar um conjunto de flags para modificar o comportamento do MongoDB;
- ```compact```: Utilizado para regravar e desfragmentar todos os dados e indices em uma coleção do database;
- ```connPoolSync```: é um comando interno aplicável no cluster;
- ```convertToCapped```: Utilizada para converter uma coleção not-capped para capped dentro da database;
- ```dropDatabase```: Utilizada para apagar um database;
- ```dropIndex```: é possível apagar um indice de uma determinada coleção no database;
- ```fsync```: força o serviço mongod para que dê um flush em todas as escritas pendentes na camada de armazenamento em disco, sendo aplicável no cluster;
- ```getParameter```: é um comando de administração interno capaz de recuperar o valor das opções configuradas anteriormente, aplicável no cluster;
- ```hostInfo```: retorna informações sobre o server que o MongoDB está rodando atualmente, aplicável no cluster;
- ```logRotate```: é um comando de administração interno capaz de fazer um "gira gira" nos logs do MongoDB para que o mesmo não concentre os logs em um único arquivo, assim não consumindo muito espaço em disco;
- ```reIndex```: apaga todos os indices de um coleção e as cria novamente;
- ```renameCollectionSameDB```: renomeia uma coleção no database em uso;
- ```repairDatabase```: checa e repara um database que apresente inconsistência no armazenamento dos dados;
- ```setParameter```: é possível alterar alguns parâmetros no cluster;
- ```shutdown```: limpa todos os databases e encerra seus processos;
- ```touch```: carrega os dados da camada de armazenamento de dados em memória.

### Ações de Diagnósticos

- ```collStats```: retorna uma variedade de estatísticas de armazenamento de uma determina coleção;
- ```connPoolSync```: é um comando interno aplicável no cluster;
- ```cursorInfo```: retorna algumas informações a respeito do cursor;
- ```dbHash```: é um comando com suporte há algumas configurações no servidor;
- ```dbStats```: retorna uma variedade de estatísticas sobre o database em uso;
- ```diagLogging```: é um comando adicional capaz de retornar dados adicionais para fins de diagnósticos;
- ```getCmdLineOpts```: é possível utilizar o comando getCmdLineOpts no cluster;
- ```getLog```: é possível utilizar o comando getLog no cluster;
- ```indexStats```: na versão 3.0 do MongoDB esse comando foi removido, porém, anteriormente, era possível retornar algumas estatísticas sobre os indices de determinadas coleções do database;
- ```listDatabases```: lista os databases com algumas estatísticas básicas;
- ```listCollections```: lista todas as coleções do database com algumas estatísticas básicas;
- ```listIndexes```: lista todos os indices de determina coleção com algumas estatísticas básicas;
- ```netstat```: é possível utilizar o comando netstat, aplicável ao mongos;
- ```serverStatus```: retorna algumas estatísticas sobre o status do servidor;
- ```validate```: verifica as estruturas de nomes das coleções e indices, retorna uma representação disso em disco;
- ```top```: é um comando de administração interno capaz de retornar estatísticas de uso para cada coleção do database.

### Ações Internas

- ```anyAction```: permite qualquer ação em um recurso especifico;
- ```internal```: permite ações internas em circunstâncias excepcionais.