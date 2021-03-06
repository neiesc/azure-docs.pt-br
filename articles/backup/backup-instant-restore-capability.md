---
title: Funcionalidade de restauração instantânea do Azure
description: Funcionalidade de restauração instantânea do Azure e perguntas frequentes para pilha de backup de VM, modelo de implantação do Gerenciador de Recursos
services: backup
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: trinadhk
ms.openlocfilehash: a0cbb8650a818c3ae6c839d69bbc9b94f8d080ca
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54351389"
---
# <a name="get-improved-backup-and-restore-performance-with-azure-backup-instant-restore-capability"></a>Obter o melhor backup e restaurar o desempenho com a funcionalidade de restauração instantânea do Backup do Azure

> [!NOTE]
> Com base nos comentários recebidos dos usuários de que a **pilha de backup da VM V2** está sendo confundida com a pilha do Azure, nós a renomeamos para **Restauração instantânea** para oferecer uma experiência melhor e atualizada.

O novo modelo para Restauração instantânea oferece os seguintes aprimoramentos de recursos:

* Capacidade de usar o instantâneo tirado como parte do trabalho de backup que está disponível para recuperação sem aguardar a conclusão da transferência de dados para o cofre. Isso reduz o tempo de espera para instantâneos para copiar para o cofre antes de disparar a restauração.
* Redução dos tempos de backup e restauração mantendo os instantâneos com os discos por sete dias.
* Suporte a tamanhos de disco de até 4 TB.
* Suporte a discos SSD Standard.
* Capacidade de usar as contas de armazenamento originais de uma VM não gerenciada (por disco) ao restaurar. Essa capacidade existe mesmo quando a VM possui discos distribuídos em contas de armazenamento. Isso acelera as operações de restauração para uma ampla variedade de configurações de VM.


## <a name="whats-new-in-this-feature"></a>Novidades deste recurso

Hoje, o trabalho de backup consiste em duas fases:

1.  Obter instantâneo da VM.
2.  Transferir o instantâneo da VM para o cofre dos Serviços de Recuperação.

Um ponto de recuperação é considerado criado somente após as fases 1 e 2 estarem concluídas. Como parte dessa atualização, um ponto de recuperação é criado assim que o instantâneo é concluído, e esse ponto de recuperação do tipo de instantâneo pode ser usado para executar uma restauração usando o mesmo fluxo de restauração. Você pode identificar esse ponto de recuperação no portal do Azure, utilizando "instantâneo" como o tipo de ponto de recuperação e, depois que o instantâneo é transferido para o cofre, o tipo de ponto de recuperação é alterado para "instantâneo e cofre".

![Trabalho de backup na pilha de backup de VM, modelo de implantação do Gerenciador de Recursos - armazenar e cofre](./media/backup-azure-vms/instant-rp-flow.png)

Os instantâneos são retidos por sete dias. Esse recurso possibilita a operação de restauração a partir desses instantâneos para diminuir os tempos de restauração. Ele reduz o tempo necessário para transformar e copiar dados do cofre para a conta de armazenamento do usuário para cenários de disco não gerenciado, enquanto que, para os usuários do disco gerenciado, ele cria discos gerenciados dos dados de backup.

## <a name="feature-considerations"></a>Considerações do recurso

* Instantâneos são armazenados com os discos para aumentar a criação do ponto de recuperação e acelerar as operações de restauração. Como resultado, você vê os custos de armazenamento correspondentes aos instantâneos obtidos durante esse período.
* Os instantâneos incrementais são armazenados como blobs de página. Todos os usuários de discos não gerenciados são cobrados pelos instantâneos armazenados na conta de armazenamento local. Uma vez que as coleções de ponto de restauração usadas por backups de VM gerenciada usam instantâneos de blob no nível de armazenamento subjacente, para discos gerenciados, você vê os custos correspondentes a preços de instantâneo de blob, e eles são incrementais.
* Para contas de armazenamento premium, os instantâneos obtidos para pontos de recuperação instantâneos contam para o limite de 10 TB de espaço alocado.

## <a name="cost-impact"></a>Impacto de custo

