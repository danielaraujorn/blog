---
layout: post
title: "Autenticação de interface"
date: 2018-09-18
author: Guilherme Matheus
cover: "https://www.greenme.com.br/images/morar/faca-voce-mesmo/cadeado.jpg"
tags: desenvolvedor autenticação interface
comments: true
---

Neste post mostraremos como autenticar uma interface gráfica utilizanto o sistema [SaIoT](https://saiot.ect.ufrn.br).

No middleware, existem rotas que para a aplicação (interface gráfica) consiga acessar os recursos. Para isso é necessário que o usuário que esteja acessando a interface esteja autenticado. O usuário se autentica ao fazer o processo de login.
A rota para registro de usuários é:
``/v1/client/auth/register``
Utilizando *HTTP(S)* e o método *POST*, os campos necessários para o registro são:
* *email* (formato de email, exemplo **user@email.com**)
* *password* (string, no mínimo 10 caracteres)
* *fullName* (string, no mínimo 2 caracteres)

É importante para o desenvolvedor de interfaces, que tenha uma rota para o registro e login de usuários, mas é possível criar um usuário no Saiot e usar para acessar as rotas que precisam de autenticação.
A rota para o login de usuários é:
``/v1/client/auth/login``
Utilizando *HTTP(S)* e o método *POST*, os campos necessários para o login são:
* *email* (formato de email, exemplo **user@email.com**)
* *password* (string, no mínimo 10 caracteres)

Depois de um usuário estar logado é possível que a aplicação tenha acesso a rotas restritas. 
A rota para o logout de usuários é:
``/v1/client/auth/logout``
Utilize *HTTP(S)* e o método *POST*.

## WebSocket
Para usar o *WebSocket* é preciso que um usuário tenha se logado no sistema e é o sistema que checa se existe um cookie de sessão que é criado quando o usuário loga ao sistema, utilizando o protocolo *HTTP(S)* e a rota mencionada anteriormente. Caso não exista o cookie de sessão no cabelhaço de conexão, não é possível conectar-se ao *WebSocket*. Caso a sessão tenha expirado, o cliente *WebSocket* é desconectado, para reconectar é preciso fazer o processo de login de usuários novamente.

Recomendamos o uso do [Socket.io](https://socket.io/).