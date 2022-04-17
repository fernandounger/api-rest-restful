# API, REST e RESTful

- API : Comunicação entre o front e o back end.


- Iniciando o projeto.

```
yarn init -y
```
- Para criarmos o nosso servidor, usaremos a dependência express.

```
yarn add express
```

- Chamando o express

```js
const express = require("express");
const app = express();
```

- Instanciando ele na variável app

```js
app.listen(3000, function() {
    console.log("Server is running");
});
```
- Iniciando o servidor, colocando na porta (_3000_), com uma function callback retornando a informação "Server is running".
-----

## Verbos HTTP

- GET - Receber dados de um Resource.
- POST - Enviar dados ou informações para serem processados por um Resource.
- PUT - Atualizar dados de um Resource.
- DELETE - Deletar um Resource.

---
- Para usarmos os verbos no express, utilizamos:


```js
app.get()
app.post()
app.put()
app.delete()
```
-  Colocamos o endpoint que iremos usar.

```js
app.get("/clients");
app.get("/clients");
app.post("/clients");
app.put("/clients");
app.delete("/clients");
```

- Quando queremos obter um unico (client), colocamos um id, que é um parâmetro para obter-ló. 

- No express ele nos permite colocar uma callback function, que tem o objetivo de processar as nossas requisições.

- Usamos por padrão _req_ (Request) e _res_ (Response).

```js
app.get("/clients", function(req, res){});
app.get("/clients/:id", function(req, res){});
app.post("/clients", function(req, res){});
app.put("/clients/:id", function(req, res){});
app.delete("/clients/:id", function(req, res){});
```

```
https://jsonplaceholder.typicode.com/users
```
- Vamos acessar essa URL, e ela vai me trazer os usuários existentes.

- Chamando os dados de data.json

```js
const data = require("./data.json");
```

- Antes de pegarmos os clients, temos que responder os client, mas primeiro o expresse precisa usar o json.

```js
app.use(express.json());
```

- Precisamos do id do cliente para o pegarmos, então mandamos ele procurar o id e depois nos responder.

```js
app.get("/clients/:id", function(req, res){
    const { id } = req.params;
    const client = data.find(cli => cli.id == id);
    res.json(client);
});
```

- Como só existir 10 client, então se buscarmos pelo 11 adiante, o status continuará  _200_, então precisamos fazer um tratamento se caso não existir o client, onde passamos um status _204_, dizendo que não foi achado nenhum conteúdo.

```js
app.get("/clients/:id", function(req, res){
    const { id } = req.params;
    const client = data.find(cli => cli.id == id);

    if(!client) return res.status(204).json();

    res.json(client);
});
```
- Agora para pegar e retornar o nome e o e-mail que enviamos

```js
app.post("/clients", function(req, res){
    const { name , email } = req.body;
    res.json({ name, email});
});
```

- Agora atualizaremos o nosso client, precisaremos do id para acharmos, fazemos o tratamento se caso não achar o client, achando o client, pegamos o name do body e damos o novo name do client.

```js
app.put("/clients/:id", function(req, res){
    const { id } = req.params;
    const client = data.find(cli => cli.id == id);

    if(!client) return res.status(204).json();
    
    const { name } = req.body;
    client.name = name;

    res.json(client);
});
```

- Para deletarmos um client, usaremos um (filter) para filtrar os que clients que o id for diferente.

```js
app.delete("/clients/:id", function(req, res){
    const { id } = req.params;
    const clientsFiltered = data.filter(client => client.id === id);

    res.json(clientsFiltered);
});
```