# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Igor Vidotto Felipe

**User:** [igorvidottof](https://github.com/igorvidottof)

**Data:** Wed Dec 9 21:00:08 BRST 2015

## Qual a diferença entre Autenticação e Autorização?
**Autenticação** é o processo de **verificar a identidade de um cliente**. Quando o controle de acesso (autorização) está habilitado, o MongoDB exige que os clientes se autentiquem para determinar seus acessos(permissões). 

Autenticação e Autorização são extremamente conectados um ao outro, todavia possuem significados diferentes. Como citado acima a autentição verifica a identidade de um usuário, ao passo que a **autorização** determina ao usuário autenticado o **acesso aos recursos e operações(ações), ou seja, suas permissões**.

###### Exemplo
Um usuário pode ter a permissão para se autenticar em algum banco por outro lado pode não ter nenhuma autorização para manipular este mesmo banco.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
Para garantir a segurança de nosso servidor MongoDB precisamos primeiramente criar um usuário administrador, autenticar-se com o mesmo e só então, a partir dele, criar outros usuários e delegar *roles*.

### Criando um Usuário Administrador
#### 1 - Subir o servidor do MongoDB sem controle de acesso numa porta específica

```js
mongod --port 27017
```

#### 2 - Conectar um cliente *mongo* à essa porta

```js
mongo --port 27017
```

> Para garantir que o cliente conecte-se adequadamente ao servidor podemos ainda passar o comando adicional `--host`

#### 3 - Conectar a db *admin*

```js
use admin
```

#### 4 - Criar o usuário com a função `createUser()` e nas roles adicionar somente a role *userAdminAnyDatabase*

```js
admin> db.createUser(
    {
        user: "IgorVidotto",
        pwd: "admin123",
        roles: [
           { role: "userAdminAnyDatabase", db: "admin" }
        ]
    }
);

//Saída
Successfully added user: {
  "user": "IgorVidotto",
  "roles": [
    {
      "role": "userAdminAnyDatabase",
      "db": "admin"
    }
  ]
}

```

> *user* se refere ao nome de usuário, *pwd* à senha e *roles* às permissões, privilégios e ações. 

> Poderíamos ainda adicionar outros campos como *customData*, *digestPassword* e *writeConcern*


#### 5 - Reiniciar o servidor MongoDB e habilitar o controle de acesso
Muito bem, tendo o usuário admin criado devemos reiniciar o servidor, agora habilitando o **controle de acesso**.

Para executarmos essa ação utilizamos a opção `--auth`

```js
mongod --auth --port 27017
```

Se repararmos no retorno do console vemos que agora o controle de acesso está habilitado.

```js
2015-12-08T19:04:37.922-0200 I CONTROL  [initandlisten] options: { net: { port: 27017 }, security: { authorization: "enabled" } }
```

> Podemos fazer essa operação também usando o arquivo de configuração, modificando a opção [*security.authorization*](https://docs.mongodb.org/manual/reference/configuration-options/#security.authorization), que por padrão é desabilitado.

#### 6 - Conectar um novo *mongo shell*(cliente) autenticando-se
Fazemos isso passando as opções `-u "user"`, `-p "password"` e `--authenticationDatabase "db"`

```js
mongo --port 27017 -u "IgorVidotto" -p "admin123" --authenticationDatabase "admin"
```

Se olharmos no servidor MongoDB, teremos a seguinte mensagem:

```js
2015-12-08T19:49:18.176-0200 I ACCESS   [conn1] Successfully authenticated as principal IgorVidotto on admin
```

Outra forma de autenticar-se é quando o mongo shell já está conectado ao servidor, podemos então executar a função `db.auth("user", "password")`

```js
use admin // db onde o usuário admin está cadastrado
db.auth("IgorVidotto", "admin123");

//Saída
1 // true, o usuário foi autenticado!
```

Agora já podemos usar os comandos de administrador para gerenciar os usuários.

###### Observações
Possivelmente alguns erros vão aparecer ao autenticar-se, porém podemos ignorá-los, pois são erros relacionados à permissões de *start up* cujos a role *userAdminAnyDatabase* não possui.

Um admin também pode ser criado mesmo depois de habilitar o controle de acesso, com a utilização do [Localhost Exception](https://docs.mongodb.org/manual/tutorial/enable-authentication/#add-users-after-enabling-access-control).

### Criando um Usuário Comum
Estando já autenticado com o administrador podemos então criar outros usuários.

Vamos criar um usuário com permissões para leitura e escrita nas dbs *be-mean-instagram* e *be-mean-pokemons*.

#### 1 - Utilizar a função `runCommand()` com o campo `createUser`

```js
db.runCommand( 
    { 
        createUser: "HardWorker",
        pwd: "123456",
        customData: { description: "Trabalha muito e ganha nada" } ,
        roles: [ 
            {role: "readWrite", db: "be-mean-instagram" },
            {role: "readWrite", db: "be-mean-pokemons"}
        ] 
    } 
);

//Saída
{
  "ok": 1
}
```

Pronto, o usuário já foi criado!

Para termos certeza que nossos usuários foram realmente criados podemos utilizar a mesma função `runCommand()` com o campo `usersInfo: 1`.

```js
db.runCommand( { usersInfo: 1 } );
{
  "users": [
    {
      "_id": "admin.IgorVidotto",
      "user": "IgorVidotto",
      "db": "admin",
      "roles": [
        {
          "role": "userAdminAnyDatabase",
          "db": "admin"
        }
      ]
    },
    {
      "_id": "admin.HardWorker",
      "user": "HardWorker",
      "db": "admin",
      "customData": {
        "description": "Trabalha muito e ganha nada"
      },
      "roles": [
        {
          "role": "readWrite",
          "db": "be-mean-instagram"
        },
        {
          "role": "readWrite",
          "db": "be-mean-pokemons"
        }
      ]
    }
  ],
  "ok": 1
}

```

> Lembrando que funções para gerenciamento de usuários como esta devem ser executados pelo admin. Se tentarmos rodar ela com o usuário comum que criamos o console retornará um erro dizendo que este usuário não tem permissão para realizar essa ação.

## Explique cada papel listado em [Cluster Administration Roles](https://docs.mongodb.org/v2.6/reference/built-in-roles/#cluster-administration-roles).
#### clusterAdmin
Proporciona a maior função(acesso) de administração de *cluster*. Esta *role* combina os privilégios concedidos pelas roles **clusterManager**, **clusterMonitor**, e **hostManager**. Além disso ainda fornece a ação `dropDatabase`.

#### clusterManager
Proporciona ações de administração e monitoramento no cluster. Um usuário com essa role pode acessar as databases `config` e `local`, que são usadas no *sharding* e na *replication*, respectivamente.

Fornece as seguintes ações no cluster:

* addShard
* applicationMessage
* cleanupOrphaned
* flushRouterConfig
* listShards
* removeShard
* replSetConfigure
* replSetGetStatus
* replSetStateChange
* resync

Fornece as seguintes ações em todas as databases do cluster:

* enableSharding
* moveChunk
* splitChunk
* splitVector

Na database `config`, fornece as seguintes ações na coleção `settings`:

* insert
* remove
* update

Na database `config`, fornece as seguintes ações em todas as coleções de configuração e nas coleções `system.indexes`, `system.js` e `system.namespaces`:

* collStats
* dbHash
* dbStats
* find
* killCursors

Na database `local`, fornece as seguintes ações na coleção `replSet`

* collStats
* dbHash
* dbStats
* find
* killCursors

#### clusterMonitor
Proporciona acesso *read-only* (somente de leitura) à ferramentas de monitoramento, como o *MongoDB Cloud Manager monitoring agent*

Fornece as seguintes ações no cluster:

* connPoolStats
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

Fornece as seguintes ações em todas as databases do cluster:

* collStats
* dbStats
* getShardVersion

Fornece a ação `find` em todas as coleções `system.profile` do cluster.

Fornece as seguintes ações nas coleções de configuração `config` da database e nas coleções `system.indexes`, `system.js` e `system.namespaces`:

* collStats
* dbHash
* dbStats
* find
* killCursors

#### hostManager
Proporciona a habilidade para monitorar e administrar servidores.

Fornece as seguintes ações no cluster:

* applicationMessage
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

Fornece as seguintes ações em todas as databases do cluster:

* killCursors
* repairDatabase

## Explique todas as ações de privilégio listadas em [Privilege Actions](https://docs.mongodb.org/manual/reference/privilege-actions/).

### Query and Write Actions 

#### find
O usuário pode executar o método `db.collection.find()`. Esta ação pode ser aplicada à database ou collection resources.

Seleciona documentos numa coleção e retorna um cursor dos documentos selecionados.

#### insert
O usuário pode executar o comando `insert`. Esta ação pode ser aplicada à database ou collection resources.

*Lançado na versão 2.6*

O comando `insert` insere um ou mais documentos e retorna um documento contendo o status de todas as inserções. O método `insert` fornecido pelos drives do MongoDB usam este comando internamente.

#### remove
O usuário pode executar o método `db.collection.remove()`. Esta ação pode ser aplicada à database ou collection resources.

Remove um documento de uma coleção.

#### update
O usuário pode executar o comando `update`. Esta ação pode ser aplicada à database ou collection resources. O método `update` fornecido pelos drives do MongoDB usam este comando internamente.

O comando `update` modifica documentos numa coleção. Um único comando `update` pode conter múltiplas declarações de atualização.

#### bypassDocumentValidation
*Lançado na versão 3.2*

O usuário pode ignorar validação de documentos com os comandos que suportam a opção `bypassDocumentValidation`. Para uma lista de comandos que suportam a opção `bypassDocumentValidation`, acesse [Document Validation](https://docs.mongodb.org/manual/release-notes/3.2/#rel-notes-document-validation). Esta ação pode ser aplicada à database ou collection resources.

### Database Management Actions

#### changeCustomData
O usuário pode mudar a *custom information* de qualquer usuário numa determinada database. Esta ação pode ser aplicada à database resources.

#### changeOwnCustomData
Os usuários podem mudar sua própria *custom information*. Esta ação pode ser aplicada à database resources.

#### changeOwnPassword
Os usuários podem mudar sua própria senha. Esta ação pode ser aplicada à database resources.

#### changePassword
O usuário pode mudar a senha de qualquer usuário numa determinada database. Esta ação pode ser aplicada à database resources.

#### createCollection
O usuário pode executar o método `db.createCollection()`. Esta ação pode ser aplicada à database ou collection resources.

Cria uma nova coleção explicitamente.

#### createIndex
Proporciona acesso ao método `db.collection.createIndex()` e ao comando `createIndexes`. Esta ação pode ser aplicada à database ou collection resources.

Cria índices em coleções.

#### createRole
O usuário pode criar novas *roles* numa determinada database. Esta ação pode ser aplicada à database resources.

#### createUser
O usuário pode criar novos usuários numa determinada database. Esta ação pode ser aplicada à database resources.

#### dropCollection
O usuário pode executar o método `db.collection.drop()`. Esta ação pode ser aplicada à database ou collection resources.

Remove uma coleção de uma database. O método também remove todos os índices associados com a coleção deletada. 

#### dropRole
O usuário pode deletar qualquer *role* numa determinada database. Esta ação pode ser aplicada à database resources.

#### dropUser
O usuário pode remover qualquer usuário numa determinada database. Esta ação pode ser aplicada à database resources.

#### emptycapped
O usuário pode executar o comando `emptycapped`. Esta ação pode ser aplicada à database ou collection resources.

O comando `emptycapped` remove todos os documentos de uma *capped collection*. 

#### enableProfiler
O usuário pode executar o método `db.setProfilingLevel()`. Esta ação pode ser aplicada à database resources.

Modifica o *database profiler level* atual usado pela *database profiling system* para obter dados sobre performance. 

#### grantRole
O usuário pode conceder qualquer *role* para qualquer usuário de qualquer database do sistema. Esta ação pode ser aplicada à database resources.

#### killCursors
O usuário pode matar cursores numa determinada collection.

#### revokeRole
O usuário pode remover qualquer *role* de qualquer usuário de qualquer database do sistema. Esta ação pode ser aplicada à database resources.

#### unlock
O usuário pode executar o método `db.fsyncUnlock()`. Esta ação pode ser aplicada ao cluster resource.

Desbloqueia uma instância `mongod` para permitir escritas e reverte a operação de uma operação `db.fsyncLock()`. Tipicamente você usará `db.fsyncUnlock()` seguindo uma operação de *backup* de database.

`db.fsyncUnlock()` é uma operação administrativa.

#### viewRole
O usuário pode ver informações sobre qualquer *role* em determinada database. Esta ação pode ser aplicada à database resources.

#### viewUser
O usuário pode ver informações de qualquer usuário em determinada database. Esta ação pode ser aplicada à database resources.

### Deployment Management Actions

#### authSchemaUpgrade
O usuário pode executar o comando `authSchemaUpgrade`. Esta ação pode ser aplicada ao cluster resource.

*Lançado na versão 2.6*

*Alterado na versão 3.0*

`authSchemaUpgrade` suporta o processo de *upgrade* para sistemas existentes que usam autenticação e autorização entre:

* 2.4 e 2.6 (acesse [Upgrade User Authorization Data to 2.6 Format](https://docs.mongodb.org/manual/release-notes/2.6-upgrade-authorization/))
* 2.6 e 3.0 (acesse [Upgrade to SCRAM-SHA-1](https://docs.mongodb.org/manual/release-notes/3.0-scram/))

#### cleanupOrphaned
O usuário pode executar o comando `cleanupOrphaned`. Esta ação pode ser aplicada ao cluster resource.

*Lançado na versão 2.6*

Deleta os documentos órfãos de um shard cujos os valores de shard key caem num único ou numa única faixa contígua que não pertencem ao shard. Por exemplo, se duas faixas contíguas não pertencem ao shard, o `cleanupOrphaned` examina ambas faixas à procura de documentos órfãos. 

#### cpuProfiler
O usuário pode habilitar e usar o *CPU profiler*. Esta ação pode ser aplicada ao cluster resource.

#### inprog
O usuário pode usar o método `db.currentOp()` para retornar operações ativas e pendentes. Esta ação pode ser aplicada ao cluster resource.

Retorna um documento que contém informação de operações em progresso da instância da database.

#### invalidateUserCache
Proporciona acesso ao comando `invalidateUserCache`. Esta ação pode ser aplicada ao cluster resource.

*Lançado na versão 2.6*

Iguala as informações de usuário do cache em memória, incluindo remoções de *credentials* e *roles* de cada usuário. Isso te permite limpar o cache a qualquer momento, independente do intervalo definido no parâmetro `userCacheInvalidationIntervalSecs`.

#### killop
O usuário pode executar o método `db.killOp()`. Esta ação pode ser aplicada ao cluster resource.

Finaliza a execução de uma operação especifica por seu ID. Para ver como encontrar operações e seus correspondentes IDs, acesse [db.currentOp()](https://docs.mongodb.org/manual/reference/method/db.currentOp/#db.currentOp).

#### planCacheRead
O usuário pode executar os comandos `planCacheListPlans` e `planCacheListQueryShapes` e os métodos `PlanCache.getPlansByQuery()` e `PlanCache.listQueryShapes()`. Esta ação pode ser aplicada à database ou collection resources.

##### planCacheListPlans
*Lançado na versão 2.6*

Mostra as *query plans* em cache para uma [*query shape*](https://docs.mongodb.org/manual/reference/glossary/#term-query-shape) especificada.

O *query optimizer* apenas coloca em cache os planos para essas *query shapes*, que podem ter mais de um plano viável. 

O mongo shell proporciona o *wrapper* `PlanCache.getPlansByQuery()` para este comando. 

##### planCacheListQueryShapes
*Lançado na versão 2.6*

Mostra as *query shapes* de cada *query plan* em cache existentes para uma coleção.

O *query optimizer* apenas coloca em cache os planos para essas *query shapes*, que podem ter mais de um plano viável. 

O mongo shell proporciona o *wrapper* `PlanCache.listQueryShapes()` para este comando. 

#### planCacheWrite
O usuário pode executar o comando `planCacheClear` e os métodos `PlanCache.clear()` e `PlanCache.clearPlansByQuery()`. Esta ação pode ser aplicada à database ou collection resources.

##### planCacheClear
*Lançado na versão 2.6*

Remove *query plans* em cache de uma coleção. Especifique uma *query shape* para remover *query plans* em cache dessa *shape*. Omita a *query shape* para limpar todas as *query plans* em cache.

##### PlanCache.clear()
Remove todas as *query plans* em cache de uma coleção. O método é disponível apenas num [*plan cache object*](https://docs.mongodb.org/manual/reference/method/db.collection.getPlanCache/#db.collection.getPlanCache) de uma coleção específica.

##### PlanCache.clearPlansByQuery() 
Limpa as *query plans* em cache especificadas pela *query shape*

O método é disponível apenas num [*plan cache object*](https://docs.mongodb.org/manual/reference/method/db.collection.getPlanCache/#db.collection.getPlanCache) de uma coleção específica.

#### storageDetails
O usuário pode executar o comando `storageDetails`. Esta ação pode ser aplicada à database ou collection resources.

### Replication Actions

#### appendOplogNote
O usuário pode acrescentar anotações ao `oplog`. Esta ação pode ser aplicada ao cluster resource.

#### replSetConfigure
O usuário pode configurar uma replica set. Esta ação pode ser aplicada ao cluster resource. 

#### replSetGetStatus
O usuário pode executar o comando `replSetGetStatus`. Esta ação pode ser aplicada ao cluster resource. 

O comando `replSetGetStatus` retorna o status de uma replica set do ponto de vista do servidor atual. Você deve rodar o comando dentro da database `admin`.

#### replSetHeartbeat
O usuário pode executar o comando `replSetHeartbeat`. Esta ação pode ser aplicada ao cluster resource. 

`replSetHeartbeat` é um comando interno que suporta a funcionalidade da replica set.

#### replSetStateChange
O usuário pode mudar o estado de uma replica set através dos comandos `replSetFreeze`, `replSetMaintenance`, `replSetStepDown`, e `replSetSyncFrom`. Esta ação pode ser aplicada ao cluster resource. 

##### replSetFreeze
Previne um membro da replica set de ser buscada na eleição por um número especificado de segundos. Use este comando em conjunto com o comando `replSetStepDown` para fazer um membro diferente primário na réplica set.

##### replSetStepDown
Força a replica primária de uma replica set tornar-se uma secundária, disparando uma eleição para primária. O comando derruba a primária por um número especificado de segundos; durante este período, o membro renunciado (que foi derrubado) fica impossibilitado de se tornar primário.

Por padrão, o comando apenas derruba a primária se uma secundária **elegível** está atualizada com a primária, esperando 10 segundos para a secundária assumir.

##### replSetMaintenance
O comando administrador `replSetMaintenance` habilita ou desabilita o modo de manutenção num membro secundário de uma replica set.

##### replSetSyncFrom
*Lançado na versão 2.2*

Configura explicitamente qual host o atual *mongod* recupera entradas do oplog. Esta operação é útil para testar diferentes padrões e em situações onde um membro não está replicando de um host desejado.

#### resync
O usuário pode executar o comando `resync`. Esta ação pode ser aplicada ao cluster resource. 

O comando `resync` força uma instância mongod *slave* desatualizada sincronizar-se novamente. Perceba que este comando é relevante apenas para uma replicação *master-slave*. Não se aplica a replica sets.

### Sharding Actions

#### addShard
O usuário pode executar o comando `addShard`. Esta ação pode ser aplicada ao cluster resource. 

Adiciona ou uma database ou uma replica set a um *sharded cluster*. A melhor configuração é implantar shards por meio de replica sets.

#### enableSharding
O usuário pode habilitar o sharding numa database usando o comando `enableSharding` e pode "shardear" uma coleção usando o comando `shardCollection`. Esta ação pode ser aplicada à database ou collection resources.

##### shardCollection
Habilita uma coleção para *sharding* e permite que o MongoDB comece a distribuir dados entre os *shards*. Você deve rodar o comando `enableSharding` antes de rodar este comando. 

#### flushRouterConfig
O usuário pode executar o comando `flushRouterConfig`. Esta ação pode ser aplicada ao cluster resource. 

O comando `flushRouterConfig` limpa as informações em cache feitas pela instância *mongos* do cluster atual e recarrega todos metadados do *sharded cluster* pelo *config database*.

Isto força uma atualização quando a *configuration database* armazena dados que são mais novos do que os dados em cache do processo *mongos*.

#### getShardMap
O usuário pode executar o comando `getShardMap`. Esta ação pode ser aplicada ao cluster resource. 

O `getShardMap` é um comando interno que suporta a funcionalidade de *sharding*.

#### getShardVersion
O usuário pode executar o comando `getShardVersion`. Esta ação pode ser aplicada à database resources. 

O `getShardVersion` é um comando interno que suporta a funcionalidade de *sharding* e não é parte do *stable client facing API*.

#### listShards
O usuário pode executar o comando `listShards`. Esta ação pode ser aplicada ao cluster resource. 

Retorna uma lista dos shards configurados. O comando `listShards` é disponível apenas em instâncias *mongos*. Você pode apenas executar este comando na database `admin`.

#### moveChunk
O usuário pode executar o comando `moveChunk`. Além disso, o usuário pode executar o comando `movePrimary` desde que seja aplicado para uma *database resource* apropriada. Esta ação pode ser aplicada à database ou collection resources.

Comando administrativo interno. Move *chunks* entre shards. O comando `moveChunk` é disponível apenas em instâncias *mongos*. Você pode apenas executar este comando na database `admin`.

##### movePrimary
Num *sharded cluster*, o comando `movePrimary` reatribui o [*primary shard*](https://docs.mongodb.org/manual/reference/glossary/#term-primary-shard). Este comando primeiramente altera o *primary shard* no *cluster metadata*, e então migra todas as coleções não-shardeadas para um shard específico. 


#### removeShard
O usuário pode executar o comando `removeShard`. Esta ação pode ser aplicada ao cluster resource. 

Remove um shard de um *sharded cluster*. Quando você roda este comando o MongoDB primeiro move os chunks deste shard para outros shards, após isso ele remove o shard.

#### shardingState
O usuário pode executar o comando `shardingState`. Esta ação pode ser aplicada ao cluster resource. 

O `shardingState` é um comando administrador que indica se o mongod é um membro de um *sharded cluster*.

#### splitChunk
O usuário pode executar o comando `splitChunk`. Esta ação pode ser aplicada à database ou collection resources. 

É um comando administrativo interno. Para separar chunks use as funções `sh.splitFind()` e `sh.splitAt()` no mongo shell.

#### splitVector
O usuário pode executar o comando `splitVector`. Esta ação pode ser aplicada à database ou collection resources. 

É um comando administrativo interno que suporta operações de meta-data em *sharded clusters*.

### Server Administration Actions

#### applicationMessage
O usuário pode executar o comando `logApplicationMessage`. Esta ação pode ser aplicada ao cluster resource. 

Disponível apenas no [MongoDB Enterprise](https://www.mongodb.com/products/mongodb-enterprise-advanced).

O comando `logApplicationMessage` permite que usuários postem uma mensagem personalizada no [*audit*](https://docs.mongodb.org/manual/core/auditing/) log. Se o MongoDB está rodando com autorização, os usuários devem ter a *role* **clusterAdmin** ou *roles* herdadas de clusterAdmin, para rodar este comando.

#### closeAllDatabases
O usuário pode executar o comando `closeAllDatabases`. Esta ação pode ser aplicada ao cluster resource. 

#### collMod
O usuário pode executar o comando `collMod`. Esta ação pode ser aplicada à database ou collection resources. 

*Lançado na versão 2.2*

`collMod` permite a adição de *flags* numa coleção para modificar o comportamento do MongoDB. *Flags* incluem **usePowerOf2Sizes** e **index**. 

#### compact
O usuário pode executar o comando `compact`. Esta ação pode ser aplicada à database ou collection resources. 

Reescreve e desfragmenta todos os dados e indexes numa coleção. Em databases [*WiredTiger*](https://docs.mongodb.org/manual/core/wiredtiger/#storage-wiredtiger), este comando irá liberar espaço de disco desnecessário para o sistema operacional.

#### connPoolSync
O usuário pode executar o comando `connPoolSync`. Esta ação pode ser aplicada ao cluster resource. 

O `connPoolSync` é um comando interno.

#### convertToCapped
O usuário pode executar o comando `convertToCapped`. Esta ação pode ser aplicada à database ou collection resources. 

O comando `convertToCapped` converte uma coleção *non-capped* para uma *capped collection* dentro da mesma database.

#### dropDatabase
O usuário pode executar o comando `dropDatabase`. Esta ação pode ser aplicada à database resources. 

O comando `dropDatabase` deleta a database atual, deletando também os arquivos de dados associados a ela.

#### dropIndex
O usuário pode executar o comando `dropIndexes`. Esta ação pode ser aplicada à database ou collection resources. 

O comando `dropIndex` deleta um ou todos os indexes da coleção atual.

#### fsync
O usuário pode executar o comando `fsync`. Esta ação pode ser aplicada ao cluster resource. 

Força o processo mongod a enviar todos os processos de escrita pendentes da *storage layer* para o disco. Opcionalmente, você pode usar o `fsync` para travar a instância mongod e bloquear operações de escrita com a intenção de executar backups.

Como aplicações escrevem dados, o MongoDB armazena os dados na *storage layer* e depois escreve os dados no disco dentro do [syncPeriodSecs](https://docs.mongodb.org/manual/reference/configuration-options/#storage.syncPeriodSecs) intervalo, que é 60 segundos por padrão. Rode fsync quando você queira enviar as escritas para o disco antes desse intervalo.

#### getParameter
O usuário pode executar o comando `getParameter`. Esta ação pode ser aplicada ao cluster resource. 

O `getParamater` é um comando administrativo para recuperar o valor de opções normalmente definidas nas linhas de comando. Este comando deve ser executado na database admin.

#### hostInfo
Proporciona informações sobre o servidor da instância MongoDB que está executando. Esta ação pode ser aplicada ao cluster resource. 

#### logRotate 
O usuário pode executar o comando `logRotate`. Esta ação pode ser aplicada ao cluster resource. 

O `logRotate` é um comando administrativo que te permite "rodar" os logs do MongoDB, para prevenir um único *logfile* de consumir muito espaço em disco.

#### reIndex
O usuário pode executar o comando `reIndex`. Esta ação pode ser aplicada à database ou collection resources. 

O comando `reIndex` remove todos os índices de uma coleção e então os recria. 

#### renameCollectionSameDB
Permite ao usuário renomear coleções na database em que se está usando o comando `renameCollection`. Esta ação pode ser aplicada à database resources. 

Além disso, o usuário deve ou ter a ação `find` na coleção de origem ou não ter a ação `find` na coleção de destino.

Se a coleção com o novo nome já existe, o usuário deve ter também a ação `dropCollection` na database de destino.

#### repairDatabase
O usuário pode executar o comando `repairDatabase`. Esta ação pode ser aplicada à database resources. 

Checa e repara erros e inconsistências no armazenamento de dados. Rode o comando `repairDatabase` para garantir a integridade depois de *restarts* inesperados do sistema ou erros que o finalizem.

#### setParameter
O usuário pode executar o comando `setParameter`. Esta ação pode ser aplicada ao cluster resource. 

O `setParamater` é um comando administrativo para modificar opções normalmente definidas nas linhas de comando. Este comando deve ser executado na database admin.

#### shutdown
O usuário pode executar o comando `shutdown`. Esta ação pode ser aplicada ao cluster resource. 

O comando `shutdown` limpa todas as *database resources* e então termina o processo. Este comando deve ser executado na database admin.

#### touch 
O usuário pode executar o comando `touch`. Esta ação pode ser aplicada ao cluster resource. 

O comando `touch` carrega os dados da *data storage layer* na memória. `touch` pode carregar os dados(documentos) ou indexes ou ambos. Use este comando para assegurar que a coleção e/ou as indexes estão na memória, antes de outra operação. Carregando a coleção ou a index na memória, o mongod vai idealmente ser capaz de executar operações subsequentes mais eficientemente.

### Diagnostic Actions

#### collStats
O usuário pode executar o comando `collStats`. Esta ação pode ser aplicada à database ou collection resources. 

O comando `collStats` retorna uma variedade de estatísticas de armazenamento de determinada collection.

#### connPoolStats
O usuário pode executar os comandos `connPoolStats` e `shardConnPoolStats`. Esta ação pode ser aplicada ao cluster resource. 

O comando `connPoolStats` retorna informação a respeito do número de conexões abertas na instância de database atual, incluindo *client connections* e *server-to-server connections* para replicação e clusterização.

#### cursorInfo
O usuário pode executar o comando `cursorInfo`. Esta ação pode ser aplicada ao cluster resource. 

O comando `cursorInfo` retorna informações sobre a atribuição e uso do cursor atual.

#### dbHash
O usuário pode executar o comando `dbHash`. Esta ação pode ser aplicada à database ou collection resources. 

`dbHash` é um comando que suporta os *config servers* e não é parte da *stable client facing API*.

#### dbStats
O usuário pode executar o comando `dbStats`. Esta ação pode ser aplicada à database resources. 

O comando `dbStats` retorna estatísticas de armazenamento de uma determina database.

#### diagLogging
O usuário pode executar o comando `diagLogging`. Esta ação pode ser aplicada ao cluster resource. 

*Deprecada desde a versão 3.0.0*

`diagLogging` é um comando que captura dados adicionais de diagnósticos e não é parte da stable client facing API.

#### getCmdLineOpts
O usuário pode executar o comando `getCmdLineOpts`. Esta ação pode ser aplicada ao cluster resource. 

O comando `getCmdLineOpts` retorna um documento contendo as linhas de comando de opções usadas para iniciar um determinado mongod ou mongos.

#### getLog
O usuário pode executar o comando `getLog`. Esta ação pode ser aplicada ao cluster resource. 

O comando `getLog` retorna um documento com um array de logs que contém mensagens recentes do processo de log do mongod. 

#### indexStats
O usuário pode executar o comando `indexStats`. Esta ação pode ser aplicada à database ou collection resources. 

*Alterado na versão 3.0*: O MongoDB 3.0 remove o comando indexStats

#### listDatabases
O usuário pode executar o comando `listDatabases`. Esta ação pode ser aplicada ao cluster resource. 

O comando `listDatabases` fornece uma lista com todas as databases existentes junto com as estatísticas básicas sobre elas.

#### listCollections
O usuário pode executar o comando `listCollections`. Esta ação pode ser aplicada à database resources. 

Recupera informações, o nome e opções sobre as coleções de uma database. 

#### listIndexes
O usuário pode executar o comando `ListIndexes`. Esta ação pode ser aplicada à database ou collection resources. 

#### netstat
O usuário pode executar o comando `netstat`. Esta ação pode ser aplicada ao cluster resource. 

`netstat` é um comando interno que é disponível apenas em instâncias mongos.

#### serverStatus
O usuário pode executar o comando `serverStatus`. Esta ação pode ser aplicada ao cluster resource. 

O comando `serverStatus` retorna um documento que fornece uma visão geral do estado dos processos da database. 

#### validate
O usuário pode executar o comando `validate`. Esta ação pode ser aplicada à database ou collection resources. 

O comando `validate` checa as estruturas com um *namespace* para correção escaneando os dados e os índices da coleção. O comando retorna informações a respeito da representação no disco da coleção.

#### top 
O usuário pode executar o comando `top`. Esta ação pode ser aplicada ao cluster resource. 

O `top` é um comando administrativo que retorna estatísticas de uso para cada coleção.

### Internal Actions

#### anyAction
Permite qualquer ação numa *resource*. Não utilize esta ação, a não ser por circunstâncias especiais.

#### internal
Permite ações *internal*. Não utilize esta ação, a não ser por circunstâncias especiais.