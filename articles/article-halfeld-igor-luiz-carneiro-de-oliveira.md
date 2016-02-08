# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Igor Luiz Carneiro de Oliveira.  
**Data** 1454937607087

## Qual a diferença entres Autenticação e Autorização?

> **Autenticação**: É a verificação das credenciais na tentativa de conexão, esse processo se realiza quando é perguntado se pode ter acesso a tal função.

> **Autorização**: É o processo que acontece depois da autenticação, se a conexão for permitida, é dada a autorização.


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

##### Criar um administrador:

```js
db.runCommand({ 
    createUser: "Admin",
    pwd: "senha1234",
    customData: { admin: true },
    roles: [{ 
        role: "userAdminAnyDatabases", db: "admin" 
    }],
    writeConcern: { w: "majority" , wtimeout: 5000 }
})
```

##### Criar um usuário:
```js
db.runCommad({
    createUser: "user1",
    pwd: "testeABC",
    customData: { commonUser: true },
    role: [{ 
        role: "dbOwner" 
    }],
    writeConcern: { w: "majority", wtimeout: 5000 }

})
```

## Explique cada papel listado em Cluster Administration Roles.

#### clusterAdmin
> É o maior nível de acesso de gerenciamento de um cluster. Este papel junta privilégios dados pelos **clusterManager, clusterMonitor, hostManager**. 

#### clusterManager
> Este permite o gerenciamento e monitoramento de um cluster. Um usuário com este papel pode acessar os bancos:  
+ Local - _Banco de dados para Replicação._
+ Config - _Banco de dados para Shading._

#### clusterMonitor
> Permite acesso de leitura apenas a ferramentas de monitoração.

#### hostManager
> Permite fazer monitoramento e gerenciamento de servidores

## Explique todas as ações de privilégio listadas em Privilege Actions.

#### Ações de busca e escrita
> + `find`: Realizar busca em um banco de dados ou em uma coleção.  
+ `insert`: Possibilidade de inserir novos dados em um banco de dados ou em uma coleção.
+ `remove`: Possibilidade de remover dados de um banco de dados ou de uma coleção.
+ `update`: Possibilidade de atualizar dados de um banco de dados ou de uma coleção.

#### Ações de gerenciamento de Banco de Dados

> + `changeCustomData`: Possibilidade de alterar a **customData** de qualquer usuário em um banco de dados.
+ `changeOwnCustomData`: Possibilidade de alterar sua própria **customData** em um banco de dados.
+ `changePassword`: Possibilidade de alterar o **pwd** de qualquer usuario em um banco de dados.
+ `changeOwnPassword`: Possibilidade de alterar o seu próprio **pwd** em um banco de dados.
+ `createCollections`: Possibilidade de criar coleções em um banco de dados.
+ `createIndex`: Possibilidade de criar Index em um banco de dados.
+ `createRole`: Possibilidade de criar papeis em um banco de dados.
+ `dropRole`: Possibilidade de excluir papeis em um banco de dados.
+ `createUser`: Possibilidade de criar usuários em um banco de dados.
+ `dropUser`: Possibilidade de excluir usuários em um banco de dados.
+ `dropCollection`: Possibilidade de excluir uma coleção em um banco de dados.
+ `emptycapped`: Possibilidade de excluir todos os dados de um coleção de tamanho limitado em um banco de dados.
+ `enableProfiler`: Possibilidade de mudar o nível de **profiler** em um banco de dados.
+ `grantRole`: Possibilidade de conceder papeis para qualquer usuário em um banco dados.
+ `revokeRule`: Possibilidade de remover papeis para qualquer usuário em um banco de dados.
+ `killCursors`: Possibilidade de derrubar um cursor em uma colleção.
+ `unlock`: Possibilidade de desbloquear um instância do **mongod** para escrita(_geralmente usado após uma operação de **backup**_).
+ `viewRole`: Possibilidade de ver papeis em um banco de dados.
+ `viewUser`: Possibilidade de ver usuário em um banco de dados.

#### Ações de gerenciamento de Deployment
> + `authSchemaUpgrade`: Possibilidade de atualizar o esquema de autencação de autorização de um **cluster**.
+ `cleanupOrphaned`: Possibilidade de remocer dados que não pertencem a um **shard**.
+ `cpuProfiler`: Possibilidade de habilitar o profile da CPU de um **cluster**.
+ `iprog`: Possibilidade de verificar operações ativas e pendentes em um **cluster**.
+ `invalidateUserCache`: Possibilidade de liberar informações de usuário do cache, incluindo remoção de credenciais para cada usuário em um **cluster**
+ `killop`: Possibilidade matar operações em um **cluster**.
+ `planCacheRead`: Possibilidade de listar planos de cache em um banco de dados ou em uma coleção.
+ `planCacheWrite`: Possibilidade de remover os planos de cache em um banco de dados ou em uma coleção.
+ `storageDetails`: Possibilidade de executar o comando **_storageDetails_** em um banco de dados ou em uma coleção.


#### Ações de replicação
> + `appendOplogNote`: Possibilidade de adicionar notas no **oplog** de um **closter**.
+ `replSetConfigure`: Possibilidade de configurar uma **Replica Set** em um **cluster**.
+ `replSetGetStatus`: Possibilidade de ver o status de uma **Replica Set** em um **cluster**.
+ `replSetHeartbeat`: Possibilidade de executar o comando **_replSetHeartbeat_** em um **cluster**.
+ `replSetStateChange`: Possibilidade de mudar o estado de uma **Replica Set** em um **cluster**.
+ `resync`: Possibilidade de resincronização de uma instancia escrava do _mongod_ em um **cluster**.


