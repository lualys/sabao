# Documentação 

Aqui será a documentação de como usar tanto o Backend quanto o Front-end.

## Back-end
Os seguintes passos devem ser feito na raiz da pasta `backend` documentação do Back-end:

Estando na raiz do projeto crie o arquivo `.env` e insira as seguintes informações dentro dele:
~~~
PORT=8000
DATABASE_URL=Sua url do banco de dados MongoDB
JWT_SECRET=Qualquer coisa aqui
JWT_SECRET_REFRESH=Qualquer coisa aqui também, porém diferente do anterior

~~~

Após isso, sendo a primeira vez que vai executar o projeto faça a instalação das dependências:
~~~
npm install
~~~

Se estiver tudo ok, basta rodar a aplicação com o seguinte comando:
~~~
npm run dev
~~~

Se estiver tudo normal a aplicação vai está rodando na seguinte url:
~~~
http://localhost:8000/
~~~
### Entendo os models que serão salvos no banco de dados

Como o banco de dados usado é o `mongoDB` todas os relacionamentos são feitos por meio de Schemas do pacote mongose. Sendo assim clique no link abaixo para ir até a documantação dos models dese projeto.

> **[MODELS](backend/README_BACK_END.md)**

### URLS da API

Aqui estarão separadas as urls da API e suas finalidades.

> Cadastro de usuários:

Nesse caso dependendo do nível de usuário terá urls diferentes:

> Cadastro usuário:
url: `http://localhost:8000/create-user`

Requisição POST com essa estrutura no body da requisição:
~~~ json
{
  "name": "testes",
  "email": "testes@teste.com",
  "password": "1234",
  "phone": "1234569",
  "adress": [
    {
      "city": "Corente",
      "street": "Central",
      "district": "Centro",
      "number": "123"
    }
  ],
  "donor": true,
  "admin": false,
  "active": true
}
~~~

O que vai diferir se o usuário vai ser um doador ou um coletor de óleo é o atributo `donor`, se ele for verdadeiro o usuário será um DOADOR, caso contrário será um COLETOR. O atributo `admin` só será verdadeiro se o usuário que tiver se cadastrando for um administrador do sistema, e somente um administrador pode cadastrar outro. E o atributo `active` determina se o usuário está ou não ativo no sistema, e apenas administradores podem desativar ou ativar contas de outros usuários.

> Autenticação: aqui temos a url para login.

url para login: `http://localhost:8000/login-user`

Requisição post com a seguinte estrutra `json`:

~~~json
{
    "email":"seu email",
    "password": "sua senha"
}
~~~

Resposta da API:
~~~ json
{
    "accessToken": "token de acesso",
    "refreshToken": "token para atualizar o token de acesso"
}
~~~

> Atualização do Token de acesso `accessToken`

Ulr:  `http://localhost:8000/refresh`

Requisição POST com o seguinte body na estrutura da requisição:
~~~json
{
    "refreshToken": "refreshToken aqui"
}
~~~

Resposta da requisição:
~~~json
{
    "accessToken": "Novo accessToken"
}
~~~

### Doações de óleo

Lembrando que somentes usuários doadores podem criar uma DOAÇÂO.

> Criação de uma doação

url: `http://localhost:8000/create-donor`

Requisição POST com o seguinte body:
~~~json
{
    "oil_situation": "Situação do óleo",
    "amount": "Quantidade em litros",
    "date": "Data para a retirada do óleo nesse padrão ano/mes/dia, ex:2023-12-17",
    "status": true 
}
~~~

O atributo status repesenta se essa doação está ativa ou não, toda vez que alguém colatar uma doação ela será alterada para `false` automáticamente. E para um usuário criar uma doação ele precisa está logado.

> Listagem de doações de um usuário

Aqui serão listadas as doções de um usuário DOADOR especifico, para isso será a do usuário que está logado no sistema.

url: `http://localhost:8000/get-donors`

Requisição GET sem nenhum body.

Resposta da API:
~~~ json
[
    {
        "_id": "Id da doaçap",
        "oil_situation": "Situação do óleo doado",
        "amount": "Quantidade em litros",
        "date": "Data para aretirada do óleo nesse padrão ano/mes/dia, ex: 2023-12-17",
        "user": "Id do usuário doador",
        "status": false,
        "__v": 0
    },
    {
        "_id": "Id da doaçap",
        "oil_situation": "Situação do óleo doado",
        "amount": "Quantidade em litros",
        "date": "Data para aretirada do óleo nesse padrão ano/mes/dia, ex: 2023-12-17",
        "user": "Id do usuário doador",
        "status": false,
        "__v": 0
    }
    /* Restante da lista caso tenha... */
]
~~~

> Listagem de todas a doações ativas

Aqui serão listadas todas as doações disponíveis, ou seja, todas as que estão ativas. Nesse caso somente o usuário COLETOR poderá fazer essa listagem estando logado.

url: `http://localhost:8000/get-all-donors`

Requisião GET sem body:

Resposta da API:
~~~json
[
    {
        "_id": "Id da doaçap",
        "oil_situation": "Situação do óleo doado",
        "amount": "Quantidade em litros",
        "date": "Data para aretirada do óleo nesse padrão ano/mes/dia, ex: 2023-12-17",
        "user": "Id do usuário doador",
        "status": true,/* Só serão listadas se esse atributor dor true */
        "__v": 0
    },
    {
        "_id": "Id da doaçap",
        "oil_situation": "Situação do óleo doado",
        "amount": "Quantidade em litros",
        "date": "Data para aretirada do óleo nesse padrão ano/mes/dia, ex: 2023-12-17",
        "user": "Id do usuário doador",
        "status": true,
        "__v": 0
    }
    /* Restante da lista caso tenha... */
]
~~~

### Coleta de óleo

Somente um usuário COLETOR de óleo pode fazer essa operção, e sempre que o usuário fizer isso o status da doaçao será alterado para false, e o único atributo passado na requisição será o id da doação. E para realizar essa operação o usuário deve está logado com o token de acesso.

url: `http://localhost:8000/oilcollection`

Requisição POST com a seguinte estrura json:
~~~ json
{
    "donor": "6570b684ee184e68df2c365e"
}
~~~

Resposta da API:
~~~ json
{
    "user": "Id do usuário que criau a coleta",
    "donor": "Id da doação",
    "_id": "Id desse objeto",
    "__v": 0
}
~~~

## Front-end


Os seguintes comandos serão apenas na raiz da pasta `front-end`.

Instalação das dependências:
~~~
npm install
~~~

Se tudo estiver ok, basta rodar a aplicação:
~~~
npm run dev
~~~

Se estiver tudo normal a aplicação vai está rodando na seguinte url:
~~~
http://localhost:5173/
~~~