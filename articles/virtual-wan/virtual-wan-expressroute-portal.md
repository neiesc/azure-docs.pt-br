---
title: Usar a WAN Virtual do Azure para criar conexões do ExpressRoute para ambientes locais e do Azure | Microsoft Docs
description: Neste tutorial, saiba como usar a WAN Virtual do Azure para criar conexões do ExpressRoute para ambientes do Azure e locais.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 10/5/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my corporoate on-premises network(s) to my VNets using Virtual WAN and ExpressRoute.
ms.openlocfilehash: c02020ba8d49b123cf8914214d52ac40896a3c20
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248173"
---
# <a name="tutorial-create-an-expressroute-association-using-azure-virtual-wan-preview"></a>Tutorial: Criar uma associação do ExpressRoute usando a WAN Virtual do Azure (versão prévia)

Este tutorial mostra como usar a WAN Virtual para conectar-se aos seus recursos no Azure com o uso de um circuito e uma associação do ExpressRoute. Para obter mais informações sobre a WAN Virtual, consulte a [Visão Geral de WAN Virtual](virtual-wan-about.md)

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar uma vWAN
> * Criar um hub
> * Localizar e associar um circuito ao hub
> * Associar o circuito a hubs
> * Conectar uma VNET a um hub
> * Exibir a WAN virtual
> * Exibir a integridade dos recursos
> * Monitorar uma conexão

> [!IMPORTANT]
> Essa versão prévia pública é fornecida sem um SLA e não deve ser usada para cargas de trabalho de produção. Determinados recursos podem não ter suporte, podem ter restrição ou podem não estar disponíveis em todos os locais do Azure. Veja os [Termos de Uso Adicionais para Visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter detalhes.
>

## <a name="before-you-begin"></a>Antes de começar

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="register"></a>Registrar este recurso

Antes de configurar a WAN Virtual, você precisa inscrever sua assinatura na versão prévia. Caso contrário, você não poderá trabalhar com a WAN Virtual no portal. Para se inscrever, envie um email para **azurevirtualwan@microsoft.com** com sua ID de assinatura. Após a inscrição da sua assinatura, você receberá um email.

**Considerações sobre a versão prévia:**

* Disponibilidade de região: Centro-oeste dos EUA
* O Circuito de ExpressRoute deve estar habilitado em um país que dá suporte ao [Alcance Global do ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-faqs#where-is-expressroute-global-reach-supported)

## <a name="vnet"></a>1. Criar uma rede virtual

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="openvwan"></a>2. Criar uma WAN virtual

Em um navegador, navegue para o [portal do Azure (versão prévia)](https://aka.ms/azurevirtualwanpreviewfeatures) e entre com sua conta do Azure.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

### <a name="getting-started-page"></a>Página Introdução

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-gettingstarted-include.md)]

## <a name="hub"></a>3. Criar um hub

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="hub"></a>4. Localizar e associar um circuito ao hub

1. Selecione sua vWAN e, em **Arquitetura da WAN Virtual**, selecione **Circuitos do ExpressRoute**
2. Se o circuito do ExpressRoute na mesma assinatura do que sua vWAN, clique em **Selecionar circuito do ExpressRoute** da(s) sua(s) assinatura(s) 
3. Usando a lista suspensa, selecione o ExpressRoute que você gostaria de associar ao hub.
4. Se o circuito da ExpressRoute não estiver na mesma assinatura ou se você tiver recebido [uma chave de autorização e uma ID de emparelhamento](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md), selecione **Encontrar um circuito ao resgatar uma chave de autorização**
5. Insira os seguintes detalhes:
* **Chave de autorização**: gerada pelo proprietário do circuito, conforme descrito acima
* **URI do circuito de pares**: o URI do circuito que é fornecido pelo proprietário do circuito e é o identificador exclusivo para o circuito
* **Peso de roteamento** - [Peso de Roteamento](../expressroute/expressroute-optimize-routing.md) permite que você prefira determinados caminhos quando vários circuitos de localizações de emparelhamento diferentes estiverem conectados ao mesmo hub
6. Clique em **Localizar circuito** e selecione o circuito, se encontrado
7. Selecione um ou mais hubs na lista suspensa e clique em **Salvar**

## <a name="vnet"></a>5. Conectar sua VNET a um hub

Nesta etapa, você pode criar a conexão de emparelhamento entre uma VNET e seu hub. Repita as etapas para cada VNET que você deseja se conectar.

1. Na página da WAN virtual, clique em **Conexão de rede virtual**.
2. Na página de conexão de rede virtual, clique em **+Adicionar conexão**.
3. Na página **Adicionar conexão**, preencha os seguintes campos:

    * **Nome da Conexão**: nomeie sua conexão.
    * **Hubs**: selecione o hub que você deseja associar a essa conexão.
    * **Assinatura**: verifique a assinatura.
    * **Rede virtual:** selecione a rede virtual que você deseja conectar a esse hub. A rede virtual não pode ter um gateway de rede virtual já existente.


## <a name="viewwan"></a>6. Exibir a WAN virtual

1. Navegue até a WAN virtual.
2. Na página de visão geral, cada ponto do mapa representa um hub. Passe o mouse sobre qualquer ponto para exibir o resumo de integridade do hub.
3. Na seção Hubs e conexões, você pode exibir status do hub, site, região, status de conexão de VPN e bytes de entrada e saída.

## <a name="viewhealth"></a>7. Exibir a integridade do recurso

1. Navegue até a WAN.
2. Na página de sua WAN, na seção **SUPORTE + Solução de problemas**, clique em **Integridade** e exiba seu recurso.

## <a name="connectmon"></a>8. Monitorar uma conexão

Crie uma conexão para monitorar a comunicação entre uma VM do Azure e um site remoto. Para obter informações sobre como configurar um monitor de conexão, consulte [Monitorar a comunicação de rede](~/articles/network-watcher/connection-monitor.md). O campo de origem é o IP da VM no Azure e o IP de destino é o IP do site.

## <a name="cleanup"></a>9. Limpar recursos

Quando não precisar mais desses recursos, você poderá utilizar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos que ele contém. Substitua "myResourceGroup" pelo nome do grupo de recursos e execute o seguinte comando do PowerShell:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Criar uma vWAN
> * Criar um hub
> * Localizar e associar um circuito ao hub
> * Associar o circuito a hubs
> * Conectar uma VNET a um hub
> * Exibir a WAN virtual
> * Exibir a integridade dos recursos
> * Monitorar uma conexão

Para saber mais sobre a WAN Virtual, consulte a página [Visão geral de WAN Virtual](virtual-wan-about.md).
