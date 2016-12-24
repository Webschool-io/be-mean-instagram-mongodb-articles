
# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** [Willian Alberto Lauber](http://www.github.com/willianlauber)
**Data** 1482274653


## Qual a diferença entre Autenticação e Autorização?



**Autenticação:**

Processo que procura validar a identidade digital do utilizador de um sistema,
normalmente,  feito no momento em que é feita uma requisição.

**Autorização:**
autorização verifica se o usuário tem permiss�o para executar determinadas tarefas.



## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.



### Encetar o mongo

mongo

### Escolha do banco

use be-mean

### Escolha do nivel de acesso

#### Administrador:

Passar como parâmetro o papel(role): userAdminAnyDatabase.

```sql
db.createUser({
   user: 'usuario',
   pwd: 'senha',
   papeis(roles): [ { role: 'userAdminAnyDatabase', db: 'be-mean' } ]
})
```

#### Comum:

Como parâmetro: read

```js
db.createUser(
    {
      user: "usuario",
      pwd: "senha",
      papeis(roles) : [{ papel(role): 'read', db : 'be-mean' }]
    }
)
```



## Explique cada papel listado em Cluster Administration papeis(roles).




**ClusterAdmin:**

Garante o maior acesso ao gerenciamento de cluster. Este papel combina os privilégios referentes aos papéis "clusterManager ",
"clusterMonitor" e "hostManager" . Tal papel proporciona a acesso da função dropDatabase.

**ClusterManager:**

Voltado para a gestão e ações de monitoramento dentro do cluster. Com essa função pode-se acessar a configuração e bancos
de dados locais , que são utilizados respectivamente nos processos de fragmentção e replicação.

**ClusterMonitor:**

Fornece acesso para leitura à ferramentas de monitoramento , como, por exemplo, o agente de monitoramento Gestor MongoDB Cloud.

**HostManager:**

Dá a capacidade de monitorar e gerenciar servidores.



## Explique todas as ações de privilégio listadas em Privilege Actions.

A função concede privilégios para executar conjuntos de tarefas sobre os recursos definidos. Pode conceder acesso a um nível de coleção, quando aplicado
ao banco de dados no qual ele está referenciado.

**Find:**

Faz uma busca nos documentos de uma coleção.

**Insert:**

Insere um ou mais documentos em uma coleçãoo.

**Remove:**

Remove um ou mais documentos de uma coleção

**Update:**

Atualiza um ou mais documentos de uma coleção.

#### Ações de gestão de banco de dados

**ChangeCustomData:**

Modifica a  informação de qualquer usuário do banco de dados informado.

**ChangeOwnCustomData:**

Permite que o usuário altere suas próprias informações.

**ChangeOwnPassword:**

Permite que o usuário mude sua própria senha.

**ChangePassword:**

Permite que se modifique a senha de qualquer usuário do banco de dados informado.

**CreateCollection:**

Cria uma coleção no banco de dados.

**CreateIndex:**

Cria um índice no banco de dados

**CreateRole:**

Cria novos papeis(papeis(roles)) no banco de dados.

**CreateUser:**

Cria novos usuários  no banco de dados.

**DropCollection:**

Exclui uma coleção do banco de dados.

**DropRole:**

Exclui uma papel(role)do banco de dados.

**DropUser:**

Exclui usuário do banco de dados.

**Emptycapped:**

Remove todos os documentos de uma coleção.

**EnableProfiler:**

Modificar o perfil do usuário atual para capturar dados sobre o seu desempenho.

**GrantRole:**

Modifica uma papel(role)para qualquer usuário do banco de dados.

**KillCursors:**

Permite matar cursores  no banco de dados.

**RevokeRole:**

Remove um papel(role)de um usuário do banco de dados.

**Unlock:**

Destrava um serviço para que seja acessado por uma operação de leitura ou gravação.

**ViewRole:**

Ve informaçães sobre uma papel(role)do banco de dados.

**ViewUser:**

Ve informaçães sobre um usuário.

#### Ações de Gerenciamento de Deploying

**AuthSchemaUpgrade:**

Suporta o processo de atualização para sistemas existentes que usam autenticação e autorização entre as versões 2.4 e 2.6 e as versões 2.6 e 3.0.

**CleanupOrphaned:**

Exclui de um "shard" documentos órfãos cujo seus valores não pertençam a esse "shard".

**CpuProfiler:**

Habilitar e usa o CPU profiler.

**Inprog:**

Retorna operaçães ativas ou pendentes.

**InvalidateUserCache:**

Libera informaçães do usuário do cache na memória RAM, incluindo a remoção de credenciais e papéis de cada usuário.

**Killop:**

Mata uma determinada operação, identificada pelo seu id.

**PlanCacheRead:**

