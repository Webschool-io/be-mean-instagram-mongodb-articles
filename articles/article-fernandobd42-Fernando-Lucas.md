# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Fernando Lucas <br>
**Data** 1474983070656


## Qual a diferença entre Autenticação e Autorização?
Autenticação verifica a identidade do usuário; Autorização determina o acesso e quais funcionalidades o usuário(após ter se identificado) poderá executar.


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
Para criar um usuário administrador, apenas deve-se adicionar a permissão userAdmin ou userAdminAnyDatabase. Tendo em vista que o userAdminAnyDatabase tem as mesmas permissões do userAdmin, todavia o userAdminAnyDatabase é aplicavel a todos os bancos no cluster.

```
> use be-mean
> use admin
> db.createUser(
    {
        user: 'fernando',
        pwd: 'qwe123',
        roles: ['userAdminAnyDatabase']
    },
    { w: 'majority', wtimeout: 5000 }
)
```

E, para criar um usuário comum, apenas deve-se adicionar a permissão readWrite ou readWriteAnyDatabase. Tendo em vista que o readWriteDatabase tem as mesmas permissões do readWrite, todavia o readWriteAnyDatabase é aplicavel a todos os bancos no cluster.

```
> use be-mean
> use admin
> db.createUser(
    {
        user: 'michel',
        pwd: 'qwe123',
        roles: ['readWriteAnyDatabase']
    },
    { w: 'majority', wtimeout: 5000 }
)
```

## Explique cada papel listado em Cluster Administration Roles.
* <i>clusterAdmin:</i> Provê acesso ao maior nível de acesso de gerenciamento de um cluster, sendo o unico com permissão de executar a ação dropDatabase.

* <i>clusterManager:</i> Provê acesso ao gerenciamento e monitoramento de um cluster, podendo executar quase todas as funcionalidades do banco exceto pela ação dropDatabase que é restrita ao clusterAdmin.

* <i>clusterMonitor:</i> Provê acesso somente a leitura e monitoramento de um cluster.

* <i>hostManager:</i> Permite monitorar e gerenciar servidores.


## Explique todas as ações de privilégio listadas em Privilege Actions.

<b>Ações de Escrita e Busca</b>

* <i>find:</i> Utilizado para buscar uma determinada coleção.
* <i>insert:</i> Utilizado para inserir registros em uma determinada coleção.
* <i>remove:</i> Utilizado para remover registros de uma determinada coleção.
* <i>update:</i> Utilizado para atualizar uma determinada coleção.


<b>Ações de Gerenciamento de Banco de Dados</b>

* <i>changeCustomData:</i> Utilizado para alterar o customDate de qualquer usuário;
* <i>changeOwnCustomData:</i> Utilizado para alterar o customDate do seu usuário;
* <i>changeOwnPassword:</i> Utilizado para mudar sua senha de acesso;
* <i>changePassword:</i> Utilizado para mudar a senha de qualquer usuário;
* <i>createCollection:</i> Utilizado para criar uma collection;
* <i>createIndex:</i> Utilizado para criar um index;
* <i>createRole:</i> Utilizado para criar um role;
* <i>dropCollection:</i> Utilizado para excluir uma collection;
* <i>createUser:</i> Utilizado para criar um user;
* <i>dropRole:</i> Utilizadoinse para excluir um role;
* <i>dropUser:</i> Utilizado para excluir um user;
* <i>emptycapped:</i> Utilizado para excluir todos os dados de uma Capped Collection;
* <i>enableProfiler:</i> Utilizado para alterar o perfil do banco de dados para capturar dados de desempenho.
* <i>grantRole:</i> Utilizado para conceder atribuir um papel(role) para qualquer usuário;
* <i>killCursors:</i> Utilizado para derrubar o cursor de uma collection;
* <i>revokeRole:</i> Utilizado para remover um papel(role) de qualquer usuário;
* <i>unlock:</i> Utilizado para desbloquear uma instância do mongod para escrita (geralmente usado após uma operação de backup);
* <i>viewRole:</i> Utilizado para ver informações de qualquer role;
* <i>viewUser:</i> Utilizado para ver informações de qualquer usuário;


<b>Ações de Gerenciamento de Deployment</b>

* <i>authSchemaUpgrade:</i> Utilizado para atualizar o esquema de autenticação e autorização de um cluster;
* <i>cleanupOrphaned:</i> Utilizado para remover dados que não pertencem a um shard;
* <i>cpuProfiler:</i> Utilizado para habilitar o uso do profile da CPU;
* <i>inprog:</i> Utilizado para verificar operações ativas e pendentes num cluster;
* <i>invalidateUseCache:</i> Utilizado para 'limpar' da memória (cache) com as informações de usuário.
* <i>killop:</i> Utilizado para matar operações num cluster;
* <i>planCacheRead:</i> Utilizado para listar planos de query em cache;
* <i>planCacheWrite:</i> Utilizado para remover os planos de query em cache;
* <i>storageDetails:</i> Utilizado para exibir os detalges de armazenamento(storage) do banco;


