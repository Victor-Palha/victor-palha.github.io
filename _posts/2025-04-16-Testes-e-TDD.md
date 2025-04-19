---
layout:
title: "Testes, TDD e por que você deveria aprender essa merda"
description: "Aprenda o que é teste, como funciona e porque você deveria usar isso no seu dia a dia, ou não... Não sou teu pai pra ficar te obrigando a fazer nada."
date: 2025-04-17 16:18:20 -0000
categories: [Elixir, Testes]
tags: [TDD, Testes]
---

# _"Teste é para quem não confia no código"_
Primeiramente, se você pensa assim, você tem o QI menor que um macaco prego. E não, não é uma ofensa, é só uma constatação.
Mas o lado bom, é que eu estou aqui para fazer com que seu QI aumente e você consiga entender o que é um teste e como ele pode te ajudar a não fuder com o seu código!

# O que são testes?
Bom, testes são um conceito que existe em absolutamente todas as linguagens de programação (Embora algumas não tenham suporte nativo para isso). Eles servem para garantir que o seu código está funcionando como deveria, isso é bem óbvio...

_Ah, mas eu sou idiota e não entendi_

Tudo bem meu amigo, pega um café, um cigarro, desliga essa porra de música e presta atenção.

Pensa assim, quando você tava na faculdade (ou escola, sla) você não tinha um professor que ficava te dando exercícios para você fazer? E quando você entregava o exercício, o professor olhava e falava "isso tá certo" ou "isso tá errado"? Então, os testes são a mesma coisa, só que ao invés de um professor, **é o computador que vai olhar e falar se tá certo ou errado**. E o melhor de tudo, é que ele não vai ficar puto com você se você errar _(só vai baixar sua autoestima, não que você tenha alguma)_, ele só vai te mostrar onde você errou e deixar você corrigir.

_Ah, mas como que eu faço isso?_

Calma, aquieta o cu, eu já vou te ensinar. Primeiro aprende os conceitos básicos de teste, depois a gente parte para o código, fechou?

# Tipos de testes
Existem alguns tipos de testes, mas os mais comuns são:

