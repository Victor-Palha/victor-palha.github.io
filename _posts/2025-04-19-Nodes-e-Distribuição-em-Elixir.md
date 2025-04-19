---
layout:
title: "Nodes e Distribuição em Elixir"
description: "Vamos entender o que são nodes e como podemos distribuir nossas aplicações em Elixir. Se você não entender com esse post, você não vai entender com mais nada."
date: 2025-04-19 16:18:20 -0000
categories: [Elixir, OTP]
tags: []
---

# _"O que caralhos são Nodes em Elixir?"_

Normalmente, eu começaria o post com uma pergunta idiota e provavelmente te xingando. Entretanto! Essa é uma pergunta muito válida e extremamente importante para entender o porquê de usar Elixir e Erlang!

Então, pega um café, um cigarro, **desliga essa música e bora!**

---

## O básico

Bom, como sempre, vou assumir que você é burro pra caralho e vou te explicar tudo do mesmo jeito que eu explicaria para um macaco manco (_não que eu tenha feito isso antes, mas você entendeu a ideia_). Então, vamos lá!

Para entender o que são **Nodes**, você precisa ter uma noção do que são **sistemas distribuídos**. Se você nunca ouviu falar sobre isso, é um assunto muito longo e não entra no escopo desse post, mas eu vou te dar um **resumo do resumo do resumo**, só pra você conseguir acompanhar esse post — mesmo que no fim você ainda não entenda porra nenhuma.

---

## Sistemas distribuídos

![img](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb145d610-0804-4a50-b74b-bf13cac6fb8f_1600x1005.png)


Sistemas distribuídos são uma forma de **distribuir um sistema** (_dã_) para que ele consiga rodar em **mais de um servidor**. Ou seja, você pode ter um sistema com muitas responsabilidades e **dividir essas responsabilidades entre vários servidores**. Assim, você consegue **escalar seu sistema vertical e horizontalmente**, ou seja, pode adicionar mais servidores ou aumentar a capacidade dos que você já tem.

_Caralho, não entendi nada!_

Eu sei que você não entendeu nada, mas não se preocupe, eu vou dar um exemplo pra você, que tem 1.5 neurônios e não consegue entender coisa nenhuma.

Vamos supor que você tem um sistema de vendas. Esse sistema tem várias responsabilidades, como:

- Cadastro de produtos  
- Cadastro de clientes  
- Autenticação  
- Vendas  
- Relatórios  
- Envio de e-mails  
- Pagamento  

Beleza! Agora imagina que você está trabalhando nesse projeto como um monolito, ou seja, tudo está rodando em **um único servidor**. Entretanto, o sistema está crescendo e você precisa escalar ele (_caso você não saiba, escalar significa que o sistema está crescendo e você precisa de mais recursos pra ele continuar funcionando direito_).

Então, você tem duas opções:

1. Aumentar a capacidade do servidor que você já tem — mais memória, mais CPU, mais disco, etc.  
2. Criar mais servidores!

No segundo caso, quando você cria mais servidores, você está **duplicando a capacidade** do sistema e **distribuindo a carga**!

Bom, agora que você tem uma leve noção do que é um sistema distribuído, bora voltar pro foco do post.

---

## Nodes em Elixir

Como você provavelmente já sabe (_ou deveria saber_), Elixir roda em cima da **máquina virtual do Erlang**, e isso dá superpoderes pra nossa linguagem! Isso porque o **Erlang foi construído para distribuição**. Tem todo um contexto histórico envolvido, mas foda-se, pesquisa isso depois!

_Tá, mas o que isso quer dizer?_

Isso quer dizer que o Elixir é **ótimo pra sistemas distribuídos também!** A gente pode aproveitar a arquitetura distribuída do Erlang pra fazer aplicações Elixir se comunicarem entre si, de forma nativa!

_Caralho, não explicou nada!_

Okay, eu esqueci que você é burro, então vamos com calma, beleza?

