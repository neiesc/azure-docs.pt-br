---
title: Introdução às consultas no Azure Log Analytics | Microsoft Docs
description: Este artigo fornece um tutorial de Introdução a consultas de começar a desenvolver no Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: bwren
ms.openlocfilehash: 326e12444067e950b9d6ae0862424589d444b83d
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52884272"
---
# <a name="get-started-with-queries-in-log-analytics"></a>Introdução às consultas no Log Analytics


> [!NOTE]
> Você deve concluir [Primeiros passos no portal do Google Analytics](get-started-portal.md) antes de concluir este tutorial.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Neste tutorial, você aprenderá a escrever consultas do Log Analytics do Azure. Ele irá ensiná-lo como para:

- Entender a estrutura de consultas
- Classificar os resultados de consulta
- Filtrar resultados da consulta
- Especifique um intervalo de tempo
- Selecione quais campos serão incluídos nos resultados
- Definir e usar campos personalizados
- Agregar e agrupar resultados


## <a name="writing-a-new-query"></a>Escrevendo uma nova consulta
As consultas podem começar com um nome de tabela ou com o comando *pesquisa*. Você deve começar com um nome de tabela, pois ele define um escopo claro para a consulta e melhora o desempenho da consulta e a relevância dos resultados.

> [!NOTE]
> A linguagem de consulta do Log Analytics do Azure diferencia maiusculas de minúsculas. Palavras-chave geralmente são escritas em minúsculas. Ao usar nomes de tabelas ou colunas em uma consulta, certifique-se de usar a capitalização correta, conforme mostrado no painel de esquema.

### <a name="table-based-queries"></a>Consultas baseadas em tabela
O Azure Log Analytics organiza dados em tabelas, cada composta por várias colunas. Todas as tabelas e colunas são mostradas no painel de esquema, no portal do Analytics. Identifica uma tabela que você está interessado e, em seguida, vejamos um pouco de dados:

```Kusto
SecurityEvent
| take 10
```

A consulta mostrada acima retorna 10 resultados do *SecurityEvent* tabela, em nenhuma ordem específica. Isso é uma maneira muito comum para fazer rapidamente em uma tabela e entender sua estrutura e conteúdo. Vamos examinar como ele é criado:

* A consulta é iniciada com o nome da tabela *SecurityEvent* -esta parte define o escopo da consulta.
* O caractere de barra vertical (|) separa os comandos, portanto, a saída da primeira na entrada do comando a seguir. Você pode adicionar qualquer número de elementos conectados.
* Seguindo o pipe está o comando **take**, que retorna um número específico de registros arbitrários da tabela.

Na verdade, estamos poderia executar a consulta mesmo sem adicionar `| take 10` - que ainda seria válida, mas ele poderia retornar resultados até 10.000.

### <a name="search-queries"></a>Consultas de pesquisa
Consultas de pesquisa são menos estruturado e geralmente mais adequada para localizar registros que incluem um valor específico em qualquer uma de suas colunas:

```Kusto
search in (SecurityEvent) "Cryptographic"
| take 10
```

Essa consulta pesquisa a tabela *SecurityEvent* em busca de registros que contenham a frase "Criptografia". Desses registros, 10 registros serão retornados e exibidos. Se omitirmos a parte e `in (SecurityEvent)`apenas executarmos`search "Cryptographic"`, a pesquisa irá passar por *todas* as tabelas, o que levaria mais tempo e seria menos eficiente.

> [!NOTE]
> Por padrão, um intervalo de tempo _últimas 24 horas_ está definido. Para usar um intervalo diferente, use o seletor de tempo (localizada ao lado de *vá* botão) ou adicionar um tempo explícito filtro de intervalo à sua consulta.

## <a name="sort-and-top"></a>Classificar e superior
Embora **levar** é útil para obter alguns registros, os resultados são selecionados e exibidos em nenhuma ordem específica. Para obter uma exibição ordenada, você poderia **classificação** pela coluna preferencial:

```Kusto
SecurityEvent   
| sort by TimeGenerated desc
```

Que poderia retornar um número excessivo de resultados no entanto e também pode levar algum tempo. A consulta acima classifica *todo o* SecurityEvent tabela pela coluna TimeGenerated. O portal do Google Analytics limita a exibição para mostrar apenas 10.000 registros. Essa abordagem, obviamente, não é a ideal.

A melhor maneira de obter os registros mais recentes 10 é usar **superior**, que classifica a tabela inteira no lado do servidor e, em seguida, retorna os primeiros registros:

```Kusto
SecurityEvent
| top 10 by TimeGenerated
```

Em ordem decrescente é o padrão, a ordem de classificação, portanto, podemos normalmente omitir as **desc** argumento. A saída terá esta aparência:

![10 principais](media/get-started-queries/top10.png)


## <a name="where-filtering-on-a-condition"></a>Em que: filtrando uma condição
Filtros, conforme indicado pelo seu nome, filtram os dados por uma condição específica. Essa é a maneira mais comum de limitar os resultados da consulta a informações relevantes.

Para adicionar um filtro a uma consulta, use o operador **onde** seguido por uma ou mais condições. Por exemplo, a consulta a seguir retorna apenas *SecurityEvent* registros em que _nível_ é igual a _8_:

```Kusto
SecurityEvent
| where Level == 8
```

Ao escrever as condições de filtro, você pode usar as expressões a seguir:

| Expression | DESCRIÇÃO | Exemplo |
|:---|:---|:---|
| == | Verificação de igualdade<br>(diferencia maiusculas de minúsculas) | `Level == 8` |
| =~ | Verificação de igualdade<br>(diferencia maiusculas de minúsculas) | `EventSourceName =~ "microsoft-windows-security-auditing"` |
| ! =, <> | Verifique a desigualdade<br>(as duas expressões são idênticas) | `Level != 4` |
| *e*, *ou* | Necessário entre condições| `Level == 16 or CommandLine != ""` |

