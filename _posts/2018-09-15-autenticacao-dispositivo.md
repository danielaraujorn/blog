---
layout: post
title: "Autenticação de dispositivos"
date: 2018-09-15
author: Judson Costa
cover: "https://www.elcorreogallego.es/img/noticias/20180130/protecciondatos_826037_manual.jpg"
tags: desenvolvedor dispositivos autenticação embarcados
comments: true
---

Dando início a uma série de postagens para desenvolvedores que querem utilizar a plataforma [Saiot](https://saiot.ect.ufrn.br) para armazenar e gerenciar dispositivos, começaremos falando sobre como fazer a autenticação de dispositivos.

### Requisitos

Para um dispositivo autenticar-se ao Saiot é necessário que ele esteja vinculado a alguma conta de usuário e tenha um serial (uma forma de identificação, número de série ou string que seja única). Esses dados são os únicos requisitos para fazer a autenticação.

### Fluxo dos dados

A seguir será mostrado o fluxo dos dados desde que um dispositivo é ligado e conectado à internet até o momento que a conexão é autenticada. Isso sendo feito para os protocolos HTTP(S), [MQTT](https://www.ibm.com/developerworks/br/library/iot-mqtt-why-good-for-iot/index.html) e [_WebSocket_](https://developer.mozilla.org/pt-BR/docs/WebSockets).

![Fluxo da autenticação de dispositivos](/blog/assets/post/autenticacao-dispositivo/autenticacao-fluxo.png)

Inicialmente, é obrigatório utilizar o protocolo [HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP) ou [HTTPS](https://pt.wikipedia.org/wiki/Hyper_Text_Transfer_Protocol_Secure) (recomendado) mesmo que o dispositivo utilize outro protocolo suportado, como MQTT ou WebSocket. Todas as requisições devem ter no cabeçalho o `Content-Type: application/json`.

Caso utilize o protocolo HTTP ou a biblioteca HTTPS utilizada não precise do _fingerprint_, avance para o tópico [Token de acesso](#token-de-acesso).

Ao utilizar HTTPS, pode ser que o dispositivo necessite do [_fingerprint_](https://www.grc.com/fingerprints.htm) para fazer a requisição, então é possível ter acesso ao código _fingerprint_ ao fazer a seguinte requisição.

**Protocolo:** `HTTP`

**URL:** `GET auth.saiot.ect.ufrn.br/finger`

**Resposta:** `23 4A C5 3B BD 8F D6 59 D0 11 BC 00 6F 0E 38 14 33 08 55 20`

Com o código em mãos, é possível fazer requisições HTTPS para o Saiot. Para exemplificar será feito uma requisição GET utilizando a biblioteca [ESP8266HTTPClient](https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266HTTPClient).

<script src="https://gist.github.com/judsonc/3a80e074bb361a270c6e51a3af51a6ac.js"></script>

### Token de acesso

O primeiro passo da autenticação é requisitar um token de acesso que representa o vínculo de uma conta existente de usuário com o serial de um dispositivo. Para isso, é preciso fazer a requisição a seguir.

**Protocolo:** `HTTP / HTTPS`

**URL:**

`POST api.saiot.ect.ufrn.br/v1/device/auth/login`

**Corpo:**

```json
{
  "email": "email@email.com",
  "password": "password",
  "serial": "number-serie"
}
```

**Resposta:** `cc1NUYhCHoB9bmrXvFk1`

### Utilizando o Token

O token de acesso deve ser enviado no corpo de **todas** as requisições seguindo o padrão abaixo.

**Corpo:**

```json
{
  "token": "cc1NUYhCHoB9bmrXvFk1",
  "data": "dado a ser enviado"
}
```

Esse padrão é obrigatório para todos os protocolos suportados pela aplicação.

### Observações

Ao utilizar **_WebSocket_**, quando um dispositivo é conectado, a verificação do token ocorre quando é enviado alguma informação para a aplicação. Se o token estiver inválido, o dispositivo é desconectado.

**Protocolo:** `WebSocket`

**URL:** `ws.api.saiot.ect.ufrn.br`

Quando um dispositivo utiliza o **MQTT**, há um passo a mais que deve ser feito na conexão com o Saiot. O MQTT tem um recurso de autenticação intrínseco ao protocolo que permite enviar credenciais de acesso assim que é feita a conexão com a plataforma. Para isso, é preciso enviar os seguintes dados:

**Protocolo:** `MQTT`

**URL:** `api.saiot.ect.ufrn.br:8000`

**Corpo:**

```json
{
  "user": "email@email.com",
  "password": "token de acesso"
}
```

O dispositivo não conseguirá se conectar caso o token de acesso seja inválido, tendo em vista que tanto o token de acesso quanto o serial do dispositivo devem ser únicos.