#### Ações de sharding
> + `addShard`: Possibilidade de adicionar im **shard** em um **cluster**.
+ `enableSharding`: Possibilidade de habilitar e começar a dividir os dados entre os **shrads** em um **cluster**.
+ `flushRouterConfig`: Possibilidade a limpeza das informações do **cluster** atual em cache e recarega todos e metadados de um **cluster** _sharded_ da configuração do banco de dados.
+ `getShardMap`: Possibilidade de poder executar o commando **_getShardMap_** num cluster.
+ `getShardVersion`: Possibilidade de poder executar o commando **_getShardVersion_** num banco de dados.
+ `listShards`: Possibilita o retorno de uma lista dos **shards** configurados.
+ `moveChunk`: Possibilita mover **chunks** entre **shards** e atribui novamente para parte primaria.
+ `removeShard`: Possibilidade de poder remover **shards** de um _sharded cluster_.
+ `shardingState`: Possibilidade de saber se o _mongod_ e membro de um _sharded cluster_.
+ `splitChunk`: Possibilidade de poder executar o commando **_splitChunk_** num banco de dados ou coleção.
+ `splitVector`: Possibilidade de poder executar o commando **_splitVector_** num banco de dados ou coleção.


#### Ações de Administração de Servidor
> + `applicationMessage`: Possibilidade de poder executar o commando **_logApplicationMessage_** em um **cluster**.
+ `closeAllDatabases`: Possibilidade de poder executar o commando **_closeAllDatabases_** em um **cluster**.
+ `collMod`: Possibilidade de poder executar o commando **_collMod_** em um banco de dados ou em uma coleção.
+ `compact`: Possibilidade de poder executar o commando **_compact_** em um banco de dados ou em uma coleção.
+ `connPoolSync`: Possibilidade de poder executar o commando **_connPoolSync_** em um **cluster**.
+ `convertToCapped`: Possibilidade de poder executar o commando **_convertToCapped_** em um banco de dados ou em uma coleção.
+ `dropDatabase`: Possibilidade de poder executar o commando **_dropDatabase_** em um banco de dados ou em uma coleção.
+ `dropIndex`: Possibilidade de poder executar o commando **_dropIndex_** em um banco de dados ou em uma coleção.
+ `fsync`: Possibilidade de poder executar o commando **_fsync_** em um **cluster**.
+ `getParameter`: Possibilidade de poder executar o commando **_getParameter_** em um **cluster**.
+ `hostInfo`: Possibilidade de poder ver informações sobre o servidor onde a instância do MongoDB está rodando em um **cluster**.
+ `logRotate`: Possibilidade de poder executar o commando **_logRotate_** em um **cluster**.
+ `reIndex`: Possibilidade de poder executar o commando **_reIndex_** em um banco de dados ou em uma coleção.
+ `renameCollectionSameDB`: Possibilidade de poder renomear uma coleção no banco de dados atual.
+ `repairDatabase`: Possibilidade de poder executar o commando **_repairDatabase_** num banco de dados.
+ `setParameter`: Possibilidade de poder executar o commando **_setParameter_** em um **cluster**.
+ `shutdown`: Possibilidade de poder executar o commando **_shutdown_** em um **cluster**.
+ `touch`: Possibilidade de poder executar o commando **_stouch_** em um **cluster**.



#### Ações de Diagnóstico
> + `collStats`: Possibilidade de poder executar o commando **_collStats_** em um banco de dados ou em uma coleção.
+ `connPoolStats`: Possibilidade de poder executar od commandod **_connPoolStats_** e **_shardConnPoolStats_** em um **cluster**.
+ `cursorInfo`: Possibilidade de poder executar o commando **_cursorInfo_** em um **cluster**.
+ `dbHash`: Possibilidade de poder executar o commando **_dbHash_** em um banco de dados ou em uma coleção.
+ `dbStats`: Possibilidade de poder executar o commando **_dbStats_** em um banco de dados ou em uma coleção.
+ `diagLogging`: Possibilidade de poder executar o commando **_diagLogging_** em um **cluster**.
+ `getCmdLineOpts`: Possibilidade de poder executar o commando `getCmdLineOpts` em um **cluster**.
+ `getLog`: Possibilidade de poder executar o commando **_getLog_** em um **cluster**.
+ `indexStats`: Possibilidade de poder executar o commando **_indexStats_** em um banco de dados ou em uma coleção.
+ `listDatabases`: Possibilidade de poder executar o commando **_listDatabases_** em um **cluster**.
+ `listCollections`: Possibilidade de poder executar o commando **_listCollections_** em um banco de dados.
+ `listIndexes`: Possibilidade de poder executar o commando **_ListIndexes_** em um banco de dados ou em uma coleção.
+ `netstat`: Possibilidade de poder executar o commando **_netstat_** em um **cluster**.
+ `serverStatus`: Possibilidade de poder executar o commando **_serverStatus_** em um **cluster**.
+ `validate`: Possibilidade de poder executar o commando **_validate_** em um banco de dados ou em uma coleção.
+ `top`: Possibilidade de poder executar o commando **_top_** em um **cluster**.




#### Ações Internas
> + `anyAction`: Possibilidade de poder executar qualquer ação num recurso.  
_Não use esse papel, exceto em circunstâncias excepcionais_.
+ `internal`: usuário pode executar qualquer ação interna.  
_Não use esse papel, exceto em circunstâncias excepcionais_.s