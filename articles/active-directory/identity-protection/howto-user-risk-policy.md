---
title: Como configurar a política de risco do usuário no Azure Active Directory Identity Protection| Microsoft Docs
description: Saiba como configurar a política de risco do usuário no Azure AD Identity Protection.
services: active-directory
keywords: azure active directory identity protection, cloud app discovery, gerenciamento de aplicativos, segurança, risco, nível de risco, vulnerabilidade, política de segurança
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 3ba8c93d158a6ff80c3db788924503e9d4e87242
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378112"
---
# <a name="how-to-configure-the-user-risk-policy"></a>Como configurar a política de risco do usuário

Com o risco do usuário, o Microsoft Azure AD detecta a probabilidade de que uma conta de usuário tenha sido comprometida. Como administrador, você pode configurar uma política de acesso condicional do risco do usuário para responder automaticamente a um nível de risco do usuário específico.
 
Este artigo fornece as informações necessárias para configurar a política de risco do usuário.


## <a name="what-is-a-user-risk-policy"></a>O que é uma política de risco do usuário?

O Microsoft Azure AD analisa cada entrada de um usuário. O objetivo da análise é detectar ações suspeitas que ocorrem junto com a entrada. No Microsoft Azure AD, as ações suspeitas que o sistema pode detectar também são conhecidas como eventos de risco. Embora alguns eventos de risco possam podem ser detectados em tempo real, há também aqueles que exigem mais tempo. Por exemplo, para detectar uma viagem impossível a locais atípicos, o sistema requer um período inicial de aprendizado de 14 dias para conhecer melhor o comportamento normal de um usuário. Há várias opções para resolver os eventos de risco detectados. Por exemplo, é possível resolver eventos de risco individuais manualmente ou por meio de uma política de acesso condicional de risco de entrada ou de usuário.

Todos os eventos de risco que foram detectados para um usuário e não são resolvidos são conhecidos como eventos de risco ativos. Os eventos de risco ativos que estão associados a um usuário são conhecidos como risco do usuário. Com base no risco do usuário, o Microsoft Azure AD calcula uma probabilidade (baixa, média, alta) de um usuário ter sido comprometido. A probabilidade é chamada de nível de risco do usuário.

![Riscos do usuário](./media/howto-user-risk-policy/1031.png)

A política de risco do usuário é uma resposta automatizada que pode ser configurada para um nível de risco do usuário específico. Com uma política de risco do usuário, é possível bloquear o acesso aos recursos ou exigir uma alteração de senha para retornar uma conta de usuário para um estado inicial.


## <a name="how-do-i-access-the-user-risk-policy"></a>Como fazer para acessar a política de risco do usuário?
   
A política de risco de entrada encontra-se na seção **Configurar** na [página do Azure AD Identity Protection](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![Política de risco do usuário](./media/howto-user-risk-policy/1014.png)



## <a name="policy-settings"></a>Configurações de política

Ao configurar a política de risco de entrada, será necessário definir:

- Os usuários e grupos aos quais a política se aplica:

    ![Usuários e grupos](./media/howto-user-risk-policy/11.png)

- O nível de risco de entrada que dispara a política:

    ![Nível de risco do usuário](./media/howto-user-risk-policy/12.png)

- O tipo de acesso que deverá ser imposto quando o nível de risco de entrada for alcançado:  

    ![Access](./media/howto-user-risk-policy/13.png)

- O estado da sua política:

    ![Impor a política](./media/howto-user-risk-policy/14.png)

A caixa de diálogo de configuração de política fornece uma opção para estimar o impacto da sua configuração.

![Impacto estimado](./media/howto-user-risk-policy/15.png)

## <a name="what-you-should-know"></a>O que você deve saber

É possível definir uma política de segurança de risco do usuário para bloquear os usuários ao entrar dependendo do nível do risco.

![Bloqueio](./media/howto-user-risk-policy/16.png)


Bloquear a entrada:

* impede a geração de novos eventos de risco do usuário para o usuário afetado
* Permite aos administradores corrigir os eventos de risco que afetam a identidade do usuário manualmente, bem como restaurá-la para um estado seguro

## <a name="best-practices"></a>Práticas recomendadas

Escolher um limite **Alto** reduz o número de vezes que uma política é disparada e minimiza o impacto para os usuários.
No entanto, isso exclui usuários sinalizados com **Baixo** e **Médio** risco da política, o que pode não proteger as identidades ou os dispositivos que sofreram comprometimento conhecido ou suspeito.

Ao definir a política,

* exclua os usuários que tendem a gerar muitos falsos positivos (desenvolvedores e analistas de segurança)
* exclua os usuários em localidades em que não é viável habilitar a política (por exemplo, sem acesso à assistência técnica)
* use um limite **Alto** durante a distribuição inicial de política ou se você precisar minimizar os desafios encontrados pelos usuários finais.
* use um limite **Baixo** se sua organização exigir uma segurança maior. Selecionar um limite **Baixo** apresenta desafios de entrada do usuário adicionais, porém representa uma segurança maior.

O padrão recomendado na maioria das organizações é configurar uma regra para um limite **Médio** para atingir um equilíbrio entre segurança e usabilidade.

Para obter uma visão geral da experiência do usuário relacionada, confira:

* [Fluxo de recuperação de conta comprometida](flows.md#compromised-account-recovery).  
* [Fluxo de conta comprometida bloqueada](flows.md#compromised-account-blocked).  

**Para abrir o diálogo de configurações relacionadas**:

- Na folha **Azure AD Identity Protection**, na seção **Configurar**, clique em **Política de risco de usuário**.

    ![Política de risco do usuário](./media/howto-user-risk-policy/1009.png "Política de risco do usuário")




## <a name="next-steps"></a>Próximas etapas

Para obter uma visão geral do Azure AD Identity Protection, veja a [Visão geral do Azure AD Identity Protection](overview.md).
