---
title: Permissões de usuário padrão - Azure Active Directory | Microsoft Docs
description: Saiba mais sobre as permissões de usuário diferentes disponíveis no Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: lizross
ms.reviewer: vincesm
ms.custom: it-pro, seodec18
ms.openlocfilehash: 58354bf95b11e497f05fdf08ba4740b3f55c6d6e
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54022777"
---
# <a name="what-are-the-default-user-permissions-in-azure-active-directory"></a>Quais são as permissões de usuário padrão no Azure Active Directory?
No Azure Active Directory (Azure AD), a todos os usuários é concedido um conjunto de permissões padrão. Um acesso do usuário é composto por tipo de usuário, suas [associações de função](https://docs.microsoft.com/azure/active-directory/active-directory-users-assign-role-azure-portal) e sua posse de objetos individuais. Este artigo descreve as permissões padrão e contém uma comparação entre os padrões do usuário membro e convidado.

## <a name="member-and-guest-users"></a>Usuários membro e convidado
O conjunto de permissões padrão recebido depende se o usuário é um membro nativo do locatário (usuário membro) ou se o usuário é um convidado de colaboração B2B (usuário convidado). Para saber mais sobre a colaboração B2B e sobre usuários convidados, consulte [O que é a colaboração B2B do Azure AD?](../b2b/what-is-b2b.md) 
* Usuários membro podem registrar aplicativos, gerenciar seu próprio número de celular e foto perfil, alterar sua senha e convidar pessoas B2B. Além disso, os usuários podem ler todas as informações de diretório (com algumas poucas exceções). 
* Usuários convidados do Azure AD B2B têm permissões de diretório restritas. Por exemplo, os usuários convidados não podem procurar informações do locatário além de suas próprias informações de perfil. No entanto, um usuário convidado pode recuperar informações sobre outro usuário, fornecendo o Nome Principal do Usuário ou o objectId. Um convidado não consegue informações sobre outros objetos de locatário, como grupos e aplicativos.

Permissões padrão para convidados são restritivas por padrão. Convidados podem ser adicionados a funções de administrador, o que lhes concedem permissões totais de leitura e gravação contidas na função. Há uma restrição adicional disponível, a capacidade de convidados convidarem outras pessoas. Ao definir a opção **Convidados podem convidar** como **Não**, impede-se que convidados convidem outras pessoas. Consulte [Delegar convites para colaboração B2B](../b2b/delegate-invitations.md) para saber como. Para conceder a usuários convidados as mesmas permissões de usuários membros por padrão, defina **Permissões a usuários convidados são limitadas** para **Não**. Essa configuração concede permissões completas de usuários membros para usuários convidados por padrão, bem como permite que convidados sejam adicionados a funções administrativas.

## <a name="compare-member-and-guest-default-permissions"></a>Comparar permissões padrão de membro e convidado

**Área** | **Permissões de usuário membro** | **Permissões de usuário convidado**
------------ | --------- | ----------
Usuários e contatos | Ler todas as propriedades públicas de usuários e contatos<br>Convidar pessoas<br>Alterar a própria senha<br>Gerenciar o próprio número de celular<br>Gerenciar a própria foto<br>Invalidar os próprios tokens de atualização | Ler as próprias propriedades<br>Ler nome de exibição, email, nome de entrada, foto, nome principal de usuário e propriedades de tipo de usuário de outros usuários e contatos<br>Alterar a própria senha
Grupos | Criar grupos de segurança<br>Criar grupos do Office 365<br>Ler todas as propriedades de grupos<br>Ler associações de grupos não ocultos<br>Ler associações de grupo oculto do Office 365 para grupo associado<br>Gerenciar propriedades, posse e associação de grupos próprios<br>Adicionar convidados a grupos próprios<br>Gerenciar configurações de associação dinâmica<br>Excluir grupos próprios<br>Restaurar grupos próprios do Office 365 | Ler todas as propriedades de grupos<br>Ler associações de grupos não ocultos<br>Ler associações de grupo do Office 365 oculto para grupos associados<br>Gerenciar grupos próprios<br>Adicionar convidados a grupos próprios (se permitido)<br>Excluir grupos próprios<br>Restaurar grupos próprios do Office 365 
APLICATIVOS | Registrar (criar) novo aplicativo<br>Ler propriedades de aplicativos registrados e corporativos<br>Gerenciar propriedades do aplicativo, atribuições e credenciais para aplicativos próprios<br>Criar ou excluir senha de aplicativo do usuário<br>Excluir aplicativos próprios<br>Restaurar aplicativos próprios | Ler propriedades de aplicativos registrados e corporativos<br>Gerenciar propriedades do aplicativo, atribuições e credenciais para aplicativos próprios<br>Excluir aplicativos próprios<br>Restaurar aplicativos próprios
Dispositivos | Ler todas as propriedades de dispositivos<br>Gerenciar todas as propriedades de dispositivos próprios<br> | Nenhuma permissão<br>Excluir dispositivos próprios<br>
Diretório | Ler todas as informações da empresa<br>Ler todos os domínios<br>Ler todos os contratos de parceiro | Ler nome de exibição e domínios verificados
Funções e escopos | Ler todas as funções e associações administrativas<br>Ler todas as propriedades e associação de unidades administrativas | Nenhuma permissão 
Assinaturas | Ler todas as assinaturas<br>Habilitar Membro de plano de serviço | Nenhuma permissão
Políticas | Ler todas as propriedades de políticas<br>Gerenciar todas as propriedades de política própria | Nenhuma permissão

## <a name="to-restrict-the-default-permissions-for-member-users"></a>Para restringir as permissões padrão para usuários membro

Permissões padrão para usuários membro podem ser restringidas das seguintes maneiras.

Permissão | Configurar explicação
---------- | ------------
Capacidade de criar grupos de segurança | Definir essa opção como Não impede que os usuários criem grupos de segurança. Administradores globais e Administradores de conta de usuário ainda podem criar grupos de segurança. Consulte [Cmdlets do Active Directory do Azure para definir as configurações de grupo](../users-groups-roles/groups-settings-cmdlets.md) para saber como.
Capacidade de criar grupos do Office 365 | Definir essa opção como Não impede que os usuários criem grupos do Office 365. Definir essa opção para Alguns permite que um conjunto selecionado de usuários crie grupos do Office 365. Administradores globais e Administradores de conta de usuário ainda poderão criar grupos do Office 365. Consulte [Cmdlets do Active Directory do Azure para definir as configurações de grupo](../users-groups-roles/groups-settings-cmdlets.md) para saber como.
Restringir o acesso ao portal de administração do Azure AD | Definir essa opção como Não, impede que os usuários acessam o Microsoft Azure Active Directory.
Capacidade de ler outros usuários | Essa configuração só está disponível no PowerShell. A definição como $false impede que todos os não administradores leiam informações de usuário do diretório. Isso não impede a leitura de informações de usuário em outros serviços da Microsoft, como o Exchange Online. Essa configuração destina-se a circunstâncias especiais e configurar como $false não é recomendado.

## <a name="object-ownership"></a>Propriedade do objeto

### <a name="application-registration-owner-permissions"></a>Permissões de proprietário de registro de aplicativo
Quando um usuário registra um aplicativo, ele é automaticamente adicionado como um proprietário do aplicativo. Como proprietário, ele pode gerenciar os metadados do aplicativo, como o nome e as permissões que o aplicativo solicita. Eles também podem gerenciar a configuração de locatário específico do aplicativo, como configuração de SSO e as atribuições de usuário. Um proprietário também pode adicionar ou remover outros proprietários. Diferentemente dos Administradores globais, proprietários só podem gerenciar os aplicativos que eles possuem.

<!-- ### Enterprise application owner permissions

When a user adds a new enterprise application, they are automatically added as an owner for the tenant-specific configuration of the application. As an owner, they can manage the tenant-specific configuration of the application, such as the SSO configuration, provisioning, and user assignments. An owner can also add or remove other owners. Unlike Global Administrators, owners can manage only the applications they own. <!--To assign an enterprise application owner, see *Assigning Owners for an Application*.-->

### <a name="group-owner-permissions"></a>Permissões de proprietário do grupo

Quando um usuário cria um grupo, ele é automaticamente adicionado como um proprietário desse grupo. Como proprietário, eles podem gerenciar as propriedades do grupo, como o nome, bem como gerenciar a associação. Um proprietário também pode adicionar ou remover outros proprietários. Diferentemente dos administradores globais e os administradores de contas de usuário, os proprietários só podem gerenciar os grupos que possuem. Para atribuir um proprietário do grupo, consulte [Gerenciando proprietários para um grupo](active-directory-accessmanagement-managing-group-owners.md).

## <a name="next-steps"></a>Próximas etapas

* Para saber mais sobre como atribuir funções de administrador do Microsoft Azure Active Directory consulte [Atribuir um usuário a funções de administrador no Microsoft Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
* Para saber mais sobre como o acesso aos recursos é controlado no Microsoft Azure, confira [Noções básicas sobre o acesso a recursos no Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para saber mais sobre como o Azure Active Directory está relacionado à sua assinatura do Azure, confira [Como as assinaturas do Azure estão associadas ao Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Gerenciar usuários](add-users-azure-active-directory.md)
