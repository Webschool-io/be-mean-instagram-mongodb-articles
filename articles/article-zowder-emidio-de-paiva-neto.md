# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor**: Emídio de Paiva Neto - [Zowder](https://github.com/Zowder)
**Data**: 1453306892223

## Qual a diferença entre Autenticação e Autorização?

 A autenticação é a validação de que um user por exemplo é quem ou o que afirma ser. Ou seja, a autenticação está provando que você é você. Normalmente, a autenticação por um servidor implica a utilização de um nome de usuário e senha. Outras maneiras de autenticar pode ser através de cartões, QRcodes, reconhecimento das digitais.

A autorização é um processo pelo qual um servidor determina se o cliente tem permissão para usar um recurso. A Autorização verifica o que você está autorizado a fazer. Um exemplo é você conseguir logar no servidor(autenticação), mas, não tem permissões de leitura, escrita ou execução, entre outras formas de garantir a segurança do servidor.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Para criarmos um usuário Administrador:

> use admin

Agora usamos o createUser

```js
db.createUser(
  {
    user: "labAdm",
    pwd: "pass",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
```
Para criarmos um usuário normal com permissão:

```js
use admin
db.createUser(
   {
     user: "accUser",
     pwd: "pass",
     roles: [{role: "readWrite", db: "admin"}]
   }
)
```
## Explique cada papel listado em [Cluster Administration Roles](https://docs.mongodb.org/v2.6/reference/built-in-roles/#cluster-administration-roles).

####**clusterAdmin**
>Concede o maior acesso de gerenciamento de Cluster. Este papel é a junção dos privilégios concedidos pelos papéis: **clusterManager**, **clusterMonitor**, **hostManager**.

####**clusterManager**
> Concede gerenciamento e ações de monitoramento no cluster. Um usuário com essa função pode acessar a configuração e bancos de dados locais, que são usados em sharding e replicação, respectivamente.

Fornece as seguintes ações no cluster:

> * addShard
* applicationMessage
* cleanupOrphaned
* flushRouterConfig
* listShards
* removeShard
* replSetConfigure
* replSetGetStatus
* replSetStateChange
* resync

Fornece as seguintes ações em todos os bancos de dados no cluster:
>* enableSharding
* moveChunk
* splitChunk
* splitVector

No banco de dados de configuração, oferece as seguintes ações na coleção configurações:
>* insert
* remove
* update

No banco de dados de configuração, concede as seguintes ações em todas as coleções de configuração bem como as coleções system.indexes, system.js e system.namespaces:

>* collStats
* dbHash
* dbStats
* find
* killCursors

No banco de dados local, fornece as seguintes ações na coleção replset:
>* collStats
* dbHash
* dbStats
* find
* killCursors

####**clusterMonitor**

>Fornece acesso somente leitura para ferramentas de monitoramento.

Fornece as seguintes ações no cluster como um todo:
>* connPoolStats
* cursorInfo
* getCmdLineOpts
* getLog
* getParameter
* getShardMap
* hostInfo
* inprog
* listDatabases
* listShards
* netstat
* replSetGetStatus
* serverStatus
* shardingState
* top

Fornece as seguintes ações em todos os bancos de dados no cluster:
>* collStats
* dbStats
* getShardVersion

No banco de dados de configuração, concede as seguintes ações em todas as coleções de configuração bem como as coleções system.indexes, system.js e system.namespaces:
>* collStats
* dbHash
* dbStats
* find
* killCursors

####**hostManager**
>Fornece a capacidade de monitorar e gerenciar servidores.

Fornece as seguintes ações no cluster como um todo:
>* applicationMessage
* closeAllDatabases
* connPoolSync
* cpuProfiler
* diagLogging
* flushRouterConfig
* fsync
* invalidateUserCache
* killop
* logRotate
* resync
* setParameter
* shutdown
* touch
* unlock

Fornece as seguintes ações em todos os bancos de dados no cluster:
>* killCursors
* repairDatabase

## Explique todas as ações de privilégio listadas em [Privilege Actions](https://docs.mongodb.org/manual/reference/privilege-actions/).

