---
title: 'Tutorial: integração do Azure Active Directory com o Icertis Contract Management Platform | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Icertis Contract Management Platform.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6627e6dd-f559-4cd4-a509-f6d9a4961b49
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 8a158f2edb5fd9c9b72f4de2dcb27cf6b3c71874
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444977"
---
# <a name="tutorial-azure-active-directory-integration-with-icertis-contract-management-platform"></a>Tutorial: integração do Azure Active Directory com o Icertis Contract Management Platform

Neste tutorial, você aprenderá a integrar a Icertis Contract Management Platform ao Azure AD (Azure Active Directory).

A integração do Icertis Contract Management Platform ao Azure AD fornece os seguintes benefícios:

- Você pode controlar no Azure AD, quem tem acesso ao Icertis Contract Management Platform
- Você pode permitir que seus usuários entrem automaticamente no Icertis Contract Management Platform (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Icertis Contract Management Platform, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura com logon único habilitado da Icertis Contract Management Platform

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar a Plataforma de Gerenciamento de Contratos Icertis da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-icertis-contract-management-platform-from-the-gallery"></a>Adicionar a Plataforma de Gerenciamento de Contratos Icertis da galeria
Para configurar a integração do Icertis Contract Management Platform ao Azure AD, você precisa adicionar o Icertis Contract Management Platform da galeria à lista de aplicativos SaaS gerenciados.

**Para adicionar o Icertis Contract Management Platform da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

1. Na caixa de pesquisa, digite **Icertis Contract Management Platform**.

    ![Criação de um usuário de teste do AD do Azure](./media/icertisicm-tutorial/tutorial_icertisicm_search.png)

1. No painel de resultados, selecione **Icertis Contract Management Platform** e clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/icertisicm-tutorial/tutorial_icertisicm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com a Icertis Contract Management Platform com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário da Icertis Contract Management Platform é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Icertis Contract Management Platform.

No Icertis Contract Management Platform, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Icertis Contract Management Platform, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
1. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** – para testar o logon único do AD do Azure com Brenda Fernandes.
1. **[Como criar um usuário de teste da Icertis Contract Management Platform](#creating-an-icertis-contract-management-platform-test-user)** – para ter um equivalente de Brenda Fernandes na Icertis Contract Management Platform vinculado à representação do usuário no Azure AD.
1. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
1. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure e configurará o logon único em seu aplicativo Icertis Contract Management Platform.

**Para configurar o logon único do Azure AD com o Icertis Contract Management Platform, execute as seguintes etapas:**

1. No portal do Azure, na página de integração de aplicativos do **Icertis Contract Management Platform**, clique em **Logon único**.

    ![Configurar o logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/icertisicm-tutorial/tutorial_icertisicm_samlbase.png)

1. Na seção **URLs e Domínio da Icertis Contract Management Platform**, execute as etapas a seguir:

    ![Configurar o logon único](./media/icertisicm-tutorial/tutorial_icertisicm_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<company name>.icertis.com`

    b. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<company name>.icertis.com`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com a URL de Entrada e o Identificador reais. Entre em contato com a [equipe de suporte ao Cliente da Icertis Contract Management Platform](https://www.icertis.com/company/contact/) para obter esses valores. 

1. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/icertisicm-tutorial/tutorial_icertisicm_certificate.png) 

1. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/icertisicm-tutorial/tutorial_general_400.png)

1. Na seção **Configuração da Icertis Contract Management Platform**, clique em **Configurar Icertis Contract Management Platform** para abrir a janela **Configurar logon**. Copie a **URL de saída, a ID da Entidade SAML e a URL do Serviço de Logon Único SAML** da **seção de Referência Rápida.**

    ![Configurar o logon único](./media/icertisicm-tutorial/tutorial_icertisicm_configure.png) 

1. Para configurar o logon único no lado da **Icertis Contract Management Platform**, você precisa enviar o **XML de Metadados** baixado e **URL de logoff, ID de Entidade do SAML e URL de Serviço de Logon Único do SAML** para a [equipe de suporte à Icertis Contract Management Platform](https://www.icertis.com/company/contact/).

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/icertisicm-tutorial/create_aaduser_01.png) 

1. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/icertisicm-tutorial/create_aaduser_02.png) 

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/icertisicm-tutorial/create_aaduser_03.png) 

1. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/icertisicm-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-an-icertis-contract-management-platform-test-user"></a>Como criar um usuário de teste da Icertis Contract Management Platform

Nesta seção, você deve criar uma usuária chamada Brenda Fernandes no Icertis Contract Management Platform. Trabalhe com a [equipe de suporte à Icertis Contract Management Platform](https://www.icertis.com/company/contact/) para adicionar os usuários à Icertis Contract Management Platform.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você habilita Brenda Fernandes a usar logon único do Azure concedendo a ela acesso à Icertis Contract Management Platform.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Icertis Contract Management Platform, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **Icertis Contract Management Platform**.

    ![Configurar o logon único](./media/icertisicm-tutorial/tutorial_icertisicm_app.png) 

1. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

O objetivo desta seção é testar sua configuração de SSO do Azure AD usando o Painel de Acesso.

Ao clicar no bloco Icertis Contract Management Platform, você deve ser conectado automaticamente ao aplicativo Icertis Contract Management Platform.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/icertisicm-tutorial/tutorial_general_01.png
[2]: ./media/icertisicm-tutorial/tutorial_general_02.png
[3]: ./media/icertisicm-tutorial/tutorial_general_03.png
[4]: ./media/icertisicm-tutorial/tutorial_general_04.png

[100]: ./media/icertisicm-tutorial/tutorial_general_100.png

[200]: ./media/icertisicm-tutorial/tutorial_general_200.png
[201]: ./media/icertisicm-tutorial/tutorial_general_201.png
[202]: ./media/icertisicm-tutorial/tutorial_general_202.png
[203]: ./media/icertisicm-tutorial/tutorial_general_203.png

