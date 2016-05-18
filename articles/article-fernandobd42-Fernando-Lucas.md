# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Fernando Lucas
**Data** Date.now() //em timestamp


## Qual a diferença entre Autenticação e Autorização?
Autenticação verifica a identidade do usuário; Autorização determina o acesso e quais funcionalidades o usuário(após ter se identificado) poderá executar.


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
Para criar um usuário administrador, apenas deve-se adicionar a permissão userAdmin ou userAdminAnyDatabase. Tendo em vista que o userAdminAnyDatabase tem as mesmas permissões do userAdmin, todavia o userAdminAnyDatabase é aplicavel a todos os bancos no cluster.

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

E, para criar um usuário comum, apenas deve-se adicionar a permissão readWrite ou readWriteAnyDatabase. Tendo em vista que o readWriteDatabase tem as mesmas permissões do readWrite, todavia o readWriteAnyDatabase é aplicavel a todos os bancos no cluster.
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


## Explique cada papel listado em Cluster Administration Roles.
clusterAdmin: Provê acesso ao maior nível de acesso de gerenciamento de um cluster, sendo o unico com permissão de executar a ação dropDatabase.

clusterManager: Provê acesso ao gerenciamento e monitoramento de um cluster, podendo executar quase todas as funcionalidades do banco exceto pela ação dropDatabase que é restrita ao clusterAdmin.

clusterMonitor: Provê acesso somente a leitura e monitoramento de um cluster.

hostManager: Permite monitorar e gerenciar servidores.


## Explique todas as ações de privilégio listadas em Privilege Actions.

Ações de Escrita e Busca

find: Utilizado para buscar uma determinada coleção.
insert: Utilizado para inserir registros em uma determinada coleção.
remove: Utilizado para remover registros de uma determinada coleção.
update: Utilizado para atualizar uma determinada coleção.



Ações de Gerenciamento de Banco de Dados

changeCustomData: Utilizado para alterar o customDate de qualquer usuário;
changeOwnCustomData: Utilizado para alterar o customDate do seu usuário;
changeOwnPassword: Utilizado para mudar sua senha de acesso;
changePassword: Utilizado para mudar a senha de qualquer usuário;
createCollection: Utilizado para criar uma collection;
createIndex: Utilizado para criar um index;
createRole: Utilizado para criar um role;
createUser: Utilizado para criar um user;
dropCollection: Utilizado para excluir uma collection;
dropRole: Utilizadoinse para excluir um role;
dropUser: Utilizado para excluir um user;
emptycapped: Utilizado para excluir todos os dados de uma Capped Collection;
enableProfiler: Utilizado para alterar o perfil do banco de dados para capturar dados de desempenho.
grantRole: Utilizado para conceder atribuir um papel(role) para qualquer usuário;
killCursors: Utilizado para derrubar o cursor de uma collection;
revokeRole: Utilizado para remover um papel(role) de qualquer usuário;
unlock: Utilizado para desbloquear uma instância do mongod para escrita (geralmente usado após uma operação de backup);
viewRole: Utilizado para ver informações de qualquer role;
viewUser: Utilizado para ver informações de qualquer usuário;



Ações de Gerenciamento de Deployment

authSchemaUpgrade: Utilizado para atualizar o esquema de autenticação e autorização de um cluster;
cleanupOrphaned: Utilizado para remover dados que não pertencem a um shard;
cpuProfiler: Utilizado para habilitar o uso do profile da CPU;
inprog: Utilizado para verificar operações ativas e pendentes num cluster;
invalidateUseCache: Utilizado para 'limpar' da memória (cache) com as informações de usuário.
killop: Utilizado para matar operações num cluster;
planCacheRead: Utilizado para listar planos de query em cache;
planCacheWrite: Utilizado para remover os planos de query em cache;
storageDetails: Utilizado para exibir os detalges de armazenamento(storage) do banco;



Ações de Replicação

