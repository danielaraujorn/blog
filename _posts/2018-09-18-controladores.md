---
layout: post
title: "Controladores"
date: 2018-09-18
author: Daniel Araújo
cover: "http://www.valisautomacao.com.br/dados/obras/sld20150824082917.JPG"
tags: desenvolvedor controladores padronização interface embarcados
comments: true
---

Neste post mostraremos o que é e como funciona os controladores segundo a padronização usada pelo sistema [SaIoT](https://saiot.ect.ufrn.br). Como já foi explicado resumidamente no post [Cadastro de dispositivos](/blog/2018/09/16/cadastro-dispositivo-parte-1.html), controladores são componentes responsáveis por ativar atuadores. O retorno de dados de um controlador é definido pelo desenvolvedor do controlador, como o atuador de um dispositivo usa esse retorno é de decisão do desenvolvedor do dispositivo.


A imagem a seguir mostra como é visualizado um dispositivo na página de controla, é evidenciado o controlador onoff e também é mostrado o ícone de uma engrenagem para abrir a página de interação com os controladores
![Controlador onoff]({{site.baseurl}}/assets/post/controladores/dispositivo.PNG)
## Classes de controladores existentes:
#### onoff
Como mostrado na ultima figura, o controlador **onoff** é um toggle indicado para ligar e desligar o dispositivo. Ele retorna o valor 0 e 1.

#### slider
![Controlador slider]({{site.baseurl}}/assets/post/controladores/slider.PNG)
O controlador slider é usado para controlar atuadores que possuem um range, como uma janela, luz, ou até o volume de uma caixa de som. Esse controlador possui parâmetros adicionais, são eles: **min**(o valor mínimo do slider), **max**(o valor máximo do slider) e **step**(o passo). Exemplo: caso for utilizar para controlar uma luz podemos colocar o mínimo dela para ser 0, o máximo para ser 1 e o step para ser 0.01, assim a luz terá 100 níveis de luminosidade.

#### rgb
![Controlador slider]({{site.baseurl}}/assets/post/controladores/rgb.PNG)
O controlador rgb renderiza um controle de *hue* para o usuário, e retorna um objeto json no seguinte formato: `{"r":255,"g":255,"b":255}`.

#### rgbx
![Controlador slider]({{site.baseurl}}/assets/post/controladores/rgbx.PNG)
O controlador rgbx renderiza um controle de *hue* para o usuário e um controle de saturação e vividez, e retorna um objeto json no seguinte formato: `{"r":255,"g":255,"b":255,"x":1}`, onde de 0 a 0.5 é controlado a saturação e de 0.5 a 1 é controlado a vividez. Para transformar para o padrão rgb, é só aplicar o seguinte algoritmo:
```javascript
if (x <= 0.5){
  porcent = x / 0.5;
  r *= porcent;
  g *= porcent;
  b *= porcent;
}
else{
  porcent = 2 - (x / 0.5);
  r = 255 - (255 - r) * porcent;
  g = 255 - (255 - g) * porcent;
  b = 255 - (255 - b) * porcent;
}
```

#### button
O controlador button renderiza um botão onde manda o valor 1 para o dispositivo, usado para ativar alguma função específica.

#### toggle
O toggle é um controlador que retorna o valor 0 e 1, usado para mudar o estado dos dispositivos, como por exemplo, um toggle em um dispositivo 'geladeira' pode significar o acionamento do modo de descongelamento.

#### pushbutton
O pushbutton é um botão no qual manda o valor 1 quando o usuário clica, e manda o valor 0 quando o usuário remove o dedo ou mouse do botão. É necessário ter cuidado com esse controlador pois se o usuário clicar com o mouse no botão e chegar a fechar a página antes de remover o clique do mouse, não será mandado o valor 0. Então é fortemente indicado utilizar um timout no seu dispositivo para que chame a função que seria chamada ao pushbutton enviar o valor 0, e então, atualizar o servidor com esse dado.

## Observação
Par fazer uso de controladores com parâmetros adicionais na biblioteca, é necessário o uso do construtor do dispositivo passando o json completo, como: `{"class":"onoff","max":100,"min":0,"step":1}`

## Exemplo
Caso o programador queira fazer um dispositivo capaz de controlar uma luz dimerizável, é necessário um controlador capaz de ligar e desligar o dispositivo e também um seletor de intensidade para dimerizar a luz.
Para esse caso, é aconselhado usar o controlar **onoff** que retorna o valor 1 ou 0 e o desenvolvedor cria uma função para que quando receber esses valores, o controlador ser capaz de desligar e ligar a luz. Para dimerizar, usa-se o controlador **slider** onde é possivel passar o valor mínimo, máximo e o step.