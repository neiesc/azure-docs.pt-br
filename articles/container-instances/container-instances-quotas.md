---
title: Cotas para Instâncias de Contêiner do Azure e disponibilidade de região
description: As cotas padrão e a disponibilidade de região do serviço de Instâncias de Contêiner do Azure.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 12/17/2018
ms.author: danlep
ms.openlocfilehash: 647890517e6f08a4602ebed8ee1057cb45f10cbe
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54075491"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Cotas e disponibilidade de região para Instâncias de Contêiner do Azure

Todos os serviços do Azure incluem certos limites padrão e cotas de recursos. As seções a seguir detalham os limites de recursos padrão de vários recursos de Instâncias de Contêiner do Azure (ACI), bem como a disponibilidade do serviço ACI em regiões do Azure.

## <a name="service-quotas-and-limits"></a>Cotas e limites de serviço

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="region-availability"></a>Disponibilidade de região

As Instâncias de Contêiner do Azure estão disponíveis nas seguintes regiões com os limites de memória e CPU especificados. Os valores estavam atualizados no momento da publicação. Para obter informações atualizadas, use a API de [Funcionalidades de Lista](/rest/api/container-instances/listcapabilities/listcapabilities). A disponibilidade e os limites de recursos podem ser diferentes ao usar instâncias de contêiner do Azure com uma [rede virtual](container-instances-vnet.md) (versão prévia) ou com [recursos de GPU](container-instances-gpu.md) (versão prévia).

| Local padrão | SO | CPU | Memória (GB) |
| -------- | -- | :---: | :-----------: |
| Canadá Central | Linux | 4 | 16 |
| Leste dos EUA, Europa Setentrional, Europa Ocidental, Oeste dos EUA, Oeste dos EUA 2 | Linux | 4 | 14 |
| Leste do Japão | Linux | 2 | 8 |
| Leste da Austrália, Leste dos EUA 2, Sudeste Asiático | Linux | 2 | 7 |
| Índia Central, Ásia Oriental, Centro-Norte dos EUA, Centro-Sul dos EUA, Sul da Índia | Linux | 2 | 3,5 |
| Leste dos EUA, Europa Ocidental, Oeste dos EUA |  Windows | 4 | 14 |
| Leste da Austrália, Canadá Central, Índia Central, Ásia Oriental, Leste dos EUA 2, Leste do Japão, Centro-Norte dos EUA, Europa Setentrional, Centro-Sul dos EUA, Sul da Índia, Sudeste Asiático, Oeste dos EUA 2 |  Windows | 2 | 3,5 |

As instâncias de contêiner criadas dentro desses limites de recursos estão sujeitas à disponibilidade dentro da região de implantação. Quando uma região está sob carga pesada, você pode enfrentar uma falha durante a implantação de instâncias. Para atenuar essa falha de implantação, tente implantar instâncias com configurações de memória e CPU inferiores ou tente implantar em um momento posterior.

Informe à equipe de regiões adicionais necessárias ou limites de CPU/memória aumentados em [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Para obter mais informações sobre como solucionar problemas de implantação de instâncias de contêiner, consulte [Solucionar problemas de implantação com Instâncias de Contêiner do Azure](container-instances-troubleshooting.md).

## <a name="next-steps"></a>Próximas etapas

Determinados limites e cotas padrão podem ser aumentados. Para solicitar um aumento de um ou mais recursos que oferecem suporte a esse aumento, envie uma [solicitação de suporte do Azure][azure-support] (selecione "Cota" em **Tipo de emissão**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