####**Query and Write Actions**
>* **find:** O usuário pode executar o método db.collection.find () para uma consulta e retorna o primeiro lote de resultados e o ID de cursor.
* **insert:** O usuário pode executar o comando de inserção para inserir um ou mais documentos.
* **remove:** O usuário pode executar o método db.collection.remove () para remover um documento.
* **update:** O usuário pode executar o comando update para modificar um documento em uma coleção.
####**Database Management Actions**
> * **changeCustomData:** O usuário pode alterar a custom information de qualquer usuário no banco de dados fornecido.
* **changeOwnCustomData:** Os usuários podem alterar suas próprias informações personalizadas(custom information).
* **changeOwnPassword:** Os usuários podem alterar suas próprias senhas.
* **changePassword:** O usuário pode alterar a senha de qualquer usuário no banco de dados fornecido.
* **createCollection :** O usuário pode criar uma coleção na database.
* **CreateIndex:** O usuário pode criar um índice.
* **createRole:** O usuário pode criar novas role no banco de dados fornecido.
* **createUser:** O usuário pode criar novos usuários no banco de dados fornecido.
* **dropCollection:** O usuário pode excluir uma coleção da database.
* **dropRole:** O usuário pode excluir uma role da databse.
* **dropUser:** O usuário pode excluir um outro usuário qualquer.
* **emptycapped:** O usuário pode remover todos os documentos de uma coleção.
* **enableProfiler:** Modifica o atual nível *database profiler*  usado pelo sistema de banco de dados de perfis para capturar dados sobre o desempenho.
* **grantRole:** O usuário pode conceder qualquer papel no banco de dados para qualquer usuário de qualquer banco de dados no sistema.
* **killCursors:** O usuário pode matar os cursores no conjunto de destino.
* **revokeRole:** O usuário pode remover qualquer papel de qualquer usuário a partir de qualquer banco de dados no sistema.
* **unlock:** O usuário pode realizar a db.fsyncUnlock ().
* **viewRole:** O usuário pode visualizar informações sobre qualquer função no banco de dados fornecido.
* **viewUser:** O usuário pode visualizar as informações de qualquer usuário no banco de dados fornecido.

####**Deployment Management Actions**
>* **authSchemaUpgrade:** O usuário pode atualizar o sistema de autenticação e autorização.
* **cleanupOrphaned:** Deleta de um shard documentos órfãs para recuperar espaço em disco.
* **cpuProfiler:** O usuário pode ativar e usar o profiler CPU.
* **inprog:** O usuário pode usar o db.currentOp () método para retornar operações pendentes e ativas.
* **invalidateUserCache:** Libera informações do usuário de cache de memória, incluindo a remoção de credenciais e os papéis de cada usuário.
* **killop:** Finaliza uma operação, tal como especificado por um código de operação.
* **planCacheRead:**  O usuário pode executar os comandos `planCacheListPlans`, `planCacheListQueryShapes` e os métodos `PlanCache.getPlansByQuery()` e `PlanCache.listQueryShapes()`.
	* **planCacheListPlans:** Exibe os planos de consulta em cache para a especificada forma de consulta.
	* **planCacheListQueryShapes:** Exibe as query shapes para cada query plan existente na coleção.
* **planCacheWrite:** Pode executar o comando `planCacheClear.`
	* **planCacheClear:** Remove as query plans em cache de uma coleção.
* **storageDetails:** O usuário pode executar o comando storageDetails, como o próprio nome sugere: Detalhes de armazenamento.

####**Replication Actions**
> * **appendOplogNote:** O usuário pode adicionar anotações ao oplog.
* **replSetConfigure:** O usuário pode configurar um conjunto de réplicas.
* **replSetGetStatus:** O usuário pode executar o comando `replSetGetStatus`.
	* **replSetGetStatus:** Retorna o status do conjunto de réplicas a partir do ponto de vista do servidor atual.
* **replSetHeartbeat:** O usuário pode executar o comando `replSetHeartbeat`.
	* **replSetHeartbeat:** É um comando interno que suporta a funcionalidade conjunto de réplicas.
* **replSetStateChange:** O usuário pode alterar o estado de um conjunto de réplicas através dos comandos `replSetFreeze`, `replSetMaintenance`, `replSetStepDown`, `replSetSyncFrom`.
	* **replSetFreeze:** Impede que um membro do conjunto de réplica procure eleição por um tempo especificado de segundos.
	* **replSetMaintenance:** Habilita ou desabilita o modo de manutenção para um membro secundário da replica set.
	* **replSetStepDown:** Força o primário do conjunto de réplicas para se tornar um secundário, desencadeando uma eleição para decidir quem vai ser o primário.
	* **replSetSyncFrom:** Explicitamente configura qual host atual do  mongod puxa as entradas de oplog.
* **resync:** Força uma instância do mongod out-of-date á sincronizar-se novamente.


