---
title: 'Tutorial: Integração do SSO (logon único) do Azure Active Directory ao CloudKnox Permissions Management Platform | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o CloudKnox Permissions Management Platform.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/27/2021
ms.author: jeedes
ms.openlocfilehash: 724bb91b4295162b32822d36d82958638d976e09
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99056690"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cloudknox-permissions-management-platform"></a>Tutorial: Integração do SSO (logon único) do Azure Active Directory ao CloudKnox Permissions Management Platform

Neste tutorial, você aprenderá a integrar o CloudKnox Permissions Management Platform ao Azure AD (Azure Active Directory). Quando você integrar o CloudKnox Permissions Management Platform ao Azure AD, você poderá:

* Controlar no Azure AD quem tem acesso ao CloudKnox Permissions Management Platform.
* Permitir que os usuários sejam conectados automaticamente ao CloudKnox Permissions Management Platform com as respectivas contas do Azure AD.
* Gerenciar suas contas em um local central: o portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Assinatura habilitada para SSO (logon único) do CloudKnox Permissions Management Platform.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste.

* O CloudKnox Permissions Management Platform é compatível com o SSO iniciado por **IdP**

> [!NOTE]
> O identificador desse aplicativo é um valor de cadeia de caracteres fixo; portanto apenas uma instância pode ser configurada em um locatário.

## <a name="adding-cloudknox-permissions-management-platform-from-the-gallery"></a>Adicionar o CloudKnox Permissions Management Platform da galeria

Para configurar a integração do CloudKnox Permissions Management Platform ao Azure AD, você precisa adicionar o CloudKnox Permissions Management Platform da galeria à sua lista de aplicativos de SaaS gerenciados.

1. Entre no portal do Azure usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. No painel de navegação esquerdo, escolha o serviço **Azure Active Directory**.
1. Navegue até **Aplicativos Empresariais** e, em seguida, escolha **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, escolha **Novo aplicativo**.
1. Na seção **Adicionar da galeria**, digite **CloudKnox Permissions Management Platform** na caixa de pesquisa.
1. Selecione **CloudKnox Permissions Management Platform** no painel de resultados e adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.


## <a name="configure-and-test-azure-ad-sso-for-cloudknox-permissions-management-platform"></a>Configurar e testar o SSO do Azure AD para o CloudKnox Permissions Management Platform

Configure e teste o SSO do Azure AD com o CloudKnox Permissions Management Platform usando um usuário de teste chamado **B.Fernandes**. Para que o SSO funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do CloudKnox Permissions Management Platform.

Para configurar e testar o SSO do Azure AD com o CloudKnox Permissions Management Platform, execute as seguintes etapas:

