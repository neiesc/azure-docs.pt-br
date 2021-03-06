---
title: 'Azure AD Connect: Referência do PowerShell ADSyncTools | Microsoft Docs'
description: Este documento fornece informações de referência para o módulo do PowerShell ADSyncTools.psm1.
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.date: 10/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 54d23bca29c38f258334d7b641b8c1f480da55e1
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54054955"
---
# <a name="azure-ad-connect--adsynctools-powershell-reference"></a>Azure AD Connect:  Referência do PowerShell do ADSyncTools
A documentação a seguir fornece informações de referência para o módulo do PowerShell ADSyncTools.psm1 que é incluído com o Azure AD Connect.

## <a name="clear-adsynctoolsconsistencyguid"></a>Clear-ADSyncToolsConsistencyGuid

### <a name="synopsis"></a>SINOPSE
Limpar o mS-Ds-ConsistencyGuid do usuário do AD

### <a name="syntax"></a>SINTAXE

```
Clear-ADSyncToolsConsistencyGuid [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>Descrição
Limpar o valor no mS-Ds-ConsistencyGuid do usuário do AD de destino

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>EXEMPLO 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARÂMETROS

#### <a name="-user"></a>-User
Usuário de destino no Active Directory a definir

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="confirm-adsynctoolsadmoduleloaded"></a>Confirm-ADSyncToolsADModuleLoaded

### <a name="synopsis"></a>SINOPSE
{{Preencher a sinopse}}

### <a name="syntax"></a>SINTAXE

```
Confirm-ADSyncToolsADModuleLoaded
```

### <a name="description"></a>Descrição
{{Preencher a descrição}}

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>Exemplo 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Adicionar uma descrição de exemplo aqui}}

## <a name="connect-adsyncdatabase"></a>Connect-AdSyncDatabase

### <a name="synopsis"></a>SINOPSE
{{Preencher a sinopse}}

### <a name="syntax"></a>SINTAXE

```
Connect-AdSyncDatabase [-Server] <String> [[-Instance] <String>] [[-Database] <String>] [[-UserName] <String>]
 [[-Password] <String>] [<CommonParameters>]
```

### <a name="description"></a>Descrição
{{Preencher a descrição}}

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>Exemplo 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Adicionar uma descrição de exemplo aqui}}

### <a name="parameters"></a>PARÂMETROS

#### <a name="-database"></a>-Database
{{Preencher descrição do banco de dados}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-instance"></a>-Instance
{{Preencher a descrição da instância}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-password"></a>-Password
{{Preencher a descrição da senha}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 4
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-server"></a>-Server
{{Preencher a descrição do servidor}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-username"></a>-UserName
{{Preencher a descrição do UserName}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="export-adsynctoolsconsistencyguidmigration"></a>Export-ADSyncToolsConsistencyGuidMigration

### <a name="synopsis"></a>SINOPSE
Exportar relatório ConsistencyGuid

### <a name="syntax"></a>SINTAXE

```
Export-ADSyncToolsConsistencyGuidMigration [-AlternativeLoginId] [-UserPrincipalName] <String>
 [-ImmutableIdGUID] <String> [-Output] <String> [<CommonParameters>]
```

### <a name="description"></a>Descrição
Gera um relatório de ConsistencyGuid com base em um arquivo CSV de importação de Import-ADSyncToolsImmutableIdMigration

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Import-Csv .\AllSyncUsers.csv | Export-ADSyncToolsConsistencyGuidMigration -Output ".\AllSyncUsers-Report"
```

#### <a name="example-2"></a>EXEMPLO 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARÂMETROS

#### <a name="-alternativeloginid"></a>-AlternativeLoginId
Use a ID de logon alternativa (email)

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-userprincipalname"></a>-UserPrincipalName
UserPrincipalName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-immutableidguid"></a>-ImmutableIdGUID
ImmutableIdGUID

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-output"></a>-Output
Nome de arquivo de saída para arquivos de log e CSV

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsyncsqlbrowserinstances"></a>Get-ADSyncSQLBrowserInstances

