---
title: Alterar a ID de locatário do cofre de chaves de depois de mover uma assinatura - Azure Key Vault| Microsoft Docs
description: Saiba como mudar a ID de locatário para um cofre de chaves depois que uma assinatura é movida para um locatário diferente
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 46d7bc21-fa79-49e4-8c84-032eef1d813e
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: ea6fc4b155075084150d5bb732f3f8a08846974f
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54074301"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Alterar a ID de locatário do cofre de chaves depois de mover uma assinatura

## <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>P: Minha assinatura foi movida do locatário A para o locatário B. Como posso alterar a ID do locatário para o cofre de chaves existente e definir as ACLs corretas para as entidades de segurança no locatário B?

Quando você cria um novo cofre de chaves em uma assinatura, ele é vinculado automaticamente à ID de locatário do Azure Active Directory padrão para essa assinatura. Todas as entradas de política de acesso também são vinculadas a essa ID de locatário. Quando você move a assinatura do Azure do locatário A para o locatário B, os cofres de chaves existentes ficam inacessíveis para as entidades (usuários e aplicativos) no locatário B. Para corrigir esse problema, você precisa:

* Alterar a ID do locatário associada a todos os cofres de chaves existentes nessa assinatura para o locatário B.
* Remover todas as entradas de política de acesso existentes.
* Adicionar novas entradas de política de acesso que estão associadas ao locatário B.

Por exemplo, se você tiver o cofre de chaves 'myvault' em uma assinatura que foi movida do locatário A para o B, veja como mudar a ID de locatário para o cofre de chaves e remover as antigas políticas de acesso.

<pre>
Select-AzureRmSubscription -SubscriptionId YourSubscriptionID
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId
$vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Já que o cofre estava no locatário A antes da mudança, o valor original de **$vault. Properties.TenantId** é locatário A e **(Get-AzureRmContext).Tenant.TenantId** é locatário B.

Agora que o cofre está associado à ID de locatário correta e as entradas de política de acesso antigas foram removidas, defina novas entradas de política de acesso com [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy).

## <a name="next-steps"></a>Próximas etapas

Se você tiver dúvidas sobre o Cofre de Chaves do Azure, visite os [Fóruns do Cofre de Chaves do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