![Tipos de testes](https://miro.medium.com/v2/resize:fit:1400/1*S-WQ9KwM7kkmwKWy41SPYw.png)

## Testes unitários
Cara, você pode ter o QI de um macaco, mas se você souber somar 1 + 1, você vai entender o que é um teste unitário.
Como o nome mesmo já diz, ele serve para testar uma unidade do seu código. Ou seja, ele vai testar uma função, um módulo, uma classe, etc.

_Ah, mas o que é uma unidade?_

Cara... Assim, unidade é uma parte do seu código que faz uma coisa só. Por exemplo, se você tem uma função que soma dois números, essa função é uma unidade. Se você tem uma classe que representa um carro, essa classe é uma unidade. Se você tem um módulo que faz requisições HTTP, esse módulo é uma unidade.
Geralmente, uma unidade é uma parte do seu código que tem uma única responsabilidade, eu poderia falar de **SOLID** mas se você não sabe nem o que é um teste, você não vai entender o que é SOLID agora.

Para deixar ainda mais simples para sua cabeça de vento, bora botar um exemplo em **ELIXIR**:

```elixir
@spec unit(String.t()) :: String.t()
def unit(name) do
    "Meu nome é #{name} e eu sou um babuíno boboca balbuciando em bando!"
end
```

Isso aí é uma função que recebe como parâmetro um nome e retorna uma string. Essa função é uma unidade de código, porque ela faz uma coisa só: retorna uma string com o nome que você passou como parâmetro.

_Ah, mas como que eu testo isso?_

Calma animal, calma. Isso é só um exemplo do que é uma unidade, depois eu vou te ensinar a testar isso. Agora vamos para o próximo tipo de teste. Entendeu? Se não entendeu, foda-se, você não vai entender mesmo e não vale a pena perder tempo com isso.

## Testes de integração
Bom, agora que tu já entendestes o que é um teste unitário, bora bater um papo sobre testes de integração.
Cara, pensa assim, você tem um computador, certo? E você tem um mouse e um teclado **(Não vem dar uma de espertinho e falar que tem um notebook)**. Se você ligar o computador e não conectar o mouse e o teclado, você simplesmente não vai conseguir fazer nada. 
Então, agora pensa em um sistema, a gente tem várias partes que se comunicam entre si, né? E se uma parte não funcionar, a outra parte que utiliza ela também não vai funcionar! Então, os testes de integração servem para garantir que todas as partes do seu sistema estão funcionando juntas, como um computador ligado com mouse e teclado.

_Ah, entendi! Eu não sou tão burro assim!_

Bom, isso é discutível... Mas vamos lá, vamos continuar com um exemplo em **ELIXIR**:

Imagine que você está desenvolvendo um sistema de cadastro de usuários. Você tem uma função que recebe os dados e outra função que os salva em algum lugar. A função que salva é uma UNIDADE do seu código, certo? Mas a que recebe os dados e chama a que salva precisa funcionar junto com ela. Então, além dos testes unitários, você também precisa garantir que essas duas funções funcionam juntas, ou seja, é necessário fazer um teste de integração.

1. Função que salva os dados:

```elixir
@spec save_monkey(map()) :: {:ok, map()}
def save_monkey(%{name: name, qi: qi}) do
  # Aqui você vai salvar os dados em algum lugar, tipo um banco de dados, ETS, etc.
  # Mas como você não tem nada disso, só vai retornar os dados mesmo
  {:ok, %{name: name, qi: qi}}
end
```
2. Função que recebe os dados:

```elixir
@spec receive_monkey(String.t(), integer()) :: {:ok, map()}
def receive_monkey(name, qi) do
  # Aqui você vai receber os dados e chamar a função que salva os dados
  %{name: name, qi: qi} |> save_monkey()
end
```

Bom, agora você tem duas funções simples, e pode aplicar testes unitários em cada uma delas separadamente. Mas também precisa garantir que elas funcionam juntas. Por isso, você vai fazer um teste de integração que chama a função receive_monkey/2 e verifica se os dados foram salvos corretamente.

Isso é um teste de integração, porque você está testando duas partes do seu código que precisam funcionar juntas.

## Testes end-to-end (ou E2E)

Agora que você já entendeu o que é um teste unitário e um de integração, vamos para o final: o **teste end-to-end**, ou E2E pros íntimos.

_Ah, mas o que é isso?_

Cara, se tu não manja dos inglês, é o seguinte: **E2E** significa "do começo ao fim", ou seja, você vai ficar mais burro ainda e fingir que é um usuário de verdade usando essa porra de sistema. Você vai fazer tudo que um usuário faria, inclusive as merdas que ele faria, tipo chamar o endpoint errado, mandar dados inválidos e tudo isso.

Esse tipo de teste é basicamente um roleplay com o seu sistema. Você vai fingir que é um usuário **de verdade**, usando a aplicação como se estivesse num ambiente real, preenchendo formulários, mandando requisições, fazendo o que for necessário pra garantir que **do começo ao fim** tudo tá funcionando como deveria.

Pensa comigo, lembra daquele sistema de cadastro de usuários? Imagina agora que ele tá rodando bonitinho numa aplicação web em Pheonix. Um teste E2E seria algo do tipo:
1. Preencher o seu nome de macaco e o QI de -2.
2. Chamar o endpoint de cadastro.
3. Verificar se apareceu uma mensagem de sucesso, ou se os dados foram salvos e retornados corretamente.

Ou seja, você tá testando **o sistema inteiro**, desde a interface do usuário até o banco de dados (ou ETS, ou qualquer merda que você tenha colocado pra armazenar os dados).

_Ah, mas isso não é muito exagero não?_

Mlk, eu to tentando te passar a call, cala a boca e lê a do merda do post. O teste E2E tá aí justamente pra garantir que a experiência do usuário não vá pro ralo só porque você esqueceu de testar se o endpoint de cadastro tá funcionando. É tipo um teste de integração, mas com uma visão mais ampla, testando tudo que envolve a aplicação.

Na prática, em Elixir, o E2E costuma ser mais comum quando você tem uma aplicação Phoenix, e você quer garantir que **as rotas**, **os controladores**, **os plugs**, **os contextos** e tudo mais estão se comportando como esperado. E se tiver frontend envolvido, aí entra Cypress, Playwright ou qualquer outro brinquedinho pra testar a parte visual também.

Pronto, agora você tá minimamente alfabetizado em testes. Agora só falta parar de ser preguiçoso e começar a escrever eles, seu idiota.

# TDD
Agora que você teóricamente já sabe o que é um teste, vou tentar te explicar o que é **TDD** (ou **Test Driven Development**). E eu sei que você é burro, então eu vou tentar ser o mais didático possível.

## O que é TDD?
Eu poderia falar sobre a história do TDD, mas eu não quero que você queime os 2 neurônios que você tem lendo isso. Então, vamos direto ao ponto.

**TDD** é uma técnica de desenvolvimento de software que se baseia na ideia de **que você deve escrever os testes antes de escrever o código**.

_Agora fudeu tudo, não entendi nada! Como que vou escrever um teste para uma coisa que não existe ainda??_

Calma aí, deixa eu terminar de explicar antes de você sair quebrando tudo. O TDD é dividido em três etapas principais:

![TDD](https://www.xenonstack.com/hubfs/images/xenonstack-test-driven-development-tools-best-practices.png)

1. **Red**: Você escreve um teste que falha. Isso significa que você tá testando uma funcionalidade que ainda não existe, então o teste vai falhar. E isso é bom, porque significa que você tá testando algo que realmente precisa ser implementado.

2. **Green**: Agora você escreve o código necessário para fazer o teste passar. Isso significa que você vai implementar a funcionalidade que você testou no passo anterior. E quando o teste passar, você pode ficar feliz, porque significa que você fez a coisa certa.

3. **Refactor**: Agora que o teste tá passando, você pode refatorar o código. Isso significa que você pode melhorar o código, deixar ele mais limpo, mais legível, mais eficiente, etc. E o melhor de tudo, você pode fazer isso com segurança, porque se alguma coisa quebrar, os testes vão te avisar.

_Porra, mas isso é muito trabalho! Eu não tenho tempo pra isso!_

Cara, infelizmente eu vou ter que concordar com você... O TDD é um trabalho extra, e não é todo mundo que gosta de fazer isso. Mas a verdade é que... **FODA-SE O QUE TU ACHA SEU ANIMAL!** Se tu tá fazendo um projeto pequeno que absolutamente ninguém vai usar ou se importar, beleza, não precisa fazer TDD ou teste nenhum (Até pq ninguém vai ver seu código mesmo). Mas se você tá fazendo um projeto grande, que vai ser usado realmente por alguém, ou que vai ser mantido por outra pessoa, você precisa fazer TDD. E não só isso, você precisa fazer testes unitários, de integração e E2E. Porque se você não fizer isso, você vai acabar com um código uma bosta, cheio de bugs e problemas que vão te dar dor de cabeça no futuro.

E acredite, você não quer isso. Você não quer passar horas tentando descobrir porque o seu código tá quebrando, ou porque uma funcionalidade que você implementou não tá funcionando. Você não quer passar horas tentando entender um código que foi escrito por outra pessoa e que não tem nenhum teste.

# Conclusão
Eu sei que eu falei que ia mostrar como fazer testes, mas a verdade é esse post já ficou muito grande e já te ensinei o básico e que é universal para qualquer linguagem. Então, se você não entendeu nada, foda-se, vai ler outro post ou vai fazer outra coisa da sua vida. Mas se você entendeu e quer aprender mais sobre testes, TDD e como aplicar isso no seu dia a dia, eu recomendo que você **leia a documentação do Elixir e do Phoenix**, porque lá tem tudo que você precisa saber.

Mas se você não quer ler a documentação, fica tranquilo, eu vou fazer um post só sobre como fazer testes em Elixir e Phoenix, com exemplos práticos e tudo mais. Então fica ligado aqui no blog, porque vai ter muita coisa boa vindo por aí.

É isso gente, espero que vocês tenham aprendido alguma coisa com esse post. E se você não aprendeu nada, pelo menos você se divertiu lendo as merdas que eu escrevi. E se você não se divertiu, foda-se também, porque eu não sou seu palhaço. bjs.