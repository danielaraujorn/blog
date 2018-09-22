---
layout: post
title: "Gerenciamento de locais"
date: 2018-09-10
author: Daniel Araújo
cover: "https://www.udf.edu.br/wp-content/uploads/2017/05/20170508_planta-baixa_arquitetura.jpg"
tags: usuario gerenciamento cadastro locais
comments: true
---

Neste post mostraremos como criar ou editar locais passo a passo pelo sistema [SaIoT](https://saiot.ect.ufrn.br).

### O que são locais?

Para modelar o formato de casas, indústrias, instituições públicas e qualquer local físico, criamos os locais virtuais onde você pode definir os cômodos de sua casa, por exemplo, e conseguir descrever as divisões dos mesmos. Cada local possui permissões de acesso para usuários cadastrados.

<!-- Colocar o link do post sobre permissões -->

### Exemplo

Para dividirmos a casa abaixo e conseguir cadastrar seus cômodos, precisamos ter uma noção de cômodos que pertencem a outros cômodos. Um banheiro, por exemplo, é um local que pode ser um banheiro social ou de uma suíte, então o local banheiro pode pertencer a um quarto que por sua vez pertence ao primeiro andar da casa e que pertence a casa em si.

![Planta baixa de uma casa]({{site.baseurl}}/assets/post/gerenciamento-locais/local-planta-baixa.jpg)

Nesse exemplo, poderíamos criar o local **Casa de praia** onde ele possui locais como **Quarto 1**, **Quarto 2**, **Suíte**, **Cozinha**, dentre outros. A **Suíte** possui o local **Banheiro**.

### Cadastrando um local

Na imagem abaixo mostramos o cadastro do local **Casa**.

![Cadastro do local Casa]({{site.baseurl}}/assets/post/gerenciamento-locais/cadastro-locais-1.png)

Em seguida vamos cadastrar um local chamado **Quarto** pertencente ao local **Casa**.

![Cadastro do local Quarto]({{site.baseurl}}/assets/post/gerenciamento-locais/cadastro-locais-2.png)

### Excluindo um local

No gerenciamento também é possível apagar um local. Quando isso é feito, todos os dispositivos que estão vinculados ao local ficarão visíveis para os usuários que os cadastraram no sistema. Assim, o usuário que cadastrou o dispositivo poderá vinculá-lo novamente a um novo local.

<hr>

Assim mostramos como cadastrar uma casa que possui um único quarto e podemos associar os dispositivos cadastrados aos locais e permitir uma mais fácil gestão de permissões de usuários e também a visualização e controle de dados.
