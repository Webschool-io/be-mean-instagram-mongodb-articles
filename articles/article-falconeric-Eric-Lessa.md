# MongoDb - Artigo sobre Autenticação no MongoDb

**Autor:** Eric Lessa

**User:** [falconeric](https://github.com/falconeric)

**Data:** 1458567407

## Qual a diferença entre Autenticação e Autorização?

Segundo a documentação do MongoDB, embora a autenticação e autorização estejam intimamente ligados, autenticação é diferente de autorização. A autenticação verifica a identidade de um usuário e autorização determina o acesso do usuário verificado a recursos e operações.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Conecte ao MongoDB com os privilégios apropriados
```
mongo --port 27017 -u myUserAdmin -p abc123 --authenticationDatabase admin
```

Criando um usuário administrador:
> o papel userAdminAnyDatabase garantirá que o novo usuário seja um administrador

```
use admin
db.createUser(
  {
      user : "euAdmin",
      pwd : "aquelaSenhaForte",
      roles : [
        {role : "userAdminAnyDatabase", db : "admin"}
      ]
  }
)
```

Criando um usuário comum para a base de dados *be-mean-pokemons*:
> o papel readWrite garantirá privilégios de leitura e escrita ao usuário para a base be-mean-pokemons

```
use be-mean-pokemons
db.createUser(
  {
      user : "Ash",
      pwd : "palletTown",
      roles : [
        {role : "readWrite", db : "be-mean-pokemons"}
      ]
  }
)
```


## Explique cada papel listado em Cluster Administration Roles.

**clusterAdmin**
>Fornece o maior acesso de gerenciamento de cluster. Este papel combina os privilégios concedidos pelos papéis clusterManager, clusterMonitor, e hostManager. Além disso, o papel a ação dropDatabase.

**clusterManager**
>Fornece ações de gerência e monitoramento no cluster. Um usuário com este papel pode acessar o *config* e base de dados locais, que são utilizados no shard e na replica, respectivamente.

Fornece as seguintes ações no cluster como um todo:

- addShard
>Adiciona uma instância de banco de dados ou replica a um cluster.
- applicationMessage
>Garante que um usuário possa executar logApplicationMessage, este comando permite ao usuário postar uma mensagem de log personalizada. Para executar este comando o usuário de ter o papel clusterAdmin ou paple que herde os privilégios de clusterAdmin.
- cleanupOrphaned
>Exclui a partir de um shard os documentos órfãos cujos valores chave cairam em um único ou uma única faixa contínua que não pertencem ao shard.
- flushRouterConfig
>Limpa as informações do cluster atual em cache por uma instância mongos e recarrega todos os metadados de cluster sharded a partir do config database.
- listShards
>Retorna uma lista de shards configurados.
- removeShard
>Remove um shard a partir de um cluster
- replSetConfigure
>Usuários podem configurar uma réplica
- replSetGetStatus
>Retorna o status da réplica do servidor corrente
- replSetStateChange
>Usuários podem alterar o estado de uma réplica através dos comandos replSetFreeze, replSetMaintenance, replSetStepDown, e replSetSyncFrom.
- resync
>Força uma instância de mongod desatualizada sincronizar-se

Fornece as seguintes ações em todas as databases no cluster:

- enableSharding
>Habilita um shard para uma database
- moveChunk
>Move chuncks entre os shards
- splitChunk
>Divide um chuck
- splitVector
>Divide um vetor

Na configuração do banco de dados, fornece as seguintes ações na configuração da collection:

- insert
>Insere um ou mais documentos e retorna o documento contendo o estado de todas inserções
- remove
>Remove o documentos de uma collection
- update
>Modifica o documento de uma collection

Na configuração da base de dados, fornece as seguintes ações em todas as configurações das coleções e nas coleções system.indexes, system.js, e system.namespaces:

- collStats
>Retorna uma variedade de estatística de armazenamento para uma determinada collection
- dbHash
>Retorna os valores de hash das collections em uma base de dados
- dbStats
>Retorna estatísticas para uma base de dados
- find
>Seleciona documentos em uma collection e retorna um cursor para o documento selecionado
- killCursors
>Mata/Destroi/Encerra um cursor específico no servidor

**clusterMonitor**

Fornece acesso read-only para ferramentas de monitoramento, como o agente de monitoramento MongoDB Cloud Manager.

Fornece as seguintes ações no cluster como um todo:

- connPullStats
>Retorna informações sobre o números de conexões abertas para a instância atual da base de dados, incluindo conexões de clientes e conexões de servidor
- cursorInfo
>Retorna informações sobre o cursor atual
- getCmdLineOpts
>Retorna um documento contendo opções de linha de comando usados para iniciar o mongod ou mongos
- getLog
>Retorna um documento com um conjunto de log contendo mensagens recentes de log do processo mongod
- getParameter
>Retorna valor das opções configuradas na linha de comando.
- getShardMap
>Retorna um documento com o mapeamento dos shards
- hostInfo
>Retorna informações sobre a instância de MongoDB que está sendo executada no servidor
- inprog
>Este tipo de permissão permite o usuário executar o método currentOp() que retorna informações sobre operações ativas na base de dados.
- listDatabases
>Retorna um listagem de todas as base de dados existentes
- listShards
>Retorna uma lista de shards configurados.
- netstat
>Retorna informações sobre as conexões
- replSetGetStatus
>Retorna o status da réplica do servidor corrente
- serverStatus
>Retorna um documento que fornece uma visão geral do estado do processo da base de dados
- shardingState
>Detecta se a instância de mongod é membro de um cluster
- top
>Retorna estatísticas de uso para cada collection.

Fornece as seguintes ações em todas as base de dados em um cluster:

- collStats
>Retorna uma variedade de estatística de armazenamento para uma determinada collection
- dbStats
>Retorna estatísticas para uma base de dados
- getShardVersion
>Retorna informações sobre o estado dos dados em um shard

Fornece a ação find em todas as system.profile collections no cluster

- collStats
>Retorna uma variedade de estatística de armazenamento para uma determinada collection
- dbHash
>Retorna os valores de hash das collections em uma base de dados
- dbStats
>Retorna estatísticas para uma base de dados
- find
>Seleciona documentos em uma collection e retorna um cursor para o documento selecionado
- killCursors
>Mata/Destroi/Encerra um cursor específico no servidor

**hostManager**

Fornece a capacidade de monitorar e gerenciar servidores.

Fornece as seguintes ações no cluster como um todo:

- applicationMessage
>Garante que um usuário possa executar logApplicationMessage, este comando permite ao usuário postar uma mensagem de log personalizada. Para executar este comando o usuário de ter o papel clusterAdmin ou paple que herde os privilégios de clusterAdmin.
- closeAllDatabases
>Invalida todos os cursores e fecha as base de dados abertas
- connPoolSync
>Comando interno para sincronizar um pool de conexões
- cpuProfiler
>O usuário pode ativar e usar o CPU profiler
- diagLogging
>Este comando captura dados para fins de diagnóstico.
- flushRouterConfig
>Limpa as informações do cluster atual em cache por uma instância mongos e recarrega todos os metadados de cluster sharded a partir do config database.
- fsync
>Força o processo de mongod limpar todas as gravações pendentes da camada de storage
- invalidateUserCache
>Libera informações de usuário em cache, incluindo remoção de credenciais e papéis de cada usuário.
- killop
>Encerra uma operação especifica através do opid
- logRotate
>Alterna o arquivo de log para impedir que um único arquivo fique grande demais
- resync
>Força uma instância de mongod desatualizada sincronizar-se
- setParameter
>configura parâmetros
- shutdown
>Encerra o processo de uma base de dados
- touch
>Carrega dados da base de dados para memória
- unlock
>Permite ao usuário executar o comando fsyncUnlock() que destrava uma instância de mongod permitindo escrever e reverter operações, normalmente usado em uma seguência de backup.

Fornece as seguintes ações em todas as base de dados em um cluster:

- killCursors
>Mata/Destroi/Encerra um cursor específico no servidor
- repairDatabase
>Verifica e repara erros e inconsistências em uma base de dados

## Explique todas as ações de privilégio listadas em Privilege Actions.

Privilege Actions definem as opereções que um usuário pode executar em um recurso. Um MongoDB privilege compreende um recurso e as ações permitidas. Esta página lista as ações disponíveis agrupadas por finalidade comum.

MongoDB fornece funções com recursos pré-definidos e ações definidas. Para lista de ações garantidas veja *Built-In Roles* na documentação do MongoDB. Para definir funções/papeis personalizadas, consulte *Create a User-Defined Role* na documentação do MongoDB.

### Consulta e ações de escrita

**find**
>Seleciona documentos em uma collection e retorna um cursor para o documento selecionado

**insert**
>Insere um ou mais documentos e retorna o documento contendo o estado de todas inserções

**remove**
>Remove o documentos de uma collection

**update**
>Modifica o documento de uma collection

**bypassDocumentValidation**
>O usuário pode ignorar a validação de documentos sobre comandos que suportem a opção bypassDocumentValidation

### Ações de gerenciamento de banco de dados

**changeCustomData**
>O usuário pode alterar as informações de qualquer usuário no banco de dados

**changeOwnCustomData**
>Usuários podem alterar suas próprias informações

**changeOwnPassword**
>Usuários podem alterar suas próprias senhas

**changePassword**
>Usuários podem alterar senhas de qualquer usuário no banco de dados

**createCollection**
>>O usuário pode criar coleções com o método createCollection()

**createIndex**
>O usuário pode criar índices com o método createIndex()

**createRole**
>O usuário pode criar novos papéis com o método createRole()

**createUser**
>O usuário pode criar novos usuários com o método createUser()

**dropCollection**
>O usuário pode remover coleções com o método dropCollection()

**dropRole**
>O usuário pode remover papéis com o método dropRole()

**dropUser**
>O usuário pode remover usuários com o método dropUser()

**emptycapped**
>O usuário pode remover todos os documentos de uma capped collection

**enableProfiler**
>O usuário pode executar o método setProfilingLevel()

**grantRole**
>O usuário pode atribuir qualquer papel no banco de dados para qualquer usuário de qualquer banco de dados no sistema

**killCursors**
>O usuário pode matar os cursores na coleção de destino

**revokeRole**
>O usuário pode remover qualquer papel de qualquer usuário a partir de qualquer banco de dados no sitema

**unlock**
>Permite ao usuário executar o método fsyncUnlock() que destrava uma instância de mongod permitindo escrever e reverter operações, normalmente usado em uma seguência de backup.

**viewRole**
>O usuário pode visualizar informações de qualquer papel no banco de dados

**viewUser**
>O usuário pode visualizar informações de qualquer usuário no banco de dados

### Ações de gerenciamento de implantação

**authSchemaUpgrade**
>O usuário pode executar o comando authSchemaUpgrade

**cleanupOrphaned**
>O usuário pode executar o comando cleanupOrphaned que exclui a partir de um shard os documentos órfãos cujos valores chave cairam em um único ou uma única faixa contínua que não pertencem ao shard.

**cpuProfiler**
>O usuário pode ativar e usar o CPU profiler

**inprog**
>O usuário pode usar o método currentOp() que retorna operações ativas e pendentes

**invalidateUserCache**
>Fornece acesso ao comando invalidateUserCache que libera informações de usuário em cache, incluindo remoção de credenciais e papéis de cada usuário.

**killop**
>O usuário pode executar o método killop() que encerra uma operação especifica através do opid

**planCacheRead**
>O usuário pode executar os comandos planCacheListPlans e planCacheListQueryShapes e os métodos PlanCache.getPlansByQuery() PlanCache.listQueryShapes()

**planCacheWrite**
>O usuário pode executar o comando planCacheClear e os métodos PlanCache.clear() e PlanCache.clearPlansByQuery()

**storageDetails**
>O usuário pode executar o comando storageDetails

### Ações de replicação

**appendOplogNote**
>O usuário pode acrescentar notas à oplog.

**replSetConfigure**
>O usuário pode configurar um conjunto de réplicas.

**replSetGetStatus**
>O usuário pode executar o comando replSetGetStatus.

**replSetHeartbeat**
>O usuário pode executar o comando replSetHeartbeat.

**replSetStateChange**
>O usuário pode alterar o estado de um conjunto de réplicas através dos comandos replSetFreeze, replSetMaintenance, replSetStepDown e creplSetSyncFrom.

**resync**
>O usuário pode executar o comando de ressincronização.

### Ações de Shard

**addShard**
>O usuário pode executar o comando addShard.

**enableSharding**
>O usuário pode ativar sharding em um banco de dados usando o comando enableSharding e não pode fragmentar uma coleção usando o comando shardCollection.

**flushRouterConfig**
>O usuário pode executar o comando flushRouterConfig.

**getShardMap**
> O usuário pode executar o comando getShardMap.

**getShardVersion**
>O usuário pode executar o comando setShardVersion.

**listShards**
>O usuário pode executar o comando listShards.

**moveChunk**
>O usuário pode executar o comando moveChunk. Além disso, o usuário pode executar o comando movePrimary desde que o privilégio seja aplicado a uma base de dados apropriada. Aplicar esta ação para banco de dados ou de cobrança de recursos.

**removeShard**
>O usuário pode executar o comando removeShard.

**shardingState**
>O usuário pode executar o comando shardingState.

**splitChunk**
>O usuário pode executar o comando splitChunk.

**splitVector**
>O usuário pode executar o comando splitVector.

### Ações de Administração de Servidor

**applicationMessage**
>O usuário pode executar o comando applicationMessage.

**closeAllDatabases**
>O usuário pode executar o comando closeAllDatabases.

**collMod**
>O usuário pode executar o comando collMod.

**compact**
>O usuário pode executar o comando compact.

**connPoolSync**
>O usuário pode executar o comando connPoolSync.

**convertToCapped**
>O usuário pode executar o comando convertToCapped.

**dropDatabase**
>O usuário pode executar o comando dropDatabase.

**dropIndex**
>O usuário pode executar o comando dropIndex.

**fsync**
>O usuário pode executar o comando fsync.

**getParameter**
>O usuário pode executar o comando getParameter.

**hostInfo**
>Fornece informações sobre o servidor a instância MongoDB é executado.

**logRotate**
>O usuário pode executar o comando logRotate.

**reIndex**
>O usuário pode executar o comando reIndex.

**renameCollectionSameDB**
>Permite ao usuário mudar o nome de coleções no banco de dados atual usando o comando renameCollection.

**repairDatabase**
>O usuário pode executar o comando repairDatabase.

**setParameter**
>O usuário pode executar o comando setParameter.

**shutdown**
>O usuário pode executar o comando shutdown.

**touch**
>O usuário pode executar o comando touch.

### Ações de Diagnóstico

**collStats**
>O usuário pode executar o comando collStats.

**connPoolStats**
>O usuário pode executar os comandos connPoolStats e shardConnPoolStats.

**cursorInfo**
>O usuário pode executar o comando cursorInfo.

**dbHash**
>O usuário pode executar o comando dbHash.

**dbStats**
>O usuário pode executar o comando dbStats.

**diagLogging**
>O usuário pode executar o comando diagLogging.

**getCmdLineOpts**
>O usuário pode executar o comando getCmdLineOpts.

**getLog**
>O usuário pode executar o comando getLog.

**indexStats**
>O usuário pode executar o comando indexStats.

**listDatabases**
>O usuário pode executar o comando listDatabases.

**listCollections**
>O usuário pode executar o comando listCollections.

**listIndexes**
>O usuário pode executar o comando listIndexes.

**netstat**
>O usuário pode executar o comando netstat.

**serverStatus**
>O usuário pode executar o comando serverStatus.

**validate**
>O usuário pode executar o comando validate.

**top**
>O usuário pode executar o comando top.

### Ações internas

**anyAction**
>Permite qualquer ação em um recurso. Não atribua essa ação, exceto em circunstâncias excepcionais.

**internal**
>Permite ações internas. Não atribua essa ação, exceto em circunstâncias excepcionais.