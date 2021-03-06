---
title: Problemas conhecidos do Azure Monitor para VMs (versão prévia) | Microsoft Docs
description: Este artigo aborda os problemas conhecidos com o Azure Monitor para VMs, uma solução do Azure que combina monitoramento da integridade, da descoberta de dependência do aplicativo e de desempenho do sistema operacional da VM do Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/09/2018
ms.author: magoedte
ms.openlocfilehash: 038c6afe94ccfea707eea3b4032a2e45f69e5102
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54187067"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>Problemas conhecidos com o Azure Monitor para VMs (versão prévia)

Este artigo aborda os problemas conhecidos com o Azure Monitor para VMs, uma solução do Azure que combina monitoramento da integridade, da descoberta de componentes de aplicativo e de desempenho do sistema operacional da VM do Azure. 

## <a name="health"></a>Integridade 
Os seguintes são problemas conhecidos com a versão atual do recurso de Integridade:

- Se uma VM do Azure for removida ou excluída, ela será mostrada na exibição de lista da VM durante algum tempo. Além disso, ao clicar no estado de uma VM removida ou excluída, a exibição **Diagnóstico de Integridade** será aberta e, em seguida, um loop de carregamento será iniciado. A seleção do nome da VM excluída abre um painel com uma mensagem informando que a VM foi excluída.
- As alterações de configuração, como a atualização de um limite, levam até 30 minutos, mesmo se o portal ou a API do Monitor da Carga de Trabalho atualizá-las imediatamente. 
- A experiência de Diagnóstico de Integridade é atualizada mais rapidamente do que outras exibições. As informações podem ser atrasadas quando você alterna entre elas. 
- O desligamento das VMs atualiza alguns dos critérios de integridade para *crítico* e outros para *íntegro*. O estado da VM de rede é exibido como *críticos*.
- Para VMs do Linux, o título da página listando os critérios de integridade para exibição de uma única VM tem o nome de domínio inteiro da VM em vez do nome da VM definido pelo usuário. 
- Depois que você desabilitar o monitoramento de uma VM usando um dos métodos compatíveis e tentar implantá-la novamente, deverá implantá-la no mesmo workspace. Se você optar por usar um workspace diferente e tentar exibir o estado de integridade dessa VM, ela poderá apresentar um comportamento inconsistente.
- O critério de integridade da Utilização da CPU total para Windows mostra um limite de *não é igual a* **4**, o que significa que a utilização da CPU é maior que 95% e o comprimento da fila do sistema é maior que 15. Esse critério de integridade não é configurável nesta versão prévia.  
- Depois de remover os componentes da solução do seu espaço de trabalho, você poderá continuar a ver o estado de integridade de suas VMs do Azure; especificamente dados de desempenho e mapa quando você navega para qualquer visualização no portal. Os dados acabarão parando de aparecer na visualização Desempenho e Mapa depois de algum tempo; no entanto, a visualização de integridade continuará mostrando o status de integridade de suas VMs. A opção **Experimente agora** estará disponível para reintegração apenas das exibições Desempenho e Mapa.

## <a name="next-steps"></a>Próximas etapas
Para entender os requisitos e os métodos para habilitar o monitoramento das máquinas virtuais, examine [Implantar o Azure Monitor para VMs](vminsights-onboard.md).
