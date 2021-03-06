---
title: Use ferramentas remotas para solucionar problemas do VM do Azure | Microsoft Docs
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 01/11/2018
ms.author: delhan
ms.openlocfilehash: d9bb13b24a16cc38f738d9a654789d4410225c09
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669303"
---
# <a name="use-remote-tools-to-troubleshoot-azure-vm-issues"></a>Use ferramentas remotas para solucionar problemas do VM do Azure

Ao solucionar problemas em uma máquina virtual do Azure (VM), você pode se conectar à VM usando as ferramentas remotas discutidas neste artigo em vez de usar o protocolo RDP.

## <a name="serial-console"></a>Console Serial

Use o [Console Serial da Máquina Virtual](serial-console-windows.md) para executar comandos na VM remota do Azure.

## <a name="remote-cmd"></a>CMD remoto

Baixe o [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec). Conecte-se à VM executando o seguinte comando:

```cmd
psexec \\<computer>-u user -s cmd
```

>[!Note]
>* O comando deve ser executado em um computador que esteja na mesma VNET.
>* Nome de host ou DIP pode ser usado para substituir <computer>.
>* O parâmetro -s garante que o comando seja chamado usando a conta do sistema (permissão de administrador).
>* PsExec usa portas TCP 135 e 445. Portanto, as duas portas devem estar abertas no Firewall.

## <a name="run-commands"></a>Execute comandos

Consulte [Executar scripts do PowerShell em sua VM do Windows com o comando Executar](../windows/run-command.md) para obter mais informações sobre como usar o recurso Executar Comandos para executar scripts na VM.

## <a name="customer-script-extension"></a>Extensão de Script de cliente

Você pode usar o recurso Custom Script Extension para executar um script personalizado na VM de destino. Para usar esse recurso, as seguintes condições devem ser atendidas:

* A VM tem conectividade.

* O agente do Azure é instalado e funcionando conforme o esperado na VM.

* Anteriormente, a extensão não foi instalada na VM.
 
  A extensão injetará o script apenas na primeira vez que ele é usado. Se você usar esse recurso posteriormente, a extensão reconhecerá que já foi usado e não carregará o novo script.

Você precisa carregar o script para uma conta de armazenamento e gerar seu próprio contêiner. Em seguida, execute o seguinte script do PowerShell do Azure em um computador que tenha conectividade com a VM.

### <a name="for-v1-vms"></a>Para VMs V1

```powershell
#Setup the basic variables
$subscriptionID = "<<SUBSCRIPTION ID>>" 
$storageAccount = "<<STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for the blob in the storage
$vmName = "<<VM NAME>>" 
$vmCloudService = "<<CLOUD SERVICE>>" #Resource group/Cloud Service where the VM is hosted. I.E.: For "demo305.cloudapp.net" the cloud service is going to be demo305

#Setup the Azure Powershell module and ensure the access to the subscription
Import-Module Azure
Add-AzureAccount  #Ensure Login with account associated with subscription ID
Get-AzureSubscription -SubscriptionId $subscriptionID | Select-AzureSubscription

#Setup the access to the storage account and upload the script
$storageKey = (Get-AzureStorageKey -StorageAccountName $storageAccount).Primary
$context = New-AzureStorageContext -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)<
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM
$vm = Get-AzureVM -ServiceName $vmCloudService -Name $vmName
Set-AzureVMCustomScriptExtension "CustomScriptExtension" -VM $vm -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName | Update-AzureVM
```

### <a name="for-v2-vms"></a>Para VMs V2

```powershell
#Setup the basic variables
$subscriptionID = "<<SUBSCRIPTION ID>>"
$storageAccount = "<<STORAGE ACCOUNT>>"
$storageRG = "<<RESOURCE GROUP OF THE STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for blob in storage
$vmName = "<<VM NAME>>" 
$vmResourceGroup = "<<RESOURCE GROUP>>"
$vmLocation = "<<DATACENTER>>" 
 
#Setup the Azure Powershell module and ensure the access to the subscription
Import-Module AzureRM
Login-AzureRmAccount #Ensure Login with account associated with subscription ID
Get-AzureRmSubscription -SubscriptionId $subscriptionID | Select-AzureRmSubscription

#Setup the access to the storage account and upload the script 
$storageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageRG -Name $storageAccount).Value[0]
$context = New-AzureStorageContext -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM
Set-AzureRmVMCustomScriptExtension -Name "CustomScriptExtension" -ResourceGroupName $vmResourceGroup -VMName $vmName -Location $vmLocation -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName
```

## <a name="remote-powershell"></a>PowerShell remoto

>[!Note]
>A porta TCP 5986 (HTTPS) deve estar aberta para que você possa usar essa opção.
>
>Para VMs de ARM, você deve abrir a porta 5986 no grupo de segurança de rede (NSG). Para mais informações, consulte Grupos de segurança. 
>
>Para VMs RDFE, você deve ter um terminal que tenha uma porta privada (5986) e uma porta pública. Então, você também tem que abrir essa porta de frente pública no NSG.

