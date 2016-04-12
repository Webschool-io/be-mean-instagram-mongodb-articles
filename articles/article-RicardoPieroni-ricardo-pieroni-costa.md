# MongoDb - Artigo sobre Autentica��o no MongoDb
**Autor:** Ricardo Pieroni Costa
**Data** > Fri Apr 08 2016 09:49:26 GMT-0300 (Hora oficial do Brasil)


## Qual a diferen�a entre Autentica��o e Autoriza��o?

**Autentica��o:** � um processo que procura verificar a identidade digital do usu�rio de um sistema, 
normalmente,  � feito no momento em que ele requisita um acesso em um programa ou computador.

**Autoriza��o:**autoriza��o verifica se o usu�rio tem permiss�o para executar determinadas tarefas.

## Descreva aqui o passo-a-passo como criar um usu�rio administrador e um usu�rio comum.

//Conecte no mongo

mongo

//escolha o banco

use be-mean

**Usuario Administrador:**
Criando um usuario administrador, passamos como par�metro a role: userAdminAnyDatabase.

db.createUser({
   user: 'usuario',
   pwd: 'senha',
   roles: [ { role: 'userAdminAnyDatabase', db: 'be-mean' } ]
})

**Usuario Comum:**

Role que diferenciara a diferen�a entre os tipos de usuarios, definiremos o usuario comum com: read

db.createUser(
    {
      user: "usuario",
      pwd: "senha",
      roles : [{ role : 'read', db : 'be-mean' }]
    }
)

## Explique cada papel listado em Cluster Administration Roles.

**clusterAdmin:**

Fornece o maior acesso de gerenciamento de cluster. Este papel combina os privil�gios concedidos pelos
pap�is clusterManager , clusterMonitor e hostManager . Al�m disso , o papel proporciona a ac��o de dropDatabase.

**clusterManager:**

Fornece gest�o e a��es de monitoramento no cluster. Um usu�rio com essa fun��o pode acessar a configura��o e bancos
de dados locais , que s�o utilizados na fragmenta��o e replica��o , respectivamente.


**clusterMonitor:**

Fornece acesso somente leitura para ferramentas de monitoramento , como o agente de monitoramento Gestor MongoDB Cloud.

**hostManager:**

Fornece a capacidade de monitorar e gerenciar servidores.

## Explique todas as a��es de privil�gio listadas em Privilege Actions.


A fun��o concede privil�gios para executar conjuntos de tarefas sobre os recursos definidos. Pode conceder acesso a um n�vel de cole��o, quando aplicado 
ao banco de dados no qual ele est� definido.

**find:**

Faz uma busca dos documentos de uma collection.

**insert:**

Insere um ou mais documentos de uma cole��o.

**remove:**

Remove um ou mais documentos de uma cole��o

**update:**

Atualiza um ou mais documentos de uma cole��o.

####A��es de gest�o de banco de dados

**changeCustomData:**

Modifica a custom information de qualquer usu�rio da datase informada.

**changeOwnCustomData:**

Permite que o usu�rio altere suas pr�prias custom information.

**changeOwnPassword:**

Permite que o usu�rio mude sua pr�pria senha.

**changePassword:**

Permite que o usu�rio modifique a senha de qualquer usu�rio da database informada.

**createCollection:**

Permite criar uma coloe��o na database.

**createIndex:**

Permite criar um �ndice na database

**createRole:**

Permite criar novas roles na database.

**createUser:**

Permite criar novos usu�rios na database.

**dropCollection:**

Permite excluir uma cole��o da database.

**dropRole:**

Permite excluir uma role da database.

**dropUser:**

Permite excluir usu�rio da database.

**emptycapped:**

Permite remover todos os documentos de uma cole��o.

**enableProfiler:**

Modificar o n�vel de perfil do usu�rio atual para capturar dados sobre o seu desempenho.

**grantRole:**

Modifica uma role para qualquer usu�rio da database.

**killCursors:**

Permite matar cursores na database.

**revokeRole:**

Permite remover uma role de um usu�rio da database.

**unlock:**

Permite destravar um servi�o para que seja acessado por uma opera��o de leitura ou grava��o.

**viewRole:**

Permite ver informa��es sobre uma role da database.

**viewUser:**

Permive ver informa��es sobre um usu�rio.

####A��es de Gerenciamento de Deploying

**authSchemaUpgrade:**

Suporta o processo de atualiza��o para sistemas existentes que usam autentica��o e autoriza��o entre as vers�es 2.4 e 2.6 e entre as vers�es 2.6 e 3.0.

**cleanupOrphaned:**

Permite excluir de um shard documentos �rf�os cujo seus valores n�o perten�am a esse shard.

**cpuProfiler:**

Permite o usu�rio habilitar e usar o CPU profiler.

**inprog:**

Retorna opera��es ativas ou pendentes.

**invalidateUserCache:**

Libera informa��es do usu�rio do cache na mem�ria RAM, incluindo a remo��o de credenciais e pap�is de cada usu�rio.