1. **[Configurar o SSO do Azure AD](#configure-azure-ad-sso)** – para permitir que os usuários usem esse recurso.
    1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD com B.Fernandes.
    1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que B.Fernandes use o logon único do Azure AD.
1. **[Configurar o SSO do CloudKnox Permissions Management Platform](#configure-cloudknox-permissions-management-platform-sso)** – para configurar o logon único no lado do aplicativo.
    1. **[Criar um usuário de teste do CloudKnox Permissions Management Platform](#create-cloudknox-permissions-management-platform-test-user)** – para ter um equivalente de B.Fernandes no CloudKnox Permissions Management Platform que esteja vinculado à representação do usuário do Azure AD.
1. **[Testar o SSO](#test-sso)** – para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No portal do Azure, na página de integração de aplicativos do **CloudKnox Permissions Management Platform**, localize a seção **Gerenciar** e selecione **logon único**.
1. Na página **Selecionar um método de logon único**, escolha **SAML**.
1. Na página **Configurar o logon único com o SAML**, clique no ícone de caneta da **Configuração Básica do SAML** para editar as configurações.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Na página **Configurar logon único com SAML**, insira os valores para os seguintes campos:

    Na caixa de texto **URL de Resposta**, digite uma URL usando o seguinte padrão: `https://app.cloudknox.io/saml/<ID>`

    > [!NOTE]
    > O valor de URL de Resposta não é real. Atualize o valor com a URL de Resposta real. Entre em contato com a [equipe de suporte ao cliente do CloudKnox Permissions Management Platform](mailto:support@cloudknox.io) para obter o valor. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

1. O aplicativo CloudKnox Permissions Management Platform espera declarações SAML em um formato específico, que requer que você adicione mapeamentos de atributo personalizados à sua configuração de atributos do token SAML. A captura de tela a seguir mostra a lista de atributos padrão.

    ![image](common/default-attributes.png)

1. Além do indicado acima, o aplicativo CloudKnox Permissions Management Platform espera que mais alguns atributos sejam passados novamente na resposta SAML, que são mostrados abaixo. Esses atributos também são pré-populados, mas você pode examiná-los de acordo com seus requisitos.
    
    | Nome |  Atributo de origem|
    | --------------- | --------- |
    | First_Name | user.givenname |
    | Grupos | user.groups |
    | Last_Name | user.surname |
    | Email_Address | user.mail |

1. Na página **Configurar o logon único com o SAML**, na seção **Certificado de Autenticação SAML**, localize **XML de Metadados de Federação** e selecione **Baixar** para baixar o certificado e salvá-lo no computador.

    ![O link de download do Certificado](common/metadataxml.png)

1. Na seção **Configurar o CloudKnox Permissions Management Platform**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você criará um usuário de teste no portal do Azure chamado B.Fernandes.

1. No painel esquerdo do portal do Azure, escolha **Azure Active Directory**, **Usuários** e, em seguida, **Todos os usuários**.
1. Selecione **Novo usuário** na parte superior da tela.
1. Nas propriedades do **Usuário**, siga estas etapas:
   1. No campo **Nome**, insira `B.Simon`.  
   1. No campo **Nome de usuário**, insira username@companydomain.extension. Por exemplo, `B.Simon@contoso.com`.
   1. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa **Senha**.
   1. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que B.Fernandes use o logon único do Azure permitindo acesso ao CloudKnox Permissions Management Platform.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos**.
1. Na lista de aplicativos, selecione **CloudKnox Permissions Management Platform**.
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e escolha **Usuários e grupos**.
1. Escolha **Adicionar usuário** e, em seguida, **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.
1. Na caixa de diálogo **Usuários e grupos**, selecione **B.Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.
1. Se você estiver esperando que uma função seja atribuída aos usuários, escolha-a na lista suspensa **Selecionar uma função**. Se nenhuma função tiver sido configurada para esse aplicativo, você verá a função "Acesso Padrão" selecionada.
1. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

## <a name="configure-cloudknox-permissions-management-platform-sso"></a>Configurar o SSO do CloudKnox Permissions Management Platform

Para configurar o logon único no lado do **CloudKnox Permissions Management Platform**, é necessário enviar o **XML de Metadados de Federação** baixado e as URLs apropriadas copiadas do portal do Azure para a [equipe de suporte do CloudKnox Permissions Management Platform](mailto:support@cloudknox.io). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-cloudknox-permissions-management-platform-test-user"></a>Criar um usuário de teste do CloudKnox Permissions Management Platform

Nesta seção, você criará uma usuária chamada Brenda Fernandes no CloudKnox Permissions Management Platform. Trabalhe com a [equipe de suporte do CloudKnox Permissions Management Platform](mailto:support@cloudknox.io) para adicionar os usuários na plataforma do CloudKnox Permissions Management Platform. Os usuários devem ser criados e ativados antes de usar o logon único.

## <a name="test-sso"></a>Testar o SSO 

Nesta seção, você testará a configuração de logon único do Azure AD com as opções a seguir.

* Clique em Testar este aplicativo no portal do Azure e você deverá ser conectado automaticamente ao CloudKnox Permissions Management Platform, para o qual configurou o SSO

* Você pode usar os Meus Aplicativos da Microsoft. Quando você clicar no bloco do CloudKnox Permissions Management Platform em Meus Aplicativos, você deverá ser conectado automaticamente ao CloudKnox Permissions Management Platform para o qual configurou o SSO. Para obter mais informações sobre os Meus Aplicativos, confira [Introdução aos Meus Aplicativos](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).


## <a name="next-steps"></a>Próximas etapas

Depois de configurar o CloudKnox Permissions Management Platform, você poderá impor controles de sessão, que fornecem proteção contra exfiltração e infiltração dos dados confidenciais da sua organização em tempo real. O controle da sessão é estendido do acesso condicional. [Saiba como impor o controle de sessão com o Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).


