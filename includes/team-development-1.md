---
title: Arquivo de inclusão
description: Arquivo de inclusão
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: fb62468f2746072cea02185dbda022a1a5830fcf
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410435"
---
Até agora, você executou o código do aplicativo como se fosse o único desenvolvedor trabalhando no aplicativo. Nesta seção, você aprenderá como o Azure Dev Spaces simplifica o desenvolvimento em equipe:
* Habilite uma equipe de desenvolvedores para trabalhar no mesmo ambiente, trabalhando em um espaço de desenvolvimento compartilhado ou em espaços de desenvolvimento distintos, conforme o necessário.
* Dá suporte a cada desenvolvedor iterando em seu código em isolamento e sem o receio de interromper os outros.
* Testa o código de ponta a ponta, antes da confirmação, sem a necessidade de criar simulações ou simular dependências.

### <a name="challenges-with-developing-microservices"></a>Desafios com o desenvolvimento de microsserviços
O aplicativo de exemplo não é muito complexo no momento. Porém, no desenvolvimento do mundo real, os desafios logo surgem à medida que você adiciona mais serviços e a equipe de desenvolvimento cresce.

Imagine-se trabalhando em um serviço que interage com dezenas de outros serviços.

- A execução de tudo localmente para o desenvolvimento pode se tornar irrealista. Seu computador de desenvolvimento pode não ter recursos suficientes para executar o aplicativo inteiro. Ou talvez o aplicativo tenha pontos de extremidade que precisam ser acessíveis publicamente (por exemplo, o aplicativo responde a um webhook por meio de um aplicativo SaaS).

- Você pode tentar executar somente os serviços dos quais depende, mas isso significa que você precisará saber o fechamento completo de dependências (por exemplo, as dependências de dependências). Ou é uma questão de não saber com facilidade como criar e executar as dependências porque você não trabalhou nelas.
- Alguns desenvolvedores recorrem à simulação de muitas de suas dependências de serviço. Essa abordagem pode ser útil às vezes, mas o gerenciamento dessas simulações logo pode competir com seu próprio esforço de desenvolvimento. Além disso, essa abordagem leva a grandes diferenças entre o espaço de desenvolvimento e o de produção, e bugs sutis podem afetá-lo.
- Entende-se que fazer qualquer tipo de teste de ponta a ponta se torna difícil. Os testes de integração só podem ocorrer de maneira realista após a confirmação, o que significa que você verá problemas mais tarde no ciclo de desenvolvimento.

![](../articles/dev-spaces/media/common/microservices-challenges.png)


### <a name="work-in-a-shared-dev-space"></a>Trabalhar em um espaço de desenvolvimento compartilhado
Com o Azure Dev Spaces, você pode configurar um espaço de desenvolvimento *compartilhado* no Azure. Cada desenvolvedor pode se concentrar exatamente em sua parte do aplicativo e pode desenvolver iterativamente um *código de pré-confirmação* em um espaço que já contém todos os outros serviços e recursos de nuvem dos quais seus cenários dependem. As dependências estão sempre atualizadas, e os desenvolvedores trabalham de uma forma que reflete a produção.

### <a name="work-in-your-own-space"></a>Trabalhar em seu próprio espaço
À medida que você desenvolve o código para seu serviço, e antes de estar pronto para fazer check-in nele, em geral, o código não estará em um bom estado. Você ainda o está moldando iterativamente, testando e experimentando soluções. O Azure Dev Spaces apresenta o conceito de um **espaço**, permitindo que você trabalhe em isolamento e sem o receio de interromper os membros de sua equipe.

> [!Note]
> Antes de continuar, feche todas as janelas do VS Code de ambos os serviços e, em seguida, execute `azds up -d` em cada uma das pastas raiz do serviço. (Essa é uma limitação da Versão Prévia.)

Vamos examinar mais detalhadamente em que local os serviços estão em execução no momento. Execute o comando `azds list-up` e você verá uma saída semelhante à seguinte:

```
Name                          DevSpace  Type     Updated  Status
----------------------------  --------  -------  -------  -------
mywebapi                      default   Service  3m ago   Running
mywebapi-56c8f45d9-zs4mw      default   Pod      3m ago   Running
webfrontend                   default   Service  1m ago   Running
webfrontend-6b6ddbb98f-fgvnc  default   Pod      1m ago   Running
```

A coluna DevSpace mostra que ambos os serviços estão em execução em um espaço chamado `default`. Qualquer pessoa que abre a URL pública e navega para o aplicativo Web invocará o caminho do código escrito anteriormente que percorre ambos os serviços. Agora suponha que você deseje continuar desenvolvendo `mywebapi`. Como fazer alterações de código e testá-las e não interromper os outros desenvolvedores que estão usando o ambiente de desenvolvimento? Para fazer isso, você configurará seu próprio espaço.

### <a name="create-a-dev-space"></a>Criar um espaço de desenvolvimento
Para executar sua própria versão de `mywebapi` em um espaço que não seja `default`, crie seu próprio espaço usando o seguinte comando:

``` 
azds space select --name scott
```

Quando solicitado, selecione `default` como o **espaço de desenvolvimento principal**. Isso significa que o nosso novo espaço, `default/scott`, será derivado do espaço `default`. Em breve, veremos como isso nos ajudará nos testes. 

No exemplo acima, usei meu nome para o novo espaço, de modo que meus colegas o identifiquem como o espaço em que estou trabalhando, mas você pode chamá-lo como desejar e ser flexível sobre o que ele significa, como 'sprint4' ou 'demo'.

Execute o comando `azds space list` para ver uma lista de todos os espaços no ambiente de desenvolvimento. Um asterisco (*) é exibido ao lado do espaço atualmente selecionado. Em seu caso, o espaço chamado 'default/scott' foi selecionado automaticamente quando foi criado. Você pode selecionar outro espaço a qualquer momento com o comando `azds space select`.