Os instantâneos incrementais são armazenados na conta de armazenamento da VM, e são usados para a recuperação instantânea. Instantâneo incremental significa que o espaço ocupado por um instantâneo é igual ao espaço ocupado por páginas escritas depois que o instantâneo foi criado. A cobrança ainda ocorre de acordo com o espaço usado em GB ocupado pelo instantâneo e o preço por GB é o mesmo mencionado na [página de preços](https://azure.microsoft.com/pricing/details/managed-disks/).


## <a name="upgrading-to-instant-restore"></a>Atualizar para a Restauração instantânea

Se você usa o portal do Azure, verá uma notificação no dashboard do cofre. Essa notificação se relaciona com o suporte a disco grande e melhorias de velocidade de backup e restauração.

![Trabalho de backup na pilha de backup de VM, modelo de implantação do Gerenciador de Recursos - suporte e cofre](./media/backup-azure-vms/instant-rp-banner.png)

Para abrir uma tela para atualização da Restauração instantânea, selecione a faixa.

![Trabalho de backup na pilha de backup de VM, modelo de implantação do Gerenciador de Recursos - atualização](./media/backup-azure-vms/instant-rp.png)

Como alternativa, você pode ir para página **Propriedades** do cofre para ver a opção **Atualizar** em **Pilha de backup da VM**.

![Trabalho de backup na da pilha de backup da VM - Página de propriedades](./media/backup-azure-vms/instant-restore-capability-properties.png)


## <a name="upgrade-to-instant-restore-using-powershell"></a>Atualizar para a Restauração instantânea usando o PowerShell

Se quiser fazer autoatendimento e atualizar para a Restauração instantânea, execute os seguintes cmdlets em um terminal do PowerShell com privilégios elevados:

1.  Entre na sua conta do Azure:

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Selecione a assinatura que você deseja registrar:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Registrar esta assinatura:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="upgrade-to-instant-restore-using-cli"></a>Atualize para a restauração instantânea usando a CLI

Execute os seguintes comandos em um shell:

1.  Entre na sua conta do Azure:

    ```
    az login
    ```

2.  Selecione a assinatura que você deseja registrar:

    ```
    az account set --subscription "Subscription Name"
    ```

3.  Registrar esta assinatura:

    ```
    az feature register --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
    ```

## <a name="verify-that-the-upgrade-is-successful"></a>Verifique se a atualização foi bem-sucedida

### <a name="powershell"></a>PowerShell
De um terminal do PowerShell elevado, execute o cmdlet a seguir:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" -ProviderNamespace Microsoft.RecoveryServices
```

### <a name="cli"></a>CLI
Em um shell, execute o seguinte comando:

```
az feature show --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
```

Se ele diz: "Registrado", então sua assinatura está atualizada para a Restauração instantânea.

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="what-are-the-cost-implications-of-instant-restore"></a>Quais são as implicações de custo da Restauração instantânea?
Instantâneos são armazenados com os discos para acelerar a criação do ponto de recuperação e as operações de restauração. Como resultado, você vê os custos de armazenamento que correspondem à retenção de instantâneo selecionada como parte da política de backup de VM.

### <a name="in-premium-storage-accounts-do-the-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>Nas contas de Armazenamento Premium, os instantâneos obtidos para o ponto de Recuperação instantânea ocupam o limite de instantâneos de 10 TB?
Sim, para contas de armazenamento premium, os instantâneos obtidos para o ponto de Recuperação instantânea ocupam os 10 TB de espaço de instantâneo alocado.

### <a name="how-does-the-snapshot-retention-work-during-the-five-day-period"></a>Como funciona o trabalho de retenção de instantâneo durante o período de cinco dias?
Cada dia um novo instantâneo é obtido. Em um determinado momento, há cinco instantâneos incrementais individuais. O tamanho do instantâneo depende da variação de dados, que, na maioria dos casos, é de aproximadamente 2% a 7%.

### <a name="is-an-instant-restore-snapshot-an-incremental-snapshot-or-full-snapshot"></a>Um instantâneo de Restauração instantânea é um instantâneo incremental ou um instantâneo completo?
Os instantâneos tirados como parte da funcionalidade de Restauração instantânea são instantâneos incrementais.

### <a name="how-can-i-calculate-the-approximate-cost-increase-due-to-instant-restore-feature"></a>Como é possível calcular o aumento de custo aproximado devido à funcionalidade da Restauração instantânea?
Isso depende da variação da VM. Em um estado estável, é possível assumir que o aumento do custo é: = Período de retenção do instantâneo * variação diária por VM * custo de armazenamento por GB.

### <a name="if-the-recovery-type-for-a-restore-point-is-snapshot-and-vault-and-i-perform-a-restore-operation-which-recovery-type-will-be-used"></a>Se o tipo de recuperação para um ponto de restauração é "Instantâneo e cofre" e eu posso executar uma operação de restauração, qual tipo de recuperação será usado?
Se o tipo de recuperação é "instantâneo e cofre", a restauração é feita automaticamente a partir do instantâneo local, o que é muito mais rápido comparado à restauração feita do cofre.

### <a name="what-happens-if-i-select-retention-period-of-restore-point-tier-2-less-than-the-snapshot-tier1-retention-period"></a>O que acontece se eu selecionar o período de retenção do ponto de restauração (Tier2) menor que o período de retenção instantâneo (Tier1)?
O novo modelo não permite excluir o ponto de restauração (Tier2), a menos que o instantâneo (Tier1) seja excluído. Atualmente, há suporte para período de retenção de 7 dias para exclusão de instantâneo (Tier1), portanto, o período de retenção do ponto de restauração (Tier2) menor que 7 dias não é atendido. É recomendável agendar o período de retenção do ponto de restauração (Tier2) maior que 7 dias.

### <a name="why-is-my-snapshot-existing-even-after-the-set-retention-period-in-backup-policy"></a>Por que meu instantâneo existe mesmo após o período de retenção definido na política de backup?
Se o ponto de recuperação tem um instantâneo e esse é o período de retenção mais recente disponível, ele é mantido até o momento em que houver um backup bem-sucedido. Isso ocorre de acordo com a política de GC projetada hoje que exige que sempre haja pelo menos um período de retenção presente em caso de todos os backups posteriores falharem devido a um problema na VM. Em cenários normais, períodos de retenção são limpos no máximo 48 horas após sua expiração.
