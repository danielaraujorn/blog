---
layout: post
title: "Cadastro de dispositivos"
date: 2018-09-10
author: Judson Costa
cover: "/blog/assets/post/cadastro-dispositivo/dispositivo.jpg"
tags: dispositivos cadastro usuario
comments: true
---

Neste post mostraremos como gerenciar os dispositivos passo a passo pelo sistema [SaIoT](https://saiot.ect.ufrn.br).

<!-- Este post faz parte de uma série de postagens que mostra como utilizar o [SaIoT](https://saiot.ect.ufrn.br), uma plataforma genérica para internet das coisas.

Essa série é dividida da seguinte forma e recomenda-se fazer a leitura seguindo essa mesma ordem:
- [Cadastro de locais]({{site.baseurl}}/2018/09/10/cadastro-de-locais)
- [Cadastro de dispositivos](#)
- [Histórico](/blog/)

# Cadastro de dispositivos -->

### Criando uma conta

Primeiramente é preciso que você se registre no [SaIoT](https://saiot.ect.ufrn.br). Isso pode ser feito inserindo as credenciais diretamente no SaIoT ou utilizando uma conta do [SIGAA](https://sigaa.ufrn.br), caso você tenha uma, como mostrado na imagem a seguir.

![Tela de registrar conta]({{site.baseurl}}/assets/post/cadastro-dispositivo/registro.png)

### Cadastrando um dispositivo
 
Após criar uma conta, é preciso [cadastrar um local]({{site.baseurl}}/2018/09/10/cadastro-de-locais). Ao ligar o dispositivo, será necessário configurar o acesso ao _Wi-Fi_ (Configure WiFi) conectando à rede criada pelo próprio dispositivo.

![tela do dispositivo no modo ap]({{site.baseurl}}/assets/post/cadastro-dispositivo/modoap.png)

É preciso definir qual o nome (SSID) e senha da rede disponível para conectar o dispositivo à internet, além do email e senha de uma conta do SaIoT.

![tela do dispositivo no modo ap]({{site.baseurl}}/assets/post/cadastro-dispositivo/wifimanager.png)

Com o dispositivo conectado à internet, acesse o [SaIoT](https://saiot.ect.ufrn.br) com as mesmas credenciais utilizadas na configuração do dispositivo. Na interface, o dispositivo aparecerá na lista de pendentes de cadastro no menu superior (+).

![ícone para o abrir o menu de pendentes]({{site.baseurl}}/assets/post/cadastro-dispositivo/pendente.png)

Pode-se identificar o dispositivo pelo serial e escolher um local e, se quiser, editar as informações do dispositivo para então cadastrá-lo ao sistema.

![tela com dispositivo pendente]({{site.baseurl}}/assets/post/cadastro-dispositivo/listapendente.png)

Finalizado o cadastro do dispositivo, pode-se gerenciá-lo na tela de cadastro disponível no menu lateral.

![tela de cadastro do dispositivo]({{site.baseurl}}/assets/post/cadastro-dispositivo/cadastro.png)

Como mostrado na imagem acima, todos os dispositivos cadastrados no local selecionado ficam acessíveis para gerenciamento, como edição de suas informações ou exclusão do sistema.

<!-- colocar link e falar do post que explica o JSON de cadastro pra desenvolvedor -->