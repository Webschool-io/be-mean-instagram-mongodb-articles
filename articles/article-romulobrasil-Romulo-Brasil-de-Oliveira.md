# MongoDb - Artigo sobre Autenticação no MongoDb
- **Autor:** Rômulo Brasil de Oliveira
- **User:** [romulobrasil](https://github.com/romulobrasil)
- **Data:** 06 Out 2016

## Qual a diferença entre Autenticação e Autorização?

**Autenticação** é um processo que procura verificar a identidade digital do usuário de um sistema, 
normalmente, é feito no momento em que ele requisita um acesso em um programa ou computador.

**Autorização** verifica se o usuário tem permissão para executar determinadas tarefas.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

Criação de usuário admin:
```  
db.createUser(
    {
        user: "adminRomulo",
        pwd: "admin123",
        roles: [ { role: "userAdminAnyDatabase", db: "be-mean" } ]
    }
)
```

Criação de usuário comum:
```
db.createUser(
    {
        user: "rbrasil",
        pwd: "user123",
        roles: [ { db : 'be-mean'} ]
    }
)
```  

## Explique cada papel listado em *Cluster Administration Roles*.

- **`clusterAdmin`** é o maior privilégio existente, ele é uma combinação de todos os papéis (`clusterManager`, `clusterMonitor`, `hostManager`) em um único papel. Adicionalmente ele possui o papel `dropDatabase`.

- **`clusterManager`** neste papel estão agrupados todos os papeis de gerenciamento e monitoramento do *cluster* e do banco de dados do cluster. Inclui praticamente todos os papéis.

- **`clusterMonitor`** prove o acesso de leitura para os papéis de monitoramento existentes no *cluster* (`getLog`, `hostInfo`, `getShardMap`, `listDatabases`, `serverStatus`, e outros).

- **`hostManager`** prove o serviço de monitoramento para o `Monitor` e gerenciamento do servidor (`resync`, `shutdown`, `setParameter`, e outros).

## Explique todas as ações de privilégio listadas em *Privilege Actions*.

As ações de privilégio, definem as operações que um usuário pode efetuar sobre o banco de dados. No MongoDb estas ações podem estar agrupadas em outros papéis, como por exemplo o `clusterAdmin` que engloba várias destas ações.

- **Ações de consulta e escrita**
- `find`: Consultar.
- `insert`: Inserir na coleção.
- `remove`: Remover registro da coleção.
- `update`: Editar registro da coleção.

- **Ações de Gerenciamento da Base de dados**
- `changeCustomData`: Alterar o campo `customData` de qualquer usuário.
- `changeOwnCustomData`: Alterar o campo `customData` apenas do seu usuário.
- `changeOwnPassword`: Alterar a própria senha.
- `changePassword`: Alterar senha de qualquer usuário.
- `createCollection`: Criar uma nova coleção.
- `createIndex`: Criar um novo índice na coleção.
- `createRole`: Criar um novo papel.
- `createUser`: Criar novo usuário.
- `dropCollection`:  Deletar uma coleção.
- `dropRole`: Deletar uma role.
- `dropUser`: Deletar um usuário.
- `emptycapped`: Deletar todos os documentos de uma *Capped Colection* (Conjunto circular de coleção que registra as atividades de uma coleção na forma em que acontecem, muito bem ordenadas e com extrema rapidez de gravação e leitura sequencial de dados. Muito utilizado para *Logs*).
- `enableProfiler`: Alterar o perfil do banco de dados para capturar dados de desempenho.
- `grantRole`: Atribuir permissão (`role`) para qualquer usuário.
- `killCursors`: Deletar todos os cursores de uma coleção.
- `revokeRole`: Retirar permissão (`role`) de qualquer usuário.
- `unlock`: Recurso do *cluster* para habilitar escrita e reversão de operações.
- `viewRole`: Visualizar informações sobre um papel existente.
- `viewUser`: Visualizar as informações de um usuário.

- **Ações de Gerenciamento de Implantação**
- `authSchemaUpgrade`: Disponível para *clusters*. Atualização do processo de Autenticação e Autorização dos usuários.
- `cleanupOrphaned`: Deletar documentos órfãos dos *shards*.
- `cpuProfiler`: Habilitar o uso de *cpu* *Profiler*.
- `inprog`: Retorna as operações que estão em execução ou pendentes.
- `invalidateUserCache`: Ação de *cluster*. Limpa o cache com as informações dos usuários.
- `killop`: "Matar" uma operação que esteja sendo executada.
- `planCacheRead`: Exibe plano de uma *query* especifica.
- `planCacheWrite`: Escrever um cache com o plano das *queries* que estão sendo executadas.
- `storageDetails`: Exibe os detalhes de *storage* do banco de dados.

- **Ações de Replicação**
- `appendOplogNote`: Incluir notas no *oplog*.
- `replSetConfigure`: Configurar uma réplica set em um *cluster*.
- `replSetGetStatus`: Verificar o status de uma réplica em um *cluster*.
- `replSetHeartbeat`: Monitora a "saúde" de um nó. Aplicado em um *cluster*.
- `replSetStateChange`: Aplicado em um *cluster*. Alterar o estado de uma `replica set`.
- `resync`: Aplicado em um *cluster*. Força a “re-sincronização” de uma instancia escrava no MongoDb que esteja fora de sincronia.

- **Ações de Sharding**
- `addShard`: Adicionar novo *shard* ao cluster.
- `enableSharding`: Habilitar um *shard* no banco de dados.
- `flushRouterConfig`: Limpar o *cache* com as informações correntes do *cluster*.
- `getShardMap`: Exibir o mapa de *shards* em um *cluster*.
- `getShardVersion`: Exibir a versão dos *shards* em um *cluster*.
- `listShards`: Lista todos os *shards* em um *cluster*.
- `moveChunk`: Realiza a movimentaçao de *chunks* entre os *shards*.
- `removeShard`: remove um *shard* do *cluster*.
- `shardingState`: Visualizar o estado de um *shard*.
- `splitChunk`: Dividir um *chunk* existente.
- `splitVector`: Dividir um vetor.

- **Ações de Administração do Servidor**
- `applicationMessage`: Ação de *cluster* para configuração de mensagem de *log* da aplicação.
- `closeAllDatabases`: Fechar todos os cursores de banco de dados. Ação de *cluster*.
- `collMod`: Adicionar *flag* em uma coleção para mudar seu comportamento.
- `compact`: Rescrever e desfragmentar todos os dados e índices de uma coleção.
- `connPoolSync`: Ação de cluster para sincronizar o *pool* de conexões.
- `convertToCapped`: converter uma coleção existente em uma *Capped Colection*.
- `dropDatabase`: Eliminar um banco de dados.
- `dropIndex`:  Eliminar um índice.
- `fsync`: Para o MongoDb e bloqueia as operações de escrita para captura de *backups*.
- `getParameter`: Recuperar o valor das opção que está *setada*.
- `hostInfo`: Exibir informações do *host* que o MongoDb está sendo executado.
- `logRotate`: “Rotacionar” os *logs* para evitar que um único arquivo de *log* ocupe um grande espaço no banco de dados.
- `reIndex`: deleta e recria todos os índices da base de dados.
- `renameCollectionSameDB`: Modifica o nome de uma coleção existente.
- `repairDatabase`: Verificar e reparar erros e inconsistências no banco de dados.
- `setParameter`: Definir o valor de parâmetro para o banco de dados. Disponível para *cluster*.
- `shutdown`: Limpar todos os recursos do banco de dados e terminar o processo.
- `touch`: Carregar os dados de armazenamento para memória.

- **Ações de Diagnóstico**
- `collStats`: Estatísticas de armazenamento de uma coleção.
- `connPoolStats`: Número de conexões abertas na instancia atual do banco de dados.
- `cursorInfo`: Informações sobre o cursor.
- `dbHash`: *Hash* do banco de dados.
- `dbStats`: Estatísticas de armazenamento do banco de dados.
- `diagLogging`: Informações adicionais para *log* dos dados presentes no banco de dados.
- `getCmdLineOpts`: Retorna um documento com dois parâmetros, `argv` and `parsed`. `Argv` um *array* de comando usado para invocar o `mongo` ou `mongos`. `Parsed`, inclui todas as opções de execução.
- `getLog`: Retorna um documento com o *log* das mensagens mais recentes dos processos do MongoDB.
- `indexStats`: Exibir o status de um índice
- `listDatabases`: Exibir uma lista sobre todas os banco de dados existentes no servidor e uma estatísticas básica sobre cada uma.
- `listCollections`: Exibir uma lista sobre todas as coleções existentes no servidor e uma estatísticas básica sobre cada uma.
- `listIndexes`: Exibir uma lista sobre todos os índices existentes no servidor e uma estatísticas básica sobre cada um.
- `netstat`: Verificar as conexões e portas do MongoDB
- `serverStatus`: Retorna um documento com as informações dos processos atuais da base de dados.
- `validate`: Verifica as estruturas dos *namespaces* para correção a partir do escaneamento da das coleções e índices.
- `top`: Retorna as estatísticas de uso de uma determinada coleção.

- **Ações Internas**
- `anyAction`: Permite qualquer ação em um recurso. ***Não*** atribua essa permissão, exceto em caso especiais.
- `internal`: Permite ações internas. ***Não*** atribua essa permissão, exceto em caso especiais.

## Referência bibliográfica
https://docs.mongodb.org/manual/