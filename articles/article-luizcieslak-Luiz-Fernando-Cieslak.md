# MongoDb - Artigo sobre Autenticação no MongoDb
**Autor:** Luiz Fernando Cieslak
**Data** 1475000853164

## Qual a diferença entre Autenticação e Autorização?

**Autenticação**: É o processo de verificação de credenciais para acessar algum serviço. Exemplo: Autenticação no mongo com --u ¨usuario" --p ¨senha¨
**Autorização**: É o processo de verificação se um usuário autenticado possui permissão para realizar uma ação.
Exemplo: Verificar se o usuario logado no mongo possui as ações de read/write.

## Descreva aqui o passo-a-passo como criar um usuário administrador e um usuário comum.
```
//criando um administrador
db.runCommand( { createUser: "admin",
  pwd: "admin",
  roles: [
     { role: "clusterAdmin", db: "admin" },
     { role: "readAnyDatabase", db: "admin" },
     "readWrite"
   ],
  writeConcern: { w: "majority" , wtimeout: 5000 }
} )

//criando um usuario comum
db.runCommand( { createUser: "user",
  pwd: "paodebatata",
  roles: [ "readWrite" ],
  writeConcern: { w: "majority" , wtimeout: 5000 }
} )
```

## Explique cada papel listado em Cluster Administration Roles.

**clusterAdmin**: É o papel com maior autorizacao. Combina o poder dos 3 papeis abaixo.

**clusterManager**: Provém ações de monitoramento e manuseamento do Cluster, como por exemplo habilitar, adicionar e administrar shardings e modificar parametros na 'config database'

**clusterMonitor**: Permite ações do tipo read-only, como listShardings(), replSetGetStatus() e dbStats().

**hostManager**: É o papel cuja funcão é o manuseamento do servidor, somente. Tem funcões como cpuProfiler, flushRouterConfig e shutdown.

## Explique todas as ações de privilégio listadas em Privilege Actions.

As ações de privilegio são divididas em 7 categorias (lembrando que o usuário precisa ter o papel necessário para realizar a acão desejada):

**1. Query and Write Actions**: Ações de CRUD no banco de dados (find, insert, remove, update).

**2. Database Management Actions**: Ações relacionadas a gestão de usuários (createUser, dropUser, changeOwnPassword, changePassword), papéis de usuarios (createRole, dropRole, grantRole, revokeRole) e as colecões (createCollection, dropCollection, createIndex).

**3. Deployment Management Actions**: Ações de desenvolvimento como invalidadeUserCache, killop, habilitar o cpuProfiler e cleanupOrphaned dos shardings

**4. Replication Actions**: Ações relacionadas as replicas (replSetConfig, replSetGetStatus e outros)

**5. Sharding Actions**: Ações relacionadas aos shardings (enableSharding, addShard, moveChunk, removeShard e outros)

**6. Server Administration Actions**: Ações relacionadas a administracão dos servidores, como closeAllDatabases(), repairDatabase(), shutdown e outros. Também possui ações relacioandas ao banco de dados como dropDatabase() e dropIndex()

**7. Diagnostic Actions**: Ações de diagnóstico, como getLog, cursorInfo, serverStatus e de diagnóstico do cluster e do db, como listDatabases, listCollections, indexStats e outros.

**8. Internal Actions**: Ações internas como internal e a permissão de realizar qualquer ação com o anyAction. A documentation não recomenda essas ações a menos que seja de absoluta necessidade.
