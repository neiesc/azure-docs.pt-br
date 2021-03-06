---
title: Importação de dados para ingestão de dados em um índice de pesquisa – Azure Search
description: Popule e carregue dados em um índice no Azure Search por meio de fontes de dados externas.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 01/05/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 731519b4e099bd696002af3aa08ada145e490260
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314849"
---
# <a name="indexing-external-data-for-queries-in-azure-search"></a>Indexando dados externos para consultas no Azure Search
> [!div class="op_single_selector"]
> * [Visão geral](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

No Azure Search, as consultas são executadas sobre o conteúdo carregado e salvo em um [índice de pesquisa](search-what-is-an-index.md). Este artigo examina as duas abordagens básicas para popular um índice: enviar os dados por *push* para o índice de forma programática ou apontar um [indexador do Azure Search](search-indexer-overview.md) em uma fonte de dados com suporte para efetuar *pull* dos dados.

## <a name="pushing-data-to-an-index"></a>Envio por push de dados para um índice
O modelo de push, usado para enviar seus dados para o Azure Search, é a abordagem mais flexível. Primeiro, ele não tem restrições de tipo de fonte de dados. Qualquer conjunto de dados composto por documentos JSON pode ser enviado a um índice do Azure Search, supondo que cada documento no conjunto de dados tenha mapeamentos para campos definidos no seu esquema de índice. Em segundo lugar, ele não tem nenhuma restrição de frequência de execução. Você pode enviar por push as alterações para um índice sempre que desejar. Para os aplicativos com requisitos de latência muito baixos (por exemplo, se você precisar que as operações de pesquisa estejam sincronizadas com os bancos de dados de inventário), um modelo de push será sua única opção.

Essa abordagem é mais flexível do que um modelo de pull, pois você pode carregar os documentos individualmente ou em lotes (até 1.000 por lote ou 16 MB, o limite que vier em primeiro lugar). O modelo de push também permite que você carregue documentos para o Azure Search, independentemente de onde os dados estão.

### <a name="how-to-push-data-to-an-azure-search-index"></a>Como enviar dados por push para um índice do Azure Search

Você pode usar as APIs a seguir para carregar um ou vários documentos em um índice:

+ [Adicionar, atualizar ou excluir documentos (API REST)](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents)
+ [Classe indexAction](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction?view=azure-dotnet) ou [Classe indexBatch](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) 

Atualmente, não há nenhum suporte de ferramenta para enviar por push dados por meio do portal.

Para obter uma introdução a cada metodologia, veja [Importar dados usando REST](search-import-data-rest-api.md) ou [Importar dados usando .NET](search-import-data-dotnet.md).


## <a name="pulling-data-into-an-index"></a>Pull de dados para um índice
O modelo de pull rastreia uma fonte de dados com suporte e carrega automaticamente os dados no índice. No Azure Search, esse recurso é implementado por meio de *indexadores*, atualmente disponíveis para essas plataformas:

+ [Armazenamento de Blobs](search-howto-indexing-azure-blob-storage.md)
+ [Armazenamento de tabelas](search-howto-indexing-azure-tables.md)
+ [Azure Cosmos DB](https://aka.ms/documentdb-search-indexer)
+ [Banco de Dados SQL do Azure e SQL Server em VMs do Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

Indexadores conectam um índice a uma fonte de dados (geralmente uma tabela, exibição ou estrutura equivalente) e mapeiam campos de origem para os campos equivalentes no índice. Durante a execução, o conjunto de linhas é automaticamente transformado em JSON e carregado no índice especificado. Todos os indexadores dão suporte à programação. Assim, você pode especificar com que frequência os dados devem ser atualizados. A maioria dos indexadores fornece controle de alterações se a fonte de dados oferecer suporte a isso. Rastreando as alterações e as exclusões para os documentos existentes, além de reconhecer novos documentos, os indexadores dispensam a necessidade de gerenciar ativamente os dados no índice. 


### <a name="how-to-pull-data-into-an-azure-search-index"></a>Como efetuar pull de dados para um índice do Azure Search

A funcionalidade de indexador é exposta no [Portal do Azure](search-import-data-portal.md), na [API REST](/rest/api/searchservice/Indexer-operations) e no [SDK do .NET](/dotnet/api/microsoft.azure.search.indexersoperationsextensions). 

Uma vantagem de usar o portal é que o Azure Search normalmente pode gerar um esquema de índice padrão para você, lendo os metadados do conjunto de dados de origem. Você pode modificar o índice gerado até que ele seja processado. Depois disso, as únicas edições de esquema permitidas são aquelas que não exigem reindexação. Se as alterações que você deseja fazer afetarem o esquema diretamente, será necessário recompilar o índice. 

## <a name="verify-data-import-with-search-explorer"></a>Verificar a importação de dados com o Search Explorer

Uma maneira rápida de executar uma verificação preliminar sobre o upload do documento é usar o **Search Explorer** no portal. O explorador permite consultar um índice sem precisar escrever código. A experiência de pesquisa se baseia nas configurações padrão, como a [sintaxe simples](/rest/api/searchservice/simple-query-syntax-in-azure-search) e padrão [searchMode query parameter](/rest/api/searchservice/search-documents). Os resultados são retornados em JSON para que você possa inspecionar o documento inteiro.

> [!TIP]
> Vários [exemplos de código de Azure Search](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) incluem conjuntos de dados incorporados ou prontamente disponíveis, oferecendo uma maneira fácil de começar a trabalhar. O portal também fornece um indexador de exemplo e uma fonte de dados que consiste em um conjunto de dados de imóveis pequeno (denominado "realestate-us-sample"). Quando você executa o indexador pré-configurado na fonte de dados de exemplo, um índice é criado e carregado com documentos que podem ser consultados no Search Explorer ou pelo código que você escreve.

## <a name="see-also"></a>Consulte também

+ [Visão geral do indexador](search-indexer-overview.md)
+ [Passo a passo do portal: criar, carregar, consultar um índice](search-get-started-portal.md)
