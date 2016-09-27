# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Sóstenes Freitas de Andrade
**Data** > 14 Jun 2016

## Qual a diferença entre Autenticação e Autorização?

Autenticação: é um processo que procura verificar a identidade digital do usuário de um sistema, 
normalmente,  é feito no momento em que ele requisita um acesso em um programa ou computador.

Autorização:autorização verifica se o usuário tem permissão para executar determinadas tarefas.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

//Conecte no mongo

mongo

//escolha o banco

use be-mean

**Usuario Administrador:**
Criando um usuario administrador, passamos como parâmetro a role ( ͡° ͜ʖ ͡°): userAdminAnyDatabase.

db.createUser({
   user: 'root',
   pwd: 'miojo',
   roles: [ { role: 'userAdminAnyDatabase', db: 'be-mean' } ]
})

**Usuario Comum:**

Role que diferenciara a diferença entre os tipos de usuarios, definiremos o usuario comum com: read

db.createUser(
    {
      user: "miojo",
      pwd: "miojo",
      roles : [{ role : 'read', db : 'be-mean' }]
    }
)

## Explique cada papel listado em Cluster Administration Roles.

**clusterAdmin:**

Fornece o maior acesso de gerenciamento de cluster. Este papel combina os privilégios concedidos pelos
papéis clusterManager , clusterMonitor e hostManager . Além disso , o papel proporciona a acção de dropDatabase.

**clusterManager:**

Fornece gestão e ações de monitoramento no cluster. Um usuário com essa função pode acessar a configuração e bancos
de dados locais , que são utilizados na fragmentação e replicação , respectivamente.


**clusterMonitor:**

Fornece acesso somente leitura para ferramentas de monitoramento , como o agente de monitoramento Gestor MongoDB Cloud.

**hostManager:**

Fornece a capacidade de monitorar e gerenciar servidores.

## Explique todas as ações de privilégio listadas em Privilege Actions.


A função concede privilégios para executar conjuntos de tarefas sobre os recursos definidos. Pode conceder acesso a um nível de coleção, quando aplicado 
ao banco de dados no qual ele está definido.

**find:**

Faz uma busca dos documentos de uma collection.

**insert:**

Insere um ou mais documentos de uma coleção.

**remove:**

Remove um ou mais documentos de uma coleção

**update:**

Atualiza um ou mais documentos de uma coleção.

####Ações de gestão de banco de dados

**changeCustomData:**

Modifica a custom information de qualquer usuário da datase informada.

**changeOwnCustomData:**

Permite que o usuário altere suas próprias custom information.

**changeOwnPassword:**

Permite que o usuário mude sua própria senha.

**changePassword:**

Permite que o usuário modifique a senha de qualquer usuário da database informada.

**createCollection:**

Permite criar uma coloeção na database.

**createIndex:**

Permite criar um índice na database

**createRole:**

Permite criar novas roles na database.

**createUser:**

Permite criar novos usuários na database.

**dropCollection:**

Permite excluir uma coleção da database.

**dropRole:**

Permite excluir uma role da database.

**dropUser:**

Permite excluir usuário da database.

**emptycapped:**

Permite remover todos os documentos de uma coleção.

**enableProfiler:**

Modificar o nível de perfil do usuário atual para capturar dados sobre o seu desempenho.

**grantRole:**

Modifica uma role para qualquer usuário da database.

**killCursors:**

Permite matar cursores na database.

**revokeRole:**

Permite remover uma role de um usuário da database.

**unlock:**

Permite destravar um serviço para que seja acessado por uma operação de leitura ou gravação.

**viewRole:**

Permite ver informações sobre uma role da database.

**viewUser:**

Permive ver informações sobre um usuário.

####Ações de Gerenciamento de Deploying

**authSchemaUpgrade:**

Suporta o processo de atualização para sistemas existentes que usam autenticação e autorização entre as versões 2.4 e 2.6 e entre as versões 2.6 e 3.0.

**cleanupOrphaned:**

Permite excluir de um shard documentos órfãos cujo seus valores não pertençam a esse shard.

**cpuProfiler:**

Permite o usuário habilitar e usar o CPU profiler.

**inprog:**

Retorna operações ativas ou pendentes.

**invalidateUserCache:**

Libera informações do usuário do cache na memória RAM, incluindo a remoção de credenciais e papéis de cada usuário.

**killop:**

Mata uma determinada operação, identificada pelo seu id.

**planCacheRead:**

Visualiza os planos de list e querys do cache na database ou coleções.

**planCacheWrite:**