### <a name="synopsis"></a>SINOPSE
{{Preencher a sinopse}}

### <a name="syntax"></a>SINTAXE

```
Get-ADSyncSQLBrowserInstances [[-hostName] <String>]
```

### <a name="description"></a>Descrição
{{Preencher a descrição}}

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>Exemplo 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Adicionar uma descrição de exemplo aqui}}

### <a name="parameters"></a>PARÂMETROS

#### <a name="-hostname"></a>-hostName
{{Preencher a descrição do hostName}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="get-adsynctoolsaduser"></a>Get-ADSyncToolsADuser

### <a name="synopsis"></a>SINOPSE
Obter usuário do AD

### <a name="syntax"></a>SINTAXE

```
Get-ADSyncToolsADuser [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>Descrição
Retorna um objeto do AD TO DO: Suporte a várias florestas

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>EXEMPLO 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARÂMETROS

#### <a name="-user"></a>-User
Usuário de destino no Active Directory para definir ConsistencyGuid

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsynctoolsconsistencyguid"></a>Get-ADSyncToolsConsistencyGuid

### <a name="synopsis"></a>SINOPSE
Obter o mS-Ds-ConsistencyGuid do usuário do AD

### <a name="syntax"></a>SINTAXE

```
Get-ADSyncToolsConsistencyGuid [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>Descrição
Retorna o valor do atributo mS-Ds-ConsistencyGuid do usuário do AD de destino em formato GUID

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>EXEMPLO 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARÂMETROS

#### <a name="-user"></a>-User
Usuário de destino no Active Directory a definir

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsynctoolsobjectguid"></a>Get-ADSyncToolsObjectGuid

### <a name="synopsis"></a>SINOPSE
Obter o ObjectGuid do usuário do AD

### <a name="syntax"></a>SINTAXE

```
Get-ADSyncToolsObjectGuid [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>Descrição
Retorna o valor no atributo ObjectGUID do usuário do AD de destino em formato GUID

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>EXEMPLO 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARÂMETROS

#### <a name="-user"></a>-User
Usuário de destino no Active Directory a definir

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsynctoolsrunhistory"></a>Get-ADSyncToolsRunHistory

### <a name="synopsis"></a>SINOPSE
Obter o histórico de execuções do AAD Connect

### <a name="syntax"></a>SINTAXE

```
Get-ADSyncToolsRunHistory [[-Days] <Int32>] [<CommonParameters>]
```

### <a name="description"></a>Descrição
Função que retorna o histórico de execuções do AAD Connect em formato XML

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Get-ADSyncToolsRunHistory
```

#### <a name="example-2"></a>EXEMPLO 2
```
Get-ADSyncToolsRunHistory -Days 1
```

### <a name="parameters"></a>PARÂMETROS

#### <a name="-days"></a>-Days
{{Preencher descrição de dias}}

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: 3
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsynctoolssourceanchorchanged"></a>Get-ADSyncToolsSourceAnchorChanged

### <a name="synopsis"></a>SINOPSE
Obter usuários com erros de SourceAnchor alterado

### <a name="syntax"></a>SINTAXE

```
Get-ADSyncToolsSourceAnchorChanged [-sourcePath] <Object> [-outputPath] <Object> [<CommonParameters>]
```

### <a name="description"></a>Descrição
A função consulta o histórico de execuções do AAD Connect e exporta todos os usuários relatando o erro: "O atributo SourceAnchor foi alterado."

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
#Required Parameters
```

$sourcePath = Read-Host -Prompt "Insira o caminho do arquivo de log com o nome do arquivo" #"\<Source_Path\>" $outputPath = Read-Host -Prompt "Insira o caminho do arquivo de saída com o nome do arquivo" #"\<Out_Path\>"
 
 Get-ADSyncToolsUsersSourceAnchorChanged -sourcePath $sourcePath -outputPath $outputPath

#### <a name="example-2"></a>EXEMPLO 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARÂMETROS

#### <a name="-sourcepath"></a>-sourcePath
{{Preencher descrição de sourcePath}}

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-outputpath"></a>-outputPath
{{Preencher descrição de outputPath}}

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="import-adsynctoolsimmutableidmigration"></a>Import-ADSyncToolsImmutableIdMigration

### <a name="synopsis"></a>SINOPSE
Importar o ImmutableID do AAD

### <a name="syntax"></a>SINTAXE

```
Import-ADSyncToolsImmutableIdMigration [-Output] <String> [-IncludeSyncUsersFromRecycleBin]
 [<CommonParameters>]
```

### <a name="description"></a>Descrição
Gera um arquivo com todos os usuários sincronizados do Azure AD que contém o valor de ImmutableID no formato GUID. Requisitos: Módulo do PowerShell MSOnline

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Import-ADSyncToolsImmutableIdMigration -OutputFile '.\AllSyncUsers.csv'
```

#### <a name="example-2"></a>EXEMPLO 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARÂMETROS

#### <a name="-output"></a>-Output
Arquivo CSV de saída

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-includesyncusersfromrecyclebin"></a>-IncludeSyncUsersFromRecycleBin
Obter usuários sincronizados da Lixeira do Azure AD

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).


## <a name="invoke-adsyncdatabasequery"></a>Invoke-AdSyncDatabaseQuery

### <a name="synopsis"></a>SINOPSE
{{Preencher a sinopse}}

### <a name="syntax"></a>SINTAXE

```
Invoke-AdSyncDatabaseQuery [-SqlConnection] <SqlConnection> [[-Query] <String>] [<CommonParameters>]
```

### <a name="description"></a>Descrição
{{Preencher a descrição}}

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>Exemplo 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Adicionar uma descrição de exemplo aqui}}

