---
title: 'Tutorial: Recortar uma imagem no SDK da Pesquisa Visual do Bing'
description: Use o SDK da Pesquisa Visual do Bing para obter ideias de ares específicos em uma imagem.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: c3a06eef594ef3a7b2dda146ad76f648c07dc666
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742143"
---
# <a name="tutorial-crop-an-image-with-the-bing-visual-search-sdk-for-c"></a>Tutorial: Recortar uma imagem no SDK da Pesquisa Visual do Bing para C#

O SDK da Pesquisa Visual do Bing permite recortar uma imagem antes de localizar imagens online que são semelhantes. Esse aplicativo recorta uma única pessoa a partir de uma imagem que contém várias pessoas e, em seguida, retorna resultados que contêm imagens semelhantes encontradas online.

O código-fonte completo desse aplicativo está disponível com anotações e tratamento de erro adicionais no [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchCropImage.cs).

Esse tutorial ilustra como:

> [!div class="checklist"]
> * Enviar uma solicitação usando o SDK da Pesquisa Visual do Bing
> * Recortar uma área de imagem para pesquisar com a Pesquisa Visual do Bing
> * Receber e lidar com a resposta
> * Encontrar as URLs de itens de ação na resposta

## <a name="prerequisites"></a>Pré-requisitos

* Qualquer edição do [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* Se você estiver usando Linux/MacOS, este aplicativo poderá ser executado usando [Mono](http://www.mono-project.com/).
- O pacote [Pesquisa Personalizada do NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) instalado. 
    - No Gerenciador de Soluções no Visual Studio, clique com o botão direito do mouse no seu projeto e selecione `Manage NuGet Packages` no menu. Instale o pacote `Microsoft.Azure.CognitiveServices.Search.CustomSearch`. A instalação do pacote Pesquisa Personalizada do NuGet também instala os assemblies a seguir:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="specify-the-image-crop-area"></a>Especificar a área de imagem recortada

Este aplicativo corta uma área da imagem do Time de liderança sênior Microsoft. Essa área de corte é definida usando as coordenadas do canto superior esquerdo e inferior direito, representadas como uma porcentagem da imagem inteira.  

![Time de Liderança Sênior Microsoft](./media/MS_SrLeaders.jpg)

Essa imagem é cortada, criando um `ImageInfo`objeto da área de corte e carrega o `ImageInfo` objeto em um `VisualSearchRequest`. O objeto `ImageInfo` também inclui a URL de uma imagem.

```csharp
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg;
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```

## <a name="search-for-images-similar-to-the-crop-area"></a>Procure por imagens similares à área de corte

A variável `VisualSearchRequest` contém informações da área de corte da imagem e sua URL. O método `VisualSearchMethodAsync()` consegue os resultados.

```csharp
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>Pegue os dados de URL de ImageModuleAction

Resultados de Pesquisa Visual do Bing são objetos `ImageTag`.  Cada etiqueta contém uma lista de `ImageAction` objetos.  Cada `ImageAction` contém um campo `Data` que é uma lista de valores que dependem do tipo de ação.

Você consegue imprimir vários tipos com o seguinte código:

```csharp
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);
```

A aplicação completa retorna:


|ActionType  |URL  | |
|---------|---------|---------|
|PagesIncluding WebSearchURL     |         |         
|MoreSizes WebSearchURL     |         |         
|VisualSearch WebSearchURL    |         |         
|ImageById WebSearchURL     |         |         
|RelatedSearches WebSearchURL     |         |         
|Entidade -> WebSearchUrl     | https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5380.1        |         
|TopicResults -> WebSearchUrl    |  https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx,5382.1        |         
|ImageResults -> WebSearchUrl    |  https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5384.1        |         

Como mostrado acima, o ActionType `Entity` contém uma consulta de Pesquisa do Bing que retorna informação sobre uma pessoa reconhecida, lugar ou coisa.  Os tipos `TopicResults` e `ImageResults` contêm consultas para imagens relacionadas. As URLs na lista vinculam os resultados de pesquisa do Bing.


## <a name="get-urls-for-pagesincluding-actiontype-images"></a>Obter as URLs de imagens PagesIncluding ActionType

Obter as URLs de imagem real requer que uma conversão que lê um `ActionType` como `ImageModuleAction`, que contém um `Data` elemento com uma lista de valores.  Cada valor é a URL de uma imagem.  As conversões a seguir `PagesIncluding` tipo de ação para`ImageModuleAction` e lê os valores.

```csharp
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

## <a name="next-steps"></a>Próximas etapas
> [!div class="nextstepaction"]
> [Compilar um aplicativo Web de página única](tutorial-bing-visual-search-single-page-app.md)

[Resposta de Pesquisa Visual](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)