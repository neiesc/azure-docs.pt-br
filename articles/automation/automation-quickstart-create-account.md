---
title: Guia de Início Rápido do Azure – Criar uma conta de Automação do Azure | Microsoft Docs
description: Como criar uma conta de Automação do Azure e executar um runbook
services: automation
author: csand-msft
ms.author: csand
ms.date: 01/15/2019
ms.topic: quickstart
ms.service: automation
ms.component: process-automation
ms.custom: mvc
ms.openlocfilehash: eea14dc781d2282bcd311b307c41270320007c39
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54319709"
---
# <a name="create-an-azure-automation-account"></a>Criar uma conta de Automação do Azure

As contas de Automação do Azure podem ser criadas por meio do Azure. Esse método fornece uma interface do usuário baseada em navegador para a criação e configuração de contas de Automação e recursos relacionados. Este guia de início rápido segue as etapas para criação de uma conta de Automação e para execução de um runbook na conta.

Caso você não tenha uma assinatura do Azure, crie uma [conta gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="sign-in-to-azure"></a>Entrar no Azure

Entre no Azure em https://portal.azure.com

## <a name="create-automation-account"></a>Criar uma conta de Automação

1. Clique no botão **Criar um recurso**, localizado no canto superior esquerdo do Azure.

1. Selecione **Ferramentas de Gerenciamento** e selecione **Automação**.

1. Insira as informações da conta. Para **Criar conta Executar como do Azure**, escolha **Sim** para que os artefatos que simplificam a autenticação do Azure sejam habilitados automaticamente. É importante observar que, ao criar uma Conta de Automação, o nome não pode ser alterado após a escolha. *Os nomes de Conta de Automação são exclusivos por região e grupo de recursos. Os nomes de Contas da Automação que foram excluídos podem não estar disponíveis imediatamente.* Uma Conta de Automação pode gerenciar recursos em todas as regiões e assinaturas para determinado locatário. Ao concluir, clique em **Criar** para iniciar a implantação da conta de Automação.

    ![Inserir informações sobre a conta de Automação na página](./media/automation-quickstart-create-account/create-automation-account-portal-blade.png)  

    > [!NOTE]
    > Para obter uma lista atualizada dos locais onde você pode implantar uma Conta de Automação, veja [Produtos disponíveis por região](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=automation&regions=all).

1. Após a conclusão da implantação, clique em ** **Todos os Serviços**, selecione **Contas de Automação** e selecione a conta de automação que você criou.

    ![Visão geral da conta de Automação](./media/automation-quickstart-create-account/automation-account-overview.png)

## <a name="run-a-runbook"></a>Executar um runbook

Execute um dos runbooks tutoriais.

1. Clique em **Runbooks** em **AUTOMAÇÃO DE PROCESSO**. A lista de runbooks é exibida. Por padrão, vários runbooks tutoriais são habilitados na conta.

    ![Lista de runbooks da conta de Automação](./media/automation-quickstart-create-account/automation-runbooks-overview.png)

1. Selecione o runbook **AzureAutomationTutorialScript**. Essa ação abre a página de visão geral do runbook.

    ![Visão geral do runbook](./media/automation-quickstart-create-account/automation-tutorial-script-runbook-overview.png)

1. Clique em **Iniciar** e, na página **Iniciar Runbook**, clique em **OK** para iniciar o runbook.

    ![Página do trabalho de runbook](./media/automation-quickstart-create-account/automation-tutorial-script-job.png)

1. Após o **Status do trabalho** se tornar **Executando**, clique em **Saída** ou **Todos os Logs** para exibir a saída do trabalho de runbook. Para este runbook tutorial, a saída é uma lista dos seus recursos do Azure.

## <a name="clean-up-resources"></a>Limpar recursos

Quando não for mais necessário, exclua o grupo de recursos, a conta de Automação e todos os recursos relacionados. Para fazer isso, selecione o grupo de recursos da conta de Automação e clique em **Excluir**.

## <a name="next-steps"></a>Próximas etapas

Neste guia de início rápido você implantou uma conta de Automação, iniciou um trabalho de runbook e exibiu os resultados do trabalho. Para saber mais sobre a Automação do Azure, continue para o início rápido para criar seu primeiro runbook.

> [!div class="nextstepaction"]
> [Guia de Início Rápido de Automação – Criar Runbook](./automation-quickstart-create-runbook.md)