### <a name="parameters"></a>PARÂMETROS

#### <a name="-query"></a>-Query
{{Preencher descrição da consulta}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-sqlconnection"></a>-SqlConnection
{{Preencher descrição de SqlConnection}}

```yaml
Type: SqlConnection
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="remove-adsynctoolsexpiredcertificates"></a>Remove-ADSyncToolsExpiredCertificates

### <a name="synopsis"></a>SINOPSE
Script para remover certificados expirados do atributo UserCertificate

### <a name="syntax"></a>SINTAXE

```
Remove-ADSyncToolsExpiredCertificates [-TargetOU] <String> [[-BackupOnly] <Boolean>] [-ObjectClass] <String>
 [<CommonParameters>]
```

### <a name="description"></a>Descrição
Esse script usa todos os objetos de uma unidade organizacional de destino no domínio do Active Directory – filtrados por classe de objeto (usuário/computador) – e exclui todos os certificados expirados presentes no atributo UserCertificate.
Por padrão, (modo BackupOnly) ele fará backup apenas de certificados expirados para um arquivo e não fará nenhuma alteração no AD.
Se você usa -BackupOnly $false, então qualquer certificado expirado presente no atributo UserCertificate para esses objetos é removido do AD depois de ser copiado para o arquivo.
Cada certificado será feito para um nome de arquivo separado: ObjectClass_ObjectGUID_CertThumprint.cer. O script também criará um arquivo de log no formato CSV mostrando todos os usuários com certificados que são válidos ou expirados, incluindo a ação real tomada ( Ignorado/Exportado/Excluído).

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Check all users in target OU - Expired Certificates will be copied to separated files and no certificates will be removed
```

Remove-ADSyncToolsExpiredCertificates -TargetOU "OU=Users,OU=Corp,DC=Contoso,DC=com" -ObjectClass user

#### <a name="example-2"></a>EXEMPLO 2
```
Delete Expired Certs from all Computer objects in target OU - Expired Certificates will be copied to files and removed from AD
```

Remove-ADSyncToolsExpiredCertificates -TargetOU "OU=Computers,OU=Corp,DC=Contoso,DC=com" -ObjectClass computer -BackupOnly $false

### <a name="parameters"></a>PARÂMETROS

#### <a name="-targetou"></a>-TargetOU
UO de destino para pesquisar por objetos do AD

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-backuponly"></a>-BackupOnly
BackupOnly não excluirá nenhum certificado do AD

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: True
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-objectclass"></a>-ObjectClass
Filtro de classe de objeto

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="repair-adsynctoolsautoupgradestate"></a>Repair-ADSyncToolsAutoUpgradeState

### <a name="synopsis"></a>SINOPSE
Descrição breve

### <a name="syntax"></a>SINTAXE

```
Repair-ADSyncToolsAutoUpgradeState
```

### <a name="description"></a>Descrição
Descrição longa

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>EXEMPLO 2
```
Another example of how to use this cmdlet
```

## <a name="resolve-adsynchostaddress"></a>Resolve-ADSyncHostAddress

### <a name="synopsis"></a>SINOPSE
{{Preencher a sinopse}}

### <a name="syntax"></a>SINTAXE

```
Resolve-ADSyncHostAddress [[-hostName] <String>]
```

### <a name="description"></a>Descrição
{{Preencher a descrição}}

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>Exemplo 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Adicionar uma descrição de exemplo aqui}}

