---
title: Tutorial sobre o Monitoramento e Ajuste no Banco de Dados do Azure para PostgreSQL
description: Este tutorial sobre o Monitoramento e Ajuste no Banco de Dados do Azure para PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: f05e0eef7680b08ce116cc0243d944f6a1db597c
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53536434"
---
# <a name="tutorial-monitor-and-tune-azure-database-for-postgresql"></a>Tutorial: Monitorar e ajustar o Banco de Dados do Azure para PostgreSQL

Banco de Dados do Azure para PostgreSQL tem recursos que ajudam você a compreender e melhorar o desempenho do servidor. Neste tutorial, você aprenderá a:
> [!div class="checklist"]
> * Habilitar a consulta e aguardar a coleta de estatísticas
> * Acessar e utilizar os dados coletados
> * Exibir desempenho de consulta e estatísticas de espera ao longo do tempo
> * Analisar um banco de dados para obter as recomendações de desempenho
> * Aplicar recomendações do desempenho

## <a name="before-you-begin"></a>Antes de começar
Você precisa de um Banco de Dados do Azure para PostgreSQL versão 9.6 ou 10. Você pode seguir as etapas no [Tutorial de criação ](tutorial-design-database-using-azure-portal.md) para criar um servidor.

> [!IMPORTANT]
> **Repositório de Consultas**, **Análise de Desempenho de Consultas**, e **Recomendação de Desempenho** estão em Visualização Pública.

## <a name="enabling-data-collection"></a>Habilitar coleta de dados
O [Repositório de Consultas](concepts-query-store.md) captura um histórico das consultas e estatísticas de espera em seu servidor e os armazena no banco de dados **azure_sys** em seu servidor. É um recurso opcional. Para habilitá-lo:

1. Abra o portal do Azure.

2. Selecione seu servidor de Banco de Dados do Azure para PostgreSQL.

3. Selecionar **Servidor de parâmetros** que estiver nas seções de **Configurações** do menu à esquerda.

4. Definir **pg_qs.query_capture_mode** a **superior** para começar a coletar dados de desempenho de consulta. Definir **pgms_wait_sampling.query_capture_mode** à **todos os** para iniciar a coltar as estatísticas de espera. Salve.
   
   ![Parâmetros de consulta do servidor Repositório de Consultas](./media/tutorial-performance-intelligence/query-store-parameters.png)

5. Permita que o primeiro lote de dados persista no banco de dados **azure_sys** por até 20 minutos.


## <a name="performance-insights"></a>Insights de desempenho
A visualização da [Análise de Desempenho de Consultas](concepts-query-performance-insight.md) no portal do Azure será superficial visualizações em informações do Repositório de Consultas. 

1. Na página do portal do Banco de Dados do Azure para PostgreSQL, selecione **Insight do Desempenho de Consulta** em **suporte + seção solução de problemas** do menu à esquerda.

2. A guia **consultas de Longa execução** mostra as 5 consultas superiores por duração média por execução, agregadas em intervalos de 15 minutos. 
   
   ![Página de aterrissagem de Análise de Desempenho de Consultas](./media/tutorial-performance-intelligence/query-performance-insight-landing-page.png)

   Você pode exibir mais consultas, selecionando a partir do **Número de consultas** lista suspensa. As cores do gráfico pode ser alteradas para uma ID de consulta específica ao fazer isso.

3. Você pode clicar e arrastar no gráfico para restringi-lo a uma janela de tempo específico.

4. Use os ícones de ampliar e afastar para exibir um período maior ou menor, respectivamente.

5. Exiba a tabela abaixo do gráfico para obter mais detalhes sobre as consultas de longa execução na janela de tempo.

6. Selecione a guia das **Estatísticas de Espera** guia para exibir as visualizações correspondentes em espera no servidor.
   
   ![Estatísticas de espera de Análise de Desempenho de Consultas](./media/tutorial-performance-intelligence/query-performance-insight-wait-statistics.png)

### <a name="permissions"></a>Permissões
**Permissões do Proprietário** ou **Colaborador** necessárias para exibir o texto das consultas na Análise de Desempenho de Consultas **Leitor** podem exibir gráficos e tabelas, mas não o texto da consulta.


## <a name="performance-recommendations"></a>Recomendações do desempenho
O recurso das [Recomendações de Desempenho](concepts-performance-recommendations.md) recurso analisa as cargas de trabalho entre seu servidor para identificar os índices com o potencial de melhorar o desempenho.

1. Abra **Recomendações de Desempenho** da seção **suporte + solução de problemas** da barra de menus, na página do portal do Azure para seu servidor PostgreSQL.
   
   ![Página das Recomendações de Desempenho](./media/tutorial-performance-intelligence/performance-recommendations-landing-page.png)

2. Selecione **Analisar** e escolha um banco de dados. Isso iniciará a análise.

3. Dependendo de sua carga de trabalho, isso pode levar vários minutos para ser concluído. Quando a análise for concluída, haverá uma notificação no portal.

4. A janela **Recomendações de Desempenho** Mostrará uma lista de recomendações, caso seja encontrada. 

5. Uma recomendação mostrará informações sobre os relevantes **banco de dados**, **tabela**, **coluna**, e **tamanho de índice**.

   ![Recomendações de Desempenho](./media/tutorial-performance-intelligence/performance-recommendations-result.png)

6. Para implementar a recomendação, copie o texto da consulta e executá-lo do seu cliente de escolha.

### <a name="permissions"></a>Permissões
**Permissões de Proprietário** ou **Colaborador** necessárias para executar a análise usando o recurso de recomendações de desempenho.

## <a name="next-steps"></a>Próximas etapas
- Saiba mais sobre [monitoramento e ajuste](concepts-monitoring.md) no Banco de Dados do Azure para PostgreSQL.