---
title: 'Início Rápido: baixar um relatório de entrada usando o portal do Azure | Microsoft Docs'
description: Saiba como baixar um relatório de entrada usando o portal do Azure
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 9131f208-1f90-4cc1-9c29-085cacd69317
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: ce242066df427163048a2ef51e79ffd98eadbc7d
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214905"
---
# <a name="quickstart-download-a-sign-in-report-using-the-azure-portal"></a>Início rápido: Baixar um relatório de entrada usando o portal do Azure

Neste início rápido, você aprenderá a baixar os dados de entrada para seu locatário nas últimas 24 horas.

## <a name="prerequisites"></a>Pré-requisitos

Você precisa de:

* Um locatário do Azure Active Directory, com uma licença Premium para exibir o relatório de atividade de entrada. Consulte [Introdução ao Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) para fazer upgrade da edição do Azure Active Directory. Observe que, se você não tiver dados de atividades antes da atualização, serão necessários alguns dias para que os dados sejam exibidos nos relatórios depois que você atualizar para uma licença premium.
* Um usuário, que está na função de **Administrador de Segurança**, **Leitor de Segurança**, **Leitor de Relatório** ou **Administrador Global** para o locatário. Além disso, qualquer usuário no locatário pode acessar suas próprias entradas.

## <a name="quickstart-download-a-sign-in-report"></a>Início Rápido: Baixar um relatório de entrada

1. Navegue até o [Portal do Azure](https://portal.azure.com).
2. Selecione **Azure Active Directory** do painel de navegação à esquerda e use o botão **Mudar diretório** para selecionar seu Active Directory.
3. No painel, selecione **Azure Active Directory** e, em seguida, selecione **Entradas**. 
4. Escolha **Últimas 24 horas** na lista suspensa do filtro **Data** e selecione **Aplicar** para exibir as entradas nas últimas 24 horas. 
5. Selecione o botão **Baixar** para baixar um arquivo CSV que contém os registros filtrados. 

![Relatórios](./media/quickstart-download-sign-in-report/download-sign-ins.png)

## <a name="next-steps"></a>Próximas etapas

* [Relatórios de atividades de entrada no portal do Azure Active Directory](concept-sign-ins.md)
* [Retenção de relatórios do Azure Active Directory](reference-reports-data-retention.md)
* [Latências de relatórios do Azure Active Directory](reference-reports-latencies.md)