<b>Ações de Replicação</b>

* <i>appendOplogNote:</i> Utilizado para adicionar algumas notas ao oplog no cluster;
* <i>replSetConfigure:</i> Utilizado para configurar uma replica_set no cluster;
* <i>replSetGetStatus:</i> Utilizado para ver o status atual da replica no cluster;
* <i>replSetHeartbeat:</i> Utilizado para determinadas funcionalidades em um conjunto de replicas;
* <i>replSetStateChange:</i> Utilizado para alterar/gerenciar o estado atual da réplica no cluster;
* <i>resync:</i> Utilizado para resincronizar;


<b>Ações de Sharding</b>

* <i>addShard:</i> Utilizado para adicionar um shard;
* <i>enableSharding:</i> Utilizado para habilitar um shard;
* <i>flushRouterConfig:</i> Limpar o cahce com as informações do cluster
* <i>getShardMap:</i> Utilizado para exibir o mapa de shards;
* <i>getShardVersion:</i> Utilizado para exibir a versão dos shards ;
* <i>listShards:</i> Utilizado para listar todos os shards em um cluster;
* <i>moveChunk:</i> Utilizado para realizar a movimentação de chunks entre os shards;
* <i>removeShard:</i> Utilizado para remover um shard em um cluster;
* <i>shardingState:</i> Utilizado para visualizar o estado de uma shard;
* <i>splitChunk:</i> Utilizado para devidir um chunk;
* <i>splitVector:</i> Utilizado para dividir um vetor;


<b>Ações de Administração de Servidor</b>

* <i>applicationMessage:</i> Utilizado para configuração de mensagem de log da aplicação;
* <i>closeAllDatabases:</i> Utilizado para fechar todos os cursores;
* <i>collMod:</i> Utilizado para adicionar flag em uma collection;
* <i>compact:</i> Utilizado para desfragmentar todos os dados e índices de uma colletion;
* <i>connPoolSync:</i> Utilizado para sincronizar o pool de conexões;
* <i>convertToCapped:</i> Utilizado para converter uma collection em uma capped collection;
* <i>dropDatabase:</i> Utilizado para excluir um banco de dados;
* <i>dropIndex:</i> Utilizado para excluir um índice;
* <i>fsync:</i> Utilizado para pausar o Mongo, bloqueando a entrada de dados para captura de backups;
* <i>getParameter:</i> Utilizado para recuperar o valor de um parametro;
* <i>hostInfo:</i> Utilizado para exibir informações do host onde o MongoDB esta sendo executado;
* <i>logRotate:</i> Utilizado para evitar que um único arquivo de log ocupe um grande espaço no banco;
* <i>reIndex:</i> Utilizado para deletar e recriar todos os índeces da base de dados;
* <i>renameCollectionSameDB:</i> Utilizado para modificar o nome de uma collection;
* <i>repairDatabase: </i>Utilizado para verificar e reparar erros e inconsistências no banco de dados;
* <i>setParameter: </i>Utilizado para definir o valor de um parâmetro;
* <i>shutdown: </i>tilizado para desligar/finalizar o processo do banco de dados;
* <i>touch: </i>Utilizado para carregar os dados de armazenamento para a memória;


</b>Ações de Diagnóstico</b>

* <i>collStats: </i>Utilizado para exibir as estatísticas de armazenamento de uma coleção;
* <i>connPoolStats: </i>Utilizado para exibir o número de coleções abertas;
* <i>cursorInfo: </i>Utilizado para exibir informações do cursor;
* <i>dbHash: </i>Utilizado para exibir a hash;
* <i>dbStats: </i>Utilizado para exibir as estatísticas de armazenamento de um banco de dados;
* <i>diagLogging: </i>Utilizado para exibir informações adicionais para o log dos dados;
* <i>getCmdLineOpts: </i>Utilizado para retornar um documento com dois parâmetros;
* <i>getLog: </i>Utilizado para retornar um documento com o log das menssagens mais recentes;
* <i>indexStats: </i>Utilizado para exibir o status de um índice;
* <i>listDatabases: </i>Utilizado para exibir uma lista sobre todos os bancos(Databases) existentes no servidos;
* <i>listCollections: </i>Utilizado para exibir uma lista sobre todas as coleções(Collections) existentes no servidor;
* <i>listIndexes: </i>Utilizado para exibir uma lista sobre todos os índices existentes no servidor;
* <i>netstat: </i>Utilizado para verificar as conexões e portas do MongoDB;
* <i>serverStatus: </i>Utilizado para retornar um documento com as informações dos processos atuais;
* <i>validate: </i>Utilizado para verificar as estruturas dos namespaces para a correção;
* <i>top: </i>Utilizado para retornar as estatísticas de uso de uma determinada coleção;


<b>Ações Internas</b>

* <i>anyAction: </i>Utilizado para permitir qualquer ação em um recurso. Não atribua essa permissão, exceto em caso especiais.
* <i>internal: </i>Utilizado para permitir ações internas. Não atribua essa permissão, exceto em caso especiais.