####**Sharding Actions**
> * **addShard:** Adiciona uma instância de banco de dados ou um conjunto de réplicas de um sharded cluster.
* **enableSharding:** O usuário pode habilitar sharding em um banco de dados usando o comando `enableSharding` e pode habilitar sharding em  uma coleção usando o comando `shardCollection`.
* **flushRouterConfig:** Limpa as informações do cluster atual em cache por uma instância do mongos e recarrega todos os metadados do banco de dados de configuração.
* **getShardMap:** È um comando interno que suporta a funcionalidade sharding.
* **getShardVersion:** é um comando que suporta a funcionalidade sharding.
* **listShards:** Retorna uma lista de todos os shards configurados.
* **moveChunk:** Comando administrativo interno. Move chunks entre os shards.
* **removeShard:** Remove o shard de um sharded cluster. Quando você executa removeShard, o MongoDB primeiro move os chunks do shard para outros shards do cluster. Em seguida, o MongoDB remove o shard.
* **shardingState:**  É um comando de administração que informa se mongod é um membro de um sharded cluster.
* **splitChunk:** É um comando administrativo interno para separar chunks.
* **splitVector:** É um comando interno que suporta operações de meta-dados em um cluster sharded.

####**Server Administration Actions**
> * **applicationMessage:** O comando logApplicationMessage permite aos usuários postar uma mensagem personalizada para o audit log.
* **closeAllDatabases:** O usuário pode executar o comando
closeAllDatabases.
* **collMod:** Torna possível adicionar um conjunto de flags numa coleção para modificar o comportamento do mongoDB.
* **compact:** Regrava e desfragmenta todos os dados e indexes em uma coleção.
* **connPoolSync:** O usuário pode executar o comando interno `connPoolSync`.
* **convertToCapped:** Converte um non-capped collection em um capped collection(Uma coleção de tamanho fixo que sobrescreve automaticamente suas entradas mais antigas quando alcança seu tamanho máximo.).
* **dropDatabase:** Apaga o banco de dados atual, apagando os arquivos de dados associados.
*  **dropIndex:** Remove um ou todos os indexes da coleção atual.
* **fsync:** Força o mongod á enviar todas as escritas pendentes da storage layer para o disco.
* **getParameter:** É um comando administrativo para recuperar o valor das opções normalmente configuradas na linha de comando.
* **hostInfo:** Fornece informações sobre o servidor em que a instância do mongoDB é executada.
* **logRotate:**  é um comando administrativo que permite que você gire o mongoDB logs para evitar que um único arquivo de log consuma muito espaço em disco.
* **reIndex:** Remove todos os índices de uma coleção e então os recria.
* **renameCollectionSameDB:** Permite o usuário renomear coleções no banco de dados atual usando o comando renameCollection.
* **repairDatabase:** Checa, repara erros e inconsistências no armazenamento de dados.
* **setParameter:** É um comando administrativo para modificar opções normalmente definidas nas linhas de comando.
* **shutdown:** Limpa todos os recursos de banco de dados e, em seguida, encerra o processo.
* **touch:** Carrega os dados da data storage layer na memória.

####**Diagnostic Actions**
>* **collStats:** Retorna uma variedade de estatísticas de armazenamento para uma determinada coleção.
* **connPoolStats:** Retorna informações sobre o número de conexões abertas para a instância do banco de dados atual, incluindo conexões de clientes e conexões de servidor para replicação e clustering.
* **cursorInfo:**  Retorna informações sobre colocação atual do cursor.
* **dbHash:** dbHash é um comando que suporta os config servers e não é parte da stable client facing API.
* **dbStats:** Retorna estatísticas de armazenamento para um banco de dados.
* **diagLogging:** É um comando que captura dados adicionais para fins de diagnóstico.
* **getCmdLineOpts:** Retorna um documento contendo as opções de linha de comando usados para iniciar o mongod ou mongos dado:
* **getLog:** Retorna um documento com um conjunto de log que contenha mensagens recentes do log de processo mongod.
* **indexStats:** O usuário pode executar o comando indexStats. Alterado na versão 3.0: MongoDB 3.0 remove o comando indexStats.
* **listDatabases:** Fornece uma lista de todos os bancos de dados existentes, juntamente com estatísticas básicas sobre eles
* **listCollections:** Recuperar informações, ou seja, o nome e opções, sobre as coleções em um banco de dados. Especificamente, o comando devolve um documento que contém informações com as quais criar um cursor para a recolha de informação.
* **listIndexes:** O usuário pode executar o comando ListIndexes.
* **netstat:** é um comando interno que está disponível apenas em instâncias mongos
* **serverStatus:** Retorna um documento que fornece uma visão geral do estado do processo de banco de dados.
* **validate:** Verifica as estruturas dentro de um espaço de nomes para correção pela digitalização de dados e índices da coleção.
* **top:** É um comando administrativo que retorna estatísticas de uso para cada coleção.
####**Internal Actions**
>* **anyAction:** Permite qualquer ação em um recurso. Não atribuir essa ação, exceto em circunstâncias excepcionais.
* **internal:** Permite ações internas. Não atribuir essa ação, exceto em circunstâncias excepcionais.
