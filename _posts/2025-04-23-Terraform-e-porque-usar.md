---
layout:
title: "Terraform, infraestrutura como código e como parar de fazer gambiarra na AWS"
description: "Se você ainda cria servidor manual no painel da AWS, esse post é pra você. Terraform é o caminho da luz. Descubra isso assim como eu descobri: na marra!"
date: 2025-04-23 16:03:00 -0000
categories: [DevOps, Terraform]
tags: [IaC]
---

# _"Eu tenho tudo anotado no Notion"_

Se você precisa abrir o Notion pra lembrar qual porta tá liberada no seu EC2, sinto muito: você está vivendo na idade da pedra da infraestrutura. E eu digo isso com carinho, porque eu aprendi da pior forma possível!

Seja bem-vindo ao maravilhoso mundo do **Terraform**, onde tudo é código, nada é esquecido, e ninguém precisa clicar em botão nenhum pra subir servidor.

# O que é Terraform?

Terraform é uma ferramenta de **Infraestrutura como Código** (IaC). Com ela, você descreve sua infraestrutura em **arquivos `.tf`**, e o Terraform cuida de criar, atualizar ou destruir tudo pra você.

É tipo escrever receita de bolo, mas em vez de farinha e ovos, você coloca `aws_instance`, `aws_s3_bucket`, `module.vpc`, e voilá: sua infra nasce do código.

E sim, você pode versionar, revisar em pull request e até fazer rollback. Bem melhor que “copiar os passos do último deploy no Google Docs”.

_Ah, mas eu uso o painel da AWS e funciona..._  
Sim, funciona! Mas aí entra um grande problema: **só você sabe como funciona**. E quando você não tá lá, o que acontece?

Vou te dar um exemplo bem real que aconteceu comigo, mas antes de tudo, como de costume, **pega um café, um cigarro e desliga essa música**.

# O que aconteceu?

Era uma vez um desenvolvedor que atuava como freelancer. Esse desenvolvedor tinha um cliente que precisava de um site de notícias, então o cliente pediu para o desenvolvedor _colocar o site na internet_.

O desenvolvedor, feliz pela proposta, foi lá e fez um site lindo, maravilhoso, com um backend em **Node.js** e um frontend em **React**. O cliente adorou e pediu para o desenvolvedor colocar o site no ar.

O desenvolvedor, sem muita experiência em **DevOps**, decidiu usar o painel da AWS e criar tudo na mão. Ele criou uma instância EC2, um bucket S3, um banco de dados RDS e tudo mais que precisava. O cliente ficou feliz, o site funcionava, e o dev achou que tava mandando bem.

Mas, como toda boa história, isso não durou muito...

Depois de alguns meses com o site no ar, o cliente decidiu que queria mudar algumas coisas. Ele pediu para o desenvolvedor adicionar novas funcionalidades e até mudar o layout! O desenvolvedor então marcou uma reunião com o cliente e começou a entender o que ele queria.

O desenvolvedor **freelancer** fez o orçamento, mas o cliente achou caro. Decidiu, então, contratar outro dev pra fazer as alterações. O antigo dev, que mesmo assim queria manter a boa relação, preparou uma documentação básica e entregou tudo certinho.

Adivinha? O novo dev, ao abrir o painel da AWS, **ficou completamente perdido**. Nunca tinha usado AWS na vida. Não sabia como o site estava funcionando, não entendia as configurações da instância, nem pra que servia o S3 ou como o banco RDS tinha sido configurado.

E aí, o novo dev resolveu fazer tudo do zero. Criou nova instância, novo bucket, novo banco. Só que **apagou a instância antiga achando que não servia mais**. E com ela, foram embora:
- Os scripts de build,
- As configurações de ambiente,
- A conexão com o banco antigo,
- E claro, **o site que ainda tava no ar**.

Resultado? O cliente ficou sem sistema e **ligou pro antigo dev desesperado**.

E o que o antigo dev fez? Passou as próximas **duas semanas**:
- Restaurando backups,
- Caçando dados no painel da AWS,
- Explicando passo a passo como tudo estava ligado,
- E ajudando o novo dev a botar o Frankenstein no ar de novo.

**Sem ganhar nada por isso**. Nem um pix, nem um café.

# Moral da história?

Fazer tudo na mão pode parecer mais rápido e fácil. Mas vira uma **bomba-relógio** que só você sabe desarmar e um dia, ela explode **no seu colo**.

Se desde o começo tivesse usado uma stack com **infraestrutura como código** (tipo Terraform, Pulumi ou CloudFormation), um **pipeline CI/CD**, e uma documentação de verdade, qualquer pessoa conseguiria dar continuidade sem precisar de um curso intensivo de AWS.

## _"Mas dá trabalho configurar isso tudo..."_

Dá. Mas mais trabalho ainda é:
- Restaurar site derrubado,
- Explicar AWS por ligação no sábado,
- E tentar lembrar onde salvou aquela senha que nunca deveria estar só com você.

Se você quiser crescer como dev, seja freelancer, sênior numa empresa ou CTO de uma startup **você precisa documentar, automatizar e compartilhar conhecimento**.

