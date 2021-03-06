---
title: Logon único para aplicativos – Azure Active Directory | Microsoft Docs
description: Saiba como escolher um método de logon único ao configurar aplicativos no Azure AD (Azure Active Directory). Use o logon único para que os usuários não precisem se lembrar de senhas para cada aplicativo e para simplificar a administração de gerenciamento de conta.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: barbkess
ms.reviewer: arvindh
ms.openlocfilehash: b7e75753f8d7a9316dd1cc644dc22c42ef284736
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54190036"
---
# <a name="single-sign-on-to-applications-in-azure-active-directory"></a>Logon único para aplicativos no Azure Active Directory
Saiba como escolher o método de logon único mais apropriado ao configurar aplicativos no Azure AD (Azure Active Directory). 

- **Com o logon único**, os usuários entram uma vez com uma conta para acessar os dispositivos ingressados no domínio, recursos da empresa, aplicativos de SaaS (software como serviço) e aplicativos Web. Depois de entrar, o usuário pode iniciar os aplicativos no portal do Office 365 ou no painel de acesso MyApps do Azure AD. Os administradores podem centralizar o gerenciamento da conta de usuário e automaticamente adicionar ou remover o acesso de usuário para aplicativos com base na associação de grupo. 

- **Sem o logon único**, os usuários devem se lembrar de senhas específicas do aplicativo e entrar em cada aplicativo. A equipe de TI precisa criar e atualizar as contas de usuário para cada aplicativo, como o Office 365, o Box e o Salesforce. Os usuários precisam se lembrar de suas senhas, além de gastar tempo para entrar em cada aplicativo.

Este artigo descreve os métodos de logon único e ajuda você a escolher o melhor para seus aplicativos.

## <a name="choosing-a-single-sign-on-method"></a>Escolhendo um método de logon único

Há várias maneiras de configurar um aplicativo para logon único. A escolha de um método de logon único depende de como o aplicativo está configurado para a autenticação. Todos os métodos de logon único, exceto desabilitado, conectam os usuários automaticamente aos aplicativos sem exigir um segundo logon.  

- Os aplicativos de nuvem podem usar os métodos SAML, baseado em senha, vinculado ou desabilitado para o logon único. O SAML é o método de logon único mais seguro.
- Os aplicativos locais podem usar os métodos baseado em senha, de Autenticação Integrada do Windows, baseado em cabeçalho, vinculado ou desabilitado para o logon único. As opções locais funcionam quando os aplicativos são configurados para Proxy de Aplicativo. 

Este fluxograma ajuda você a decidir qual método de logon único é melhor para sua situação. 

![Escolher o método de logon único](./media/what-is-single-sign-on/choose-single-sign-on-method.png)

A tabela a seguir resume os métodos de logon único e os links para obter mais detalhes. 

