---
title: Atualizar o serviço do QnA Maker – QnA Maker
titleSuffix: Azure Cognitive Services
description: Você pode optar por fazer upgrade dos componentes individuais da pilha QnA Maker após a criação inicial.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 5e5392f04b7894f0fe39a940865035d967c2d19a
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53629931"
---
# <a name="upgrade-your-qna-maker-service"></a>Atualizar seu serviço QnA Maker
Você pode optar por fazer upgrade dos componentes individuais da pilha QnA Maker após a criação inicial. Consulte os detalhes dos componentes dependentes e da seleção de SKU [aqui](https://aka.ms/qnamaker-docs-capacity).

## <a name="upgrade-qna-maker-management-sku"></a>Fazer upgrade da SKU de Gerenciamento do QnA Maker

Quando você precisar ter mais perguntas e respostas em sua base de dados de conhecimento, além de sua camada atual, atualize o seu serviço QnA Maker tipo de preço. 

Para fazer upgrade da SKU de gerenciamento do QnA Maker:

1. Vá para o recurso QnA Maker no portal do Azure e selecione **Camada de preços**.

    ![Recurso do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

2. Escolha o SKU apropriado e pressione **Selecionar**.

    ![Preços do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>Fazer upgrade do serviço Aplicativo

 Quando sua base de dados de conhecimento precisar fornecer mais solicitações do aplicativo cliente, atualize o tipo de preço do serviço de aplicativo.

Você pode [escalar verticalmente](https://docs.microsoft.com/azure/app-service/web-sites-scale) ou reduzir verticalmente o serviço Aplicativo.

1. Vá para o recurso de serviço Aplicativo no portal do Azure e selecione as opções **escalar verticalmente** ou **reduzir verticalmente** conforme necessário.

    ![Escala do serviço de aplicativo do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-azure-search-service"></a>Fazer upgrade do serviço Azure Search

Ao planejar ter muitas bases de conhecimento, atualize o tipo de preço do serviço do Azure Search. 

Atualmente, não é possível executar um local na SKU de pesquisa de atualização do Azure. No entanto, pode criar um novo recurso de pesquisa do Azure com o SKU desejado, restaurar os dados para o novo recurso e vinculá-lo com a pilha do QnA Maker.

1. Crie um novo recurso de pesquisa do Azure no portal do Azure e escolha a SKU desejada.

    ![Recurso de pesquisa do Azure do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

2. Restaure os índices de seu recurso original da pesquisa do Azure para o novo. Consulte o código de exemplo de backup e restauração [aqui](https://github.com/pchoudhari/QnAMakerBackupRestore).

3. Uma vez que os dados sejam restaurados, vá para o novo recurso de pesquisa do Azure, selecione **Chaves**e anote o **Nome** e a **chave Admin**.

    ![Chaves de pesquisa do QnA Maker Azure](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

4. Para vincular o novo recurso de pesquisa do Azure para a pilha do QnA Maker, vá para o serviço de Aplicativo QnA Maker.

    ![Serviço de aplicativo QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

5. Selecione **Configurações do aplicativo** e substitua os campos **AzureSearchName** e **AzureSearchAdminKey** da etapa 3.

    ![Configuração de serviço de aplicativo QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

6. Reinicie o serviço de Aplicativo.

    ![Reinício do serviço de aplicativo QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Usar a API de QnA Maker](../Quickstarts/csharp.md)
