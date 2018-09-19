---
layout: post
title: "Cadastro de dispositivos - Parte 2"
date: 2018-09-16
author: Judson Costa
cover: "/blog/assets/post/cadastro-dispositivo/dispositivos.jpg"
tags: desenvolvedor dispositivos cadastro
comments: true
---

Este post mostra como fazer o cadastro de dispositivo no [SaIoT](https://saiot.ect.ufrn.br).

Antes de fazer o cadastro de um dispositivo, é necessário fazer a sua descrição. Mais detalhes em [Cadastro de dispositivo - Parte 1](/blog/2018/09/16/cadastro-dispositivo-parte-1.html).

## Fluxo dos dados

Ao enviar a configuração de um dispositivo, o SaIoT verifica se o serial de identificação já está ativado no sistema. Caso esteja em uso, é retornado a configuração salva, se não estiver, é retornado que o dispositivo está cadastrado como pendente (`device pending`). A resposta da requisição varia de acordo com o protocolo utilizado pelo dispositivo.

**Todas as requisições feitas por dispositivos devem enviar juntamente o *token* de acesso, como informado na [Autenticação de dispositivo](/blog/2018/09/15/autenticacao-dispositivo.html).**

## HTTP(S)

Após fazer a descrição de um dispositivo, faça uma requisição [POST](https://pt.wikipedia.org/wiki/POST_(HTTP)) enviando a configuração para o SaIoT para efetuar o cadastro.

**Protocolo:** `HTTP / HTTPS`

**URL:** `POST api.saiot.ect.ufrn.br/v1/device/manager/device`

**Corpo:**

<script src="https://gist.github.com/judsonc/fa3e5b6e48662ef9363d01239293d3b9.js"></script>

**Resposta:** `device pending`

## MQTT e _WebSocket_

Com o dispositivo conectado utilizando **MQTT** ele deve se inscrever (_subscribe_) no tópico com o mesmo nome do serial de identificação para receber as respostas das publicações (_publish_). Se o dispositivo utilizar o **_WebSocket_**, as respostas das requisições (_emit_) serão recebidas no evento (_on_) com o mesmo nome do serial.

**Protocolo:** `MQTT / WebSocket`

**Tópico/Evento:** `/manager/post/device/`

**Corpo:** Mesma mensagem enviada no protocolo HTTP(S)

**Resposta:** `device pending`

Com o dispositivo cadastrado como pendente, ele está visível para o usuário na interface e poderá cadastrá-lo. Ao usar o protocolo MQTT ou _WebSocket_, qualquer edição dos dados de configuração do dispositivo feita pelo usuário, o SaIoT enviará a nova configuração para o dispositivo no evento/tópico com o nome do serial. Para ter acesso a nova configuração utilizando o protocolo HTTP(S), faça a [requisição novamente](/blog/2018/09/16/cadastro-dispositivo-parte-2.html#https).

<hr>

Assim terminamos o cadastro de dispositivo na plataforma SaIoT.