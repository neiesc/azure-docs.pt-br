---
title: Remover um aplicativo registrado na Microsoft Identity Platform | Azure
description: Saiba como remover um aplicativo registrado na Microsoft Identity Platform.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/25/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.openlocfilehash: 916d34a0af1490404cb9f7851b3117f1967c54d0
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240895"
---
# <a name="quickstart-remove-an-application-registered-with-the-microsoft-identity-platform-preview"></a>Início Rápido: Remover um aplicativo registrado na Microsoft Identity Platform (versão prévia)

Os desenvolvedores corporativos e provedores de SaaS (software como serviço) que registraram aplicativos na Microsoft Identity Platform podem precisar remover o registro do aplicativo.

Neste início rápido, você aprenderá a:

* [Remover um aplicativo criado por você ou por sua organização](#remove-an-application-authored-by-your-organization)
* [Remover um aplicativo criado por outra organização](#remove-an-application-authoried-by-another-organization)

## <a name="prerequisites"></a>Pré-requisitos

Para começar, atenda aos seguintes pré-requisitos:

* Obtenha um locatário com aplicativos registrados. Para aprender a adicionar aplicativos registrados, confira [Registrar um aplicativo na Microsoft Identity Platform](quickstart-register-app.md).
* Inscreva-se na experiência de versão prévia de registros de aplicativos no portal do Azure. As etapas neste início rápido correspondem à nova interface do usuário e só funcionarão se você aceitou a experiência de versão prévia.

## <a name="remove-an-application-authored-by-you-or-your-organization"></a>Remover um aplicativo criado por você ou por sua organização

Os aplicativos que você ou sua organização registraram são representados por um objeto de aplicativo e pelo objeto de entidade de serviço em seu locatário. Para saber mais, consulte [Objetos de aplicativo e objetos de entidade de serviço](active-directory-application-objects.md).

### <a name="to-remove-an-application"></a>Para remover um aplicativo

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
2. Se a conta der acesso a mais de um locatário, selecione sua conta no canto superior direito e defina a sessão do portal para o locatário do Azure AD desejado.
3. No painel de navegação à esquerda, selecione o serviço **Azure Active Directory** e selecione **Registros de aplicativo (versão prévia)**. Encontre e selecione o aplicativo que você deseja configurar. Depois de selecionar o aplicativo, você verá a página **Visão Geral** do aplicativo.
4. Na página **Visão Geral**, selecione **Excluir**.
5. Selecione **Sim** para confirmar que você deseja excluir o aplicativo.

  > [!NOTE]
  > Para excluir um aplicativo, você precisa estar listado como proprietário do aplicativo ou ter privilégios de administrador.

## <a name="remove-an-application-authored-by-another-organization"></a>Remover um aplicativo criado por outra organização

Se você estiver exibindo **Registros de aplicativo** no contexto de um locatário, um subconjunto dos aplicativos que são exibidos na guia **Todos os aplicativos** são de outro locatário e foram registrados no seu locatário durante o processo de consentimento. Mais especificamente, eles são representados apenas por um objeto de entidade de serviço em seu locatário, sem objeto de aplicativo correspondente. Para saber mais sobre as diferenças entre os objetos de aplicativo e entidade de serviço, confira [Objetos de aplicativo e entidade de serviço no Azure AD](active-directory-application-objects.md).

Para remover o acesso de um aplicativo ao seu diretório (depois de ter dado autorização), o administrador da empresa deverá remover sua entidade de serviço. O administrador deve ter acesso de administrador global e pode remover o aplicativo pelo Azure ou usar os [Cmdlets do PowerShell do Azure AD](https://go.microsoft.com/fwlink/?LinkId=294151) para remover o acesso.

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre esses outros inícios rápidos de gerenciamento de aplicativos relacionados:

* [Registrar um aplicativo com a Microsoft Identity Platform](quickstart-register-app.md)
* [Configurar um aplicativo cliente para acessar APIs Web](quickstart-configure-app-access-web-apis.md)
* [Configurar um aplicativo para expor APIs Web](quickstart-configure-app-expose-web-apis.md)
* [Modificar as contas que têm suporte por um aplicativo](quickstart-modify-supported-accounts.md)
