---
title: Usando token de insights – Pesquisa Visual do Bing
titleSuffix: Azure Cognitive Services
description: Mostra como usar o token de insights de uma imagem com a API da Pesquisa Visual do Bing para obter insights sobre uma imagem.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: conceptual
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 76fb61f0f1dd3c1f72c4c6132d1bfdc8b516477a
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52955540"
---
# <a name="using-an-insights-token-to-get-insights-about-an-image"></a>Usar um token de insights para obter insights sobre uma imagem

A API de Pesquisa Visual do Bing retorna informações sobre uma imagem que você fornece. Você pode fornecer a imagem usando a URL da imagem, um token de insights ou carregando uma imagem. Para obter informações sobre essas opções, consulte [O que é a API de Pesquisa Visual do Bing?](overview.md). Este artigo demonstra como usar um token de insights. Para exemplos que demonstram upload de uma imagem para obter insights, consulte os inícios rápidos do ([C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [Node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md)).


Se você enviar uma URL ou um token de imagem à Pesquisa Visual, inclua os dados de formulário mostrados a seguir no corpo do POST. Os dados de formulário devem incluir o cabeçalho Conteúdo-Disposição e o parâmetro `name` deve ser definido como "knowledgeRequest". Para obter detalhes sobre o objeto `imageInfo`, consulte [A solicitação](#the-request).

```json
{
    "imageInfo" : {
        "url" : "",
        "imageInsightsToken" : "",
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.5,
            "right" : 0.9,
            "bottom" : 0.9
        }
    },
    "knowledgeRequest" : {
      "filters" : {
        "site" : ""
      }
    }
}
```

Os exemplos neste artigo mostram como usar o token de insights. Você obtém o token de insights de um objeto de imagem em uma resposta de API /images/search. Para obter informações sobre como obter o token de insights, consulte [API de Pesquisa de Imagem do Bing](../Bing-Image-Search/overview.md).

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "imageInsightsToken" : "ccid_tmaGQ2eU*mid_D12339146CFEDF3D40...",
    }
}

--boundary_1234-abcd--
```


Para exemplos que usam o token de insights, consulte [C#](#using-csharp) | [Java](#using-java) | [Node.js](#using-nodejs) | [Python](#using-python).

<a name="using-csharp" />

## <a name="using-c"></a>Usando C#

### <a name="prerequisites"></a>Pré-requisitos

Você precisará do [Visual Studio 2017](https://www.visualstudio.com/downloads/) para obter esse código em execução no Windows. (O Community Edition gratuito funcionará.)

Para este guia de início rápido, você pode usar uma chave de assinatura de [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ou uma chave de assinatura paga.

## <a name="running-the-application"></a>Executando o aplicativo

Para executar este aplicativo, siga estas etapas:

1. Crie uma nova solução de Console no Visual Studio.
1. Substitua o conteúdo de `Program.cs` pelo código mostrado neste guia de início rápido.
2. Substitua o valor `accessKey` pela sua chave de assinatura.
2. Substitua o valor `insightsToken` por um símbolo de insights de uma resposta /images/search.
3. Execute o programa.

```csharp
using System;
using System.Text;
using System.Net;
using System.IO;
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using System.Threading;

namespace VisualSearchInsightsToken
{

    class Program
    {
        // Replace the accessKey string value with your valid subscription key.
        const string accessKey = "<yoursubscriptionkeygoeshere>";

        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";

        // Update with an insights token from an image object that the /images/search API endpoint returns.
        static string insightsToken = @"ccid_tmaGQ2eU*mid_D12339146CFEDF3D409CC7A66D2C98D0D71904D4*simid_608022145667564759*thid_OIP.tmaGQ2eUI1yq3yll!_jn9kwHaFZ";

        static string BoundaryTemplate = "batch_{0}";

        static void Main(string[] args)
        {
            try { 
                Console.WriteLine("Getting image insights using insights token: " + insightsToken);

                var knowledgeRequest = "{\"imageInfo\" : {\"imageInsightsToken\" : \"" + insightsToken + "\"}}";
                var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());

                var json = BingVisualSearch(knowledgeRequest, boundary, uriBase, accessKey);

                Console.WriteLine("\nJSON Response:\n");
                Console.WriteLine(JsonPrettyPrint(json));

                Console.Write("\nPress Enter to exit ");
                Console.ReadLine();
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }

        }


