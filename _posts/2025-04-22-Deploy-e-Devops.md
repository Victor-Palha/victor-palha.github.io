---
layout:
title: "Deploy, DevOps e por que você deveria parar de fazer gambiarra em produção"
description: "Descubra o que é DevOps, como fazer um deploy sem causar um incêndio, e por que subir código via FTP é coisa de criminoso."
date: 2025-04-22 15:32:00 -0000
categories: [DevOps, Deploy]
tags: [CI/CD, Docker]
---

# _"Se funcionar em produção, tá certo"_
Essa frase aí é o equivalente de dizer: "Se não explodiu, pode acender outro fósforo." E se você vive assim, sinto te dizer, mas você é o terror dos times de suporte.

Mas calma, ninguém nasce sabendo. Eu tô aqui pra te ensinar o que é DevOps, o que é Deploy e como parar de ser um agente do caos digital.

# O que é Deploy?
Deploy (ou implantação, se você for purista do português) é o ato de **colocar sua aplicação no ar**. Simples assim. É o momento mágico onde todo o seu código maravilhoso (ou nem tanto) vai pra um servidor, e os usuários finalmente vão poder dizer: “essa merda não funciona”.

_Ah, mas eu uso FileZilla e jogo os arquivos no FTP, e sempre funcionou..._

Funcionou porque Deus é bom e ainda tem paciência contigo. Mas um dia, essa paciência acaba. E aí, meu querido, você vai deletar a pasta `public_html` e derrubar o site inteiro. Vai ser demitido com estilo.

# O que é DevOps?
DevOps é uma cultura, uma filosofia, um estilo de vida... Mentira, é só uma forma mais inteligente de você parar de jogar código na sorte e começar a usar **ferramentas, automações e boas práticas** pra garantir que seu sistema não vá pro caralho na próxima mudança.

Pensa no DevOps como aquele colega organizado da escola que tinha tudo separado por cor, fazia resumo com marca-texto e ainda colava a resposta certa na prova pra te ajudar. Ele é o cara que diz: “Vamos fazer CI/CD”, enquanto você ainda tá usando `scp` com senha no terminal.

# CI/CD? Que merda é essa?

CI/CD é a sigla pra:

- **CI** – Integração Contínua (Continuous Integration): você sobe o código e ele já é testado automaticamente.
- **CD** – Entrega Contínua (Continuous Delivery) ou Implantação Contínua (Continuous Deployment): seu código sobe sozinho pro servidor, sem precisar de intervenção manual. Tipo magia negra, só que com YAML.

Um exemplo com **GitHub Actions**:

```yaml
name: Deploy de verdade

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout do código
        uses: actions/checkout@v3

      - name: Instalar dependências
        run: mix deps.get

      - name: Rodar testes
        run: mix test

      - name: Deploy pra produção
        run: echo "Aqui iria o script de deploy, se você não fosse um preguiçoso"
```

_Ah, mas eu não sei configurar isso..._

E nem vai saber se continuar só reclamando. Vai estudar, cacete.

# E Docker, serve pra quê?

Docker é tipo um container mágico que embala sua aplicação com tudo que ela precisa pra rodar. É como se você mandasse o seu app com mochila, lancheira, água, e um bilhete da mamãe dizendo: “esse aqui tá pronto, só precisa rodar”.

Um exemplo básico de `Dockerfile` pra Elixir:

```Dockerfile
FROM elixir:1.15

WORKDIR /app
COPY . .

RUN mix deps.get
RUN mix compile

CMD ["mix", "phx.server"]
```

Tá aí. Agora em vez de “funciona na minha máquina”, você vai ter um: “funciona na porra de qualquer lugar”.

# Automatização não é frescura
Você não é especial por fazer tudo na mão. Você é só burro. Automatizar é como escovar os dentes: você não precisa, mas se não fizer, eventualmente vai feder e cair tudo.

Quer subir a aplicação sem precisar conectar no servidor manualmente? Usa **GitHub Actions**, **GitLab CI**, **CircleCI**, **Fly**, **Render**, **Railway**, o que quiser, só não usa mais `ssh` e `vim` pra editar código em produção. Isso é feitiçaria reversa.

# Resumo para você que não leu nada
- Deploy é botar sua aplicação no mundo sem derrubar tudo.
- DevOps é pra deixar isso menos traumático.
- Se você faz deploy manual, você está um passo de distância de perder seu emprego.
- CI/CD existe pra automatizar esse processo.
- Docker existe pra você não depender da sorte da “sua máquina”.
- Automatizar é o mínimo.

Se você leu até aqui e ainda não entendeu, me desculpa, mas nem o Kubernetes vai te salvar.