| Método de logon único | Tipos de aplicativos | Quando usar |
| :------ | :------- | :----- |
| [SAML](#saml-sso) | Somente na nuvem | Sempre que possível, use SAML. O SAML funciona quando os aplicativos são configurados para usar um dos protocolos SAML.|
| [Baseado em senha](#password-based-sso) | nuvem e local | Use quando o aplicativo for autenticado com o nome de usuário e senha. O logon único baseado em senha permite o armazenamento e a reprodução segura de senhas do aplicativo usando uma extensão de navegador da Web ou aplicativo móvel. Esse método utiliza o processo de entrada existente fornecido pelo aplicativo, mas permite que um administrador gerencie as senhas. |
| [Vinculado](#linked-sso) | nuvem e local | Use o logon único vinculado quando o aplicativo estiver configurado para o logon único em outro serviço de provedor de identidade. Essa opção não adiciona o logon único ao aplicativo. No entanto, o aplicativo pode já ter o logon único implementado usando outro serviço, como Serviços de Federação do Active Directory (AD FS).|
| [Desabilitado](#disabled-sso) | nuvem e local | Use o logon único desabilitado quando o aplicativo não estiver pronto para ser configurado para o logon único. Os usuários precisam inserir seu nome de usuário e senha toda vez que iniciarem este aplicativo.|
| [IWA (Autenticação Integrada do Windows)](#integrated-windows-authentication-iwa-sso) | apenas local | Use esse método de logon único para aplicativos que usam a [IWA (Autenticação Integrada do Windows)](/aspnet/web-api/overview/security/integrated-windows-authentication) ou aplicativos com reconhecimento de declaração. Os conectores de Proxy de Aplicativo usam a KCD (Delegação Restrita de Kerberos) para autenticar os usuários no aplicativo. | 
| [Baseado em cabeçalho](#header-based-sso) | apenas local | Use o logon único baseado em cabeçalho quando o aplicativo usar cabeçalhos para autenticação. O logon único baseado em cabeçalho requer o PingAccess para o Azure Active Directory. O Proxy de Aplicativo usa o Azure AD para autenticar o usuário e, em seguida, passa o tráfego pelo serviço de conector.  | 

## <a name="saml-sso"></a>SSO do SAML
Com o **logon único SAML**, o Azure AD realiza a autenticação para o aplicativo usando a conta do Azure AD do usuário. O Azure AD comunica as informações do logon para o aplicativo por meio de um protocolo de conexão. Com o logon único baseado em SAML, você pode mapear os usuários para funções de aplicativo específicas com base nas regras definidas nas suas declarações SAML

O logon único baseado em SAML é:

- Mais seguro do que o logon único baseado em senha e todos os outros métodos de logon.
- Nosso método recomendado para logon único.

O logon único baseado em SAML é compatível com aplicativos que usam qualquer um desses protocolos:

- SAML 2.0
- O certificado do provedor de identidade do Web Services Federation

Para configurar um aplicativo para o logon único baseado em SAML, confira [Configurar o logon único baseado em SAML](configure-single-sign-on-portal.md). Além disso, muitos aplicativos têm [tutoriais específicos deles](../saas-apps/tutorial-list.md) que orientam você pela configuração do logon único baseado em SAML para aplicativos específicos. 

Para obter mais informações sobre como o protocolo SAML funciona, confira [Protocolo SAML de logon único](../develop/single-sign-on-saml-protocol.md).

## <a name="password-based-sso"></a>SSO baseado em senha
Com o logon baseado em senha, os usuários finais entram no aplicativo com um nome de usuário e senha a primeira vez que acessam. Após o primeiro logon, o Azure Active Directory fornece o nome de usuário e senha para o aplicativo. 

O logon único baseado em senha usa os processos de autenticação existentes fornecidos pelo aplicativo. Quando você habilita o logon único de senha para um aplicativo, o Azure AD coleta e armazena de forma segura os nomes de usuário e senhas do aplicativo. As credenciais do usuário são armazenadas em um estado criptografado no diretório. 

Use o logon único baseado em senha quando:

- Um aplicativo não for compatível com o protocolo de logon único.
- Um aplicativo for autenticado com um nome de usuário e senha em vez de tokens de acesso e cabeçalhos.

O logon único baseado em senha for compatível com qualquer aplicativo baseado em nuvem que tenha uma página de entrada baseada em HTML. O usuário pode usar qualquer um dos seguintes navegadores:

- Internet Explorer 11 no Windows 7 ou posterior
- O Microsoft Edge na Edição de Aniversário do Windows 10 ou posterior 
- Chrome no Windows 7 ou posterior e no MacOS X ou posterior
- Firefox 26.0 ou posterior no Windows XP SP2 ou posterior e no Mac OS X 10.6 ou posterior

Para configurar um aplicativo em nuvem para o logon único baseado em senha, confira [Configurar o aplicativo para logon único com senha](application-sign-in-problem-password-sso-gallery.md#configure-the-application-for-password-single-sign-on).

Para configurar um aplicativo local para logon único por meio do Proxy de Aplicativo, confira [Cofre para senhas para logon único com o Proxy de Aplicativo](application-proxy-configure-single-sign-on-password-vaulting.md)

### <a name="managing-credentials-for-password-based-sso"></a>Gerenciando credenciais para o SSO baseado em senha

Para autenticar um usuário para um aplicativo, o Azure AD recupera as credenciais do usuário do diretório e as insere na página de entrada do aplicativo.  O Azure AD passa com segurança as credenciais do usuário por meio de uma extensão de navegador da Web ou aplicativo móvel. Esse processo permite que um administrador gerencie as credenciais do usuário e não exige que os usuários se lembrem de sua senha.

> [!IMPORTANT]
> As credenciais são ocultadas do usuário final durante o processo de logon automatizado. No entanto, as credenciais são detectáveis usando ferramentas de depuração da Web. Os usuários e administradores precisam seguir as mesmas políticas de segurança como se as credenciais fossem inseridas diretamente pelo usuário.

As senhas de cada aplicativo podem ser gerenciadas pelo administrador do Azure AD ou pelos usuários.

Quando o administrador do Azure AD gerencia as credenciais:  

- O usuário não precisa redefinir ou lembrar o nome de usuário e senha. O usuário pode acessar o aplicativo clicando no seu painel de acesso ou por meio de um link fornecido.
- O administrador pode realizar tarefas de gerenciamento nas credenciais. Por exemplo, o administrador pode atualizar o acesso de aplicativo de acordo com as associações de grupo do usuário e o status do funcionário.
- O administrador pode usar as credenciais administrativas para fornecer acesso aos aplicativos compartilhados entre vários usuários. Por exemplo, o administrador pode permitir que qualquer pessoa que pode acessar um aplicativo tenha acesso a uma mídia social ou o aplicativo de compartilhamento de documentos.

Quando o usuário final gerencia as credenciais:

- Os usuários podem gerenciar as senhas atualizando-as ou excluindo-as, conforme necessário. 
- Os administradores ainda são capazes de definir novas credenciais para o aplicativo.


## <a name="linked-sso"></a>SSO vinculado
O logon vinculado permite que o Azure AD forneça o logon único para um aplicativo que já está configurado para um logon único em outro serviço. O aplicativo vinculado pode aparecer para os usuários finais no portal do Office 365 ou portal MyApps do Azure AD. Por exemplo, um usuário pode iniciar um aplicativo que está configurado para logon único no Serviços de Federação do Active Directory (AD FS) 2.0 no portal do Office 365. Relatórios adicionais também estão disponíveis para aplicativos vinculados que são iniciados no portal do Office 365 ou no portal MyApps do Azure AD. 

Use o logon único vinculado para:

- Fornecer uma experiência de usuário consistente enquanto migra aplicativos durante um período. Se você estiver migrando aplicativos para o Azure Active Directory, poderá usar o logon único vinculado para publicar rapidamente links para todos os aplicativos que pretende migrar.  Os usuários podem encontrar todos os links no [portal MyApps](../user-help/active-directory-saas-access-panel-introduction.md) ou no [iniciador de aplicativos do Office 365](https://support.office.com/article/meet-the-office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a). Os usuários não saberão que estão acessando um aplicativo vinculado ou migrado.  

Depois que um usuário for autenticado com um aplicativo vinculado, um registro de conta precisa ser criado antes que o usuário final receba o acesso de logon único. O provisionamento deste registro de conta pode ocorrer automaticamente ou manualmente por um administrador.

## <a name="disabled-sso"></a>SSO desabilitado

O modo desabilitado significa que o logon único não é usado para o aplicativo. Quando o logon único está desabilitado, pode ser necessário que os usuários se autentiquem duas vezes. Primeiro, os usuários se autenticam no Azure AD e, em seguida, entram no aplicativo. 

Use o modo de logon único desabilitado:

- Se você não estiver pronto para integrar este aplicativo ao logon único do Azure AD,
- Se você estiver testando outros aspectos do aplicativo ou
- Como uma camada de segurança para um aplicativo local que não exige que os usuários se autentiquem. Com logon único desabilitado, o usuário precisa se autenticar. 

## <a name="integrated-windows-authentication-iwa-sso"></a>SSO de IWA (Autenticação Integrada do Windows)

O Proxy de Aplicativo do Azure AD oferece SSO (logon único ) para aplicativos que usam a [IWA (Autenticação Integrada do Windows)](/aspnet/web-api/overview/security/integrated-windows-authentication) ou aplicativos com reconhecimento de declaração. Se o aplicativo usar a IWA, o Proxy de Aplicativo autenticará o aplicativo usando a KCD (Delegação Restrita de Kerberos). Para um aplicativo com reconhecimento de declaração que confia no Azure Active Directory, o logon único funciona porque o usuário já foi autenticado usando o Azure AD.

Use o modo de logon único de Autenticação Integrada do Windows:

- Para fornecer o logon único para um aplicativo local que se autentica com IWA. 

Para configurar um aplicativo local para IWA, confira [Delegação restrita de Kerberos para logon único em seus aplicativos com o Proxy de Aplicativo](application-proxy-configure-single-sign-on-with-kcd.md). 

### <a name="how-single-sign-on-with-kcd-works"></a>Como funciona o logon único com a KCD
Este diagrama explica o fluxo de quando um usuário acessa um aplicativo local que usa IWA.

![Diagrama de fluxo de autenticação do Microsoft AAD](./media/application-proxy-configure-single-sign-on-with-kcd/AuthDiagram.png)

1. O usuário insere a URL para acessar o aplicativo local por meio do Proxy de Aplicativo.
2. O Proxy de Aplicativo redireciona a solicitação para serviços de autenticação do AD do Azure para pré-autenticação. Neste ponto, o AD do Azure se aplica a qualquer política de autenticação e autorização aplicável, tal como autenticação multifator. Se o usuário for validado, o AD do Azure cria um token e o envia para o usuário.
3. O usuário passa o token para o Proxy de Aplicativo.
4. O Proxy de Aplicativo valida o token e recupera o nome UPN dele. Em seguida, ele envia a solicitação, o nome UPN e o SPN (nome da entidade de serviço) para o conector por meio de um canal seguro duplamente autenticado.
5. O conector usa a negociação da KCD (Delegação Restrita de Kerberos) com o AD local, representando o usuário para obter um token Kerberos para o aplicativo.
6. O Active Directory envia o token Kerberos do aplicativo para o conector.
7. O conector envia a solicitação original para o servidor de aplicativos usando o token Kerberos recebido do AD.
8. O aplicativo envia a resposta para o conector, que é retornada para o serviço de Proxy de Aplicativo e, finalmente, para o usuário.

## <a name="header-based-sso"></a>SSO baseado em cabeçalho

O logon único baseado em cabeçalho funciona para aplicativos que usam cabeçalhos de HTTP para autenticação. Esse método de logon usa um serviço de autenticação de terceiros chamado PingAccess. O usuário precisa se autenticar apenas no Azure AD. 

Use o logon único baseado em cabeçalho quando:

- O Proxy de Aplicativo e o PingAccess são configurados para o aplicativo

Para configurar a autenticação baseada em cabeçalho, confira [Autenticação baseada em cabeçalho para logon único com o Proxy de Aplicativo](application-proxy-configure-single-sign-on-with-ping-access.md). 

### <a name="what-is-pingaccess-for-azure-ad"></a>O que é PingAccess para Azure AD?

Usando o PingAccess para o Azure AD, os usuários podem acessar e realizar o logon único em aplicativos que usam cabeçalhos para autenticação. O Proxy de Aplicativo trata esses aplicativos como qualquer outro, usando o Azure AD para autenticar o acesso e, depois, passando o tráfego por meio do serviço de conector. Após a autenticação, o serviço de PingAccess converte o token de acesso do Azure AD em um formato de cabeçalho que é enviado para o aplicativo.

Os usuários não perceberão algo diferente quando entrarem para usar seus aplicativos corporativos. Eles ainda podem trabalhar de qualquer lugar e em qualquer dispositivo. Os conectores do Proxy de Aplicativo direcionam o tráfego remoto a todos os aplicativos e eles continuarão a balancear a carga automaticamente.

### <a name="how-do-i-get-a-license-for-pingaccess"></a>Como obtenho uma licença para o PingAccess?

Como esse cenário é oferecido por meio de uma parceria entre o Azure Active Directory e o PingAccess, você precisa de licenças para os dois serviços. No entanto, as assinaturas do Azure Active Directory Premium incluem uma licença básica PingAccess que abrange até 20 aplicativos. Se você precisar publicar mais de 20 aplicativos com base em cabeçalho, poderá adquirir uma licença adicional do PingAccess. 

Para obter mais informações, consulte [Edições do Active Directory do Azure](../fundamentals/active-directory-whatis.md).

## <a name="related-articles"></a>Artigos relacionados
* [Tutoriais para a integração de aplicativos SaaS ao Azure Active Directory](../saas-apps/tutorial-list.md)
* [Tutorial para a configuração do logon único](configure-single-sign-on-portal.md)
* [Introdução ao gerenciamento do acesso a aplicativos](what-is-access-management.md)
* Link de download: [Plano de implantação de logon único](https://aka.ms/SSODeploymentPlan).


