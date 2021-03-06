---
title: 'Tutorial: reconhecer emoções em um rosto em uma imagem – API de Detecção de Emoções, Python'
titlesuffix: Azure Cognitive Services
description: Use um Jupyter Notebook para saber como usar a API de Detecção de Emoções com Python. Visualize os resultados usando bibliotecas populares.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: tutorial
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 31e346cd9a3f43f8181ebee4474ae6c9ee2cc6fc
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237850"
---
# <a name="tutorial-use-the-emotion-api-with-a-jupyter-notebook--python"></a>Tutorial: use a API de Detecção de Emoções com um Jupyter Notebook e Python.

> [!IMPORTANT]
> A API de Detecção de Emoções será preterida em 15 de fevereiro de 2019. A funcionalidade de Detecção de Emoções agora está disponível como parte da [API de Detecção Facial](https://docs.microsoft.com/azure/cognitive-services/face/). 

Para facilitar a introdução à API de Detecção de Emoções, o Jupyter Notebook vinculado abaixo mostra como usar a API no Python e como visualizar os resultados usando algumas bibliotecas populares.

[Link para o bloco de anotações no GitHub](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### <a name="using-the-jupyter-notebook"></a>Usando o Bloco de Anotações do Jupyter

Para usar o bloco de anotações interativamente, você precisará cloná-lo e executá-lo no Jupyter. Para saber como começar a usar blocos de anotações interativos do Jupyter, siga as instruções descritas em http://jupyter.readthedocs.org/en/latest/install.html.

Para usar esse bloco de anotações, você precisará de uma chave de assinatura para a API de Detecção de Emoções. Visite a [página Assinatura](https://azure.microsoft.com/try/cognitive-services/) para se inscrever. Na página “Entre”, use sua conta Microsoft para entrar e você poderá assinar e obter chaves gratuitas. Depois de concluir o processo de inscrição, cole a chave na seção de variáveis mostrada abaixo. Tanto a chave primária quanto a secundária funcionam.

```
Python Example

#Variables

_url = 'https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```