### <a name="parameters"></a>PARÂMETROS

#### <a name="-hostname"></a>-hostName
{{Preencher a descrição do hostName}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="restore-adsynctoolsexpiredcertificates"></a>Restore-ADSyncToolsExpiredCertificates

### <a name="synopsis"></a>SINOPSE
(PARA FAZER) Restaura o atributo UserCertificate de AD de um arquivo de certificado

### <a name="syntax"></a>SINTAXE

```
Restore-ADSyncToolsExpiredCertificates
```

### <a name="description"></a>Descrição
Descrição longa

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>EXEMPLO 2
```
Another example of how to use this cmdlet
```

## <a name="set-adsynctoolsconsistencyguid"></a>Set-ADSyncToolsConsistencyGuid

### <a name="synopsis"></a>SINOPSE
Set mS-Ds-ConsistencyGuid on AD User

### <a name="syntax"></a>SINTAXE

```
Set-ADSyncToolsConsistencyGuid [-User] <Object> [-Value] <Object> [<CommonParameters>]
```

### <a name="description"></a>Descrição
Definir um valor no atributo mS-Ds-ConsistencyGuid do usuário do AD de destino

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>EXEMPLO 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARÂMETROS

#### <a name="-user"></a>-User
Usuário de destino no Active Directory para definir ConsistencyGuid

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-value"></a>-Value
ImmutableId (matriz de bytes, GUID, cadeia de caracteres de GUID ou cadeia de caracteres Base64)

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="test-adsyncnetworkport"></a>Test-ADSyncNetworkPort

### <a name="synopsis"></a>SINOPSE
{{Preencher a sinopse}}

### <a name="syntax"></a>SINTAXE

```
Test-ADSyncNetworkPort [[-hostName] <String>] [[-port] <String>]
```

### <a name="description"></a>Descrição
{{Preencher a descrição}}

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>Exemplo 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Adicionar uma descrição de exemplo aqui}}

### <a name="parameters"></a>PARÂMETROS

