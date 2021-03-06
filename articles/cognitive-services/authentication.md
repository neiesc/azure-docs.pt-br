---
title: Autenticação
titleSuffix: Cognitive Services - Azure
description: 'Há três maneiras de autenticar uma solicitação para um recurso dos Serviços Cognitivos do Azure: uma chave de assinatura, um token de portador ou uma assinatura de vários serviços. Neste artigo, você saberá mais sobre cada método e como fazer uma solicitação.'
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.openlocfilehash: 11259b99ea9d2486c8c0afde21398710ccc6ccd8
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53726283"
---
# <a name="authenticate-requests-to-azure-cognitive-services"></a>Autenticar solicitações para os Serviços Cognitivos do Azure

Cada solicitação para um Serviço Cognitivo do Azure deve incluir um cabeçalho de autenticação. Esse cabeçalho passa uma chave de assinatura ou token de acesso, que é usado para validar sua assinatura em um serviço ou grupo de serviços. Neste artigo, você aprenderá três maneiras de autenticar uma solicitação e os requisitos para cada uma.

* [Autenticar com uma chave de assinatura para um único serviço](#authenticate-with-a-single-service-subscription-key)
* [Autenticar com uma chave de assinatura para vários serviços](#authenticate-with-a-multi-service-subscription-key)
* [Autenticar com um token](#authenticate-with-an-authentication-token)

## <a name="prerequisites"></a>Pré-requisitos

Antes de fazer uma solicitação, você precisará de uma conta do Azure e uma assinatura dos Serviços Cognitivos do Azure. Se você já tiver uma conta, pule para a próxima seção. Se você não tiver uma conta, temos um guia para ajudá-lo a configurá-la em minutos: [Criar uma conta dos Serviços Cognitivos para o Azure](cognitive-services-apis-create-account.md).

## <a name="authentication-headers"></a>Cabeçalhos de autenticação

Vamos analisar rapidamente os cabeçalhos de autenticação disponíveis para uso com os Serviços Cognitivos do Azure.

| Cabeçalho | DESCRIÇÃO |
|--------|-------------|
| Ocp-Apim-Subscription-Key | Use esse cabeçalho para autenticar com uma chave de assinatura para um serviço específico ou uma chave de assinatura para vários serviços. |
| Ocp-Apim-Subscription-Region | Esse cabeçalho só é necessário ao usar uma chave de assinatura para vários serviços com a [API de Tradução de Texto](./Translator/reference/v3-0-reference.md). Use esse cabeçalho para especificar a região da assinatura. |
| Autorização | Use esse cabeçalho se você está usando um token de autenticação. As etapas para executar uma troca de tokens estão detalhadas nas seções a seguir. O valor fornecido segue este formato: `Bearer <TOKEN>`. |

## <a name="authenticate-with-a-single-service-subscription-key"></a>Autenticar com uma chave de assinatura para um único serviço

A primeira opção é autenticar uma solicitação com uma chave de assinatura para um serviço específico, como a Tradução de Texto. As chaves estão disponíveis no portal do Azure para cada recurso que você criou. Para usar uma chave de assinatura e autenticar uma solicitação, ela deve ser transmitida como o cabeçalho `Ocp-Apim-Subscription-Key`.

Essas solicitações de exemplo demonstram como usar o cabeçalho `Ocp-Apim-Subscription-Key`. Tenha em mente, ao usar este exemplo, que você precisará incluir uma chave de assinatura válida.

Isso é um exemplo de chamada à API de Pesquisa na Web do Bing:
```cURL
curl -X GET 'https://api.cognitive.microsoft.com/bing/v7.0/search?q=Welsch%20Pembroke%20Corgis' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' | json_pp
```

Essa é uma chamada de exemplo para a API de Tradução de Texto:
```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

## <a name="authenticate-with-a-multi-service-subscription-key"></a>Autenticar com uma chave de assinatura para vários serviços

>[!WARNING]
> No momento, estes serviços **não** têm suporte para chaves de vários serviços: QnA Maker, Serviços de Fala e Visão Personalizada.

Essa opção também usa uma chave de assinatura para autenticar solicitações. A principal diferença é que uma chave de assinatura não está vinculada a um serviço específico; na verdade, uma mesma chave pode ser usada para autenticar solicitações para vários Serviços Cognitivos. Confira [Preço dos Serviços Cognitivos](https://azure.microsoft.com/pricing/details/cognitive-services/) para obter informações sobre disponibilidade regional, recursos com suporte e preços.

A chave de assinatura é fornecida em cada solicitação como o cabeçalho `Ocp-Apim-Subscription-Key`.

### <a name="supported-regions"></a>Regiões com suporte

Quando usar a chave de assinatura de vários serviços a fim de fazer uma solicitação para `api.cognitive.microsoft.com`, você deve incluir a região na URL. Por exemplo: `westus.api.cognitive.microsoft.com`.

Ao usar a chave de assinatura para vários serviços com a API de Tradução de Texto, você deve especificar a região de assinatura com o cabeçalho `Ocp-Apim-Subscription-Region`.

A autenticação de vários serviços tem suporte nestas regiões:

| | | |
|-|-|-|
| `australiaeast` | `brazilsouth` | `canadacentral` |
| `centralindia` | `eastasia` | `eastus` |
| `japaneast` | `northeurope` | `southcentralus` |
| `southeastasia` | `uksouth` | `westcentralus` |
| `westeurope` | `westus` | `westus2` |


### <a name="sample-requests"></a>Solicitações de exemplo

Isso é um exemplo de chamada à API de Pesquisa na Web do Bing:

```cURL
curl -X GET 'https://YOUR-REGION.api.cognitive.microsoft.com/bing/v7.0/search?q=Welsch%20Pembroke%20Corgis' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' | json_pp
```

Essa é uma chamada de exemplo para a API de Tradução de Texto:

```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' \
-H 'Ocp-Apim-Subscription-Region: YOUR_SUBSCRIPTION_REGION' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

## <a name="authenticate-with-an-authentication-token"></a>Autenticar com um token de autenticação

Alguns Serviços Cognitivos do Azure aceitam (e, em alguns casos, exigem) um token de autenticação. Atualmente, estes serviços dão suporte a tokens de autenticação:

* API de Tradução de Texto
* Serviços de Fala: API REST de conversão de fala em texto
* Serviços de Fala: API REST conversão de texto em fala

>[!NOTE]
> O QnA Maker também usa o cabeçalho Authorization, mas requer uma chave de ponto de extremidade. Para obter mais informações, confira [QnA Maker: obter resposta da base de dados de conhecimento](./qnamaker/quickstarts/get-answer-from-kb-using-curl.md).

>[!WARNING]
> Os serviços que dão suporte a tokens de autenticação podem mudar ao longo do tempo; confira a referência de API do serviço antes de usar esse método de autenticação.

Tanto as chaves de assinatura para um único serviço quanto a para vários serviços podem ser trocadas por tokens de autenticação. Os tokens de autenticação são válidos por 10 minutos.

Os tokens de autenticação são incluídos em uma solicitação como o cabeçalho `Authorization`. O valor do token fornecido deve ser precedido por `Bearer`, por exemplo, `Bearer YOUR_AUTH_TOKEN`.

### <a name="sample-requests"></a>Solicitações de exemplo

Use esta URL para trocar uma chave de assinatura para um único serviço por um token de autenticação: `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`.

```cURL
curl -v -X POST \
"https://api.cognitive.microsoft.com/sts/v1.0/issueToken" \
-H "Content-type: application/x-www-form-urlencoded" \
-H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

Ao usar uma chave de assinatura para vários serviços, você deve usar um ponto de extremidade específico da região para a troca de tokens. Use esta URL para trocar uma chave de assinatura para vários serviços por um token de autenticação: `https://YOUR-REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken`.

Estas regiões de vários serviços dão suporte à troca de tokens:

| | | |
|-|-|-|
| `australiaeast` | `brazilsouth` | `canadacentral` |
| `centralindia` | `eastasia` | `eastus` |
| `japaneast` | `northeurope` | `southcentralus` |
| `southeastasia` | `uksouth` | `westcentralus` |
| `westeurope` | `westus` | `westus2` |

```cURL
curl -v -X POST \
"https://YOUR-REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" \
-H "Content-type: application/x-www-form-urlencoded" \
-H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

Depois de obter um token de autenticação, você precisará transmiti-lo em cada solicitação como o cabeçalho `Authorization`. Essa é uma chamada de exemplo para a API de Tradução de Texto:

```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Authorization: Bearer YOUR_AUTH_TOKEN' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

## <a name="see-also"></a>Consulte também

* [O que são Serviços Cognitivos?](welcome.md)
* [Preço dos Serviços Cognitivos](https://azure.microsoft.com/pricing/details/cognitive-services/)
* [Criar uma conta](cognitive-services-apis-create-account.md)
