---
title: Associações de Hubs de Eventos do Azure para o Azure Functions
description: Entenda como usar associações dos Hubs de Eventos do Azure no Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: azure functions, funções, processamento de eventos, computação dinâmica, arquitetura sem servidor
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/08/2017
ms.author: cshoe
ms.openlocfilehash: bc7ed9051f95877760bccec65ff2fa7f49e44993
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/06/2018
ms.locfileid: "53002153"
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Associações de Hubs de Eventos do Azure para o Azure Functions

Este artigo explica como trabalhar com associações de [Hubs de Eventos do Azure](../event-hubs/event-hubs-what-is-event-hubs.md) para o Azure Functions. O Azure Functions dá suporte a associações de gatilho e de saída para os Hubs de Eventos.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pacotes - Functions 1. x

Para a versão de funções do Azure 1. x, em que as associações de Hubs de eventos são fornecidas a [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) pacote NuGet, versão 2. x.
O código-fonte do pacote está no repositório GitHub [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs).


[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pacotes - Functions 2. x

Para funções 2. x, use o [Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) pacote, versão 3. x.
O código-fonte do pacote está no repositório GitHub [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Gatilho

Use o gatilho dos Hubs de Eventos do Azure para responder a um evento enviado para uma transmissão de evento do hub de eventos. É necessário ter acesso de leitura ao hub de eventos para configurar o gatilho.

Quando uma função de gatilho dos Hubs de Eventos é disparada, a mensagem que a dispara é passada para a função como cadeia de caracteres.

## <a name="trigger---scaling"></a>Gatilho - dimensionamento

Cada instância de uma função disparada pelo hub de eventos tem suporte apenas de uma instância de [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor). Os Hubs de Eventos garantem que apenas uma instância de [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) possa obter uma concessão em uma determinada partição.

Por exemplo, considere um Hub de eventos da seguinte maneira:

* 10 partições.
* 1000 eventos distribuídos uniformemente em todas as partições, com 100 mensagens em cada partição.

Quando sua função é habilitada pela primeira vez, há apenas uma instância da função. Vamos chamar essa instância de função `Function_0`. `Function_0` tem uma única instância de [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) que tem uma concessão em todas as dez partições. Esta instância está lendo eventos das partições 0 a 9. Deste ponto em diante, uma destas opções ocorre:

* **Novas instâncias de função não são necessárias**: `Function_0` é capaz de processar todos os 1000 eventos antes da lógica de dimensionamento do Functions entrar em ação. Nesse caso, todas as 1000 mensagens são processadas pelo `Function_0`.

* **Uma instância de função adicional é adicionada**: A lógica de dimensionamento do Functions determina que `Function_0` tem mais mensagens do que pode processar. Neste caso, uma nova instância do aplicativo de função (`Function_1`) é criada, junto com uma nova instância do [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor). Os Hubs de Eventos detectam que uma nova instância de host está tentando ler mensagens. Os Hubs de Eventos fazem o balanceamento de carga das partições entre essas instâncias de host. Por exemplo, as partições de 0 a 4 podem ser atribuídas a `Function_0` e as partições de 5 a 9 para `Function_1`.

* **Mais N instâncias de função são adicionadas**: A lógica de dimensionamento do Functions determina que `Function_0` e `Function_1` têm mais mensagens do que podem processar. Novas instâncias do aplicativo de função `Function_2` a `Functions_N` são criadas, em que `N` é maior que o número de partições do hub de eventos. Em nosso exemplo, os Hubs de Eventos balanceiam a carga das partições novamente e, nesse caso, entre as instâncias `Function_0` e `Functions_9`.

Observe isso quando o Functions é dimensionado para `N` instâncias, que é um número maior que o número de partições do hub de eventos. Isso é feito para certificar-se de que sempre haja instâncias de [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) disponíveis para obter bloqueios em partições conforme elas ficam disponíveis de outras instâncias. Você paga apenas pelos recursos usados quando a instância de função é executada; você não é cobrado por esse excesso de provisionamento.

Quando todas as execuções de função são concluídas (com ou sem erros), os pontos de verificação são adicionados à conta de armazenamento associada. Quando a verificação é bem-sucedida, todas as 1000 mensagens não são mais recuperadas.

## <a name="trigger---example"></a>Gatilho - exemplo

Consulte o exemplo específico a um idioma:

* [C#](#trigger---c-example)
* [Script do C# (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [Java](#trigger---java-example)
* [JavaScript](#trigger---javascript-example)
* [Python](#trigger---python-example)

### <a name="trigger---c-example"></a>Gatilho - exemplo C#

O exemplo a seguir mostra uma [função C#](functions-dotnet-class-library.md) que registra o corpo da mensagem do gatilho dos hub de eventos.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
{
    log.LogInformation($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Para obter acesso aos [metadados](#trigger---event-metadata) no código da função, associe um objeto de [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) (requer o uso de uma instrução para `Microsoft.Azure.EventHubs`). Você também pode acessar as mesmas propriedades usando expressões de associação na assinatura do método.  O exemplo a seguir mostra duas formas de obter os mesmos dados:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run(
    [EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData myEventHubMessage,
    DateTime enqueuedTimeUtc,
    Int64 sequenceNumber,
    string offset,
    ILogger log)
{
    log.LogInformation($"Event: {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
    // Metadata accessed by binding to EventData
    log.LogInformation($"EnqueuedTimeUtc={myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.LogInformation($"SequenceNumber={myEventHubMessage.SystemProperties.SequenceNumber}");
    log.LogInformation($"Offset={myEventHubMessage.SystemProperties.Offset}");
    // Metadata accessed by using binding expressions in method parameters
    log.LogInformation($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.LogInformation($"SequenceNumber={sequenceNumber}");
    log.LogInformation($"Offset={offset}");
}
```

Para receber eventos em um lote, faça `string` ou `EventData` uma matriz.  

> [!NOTE]
> Ao receber em um lote, não é possível associar aos parâmetros do método, como no exemplo acima com `DateTime enqueuedTimeUtc`, e deve recebê-los de cada objeto `EventData`  

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData[] eventHubMessages, ILogger log)
{
    foreach (var message in eventHubMessages)
    {
        log.LogInformation($"C# Event Hub trigger function processed a message: {Encoding.UTF8.GetString(message.Body)}");
        log.LogInformation($"EnqueuedTimeUtc={message.SystemProperties.EnqueuedTimeUtc}");
    }
}
```

### <a name="trigger---c-script-example"></a>Gatilho - exemplo de script C#

O exemplo a seguir mostra uma associação de gatilho de hub de eventos em um arquivo *function.json* e uma [função C# script](functions-reference-csharp.md) que usa a associação. A função registra o corpo da mensagem do gatilho de hub de eventos.

Os exemplos a seguir mostram dados de associação de Hubs de eventos no *function.json* arquivo.

#### <a name="version-2x"></a>Versão 2.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

#### <a name="version-1x"></a>Versão 1.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Aqui está o código de script do C#:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Para obter acesso aos [metadados](#trigger---event-metadata) no código da função, associe um objeto de [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) (requer o uso de uma instrução para `Microsoft.Azure.EventHubs`). Você também pode acessar as mesmas propriedades usando expressões de associação na assinatura do método.  O exemplo a seguir mostra duas formas de obter os mesmos dados:

```cs
#r "Microsoft.Azure.EventHubs"

using System.Text;
using System;
using Microsoft.ServiceBus.Messaging;
using Microsoft.Azure.EventHubs;

public static void Run(EventData myEventHubMessage,
    DateTime enqueuedTimeUtc,
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SystemProperties.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.SystemProperties.Offset}");

    // Metadata accessed by using binding expressions
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"SequenceNumber={sequenceNumber}");
    log.Info($"Offset={offset}");
}
```

Para receber eventos em um lote, faça `string` ou `EventData` uma matriz:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---f-example"></a>Gatilho - exemplo F#

O exemplo a seguir mostra uma associação de gatilho de hub de eventos em um arquivo *function.json* e uma [função F#](functions-reference-fsharp.md) que usa a associação. A função registra o corpo da mensagem do gatilho de hub de eventos.

Os exemplos a seguir mostram dados de associação de Hubs de eventos no *function.json* arquivo. 

#### <a name="version-2x"></a>Versão 2.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

#### <a name="version-1x"></a>Versão 1.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

O código F# é o seguinte:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Log(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Gatilho - exemplo de JavaScript

O exemplo a seguir mostra uma associação de gatilho de hub de eventos em um arquivo *function.json* e uma [função JavaScript](functions-reference-node.md) que usa a associação. A função lê os [metadados de evento](#trigger---event-metadata) e registra a mensagem.

Os exemplos a seguir mostram dados de associação de Hubs de eventos no *function.json* arquivo.

#### <a name="version-2x"></a>Versão 2.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

#### <a name="version-1x"></a>Versão 1.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Aqui está o código JavaScript:

```javascript
module.exports = function (context, eventHubMessage) {
    context.log('Event Hubs trigger function processed message: ', myEventHubMessage);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('SequenceNumber =', context.bindingData.sequenceNumber);
    context.log('Offset =', context.bindingData.offset);

    context.done();
};
```

Para receber eventos em um lote, defina `cardinality` para `many` no arquivo *function.json*, conforme mostrado nos exemplos a seguir.

#### <a name="version-2x"></a>Versão 2.x

```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

#### <a name="version-1x"></a>Versão 1.x

```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "path": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Aqui está o código JavaScript:

```javascript
module.exports = function (context, eventHubMessages) {
    context.log(`JavaScript eventhub trigger function called for message array ${eventHubMessages}`);

    eventHubMessages.forEach((message, index) => {
        context.log(`Processed message ${message}`);
        context.log(`EnqueuedTimeUtc = ${context.bindingData.enqueuedTimeUtcArray[index]}`);
        context.log(`SequenceNumber = ${context.bindingData.sequenceNumberArray[index]}`);
        context.log(`Offset = ${context.bindingData.offsetArray[index]}`);
    });

    context.done();
};
```

### <a name="trigger---python-example"></a>Gatilho – Exemplo do Python

O exemplo a seguir mostra uma ligação de acionador do Hub de Eventos em um arquivo *function.json* e uma [função Python](functions-reference-python.md) que usa a ligação. A função lê os [metadados de evento](#trigger---event-metadata) e registra a mensagem.

Os exemplos a seguir mostram dados de associação de Hubs de eventos no *function.json* arquivo.

```json
{
  "type": "eventHubTrigger",
  "name": "event",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Aqui está o código Python:

```python
import logging
import azure.functions as func

def main(event: func.EventHubEvent):
    logging.info('Event Hubs trigger function processed message: ', event.get_body())
    logging.info('  EnqueuedTimeUtc =', event.enqueued_time)
    logging.info('  SequenceNumber =', event.sequence_number)
    logging.info('  Offset =', event.offset)
```

### <a name="trigger---java-example"></a>Gatilho - exemplo Java

O exemplo a seguir mostra uma ligação de acionador do Hub de Eventos em um arquivo *function.json* e uma [função Java](functions-reference-java.md) que usa a ligação. A função registra o corpo da mensagem do gatilho de Hub de eventos.

```json
{
  "type": "eventHubTrigger",
  "name": "msg",
  "direction": "in",
  "eventHubName": "myeventhubname",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

```java
@FunctionName("ehprocessor")
public void eventHubProcessor(
  @EventHubTrigger(name = "msg",
                  eventHubName = "myeventhubname",
                  connection = "myconnvarname") String message,
       final ExecutionContext context )
       {
          context.getLogger().info(message);
 }
 ```

 No [biblioteca de tempo de execução de funções Java](/java/api/overview/azure/functions/runtime), use o `EventHubTrigger` anotação em parâmetros cujo valor virá do Hub de eventos. Parâmetros com essas anotações fazem com que a função seja executada quando um evento chega.  Essa anotação pode ser usada com tipos Java nativos, POJOs ou valores anuláveis usando <T>Optional.

## <a name="trigger---attributes"></a>Gatilho – atributos

Em [bibliotecas de classes de C#](functions-dotnet-class-library.md), utilize o atributo [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs/EventHubTriggerAttribute.cs).

O construtor do atributo usa os nomes do hub de eventos, do grupo de consumidores e de uma configuração de aplicativo que contenham a cadeia de caracteres de conexão. Para obter mais informações sobre essas configurações, consulte a [seção sobre configuração do gatilho](#trigger---configuration). Este é um `EventHubTriggerAttribute` exemplo de atributo:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
{
    ...
}
```

Para ver um exemplo completo, consulte [Gatilho – exemplo de C#](#trigger---c-example).

## <a name="trigger---configuration"></a>Gatilho – configuração

A tabela a seguir explica as propriedades de configuração de associação que você define no arquivo *function.json* e no atributo `EventHubTrigger`.

|Propriedade function.json | Propriedade de atributo |DESCRIÇÃO|
|---------|---------|----------------------|
|**tipo** | n/d | Deve ser definido como `eventHubTrigger`. Essa propriedade é definida automaticamente quando você cria o gatilho no portal do Azure.|
|**direction** | n/d | Deve ser definido como `in`. Essa propriedade é definida automaticamente quando você cria o gatilho no portal do Azure. |
|**name** | n/d | O nome da variável que representa o item de evento no código de função. |
|**path** |**EventHubName** | Funciona apenas 1. x. O nome do hub de eventos. Quando o nome do hub de eventos também estiver presente na cadeia de conexão, esse valor substitui essa propriedade em tempo de execução. |
|**eventHubName** |**EventHubName** | Funciona apenas 2. x. O nome do hub de eventos. Quando o nome do hub de eventos também estiver presente na cadeia de conexão, esse valor substitui essa propriedade em tempo de execução. |
|**consumerGroup** |**ConsumerGroup** | É uma propriedade opcional que define o [grupo de consumidores ](../event-hubs/event-hubs-features.md#event-consumers) usado para assinar eventos no hub. Se omitido, o grupo de consumidores `$Default` será usado. |
|**cardinalidade** | n/d | Para JavaScript. Definido como `many` para habilitar o envio em lote.  Se omitido ou definido como `one`, uma única mensagem será passada para a função. |
|**conexão** |**Conexão** | É o nome de uma configuração de aplicativo que contém a cadeia de conexão para o namespace do hub de eventos. Copie essa cadeia de conexão clicando no botão **Informações de Conexão** do [namespace](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace), não no próprio hub de eventos. Essa cadeia de conexão deve ter, pelo menos, permissões de leitura para ativar o gatilho.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---event-metadata"></a>Gatilho: metadados de evento

O gatilho dos Hubs de Evento fornece várias propriedades de [metadados](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Essas propriedades podem ser usadas como parte de expressões de associação em outras associações ou como parâmetros em seu código. Essas são propriedades da classe [EventData](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventdata).

|Propriedade|Tipo|DESCRIÇÃO|
|--------|----|-----------|
|`PartitionContext`|[PartitionContext](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.partitioncontext)|A instância `PartitionContext`.|
|`EnqueuedTimeUtc`|`DateTime`|O tempo de enfileiramento no UTC.|
|`Offset`|`string`|O deslocamento dos dados em relação ao fluxo de partição do Hub de Eventos. O deslocamento é um marcador ou um identificador para um evento dentro do fluxo do Hubs de Eventos. O identificador é exclusivo dentro de uma partição do fluxo de Hubs de Eventos.|
|`PartitionKey`|`string`|A partição para os dados de evento deve ser enviada.|
|`Properties`|`IDictionary<String,Object>`|Propriedades do usuário dos dados do evento.|
|`SequenceNumber`|`Int64`|O número de sequência lógica do evento.|
|`SystemProperties`|`IDictionary<String,Object>`|Propriedades do sistema, incluindo dos dados do evento.|

Consulte [exemplos de código](#trigger---example) que usam essas propriedades neste artigo.

## <a name="trigger---hostjson-properties"></a>Gatilho - propriedades de host.json

O arquivo [host.json](functions-host-json.md#eventhub) contém configurações que controlam o comportamento de gatilho dos Hubs de Eventos.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Saída

Use a associação de saída dos Hubs de Eventos para gravar eventos em um fluxo de eventos. É necessário ter permissão de envio para um hub de eventos a fim de gravar eventos nele.

Verifique se as referências de pacote necessárias foram implementadas: [Functions 1.x](#packages---functions-1.x) ou [Functions 2.x](#packages---functions-2.x)

## <a name="output---example"></a>Saída - exemplo

Consulte o exemplo específico a um idioma:

* [C#](#output---c-example)
* [Script do C# (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [Java](#output---java-example)
* [JavaScript](#output---javascript-example)
* [Python](#output---python-example)

### <a name="output---c-example"></a>Saída - exemplo C#

O exemplo a seguir mostra uma [função C#](functions-dotnet-class-library.md) que grava uma mensagem em um hub de eventos, usando o valor retornado do método como a saída:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

### <a name="output---c-script-example"></a>Saída - exemplo de script C#

O exemplo a seguir mostra uma associação de gatilho de hub de eventos em um arquivo *function.json* e uma [função C# script](functions-reference-csharp.md) que usa a associação. A função grava uma mensagem em um hub de eventos.

Os exemplos a seguir mostram dados de associação de Hubs de eventos no *function.json* arquivo. O primeiro exemplo é para funções de 2.x e a segunda é para funções 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Este é o código C# script que cria uma mensagem:

```cs
using System;
using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, ILogger log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.LogInformation(msg);   
    outputEventHubMessage = msg;
}
```

Este é o código de script C# que cria várias mensagens:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, ILogger log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.LogInformation(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Saída - Exemplo #F

O exemplo a seguir mostra uma associação de gatilho de hub de eventos em um arquivo *function.json* e uma [função F#](functions-reference-fsharp.md) que usa a associação. A função grava uma mensagem em um hub de eventos.

Os exemplos a seguir mostram dados de associação de Hubs de eventos no *function.json* arquivo. O primeiro exemplo é para funções de 2.x e a segunda é para funções 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

O código F# é o seguinte:

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: ILogger) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.LogInformation(msg);
    outputEventHubMessage <- msg;
```

### <a name="output---javascript-example"></a>Saída - exemplo JavaScript

O exemplo a seguir mostra uma associação de gatilho de hub de eventos em um arquivo *function.json* e uma [função JavaScript](functions-reference-node.md) que usa a associação. A função grava uma mensagem em um hub de eventos.

Os exemplos a seguir mostram dados de associação de Hubs de eventos no *function.json* arquivo. O primeiro exemplo é para funções de 2.x e a segunda é para funções 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Este é o código JavaScript que envia uma mensagem única:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Este é o código JavaScript que envia várias mensagens:

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

### <a name="output---python-example"></a>Saída – exemplo do Python

O exemplo a seguir mostra uma ligação de acionador do Hub de Eventos em um arquivo *function.json* e uma [função Python](functions-reference-python.md) que usa a ligação. A função grava uma mensagem em um hub de eventos.

Os exemplos a seguir mostram dados de associação de Hubs de eventos no *function.json* arquivo.

```json
{
    "type": "eventHub",
    "name": "$return",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Este é o código Python que envia uma mensagem única:

```python
import datetime
import logging
import azure.functions as func

def main(timer: func.TimerRequest) -> str:
    timestamp = datetime.datetime.utcnow()
    logging.info('Event Hub message created at: %s', timestamp);   
    return 'Event Hub message created at: {}'.format(timestamp)
```

### <a name="output---java-example"></a>Saída - exemplo de Java

O exemplo a seguir mostra uma função Java que grava uma mensagem contendo a hora atual em um Hub de Eventos.

```java
@}FunctionName("sendTime")
@EventHubOutput(name = "event", eventHubName = "samples-workitems", connection = "AzureEventHubConnection")
public String sendTime(
   @TimerTrigger(name = "sendTimeTrigger", schedule = "0 *&#47;5 * * * *") String timerInfo)  {
     return LocalDateTime.now().toString();
 }
 ```

No [biblioteca de tempo de execução de funções Java](/java/api/overview/azure/functions/runtime), use o `@EventHubOutput` anotação em parâmetros cujo valor seria poublished ao Hub de eventos.  O parâmetro deve ser do tipo `OutputBinding<T>`, em que T é um POJO ou qualquer tipo Java nativo.

## <a name="output---attributes"></a>Saída - atributos

Para [bibliotecas de classes de C#](functions-dotnet-class-library.md), utilize o atributo [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs).

O construtor do atributo usa os nomes do hub de eventos e de uma configuração de aplicativo que contenham a cadeia de caracteres de conexão. Para obter mais informações sobre essas configurações, consulte [Saída - configuração](#output---configuration). Este é um `EventHub` exemplo de atributo:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    ...
}
```

Para ver um exemplo completo, consulte [Saída – exemplo de C#](#output---c-example).

## <a name="output---configuration"></a>Saída - configuração

A tabela a seguir explica as propriedades de configuração de associação que você define no arquivo *function.json* e no `EventHub` atributo.

|Propriedade function.json | Propriedade de atributo |DESCRIÇÃO|
|---------|---------|----------------------|
|**tipo** | n/d | Deve ser definido como "eventHub". |
|**direction** | n/d | Deve ser definido como "out". Esse parâmetro é definido automaticamente quando você cria a associação no portal do Azure. |
|**name** | n/d | É o nome da variável usada no código da função que representa o evento. |
|**path** |**EventHubName** | Funciona apenas 1. x. O nome do hub de eventos. Quando o nome do hub de eventos também estiver presente na cadeia de conexão, esse valor substitui essa propriedade em tempo de execução. |
|**eventHubName** |**EventHubName** | Funciona apenas 2. x. O nome do hub de eventos. Quando o nome do hub de eventos também estiver presente na cadeia de conexão, esse valor substitui essa propriedade em tempo de execução. |
|**conexão** |**Conexão** | É o nome de uma configuração de aplicativo que contém a cadeia de conexão para o namespace do hub de eventos. Copie essa cadeia de conexão clicando no botão **Informações de Conexão** do *namespace*, não no próprio hub de eventos. Essa cadeia de conexão deve ter permissões de envio para enviar a mensagem à transmissão do evento.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Saída - uso

Em C# e C# script, envie mensagens usando um parâmetro de método, como `out string paramName`. No script do C#, `paramName` é o valor especificado na propriedade `name` de *function.json*. Para gravar várias mensagens, você pode usar `ICollector<string>` ou `IAsyncCollector<string>` no lugar de `out string`.

Em JavaScript, acesse o evento de saída usando `context.bindings.<name>`. `<name>` é o valor especificado na propriedade `name` de *function.json*.

## <a name="exceptions-and-return-codes"></a>Exceções e códigos de retorno

| Associação | Referência |
|---|---|
| Hub de evento | [Guia de Operações](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>configurações de host.json

Esta seção descreve as definições de configuração globais disponíveis para esta associação na versão 2.x. O arquivo host.json de exemplo abaixo contém apenas as configurações da versão 2.x para essa associação. Para obter mais informações sobre as configurações globais na versão 2.x, confira a [referência de host.json para o Azure Functions versão 2.x](functions-host-json.md).

> [!NOTE]
> Para obter uma referência de host.json no Functions 1.x, confira [Referência de host.json para o Azure Functions 1.x](functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "eventHubs": {
            "batchCheckpointFrequency": 5,
            "eventProcessorOptions": {
                "maxBatchSize": 256,
                "prefetchCount": 512
            }
        }
    }
}  
```  

|Propriedade  |Padrão | DESCRIÇÃO |
|---------|---------|---------| 
|maxBatchSize|64|A contagem máxima de eventos recebidos por loop de recebimento.|
|prefetchCount|n/d|O PrefetchCount padrão que será usado pelo EventProcessorHost subjacente.| 
|batchCheckpointFrequency|1|O número de lotes de eventos para processar antes de criar um ponto de verificação do cursor do EventHub.| 

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Aprenda mais sobre gatilhos e de associações do Azure Functions](functions-triggers-bindings.md)
