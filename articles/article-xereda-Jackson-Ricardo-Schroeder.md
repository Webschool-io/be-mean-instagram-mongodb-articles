# MongoDB #### Artigo sobre Autenticação no MongoDB

**Autor:** Jackson Ricardo Schroeder

**Usuário:** [xereda](https://github.com/xereda)

**Data:** 1464123003469

## 1) Qual a diferença entre Autenticação e Autorização?

A **_Autenticação_** resume-se apenas no processo de acesso de um determinado usuário a um serviço. Já a **_autorização_** vem após uma autenticação bem sucedida e consiste na liberação de permissões, baseadas em papéis ou em ações, vinculadas ao usuário _autenticador_ previamente.


## 2) Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

A criação de um usuário no MongodDB pode ser realizada através de duas funções disponíveis em seu console: db.createUser() ou [db.runCommand()](https://docs.mongodb.com/v3.0/reference/method/db.runCommand/). Vamos ilustrar com base na primeira opção:


### Usuário _admnistrador_ (no console do mongodb)


```javascript

  use admin
  //crie o objeto com as propriedades user, senha e com a array de roles. A seguir, neste mesmo artigo, você terá mais detalhes sobre roles na definição de usuários no MongoDB.
  var usuario = {
     user: "usuarioAdm",
  	 pwd: "senha",
  	 roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  };
  // cria o usuário
  db.createUser(usuario);

```

### Usuário _comum_ (no console do mongodb)

```javascript

use database
//crie o objeto com as propriedades user, senha e com a array de roles. A seguir, neste mesmo artigo, você terá mais detalhes sobre roles na definição de usuários no MongoDB.
var usuario = {
   user: "usuarioApp",
   pwd: "senha",
   roles: [ { role: "readWrite", db: "database" } ]
};
// cria o usuário
db.createUser(usuario);

```


## 3) Explique cada papel listado em Cluster Administration Roles.

Em vez de somente databases específicas em um servidor, as permissões de administração de cluster são capazes de administrar várias instâncias do MongoDB organizadas em cluster.


### clusterAdmin

Mais alto nível de gerenciamento de cluster. O mesmo que conceder individualmente as roles `clusterManager`, `clusterMonitor` e `hostManager`. Também permite que o um usuário com esta role possa dropar uma database (`dropDatabase`).


### clusterManager

Permite, em um cluster, ações de monitoramento e gerenciamento. Usuários com essa permissão podem acessar as databases `config` e `local`, usadas em sharding e replicação.

Ações providas pela role:

#### addShard
#### applicationMessage
#### cleanupOrphaned
#### flushRouterConfig
#### listShards
#### removeShard
#### replSetConfigure
#### replSetGetStatus
#### replSetStateChange
#### resync

Em todas as databases do cluster, provê as seguintes ações:

#### enableSharding
#### moveChunk
#### splitChunk
#### splitVector

Na collection `settings` da database `config`, provê acesso às seguintes ações:

#### insert
#### remove
#### update

Dentro da database `config`, nas collections `system.indexes`, `system.js`, `system.namespaces` e em todas as collections de configuração, permite às seguntes ações:

#### collStats
#### dbHash
#### dbStats
#### find
#### killCursors

Na database `local`, permite acões na collection `replset`:

#### collStats
#### dbHash
#### dbStats
#### find
#### killCursors

### clusterMonitor

Autoriza acess somente leitura a ferramentas de monitoração, como o [MongoDB Cloud Manager](https://cloud.mongodb.com/?jmp=docs&_ga=1.132013837.389647211.1462567638).

Provê as seguintes permissões no cluster como um todo:

#### connPoolStats
#### cursorInfo
#### getCmdLineOpts
#### getLog
#### getParameter
#### getShardMap
#### hostInfo
#### inprog
#### listDatabases
#### listShards
#### netstat
#### replSetGetStatus
#### serverStatus
#### shardingState
#### top

Provê as seguintes permissões em todas as databases do cluster:

#### collStats
#### dbStats
#### getShardVersion

Provê a ação `find` em todas as collections `system.profile` do cluster.

Nas collections `system.indexes`, `system.js`, `system.namespaces` e em todas as collections de configuração da database `config`, provê acesso às seguntes ações:

#### collStats
#### dbHash
#### dbStats
#### find
#### killCursors

### hostManager

Autoriza o monitoramento e gerenciamento de servidores.

Permite seguintes ações no cluster:

#### applicationMessage
#### closeAllDatabases
#### connPoolSync
#### cpuProfiler
#### diagLogging
#### flushRouterConfig
#### fsync
#### invalidateUserCache
#### killop
#### logRotate
#### resync
#### setParameter
#### shutdown
#### touch
#### unlock

Permite as seguintes ações em qualquer databases do cluster:

#### killCursors
#### repairDatabase


## 4) Explique todas as ações de privilégio listadas aqui Privilege Actions.


As ações de previlégio definem as operações que um usuário pode executar em um recurso. Um previlégio do MongoDB dispõe de um recurso e as ações permitidas. .


### Consulta e Ações de Escrita

#### find

>Faz uma busca dos documentos de uma collection.

#### insert

>Insere um ou mais documentos de uma coleção.

#### remove

>Remove um ou mais documentos de uma coleção

#### update

>Atualiza um ou mais documentos de uma coleção.


### Ações para gerenciamento da database

#### changeCustomData

>Modifica a _custom information_ de qualquer usuário da _datase_ informada.

#### changeOwnCustomData

>Permite que o usuário altere suas próprias _custom information_.

#### changeOwnPassword

>Permite que o usuário mude sua própria senha.

#### changePassword

>Permite que o usuário modifique a senha de qualquer usuário da _database_ informada.

#### createCollection

>Permite criar uma coloeção na _database_.

#### createIndex

>Permite criar um índice na _database_

#### createRole

>Permite criar novas _roles_ na _database_.

#### createUser

>Permite criar novos usuários na _database_.

#### dropCollection

>Permite excluir uma coleção da _database_.

#### dropRole

>Permite excluir uma _role_ da _database_.

#### dropUser

>Permite excluir usuário da _database_.

#### emptycapped

>Permite remover todos os documentos de uma coleção.

#### enableProfiler

>Modificar o nível de perfil do usuário atual para capturar dados sobre o seu desempenho.

#### grantRole

>Modifica uma _role_ para qualquer usuário da _database_.

#### killCursors

>Permite matar cursores na _database_.

#### revokeRole

>Permite remover uma _role_ de um usuário da _database_.

#### unlock

>Permite destravar um serviço para que seja acessado por uma operação de leitura ou gravação.

#### viewRole

>Permite ver informações sobre uma _role_ da _database_.

#### viewUser

>Permive ver informações sobre um usuário.



### Ações de Gerenciamento de Deploying

#### authSchemaUpgrade

>Suporta o processo de atualização para sistemas existentes que usam autenticação e autorização entre as versões 2.4 e 2.6 e entre as versões 2.6 e 3.0.

#### cleanupOrphaned

>Permite excluir de um shard documentos órfãos cujo seus valores não pertençam a esse shard.

#### cpuProfiler

>Permite o usuário habilitar e usar o _CPU profiler_.

#### inprog

>Retorna operações ativas ou pendentes.

#### invalidateUserCache

>Libera informações do usuário do cache na memória RAM, incluindo a remoção de credenciais e papéis de cada usuário.

#### killop

>Mata uma determinada operação, identificada pelo seu id.

#### planCacheRead

>Visualiza os planos de _list_ e _querys_ do cache na _database_ ou coleções.

#### planCacheWrite

>Remove os _cached query plans_ da coleção.

#### storageDetails

>Permite executar o comando _storageDetails_.



### Ações de Replicação

#### appendOplogNote

>Permite adicionar notas ao oplog.

#### replSetConfigure

>Permite o usuário configurar uma _replica set_.

#### replSetGetStatus

>Permite ver o atual statis da _replica set_.

#### replSetHeartbeat

>Permite enviar um sinal para verificar se o _replica set_ está ativo.

#### replSetStateChange

>Permite mudar o estado de uma _replica set_ através dos comandos _replSetFreeze_, _replSetMaintenance_, _replSetStepDown_ e _replSetSyncFrom_.

#### resync

>Permite resincronizar as réplicas primárias e secundárias.



### Ações de Sharding

#### addShard

>Permite adicionar um _shard_ no _cluster_.

#### enableSharding

>Permite ativar um _shard_ na _database_ usando o comando _enableSharding_.

#### flushRouterConfig

>Permite limpar as informações do _cluster_ em cache e faz o _overload_ dos metadados.

#### getShardMap

>Permite mapear funcionalidades do _shard_.

#### getShardVersion

>Permite obter a versão do _shard_.

#### listShards

>Exibe a lista de _shards_.

#### moveChunk

>Permite mover os _chunks_ entre os _shards_.

#### removeShard

>Remove um _shard_ do _cluster_.

#### shardingState

>Mostra o estado do _sharding_.

#### splitChunk

>Permite dividir os _chunks_ das coleções ou _databases_.

#### splitVector

>Permite executar operações de metadados nos _clusters_ que possuem _shards_.




### Ações dos Administração de Servidores

#### applicationMessage

>Permite postar uma mensagem personalizada para o log de auditoria, através do comando logApplicationMessage.

#### closeAllDatabases

>Permite fechar todas as _databases_ do _cluster_.

#### collMod

>Permite adicionar um conjunto de flags para modificar o comportamento do MongoDB.

#### compact

>Permite regravar e desfragmentar todos os dados e índices de uma coleção na _database_.

#### connPoolSync

>Permite sincronizar o _pool_ do _cluster_.

#### convertToCapped

>Permite converter uma coleção _not-capped_ para _capped_.

#### dropDatabase

>Permite excluir uma _database_.

#### dropIndex

>Permite excluir índices de uma coleção da _database_.

#### fsync

>Permite forçar o serviço mongod para que execute um _flush_ em todas as escritas pendentes na camada de armazenamento em disco.

#### getParameter

>Permite recuperar o valor das opções configuradas anteriormente no _cluster_.

#### hostInfo

>Exibe informações sobre o _server_ que o MongoDB está rodando.

#### logRotate

>Permite fazer uma rotação nos logs para que este não se concerte em um único arquivo, diminuindo o espaço necessário para o armazenamento.

#### reIndex

>Permite apagar e recriar índices da coleção.

#### renameCollectionSameDB

>Permite renomear uma coleção na _database_ corrente.

#### repairDatabase

>Permite checar e reparar uma _database_ com insconsistências no armazenamento dos dados.

#### setParameter

>Permite alterar alguns parâmetros do _cluster_.

#### shutdown

>Limpa todos as _databases_ e encerra os processos.

#### touch

>Permite carregar os dados da camada de armazenamento de dados na memória.



### Ações de Diagnósticos

#### collStats

>Visualiza estatísticas de armazenamento de uma determina coleção.

#### connPoolStats

>Permite executar um comando interno no _cluster_.

#### cursorInfo

>Permite retornar informações sobre o cursor.

#### dbHash

>Permite executar comandos com suporte à algumas configurações do servidor.

#### dbStats

>Permite verificar estatísticas sobre o servidor.

#### diagLogging

>Permite retornar dados adicionais para diagnóstico.

#### getCmdLineOpts

>Permite executar o comando  o comando _getCmdLineOpts_ no _cluster_.

#### getLog

>Permite executar o comando _getLog_ no _cluster_.

#### indexStats

>Descontinuado na versão 3.0 do MongoDB, permite retornar estatísticas sobre índices de coleções da _database_.

#### listDatabases

>Permite listar estatísticas das _databases_.

#### listCollections

>Permite listar estatísticas das coleções da _database_.

#### listIndexes

>Permite listar os índices da coleção.

#### netstat

>Permite utilizar o comando _netstat_.

#### serverStatus

>Permite visualizar estatísticas sobre o _status_ do servidor.

#### validate

>Permite verificar estruturar de nomes de coleções e/ou índices.

#### top

>Permite retornar estatísticas de uso para cada coleção da _database_.



### Ações Internas

#### anyAction

>Permite uma ação em um recurso. Não utilize essa ação a não ser em circunstâncias excepcionais.

#### internal

>Permite ações internas. Não utilize essa ação a não ser em circunstâncias excepcionais.
