---
layout: post
title: "Cadastro de dispositivos - Parte 1"
date: 2018-09-16
author: Judson Costa
cover: "/blog/assets/post/cadastro-dispositivo/dispositivos.png"
tags: desenvolvedor dispositivos cadastro
comments: true
---

Continuando a série de postagens para desenvolvedores de dispositivos inteligentes, este post será divididos em duas partes.

  * Na primeira parte será mostrado como descrever um dispositivo, definindo suas características de forma compatível com a plataforma [SaIoT](https://saiot.ect.ufrn.br) ([leia mais aqui](/blog/2018/08/20/padronizacao-comunicacao.html)).
  * E na segunda parte como cadastrar um dispositivo na plataforma, ([leia mais aqui](/blog/2018/09/16/cadastro-dispositivo-parte-2.html).

Antes de iniciar a leitura, recomendamos que faça a leitura do post sobre a [autenticação de dispositivos](/blog/2018/09/15/autenticacao-dispositivo.html), porque esta postagem mostrará exemplos que necessitam que o dispositivo esteja conectado e autenticado.

Para cadastrar um dispositivo no SaIoT é preciso fazer uma descrição das características do dispositivo seguindo a [padronização suportada](/blog/2018/08/20/padronizacao-comunicacao.html) pela plataforma.

## Descrevendo um dispositivo

A padronização diz que todo dispositivo pode ser descrito por um conjunto de controladores e sensores. Cada um deles deve ter um identificador único (**_key_**) pra aquele dispositivo. Além disso, o próprio dispositivo deve ter um identificador único (**serial**) entre todos os dispositivos existentes.

A seguir tem um exemplo simples de descrição de um medidor inteligente que envia a quantidade de água consumida naquele intervalo para o SaIoT.

<script src="https://gist.github.com/judsonc/bfba690ccdea36a703628eed3e5158b0.js"></script>

A descrição é feita informando o nome do dispositivo (`medidor inteligente`), o serial de identificação (`medidor-unico`) e os sensores que integram o dispositivo. Um sensor é descrito por um identificador (`medidor`), uma tag (`medidor de água`), a unidade de medida (`Litros`) e o tipo dos dados enviados (`number`). A seguir entraremos em mais detalhes sobre a configuração de sensores.

### Sensores

Como foi falado anteriormente, um dispositivo pode ter um conjunto de sensores em que cada sensor tem os seguintes parâmetros de configuração.

  * key*: permitido qualquer valor do tipo `string`. Informa a identificação do sensor dentro do dispositivo;
  * tag*: permitido qualquer valor do tipo `string`. Nome do sensor;
  * unit*: permitido qualquer valor do tipo `string`. Unidade de medida do sensor;
  * type*: permitido os valores `number`, `string`, `point`, `boolean`. Tipo do valor medido pelo sensor;
  * class: permitido os valores `energy`, `water`. Classe de medição em que o sensor é categorizado. Caso não faça parte de nenhuma classe, ignore este parâmetro;
  * timeout: permitido qualquer valor do tipo `number`. Intervalo de tempo que o sensor envia os dados para a nuvem;
  * resolution: permitido qualquer valor do tipo `number`. Intervalo de tempo que o sensor forma pacotes de dados;
  * deadBand: permitido qualquer valor do tipo `number`. Valor máximo da variação da última com a medição atual;
  * accumulated: permitido os valores `true`, `false`, `1`, `0`. Se deve ser feito o somatório de todos os valores medidos. Válido apenas para type number;

<span style="font-size:14px">\* parâmetro obrigatório.</span>

Agora será mostrado um exemplo de dispositivo que contém apenas controladores, como uma lâmpada inteligente.

<script src="https://gist.github.com/judsonc/e8e12751571c03c509cd72cbce8d2a62.js"></script>

Cada controlador é descrito por um identificador (`liga`), uma tag (`ligar`) e uma classe (`toggle`). A seguir entraremos em mais detalhes sobre a configuração de controladores.

### Controladores

Um dispositivo pode ter um conjunto de controladores em que cada controlador tem os seguintes parâmetros de configuração.

  * key*: permitido qualquer valor do tipo `string`. Informa a identificação do controlador dentro do dispositivo;
  * tag*: permitido qualquer valor do tipo `string`. Nome do controlador;
  * class*: permitido os valores `toggle`, `slider`, `rgbx`, `onoff`, `pushbutton`, `button`. A classe diz como o controlador será visualizado pelo usuário na interface;

As classes de controladores apresentam alguns detalhes que serão falados em uma futura postagem.

<span style="font-size:14px">\* parâmetro obrigatório.</span>

Agora será mostrado um exemplo de dispositivo com controladores e sensores. Esse dispositivo é um tomada inteligente que mede o consumo de energia e permite ser acionada remotamente. Segue configuração da tomada.

<script src="https://gist.github.com/judsonc/12f9d5e427d6ebefc0b2b7c7e1a1adb5.js"></script>

<hr>

Assim finalizamos a primeira parte sobre o cadastro de dispositivo. Na próxima parte iremos falar sobre o envio da configuração de dispositivo para o SaIoT, permitindo assim realizar o cadastro do dispositivo. 