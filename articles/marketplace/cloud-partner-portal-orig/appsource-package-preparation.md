---
title: Preparação do pacote do AppSource | Microsoft Docs
description: Explicação sobre como preparar e criar pacotes do AppSource.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricardo.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 0b24c5d2f174c9a656e81d0c85e12b589d7d7799
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48805149"
---
# <a name="appsource-package-preparation"></a>Preparação do pacote do AppSource

Além de um arquivo solution.zip, você precisará de um **pacote do AppSource**. Trata-se de um arquivo .zip que inclui todos os ativos necessários para automatizar o processo de implantação de sua solução no ambiente de CRM dos clientes. O **pacote do AppSource** tem os seguintes componentes

* Pacote do Package Deployer
* Arquivo **Content_Types.xml** com os ativos que você usa
* Arquivo XML com os dados específicos do aplicativo
* Logotipo de 32 x 32 que será exibido com sua listagem no Centro de Administração
* Termos de licença, política de privacidade

As etapas a seguir o ajudarão a criar seu pacote do AppSource.

## <a name="a-create-a-package-for-the-package-deployer"></a>a. Criar um pacote do Package Deployer

O pacote do Package Deployer é uma parte do pacote do AppSource.

Para criar um pacote do Package Deployer, siga as instruções a seguir: [https://msdn.microsoft.com/library/dn688182.aspx](https://msdn.microsoft.com/library/dn688182.aspx). Ao concluir, seu pacote consistirá nos ativos abaixo:

1. Pasta do pacote: contém todas as soluções, os dados de configuração, os arquivos simples e o conteúdo do pacote. _Observação: no exemplo a seguir vamos considerar que a pasta do pacote se chama "PkgFolder"_
2. dll: o assembly contém o código personalizado do pacote. _Observação: no exemplo a seguir vamos considerar que esse arquivo se chama "MicrosoftSample.dll"._

Agora, você precisa criar um arquivo chamado "**Content_Types.xml**" Este arquivo listará todas as extensões de ativos que fazem parte do pacote. Aqui está o código de exemplo do arquivo.

    <?xml version="1.0" encoding="utf-8"?>
        <Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types">
        <Default Extension="xml" ContentType="application/octet-stream" />
        <Default Extension="xaml" ContentType="application/octet-stream" />
        <Default Extension="dll" ContentType="application/octet-stream" />
        <Default Extension="zip" ContentType="application/octet-stream" />
        <Default Extension="jpg" ContentType="application/octet-stream" />
        <Default Extension="gif" ContentType="application/octet-stream" />
        <Default Extension="png" ContentType="application/octet-stream" />
        <Default Extension="htm" ContentType="application/octet-stream" />
        <Default Extension="html" ContentType="application/octet-stream" />
        <Default Extension="db" ContentType="application/octet-stream" />
        <Default Extension="css" ContentType="application/octet-stream" />
    </Types>

A etapa final é zipar o seguinte em um único arquivo. Nomeie-o como **package.zip**. Ele conterá

1. PkgFolder (incluindo tudo dentro da pasta)
2. dll
3. **Content_Types.xml**

Etapas para criar o package.zip:

1. Coloque a pasta do pacote, o arquivo **Content_Types.xml** e o PackageName.dll em um diretório.

![CRMScreenShot2](media/CRMScreenShot2.png)

2. Selecione todos os itens na pasta, clique com o botão direito do mouse e escolha Enviar para pasta compactada (zip)

![CRMScreenShot3](media/CRMScreenShot3.png)

3. Altere o nome para package.zip

![CRMScreenShot4](media/CRMScreenShot4.png)

## <a name="b-create-an-appsource-package"></a>b. Criar um pacote do AppSource

O pacote do AppSource requer alguns arquivos adicionais.

1. jpg (resolução de 32 x 32)
2. html (arquivo formatado em HTML)
3. **Content_Types.xml** (o mesmo que acima)
4. xml

Aqui está o código de exemplo de input.xml. Confira as definições na tabela a seguir.

    <PvsPackageData>
        <ProviderName>Microsoft</ProviderName>
        <PackageFile>package.zip</PackageFile>
        <SolutionAnchorName>SampleSolution.zip</SolutionAnchorName>
        <StartDate>01/01/2016</StartDate>
        <EndDate>01/01/2021</EndDate>
        <SupportedCountries>US,CA</SupportedCountries>
        <LearnMoreLink>http://www.microsoft.com</LearnMoreLink>
        <Locales>
        <PackageLocale Code="1033" IsDefault="true">
        <Logo>logo32x32.png</Logo>
        <Terms>
        <PackageTerm File="TermsOfUse.html" />
        </Terms>
        </PackageLocale>
        </Locales>
    </PvsPackageData>
 
**Em que:**

|Campo|Detalhes|
|---|---|
|ProviderName|De quem a solução provém. Se for de uma equipe da Microsoft, será Microsoft.|
|PackageFile |Ativos do Package Deployer compactados juntos com um arquivo content\_types.xml. Esse arquivo zip deve conter o assembly do Package Deployer e a pasta com os ativos do Package Deployer. Ou seja, package.zip|
|SolutionAnchorName |Nome do arquivo zip da solução no Package Deployer que é usado para o nome de exibição e a descrição dos ativos da solução.|
| StartDate| Essa é a data em que o pacote da solução será disponibilizado. O formato é MM/DD/AAAA|
|EndDate|Essa é a data em que o pacote da solução deixará de estar disponível. O formato é MM/DD/AAAA |
|SupportedCountries |Essa é uma lista delimitada por vírgulas dos países que devem ver esse pacote. Entre em contato com os serviços online para obter uma lista de todos os códigos de país/região atuais. No momento da escrita desta lista, os códigos eram: AE, AL, AM, AO, AR, AT, AU, AZ, BA, BB, BD, BE, BG, BH, BM, BN, BO, BR, BY, CA, CH, CI, CL, CM, CO, CR, CV, CW, CY, CZ, DE, DK, DO, DZ, EC, EE, EG, ES, FI, FR, GB, GE, GH, GR, GT, HK, HN, HR, HU, ID, IE, IL, IN, IQ, IS, IT, JM, JO, JP, KE, KG, KN, KR, KW, KY, KZ, LB, LK, LT, LU, LV, LY, MA, MC, MD, ME, MK, MN, MO, MT, MU, MX, MY, NG, NI, NL, NO, NZ, OM, PA, PE, PH, PK, PL, PR, PS, PT, PY, QA, RO, RS, RU, RW, SA, SE, SG, SI, SK, SN, SV, TH, TM, TN, TR, TT, TW, UA, US, UY, UZ, VE, VI, VN, ZA, ZW |
|LearnMoreLink | URL da página de mais informações desse pacote. |
|Localidades|Uma instância desse nó para cada idioma da UX ao qual você deseja dar suporte na UX da solução preferencial. Esse nó contém filhos que descrevem a localidade, o logotipo e os termos para cada idioma|
|Localidades: PackageLocale.Code|LCID do idioma desse nó. Exemplo: inglês dos EUA é 1033|
|Localidades: PackageLocale.IsDefault|Indica que esse é o idioma padrão. Ele será usado como o idioma de fallback caso o idioma da UX escolhido pelo cliente não esteja disponível.|
|Localidades: logotipo|Esse é o logotipo que você deseja usar para este pacote. O tamanho de ícone é 32 x 32. Os formatos permitidos são PNG e JPG|
|Locales:Terms: PackageTerm.File|Esse é o nome do arquivo do documento HTML que contém os termos da licença.|

O logotipo será exibido aqui:

![CRMScreenShot5](media/CRMScreenShot5.png)

A etapa final é zipar o seguinte em um único arquivo.

1. zip (criado anteriormente)
2. **Content_Types.xml**
3. xml
4. png
5. html

![CRMScreenShot6](media/CRMScreenShot6.png)

Renomeie o arquivo para algo apropriado para seu aplicativo. Preferimos que você inclua o nome da empresa e o nome do aplicativo. Por exemplo: **_Microsoft_SamplePackage.zip**.
