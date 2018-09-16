---
layout: post
title: "Cadastro de dispositivos"
date: 2018-09-16
author: Judson Costa
cover: "/blog/assets/post/cadastro-dispositivo/dispositivos.png"
tags: desenvolvedor dispositivos cadastro
comments: true
---

Continuando a série de postagens para desenvolvedores de dispositivos inteligentes, neste post será mostrado como descrever um dispositivo, definindo sua característica de forma compatível com a plataforma SaIoT ([leia mais aqui](/blog/2018/08/20/padronizacao-comunicacao.html)).

Antes de iniciar a leitura, recomendamos que faça a leitura do post sobre a [autenticação de dispositivos](/blog/2018/09/15/autenticacao-dispositivo.html), porque esta postagem mostrará exemplos que necessitam que o dispositivo esteja conectado e autenticado.

Para cadastrar um dispositivo no SaIoT é preciso fazer uma descrição das características do dispositivo seguindo a [padronização suportada](/blog/2018/08/20/padronizacao-comunicacao.html) pela plataforma.

### Descrevendo um dispositivo

A padronização diz que todo dispositivo pode ser descrito por um conjunto de controladores e sensores. Cada um deles deve ter um identificador único (**_key_**) pra aquele dispositivo. Além disso, o próprio dispositivo deve ter um identificador único (**serial**) entre todos os dispositivos existentes.

A seguir tem um exemplo de descrição de um medidor inteligente que envia a quantidade de água consumida naquele intervalo para o SaIoT.

<script src="https://gist.github.com/judsonc/bfba690ccdea36a703628eed3e5158b0.js"></script>

A descrição é feita informando o nome do dispositivo (`medidor inteligente`), o serial de identificação (`medidor-unico`) e os sensores que integram o dispositivo. Um sensor é descrito por um identificador (`medidor`), uma tag (`medidor de água`), a unidade de medida (`Litros`) e o tipo dos dados enviados (`number`).