---
title: Instalar o cliente do Chef no portal do Azure
description: Saiba como implantar e configurar o cliente do Chef no portal do Azure
keywords: azure, chef, devops, cliente, instalar, portal
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: e5de21e8e493871150d302f1d2c0e31d067affde
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54051321"
---
# <a name="install-the-chef-client-from-the-azure-portal"></a>Instalar o cliente do Chef no portal do Azure
Ao criar ou modificar uma máquina virtual Linux ou Windows do portal do Azure, adicione a extensão do Chef para a máquina virtual. Este artigo o orienta no processo usando uma nova máquina virtual Linux.

## <a name="prerequisites"></a>Pré-requisitos
- **Assinatura do Azure**: Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

- **Chef**: se você não tem uma conta ativa do Chef, inscreva-se para uma [avaliação gratuita do Hosted Chef](https://manage.chef.io/signup). Para acompanhar as instruções neste artigo, você precisa dos seguintes valores de sua conta do Chef: 
    - chave organization_validation
    - rb
    - run_list

## <a name="install-the-chef-extension-on-a-new-linux-virtual-machine"></a>Instale a extensão do Chef em uma nova máquina virtual Linux
Nesta seção, use primeiro o Portal do Azure para criar uma máquina Linux. Durante o processo, você também aprende a instalar a extensão do Chef na nova máquina virtual.

1. Navegue até o [Portal do Azure](http://portal.azure.com).

1. No menu à esquerda, selecione a opção **Máquinas virtuais**. Se a opção **Máquinas Virtuais** não estive presente, selecione **Todos os serviços** e, em seguida, selecione **Máquinas virtuais**.

1. Na guia **Máquinas virtuais** guia, selecione **Adicionar**.

    ![Adicione uma nova máquina virtual usando o portal do Azure](./media/chef-extension-portal/add-vm.png)

1. Na guia **Computação**, selecione o sistema operacional desejado. Para esta demonstração, o **Ubuntu Server** está selecionado.

1. Na guia **Ubuntu Server**, selecione **Ubuntu Server 16.04 LTS**.

    ![Ao criar uma máquina virtual de Ubuntu, especifique a versão necessária](./media/chef-extension-portal/ubuntu-server-version.png)

1. Na guia **Ubuntu Server 16.04 LTS**, selecione **Criar**.

    ![O Ubuntu fornece informações adicionais sobre o produto](./media/chef-extension-portal/create-vm.png)

1. Na guia **Criar a máquina virtual**, selecione **Básico**.

1. Na guia **Básico**, especifique os valores a seguir, e em seguida, selecione **OK**.

    - **Nome** - Insira um nome para a nova máquina virtual.
    - **Tipo de disco VM** - especifique o **SSD** ou **HDD** para o tipo de disco de armazenamento. Para obter mais informações sobre tipos de disco de máquina virtual no Azure, consulte o artigo [Armazenamento Premium de Alto Desempenho e discos gerenciados para VMs](/azure/virtual-machines/windows/premium-storage).
    - **Nome de usuário**: insira um nome de usuário para receber privilégios de administrador na máquina virtual.
    - **Tipo de Autenticação**, Selecione **Senha**. Você também pode selecionar **chave pública SSH**e forneça um valor de chave pública SSH. Para fins desta demonstração (e nas capturas de tela), **Senha** é selecionada.
    - **Senha** e **Confirmar senha** - digite uma senha para o usuário.
    - **Logon com o Microsoft Azure Active Directory**  - Selecione **Desabilitado**.
    - **Assinatura** – selecione a assinatura do Azure desejada, se você tiver mais de uma.
    - **Grupo de recursos** - Insira um nome para seu grupo de recursos.
    - **Local** - Selecione **Leste dos EUA**.

    ![Guia básico para a criação de uma máquina virtual](./media/chef-extension-portal/add-vm-basics.png)

1. Na guia **Escolher um tamanho**, selecione o tamanho para a máquina virtual, em seguida, selecione **Selecionar**.

1. Na guia **Configurações**, a maioria dos valores deve ser preenchida com base nos valores selecionados nas guias anteriores. Selecione **Extensões**.

    ![As extensões são adicionadas às máquinas virtuais por meio da guia Configurações](./media/chef-extension-portal/add-vm-select-extensions.png)

1. Na guia **Extensões**, selecione **Adicionar extensão**.

    ![Selecione Adicionar extensão para adicionar uma extensão para uma máquina virtual](./media/chef-extension-portal/add-vm-add-extension.png)

1. Na guia **Novo recurso** guia, selecione **extensão Chef do Linux (1.2.3)**.

    ![O Chef tem extensões para as máquinas virtuais Linux e Windows](./media/chef-extension-portal/select-linux-chef-extension.png)

1. Na guia **Extensão Chef Linux**, selecione **Criar**.

1. Na guia **Instalar extensão**, especifique os valores a seguir, e em seguida, selecione **OK**.

    - **URL de servidor do Chef** – insira a URL de servidor do Chef que inclui o nome da organização, por exemplo, *https://api.chef.io/organization/hessco*.
    - **Nome do nó Chef** -insira o nome do nó do Chef. Isso pode ser qualquer valor.
    - **Lista de execução** - insira a lista de execução do Chef que é adicionado à máquina. Isso pode ser deixado em branco.
    - **Nome do cliente de validação** - insira o nome do cliente de validação do Chef. por exemplo, *tarcher-validator*.
    - **Chave de validação** - selecione um arquivo que contém a chave de validação usada ao inicializar suas máquinas. 
    - **Arquivo de configuração do cliente** - selecione um arquivo de configuração de cliente do chef. Isso pode ser deixado em branco.
    - **Versão de cliente do Chef** - insira a versão do cliente para instalar o chef. Isso pode ser deixado em branco. Um valor em branco resulta na instalação da versão mais recente. 
    - **Modo de verificação de SSL** - selecione **Nenhum** ou **Par**. *Nenhum* foi selecionado para a demonstração.
    - **Ambiente do Chef** - inserir o ambiente do Chef neste nó deve ser um membro do. Isso pode ser deixado em branco.
    - **Segredo do Databag Criptografado** - selecione um arquivo que contém o segredo para o Databag criptografado neste computador ter acesso. Isso pode ser deixado em branco.
    - **Certificado SSL de servidor do Chef** - selecione o certificado SSL atribuído ao seu servidor do Chef. Isso pode ser deixado em branco.

    ![Instalando o servidor do Chef em uma máquina virtual Linux](./media/chef-extension-portal/install-extension.png)

1. Ao retornar para a guia **Extensões**, selecione **OK**.

1. Ao retornar para a guia **Configurações**, selecione **OK**.

1. Ao retornar para a guia **Criar** (que representa um resumo das opções selecionadas e inseridas), verifique as informações, bem como os **Termos de uso**, e selecione **Criar**.

Quando o processo de criar e implantar a máquina virtual com a extensão do Chef estiver concluído, uma notificação indica o êxito ou falha da operação. Além disso, a página do recurso para a nova máquina virtual é aberta automaticamente no portal do Azure depois que é criada.

![Instalando o servidor do Chef em uma máquina virtual Linux](./media/chef-extension-portal/resource-created.png)

## <a name="next-steps"></a>Próximas etapas
* [Criar uma Máquina Virtual do Windows no Azure usando o Chef](/azure/virtual-machines/windows/chef-automation)