![img-nodes](https://www.researchgate.net/publication/331679222/figure/fig3/AS:735622553681922@1552397492158/Network-nodes-relationship-diagram-A-network-node-undirected-graph-is-shown-in-figure-3.ppm)

Imagine que você tem um **sistema em Elixir responsável por envio de e-mails**, certo?  
Agora imagina que nesse sistema existe um **GenServer** que fica esperando uma mensagem pra enviar um e-mail pra um usuário.

Agora... pensa que você quer **escalar esse sistema**, separando o envio de e-mails do resto da aplicação principal. Em vez de colocar tudo num só servidor, você pode deixar o módulo de envio de e-mails rodando **em outro Node**, ou seja, **em outro servidor**, mas ainda assim, os dois Nodes podem **se comunicar entre si como se estivessem no mesmo lugar**.

Isso é possível porque no mundo mágico do Erlang/Elixir, os Nodes podem se conectar, trocar mensagens, spawnar processos uns nos outros, e até monitorar uns aos outros como se tudo fosse parte do mesmo sistema mesmo espalhado pelo mundo!

---

## Respondendo: o que são Nodes?

Nodes são **NÓS** em uma rede. Se você tem um cluster e **dentro desse cluster você tem 10 sistemas Elixir, cada um desses sistemas são _NODES_** e eles podem conversar entre si!

---

## Conectando Nodes

Pra deixar a parada mais concreta, aqui vai um exemplo rapidola pra você que é burro e precisa ver código pra confiar no que eu tô dizendo!

Primeiro, abre **dois terminais** e roda o seguinte comando em cada um deles:

```bash
# Terminal 1
iex --sname node1 --cookie chavesupersecreta

# Terminal 2
iex --sname node2 --cookie chavesupersecreta
```

O `--sname` define o nome do node, e o `--cookie` é uma chave de segurança que permite que os nodes se conectem entre si. Você pode usar qualquer nome e qualquer cookie, **mas eles precisam ser IGUAIS nos dois nodes**, senão nada vai funcionar e você vai ficar chorando sem saber por quê.

Agora vá para o **terminal 2**, ou seja, onde o **node2** tá rodando, e execute o seguinte comando:

```elixir
Node.connect(:"node1@nome_da_maquina")
# => true
```

Isso vai conectar o **node2** ao **node1**. Se você prestar atenção, a sintaxe é bastante simples:

```elixir
:"nome_do_node@nome_da_maquina"
```

Onde `node1` é o nome do node e `nome_da_maquina` é o nome da máquina onde o node está rodando. Você pode descobrir o nome da sua máquina só olhando pro terminal:

```bash
iex --sname node2 --cookie chavesupersecreta
Interactive Elixir (1.18.3) - press Ctrl+C to exit (type h() ENTER for help)
iex(node2@Strahds-MacBook-Air)1>
```

O nome da máquina é o que vem **depois do `@`**, ou seja, `Strahds-MacBook-Air` no meu caso.

_"Ah, beleza! Mas como eu posso saber que eles tão mesmo conectados?"_

Quando você escreveu o `Node.connect(:"node1@nome")` no terminal, você recebeu um `true`. Mas já que você é desconfiado...

Vai no **terminal 1**, onde o **node1** tá rodando, e executa o seguinte:

```elixir
Node.list()
# => [:"node2@nome_da_maquina"]
```

Esse comando vai listar todos os nodes conectados ao **node1**. Se tudo estiver certo, você deve ver o **node2** na lista.


# Parabéns, você aprendeu o que são nodes e como conectar eles!  
**Você não é tão burro assim!** 👏👏👏

_Okay... Eles tão conectados... E agora?_

Agora, meu querido, começa a mágica: **processos distribuídos, envio de mensagens entre nodes, clusters, tolerância a falhas e tudo aquilo que faz Elixir parecer coisa de outro planeta.**

Mas isso é assunto pro próximo post, esse aqui foi só para te explicar o que são **Nodes** e por que quando falamos de sistemas distribuidos o **Elixir e Erlang** brilham

Se você chegou até aqui, toma um café, acende outro cigarro e vai dar uma volta. Você merece.

É isso gente, espero que vocês tenham aprendido alguma coisa com esse post. E se você não aprendeu nada, pelo menos você se divertiu lendo as merdas que eu escrevi. E se você não se divertiu, foda-se também, porque eu não sou seu palhaço. bjs.

# Referências
Se você quiser aprender mais sobre Nodes e distribuição em Elixir, toma aqui alguns links que podem te ajudar:

[!Elixir Node Module](https://hexdocs.pm/elixir/1.18.3/Node.html)
[!Distribuição OTP](https://elixirschool.com/pt/lessons/advanced/otp_distribution)
[!28 Days - Elixir Node Networking Basics](https://stephenbussey.com/2018/02/06/elixir-node-networking-basics.html)
[!Managing a massive amount of distributed Elixir nodes – ElixirConf 2023 (YouTube)](https://www.youtube.com/watch?v=ijPph39FY4Q)