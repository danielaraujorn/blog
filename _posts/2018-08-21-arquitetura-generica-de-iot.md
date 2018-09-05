---
layout: post
title: "Arquitetura genérica de IoT"
date: 2018-08-21
author: Judson Costa
cover: "https://inforchannel.com.br/wp-content/uploads/2017/05/cidade-inteligente2.jpg"
tags: arquitetura middleware sistema
comments: true
---

Ao longo desta postagem será caracterizado uma arquitetura para internet das coisas que facilita instalar dispositivos inteligentes sem complicações e conhecimentos técnicos; monitorar seus dados e visualizá-los em relatórios gráficos e; facilitar o controle individual ou em grupo para todo e qualquer dispositivo conectado, além de possibilitar o seu uso sem estar necessariamente conectado externamente à internet, ou seja, usar apenas na rede interna da residência. A aplicação mostrará os dados de consumo de energia a partir de tomadas inteligentes e de água por meio de hidrômetros conectados, além de acionamento de lâmpadas e grupos com diversos dispositivos. Tudo isso a partir de um [aplicativo web](https://developers.google.com/web/fundamentals/codelabs/your-first-pwapp/?hl=pt-br) e uma central conectada a todos os dispositivos da residência.

Esta arquitetura adota: o protocolo **Wi-Fi** - por se tratar de uma rede de comunicação sem fio que apresenta diversas vantagens como: mobilidade, compatibilidade, compartilhamento, acessibilidade, baixo custo de manutenção, segurança e popularidade; a tecnologia [**_WebSocket_**](https://developer.mozilla.org/pt-BR/docs/WebSockets) e [**MQTT**](https://www.ibm.com/developerworks/br/library/iot-mqtt-why-good-for-iot/index.html) - para comunicação em tempo-real e bidirecional; e o protocolo  [**HTTPS**](https://pt.wikipedia.org/wiki/Hyper_Text_Transfer_Protocol_Secure) - para a transferência de dados com conexão criptografada.

### Unidade central

A unidade central (ou _middleware_) é o principal componente desta arquitetura, é o intermediador entre a aplicação web acessada pelo usuário e todos os dispositivos inteligentes conectados na residência. Além de permitir o controle sem que o usuário esteja obrigatoriamente conectado à internet, a não ser para acesso externo, ela é responsável pelo gerenciamento de todos os pedidos da interface web; pelas solicitações do bloco de regras; pelo armazenamento de todos os dados gerados pelos sensores; e pelo acionamento dos atuadores por usuários e/ou bloco de regras. A central é um [_Web Service_](https://pt.wikipedia.org/wiki/Web_service) que recebe dados seguindo um formato pré-definido e retorna as informações solicitadas. É a partir desse formato de saída e entrada de dados que é possível universalizar essa aplicação para se comunicar com requisições de dados a partir de qualquer cliente e dispositivo, independente de quem esteja solicitando ou inserindo esses dados.

A aplicação segue os princípios do padrão [_Representational State Transfer (RESTful)_](https://becode.com.br/o-que-e-api-rest-e-restful/) que utiliza os métodos do [HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP) para acesso externo à central que recebe os dados gerados dos seguintes componentes: consumo de água a partir de hidrômetros; consumo energético a partir de tomadas; registro de acionamento do sistema de iluminação e das tomadas. O _middleware_ é constituído de várias ferramentas e bibliotecas que funcionam integradas entre si, como é mostrado na Figura a seguir.

![Unidade central]({{site.baseurl}}/assets/central.png)

### Integração

Os dispositivos conectam-se à unidade central através da rede _Wi-Fi_ da residência e utilizam os protocolos HTTPS, MQTT ou _WebSocket_. A escolha por qual protocolo utilizar depende principalmente do tempo de resposta que se exige nas transferências dos dados, como a ação de ativar determinada lâmpada e o envio de uma medição realizada por um hidrômetro têm prioridades diferentes, enquanto o primeiro caso necessita que a ação seja realizada instantaneamente, o segundo não precisa ter uma resposta tão imediata. Todos os dispositivos reportam-se à central ao identificarem alguma anormalidade, como problemas no envio dos dados, na conexão com a internet ou seu próprio reinício inesperado. Os dispositivos e usuários são autenticados seguindo a especificação do [_OAuth 2.0_](https://www.digitalocean.com/community/tutorials/uma-introducao-ao-oauth-2-pt) que padroniza a autenticação através de chaves de segurança temporárias, diminuindo a exposição de dados sensíveis, como senhas.

![Comunicação entre os dispositivos]({{site.baseurl}}/assets/integracao.png)

Como é visto na Figura acima, os dispositivos inteligentes (tomada, hidrômetro e lâmpada) comunicam-se com a unidade central ou a nuvem, recebendo ações a serem executadas e enviando dados monitorados. Devido ao baixo nível de urgência de resposta, o hidrômetro é o único que conecta-se diretamente à nuvem e utiliza unicamente o protocolo HTTP, representado pelo POST, que é um dos métodos deste protocolo. Utiliza-se o método POST para enviar dados para o servidor, sendo essa a finalidade do hidrômetro, fazer o monitoramento do consumo de água e armazená-las no servidor. Diferente do hidrômetro, a interface do usuário, a tomada e a lâmpada tem alto nível de urgência de resposta, exigindo uma resposta imediatamente após o acionamento. Para suprir essa necessidade, ambos os dispositivos conectam-se diretamente à unidade central por meio do protocolo _WebSocket_ para comunicação em tempo-real. Além de gerenciar todas essas conexões com diferentes dispositivos e protocolos, o _middleware_ também conecta-se à nuvem em casos específicos em que a requisição solicita alguma informação que não é encontrada localmente, ou quando necessita, por segurança, fazer _backup_ dos dados já armazenados, ou ainda possibilitar o acesso externo do usuário à própria residência, sendo isso opcional.

### Validação

Para validar a arquitetura proposta, os dispositivos foram desenvolvidos e utilizados em ambiente experimental que permite monitorar o consumo de energia e água de aparelhos reais. O ambiente de teste consiste em um aquário com água, uma pequena bomba de aquário, um hidrômetro e uma tomada inteligente. A bomba, localizada dentro do aquário, bombeia a água para o hidrômetro através de um pequeno cano. A bomba está plugada à tomada para assim permitir a medição do seu consumo energético. Todos os equipamentos ficam ligados 24 horas por dia, enviando os dados captados durante todo esse tempo para a nuvem.

### Conclusão

A partir dos dados obtidos, pode-se comparar as medições com os valores fornecidos pela fabricante da bomba de aquário, verificando se o consumo obtido pela tomada inteligente é coerente com o consumo teórico disponibilizado. Quanto ao hidrômetro, compara-se o valor acumulado registrado analogicamente com o somatório dos pulsos contabilizados digitalmente, resultando no mesmo valor total. Ao fim da análise, verificou-se que a unidade central se comportou de forma correta, não perdeu nenhum dado recebido dos dispositivos conectados e disponibiliza as informações para a interface do usuário sem nenhuma inconsistência.
