---
title: Noções básicas sobre o monitoramento de trabalhos no Azure Stream Analytics
description: Este artigo descreve como monitorar trabalhos do Azure Stream Analytics no portal do Azure.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 656f749fd2a930c51bfd7d1a99642fae87694846
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53096562"
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Noções básicas sobre o monitoramento de trabalhos do Stream Analytics e como monitorar consultas

## <a name="introduction-the-monitor-page"></a>Introdução A página do monitor
O Portal do Azure exibe as principais métricas de desempenho que podem ser usadas para monitorar e solucionar problemas de desempenho de seus trabalhos e consultas. Para ver essas métricas, procure o trabalho do Stream Analytics em cujas métricas você está interessado em ver e exiba a seção **Monitoramento** na página de visão geral.  

![Link do monitoramento de trabalho do Stream Analytics](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

A janela será exibida conforme mostrado:

![Painel do monitoramento de trabalho do Stream Analytics](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Métricas disponíveis para o Stream Analytics
| Métrica                 | Definição                               |
| ---------------------- | ---------------------------------------- |
| Eventos de Entrada Acumulados       | Número de eventos de entrada com lista de pendências. |
| Erros de conversão de dados | Número de eventos de saída que não pôde ser convertido para o esquema de saída esperado. |
| Eventos de Entrada Antecipados       | Notificar os usuários de eventos recebidos. |
| Solicitações de função com falha | Número de chamadas à função Azure Machine Learning com falha (se presente). |
| Eventos de função        | Número de eventos enviados à função Azure Machine Learning (se presente). |
| Solicitações de função      | Número de chamadas à função Azure Machine Learning (se presente). |
| Erros de desserialização de entrada       | Número de eventos que não pôdem ser desserializados.  |
| Bytes de evento de entrada      | Quantidade de dados recebidos pelo trabalho do Stream Analytics, em bytes. Isso pode ser usado para validar que os eventos estão sendo enviados para a fonte de entrada. |
| Eventos de entrada           | Quantidade de dados recebidos pelo trabalho do Stream Analytics, em termos de eventos. Isso pode ser usado para validar que os eventos estão sendo enviados para a fonte de entrada. |
| Fontes de Entrada Recebidas       | Número de eventos provenientes de uma fonte de entrada. |
| Eventos de entrada atrasados      | Número de eventos que chegam atrasados da fonte que podem ter sido descartados ou ter tido o carimbo de data/hora ajustado, com base na configuração de Política de ordenação de eventos da configuração da definição da Janela de tolerância de chegada atrasada. |
| Eventos fora de ordem    | Número de eventos recebidos fora de ordem que foram descartados ou que receberam um carimbo de data/hora ajustado, com base na Política de ordenação de evento. Isso pode ser afetado pela configuração da definição da Janela de tolerância fora de ordem. |
| Eventos de saída          | Quantidade de dados enviados pelo trabalho do Stream Analytics para o destino de saída, em números de evento. |
| Erros de tempo de execução         | O número total de erros relatados ao processamento de consultas (excluindo-se os erros encontrados durante a ingestão de eventos ou a saída de resultados) |
| % de utilização do SU       | A utilização das unidades de Streaming é atribuída a um trabalho na guia Escala do trabalho. Se esse indicador alcançar 80% ou acima, há grande probabilidade de que o processamento de eventos se atrase ou pare. |
| Atraso de Marca-d'água       | O atraso máximo de marca d'água em todas as partições de todas as saídas no trabalho. |

## <a name="customizing-monitoring-in-the-azure-portal"></a>Personalizando o Monitoramento no Portal do Azure
Você pode ajustar o tipo de gráfico, as métricas mostradas e o intervalo de hora nas configurações de Editar Gráfico. Para obter detalhes, veja [Como personalizar o monitoramento](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Gráfico de tempo do monitor de consultas do Stream Analytics](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Saída mais recente
Outro ponto de dados interessante para monitorar o trabalho é a hora da última saída, mostrada na página Visão geral.
Essa é o tempo de aplicação (ou seja, a hora usando o carimbo de data/hora dos dados de evento) da saída mais recente do trabalho.

## <a name="get-help"></a>Obter ajuda
Para obter mais assistência, experimente nosso [Fórum do Stream Analytics do Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Próximas etapas
* [Introdução ao Stream Analytics do Azure](stream-analytics-introduction.md)
* [Introdução ao uso do Stream Analytics do Azure](stream-analytics-real-time-fraud-detection.md)
* [Dimensionar trabalhos do Stream Analytics do Azure](stream-analytics-scale-jobs.md)
* [Referência de Linguagem de Consulta do Stream Analytics do Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência da API REST do Gerenciamento do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