        /// <summary>
        /// Calls the Bing visual search endpoint and returns the JSON response with insights.
        /// </summary>
        static string BingVisualSearch(string knowledgeRequest, string boundary, string uri, string subscriptionKey)
        {
            var requestMessage = new HttpRequestMessage(HttpMethod.Post, uri);
            requestMessage.Headers.Add("Ocp-Apim-Subscription-Key", accessKey);

            var content = new MultipartFormDataContent(boundary);
            content.Add(new StringContent(knowledgeRequest, Encoding.UTF8, "application/json"), "knowledgeRequest");
            requestMessage.Content = content;

            var httpClient = new HttpClient();

            Task<HttpResponseMessage> httpRequest = httpClient.SendAsync(requestMessage, HttpCompletionOption.ResponseContentRead, CancellationToken.None);
            HttpResponseMessage httpResponse = httpRequest.Result;
            HttpStatusCode statusCode = httpResponse.StatusCode;
            HttpContent responseContent = httpResponse.Content;

            string json = null;

            if (responseContent != null)
            {
                Task<String> stringContentsTask = responseContent.ReadAsStringAsync();
                json = stringContentsTask.Result;
            }


            return json;
        }


        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            char last = ' ';
            int offset = 0;
            int indentLength = 2;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\\':
                        if (quote && last != '\\') ignore = true;
                        break;
                }

                if (quote)
                {
                    sb.Append(ch);
                    if (last == '\\' && ignore) ignore = false;
                }
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (quote || ch != ' ') sb.Append(ch);
                            break;
                    }
                }
                last = ch;
            }

            return sb.ToString().Trim();
        }
    }
}
```

<a name="using-java" />

## <a name="using-java"></a>Usando Java

### <a name="prerequisites"></a>Pré-requisitos

É necessário ter o [JDK 7 ou 8](https://aka.ms/azure-jdks) para compilar e executar esse código. Você poderá usar um Java IDE, se tiver um favorito, mas um editor de texto é suficiente.

Para este guia de início rápido, você pode usar uma chave de assinatura de [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ou uma chave de assinatura paga.

## <a name="running-the-application"></a>Executando o aplicativo

Para executar este aplicativo, siga estas etapas:

1. Baixe ou instale a [biblioteca gson](https://github.com/google/gson). Você também pode obtê-la por meio do Maven.
2. Crie um projeto Java em seu IDE ou editor favorito.
3. Adicione o código fornecido em um arquivo nomeado `VisualSearch.java`.
4. Substitua o valor `subscriptionKey` pela sua chave de assinatura.
5. Execute o programa.

```java
package insightstoken;

import java.util.*;
import java.io.*;



/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.2
 *
 * Once you have compiled or downloaded gson-2.8.2.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac BingImageSearch.java -classpath .;gson-2.8.2.jar -encoding UTF-8
 * java -cp .;gson-2.8.2.jar BingImageSearch
 */

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

// http://hc.apache.org/downloads.cgi (HttpComponents Downloads) HttpClient 4.5.5

import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;


public class InsightsToken {

    
    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "<yoursubscriptionkeygoeshere>";

    // To get an insights, call the /images/search endpoint. Get the token from
    // the imageInsightsToken field in the Image object.
    static String insightsToken = "ccid_tmaGQ2eU*mid_D12339146CFEDF3D409CC7A66D2C98D0D71904D4*simid_608022145667564759*thid_OIP.tmaGQ2eUI1yq3yll!_jn9kwHaFZ";

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        
        String knowledgeRequest = "{\"imageInfo\" : {\"imageInsightsToken\" : \"" + insightsToken + "\"}}";

