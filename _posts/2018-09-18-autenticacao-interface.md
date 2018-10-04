---
layout: post
title: "Autenticação de interface"
date: 2018-09-18
author: Guilherme Matheus
cover: "https://www.greenme.com.br/images/morar/faca-voce-mesmo/cadeado.jpg"
tags: desenvolvedor autenticação interface
comments: true
---

Neste post mostraremos como autenticar uma interface gráfica utilizando o sistema [Saiot](https://saiot.ect.ufrn.br).

A [API](https://canaltech.com.br/software/o-que-e-api/) fornecido pelo Saiot fornece endereços que permite aplicações (interfaces gráficas) acessarem os recursos. Para isso é necessário que a interface utilizada pelo usuário esteja autenticada.

O Saiot é uma plataforma com suporte aos protocolos HTTP(S) e [_WebSocket_](https://developer.mozilla.org/pt-BR/docs/WebSockets) para conexões com interface de usuário. No entanto, todo o processo de autenticação pode ser feito utilizando o protocolo [HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP) ou [HTTPS](https://pt.wikipedia.org/wiki/Hyper_Text_Transfer_Protocol_Secure) (recomendado).

### Registro de usuário

Para registrar uma conta de usuário é preciso fazer uma requisição ao seguinte endereço.

**Protocolo:** `HTTP / HTTPS`

**URL:** 

`POST api.saiot.ect.ufrn.br/v1/client/auth/register`

**Corpo:**

```json
{
  "email": "user@email.com",
  "password": "password",
  "fullName": "nome completo"
}
```

**Resposta:**

```json
{
  "success": true
}
```

Mais detalhes sobre os parâmetros de envio:

* email*: permitido qualquer valor do tipo `string` com formato de email;
* password*: permitido qualquer valor do tipo `string`, no mínimo 10 caracteres;
* fullName*: permitido qualquer valor do tipo `string`, no mínimo 2 caracteres;

<span style="font-size:14px">\* parâmetro obrigatório.</span>

### Login de usuário

A rota a seguir realiza o login de uma conta de usuário.

**Protocolo:** `HTTP / HTTPS`

**URL:** 

`POST api.saiot.ect.ufrn.br/v1/client/auth/login`

**Corpo:**

```json
{
  "email": "user@email.com",
  "password": "password"
}
```

**Resposta:**

```json
{
  "success": true
}
```

Mais detalhes sobre os parâmetros de envio:

* email*: permitido qualquer valor do tipo `string` com formato de email;
* password*: permitido qualquer valor do tipo `string`, no mínimo 10 caracteres;

Quando um usuário faz o login na interface, o Saiot registra um cookie de sessão no cabelhaço da conexão. Com isso, é possível que a aplicação tenha acesso a rotas restritas, ou seja, que necessitam de um usuário autenticado. 

### Logout de usuário

A rota a seguir encerra a sessão de uma conta de usuário.

**Protocolo:** `HTTP / HTTPS`

**URL:** 

`POST api.saiot.ect.ufrn.br/v1/client/auth/logout`

**Resposta:**

```json
{
  "success": true
}
```

## WebSocket

Para usar o *WebSocket* é preciso que um usuário tenha se logado no sistema para permitir checar se existe um *cookie* de sessão. Caso não exista um *cookie* de sessão no cabelhaço da conexão, não é possível conectar-se à aplicação. 

Quando a sessão de usuário expira, a conexão *WebSocket* é desconectada. Para reconectar é preciso fazer o processo de login de usuários novamente.

Toda a aplicação Saiot é feita utilizando o módulo [Socket.io](https://socket.io/) que também tem uma biblioteca para interfaces.