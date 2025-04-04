# Criando uma REST API Simples com Node.js e Express

![Image](https://cdn-images-1.medium.com/max/800/1*MkjK86cFTeY9h8PeedaP5w.png)

Neste tutorial você aprenderá a criar uma API REST simples usando Node.js com Express, em linguagem JavaScript.

A API gerenciará um recurso chamado **Produto**, com os campos `id`, `nome` e `preco`.

Para manter a simplicidade, utilizaremos um array em memória como repositório de dados.

> 🔗 Adaptação deste tutorial para se conectar a um **banco de dados** web: [*Criando uma API REST simples com Node.js, Express e banco de dados web, via Prisma*](https://eldes.com).

> Adaptação o projeto deste tutorial para organizar o código de acordo com os princípios de **SOLID**: [*Criando uma API REST simples com Node.js, Express e banco de dados web, via Prisma, estruturado de acordo com os princípios de SOLID*](https://www.eldes.com)

## 1. Configurando o ambiente

Antes de começar, certifique-se de ter o [Node.js](https://nodejs.org/) instalado.

### Criando o projeto

Abra um terminal e execute:

```sh
mkdir tutorial-back-end
cd tutorial-back-end
npm init -y
```

Isso criará um novo diretório para o projeto e inicializará um arquivo `package.json`.

Edite o `package.json` e defina `"src/index.js"` como valor da propriedade `main`.

### Instalando dependências

Agora, instale o Express para criar o servidor:

```sh
npm install express cors body-parser
```

O pacote `cors` será utilizado para permitir requisições de origens diferentes.

## 2. Criando o Servidor Passo a Passo

Vamos construir o servidor adicionando funcionalidades gradativamente.

### Passo 1: Criando o arquivo principal

No diretório do projeto, crie um arquivo chamado `src/index.js`. Este será o ponto de entrada da nossa aplicação.

### Passo 2: Criando um servidor básico com uma rota simples

Primeiro, importamos o Express e criamos um servidor básico:

```js
const express = require('express');
const cors = require('cors');
const app = express();
const port = 4000;

app.use(cors()); // Habilita CORS

// Criamos uma rota simples que retorna uma mensagem de boas-vindas
app.get('/', (req, res) => {
  res.send('Olá!');
});

// Iniciamos o servidor
app.listen(port, () => {
  console.log(`Servidor rodando em http://localhost:${port}`);
});
```

Agora, execute:

```sh
node .
```

Acesse `http://localhost:4000/` e você verá "Olá!" no navegador.

### Passo 3: Criando um mock de banco de dados

Como não usaremos um banco de dados real, armazenamos os produtos em um array:

```js
const produtos = [
  { id: 1, nome: 'Produto A', preco: 100.0 },
  { id: 2, nome: 'Produto B', preco: 200.0 }
];
```

app.get(🦏, 🎃)

onde 🦏 = rota e 🎃 = função.

Isso significa que quando alguém se conectar nesse servidor, via GET, na rota 🦏, então a função 🎃 será chamada.
 

### Passo 4: Criando a rota para listar todos os produtos

Agora, adicionamos uma nova rota `GET /produtos` para retornar todos os produtos:

```js
app.get('/produtos', (req, res) => {
  res.json(produtos);
});
```

### Passo 5: Criando a rota para obter um produto por ID

```js
app.get('/produtos/:id', (req, res) => {
  const produto = produtos.find(p => p.id === parseInt(req.params.id));
  if (produto) {
    res.json(produto);
  } else {
    res.status(404).json({ mensagem: 'Produto não encontrado' });
  }
});
```

### Passo 6: Adicionando suporte a JSON e criando a rota para adicionar produtos

Agora, adicionamos o middleware `body-parser` para processar JSON:

```js
const bodyParser = require('body-parser');
app.use(bodyParser.json());
```

Criamos a rota `POST /produtos` para adicionar um novo produto:

```js
app.post('/produtos', (req, res) => {
  const { nome, preco } = req.body;
  const novoProduto = { id: produtos.length + 1, nome, preco };
  produtos.push(novoProduto);
  res.status(201).json(novoProduto);
});
```

### Passo 7: Criando a rota para atualizar um produto existente

```js
app.put('/produtos/:id', (req, res) => {
  const { nome, preco } = req.body;
  const produto = produtos.find(p => p.id === parseInt(req.params.id));
  
  if (produto) {
    produto.nome = nome;
    produto.preco = preco;
    res.json(produto);
  } else {
    res.status(404).json({ mensagem: 'Produto não encontrado' });
  }
});
```

### Passo 8: Criando a rota para deletar um produto

```js
app.delete('/produtos/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const index = produtos.findIndex(p => p.id === id);
  
  if (index !== -1) {
    produtos.splice(index, 1);
    res.status(204).send();
  } else {
    res.status(404).json({ mensagem: 'Produto não encontrado' });
  }
});
```

## 3. Executando o Servidor

No terminal, execute:

```sh
node server.js
```

O servidor será iniciado em `http://localhost:4000`.

## 4. Testando a API

### Listar produtos

```sh
curl http://localhost:4000/produtos
```

### Obter um produto por ID

```sh
curl http://localhost:4000/produtos/1
```

### Criar um novo produto

```sh
curl -X POST http://localhost:4000/produtos -H "Content-Type: application/json" -d '{"nome":"Produto C", "preco": 300.0}'
```

### Atualizar um produto existente

```sh
curl -X PUT http://localhost:4000/produtos/1 -H "Content-Type: application/json" -d '{"nome":"Produto A Modificado", "preco": 150.0}'
```

### Deletar um produto

```sh
curl -X DELETE http://localhost:4000/produtos/1
```

## Conclusão

Neste tutorial, construímos um servidor REST API passo a passo, começando com uma rota simples e evoluindo para um CRUD completo com Express e um array em memória para armazenar os produtos. Esse é um ótimo ponto de partida para expandir a API futuramente, conectando-a a um banco de dados real, adicionando autenticação e implementando testes automatizados.

