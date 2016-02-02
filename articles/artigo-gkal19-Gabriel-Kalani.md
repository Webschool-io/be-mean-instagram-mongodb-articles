# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Gabriel Kalani
**Data:** 1422549840



## Qual a diferença entre Autenticação e Autorização?

Para mim, **Autenticação** é como um processo de verificação no qual um servidor checa as credenciais de um usuário dentro de uma conexão.
E o processo de **Autorização** vai verificando se a conexão e as credenciais são permitidas para o acesso do sistema.
Isso pode ser usado muito em redes sociais, e-commerce, blogs e muito mais.

Resumidamente: A **Autenticação irá iniciar um processo de verificação de um determinado usuário e a **Autorização** determina quais recursos e operações serão permitidos ao determinado usuário.


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

OBS: Foi o que eu entendi.

Para criar um usuário comum:

```js
  db.createUser({
    user: "commonUser",
    pwd: "1956a",
    roles: [{
      role: "readWrite",
      db: "db" }]
  })
```
Para criar um usuário administrador:

```js
  db.createUser({
    user: "adminUser",
    pwd: "1956b",
    roles: [{
      role: "userAdminAnyDatabase",
      db: "db" }]
  })
```


## Explique cada papel listado em Cluster Administration Roles.

###clusterAdmin

É o papel principal de maior nível com o acesso ao gerenciamento de um cluster.
E é esse papel que nos permite usarmos a ação `dropDatabase`.


###clusterManager

Esse papel faz com que as ações de gerenciamento e monitoramento em um cluster sejam executadas.
O usuário que obter esse papel possui o "poder" de acessar as databases `local` e `config`, respectivamente usadas para replicação e sharding.


###clusterMonitor

O `clusterMonitor` nos dá acesso apenas a leitura das ferramentas de monitoração.


###hostManager

Nos fornece a possibilidade de monitorar e gerenciar os servidores.


## Explique todas as ações de privilégio listadas em Privilege Actions.

A função concede privilégios para executar conjuntos de ações sobre os recursos definidos.
Pode conceder acesso a um nível de coleção, quando aplicado ao banco de dados no qual ele está definido.

####find
Faz uma busca dos documentos de uma collection.

####insert
Insere um ou mais documentos de uma coleção.

####remove
Remove um ou mais documentos de uma coleção

####update
Atualiza um ou mais documentos de uma coleção.


###Database Management Actions

####changeCustomData
Modifica a _custom information_ de qualquer usuário da _datase_ informada.

####changeOwnCustomData
Permite que o usuário altere suas próprias _custom information_.

####changeOwnPassword
Permite que o usuário mude sua própria senha.

####changePassword
Permite que o usuário modifique a senha de qualquer usuário da _database_ informada.

####createCollection
Permite criar uma coloeção na _database_.

####createIndex
Permite criar um índice na _database_

####createRole
Permite criar novas _roles_ na _database_.

####createUser
Permite criar novos usuários na _database_.

####dropCollection
Permite excluir uma coleção da _database_.

####dropRole
Permite excluir uma _role_ da _database_.

####dropUser
Permite excluir usuário da _database_.

####emptycapped
Permite remover todos os documentos de uma coleção.

####enableProfiler
Modificar o nível de perfil do usuário atual para capturar dados sobre o seu desempenho.

####grantRole
Modifica uma _role_ para qualquer usuário da _database_.

####killCursors
Permite matar cursores na _database_.

####revokeRole
Permite remover uma _role_ de um usuário da _database_.

####unlock
Permite destravar um serviço para que seja acessado por uma operação de leitura ou gravação.

####viewRole
Permite ver informações sobre uma _role_ da _database_.

####viewUser
Permive ver informações sobre um usuário.

###Ações de Gerenciamento de Deploying

####authSchemaUpgrade
Suporta o processo de atualização para sistemas existentes que usam autenticação e autorização entre as versões 2.4 e 2.6 e entre as versões 2.6 e 3.0.

####cleanupOrphaned
Permite excluir de um shard documentos órfãos cujo seus valores não pertençam a esse shard.

####cpuProfiler
Permite o usuário habilitar e usar o _CPU profiler_.

####inprog
Retorna operações ativas ou pendentes.

####invalidateUserCache
Libera informações do usuário do cache na memória RAM, incluindo a remoção de credenciais e papéis de cada usuário.

####killop
Mata uma determinada operação, identificada pelo seu id.

####planCacheRead
Visualiza os planos de _list_ e _querys_ do cache na _database_ ou coleções.

####planCacheWrite
Remove os _cached query plans_ da coleção.

