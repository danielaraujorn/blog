---
layout: post
title: "Controladores"
date: 2018-09-18
author: Daniel Araújo
cover: "http://www.valisautomacao.com.br/dados/obras/sld20150824082917.JPG"
tags: desenvolvedor controladores padronização embarcados
comments: true
---

Neste post mostraremos o que é e como funciona os controladores segundo a padronização usada pelo sistema [SaIoT](https://saiot.ect.ufrn.br). Como já foi explicado resumidamente no post [Cadastro de dispositivos](/blog/2018/09/16/cadastro-dispositivo-parte-1.html), controladores são componentes responsáveis por ativar atuadores. O retorno de dados de um controlador é definido pelo desenvolvedor do controlador, como o atuador de um dispositivo usa esse retorno é de decisão do desenvolvedor do dispositivo.


A imagem a seguir mostra como é visualizado um dispositivo na página de controla, é evidenciado o controlador onoff e também é mostrado o ícone de uma engrenagem para abrir a página de interação com os controladores
![Controlador onoff]({{site.baseurl}}/assets/post/controladores/dispositivo.PNG)

<!-- ## Classes de controladores
 -->
## Exemplo
Caso o programador queira fazer um dispositivo capaz de controlar uma luz dimerizável, é necessário um controlador capaz de ligar e desligar o dispositivo e também um seletor de intensidade para dimerizar a luz.
Para esse caso, é aconselhado usar o controlar **onoff** que retorna o valor 1 ou 0 e o desenvolvedor cria uma função para que quando receber esses valores, o controlador ser capaz de desligar e ligar a luz. Para dimerizar, usa-se o controlador **slider** onde é possivel passar o valor mínimo, máximo e o step.