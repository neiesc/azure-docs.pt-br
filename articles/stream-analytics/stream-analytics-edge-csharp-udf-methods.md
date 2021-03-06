---
title: Desenvolver funções do .NET Standard para trabalhos do Edge do Azure Stream Analytics (versão prévia)
description: Saiba como gravar funções definidas pelo usuário C# para trabalhos do Edge do Stream Analytics.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 5df4c9dfe18b02ade3a37717da9c68acbfcf1853
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106593"
---
# <a name="develop-net-standard-user-defined-functions-for-azure-stream-analytics-edge-jobs-preview"></a>Desenvolver funções definidas pelo usuário do .NET Standard para trabalhos do Edge do Azure Stream Analytics (versão prévia)

O Azure Stream Analytics oferece uma linguagem de consulta semelhante à SQL para executar transformações e cálculos sobre fluxos de dados de eventos. Há muitas funções internas, mas alguns cenários complexos exigem flexibilidade adicional. Com as UDF (funções definidas pelo usuário) do .NET Standard, é possível invocar suas próprias funções gravadas em qualquer linguagem padrão do .NET (C#, F# etc.) para estender a linguagem de consulta do Stream Analytics. As UDFs permitem que você execute cálculos matemáticos complexos, importe modelos de ML personalizados usando ML.NET e use lógica de imputação personalizada para dados ausentes. O recurso UDF para trabalhos de borda do Stream Analytics está atualmente em visualização e não deve ser usado nas cargas de trabalho de produção.

## <a name="overview"></a>Visão geral
As ferramentas do Visual Studio para Azure Stream Analytics facilitam a gravação de UDFs, testam os trabalhos localmente (mesmo offline) e publicam o trabalho do Stream Analytics no Azure. Uma vez publicado no Azure, será possível implantar o trabalho em dispositivos IoT usando Hub IoT.

Há três maneiras de implementar UDFs:

* Arquivos CodeBehind em um projeto ASA
* UDF de um projeto local
* Um pacote existente de uma conta de armazenamento do Azure

## <a name="package-path"></a>Caminho do pacote

O formato de qualquer pacote UDF possui o caminho `/UserCustomCode/CLR/*`. As DLLs (Bibliotecas de Vínculo Dinâmico) e os recursos são copiados na pasta `/UserCustomCode/CLR/*`, o que ajuda a isolar DLLs do usuário do sistema e DLLs do Azure Stream Analytics. Esse caminho de pacote é usado para todas as funções, independentemente do método usado para empregá-las.

## <a name="supported-types-and-mapping"></a>Tipos com suporte e mapeamento

|**Tipo de UDF (C#)**  |**Tipo do Azure Stream Analytics**  |
|---------|---------|
|longo  |  bigint   |
|double  |  double   |
|string  |  nvarchar(max)   |
|dateTime  |  dateTime   |
|struct  |  IRecord   |
|objeto  |  IRecord   |
|Matriz<object>  |  IArray   |
|dictionary<string, object>  |  IRecord   |

## <a name="codebehind"></a>CodeBehind
É possível gravar funções definidas pelo usuário no CodeBehind do **Script.asql**. As ferramentas do Visual Studio compilarão automaticamente o arquivo CodeBehind em um arquivo do assembly. Os assemblies são empacotados como um arquivo zip e carregados na conta de armazenamento quando você envia o trabalho para o Azure. Você pode aprender como gravar uma UDF em C# usando CodeBehind seguindo o tutorial [UDF em C# para trabalhos do Edge do Stream Analytics](stream-analytics-edge-csharp-udf.md). 

## <a name="local-project"></a>Projeto local
As funções definidas pelo usuário podem ser gravadas em um assembly que é posteriormente referenciado em uma consulta do Azure Stream Analytics. Essa é a opção recomendada para funções complexas que exigem todo o potencial de uma linguagem .NET Standard além de sua linguagem de expressão como lógica de procedimento ou recursão. As UDFs de um projeto local também podem ser usadas quando for necessário compartilhar a lógica de função em várias consultas do Azure Stream Analytics. Adicionar UDFs ao projeto local permite depurar e testar as funções localmente no Visual Studio.

Para referenciar um projeto local:

1. Crie uma nova biblioteca de classes na solução.
2. Grave o código da classe. Lembre-se de que as classes devem ser definidas como *públicas* e os objetos devem ser definidos como *públicos estáticos*. 
3. Compile o projeto. As ferramentas empacotarão todos os artefatos na pasta bin em um arquivo zip e carregarão o arquivo zip na conta de armazenamento. Para obter referências externas, use a referência de assembly em vez do pacote NuGet.
4. Faça referência à nova classe no projeto do Azure Stream Analytics.
5. Adicione uma nova função no projeto do Azure Stream Analytics.
6. Configure o caminho do assembly no arquivo de configuração do trabalho, `JobConfig.json`. Defina o caminho do assembly para **Referência de projeto local ou CodeBehind**.
7. Recompile o projeto de função e o projeto do Azure Stream Analytics.  

### <a name="example"></a>Exemplo

Nesse exemplo, **UDFTest** é um projeto de biblioteca de classes C# e **ASAEdgeUDFDemo** é o projeto do Edge do Azure Stream Analytics, que referenciará **UDFTest**.

![Projeto do Azure IoT Edge do Azure Stream Analytics no Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-demo.png)

1. Crie o projeto C#, que permitirá adicionar uma referência à UDF em C# a partir da consulta do Azure Stream Analytics.
    
   ![Compilar um projeto do IoT Edge do Azure Stream Analytics no Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-build-project.png)

2. Adicione a referência ao projeto C# no projeto do ASA Edge. Clique com o botão direito do mouse no nó References e escolha Adicionar Referência.

   ![Adicionar uma referência a um projeto C# no Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-reference.png)

3. Escolha o nome do projeto C# na lista. 
    
   ![Escolher o nome de projeto C# na lista de referência](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-choose-project-name.png)

4. Você deve ver o **UDFTest** listado em **Referências** no **Gerenciador de Soluções**.

   ![Exibir a referência de função definida pelo usuário no gerenciador de soluções](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-added-reference.png)

5. Clique com o botão direito do mouse na pasta **Funções** e escolha **Novo Item**.

   ![Adicionar um novo item a Funções na solução do Edge do Azure Stream Analytics](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function.png)

6. Adicione uma função C# **SquareFunction.json** ao projeto do Azure Stream Analytics.

   ![Selecione a função C Sharp dos itens do Edge do Stream Analytics no Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function-2.png)

7. Clique duas vezes na função no **Gerenciador de Soluções** para abrir a caixa de diálogo de configuração.

   ![Configuração de função C Sharp no Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-csharp-function-config.png)

8. Na configuração de função C#, escolha **Carregar da Referência do Projeto ASA** e os nomes de métodos, classe e assembly relacionados na lista suspensa. Para fazer referência aos métodos, tipos e funções na consulta do Edge do Stream Analytics, as classes devem ser definidas como *públicas* e os objetos devem ser definidos como *públicos estáticos*.

   ![Configuração de função C Sharp do Stream Analytics](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-asa-csharp-function-config.png)

## <a name="existing-packages"></a>Pacotes existentes

É possível criar UDFs do .NET Standard em qualquer IDE de sua escolha e invocá-las na consulta do Azure Stream Analytics. Primeiro compile o código e empacote todas as DLLs. O formato do pacote tem o caminho `/UserCustomCode/CLR/*`. Em seguida, carregue `UserCustomCode.zip` para a raiz do contêiner na conta de armazenamento do Azure.

Depois que os pacotes zip de assembly forem carregados na conta de armazenamento do Azure, será possível usar as funções nas consultas do Azure Stream Analytics. Tudo o que você precisa fazer é incluir as informações de armazenamento na configuração do trabalho do Edge do Stream Analytics. Não é possível testar a função localmente com essa opção porque as ferramentas do Visual Studio não farão download do pacote. O caminho do pacote é analisado diretamente para o serviço. 

Para configurar o caminho do assembly no arquivo de configuração de trabalho, `JobConfig.json`:

Expanda o **a configuração de código definidos pelo usuário** seção e, em seguida, preencha a configuração com os seguintes valores sugeridos:

 |**Configuração**  |**Valor sugerido**  |
 |---------|---------|
 |Origem do assembly  | Pacotes existentes do assembly da nuvem    |
 |Recurso  |  Escolha os dados da conta atual   |
 |Assinatura  |  Escolha sua assinatura.   |
 |Conta de armazenamento  |  Escolha sua conta de armazenamento.   |
 |Contêiner  |  Escolha o contêiner que você criou na sua conta de armazenamento.   |

![Configuração do trabalho do Edge do Azure Stream Analytics no Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-job-config.png)

## <a name="limitations"></a>Limitações
A versão prévia da UDF atualmente possui as seguintes limitações:

* As linguagens .NET Standard somente podem ser usadas para Azure Stream Analytics no IoT Edge. Para trabalhos na nuvem, é possível gravar funções definidas pelo usuário do JavaScript. Para saber mais, visite o tutorial [UDF do JavaScript do Azure Stream Analytics](stream-analytics-javascript-user-defined-functions.md).

* As UDFs do .NET Standard somente podem ser criadas no Visual Studio e publicadas no Azure. As versões somente leitura das UDFs do .NET Standard podem ser exibidas em **Funções** no portal do Azure. Não há suporte para a criação de funções do .NET Standard no portal do Azure.

* O editor de consulta do portal do Azure mostra um erro ao usar a UDF do .NET Standard no portal. 

* Como o código personalizado compartilha o contexto com o mecanismo do Azure Stream Analytics, o código personalizado não pode fazer referência a nada que tenha um namespace/dll_name conflitante com o código do Azure Stream Analytics. Por exemplo, não é possível referenciar *Newtonsoft Json*.

## <a name="next-steps"></a>Próximas etapas

* [Tutorial: Gravar uma função C# definida pelo usuário para um trabalho do Azure Stream Analytics Edge (versão prévia)](stream-analytics-edge-csharp-udf.md)
* [Tutorial: Funções definidas pelo usuário do JavaScript do Azure Stream Analytics](stream-analytics-javascript-user-defined-functions.md)
* [Use o Microsoft Visual Studio para visualizar os trabalhos do Azure Stream Analytics](stream-analytics-vs-tools.md)
