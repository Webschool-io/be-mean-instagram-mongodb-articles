# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Geriel Castro
**Data** 2015-12-06 02:44

## Qual a diferença entre Autenticação e Autorização?

  Autenticação:
    É realizada quando é enviada dados de usuário para um servidor, ele pode ser criptografado ou não.

  Autorização:
    É realizada após a autenticação ser validada com sucesso, permite ou não o acesso.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.

  Para criar um usuário administrador você precisa passar algumas informações por parâmetro no array `roles`.
  Exemplo:

  ```
  db.createUser({
    user: "geriel",
    pwd: "geriel123",
    roles: [{role: "userAdminAnyDatabase", db: "app"}]
  })
  ```

  Para criação de um usuário comum, você precisa passar apenas o db que será utilizado no array `roles`.
  Exemplo:

  ```
  db.createUser({
    user: "geriel",
    pwd: "geriel123",
    roles: [{db: "app"}]
  })

  ```

## Explique cada papel listado em Cluster Administration Roles.
    * Database User Roles
      * `read:` Oferece a capacidade de ler dados em coleções.
      * `readWrite:` fornece leitura e escrita de informações nas coleções.

    * Database Administration Roles
      * `dbAdmin:` habilita ações como: find, createIndex, createRole...) em determinadas collections.
      * `dbOwner:` todas as ações administrativas estão são liberadas;
      * `userAdmin:` oferece a capacidade de criar e modificar funções e usuários no banco de dados atual.

    * Cluster Administration Roles
      * `clusterAdmin:` libera o total gerenciamento de um cluster.
      * `clusterManager:` libera o total gerenciamento e monitoramento do cluster.
      * `clusterMonitor:` permite acesso somente leitura para ferramentas de monitoramento.
      * `hostManager:` fornece a capacidade de monitorar e gerenciar servidores.

    * Backup and Restoration Roles
      * `backup:` fornece privilégios necessários para fazer backup de dados.
      * `restore:` permite privilégios necessários para restaurar os dados a partir de backups.

    * All-Database Roles
      * `readAnyDatabase:` fornece as mesmas permissões somente leitura como leitura, exceto que ele se aplica a todos os bancos de dados no cluster.
      * `readWriteAnyDatabase:` oferece as mesmas permissões de leitura e gravação como READWRITE, exceto que ele se aplica a todos os bancos de dados no cluster.
      * `userAdminAnyDatabase:` fornece o mesmo acesso a operações de administração de usuário como `useradmin`, exceto que ele se aplica a todos os bancos de dados no cluster.
      * `dbAdminAnyDatabase:` oferece o mesmo acesso a operações de administração de banco de dados como dbadmin, exceto que ele se aplica a todos os bancos de dados no cluster.

    * Superuser Roles
      * `root:` permite acesso às operações e todos recursos do readWriteAnyDatabase, dbAdminAnyDatabase, userAdminAnyDatabase e clusterAdmin de forma combinada;

    * Internal role
      * `__ system:` atribui esse papel a objetos de usuário que representam os membros do cluster, como os membros do conjunto de réplicas e Mongos instâncias. O papel dá direito a tomar qualquer ação contra qualquer objeto no banco de dados.


## Explique todas as ações de privilégio listadas em Privilege Actions.

    * Privilege Actions
      * `find:` o usuário pode realizar uma busca `db.collection.find()` em uma collection.
      * `insert:` realiza a ação de inserir dados em uma collection.
      * `remove:` realiza a ação de remover dados em uma collection.
      * `update:` realiza a ação de atualizar dados em uma collection.