Caso contrário, você vai ser aquele nome que aparece nos arquivos antigos com a legenda:  
_"ninguém sabe como esse cara fazia as coisas, mas funcionava..."_

## Pra não dizer que não avisei:
- AWS sem organização é só uma conta de luz mais cara.
- Deploy manual é dívida técnica com juros compostos.
- Se só você sabe como funciona, você não fez direito.
- Infra como código é o caminho da paz.
- Se não documentou, nem adianta dizer que terminou.


# Por que isso importa?

Porque **fazer tudo na mão é burrice**. Se você leu a história e ainda acha que tá tudo certo, sinto muito, mas você é o tipo de dev que vai fazer o time de suporte chorar.

- Já errou uma configuração de VPC e perdeu meia hora tentando entender por que o container não conecta?
- Já apagou uma instância sem querer?
- Já fez deploy e ninguém lembra mais como replicar em outro ambiente?

Pois é. Com Terraform, tudo isso pode ser evitado. Você escreve o código, versiona, e se der ruim, é só voltar pra versão anterior. **Sem drama, sem estresse.**

# Como funciona?

O ciclo básico do Terraform é assim:

1. **Escreve o código** com sua infra desejada.
2. Roda `terraform init` pra preparar o ambiente.
3. Roda `terraform plan` pra ver o que vai acontecer.
4. Roda `terraform apply` e… **tá tudo lá**.

Quer deletar tudo depois? `terraform destroy`. Prático, limpo, sem choradeira.

# Um exemplo simples

```hcl
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "5.95.0"
    }
  }
}

provider "aws" {
    region = "us-east-2"
    profile = "someProfile"
}

resource "aws_ecr_repository" "some-repo" {
  name                 = "some-repo"
  image_tag_mutability = "MUTABLE"

  image_scanning_configuration {
    scan_on_push = true
  }

  tags = {
    IAC = "True"
  }
}
```

Roda isso e pronto! **Você criou um repositório ECR** na AWS.

# Mas e se eu usar GCP, Azure, Cloudflare, GitHub, Datadog...?

Tanto faz. Terraform tem **provider pra tudo**. Se tiver API, o Terraform provavelmente consegue orquestrar. E se não tiver, tem um monte de maluco que já fez um provider custom.

Quer criar infraestrutura, gerenciar DNS, configurar repositório, automatizar monitoramento e ainda parecer um gênio? Terraform.

_Ah, mas onde eu acho esses providers?_

A documentação do Terraform é bem completa. Você pode procurar por `terraform registry` e vai achar tudo lá. Principalmente no [Terraform Registry](https://registry.terraform.io/).
Quando se trata de **AWS, GCP, Azure**, e outras nuvens, a documentação é bem rica e cheia de exemplos.

# Módulos são seus amigos

Não repita código. Faça módulo.

```hcl
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  name   = "minha-vpc"
  cidr   = "10.0.0.0/16"
}
```

Você escreve uma vez, usa pra sempre. Igual as mentiras no currículo, só que úteis.
E se você não sabe o que é um módulo, tá na hora de aprender. Módulos são como funções: você escreve uma vez e pode chamar várias vezes.

# E se der ruim?

O Terraform mantém um **state file** (estado da infra). Com ele, sabe exatamente o que existe e o que precisa mudar.  
Errou? Rola pra trás. Quer mudar algo? Ele calcula tudo antes e mostra o impacto.  
Melhor que ir no painel da AWS e *achar* que deletar o bucket não ia afetar nada.
E se você não tem certeza do que vai acontecer, o `terraform plan` mostra tudo antes de aplicar. É como um preview do que vai rolar. Além disso, se você utilizar o GitHub, você sempre pode manter versões do seu código e reverter caso algo dê errado.

# Se você ainda tá fazendo tudo na mão…

- Você vai esquecer.
- Vai errar permissão.
- Vai deixar recurso sem tag.
- Vai ficar sem saber o que foi criado.
- Vai ter pesadelo com configurações.

**Use Terraform.**  
Versão, documenta, revisa, replica e dorme tranquilo.

Se você leu isso e pensou “depois eu vejo isso aí com calma”, cuidado: seu próximo deploy pode ser o último antes da demissão.

# _Onde eu aprendo mais?_
Que orgulho meu jovem padawan! Finalmente fazendo as perguntas certas.
Para aprender mais sobre IAC e Terraform, inevitavelmente você vai precisar aprender mais sobre **Cloud Computing**, mas antes disso você **DEVE** aprender sobre **Linux** e **CLI**.

_Parece muito trabalho..._

Novamente voltando as perguntas idiotas! tsc, tsc tsc...
Sim, é muito trabalho, mas quem disse que ser dev é fácil?

Para aprender mais sobre IAC e Terraform, eu recomendo os seguintes links:
- [Terraform - Getting Started](https://learn.hashicorp.com/terraform?track=terraform-getting-started)
- [Terraform - AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Terraform em 10 Minutos // Dicionário do Programador](https://www.youtube.com/watch?v=0EAjJe8aPkc)