### <a name="set-up-the-client-computer"></a>Configurar o computador cliente

Para usar o PowerShell para conectar-se à VM remotamente, primeiro você precisa configurar o computador cliente para permitir a conexão. Para isso, adicione a VM à lista de hosts confiáveis do PowerShell executando o seguinte comando, conforme apropriado.

Para adicionar uma VM à lista de hosts confiáveis:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName>
```

Para adicionar várias VMs à lista de hosts confiáveis:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName1>,<ComputerName2>
```

Para adicionar todos os computadores à lista de hosts confiáveis:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value *
```

### <a name="enable-remoteps-on-the-vm"></a>Habilitar RemotePS na VM

Para VMs clássicas, use a extensão de Script personalizado para executar o script a seguir:

```powershell
Enable-PSRemoting -Force
New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My).Thumbprint
$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername""; CertificateThumbprint=""$thumbprint""}"
cmd.exe /C $command
```

For ARM VMs, use Run Commands from the portal to run the EnableRemotePS script:

![Executar Comando](./media/remote-tools-troubleshoot-azure-vm-issues/run-command.png)

### <a name="connect-to-the-vm"></a>Conectar-se à VM

Run the following command, depending on the client computer location:

* Outside the VNET or deployment

    * For a classic VM, run the following command:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName  "<<CLOUDSERVICENAME.cloudapp.net>>" -port "<<PUBLIC PORT NUMBER>>" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

    * For an ARM VM, first add a DNS name to the public IP address. Para obter etapas detalhadas, consulte [Criar um nome de domínio totalmente qualificado no portal do Azure para uma VM do Windows](../windows/portal-create-fqdn.md). Em seguida, execute o seguinte comando:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName "<<DNSname.DataCenter.cloudapp.azure.com>>" -port "5986" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

* Dentro da VNET ou implantação, execute o seguinte comando:
  
  ```powershell
  $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
  Enter-PSSession -ComputerName  "<<HOSTNAME>>" -port 5986 -Credential (Get-Credential) -useSSL -SessionOption $Skip
  ```

>[!Note] 
>Definir o sinalizador SkipCaCheck ignora o requisito para importar um certificado para a VM quando você inicia a sessão.

Você também pode usar o cmdlet Invoke-Command para executar um script na VM remotamente:

```powershell
Invoke-Command -ComputerName "<<COMPUTERNAME>" -ScriptBlock {"<<SCRIPT BLOCK>>"}
```

## <a name="remote-registry"></a>Registro remoto

>[!Note]
>A porta TCP 135 ou 445 deve estar aberta para usar esta opção.
>
>Para VMs de ARM, você precisa abrir a porta 5986 no NSG. Para mais informações, consulte Grupos de segurança. 
>
>Para VMs RDFE, você deve ter um terminal que tenha uma porta privada 5986 e uma porta pública. Você também precisa abrir essa porta voltada para o público no NSG.

1. De outra VM na mesma VNET, abra o editor de registro (regedit.exe).

2. Selecione **Arquivo** >**Conecte o Registro de Rede**.

   ![Opção Remota](./media/remote-tools-troubleshoot-azure-vm-issues/remote-registry.png) 

3. Localize a VM de destino por **hostname** ou **Dynamic IP** (preferível) inserindo-a na caixa "Digite o nome do objeto a ser selecionado".

   ![Opção Remota](./media/remote-tools-troubleshoot-azure-vm-issues/input-computer-name.png) 
 
4. Digite as credenciais para a VM de destino.

5. Faça as alterações necessárias no registro.

## <a name="remote-services-console"></a>Console de serviços remotos

>[!Note]
>As portas TCP 135 ou 445 devem estar abertas para usar essa opção.
>
>Para VMs de ARM, você precisa abrir a porta 5986 no NSG. Para mais informações, consulte Grupos de segurança. 
>
>Para VMs RDFE, você deve ter um terminal que tenha uma porta privada 5986 e uma porta pública. Em seguida, você também deve abrir essa porta voltada para o público no NSG.

1. De outra VM na mesma VNET, abra uma instância de **Services.msc**.

2. Clique com botão direito **serviços (Local)**.

3. Selecione **Conectar-se a outro computador**.

   ![Serviço remoto](./media/remote-tools-troubleshoot-azure-vm-issues/remote-services.png)

4. Insira o IP dinâmico de VM de destino.

   ![Entrada DIP](./media/remote-tools-troubleshoot-azure-vm-issues/input-ip-address.png)

5. Faça as alterações necessárias para os serviços.

## <a name="next-steps"></a>Próximas etapas

[ENTER-PSSession](https://technet.microsoft.com/library/hh849707.aspx)

[Personalizado Script de extensão para Windows usando o modelo de implantação clássico](../extensions/custom-script-classic.md)

PsExec é parte do [conjunto PSTools](https://download.sysinternals.com/files/PSTools.zip).

Para obter mais informações sobre o conjunto PSTools, consulte [conjunto PSTools](https://docs.microsoft.com/sysinternals/downloads/pstools).