**killop:**

Mata uma determinada opera��o, identificada pelo seu id.

**planCacheRead:**

Visualiza os planos de list e querys do cache na database ou cole��es.

**planCacheWrite:**

Remove os cached query plans da cole��o.

**storageDetails:**

Permite executar o comando storageDetails.

####A��es de Replica��o

**appendOplogNote:**

Permite adicionar notas ao oplog.

**replSetConfigure:**

Permite o usu�rio configurar uma replica set.

**replSetGetStatus:**

Permite ver o atual statis da replica set.

**replSetHeartbeat:**

Permite enviar um sinal para verificar se o replica set est� ativo.

**replSetStateChange:**

Permite mudar o estado de uma replica set atrav�s dos comandos replSetFreeze, replSetMaintenance, replSetStepDown e replSetSyncFrom.

**resync:**

Permite resincronizar as r�plicas prim�rias e secund�rias.

####A��es de Sharding

**addShard:**

Permite adicionar um shard no cluster.

**enableSharding:**

Permite ativar um shard na database usando o comando enableSharding.

**flushRouterConfig:**

Permite limpar as informa��es do cluster em cache e faz o overload dos metadados.

**getShardMap:**

Permite mapear funcionalidades do shard.

**getShardVersion:**

Permite obter a vers�o do shard.

**listShards:**

Exibe a lista de shards.

**moveChunk:**

Permite mover os chunks entre os shards.

**removeShard:**

Remove um shard do cluster.

**shardingState:**

Mostra o estado do sharding.

**splitChunk:**

Permite dividir os chunks das cole��es ou databases.

**splitVector:**

Permite executar opera��es de metadados nos clusters que possuem shards.

####A��es dos Administra��o de Servidores

**applicationMessage:**

Permite postar uma mensagem personalizada para o log de auditoria, atrav�s do comando logApplicationMessage.

**closeAllDatabases:**

Permite fechar todas as databases do cluster.

**collMod:**

Permite adicionar um conjunto de flags para modificar o comportamento do MongoDB.

**compact:**

Premite regravar e desfragmentar todos os dados e �ndices de uma cole��o na database.

**connPoolSync:**

Permite sincronizar o pool do cluster.

**convertToCapped:**

Permite converter uma cole��o not-capped para capped.

**dropDatabase:**

Permite excluir uma database.

**dropIndex:**

Permite excluir �ndices de uma cole��o da database.

**fsync:**

Permite for�ar o servi�o mongod para que execute um flush em todas as escritas pendentes na camada de armazenamento em disco.

**getParameter:**

Permite recuperar o valor das op��es configuradas anteriormente no cluster.

**hostInfo:**

Exibe informa��es sobre o server que o MongoDB est� rodando.

**logRotate:**

Permite fazer uma rota��o nos logs para que este n�o se concerte em um �nico arquivo, diminuindo o espa�o necess�rio para o armazenamento.

**reIndex:**

Permite apagar e recriar �ndices da cole��o.

**renameCollectionSameDB:**

Permite renomear uma cole��o na database corrente.

**repairDatabase:**

Permite checar e reparar uma database com insconsist�ncias no armazenamento dos dados.

**setParameter:**

Permite alterar alguns par�metros do cluster.

**shutdown:**

Limpa todos as databases e encerra os processos.

**touch:**

Permite carregar os dados da camada de armazenamento de dados na mem�ria.

####A��es de Diagn�sticos

**collStats:**

Visualiza estat�sticas de armazenamento de uma determina cole��o.

**connPoolStats:**

Permite executar um comando interno no cluster.

**cursorInfo:**

Permite retornar informa��es sobre o cursor.

**dbHash:**

Permite executar comandos com suporte � algumas configura��es do servidor.

**dbStats:**

Permite verificar estat�sticas sobre o servidor.

**diagLogging:**

Permite retornar dados adicionais para diagn�stico.

**getCmdLineOpts:**

Permite executar o comando o comando getCmdLineOpts no cluster.

**getLog:**

Permite executar o comando getLog no cluster.

**indexStats:**

Descontinuado na vers�o 3.0 do MongoDB, permite retornar estat�sticas sobre �ndices de cole��es da database.

**listDatabases:**

Permite listar estat�sticas das databases.

**listCollections:**

Permite listar estat�sticas das cole��es da database.

**listIndexes:**

Permite listar os �ndices da cole��o.

**netstat:**

Permite utilizar o comando netstat.

**serverStatus:**

Permite visualizar estat�sticas sobre o status do servidor.

**validate:**

Permite verificar estruturar de nomes de cole��es e/ou �ndices.

**top:**

Permite retornar estat�sticas de uso para cada cole��o da database.

####A��es Internas

**anyAction:**

Permite uma a��o em um recurso. N�o utilize essa a��o a n�o ser em circunst�ncias excepcionais.

**internal:**

P�rmite a��es internas. N�o utilize essa a��o a n�o ser em circunst�ncias excepcionais.














