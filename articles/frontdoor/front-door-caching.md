---
title: Azure Front Door Service - cache | Microsoft Docs
description: Este artigo ajuda você a entender como o Azure Front Door Service monitora a integridade dos back-ends
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 97c02726c7e359195c6bf4ea793404562f2acccf
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/06/2018
ms.locfileid: "53001957"
---
# <a name="caching-with-azure-front-door-service"></a>Cache com o Azure Front Door Service
O documento a seguir especifica o comportamento do Azure Front Door Service com as regras de roteamento que tem o cache habilitado.

## <a name="delivery-of-large-files"></a>Entrega de arquivos grandes
O Azure Front Door Service entrega arquivos grandes sem um limite no tamanho do arquivo. O Front Door usa uma técnica chamada agrupamento de objeto. Quando um arquivo grande é solicitado, o Front Door recupera partes menores do arquivo a partir do back-end. Após receber uma solicitação de arquivo completo ou de intervalo de bytes, um ambiente do Front Door solicitará o arquivo do back-end em partes de 8 MB.

</br>Depois que a parte chega no ambiente do Front Door, ela é armazenada em cache e imediatamente distribuída para o usuário. Em seguida, o Front Door efetua a pré-busca da próxima parte em paralelo. Essa pré-busca garante que o conteúdo permaneça uma parte à frente do usuário, o que reduz a latência. Esse processo continua até que todo o arquivo é baixado (se solicitado), todos os intervalos de bytes estão disponíveis (se solicitado) ou o cliente encerra a conexão.

