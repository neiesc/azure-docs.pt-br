---
title: Configurar fontes de dados do agente no Log Analytics | Microsoft Docs
description: As fontes de dados definem os dados de log que o Log Analytics coleta de agentes e outras fontes conectadas.  Este artigo descreve o conceito de como o Log Analytics usa fontes de dados, explica os detalhes de como configurá-las e fornece um resumo das diferentes fontes de dados disponíveis.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2018
ms.author: bwren
ms.openlocfilehash: d9bedeeb2e354dab8bc6a7be56826f28914326be
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54101523"
---
# <a name="agent-data-sources-in-log-analytics"></a>Fontes de dados do agente no Log Analytics
Os dados coletados dos agentes pelo Log Analytics são definidos pelas fontes de dados configuradas.  Os dados dos agentes são armazenados como [dados de log](data-collection.md) com um conjunto de registros.  Cada fonte de dados cria registros de um determinado tipo com cada tipo de tendo seu próprio conjunto de propriedades.

![Coleta de dados de log](media/agent-data-sources/overview.png)

## <a name="summary-of-data-sources"></a>Resumo das fontes de dados
A tabela a seguir lista as fontes de dados de agente atualmente disponíveis no Log Analytics.  Cada uma tem um link para um artigo à parte que fornece detalhes para aquela fonte de dados.   Fornece também informações sobre o método e a frequência de coleta. 


| Fonte de dados | Plataforma | Agente de monitoramento da Microsoft | Agente do Operations Manager | Armazenamento do Azure | Operations Manager necessário? | Dados de agente do Operations Manager enviados por meio do grupo de gerenciamento | Frequência de coleta |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Logs personalizados](data-sources-custom-logs.md) |  Windows |&#8226; |  | |  |  | na chegada |
| [Logs personalizados](data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | na chegada |
| [Logs do IIS](data-sources-iis-logs.md) |  Windows |&#8226; |&#8226; |&#8226; |  |  |depende da configuração de rolagem de arquivo de log |
| [Contadores de desempenho](data-sources-performance-counters.md) |  Windows |&#8226; |&#8226; |  |  |  |conforme agendado, mínimo de 10 segundos |
| [Contadores de desempenho](data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |conforme agendado, mínimo de 10 segundos |
| [Syslog](data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |do armazenamento do Azure: 10 minutos; do agente: na chegada |
| [Logs de eventos do Windows](data-sources-windows-events.md) | Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | na chegada |


## <a name="configuring-data-sources"></a>Configurando fontes de dados
Configure fontes de dados no menu **Dados** nas **Configurações Avançadas** do workspace.  Todas as configurações são entregues a todas as fontes conectadas em seu workspace.  No momento, você não pode excluir nenhum agente dessa configuração.

![Configurar eventos do Windows](./media/agent-data-sources/configure-events.png)

1. No portal do Azure, selecione **Log Analytics** &gt; seu workspace &gt; **Configurações Avançadas**.
2. Selecione **Dados**.
3. Clique na fonte de dados que você deseja configurar.
4. Siga o link para a documentação para cada fonte de dados na tabela acima para obter detalhes sobre sua configuração.


## <a name="data-collection"></a>Coleta de dados
As configurações de fonte de dados são entregues aos agentes que estão diretamente conectados ao Log Analytics dentro de alguns minutos.  Os dados especificados são coletados do agente e entregues diretamente para o Log Analytics em intervalos específicos para cada fonte de dados.  Consulte a documentação para cada fonte de dados para encontrar essas especificações.

Para agentes do System Center Operations Manager em um grupo de gerenciamento conectado, as configurações de fonte de dados são convertidas em pacotes de gerenciamento e entregues ao grupo de gerenciamento a cada cinco minutos por padrão.  O agente baixa o pacote de gerenciamento como qualquer outro e coleta os dados especificados. Dependendo da fonte de dados, os dados serão enviados para um servidor de gerenciamento que os encaminhará para o Log Analytics ou o agente enviará os dados para o Log Analytics sem passar pelo servidor de gerenciamento. Confira [Detalhes da coleta de dados para soluções de monitoramento no Azure](../../azure-monitor/insights/solutions-inventory.md) para obter detalhes.  Você pode ler sobre os detalhes da conexão do Operations Manager e do Log Analytics e a modificação da frequência em que a configuração é entregue em [Configure Integration with System Center Operations Manager](../../log-analytics/log-analytics-om-agents.md)(Configurar a integração com o System Center Operations Manager).

Se o agente não pode se conectar ao Log Analytics ou Operations Manager, ele continuará a coletar dados que fornecerá ao estabelecer uma conexão.  Dados podem ser perdidos se a quantidade de dados atingir o tamanho máximo do cache do cliente, ou se o agente não for capaz de estabelecer uma conexão dentro de 24 horas.

## <a name="log-records"></a>Registros de log
Todos os dados coletados pelo Log Analytics são armazenados no workspace como registros.  Registros coletados por diferentes fontes de dados terão seu próprio conjunto de propriedades e serão identificados por sua propriedade **Type** .  Consulte a documentação para cada fonte de dados e a solução para obter detalhes sobre cada tipo de registro.

## <a name="next-steps"></a>Próximas etapas
* Conheça as [soluções de monitoramento](../../azure-monitor/insights/solutions.md) que adicionam funcionalidades ao Azure Monitor e também coletam dados no workspace.
* Saiba mais sobre as [consultas de log](../../log-analytics/log-analytics-queries.md) para analisar os dados coletados de fontes de dados e soluções de monitoramento.  
* Configure [alertas](../../monitoring-and-diagnostics/monitoring-overview-alerts.md) para notificá-lo de forma proativa sobre dados críticos coletados de fontes de dados e soluções de monitoramento.
