---
title: Registre os locatários para o uso de controle no Azure Stack | Microsoft Docs
description: Detalhes sobre operações usadas para gerenciar os registros do inquilino e como o uso do locatário é controlado no Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/08/2019
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: af3858ae283213de1591cf8f3c4e2acb307ce182
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54246119"
---
# <a name="manage-tenant-registration-in-azure-stack"></a>Gerenciar o registro de locatário no Azure Stack

*Aplica-se a: Sistemas integrados do Azure Stack*

Este artigo contém detalhes sobre operações de registro. Você pode usar essas operações:
- Gerenciar registros do inquilino
- Gerenciar o controle de uso do locatário

Você pode encontrar detalhes sobre como adicionar, lista, ou remover mapeamentos de locatário. Você pode usar o PowerShell ou os pontos de extremidade de API de cobrança para gerenciar o uso de controle. Você pode encontrar detalhes sobre como adicionar, lista, ou remover mapeamentos de locatário. Você pode usar o PowerShell ou os pontos de extremidade de API de cobrança para gerenciar o uso de controle.

## <a name="add-tenant-to-registration"></a>Adicione o locatário ao registro

Use a operação quando você deseja adicionar um novo locatário em seu registro. Uso de locatário é relatado em uma assinatura do Azure conectada com seu locatário do Azure Active Directory (Azure AD).

Se você quiser alterar a assinatura associada a um locatário, você também pode usar a operação. Chame PUT/New-AzureRMResource para substituir o mapeamento anterior.

Você pode associar uma única assinatura do Azure com um locatário. Se você tentar adicionar uma segunda assinatura para um locatário existente, a primeira assinatura é escrita em excesso.

### <a name="use-api-profiles"></a>Use perfis de API

Os cmdlets do registro exige que você especifique um perfil de API, quando a execução do PowerShell. Perfis de API representam um conjunto de provedores de recursos do Azure e suas versões de API. Eles ajudam você a usar a versão correta da API ao interagir com várias nuvens do Azure. Por exemplo, você trabalha com várias nuvens ao trabalhar com global do Azure e o Azure Stack. Os perfis especificam um nome que corresponda à sua data de lançamento. Você precisará usar o **2017-09-03** perfil.

Para obter mais informações sobre perfis de API e o Azure Stack, consulte [perfis de versão da API de gerenciar no Azure Stack](user/azure-stack-version-profiles.md). Para obter instruções sobre como colocar em funcionamento com o perfil de API com o PowerShell, consulte [perfis de versão da API de uso do PowerShell no Azure Stack](user/azure-stack-version-profiles-powershell.md).

### <a name="parameters"></a>parâmetros

| Parâmetro                  | DESCRIÇÃO |
|---                         | --- |
| registrationSubscriptionID | A assinatura do Azure que foi usada para o registro inicial. |
| customerSubscriptionID     | A assinatura do Azure (não o Azure Stack) que pertencem ao cliente a ser registrado. Deve ser criado na oferta de provedor de serviços de nuvem (CSP) por meio do Partner Center. Se um cliente tiver mais de um locatário, criou uma assinatura do locatário para fazer logon no Azure Stack. |
| resourceGroup              | O grupo de recursos no Azure em que o registro é armazenado. |
| registrationName           | O nome do registro do seu Azure Stack. Ele é um objeto armazenado no Azure. O nome geralmente está no CloudID-azurestack em formulário, onde CloudID é a ID de nuvem da sua implantação do Azure Stack. |

> [!Note]  
> Os locatários precisam ser registrados com cada Azure Stack que eles usam. Se um locatário usa mais de uma pilha do Azure, você precisará atualizar os registros iniciais de cada implantação com a assinatura de locatário.

### <a name="powershell"></a>PowerShell

Use o cmdlet New-AzureRmResource para atualizar o recurso de registro. Aqui está um exemplo de como adicionar um locatário:

```powershell
  New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```

### <a name="api-call"></a>Chamada à API

**Operação**: PUT  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  /providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Resposta**: 201 Criado  
**Corpo da resposta**: Vazio  

## <a name="list-all-registered-tenants"></a>Listar todos os locatários

Obtenha uma lista de todos os locatários que foram adicionados a um registro.

 > [!Note]  
 > Se nenhum locatário tenha sido registrado, você não receberá uma resposta.

### <a name="parameters"></a>parâmetros

| Parâmetro                  | DESCRIÇÃO          |
|---                         | ---                  |
| registrationSubscriptionId | A assinatura do Azure que foi usada para o registro inicial.   |
| resourceGroup              | O grupo de recursos no Azure em que o registro é armazenado.    |
| registrationName           | O nome do registro do seu Azure Stack. Ele é um objeto armazenado no Azure. O nome é normalmente na forma de **azurestack**-***CloudID***, onde ***CloudID*** é a ID de nuvem da sua implantação do Azure Stack.   |

### <a name="powershell"></a>PowerShell

Use o cmdlet Get-AzureRmResource para listar todos os locatários. Entrar no Azure (`Add-AzureRmAccount`) usando a conta usada para o registro inicial. Aqui está um exemplo de como adicionar um locatário:

```powershell
  Get-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>Chamada à API

Você pode obter uma lista de todos os mapeamentos de locatário usando a operação GET

**Operação**: GET  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions?  
api-version=2017-06-01 HTTP/1.1`  
**Resposta**: 200  
**Corpo da resposta**: 

```JSON  
{
    "value": [{
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 1}”,
            "name": " cspSubscriptionId 1",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId1" }
        },
        {
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 2}”,
            "name": " cspSubscriptionId2 ",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId2" }
        }
    ],
    "nextLink": "{originalRequestUrl}?$skipToken={opaqueString}"
}
```

## <a name="remove-a-tenant-mapping"></a>Remover um mapeamento de locatários

Você pode remover um locatário que tenha sido adicionado a um registro. Se esse locatário ainda está usando recursos no Azure Stack, seu uso será cobrado para a assinatura usada no registro inicial do Azure Stack.

### <a name="parameters"></a>parâmetros

| Parâmetro                  | DESCRIÇÃO          |
|---                         | ---                  |
| registrationSubscriptionId | ID da assinatura para o registro.   |
| resourceGroup              | O grupo de recursos para o registro.   |
| registrationName           | O nome do registro.  |
| customerSubscriptionId     | A ID de assinatura de cliente.  |

### <a name="powershell"></a>PowerShell

```powershell
  Remove-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>Chamada à API

Você pode remover mapeamentos de locatário usando a operação de exclusão.

**Operação**: EXCLUIR  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Resposta**: 204 Sem Conteúdo  
**Corpo da resposta**: Vazio

## <a name="next-steps"></a>Próximas etapas

 - Para saber mais sobre como recuperar informações de uso de recursos do Azure Stack, consulte [uso e cobrança do Azure Stack](azure-stack-billing-and-chargeback.md).