Visualiza os planos de listas e querys do cache  no banco de dados ou coleções.

**PlanCacheWrite:**

Remove os  query plans da coleção já salvos da memória ram.

**storageDetails:**

Permite executar o comando storageDetails.

#### Ações de Replicação

**AppendOplogNote:**

Adicionar notas ao oplog.

**ReplSetConfigure:**

Configura uma replica set.

**ReplSetGetStatus:**

Ve o status atual da replica set.

**ReplSetHeartbeat:**

Envia um sinal para verificar se o replica set está ativo.

**ReplSetStateChange:**

Muda o estado de uma replica set através de comandos como replSetFreeze, replSetMaintenance, replSetStepDown e replSetSyncFrom.

**Resync:**

Permite sincronizar novamente as réplicas primárias e secundárias.

#### Ações de Sharding


**AddShard:**

Adiciona um shard no cluster.

**EnableSharding:**

Ativa um shard  no banco de dados usando o comando enableSharding.

**FlushRouterConfig:**

Limpa as informaçães do cluster em cache e faz o overload dos metadados.

**GetShardMap:**

Mapeia funcionalidades do shard.

**GetShardVersion:**

Obtẽm a versoẽs do shard.

**ListShards:**

Exibe a lista de shards.

**MoveChunk:**

Move os chunks entre os shards.

**RemoveShard:**

Remove um shard do cluster.

**ShardingState:**

Mostra o estado do sharding.

**SplitChunk:**

Divide os chunks das coleçães ou databases.

**SplitVector:**

Executa operaçães de metadados nos clusters que possuem shards.

#### Açães dos Administração de Servidores

**ApplicationMessage:**

Posta uma mensagem personalizada para o log de auditoria, através do logApplicationMessage.

**CloseAllDatabases:**

Fecha todas os banco de dados de um cluster.

**CollMod:**

Adiciona um conjunto de flags para modificar o comportamento do MongoDB.

**Compact:**

Grava e desfragmenta todos os dados e índices de uma coleção  no banco de dados.

**ConnPoolSync:**

Sincroniza o pool do cluster.

**ConvertToCapped:**

Converte uma coleção not-capped para capped.

**DropDatabase:**

Exclui um banco de dados

**DropIndex:**

Exclui índices de uma coleção do banco de dados.


**Fsync:**

Força no serviço mongod a execução um flush em todas as escritas pendentes na camada de armazenamento em disco.

**GetParameter:**

Recupera o valor das opçães configuradas anteriormente no cluster.

**HostInfo:**

Exibe informaçães sobre o server que o MongoDB está rodando.

**LogRotate:**

Faz uma rotação nos logs para que este não se concerte em um único arquivo, diminuindo o espaço necessário para o armazenamento.

**ReIndex:**

Permite apagar e recriar índices da coleção.

**RenameCollectionSameDB:**

Renomeia uma coleção  no banco de dados corrente.

**RepairDatabase:**

Permite checar e reparar uma database com insconsistâncias no armazenamento dos dados.

**SetParameter:**

Altera alguns parâmetros do cluster.

**Shutdown:**

Limpa todos as databases e encerra os processos.

**Touch:**

Carrega os dados da camada de armazenamento de dados na memória.

#### Ações de Diagnósticos

**CollStats:**

Visualiza estatísticas de armazenamento de uma determina coleção.

**ConnPoolStats:**

Executar um comando interno no cluster.

**CursorInfo:**

Retorna informações sobre um cursor.

**DbHash:**

Executa comandos com acesso à algumas configurações do servidor.

**DbStats:**

Ve estatísticas sobre o servidor.

**DiagLogging:**

Retorna dados adicionais para diagnóstico.

**GetCmdLineOpts:**

Executa getCmdLineOpts no cluster.

**GetLog:**

Executa getLog no cluster.

**IndexStats:**

Retorna estatísticas sobre índices de coleções do banco de dados.Foi descontinuado na versão 3.0 do MongoDB.

**ListDatabases:**

Lista estatísticas dos bancos de dados.

**ListCollections:**

Lista estatisticas das coleções do banco de dados.

**ListIndexes:**

Lsta os índices da coleção.

**Netstat:**

Verificação dos endereços tcp/ip ativos no contexto em que foi utilizado.

**ServerStatus:**

Visualiza estatísticas sobre o status do servidor.

**Validate:**

Verifica estruturar de nomes da coleção e índices.

**Top:**

Estatísticas de uso de cada coleção do  banco de dados.

#### Ações Internas

**AnyAction:**

Permite uma ação em um recurso. Não é recomendavel seu uso a não ser em circunstãncias excepcionais.

**Internal:**

Permite ações internas.Não é recomendavel seu uso a não ser em circunstâncias excepcionais.
