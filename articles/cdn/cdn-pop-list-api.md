---
title: Recuperar a lista POP atual da Verizon para o CDN do Azure| Microsoft Docs
description: Saiba como recuperar a lista POP atual da Verizon usando o API REST.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: f703a934b0eaf4bff5be3811adeed8f0287bc658
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50237818"
---
# <a name="retrieve-the-current-verizon-pop-list-for-azure-cdn"></a>Recupere a lista POP da Verizon para a CDN do Azure

Você pode usar a API REST para recuperar o conjunto de IPs para o ponto da Verizon dos servidores de presença (POP). Esses servidores POP fazem solicitações para servidores de origem que estão associados com os pontos de extremidade do Azure Content Delivery Network (CDN) em um perfil Verizon (**Azure CDN padrão da Verizon** ou **Premium do Azure CDN da Verizon**). Observe que esse conjunto de IPs é diferente dos IPs que um cliente veria ao fazer solicitações aos POPs. 

Para obter a sintaxe da operação da API REST para recuperar a lista POP, consulte [Nós de Borda - Lista](https://docs.microsoft.com/rest/api/cdn/edgenodes/list).

## <a name="typical-use-case"></a>Caso de uso típico

Por motivos de segurança, use essa lista de IPs para impor que as solicitações para o seu servidor de origem sejam feitas apenas a partir de um POP válido da Verizon. Por exemplo, se alguém descobrir o nome do host ou o endereço IP do servidor de origem de um ponto de extremidade de CDN, seria possível fazer solicitações diretamente ao servidor de origem, ignorando os recursos de dimensionamento e segurança fornecidos pela CDN do Azure. Ao definir os IPs na lista retornada como os únicos IPs permitidos em um servidor de origem, esse cenário pode ser evitado. Para garantir que você tem a lista POP mais recente, recupere pelo menos uma vez por dia. 

## <a name="next-steps"></a>Próximas etapas

Para obter informações sobre a API REST, consulte [Azure CDN REST API](https://docs.microsoft.com/rest/api/cdn/).
