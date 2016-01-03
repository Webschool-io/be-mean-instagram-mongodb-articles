# Artigo sobre Autenticação no MongoDb

**Autor**: Mauricio Gravena de Oliveira

**User**: maugravena

**Data**: 03/01/2016 -

## Qual a diferença entre Autenticação e Autorização?

### Autenticação

Segundo o dicionário significa:

> s.f. Ação ou efeito de autenticar.

Para melhor entendimento vamos ao significado da palavra **autenticar**:

> v.t.d. Validar; reconhecer algo como verdadeiro; admitir a autenticidade, a veracidade de algo...

### Autorização

Novamente com ajuda do dicionário:

> s.f. Ação ou resultado de autorizar; conceder permissão para que alguém faça alguma coisa; concessão.

Dessa vez já conseguimos identificar o que significa de primeira, não é mesmo?

Assim a diferença fica bem nítida, enquanto a autenticação cuida da veracidade de algo a autorização fica com a parte da permissão de alguma determinada ação.


## Passo-a-passo como criar um usuário administrador e um usuário comum.

### Usuário Administrador

1. **Comece MongoDB sem controle de acesso.**

  ```
  mongod --port 27017 --dbpath / data / db1
  ```

2. **Conecte-se à instância.**

  ```
  mongo --port 27017
  ```
3. **Crie o usuário administrador.**

  ```
  use admin
  db. createUser (
    {
      user: "UsuarioAdmin",
      pwd: "abc123",
      roles: [{role: "userAdminAnyDatabase", db: "admin"}]
    }
  )
  ```

4. **Reiniciar a instância MongoDB com controle de acesso.**

  ```
  mongod --auth --port 27017 --dbpath / data / db1
  ```

5. **Autenticar como usuário administrador.**

  ```
  mongo --port 27017 -u "UsuarioAdmin" -p "abc123" --authenticationDatabase "admin"
  ```

    Ou, no [mongo](https://docs.mongodb.org/manual/reference/program/mongo/) shell conectado sem autenticação, mude para o banco de dados de autenticação, e use o método **db.auth()** para autenticar.

  ```
  use admin
  db.auth("UsuarioAdmin", "abc123")
  ```

### Usuário Comum

1. **Conectar-se ao MongoDB com os privilégios apropriados.**

  ```
  mongo --port 27017 -u UsuarioAdmin -p abc123 --authenticationDatabase admin
  ```
2. **Criar um novo usuário.**

  Criar usuário em um determindo banco de dados. Utilizar a função **db.createUser()** e passar como parâmetro o usuário.

  A operação a seguir cria um usuário no banco de dados **meu-banco** com o nome, senha e papel(*roles*).

```
use meu-banco
db.createUser(
    {
      user: "mauricio",
      pwd: "123456",
      roles: [{role: "read", db: "meu-banco"}]
    }
)
```

## Cada papel listado em **Cluster Administration Roles.**

## Ações de privilégio listadas em **Privilege Actions.**
