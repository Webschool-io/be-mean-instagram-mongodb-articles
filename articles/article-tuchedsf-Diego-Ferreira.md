# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Diego Ferreira
**Data** 07/12/2015 14:54

## Qual a diferença entre Autenticação e Autorização?

Autenticação trata da autenticidade do usuário, uma forma de garantir que um usário ou pessoa seja realmente quem ele é, evitando assim que outros se passem pela pessoa. Ou seja, uma forma de se confirmar que o usuário em questão é realmente autêntico, verificando e confirmando a identidade do mesmo.

Autorização, a autorização do usuário se diz respeito ao oque o mesmo pode realizar dentro de um sistema, ou seja, qual a funcionalidade que o mesmo estando autenticado ou não pode realizar.


## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Primeiramente para se criar um usuário no MongoDB é necessário ter um usuário criado na admin database com o  papel:
- userAdminAnyDatabase (permite criar usuario etc, em qualquer database exitente no banco) ou
- userAdmin ( permite criar usuários apenas no banco selecionado.).


Passos comuns para criação de usuários:
- mongo //Conectar ao mongo.
- use admin //conectar a base de dados admin

Passos específicos de cada usuário:
- Usuário administrador
  db.createUser(
  {
    user: "diegoAdmin",
    pwd: "12345",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
  )

```
Air-de-Diego(mongod-3.0.7) Admin> use admin
switched to db admin
Air-de-Diego(mongod-3.0.7) admin> db.createUser(
...   {
...     user: "diegoAdmin",
...     pwd: "12345",
...     roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
...   }
...   )
Successfully added user: {
  "user": "diegoAdmin",
  "roles": [
    {
      "role": "userAdminAnyDatabase",
      "db": "admin"
    }
  ]
}
```  
- Usuário Comum
Para se criar um usuário comum o procedimento é identico ao de se criar um usuário administrador, o que muda são os papeis (roles) que este  ira possuir.


db.createUser(
  {
    user: "teste",
    pwd: "abc123",
    roles: [ { role: "read", db: "admin" } ]
  }
)


```
Air-de-Diego(mongod-3.0.7) admin> db.createUser(
...   {
...     user: "teste",
...     pwd: "abc123",
...     roles: [ { role: "read", db: "admin" } ]
...   }
... )
Successfully added user: {
  "user": "teste",
  "roles": [
    {
      "role": "read",
      "db": "admin"
    }
  ]
}
```

## Explique cada papel listado em Cluster Administration Roles.

O MongoDB possuim uma série de papeis (roles) que são utilizados para administrar todo o sistema, não somente a base admin, estes papeis sao conhecidos como Cluster Administration Roles, os quais iremos falar u pouco mais abaixo:

- clusterAdmin : É o maior previlégio existente, ele é uma combinação de todos os papéis (clusterManager, clusterMonitor, hostManager) em um único papel. Adicionalmente ele possui o papel (role) dropDatabase.

- clusterManager: Aqui neste papel estao agrupados todos os papeis gerenciamente e monitoramento do cluster e da base de dados em um cluster. Inclui praticamente todos os papéis por exemplo exemplo: 'addShard', 'listShards', 'replSetConfigure', 'resync', 'insert', 'update', 'remove', entre outros.

- clusterMonitor: Prove o acesso de leitura para os papeis de monitoramento existentes no cluster, como por exemplo: 'getLog', 'hostInfo', 'getShardMap', 'listDatabases', 'serverStatus' entre outros.

- hostManager: Prove o serviço de monitoramento para o Monitor e gerenciamento do servidor. Exemplo: 'resync', 'shutdown', 'setParameter', etc...


## Explique todas as ações de privilégio listadas em Privilege Actions.

As ações de privilégio, definem as operações que um usuário pode efetuar sobre o recurso. No MongoDb estas ações podem estar agrupadas em outros papéis, como por exemplo o 'clusterAdmin' da sessão anterior que engloba várias destas ações.

Segue abaixo resumidamente a finalidade de cada uma destas ações:

###### Ações de pesquisa e escrita

- find -> realizar a busca
- insert ->  inserir um registro em uma coleção
- remove -> remover um registro de uma coleção.
- update -> atualizar um registro de uma coleção.

###### Ações de Gerenciamento da Base de dados
- changeCustomData -> Realizar alteração no campo customData de qualquer usuário
- changeOwnCustomData -> Realizar alteração no campo customData apenas do seu usuário
- changeOwnPassword -> Realizar alteração do próprio password
- changePassword -> realizar alteraçao de qualquer password na Base
- createCollection -> criar uma nova coleção na base
- createIndex -> criar um novo índice na coleção
- createRole -> criar um novo papel na database.
- createUser -> criar novo usuário na base.
- dropCollection ->  excluir/apagar uma coleção da base de dados.
- dropRole -> deletar uma role existente na base de dados.
- dropUser -> deleter um usuário da base de dados.
- emptycapped -> Remove todos os documntos de uma Capped Colection ( Conjunto circular de coleção que registra as atividades de uma coleçao na forma em que acontecem, muito bem ordenadas e com extrema rapidez de gravaçao e leitura sequencial de dados. Muito utilizado para Logs )
- enableProfiler -> alterar o perfil do banco de dados para capturar dados sobre desempenho.
- grantRole -> permite dar permissão em qualquer papel para qualquer usuário na base de dados.
- killCursors -> deleta todos os cursores de uma coleção.
- revokeRole -> permite retirar um papel de qualquer usuário da base de dados.
- unlock -> Um recurso do cluster, para habilitar escrita e reverçao de operações.
- viewRole -> Vizualizar informação sobre uma um papel(role) existente na base de dados.
- viewUser -> visualizar as informações de um usuário na base de dados.

###### Ações de Gerenciamento de Implantação
- authSchemaUpgrade -> Disponivel para clusters. Atualizaçao do processo de Autenticação e Autorização dos usuários.
- cleanupOrphaned -> Deletar documentos orfãos dos shards.
- cpuProfiler -> Habilitar o uso de cpu Profiler.
- inprog -> Retorna as operações que estão em execução ou pendentes.
- invalidateUserCache -> Cluster ação. Limpa o cache com as informações dos usuários.
- killop -> Matar uma operaçao que esteja sendo executada.
- planCacheRead -> Exibe plano da query para uma query especificada.
- planCacheWrite -> Escrever um cache com o plano das querys que estao sendo executadas.
- storageDetails -> Exibe os detalhes de storage do banco de dados.

###### Ações de Replicação
- appendOplogNote -> Permite incluir notas no oplog.
- replSetConfigure -> permite configurar uma replica set em um cluster.
- replSetGetStatus -> permite verificar o status de uma replica em um cluster.
- replSetHeartbeat -> Monitora a "saúde" de um node. Aplicado em um cluster.
- replSetStateChange -> Aplicado em um cluster. Alterar o estado de uma replica set.
- resync -> Aplicado em um cluster. Força a re-sincronização de uma instancia escrava no mongo db que esteja fora de sincronia.

##### Ações de Sharding
- addShard -> Adiciona um novo shard ao cluster.
- enableSharding -> Habilitar um shard na base de dados.
- flushRouterConfig -> Limpa o cache com as informações correntes do cluster.
- getShardMap -> Exibe o mapa de shards em um cluster.
- getShardVersion -> Exibe a versao dos shards em um cluster.
- listShards -> Lista todos os shards em um cluster.
- moveChunk -> Realiza a movimentaçao de chunks entre os Shards.
- removeShard -> remove um shard do cluster.
- shardingState -> comando para vizualizar o estado de um shard.
- splitChunk -> Compando para dividir um chunk existente.
- splitVector -> comando para dividir um vetor.

##### Ações de Administração do Servidor
- applicationMessage -> Ação de cluster para configuraçao de mensagem de log da aplicaçao.
- closeAllDatabases -> Fecha todas as bases de dados. Ação de cluster.
- collMod -> Adicionar flag em uma coleção para poder mudar seu comportamento.
- compact -> rescrever e desfragmentar todos os dados e indices de uma coleção.
- connPoolSync -> Ação de cluster para sincronizar o pool de conexoes.
- convertToCapped -> converter uma coleçao existente em uma Capped Colection.
- dropDatabase -> Eliminar uma base de dados.
- dropIndex ->  Eliminar um indice.
- fsync -> "Trava" o MongoDB e bloqueia as operações de escrita para captura de backups.
- getParameter -> recuperar o valor das opção que esta setada.
- hostInfo -> exibe informações do host que o mongo esta sendo executado.
- logRotate -> rotaciona os logs para evitar que um único arquivo de log ocupe um grande espaço na base de dados.
- reIndex -> deleta e recria todos os indices da base de dados.
- renameCollectionSameDB -> Modifica o nome de uma coleção existente.
- repairDatabase -> checa e repara erros e inconsistencias na base de dados.
- setParameter -> setar o valor de parametro para o banco de dados.Disponivel para cluster.
- shutdown -> Limpa todos os recursos da base de dados e termina o processo.
- touch -> carrega os dados de armazenamento para memória.

##### Ações de Diagnóstico
- collStats -> retorna as estatisticas de armazenamento de uma coleção.
- connPoolStats -> retorna o número de conexões abertas na instancia atual da base de dados.
- cursorInfo -> retorna informaçao sobre o cursor.
- dbHash -> retorna o hash do banco de dados.
- dbStats -> retorna as estatisticas de armazenamento da base de dados.
- diagLogging -> retorna informaçoes adicionais para log dos dados presentes no banco de dados.
- getCmdLineOpts -> retorna um documento com dois parametros, argv and parsed. Argv um array de comando usado para invocar o mongo ou mongos. Parsed, inlui todas as opçoes de execuçao.
- getLog -> retorna um documento com o log das mensagens mais recentes dos processos do MongoDB.
- indexStats -> mostra o stats de um indice
- listDatabases -> exibe uma lista sobre todas as base de dados existentes no servidor e uma estatisticas basica sobre cada uma.
- listCollections -> exibe uma lista sobre todas as coleções existentes no servidor e uma estatisticas basica sobre cada uma.
- listIndexes -> exibe uma lista sobre todos os indices existentes no servidor e uma estatisticas basica sobre cada um.
- netstat -> utilizado pra verificar as conexoes e portas do MongoDB
- serverStatus -> retorna um documento com as informações dos processos atuais da base de dados.
- validate -> checa as estruturas dos namespaces para correçao a partir do scaneamento da das coleçoes e indices.
- top -> retorna as estatisticas de uso de uma determinada coleçao.

##### Ações Internas
- anyAction
- internal


##Bibliografia
https://docs.mongodb.org/manual/tutorial/manage-users-and-roles/
https://docs.mongodb.org/manual/reference/built-in-roles/#cluster-administration-roles
https://docs.mongodb.org/manual/reference/privilege-actions/
https://mongodbwise.wordpress.com/2014/05/15/databases-documents-and-collections-no-mongodb/comment-page-1/
