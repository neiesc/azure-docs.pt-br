---
title: Verificação em duas etapas e AD FS - Azure MFA | Microsoft Docs
description: Esta é a página do Azure Multi-Factor Authentication que descreve como começar a usar o Azure MFA e o AD FS.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 6ce31bba9fb2f237586c6e59cbb5be7643c35aa1
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39161426"
---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Introdução ao Autenticação Multifator do Azure e aos Serviços de Federação do Active Directory

<center>![Nuvem](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Caso organização tenha federado seu Active Directory local com o Azure Active Directory usando o AD FS, existem duas opções para usar a Autenticação Multifator do Azure.

* Proteger os recursos de nuvem usando a Autenticação Multifator do Azure ou os Serviços de Federação do Active Directory
* Proteger os recursos de nuvem e locais usando o Servidor de Autenticação Multifator do Azure

A tabela a seguir resume a experiência de verificação entre a proteção dos recursos com a Autenticação Multifator do Azure e o AD FS

| Experiência de verificação – Aplicativos baseados em navegador | Experiência de verificação – Aplicativos não baseados em navegador |
|:--- |:--- |:--- |
| Protegendo os recursos do AD do Azure usando a Autenticação Multifator do Azure |<li>A primeira etapa de verificação é executada localmente usando o AD FS.</li> <li>A segunda etapa é um método baseado em telefone executado com a autenticação na nuvem.</li> |
| Protegendo recursos do AD do Azure usando os Serviços de Federação do Active Directory |<li>A primeira etapa de verificação é executada localmente usando o AD FS.</li><li>A segunda etapa é realizada localmente, cumprindo a declaração.</li> |

Advertências sobre senhas de aplicativo para usuários federados:

* Senhas de aplicativo são verificadas usando a autenticação na nuvem e, portanto, ignora as federações. A federação só é usada ativamente ao configurar a senha de aplicativo.
* As configurações do Controle de Acesso do Cliente local não são consideradas pela Senha de aplicativo.
* Você perde a capacidade de registro de autenticação local para senha de aplicativo.
* A desabilitação/exclusão da conta pode levar até três horas para a sincronização do diretório, atrasando a desabilitação/exclusão da senha de aplicativo na identidade da nuvem.

Para obter informações sobre como configurar a Autenticação Multifator do Azure ou o Servidor de Autenticação Multifator do Azure com o AD FS, consulte os artigos:

* [Proteger os recursos de nuvem usando a Autenticação Multifator do Azure e o AD FS](howto-mfa-adfs.md)
* [Proteger recursos de nuvem e locais usando o Servidor de Autenticação Multifator do Azure com o AD FS do Windows Server 2012 R2](howto-mfaserver-adfs-2012.md)
* [Proteger recursos de nuvem e locais usando o Servidor de Autenticação Multifator do Azure com AD FS 2.0](howto-mfaserver-adfs-2.md)