        try {
            HttpEntity entity = MultipartEntityBuilder
                .create()
                .addTextBody("knowledgeRequest", knowledgeRequest, ContentType.create("application/json"))
                .build();

            HttpPost httpPost = new HttpPost(endpoint);
            httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            httpPost.setEntity(entity);
            HttpResponse response = httpClient.execute(httpPost);

            InputStream stream = response.getEntity().getContent();
            String json = new Scanner(stream).useDelimiter("\\A").next();

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(json));
        }
        catch (IOException e)
        {
            e.printStackTrace(System.out);
            System.exit(1);
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }
    
    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    
}
```


<a name="using-nodejs" />

## <a name="using-nodejs"></a>Usando o Node.js

### <a name="prerequisites"></a>Pré-requisitos

É necessário ter o [Node.js 6](https://nodejs.org/en/download/) para executar esse código.

Para este guia de início rápido, você pode usar uma chave de assinatura de [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ou uma chave de assinatura paga.

## <a name="running-the-application"></a>Executando o aplicativo

Para executar este aplicativo, siga estas etapas:

1. Crie uma pasta para o seu projeto (ou use seu IDE ou editor favorito).
2. Em um prompt de comando ou terminal, navegue até a pasta que você acabou de criar.
3. Instale os módulos de solicitação:  
  ```  
  npm install request  
  ```  
3. Instale os módulos de dados do formulário:  
  ```  
  npm install form-data  
  ```  
4. Crie um arquivo nomeado GetVisualInsights.js e adicione o seguinte código a ele.
5. Substitua o valor `subscriptionKey` pela sua chave de assinatura.
7. Execute o programa.  
  ```
  node GetVisualInsights.js
  ```

```javascript
var request = require('request');
var FormData = require('form-data');

var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
var subscriptionKey = '<yoursubscriptionkeygoeshere>';

// To get an insights, call the /images/search endpoint. Get the token from
// the imageInsightsToken field in the Image object.
var insightsToken = "ccid_tmaGQ2eU*mid_D12339146CFEDF3D409CC7A66D2C98D0D71904D4*simid_608022145667564759*thid_OIP.tmaGQ2eUI1yq3yll!_jn9kwHaFZ";

var knowledgeRequest = {
    "imageInfo" : {
        "imageInsightsToken" : insightsToken
    }
};

var form = new FormData();
form.append('knowledgeRequest', JSON.stringify(knowledgeRequest));

form.getLength(function(err, length){
  if (err) {
    return requestCallback(err);
  }

  var r = request.post(baseUri, requestCallback);
  r._form = form; 
  r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
});

function requestCallback(err, res, body) {
    console.log(JSON.stringify(JSON.parse(body), null, '  '))
}
```


<a name="using-python" />

## <a name="using-python"></a>Usando Python


### <a name="prerequisites"></a>Pré-requisitos

Você precisa do [Python 3](https://www.python.org/) para executar esse código.

Para este guia de início rápido, você pode usar uma chave de assinatura de [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ou uma chave de assinatura paga.

## <a name="running-the-walkthrough"></a>Executando o passo a passo

Para executar este aplicativo, siga estas etapas:

1. Crie um novo projeto Python no seu IDE ou editor favorito.
2. Crie um arquivo nomeado visualsearch.py e adicione o código mostrado neste guia de início rápido.
3. Substitua o valor `SUBSCRIPTION_KEY` pela sua chave de assinatura.
4. Execute o programa.


```python
"""Bing Visual Search example"""

# Download and install Python at https://www.python.org/
# Run the following in a command console window
# pip3 install requests

import requests, json

BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'

SUBSCRIPTION_KEY = '<yoursubscriptionkeygoeshere>'

HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}

# To get an insights, call the /images/search endpoint. Get the token from
# the imageInsightsToken field in the Image object.
insightsToken = 'ccid_tmaGQ2eU*mid_D12339146CFEDF3D409CC7A66D2C98D0D71904D4*simid_608022145667564759*thid_OIP.tmaGQ2eUI1yq3yll!_jn9kwHaFZ'

formData = '{"imageInfo":{"imageInsightsToken":"' + insightsToken + '"}}'


file = {'knowledgeRequest' : (None, formData)}

def main():
    
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())

    except Exception as ex:
        raise ex


def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))



# Main execution
if __name__ == '__main__':
    main()
```

## <a name="next-steps"></a>Próximas etapas

[Tutorial do aplicativo de página única da Pesquisa Visual do Bing](tutorial-bing-visual-search-single-page-app.md)  
[Visão geral da Pesquisa Visual do Bing](overview.md)  
[Experimente](https://aka.ms/bingvisualsearchtryforfree)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Referência de API de Pesquisa Visual do Bing](https://aka.ms/bingvisualsearchreferencedoc)