#### <a name="-hostname"></a>-hostName
{{Preencher a descrição do hostName}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-port"></a>-port
{{Preencher descrição da porta}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="trace-adsynctoolsadimport"></a>Trace-ADSyncToolsADImport

### <a name="synopsis"></a>SINOPSE
Cria um arquivo de rastreamento de uma etapa de importação do AD

### <a name="syntax"></a>SINTAXE

```
Trace-ADSyncToolsADImport [[-ADConnectorXML] <String>] [[-dc] <String>] [[-rootDN] <String>]
 [[-filter] <String>] [-SkipCredentials] [[-ADwatermark] <String>] [<CommonParameters>]
```

### <a name="description"></a>Descrição
Rastreia todas as consultas ldap de uma importação do AD do AAD Connect executada de um determinado ponto de verificação de marca-d'água do AD (cookie de partição). Cria um arquivo de rastreamento '.\ADimportTrace_yyyyMMddHHmmss.log' na pasta atual.

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>EXEMPLO 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARÂMETROS

#### <a name="-adconnectorxml"></a>-ADConnectorXML
{{Preencher a descrição de ADConnectorXML}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dc"></a>-dc
Arquivo XML de uma exportação de conector do AD

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: DC1.domain.contoso.com
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-rootdn"></a>-rootDN
Controlador de domínio de destino

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: DC=Domain,DC=Contoso,DC=com
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-filter"></a>-filter
DN de raiz de floresta

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 4
Default value: (&(objectClass=*))
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipcredentials"></a>-SkipCredentials
Tipos de objetos do AD para rastrear \> * = todos os tipos de objeto

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adwatermark"></a>-ADwatermark
Se já estiver executando como administrador de domínio, não há necessidade de fornecer credenciais do AD.
Entrada manual de marca d'água, em vez de arquivo XML, por exemplo $ ADwatermark = "TVNEUwMAAAAXyK9ir1zSAQAAAAAAAAAA (...)"

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 5
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="trace-adsynctoolsldapquery"></a>Trace-ADSyncToolsLdapQuery

### <a name="synopsis"></a>SINOPSE
Descrição breve

### <a name="syntax"></a>SINTAXE

```
Trace-ADSyncToolsLdapQuery [-Context] <String> [-Server] <String> [-Port] <Int32> [-Filter] <String>
 [<CommonParameters>]
```

### <a name="description"></a>Descrição
Descrição longa

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>EXEMPLO 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARÂMETROS

#### <a name="-context"></a>-Context
Descrição de ajuda Param1

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-server"></a>-Server
Descrição de ajuda Param2

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: Localhost
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-port"></a>-Port
Descrição de ajuda Param2

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: 389
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-filter"></a>-Filter
Descrição de ajuda Param2

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 4
Default value: (objectClass=*)
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="update-adsynctoolsconsistencyguidmigration"></a>Update-ADSyncToolsConsistencyGuidMigration

### <a name="synopsis"></a>SINOPSE
Atualiza os usuários com o novo ConsistencyGuid (ImmutableId)

### <a name="syntax"></a>SINTAXE

```
Update-ADSyncToolsConsistencyGuidMigration [[-DistinguishedName] <String>] [-ImmutableIdGUID] <String>
 [-Action] <String> [-Output] <String> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>Descrição
Atualiza os usuários com o novo valor de ConsistencyGuid (ImmutableId) obtido do Relatório de ConsistencyGuid. Essa função dá suporte à opção WhatIf. Observação: O relatório ConsistencyGuid precisa ser importado com delimitador de guias

### <a name="examples"></a>EXEMPLOS

#### <a name="example-1"></a>EXEMPLO 1
```
Import-Csv .\AllSyncUsersTEST-Report.csv -Delimiter "`t"| Update-ADSyncToolsConsistencyGuidMigration -Output .\AllSyncUsersTEST-Result2 -WhatIf
```

#### <a name="example-2"></a>EXEMPLO 2
```
Import-Csv .\AllSyncUsersTEST-Report.csv -Delimiter "`t"| Update-ADSyncToolsConsistencyGuidMigration -Output .\AllSyncUsersTEST-Result2
```

### <a name="parameters"></a>PARÂMETROS

#### <a name="-distinguishedname"></a>-DistinguishedName
DistinguishedName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: False
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-immutableidguid"></a>-ImmutableIdGUID
ImmutableIdGUID

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-action"></a>-Action
Ação

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-output"></a>-Output
Nome de arquivo de saída para arquivos de log

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 4
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Mostra o que aconteceria se o cmdlet fosse executado.
O cmdlet não é executado.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Confirm
Solicita sua confirmação antes de executar o cmdlet.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet oferece suporte aos parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable.
Para obter mais informações, confira about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).