</br>Para obter mais informações sobre a solicitação de intervalo de bytes, leia [RFC 7233](https://web.archive.org/web/20171009165003/ http://www.rfc-base.org/rfc-7233.html).
O Front Door armazena em cache todas as partes conforme são recebidas e, portanto, o arquivo inteiro não precisa ser armazenado no cache do Front Door. As solicitações subsequentes para o arquivo ou intervalos de bytes são disponibilizadas pela cache. Se nem todas as partes forem armazenadas em cache, a pré-busca será usada para solicitar as partes do back-end. Essa otimização depende da capacidade do servidor de origem para dar suporte a solicitações de intervalo de bytes; se o servidor de origem não dá suporte a solicitações de intervalo de bytes, essa otimização não será efetiva.

## <a name="file-compression"></a>Compactação de arquivos
O Front Door pode compactar dinamicamente conteúdo na borda, resultando em uma resposta menor e mais rápida aos clientes. Todos os arquivos são qualificados para compactação. No entanto, o arquivo deve ser um tipo MIME que seja qualificado para a lista de compactação. Atualmente, o Front Door não permite que essa lista seja alterada. A lista atual é:</br>
- "application/eot"
- "application/font"
- "application/font-sfnt"
- "application/javascript"
- “application/json”
- "application/opentype"
- "application/otf"
- "application/pkcs7-mime"
- "application/truetype"
- "application/ttf",
- "application/vnd.ms-fontobject"
- "application/xhtml+xml"
- "application/xml"
- "application/xml+rss"
- "application/x-font-opentype"
- "application/x-font-truetype"
- "application/x-font-ttf"
- "application/x-httpd-cgi"
- "application/x-mpegurl"
- "application/x-opentype"
- "application/x-otf"
- "application/x-perl"
- "application/x-ttf"
- "application/x-javascript"
- "font/eot"
- "font/ttf"
- "font/otf"
- "font/opentype"
- "image/svg+xml"
- "text/css"
- "text/csv"
- "text/html"
- "text/javascript"
- "text/js", "text/plain"
- "text/richtext"
- "text/tab-separated-values"
- "text/xml"
- "text/x-script"
- "text/x-component"
- "text/x-java-source"

Além disso, o arquivo também deve ter entre 1 KB e 8 MB de tamanho, inclusive.

Esses perfis dão suporte às seguintes codificações de compactação:
- [Gzip (GNU zip)](https://en.wikipedia.org/wiki/Gzip)
- [Brotli](https://en.wikipedia.org/wiki/Brotli)

Se uma solicitação com suporte para gzip e compactação Brotli, a compactação Brotli terá precedência.</br>
Quando uma solicitação de um ativo especificar a compactação e os resultados da solicitação em um cache forem perdidos, o Front Door realiza a compactação do ativo diretamente no servidor POP. Depois disso, o arquivo compactado será servido do cache. O item resultante é retornado com uma codificação de transferência: em partes.

## <a name="query-string-behavior"></a>Comportamento da cadeia de caracteres de consulta
Com o Front Door, é possível controlar como os arquivos são armazenados em cache para uma solicitação da Web que contenha uma cadeia de caracteres de consulta. Em uma solicitação da Web com uma cadeia de caracteres de consulta, a cadeia de caracteres de consulta é aquela parte da solicitação que ocorre após um ponto de interrogação (?). Uma cadeia de caracteres de consulta pode conter um ou mais pares de chave-valor, no qual o nome do campo e seu valor são separados por um sinal de igual (=). Cada par chave-valor é separado por um e comercial (&). Por exemplo, http://www.contoso.com/content.mov?field1=value1&field2=value2. Se houver mais de um par chave-valor em uma cadeia de caracteres de consulta de uma solicitação, a ordem não importa.
- **Ignorar as cadeias de caracteres de consulta**: Modo padrão. Neste modo, o Front Door passa as cadeias de caracteres de consulta do solicitante para a origem na primeira solicitação e armazena em cache o ativo. Todas as solicitações subsequentes para esse ativo que forem atendidas pelo ambiente do Front Door ignoram a cadeia de caracteres de consulta até que o ativo em cache expire.

- **Armazenar em cache todas as URLs exclusivas**: Nesse modo, cada solicitação com um URL exclusiva, incluindo a cadeia de caracteres de consulta, é tratada como um ativo exclusivo com seu próprio cache. Por exemplo, a resposta do servidor de back-end para uma solicitação de `www.example.ashx?q=test1` é armazenada em cache no Front Door e retornada para caches subsequentes com a mesma cadeia de caracteres de consulta. Uma solicitação `www.example.ashx?q=test2` é armazenada em cache como um ativo separado com sua própria configuração de vida útil.

## <a name="cache-purge"></a>Limpeza do cache
O Front Door armazenará em cache os ativos até a TTL (vida útil) do ativo expirar. Após a TTL do ativo expirar, quando um cliente solicitar o ativo, o ambiente do Front Door irá recuperar uma nova cópia atualizada do ativo para atender à solicitação do cliente e atualizar o armazenamento da cache.
</br>A prática recomendada para garantir que os usuários sempre obtenham a cópia mais recente de seus ativos é verter os ativos para cada atualização e publicá-los como novas URLs. O Front Door recuperará imediatamente os novos ativos para as próximas solicitações do cliente. Às vezes, convém limpar o conteúdo em cache de todos os nós de borda e forçá-los a recuperar novos ativos atualizados. Isso pode ocorrer devido a atualizações do aplicativo Web ou para atualizar rapidamente ativos que contenham informações incorretas.

</br>Selecione os ativos que você deseja limpar dos nós de borda. Para limpar todos os ativos, clique na caixa de seleção Limpar todos. Caso contrário, digite o caminho de cada ativo que você quer limpar na caixa de texto Caminho. Veja abaixo os formatos com suporte no caminho.
1. **Limpeza de URL única**: Limpe o ativo individual especificando a URL completa, com a extensão de arquivo, por exemplo, /pictures/strasbourg.png;
2. **Limpeza de caracteres curinga**: O asterisco (\*) pode ser usado como um caractere curinga. Limpe todos os arquivos, pastas e subpastas em um ponto de extremidade com /\* no caminho ou limpe todos os arquivos e subpastas em uma determinada pasta especificando a pasta seguida por /\*, por exemplo, /pictures/\*.
3. **Limpeza do domínio raiz**: Limpe a raiz do ponto de extremidade com "/" no caminho.

As limpezas de cache do Front Door diferenciam maiúsculas de minúsculas. Além disso, as limpezas são independentes da cadeia de caracteres de consulta, significando que a limpeza de uma URL apagará todas as variações da cadeia de caracteres de consulta dessa URL. 

## <a name="cache-expiration"></a>Expiração do cache
A ordem de cabeçalhos a seguir é usada para determinar por quanto tempo um item será armazenado em nosso cache:</br>
1. Cache-Control: s-maxage=<seconds>
2. Cache-Control: maxage=<seconds>
3. Expires: <http-date>

Cabeçalhos de resposta de Cache-Control que indicam que a resposta não será ser armazenada em cache, como Cache-Control: private, Cache-Control: no-cache e Cache-Control: no-store não são respeitados. No entanto, se houverem várias solicitações em andamento em um POP para a mesma URL, elas podem compartilhar a resposta.


## <a name="request-headers"></a>Cabeçalhos da solicitação

Os seguintes cabeçalhos de solicitação não serão encaminhados para um back-end ao usar o cache.
- Autorização
- Content-Length
- Transfer-Encoding

## <a name="next-steps"></a>Próximas etapas

- Saiba como [criar um Front Door](quickstart-create-front-door.md).
- Saiba [como o Front Door funciona](front-door-routing-architecture.md).