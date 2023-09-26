# API de banco

> API de banco construida com [Node.js](https://nodejs.org) + [Express](https://expressjs.com/).

A API já foi construida para você e não fará parte do exercício.

Entrentanto, se você se interessar por aprender como construir uma API como essa você pode seguir essa série de videos: https://aka.ms/NodeBeginner (os videos 17 ao 21 cobrem exatamente essa API).

Você também pode dar uma olhada nesse tutorial interativo: https://aka.ms/learn/express-api

## Rodando o servidor

Make sure you have [Node.js](https://nodejs.org) installed.
Tenha certeza que você tem [Node.js](https://nodejs.org) instalado em sua última versão.

1. Clone esse repositório [The Web-Dev-For-Beginners](https://github.com/microsoft/Web-Dev-For-Beginners).
2. Abra seu terminal e navegue até a pasta `Aula 9/api`
3. Rode o comando `npm install` e espere os pacotes serem instalados (pode levar um tempo dependendo da sua internet).
4. Quando a instalação terminar, rode `npm start` e pode começar.

O servidor deve estar escutando na porta `5000`
*O servidor deve estar rodando junto com o app do banco principal(escutando na porta `3000`), não feche o terminal!*

> Note: todas as entradas são guardadas na memória não são persistentes, então quando o servidor for parado todos os dados serão perdidos.

## detalhes da API

Route                                        | Description
---------------------------------------------|------------------------------------
GET    /api/                                 | obtem informação do servidor
POST   /api/accounts/                        | cria uma conta, ex: `{ user: 'Yohan', description: 'My budget', currency: 'EUR', balance: 100 }`
GET    /api/accounts/:user                   | obtem todos os dados de uma conta especifica
DELETE /api/accounts/:user                   | remove uma conta especifica
POST   /api/accounts/:user/transactions      | adiciona uma transação, ex: `{ date: '2020-07-23T18:25:43.511Z', object: 'Bought a book', amount: -20 }`
DELETE  /api/accounts/:user/transactions/:id | remove uma transação especifica