Remove os cached query plans da coleção.

**storageDetails:**

Permite executar o comando storageDetails.

####Ações de Replicação

**appendOplogNote:**

Permite adicionar notas ao oplog.

**replSetConfigure:**

Permite o usuário configurar uma replica set.

**replSetGetStatus:**

Permite ver o atual statis da replica set.

**replSetHeartbeat:**

Permite enviar um sinal para verificar se o replica set está ativo.

**replSetStateChange:**

Permite mudar o estado de uma replica set através dos comandos replSetFreeze, replSetMaintenance, replSetStepDown e replSetSyncFrom.

**resync:**

Permite resincronizar as réplicas primárias e secundárias.

####Ações de Sharding

**addShard:**

Permite adicionar um shard no cluster.

**enableSharding:**

Permite ativar um shard na database usando o comando enableSharding.

**flushRouterConfig:**

Permite limpar as informações do cluster em cache e faz o overload dos metadados.

**getShardMap:**

Permite mapear funcionalidades do shard.

**getShardVersion:**

Permite obter a versão do shard.

**listShards:**

Exibe a lista de shards.

**moveChunk:**

Permite mover os chunks entre os shards.

**removeShard:**

Remove um shard do cluster.

**shardingState:**

Mostra o estado do sharding.

**splitChunk:**

Permite dividir os chunks das coleções ou databases.

**splitVector:**

Permite executar operações de metadados nos clusters que possuem shards.

####Ações dos Administração de Servidores

**applicationMessage:**

Permite postar uma mensagem personalizada para o log de auditoria, através do comando logApplicationMessage.

**closeAllDatabases:**

Permite fechar todas as databases do cluster.

**collMod:**

Permite adicionar um conjunto de flags para modificar o comportamento do MongoDB.

**compact:**

Premite regravar e desfragmentar todos os dados e índices de uma coleção na database.

**connPoolSync:**

Permite sincronizar o pool do cluster.

**convertToCapped:**

Permite converter uma coleção not-capped para capped.

**dropDatabase:**

Permite excluir uma database.

**dropIndex:**

Permite excluir índices de uma coleção da database.

**fsync:**

Permite forçar o serviço mongod para que execute um flush em todas as escritas pendentes na camada de armazenamento em disco.

**getParameter:**

Permite recuperar o valor das opções configuradas anteriormente no cluster.

**hostInfo:**

Exibe informações sobre o server que o MongoDB está rodando.

**logRotate:**

Permite fazer uma rotação nos logs para que este não se concerte em um único arquivo, diminuindo o espaço necessário para o armazenamento.

**reIndex:**

Permite apagar e recriar índices da coleção.

**renameCollectionSameDB:**

Permite renomear uma coleção na database corrente.

**repairDatabase:**

Permite checar e reparar uma database com insconsistências no armazenamento dos dados.

**setParameter:**

Permite alterar alguns parâmetros do cluster.

**shutdown:**

Limpa todos as databases e encerra os processos.

**touch:**

Permite carregar os dados da camada de armazenamento de dados na memória.

####Ações de Diagnósticos

**collStats:**

Visualiza estatísticas de armazenamento de uma determina coleção.

**connPoolStats:**

Permite executar um comando interno no cluster.

**cursorInfo:**

Permite retornar informações sobre o cursor.

**dbHash:**

Permite executar comandos com suporte à algumas configurações do servidor.

**dbStats:**

Permite verificar estatísticas sobre o servidor.

**diagLogging:**

Permite retornar dados adicionais para diagnóstico.

**getCmdLineOpts:**

Permite executar o comando o comando getCmdLineOpts no cluster.

**getLog:**

Permite executar o comando getLog no cluster.

**indexStats:**

Descontinuado na versão 3.0 do MongoDB, permite retornar estatísticas sobre índices de coleções da database.

**listDatabases:**

Permite listar estatísticas das databases.

**listCollections:**

Permite listar estatísticas das coleções da database.

**listIndexes:**

Permite listar os índices da coleção.

**netstat:**

Permite utilizar o comando netstat.

**serverStatus:**

Permite visualizar estatísticas sobre o status do servidor.

**validate:**

Permite verificar estruturar de nomes de coleções e/ou índices.

**top:**

Permite retornar estatísticas de uso para cada coleção da database.

####Ações Internas

**anyAction:**

Permite uma ação em um recurso. Não utilize essa ação a não ser em circunstâncias excepcionais.

**internal:**

Pèrmite ações internas. Não utilize essa ação a não ser em circunstâncias excepcionais.














