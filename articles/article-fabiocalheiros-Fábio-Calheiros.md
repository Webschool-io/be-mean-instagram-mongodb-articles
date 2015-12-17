# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Fábio Calheiros
**Data** 1450143505024

## Qual a diferença entre Autenticação e Autorização?

A principio, para que temos acesso a autorização, é necessário realizar a autenticação. Ou seja:

Autenticação é a validação do seu acesso ao banco de dados, seja ela por meio de cripitografia ou não.
Autorização é o acesso que você possui após a autenticação no banco.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Para criar um usuário administrador, devemos passar como parâmetro em roles a role "userAdminAnyDatabase"

db.createUser({
    user: "fabiocalheiros",
    pwd: "admin123",
    roles: [{role: "userAdminAnyDatabase", db: "be-mean-projeto"}]
})

Para criar um usuário comum, basta rodarmos o comando createUser passando como parâmetro a role "read"

 db.createUser({
    user: "fabiocalheiros",
    pwd: "user123",
    roles: [{role: "read", db: "admin"}]
})

## Explique cada papel listado em Cluster Administration Roles.

Cluster Administration Roles tem o poder de administrar todo o sistema, ao invés de somente um banco de dados, não se limitando a réplicas e shardings.

1 - clusterAdmin
Fornece o maior acesso de gerenciamento de cluster. Este papel combina os privilégios que serão citados e mostrados abaixo

2 - clusterManager
Fornece gerenciamento e ações de monitoramento no cluster. Um usuário com essa função pode acessar a configuração e bancos de dados locais, que são usados em sharding e replicação, respectivamente.
Fornece as seguintes ações no cluster como um todo:

    addShard
    applicationMessage
    cleanupOrphaned
    flushRouterConfig
    listShards
    removeShard
    replSetConfigure
    replSetGetStatus
    replSetStateChange
    resync

Fornece as seguintes ações em todos os bancos de dados no cluster:

    enableSharding
    moveChunk
    splitChunk
    splitVector

No banco de dados de configuração, oferece as seguintes ações nas configurações da coleção:

    insert
    remove
    update

No banco de dados de configuração, oferece as seguintes ações em todas as coleções de configuração e sobre as system.indexes, system.js e system.namespaces coleções:

    collStats
    dbHash
    dbStats
    find
    killCursors

No banco de dados local, fornece as seguintes ações na coleção replset:

    collStats
    dbHash
    dbStats
    find
    killCursors


3 - clusterMonitor
Fornece acesso somente de leitura para ferramentas de monitoramento, como o MongoDB Cloud Manager monitoring agent.
Fornece as seguintes ações no cluster como um todo:

    connPoolStats
    cursorInfo
    getCmdLineOpts
    getLog
    getParameter
    getShardMap
    hostInfo
    inprog
    listDatabases
    listShards
    netstat
    replSetGetStatus
    serverStatus
    shardingState
    top


Fornece as seguintes ações em todos os bancos de dados no cluster:

    collStats
    dbStats
    getShardVersion

Oferece também a ação find em todos as coleções system.profile no cluster.

Fornece as seguintes ações nas configurações das coleções do banco de dados system.index, system.js e system.namespaces:
	
    collStats
    dbHash
    dbStats
    find
    killCursors


4 - hostManager
Fornece a capacidade de monitorar e gerenciar servidores.
Fornece as seguintes ações no cluster como um todo:

    applicationMessage
    closeAllDatabases
    connPoolSync
    cpuProfiler
    diagLogging
    flushRouterConfig
    fsync
    invalidateUserCache
    killop
    logRotate
    resync
    setParameter
    shutdown
    touch
    unlock

Fornece as seguintes ações em todos os bancos de dados no cluster:

    killCursors
    repairDatabase

## Explique todas as ações de privilégio listadas em Privilege Actions.

find
O usuário poderá executar o comando .find() para pesquisar e obter retornos de uma coleção

insert
O usuário poderá executar o comando .insert() para inserir novos dados na coleção

remove
O usuário poderá executar o comando .remove() para remover itens da coleção

update
O usuário poderá executar o comando .update() para atualizar itens na coleção