appendOplogNote: Utilizado para adicionar algumas notas ao oplog no cluster;
replSetConfigure: Utilizado para configurar uma replica_set no cluster;
replSetGetStatus: Utilizado para ver o status atual da replica no cluster;
replSetHeartbeat: Utilizado para determinadas funcionalidades em um conjunto de replicas;
replSetStateChange: Utilizado para alterar/gerenciar o estado atual da réplica no cluster;
resync: Utilizado para resincronizar;



Ações de Sharding

addShard: Utilizado para adicionar um shard;
enableSharding: Utilizado para habilitar um shard;
flushRouterConfig: Limpar o cahce com as informações do cluster
getShardMap: Utilizado para exibir o mapa de shards;
getShardVersion: Utilizado para exibir a versão dos shards ;
listShards: Utilizado para listar todos os shards em um cluster;
moveChunk: Utilizado para realizar a movimentação de chunks entre os shards;
removeShard: Utilizado para remover um shard em um cluster;
shardingState: Utilizado para visualizar o estado de uma shard;
splitChunk: Utilizado para devidir um chunk;
splitVector: Utilizado para dividir um vetor;



Ações de Administração de Servidor

applicationMessage: Utilizado para configuração de mensagem de log da aplicação;
closeAllDatabases: Utilizado para fechar todos os cursores;
collMod: Utilizado para adicionar flag em uma collection;
compact: Utilizado para desfragmentar todos os dados e índices de uma colletion;
connPoolSync: Utilizado para sincronizar o pool de conexões;
convertToCapped: Utilizado para converter uma collection em uma capped collection;
dropDatabase: Utilizado para excluir um banco de dados;
dropIndex: Utilizado para excluir um índice;
fsync: Utilizado para pausar o Mongo, bloqueando a entrada de dados para captura de backups;
getParameter: Utilizado para recuperar o valor de um parametro;
hostInfo: Utilizado para exibir informações do host onde o MongoDB esta sendo executado;
logRotate: Utilizado para evitar que um único arquivo de log ocupe um grande espaço no banco;
reIndex: Utilizado para deletar e recriar todos os índeces da base de dados;
renameCollectionSameDB: Utilizado para modificar o nome de uma collection;
repairDatabase:Utilizado para verificar e reparar erros e inconsistências no banco de dados;
setParameter: Utilizado para definir o valor de um parâmetro;
shutdown: Utilizado para desligar/finalizar o processo do banco de dados;
touch: Utilizado para carregar os dados de armazenamento para a memória;



Ações de Diagnóstico

collStats: Utilizado para exibir as estatísticas de armazenamento de uma coleção;
connPoolStats: Utilizado para exibir o número de coleções abertas;
cursorInfo: Utilizado para exibir informações do cursor;
dbHash: Utilizado para exibir a hash;
dbStats: Utilizado para exibir as estatísticas de armazenamento de um banco de dados;
diagLogging: Utilizado para exibir informações adicionais para o log dos dados;
getCmdLineOpts: Utilizado para retornar um documento com dois parâmetros;
getLog: Utilizado para retornar um documento com o log das menssagens mais recentes;
indexStats: Utilizado para exibir o status de um índice;
listDatabases: Utilizado para exibir uma lista sobre todos os bancos(Databases) existentes no servidos;
listCollections: Utilizado para exibir uma lista sobre todas as coleções(Collections) existentes no servidor;
listIndexes: Utilizado para exibir uma lista sobre todos os índices existentes no servidor;
netstat: Utilizado para verificar as conexões e portas do MongoDB;
serverStatus: Utilizado para retornar um documento com as informações dos processos atuais;
validate: Utilizado para verificar as estruturas dos namespaces para a correção;
top: Utilizado para retornar as estatísticas de uso de uma determinada coleção;



Ações Internas

anyAction: Utilizado para permitir qualquer ação em um recurso. Não atribua essa permissão, exceto em caso especiais.
internal: Utilizado para permitir ações internas. Não atribua essa permissão, exceto em caso especiais.
