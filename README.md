# apifast-node
Fast mode to build API en Nodejs
<br>

<div align="center"> <img src="node.jpg" width="800" height="200"> </div>

Para quem não conhece o Express.js ele é um excelente framework do Node.js que nos auxilia na construção das nossas aplicações Web. Ele é um framework muito simples de ser utilizado, por isso vem sendo adotado pelos desenvolvedores de todos os níveis. Para que possamos conhecer ele um pouco mais vejamos 10 passos a baixo para criação de um Web Site.

1. Instalação
Para esse artigo não iremos abordar a instalação do Node.js, iremos partir de uma maquina com ele já instalado e configurado. Crie um novo diretório no seu computador e crie uma nova pasta, nós iremos utilizar node-express, mas você pode escolher um outro nome de sua preferência. Feito isso execute o comando a baixo para baixar o nosso modulo.

npm install express

2. Configuração
Agora nós precisamos criar o nosso arquivo package.json, esse é o arquivo de ponto de partido dos nossos projetos Node. Para isso, execute o comando a baixo, ele irá criar o nosso arquivo com a referencia do module express.

npm init -y

3. Estrutura do nosso projeto
Crie uma estrutura de pastas e arquivos conforme está na imagem a baixo:

<div align="center"> <img src="estrutura.jpg" width="400" height="300"> </div>

4. Criando arquivo de Server
Vamos agora criar o arquivo de inicialização do nosso projeto, para quem vem do mundo php seria o nosso index.php ou HomeController.cs no MVC do .NET. Para isso, abra o seu arquivo server.js e cole o código a baixo nele:

<br>

> [!NOTE]
>const app = require('../src/app');<br>
>const port = normalizaPort(process.env.PORT || '3000');<br>
>function normalizaPort(val) {<br>
>    const port = parseInt(val, 10);<br>
>    if (isNaN(port)) {<br>
>        return val;<br>
>    }<br>
>if (port >= 0) {<br>
>        return port;<br>
>    }<br>
>return false;<br>
>}<br>
>app.listen(port, function () {<br>
>    console.log(`app listening on port ${port}`)<br>
>})<br>

<br>
No código a cima nós estamos importando um modulo que iremos criar nos próximos passos, depois estamos definindo uma porta para que ele seja executado, no final estamos passando para o método app.listen a porta que queremos que ele escute o nosso projeto e de um console.log com ela.

5. Controller
Para que possamos organizar o nosso código, nós dividimos ele pensando em um padrão MVC, no código a baixo nós temos as nossas Actions das nossas Controllers.<br>

> [!NOTE]
>exports.post = (req, res, next) => {<br>
>    res.status(201).send('Requisição recebida com sucesso!');<br>
>};<br>
>exports.put = (req, res, next) => {<br>
>    let id = req.params.id;<br>
>    res.status(201).send(`Requisição recebida com sucesso! ${id}`);<br>
>};<br>
>exports.delete = (req, res, next) => {<br>
>    let id = req.params.id;<br>
>    res.status(200).send(`Requisição recebida com sucesso! ${id}`);<br>
>};<br>

6. Rotas
Agora vamos criar as nossas rotas, nessa parte nós temos dois arquivos: index.js e personRoute.js. O arquivo index.js seria para passar a versão que esta a nossa API ou para que possamos passar para um balanceador (Load Balancer) verificar se a nossa API está no ar, o personRoute.js contem as rotas que iremos utilizar para nossa PersonController.<br>

Index.js

> [!NOTE]
>const express = require('express');<br>
>const router = express.Router();<br>
>router.get('/', function (req, res, next) {<br>
>    res.status(200).send({<br>
>        title: "Node Express API",<br>
>        version: "0.0.1"<br>
>    });<br>
>});<br>
>module.exports = router;<br><br>

PersonRoute

> [!NOTE]
>const express = require('express');<br>
>const router = express.Router();<br>
>const controller = require('../controllers/personController')<br>
>router.post('/', controller.post);<br>
>router.put('/:id', controller.put);<br>
>router.delete('/:id', controller.delete);<br>
>module.exports = router;<br>
<br>
7. Configurações.<br>

O arquivo app.js é responsável pelas configurações do nosso projeto, nele que nós devemos configurar a nossa base de dados, rotas … etc. Pensando novamente no mundo .NET eu ousaria dizer que ele seria o nosso web.config.<br>

> [!NOTE]
>const express = require('express');<br>
>const app = express();<br>
>app.use(express.json());<br>
>const router = express.Router();<br>
>//Rotas<br>
>const index = require('./routes/index');<br>
>const personRoute = require('./routes/personRoute');<br>
>app.use('/', index);<br>
>app.use('/persons', personRoute);<br>
>module.exports = app;<br><br>


8. Nodemon<br>

O pacote nodemon nós auxilia no momento do nosso desenvolvimento, com ele nós não precisamos dar stop e subir novamente a nossa APP, ele verifica que ocorreu uma alteração e já faz o refresh automaticamente. Para instalar ele, execute o comando a baixo na sua console.<br><br>

npm install -g nodemon<br><br>


9. Arquivo Package.config<br>

Esse seria o arquivo inicial nos projetos Node, nele nós temos todas as dependência<br>

> [!NOTE]
>{<br>
>  "name": "node-express",<br>
>  "version": "1.0.0",<br>
> "description": "",<br>
>  "main": "index.js",<br>
>  "dependencies": {<br>
>    "express": "^4.15.4"<br>
>  },<br>
>  "devDependencies": {},<br>
>  "scripts": {<br>
>    "test": "echo \"Error: no test specified\" && exit 1",<br>
>    "start": "npx nodemon ./bin/server.js"<br>
>  },<br>
>  "keywords": [],<br>
>  "author": "",<br>
>  "license": "ISC"<br>
> }<br><br>

Rode o comando: npm install

e depois

npx nodemon ./bin/server.js

10. Testes<br>

Para que possamos testar o nosso projeto, digite o comando npm install na sua console para importar os pacotes necessários para a nossa aplicação, assim que ele finalizar execute o comando npm start. Caso tudo OK nos passos anteriores, você irá ver a mensagem a baixo na sua console.

<div align="center"> <img src="terminal.jpg" width="400" height="140"> </div>

Agora abra no seu navegador o endereço http://localhost:3000/. Ele deve apresentar a mensagem a baixo como retorno da nossa rota Index.

<div align="center"> <img src="final.jpg" width="300" height="140"> </div>

Obs: olhe dentro do app.js verá '/person' que é usada na url para testar a API, exeplo: http://localhost:3000/persons/1
