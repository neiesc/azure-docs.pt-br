---
title: Alinhamento de palavras – API de Tradução de Texto
titlesuffix: Azure Cognitive Services
description: Receber informações de alinhamento de palavras da API de Tradução de Texto.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: v-jansko
ms.custom: seodec18
ms.openlocfilehash: 74e26d4bb9b9c901524a85248dbf7f9d6ead5e95
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091468"
---
# <a name="how-to-receive-word-alignment-information"></a>Como receber informações de alinhamento de palavras

## <a name="receiving-word-alignment-information"></a>Receber informações de alinhamento de palavras
Para receber informações de alinhamento, use o método Traduzir e inclua o parâmetro opcional includeAlignment.

## <a name="alignment-information-format"></a>Formato de informações de alinhamento
O alinhamento é retornado como um valor de cadeia de caracteres do seguinte formato para cada palavra da fonte. As informações para cada palavra são separadas por um espaço, incluindo para idiomas não separados por espaço (scripts), como o chinês:

[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]] *

Exemplo de cadeia de caracteres de alinhamento: "0:0-7:10 1:2-11:20 3:4-0:3 3:4-4:6 5:5-21:21".

Em outras palavras, os dois-pontos separam o índice inicial e final, o traço separa os idiomas e o espaço separa as palavras. Uma palavra pode alinhar com zero, uma, ou várias palavras em outro idioma e, as palavras alinhadas podem ser não contíguas. Quando nenhuma informação de alinhamento estiver disponível, o elemento Alinhamento ficará vazio. Nesse caso, método não retornará nenhum erro.

## <a name="restrictions"></a>Restrições
O alinhamento somente será retornado para um subconjunto dos pares de idiomas neste ponto:
* do inglês para qualquer outro idioma;
* de qualquer outro idioma para o inglês, exceto chinês simplificado, chinês tradicional e letão para inglês
* de japonês para coreano ou de coreano para japonês e, se a sentença for uma tradução automática, você não receberá informações de alinhamento. Exemplo de uma tradução predefinida é "Isso é um teste", "eu te amo" e outras frases de alta frequência.

## <a name="example"></a>Exemplo

JSON de exemplo

```json
[
  {
    "translations": [
      {
        "text": "Kann ich morgen Ihr Auto fahren?",
        "to": "de",
        "alignment": {
          "proj": "0:2-0:3 4:4-5:7 6:10-25:30 12:15-16:18 17:19-20:23 21:28-9:14 29:29-31:31"
        }
      }
    ]
  }
]
```
