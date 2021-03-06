---
title: 'Tutorial: integração do Azure Active Directory ao Consent2Go | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Consent2Go.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ea93bc02-58ca-4468-84ff-359888fc6183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2018
ms.author: jeedes
ms.openlocfilehash: 89fc0b69a3a7ca3f795b4ae0e79b11a4bcd9c9fb
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49432195"
---
# <a name="tutorial-azure-active-directory-integration-with-consent2go"></a>Tutorial: integração do Azure Active Directory ao Consent2Go

Neste tutorial, você aprende a integrar o Consent2Go ao Azure AD (Azure Active Directory).

A integração do Consent2Go ao Azure AD oferece os seguintes benefícios:

- no Azure AD, é possível controlar quem tem acesso ao Consent2Go.
- Você pode permitir que seus usuários façam logon automaticamente no Consent2Go (Logon Único) com suas contas do Azure AD.
- Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Consent2Go, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Consent2Go habilitada para logon único

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o Consent2Go da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-consent2go-from-the-gallery"></a>Adicionar o Consent2Go da galeria
Para configurar a integração do Consent2Go ao Azure AD, você precisa adicionar o Consent2Go da galeria à sua lista de aplicativos de SaaS gerenciados.

**Para adicionar o Consent2Go da galeria, execute estas etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![image](./media/consent2go-tutorial/selectazuread.png)

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![image](./media/consent2go-tutorial/a_select_app.png)
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![image](./media/consent2go-tutorial/a_new_app.png)

4. Na caixa de pesquisa, digite **Consent2Go**, selecione **Consent2Go** no painel de resultados e clique no botão **Adicionar** para adicionar o aplicativo.

     ![image](./media/consent2go-tutorial/tutorial_consent2go_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Consent2Go com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Consent2Go é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Consent2Go.

Para configurar e testar o logon único do Azure AD com o Consent2Go, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criar um usuário de teste do Consent2Go](#create-a-consent2go-test-user)**: para ter um equivalente de Brenda Fernandes no Consent2Go que esteja vinculado à representação de usuário do Azure AD.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no novo Portal do Azure e configura o logon único em seu aplicativo Consent2Go.

**Para configurar o logon único do Azure AD com o Consent2Go, execute estas etapas:**

1. No [Portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Consent2Go**, selecione **Logon único**.

    ![image](./media/consent2go-tutorial/b1_b2_select_sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, clique em **Selecionar** do modo **SAML** para habilitar o logon único.

    ![image](./media/consent2go-tutorial/b1_b2_saml_sso.png)

3. Na página **Definir logon único com SAML**, clique no botão **Editar** para abrir a caixa de diálogo **Configuração básica de SAML**.

    ![image](./media/consent2go-tutorial/b1-domains_and_urlsedit.png)

4. Na seção **Configuração básica de SAML**, realize as seguintes etapas:

    No **URL de logon** caixa de texto, digite uma URL como: `   https://www.mcbschools.com/Login`

    ![image](./media/consent2go-tutorial/tutorial_consent2go_url.png)

5. Na página **Configurar logon único com SAML**, na seção **Certificado de Autenticação SAML**, clique no ícone para Copiar a **URL de metadados de federação de aplicativos** e salve-a no computador.

    ![image](./media/consent2go-tutorial/tutorial_consent2go_certificate.png) 

6. Para configurar o logon único no lado do **Consent2Go**, é necessário enviar a **URL de metadados de federação de aplicativos** copiada para a [equipe de suporte do Consent2Go](mailto:support@consent2go.com). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory**, selecione **Usuários** e, em seguida, **Todos os usuários**.

    ![image](./media/consent2go-tutorial/d_users_and_groups.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![image](./media/consent2go-tutorial/d_adduser.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![image](./media/consent2go-tutorial/d_userproperties.png)

    a. No campo **Nome**, insira **BrendaFernandes**.
  
    b. No campo **Nome de usuário**, digite **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **Propriedades**, marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Selecione **Criar**.
 
### <a name="create-a-consent2go-test-user"></a>Criar um usuário de teste do Consent2Go

Nesta seção, você criará um usuário chamado Brenda Fernandes no Consent2Go. Trabalhe com a [equipe de suporte do Consent2Go](mailto:support@consent2go.com) para adicionar os usuários à plataforma do Consent2Go. Os usuários devem ser criados e ativados antes de usar o logon único.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao Consent2Go.

1. No Portal do Azure, selecione **Aplicativos Empresariais**, selecione **Todos os aplicativos**.

    ![image](./media/consent2go-tutorial/d_all_applications.png)

2. Na lista de aplicativos, selecione **Consent2Go**.

    ![image](./media/consent2go-tutorial/tutorial_consent2go_app.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![image](./media/consent2go-tutorial/d_leftpaneusers.png)

4. Selecione o botão **Adicionar** e, em seguida, selecione **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![image](./media/consent2go-tutorial/d_assign_user.png)

4. Na caixa de diálogo **Usuários e grupos**, selecione **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

5. Na caixa de diálogo **Adicionar Atribuição** selecione o botão **Atribuir**.
    
### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clica no bloco Consent2Go no Painel de Acesso, deve fazer logon automaticamente no seu aplicativo do Consent2Go.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


