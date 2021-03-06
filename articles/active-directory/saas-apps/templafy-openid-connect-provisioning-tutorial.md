---
title: 'Tutorial: Configurar o Templafy OpenID Connect para o provisionamento automático de usuário com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para provisionar e desprovisionar automaticamente contas de usuário no Templafy OpenID Connect.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.assetid: 8cbb387a-e3fb-4588-bb87-bf4f88144361
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/19/2021
ms.author: zhchia
ms.openlocfilehash: 7afcf6c5814917b3356d86e7288fd2920a04cad7
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98695985"
---
# <a name="tutorial-configure-templafy-openid-connect-for-automatic-user-provisioning"></a>Tutorial: Configurar o Templafy OpenID Connect para o provisionamento automático de usuário

O objetivo deste tutorial é demonstrar as etapas a serem executadas no Templafy OpenID Connect e no Azure AD (Azure Active Directory) a fim de configurar o Azure AD para provisionar e desprovisionar automaticamente usuários e/ou grupos no Templafy OpenID Connect.

> [!NOTE]
> Este tutorial descreve um conector compilado na parte superior do Serviço de Provisionamento de Usuário do Microsoft Azure AD. Para detalhes importantes sobre o que esse serviço faz, como funciona e as perguntas frequentes, consulte [Automatizar o provisionamento e desprovisionamento de usuários para aplicativos SaaS com o Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Atualmente, esse conector está em versão prévia pública. Para obter mais informações sobre os Termos de uso gerais do Microsoft Azure para a versão prévia de recursos, confira [Termos de uso adicionais para versões prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial pressupõe que você já tem os seguintes pré-requisitos:

* Um locatário do Azure AD.
* [Um locatário do Templafy](https://www.templafy.com/pricing/).
* Uma conta de usuário no Templafy com permissões de Administrador.

## <a name="step-1-plan-your-provisioning-deployment"></a>Etapa 1. Planeje a implantação do provisionamento
1. Saiba mais sobre [como funciona o serviço de provisionamento](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Determine quem estará no [escopo de provisionamento](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Determine quais dados serão [mapeados entre o Azure AD e o Templafy OpenID Connect](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="assigning-users-to-templafy-openid-connect"></a>Como atribuir usuários ao Templafy OpenID Connect

O Azure Active Directory usa um conceito chamado *atribuições* para determinar quais usuários devem receber acesso aos aplicativos selecionados. No contexto do provisionamento automático de usuário, somente os usuários e/ou os grupos que foram atribuídos a um aplicativo no Azure AD são sincronizados.

Antes de configurar e habilitar o provisionamento automático de usuário, decida quais usuários e/ou grupos no Azure AD precisam ter acesso ao Templafy OpenID Connect. Depois de decidir isso, você poderá atribuir esses usuários e/ou grupos ao Templafy OpenID Connect seguindo estas instruções:
* [Atribuir um usuário ou um grupo a um aplicativo empresarial](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-templafy-openid-connect"></a>Dicas importantes para atribuir usuários ao Templafy OpenID Connect

* Recomendamos que um só usuário do Azure AD seja atribuído ao Templafy OpenID Connect para testar a configuração de provisionamento automático de usuário. Outros usuários e/ou grupos podem ser atribuídos mais tarde.

* Ao atribuir um usuário ao Templafy OpenID Connect, você precisará selecionar qualquer função válida específica do aplicativo (se disponível) na caixa de diálogo de atribuição. Usuários com a função **Acesso padrão** são excluídos do provisionamento.

## <a name="step-2-configure-templafy-openid-connect-to-support-provisioning-with-azure-ad"></a>Etapa 2. Configurar o Templafy OpenID Connect para dar suporte ao provisionamento com o Azure AD

Antes de configurar o Templafy OpenID Connect para o provisionamento automático de usuário com o Azure AD, você precisará habilitar o provisionamento do SCIM no Templafy OpenID Connect.

1. Entre no Console de Administração do Templafy. Clique em **Administração**.

    ![Console de Administração do Templafy](media/templafy-openid-connect-provisioning-tutorial/templafy-admin.png)

2. Clique em **Método de Autenticação**.

    ![Captura de tela da seção Administração do Templafy com a opção Método de autenticação em destaque.](media/templafy-openid-connect-provisioning-tutorial/templafy-auth.png)

3. Copie o valor da **Chave de API do SCIM**. Esse valor será inserido no campo **Token Secreto** na guia Provisionamento do aplicativo Templafy OpenID Connect no portal do Azure.

    ![Uma captura de tela da chave de API do SCIM.](media/templafy-openid-connect-provisioning-tutorial/templafy-token.png)

## <a name="step-3-add-templafy-openid-connect-from-the-gallery"></a>Etapa 3. Adicionar o Templafy OpenID Connect por meio da galeria

Para configurar o Templafy OpenID Connect para o provisionamento automático de usuário com o Azure AD, é necessário adicionar o Templafy OpenID Connect por meio da galeria de aplicativos do Azure AD à lista de aplicativos SaaS gerenciados.

**Para adicionar o Templafy OpenID Connect por meio da galeria de aplicativos do Azure AD, execute as seguintes etapas:**

1. No **[portal do Azure](https://portal.azure.com)** , no painel de navegação esquerdo, selecione **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Vá para **Aplicativos da empresa**, em seguida, selecione **Todos os aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Para adicionar um novo aplicativo, selecione o botão **Novo aplicativo** na parte superior do painel.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, insira **Templafy OpenID Connect**, selecione **Templafy OpenID Connect** no painel de resultados e clique no botão **Adicionar** para adicionar o aplicativo.

    ![Templafy OpenID Connect na lista de resultados](common/search-new-app.png)

## <a name="step-4-configure-automatic-user-provisioning-to-templafy-openid-connect"></a>Etapa 4. Configurar o provisionamento automático de usuário para o Templafy OpenID Connect 

Esta seção descreve as etapas de configuração do serviço de provisionamento do Azure AD para criar, atualizar e desabilitar usuários e/ou grupos no Templafy OpenID Connect com base em atribuições de usuário e/ou de grupo no Azure AD.

> [!TIP]
> Você também pode optar por habilitar o logon único baseado no OpenID Connect para o Templafy seguindo as instruções fornecidas no [Tutorial de logon único do Templafy](templafy-tutorial.md). O logon único pode ser configurado independentemente do provisionamento automático de usuário, embora esses dois recursos sejam complementares.

### <a name="to-configure-automatic-user-provisioning-for-templafy-openid-connect-in-azure-ad"></a>Para configurar o provisionamento automático de usuário para o Templafy OpenID Connect no Azure AD:

1. Entre no [portal do Azure](https://portal.azure.com). Selecione **Aplicativos Empresariais** e **Todos os Aplicativos**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, escolha **Templafy OpenID Connect**.

    ![O link do Templafy OpenID Connect na lista Aplicativos](common/all-applications.png)

3. Selecione a guia **Provisionamento**.

    ![Captura de tela das opções Gerenciar com a opção Provisionamento destacada.](common/provisioning.png)

4. Defina o **Modo de Provisionamento** como **Automático**.

    ![Captura de tela da lista suspensa Modo de Provisionamento com a opção Automático destacada.](common/provisioning-automatic.png)

5. Na seção **Credenciais de Administrador**, insira `https://scim.templafy.com/scim` em **URL do Locatário**. Insira o valor da **Chave de API do SCIM** recuperado anteriormente em **Token Secreto**. Clique em **Testar Conectividade** para verificar se o Azure AD pode se conectar ao Templafy. Se a conexão falhar, verifique se a sua conta do Templafy tem permissões de Administrador e tente novamente.

    ![URL do locatário + token](common/provisioning-testconnection-tenanturltoken.png)

6. No campo **Notificação por Email**, insira o endereço de email de uma pessoa ou grupo que deverá receber as notificações de erro de provisionamento e selecione a caixa de seleção - **Enviar uma notificação por email quando ocorrer uma falha**.

    ![Email de notificação](common/provisioning-notification-email.png)

7. Clique em **Save** (Salvar).

8. Na seção **Mapeamentos**, selecione **Sincronizar Usuários do Azure Active Directory com o Templafy OpenID Connect**.

    ![Mapeamentos de Usuários no Templafy OpenID Connect](media/templafy-openid-connect-provisioning-tutorial/user-mapping.png)

9. Examine os atributos de usuário que serão sincronizados do Azure AD para o Templafy OpenID Connect na seção **Mapeamento de Atributos**. Os atributos selecionados como propriedades **Correspondentes** são usados para fazer a correspondência das contas de usuário no Templafy OpenID Connect em operações de atualização. Selecione o botão **Salvar** para confirmar as alterações.

   |Atributo|Type|Com suporte para filtragem|
   |---|---|---|
   |userName|String|&check;|
   |ativo|Boolean|
   |displayName|String|
   |título|String|
   |preferredLanguage|String|
   |name.givenName|String|
   |name.familyName|String|
   |phoneNumbers[type eq "work"].value|String|
   |phoneNumbers[type eq "mobile"].value|String|
   |phoneNumbers[type eq "fax"].value|String|
   |externalId|String|
   |addresses[type eq "work"].locality|String|
   |addresses[type eq "work"].postalCode|String|
   |addresses[type eq "work"].region|String|
   |addresses[type eq "work"].streetAddress|String|
   |addresses[type eq "work"].country|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|String|

10. Na seção **Mapeamentos**, selecione **Sincronizar Grupos do Azure Active Directory com o Templafy**.

    ![Mapeamentos de Grupos no Templafy OpenID Connect](media/templafy-openid-connect-provisioning-tutorial/group-mapping.png)

11. Examine os atributos de grupo que serão sincronizados do Azure AD para o Templafy OpenID Connect na seção **Mapeamento de Atributos**. Os atributos selecionados como propriedades **Correspondentes** são usados para fazer a correspondência dos grupos no Templafy OpenID Connect em operações de atualização. Selecione o botão **Salvar** para confirmar as alterações.

      |Atributo|Type|Com suporte para filtragem|
      |---|---|---|
      |displayName|String|&check;|
      |membros|Referência|
      |externalId|String|      

12. Para configurar filtros de escopo, consulte as seguintes instruções fornecidas no [tutorial do Filtro de Escopo](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar o serviço de provisionamento do Azure AD no Templafy OpenID Connect, altere o **Status de Provisionamento** para **Ativado** na seção **Configurações**.

    ![Status do provisionamento ativado](common/provisioning-toggle-on.png)

14. Defina os usuários e/ou os grupos que deseja provisionar no Templafy OpenID Connect escolhendo os valores desejados em **Escopo** na seção **Configurações**.

    ![Escopo de provisionamento](common/provisioning-scope.png)

15. Quando estiver pronto para provisionar, clique em **Salvar**.

    ![Salvando a configuração de provisionamento](common/provisioning-configuration-save.png)

    Essa operação inicia a sincronização inicial de todos os usuários e/ou grupos definidos no **Escopo** na seção **Configurações**. Observe que a sincronização inicial levará mais tempo do que as sincronizações subsequentes, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço de provisionamento do Microsoft Azure Active Directory esteja em execução. Use a seção **Detalhes de Sincronização** para monitorar o progresso e siga os links para o relatório das atividades de provisionamento, que descreve todas as ações executadas pelo serviço de provisionamento do Azure AD no Templafy OpenID Connect.

## <a name="step-5-monitor-your-deployment"></a>Etapa 5. Monitorar a implantação
Depois de configurar o provisionamento, use os seguintes recursos para monitorar a implantação:

* Use os [logs de provisionamento](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) para determinar quais usuários foram provisionados com êxito ou não
* Confira a [barra de progresso](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) para ver o status do ciclo de provisionamento e saber como fechá-la para concluir
* Se a configuração de provisionamento parecer estar em um estado não íntegro, o aplicativo entrará em quarentena. Saiba mais sobre os estados de quarentena [aqui](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciamento do provisionamento de conta de usuário para Aplicativos Empresariais](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Próximas etapas

* [Saiba como fazer revisão de logs e obter relatórios sobre atividade de provisionamento](../app-provisioning/check-status-user-account-provisioning.md)
