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

Antes de fazer o cadastro de um dispositivo, é necessário fazer a descrição dele. É possível ler sobre isso no [cadastro de dispositivo - Parte 1](/blog/2018/09/16/cadastro-dispositivo-parte-1.html).

## Fluxo dos dados

Ao enviar a configuração de um dispositivo, o SaIoT verifica se o serial de identificação já está ativado no sistema, caso esteja em uso, o SaIoT envia uma resposta informando a configuração salva para o dispositivo. A resposta da requisição varia de acordo com o protocolo utilizado pelo dispositivo, assim como é feita a requisição também.

## Cadastro de dispositivo

Após fazer a descrição de um dispositivo, faça uma requisição POST enviando a configuração para o SaIoT para realizar o cadastro. Será mostrado um exemplo apenas com o protocolo HTTP(S).

**Protocolo:** `HTTP / HTTPS`

**URL:** `POST api.saiot.ect.ufrn.br/v1/device/manager/device`

**Corpo:**

<script src="https://gist.github.com/judsonc/bfba690ccdea36a703628eed3e5158b0.js"></script>

**Resposta:** `device pending`