####storageDetails
Permite executar o comando _storageDetails_.



###Ações de Replicação

####appendOplogNote
Permite adicionar notas ao oplog.

####replSetConfigure
Permite o usuário configurar uma _replica set_.

####replSetGetStatus
Permite ver o atual statis da _replica set_.

####replSetHeartbeat
Permite enviar um sinal para verificar se o _replica set_ está ativo.

####replSetStateChange
Permite mudar o estado de uma _replica set_ através dos comandos _replSetFreeze_, _replSetMaintenance_, _replSetStepDown_ e _replSetSyncFrom_.

####resync
Permite resincronizar as réplicas primárias e secundárias.



###Ações de Sharding

####addShard
Permite adicionar um _shard_ no _cluster_.

####enableSharding
Permite ativar um _shard_ na _database_ usando o comando _enableSharding_.

####flushRouterConfig
Permite limpar as informações do _cluster_ em cache e faz o _overload_ dos metadados.

####getShardMap
Permite mapear funcionalidades do _shard_.

####getShardVersion
Permite obter a versão do _shard_.

####listShards
Exibe a lista de _shards_.

####moveChunk
Permite mover os _chunks_ entre os _shards_.

####removeShard
Remove um _shard_ do _cluster_.

####shardingState
Mostra o estado do _sharding_.

####splitChunk
Permite dividir os _chunks_ das coleções ou _databases_.

####splitVector
Permite executar operações de metadados nos _clusters_ que possuem _shards_.




###Ações dos Administração de Servidores

####applicationMessage
Permite postar uma mensagem personalizada para o log de auditoria, através do comando logApplicationMessage.

####closeAllDatabases
Permite fechar todas as _databases_ do _cluster_.

####collMod
Permite adicionar um conjunto de flags para modificar o comportamento do MongoDB.

####compact
Premite regravar e desfragmentar todos os dados e índices de uma coleção na _database_.

####connPoolSync
Permite sincronizar o _pool_ do _cluster_.

####convertToCapped
Permite converter uma coleção _not-capped_ para _capped_.

####dropDatabase
Permite excluir uma _database_.

####dropIndex
Permite excluir índices de uma coleção da _database_.

####fsync
Permite forçar o serviço mongod para que execute um _flush_ em todas as escritas pendentes na camada de armazenamento em disco.

####getParameter
Permite recuperar o valor das opções configuradas anteriormente no _cluster_.

####hostInfo
Exibe informações sobre o _server_ que o MongoDB está rodando.

####logRotate
Permite fazer uma rotação nos logs para que este não se concerte em um único arquivo, diminuindo o espaço necessário para o armazenamento.

####reIndex
Permite apagar e recriar índices da coleção.

####renameCollectionSameDB
Permite renomear uma coleção na _database_ corrente.

####repairDatabase
Permite checar e reparar uma _database_ com insconsistências no armazenamento dos dados.

####setParameter
Permite alterar alguns parâmetros do _cluster_.

####shutdown
Limpa todos as _databases_ e encerra os processos.

####touch
Permite carregar os dados da camada de armazenamento de dados na memória.



###Ações de Diagnósticos

####collStats
Visualiza estatísticas de armazenamento de uma determina coleção.

####connPoolStats
Permite executar um comando interno no _cluster_.

####cursorInfo
Permite retornar informações sobre o cursor.

####dbHash
Permite executar comandos com suporte à algumas configurações do servidor.

####dbStats
Permite verificar estatísticas sobre o servidor.

####diagLogging
Permite retornar dados adicionais para diagnóstico.

####getCmdLineOpts
Permite executar o comando  o comando _getCmdLineOpts_ no _cluster_.

####getLog
Permite executar o comando _getLog_ no _cluster_.

####indexStats
Descontinuado na versão 3.0 do MongoDB, permite retornar estatísticas sobre índices de coleções da _database_.

####listDatabases
Permite listar estatísticas das _databases_.

####listCollections
Permite listar estatísticas das coleções da _database_.

####listIndexes
Permite listar os índices da coleção.

####netstat
Permite utilizar o comando _netstat_.

####serverStatus
Permite visualizar estatísticas sobre o _status_ do servidor.

####validate
Permite verificar estruturar de nomes de coleções e/ou índices.

####top
Permite retornar estatísticas de uso para cada coleção da _database_.



###Ações Internas

####anyAction
Permite uma ação em um recurso. Não utilize essa ação a não ser em circunstâncias excepcionais.

####internal
Pèrmite ações internas. Não utilize essa ação a não ser em circunstâncias excepcionais.
