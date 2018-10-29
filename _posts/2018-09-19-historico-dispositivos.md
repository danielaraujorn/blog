---
layout: post
title: "Histórico para dispositivos"
date: 2018-09-19
author: Judson Costa
cover: "https://st3.depositphotos.com/1029662/12502/v/950/depositphotos_125021734-stock-illustration-businessman-climbing-up-on-a.jpg"
tags: desenvolvedor dispositivos embarcados
comments: true
---

Esta postagem fala sobre o módulo Histórico e como utilizá-lo para armazenar dados gerados por dispositivos inteligentes. O Histórico está integrado ao sistema [Saiot](https://saiot.ect.ufrn.br).

## Histórico

O Histórico (_history_) é um módulo historiador que permite armazenar dados em série temporal e também possibilita a leitura através de filtros. O armazenamento é acessível através dos protocolos HTTP(S), seguindo o padrão [_RESTful_](https://becode.com.br/o-que-e-api-rest-e-restful/), [**MQTT**](https://www.ibm.com/developerworks/br/library/iot-mqtt-why-good-for-iot/index.html) e [**_WebSocket_**](https://developer.mozilla.org/pt-BR/docs/WebSockets). E a leitura de dados pode ser acessada por HTTP(S) e _WebSocket_.

## Requisitos

Para utilizar o Histórico, é preciso que o dispositivo esteja autenticado e envie os dados no formato definido pelo Saiot (falado a seguir). A postagem [Autenticação de dispositivos](/blog/2018/09/15/autenticacao-dispositivo.html) explica sobre isso.

Além da autenticação, os valores reportados devem ter o mesmo tipo (`type`) definido no arquivo de configuração do dispositivo. Esses parâmetros foram explicados em [Cadastro de dispositivos - Parte 1
](/blog/2018/09/16/cadastro-dispositivo-parte-1.html).

E também é obrigatório o envio da data no formato `ano-mes-dia hr:mm:ss.ms` para dizer o horário que o dado foi captado pelo dispositivo. O Saiot tem um endereço que fornece a data-hora no formato definido.

**Protocolo:**

`HTTP / HTTPS`

**URL:**

`GET api.saiot.ect.ufrn.br/v1/device/history/datetime`

**Resposta:**

`2018-09-19 12:41:52.220`

## Como utilizar

Para enviar dados para a plataforma, o dispositivo pode utilizar um dos três protocolos disponíveis e também escolher dois formatos de envio.

- O **Formato 1** facilita o envio de valores reportados por sensores diferentes mas apresentam o mesmo horário de captação;
- O **Formato 2** facilita o envio de valores que tem um horário de captação diferente, independente se foi reportado por um ou vários sensores;

## Exemplos

A seguir será apresentado exemplos de envio com todos os protocolos suportados pelo Saiot e com as duas formatações de envio válidas.

**Todas as requisições feitas por dispositivos devem enviar juntamente o _token_ de acesso, como informado na [Autenticação de dispositivos](/blog/2018/09/15/autenticacao-dispositivo.html).**

### Formato 1

**Protocolo:**

`HTTP / HTTPS`

**URL:**

`POST api.saiot.ect.ufrn.br/v1/device/history/logs`

**Corpo:**

```json
{
  "token": "token da autenticação",
  "data": [
    {
      "data": [
        {
          "serial": "device1",
          "sensor1": 4,
          "sensor2": 5
        },
        {
          "serial": "device2",
          "sensor2": 5
        }
      ],
      "dateTime": "2018-09-19 12:51:52.220"
    }
  ]
}
```

**Resposta:**

`200 OK`

**Protocolo:**

`MQTT / WebSocket`

**Tópico/Evento:**

`/history/post/logs/`

**Corpo:** Mesma mensagem enviada no protocolo HTTP(S)

### Formato 2

**Protocolo:**

`HTTP / HTTPS`

**URL:**

`POST api.saiot.ect.ufrn.br/v1/device/history/logs/sensor`

**Corpo:**

```json
{
  "token": "token da autenticação",
  "data": {
    "serial": "device1",
    "key": "port01",
    "dateTime": "2018-09-19 12:41:52.220",
    "value": true
  }
}
```

**Resposta:**

`200 OK`

**Protocolo:**

`MQTT / WebSocket`

**Tópico/Evento:**

`/history/post/logs/sensor/`

**Corpo:**

_Mesma mensagem enviada no protocolo HTTP(S)_

Atualmente o sistema suporta quatro tipos de valores que podem ser armazenados, são eles: `number`, `string`, `point`, `boolean`. Abaixo será mostrado como esses tipos de valores (`value`) podem ser enviados.

- boolean

```json
{
  "serial": "device1",
  "key": "port01",
  "dateTime": "2018-09-19 11:51:52.220",
  "value": true
}
```

- string

```json
{
  "serial": "device1",
  "key": "port01",
  "dateTime": "2018-09-19 11:51:51",
  "value": "[12,433,232,1,32]"
}
```

- number

```json
{
  "serial": "device1",
  "key": "port01",
  "dateTime": "2018-09-19 11:50:52.220",
  "value": 142
}
```

- point (GeoLocation)

```json
{
  "serial": "device1",
  "key": "port01",
  "dateTime": "2018-09-19 11:51:55",
  "value": {
    "lat": -73.856077,
    "lnt": 40.848447,
    "alt": 629.055643
  }
}
```

<hr>

Assim terminamos mais uma postagem sobre a plataforma Saiot que contém diversos módulos, sendo o Histórico um deles, que visam facilitar o desenvolvimento de aplicações de IoT.
