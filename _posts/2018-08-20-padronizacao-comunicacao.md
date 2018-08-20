---
layout: post
title: 'Padronização de comunicação'
date: 2018-08-20
author: Daniel Araújo
cover: 'https://startupi.com.br/wp-content/uploads/2018/03/iot-770x250.jpg'
tags: padronização protocolo comunicação
---

O trabalho propõe uma padronização semântica genérica para dispositivos de Internet das coisas, permitindo uma comunicação e intercâmbio de serviços entre dispositivos sem compatibilidade prévia entre os equipamentos. O padrão proposto visa permitir a descrição de qualquer maquinário em atributos para facilitar o desenvolvimento sem que seja necessário alteração em nenhuma das camadas escolhidas previamente pelos desenvolvedores, beneficiando o ambiente de produção de novas ferramentas inteligentes por empresas ou grupos de pesquisa, bem como permitindo a integração de diversos dispositivos sejam eles compostos por um conjunto de sensores, de atuadores ou de ambos os fins e independente de quais protocolos de comunicação utilizem.

## Desenvolvimento:
### Dispositivo:
Um dispositivo é definido como um conjunto de controladores e sensores além de informações que ditarão sua comunicação com servidores. Algumas propriedades internas aos sensores e atuadores permitem a troca de dados com serviços que utilizam a padronização. Alguns desses são: a chave (*key*) é uma identificação única para cada controlador ou sensor em um dispositivo que juntamente com a identificação do serial, permite que as trocas de informações entre serviços sejam otimizadas.

### Produto:
Para permitir que um produto seja usado no controle ou monitoramento de vários equipamentos, um produto é definido como um conjunto de dispositivos, barateando, por exemplo, aparatos para medição de água de uma rede de hidrômetros em que um mesmo produto (*hardware*) composto por vários dispositivos pode ser ligado a cada um dos hidrômetros, não sendo necessário a aquisição de um produto dedicado a cada hidrômetro.

A estrutura demonstrada na primeira listagem, criada pela padronização proposta no presente artigo, permite que um desenvolvedor defina um produto a partir de um JSON de configuração que possui detalhes sobre os dispositivos e o funcionamento de sensores e atuadores vinculados. A formatação da listagem define um produto com apenas um dispositivo composto por sensores e controladores:
```javascript
[
	{
		name: "nome do dispositivo",
		serial: "numero_de_serie",
		protocol: "ws",
		sensors: [],
		controllers: []
	}
]
```
#### Controlador:
É definido como a menor estrutura de controle em um dispositivo, um mesmo controlador pode comandar um ou mais atuadores, como também pode ser necessário mais de um controlador para reger um único atuador. Controladores são especificados para cada uso e são criados pelos programadores dos dispositivos e interface, o *middleware* não restringe quais controladores um dispositivo pode conter, essa informação é repassada para a interface e ela irá exibir os controladores existentes.

A Listagem a seguir mostra os controladores cadastrados em um dispositivo de exemplo, essas informações são armazenadas pelo servidor e repassadas para os serviços que utilizam o *middleware*, como uma interface.
```javascript 
{
	controllers: [
    {
      key: "port1",
      type: "onoff",
      tag: "liga e desliga",
      description: "liga e desliga a luz"
    },
    {
      key: "port2",
      type: "rgbx",
      tag: "cor",
      description: "muda a cor da luz"
    },
    {
      key: "port3",
      type: "toggle",
      tag: "motor",
      description: "Liga um motor"
    },
    {
      key: "port4",
      type: "button",
      tag: "cor aleatoria"
    }
  ]
}
```
#### Sensor:
Um sensor é formado por atributos, descritos na próxima listagem ,como o tipo (*type*), que pode ser \textit{number}, *boolean*, *point*, dentre outros, que define o tipo de dado enviado na comunicação; um sensor opera em estado de envio síncrono e assíncrono simultaneamente, ou seja, por intervalo de tempo (*timeout*) ou por extrapolação do valor de sua variação da medição do ultimo envio e atual medição excedendo o *deadBand*; caso esteja em regimento síncrono, o sensor forma pacotes de dados com intervalo de tempo com uma resolução (*resolution*); alguns tipos necessitam de informações adicionais como o tipo *number*, que em alguns casos possui um funcionamento acumulado (*accumulate*) informando se os dados são acumulados com o tempo ou se são valores instantâneos, possibilitando o servidor acumular e o dispositivo enviar somente o diferencial; e a unidade de medida (*unit*) na qual o sensor funciona e é visualizada pelo usuário.
```javascript 
{
  sensors: [
    {
      key: "senDev1",
      deadBand: 300,
      timeout: 60000,
      resolution: 600,
      type: "number",
      accumulate: true,
      tag: "hidrômetro geral",
      unit: "litros"
    },
    {
      key: "sensDev2",
      timeout: 100000,
      resolution: 10000,
      type: "boolean",
      tag: "porta entrada",
      unit: "presença",
      description: "porta da entrada"
    },
    {
      key: "sensDev3",
      timeout: 100000,
      resolution: 5000,
      type: "point",
      tag: "gps carro",
      unit: "coordenadas",
      description: "gps do carro"
    }
  ]
}
```

## Resultados
Os testes demonstraram que a proposta possibilita a criação de uma plataforma genérica para dispositivos inteligentes baseados em Internet das coisas com tipagem fraca, possibilitando um desenvolvimento rápido de sistemas embarcados sem a necessidade de adequação específica da arquitetura ou do servidor para que um novo dispositivo seja compatível. As escolhas específicas de desenvolvedores para as camadas abaixo de aplicação ou a sua topologia de rede não interferem no serviço disponível, tornando sólida a proposta de dividir as responsabilidades para as arquiteturas dos dispositivos e aplicações.