Para filtrar por várias condições, você pode usar **e**:

```Kusto
SecurityEvent
| where Level == 8 and EventID == 4672
```

ou canalizar vários **em que** elementos um após o outro:

```Kusto
SecurityEvent
| where Level == 8 
| where EventID == 4672
```
    
> [!NOTE]
> Os valores podem ter tipos diferentes, portanto, talvez seja necessário convertê-los para realizar a comparação no tipo correto. Por exemplo, a coluna SecurityEvent *Level* é do tipo String, então você deve convertê-la em um tipo numérico como *int* ou *long*, antes de poder usar operadores numéricos sobre isso: `SecurityEvent | where toint(Level) >= 10`

## <a name="specify-a-time-range"></a>Especifique um intervalo de tempo

### <a name="time-picker"></a>Seletor de tempo
O seletor de hora está próximo ao botão Executar e indica que estamos consultando apenas registro das últimas 24 horas. Isso é o intervalo de tempo padrão aplicado a todas as consultas. Para obter apenas os registros da última hora, selecione _última hora_ e execute a consulta novamente.

![Seletor de Tempo](media/get-started-queries/timepicker.png)


### <a name="time-filter-in-query"></a>Filtro de tempo na consulta
Você também pode definir seu próprio intervalo de tempo adicionando um filtro de tempo à consulta. É melhor colocar o filtro de tempo imediatamente após o nome da tabela: 

```Kusto
SecurityEvent
| where TimeGenerated > ago(30m) 
| where toint(Level) >= 10
```

No filtro de hora acima, `ago(30m)` significa "30 minutos atrás", portanto, essa consulta retorna apenas registros dos últimos 30 minutos. Outras unidades de tempo incluem dias (2d), minutos (25m) e segundos (10s).


## <a name="project-and-extend-select-and-compute-columns"></a>Projeto e Extensão: selecione e calcule colunas
Use **projeto** para selecionar colunas específicas a serem incluídas nos resultados:

```Kusto
SecurityEvent 
| top 10 by TimeGenerated 
| project TimeGenerated, Computer, Activity
```

O exemplo anterior gera esta saída:

![Resultados do projeto de análise de log](media/get-started-queries/project.png)

Você também pode usar **projeto** para renomear colunas e definir novos. O exemplo a seguir usa o projeto para fazer o seguinte:

* Selecione apenas o *computador* e *TimeGenerated* colunas originais.
* Renomeie o *atividade* coluna *EventDetails*.
* Criar uma nova coluna chamada *EventCode*. A função **substring ()** é usada para obter apenas os quatro primeiros caracteres do campo Activity.


```Kusto
SecurityEvent
| top 10 by TimeGenerated 
| project Computer, TimeGenerated, EventDetails=Activity, EventCode=substring(Activity, 0, 4)
```

**estender** mantém todas as colunas originais no conjunto de resultados e define outros adicionais. A seguinte consulta utiliza **estender** para adicionar um *localtime* coluna, que contém um valor de TimeGenerated localizado.

```Kusto
SecurityEvent
| top 10 by TimeGenerated
| extend localtime = TimeGenerated-8h
```

## <a name="summarize-aggregate-groups-of-rows"></a>Resumo: agregar grupos de linhas
Use **resumir** para identificar grupos de registros, de acordo com uma ou mais colunas, e aplicar agregações a eles. O uso mais comum de **resumir** é a *contagem*, que retorna o número de resultados em cada grupo.

A consulta a seguir examina todos os *Perf* registros de última hora, agrupa-os por *ObjectName*e conta os registros em cada grupo: 
```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize count() by ObjectName
```

Às vezes, faz sentido para definir grupos por várias dimensões. Cada combinação exclusiva desses valores define um grupo separado:

```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize count() by ObjectName, CounterName
```

Outro uso comum é realizar cálculos matemáticos ou estatísticos em cada grupo. Por exemplo, o seguinte calcula a média *CounterValue* para cada computador:

```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize avg(CounterValue) by Computer
```

Infelizmente, os resultados dessa consulta não têm sentido, pois combinamos diferentes contadores de desempenho. Para tornar isso mais significativo, devemos calcular a média separadamente para cada combinação de *CounterName* e *computador*:

```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize avg(CounterValue) by Computer, CounterName
```

### <a name="summarize-by-a-time-column"></a>Resumir por uma coluna de hora
Agrupar os resultados também pode ser baseado em uma coluna de hora ou outro valor contínuo. Resumindo simplesmente `by TimeGenerated` entanto seria criar grupos para cada milissegundo de único ao longo do intervalo de tempo, pois esses são valores exclusivos. 

Para criar grupos com base em valores contínuos, é melhor dividir o intervalo em unidades gerenciáveis usando **bin**. A consulta a seguir analisa *Perf* registros que medem a memória livre (*MBytes disponíveis*) em um computador específico. Calcula o valor médio para cada período, se 1 hora, nos últimos 7 dias:

```Kusto
Perf 
| where TimeGenerated > ago(7d)
| where Computer == "ContosoAzADDS2" 
| where CounterName == "Available MBytes" 
| summarize avg(CounterValue) by bin(TimeGenerated, 1h)
```

Para tornar a saída mais clara, selecione-a como um gráfico de tempo, mostrando a memória disponível ao longo do tempo:

![Memória de análise de log ao longo do tempo](media/get-started-queries/chart.png)



## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre [como escrever consultas de pesquisa](search-queries.md)
