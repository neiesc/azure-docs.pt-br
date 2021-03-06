---
title: Gerenciar domínios de falha em conjuntos de dimensionamento de máquinas virtuais do Azure | Microsoft Docs
description: Saiba como escolher o número correto de FDs ao criar um conjunto de dimensionamento de máquinas virtuais.
services: virtual-machine-scale-sets
documentationcenter: ''
author: rajsqr
manager: drewm
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/18
ms.author: rajraj
ms.openlocfilehash: 3b5568e0eed06684667b8aeefa389b6d0aab2a5f
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53636092"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>Escolher o número correto de domínios de falha para o conjunto de dimensionamento de máquinas virtuais
Por padrão, os conjuntos de dimensionamento de máquinas virtuais são criados com cinco domínios de falha em regiões do Azure sem zonas. Em regiões com suporte para implantação zonal de conjuntos de dimensionamento de máquinas virtuais, o valor padrão da contagem de domínio de falha é 1 para cada uma das zonas. FD=1, nesse caso, implica que as instâncias de VM pertencentes ao conjunto de dimensionamento serão distribuídas em vários racks com base no melhor esforço.

Também é possível considerar o alinhamento do número de domínios de falha do conjunto de dimensionamento com o número de domínios de falha de Managed Disks. Esse alinhamento poderá ajudar a evitar perda de quorum, se um domínio de falha de Managed Disks inteiro for desativado. A contagem de FD pode ser definida como menor ou igual ao número de domínios de falha de Managed Disks disponíveis em cada uma das regiões. Consulte este [documento](../virtual-machines/windows/manage-availability.md) para saber mais sobre o número de domínios de falha de Managed Disks por região.

## <a name="rest-api"></a>API REST
É possível definir a propriedade `properties.platformFaultDomainCount` para 1, 2 ou 3 (padrão de 5, se não estiver especificado). Consulte a documentação da API REST [aqui](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate).

## <a name="azure-cli"></a>CLI do Azure
Você pode definir o parâmetro `--platform-fault-domain-count` para 1, 2 ou 3 (padrão de 5, se não estiver especificado). Consulte a documentação da CLI do Azure [aqui](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az-vmss-create).

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --platform-fault-domain-count 3\
  --generate-ssh-keys
```

Leva alguns minutos para criar e configurar todos os recursos e as VMs do conjunto de dimensionamento.

## <a name="next-steps"></a>Próximas etapas
- Saiba mais sobre [recursos de redundância e disponibilidade](../virtual-machines/windows/regions-and-availability.md) para ambientes do Azure.
