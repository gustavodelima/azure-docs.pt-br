---
title: configurar credenciais de implantação
description: Saiba quais são os tipos de credenciais de implantação no Serviço de Aplicativo do Azure e como configurá-las e usá-las.
ms.topic: article
ms.date: 08/14/2019
ms.reviewer: byvinyal
ms.custom: seodec18
ms.openlocfilehash: e5793d21f27128162095e2d86e13006c5b6e7b7c
ms.sourcegitcommit: 273c04022b0145aeab68eb6695b99944ac923465
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97007986"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Configurar as credenciais de implantação do Serviço de Aplicativo do Azure
O [Serviço de Aplicativo do Azure](./overview.md) oferece suporte a dois tipos de credenciais para a [implantação local do Git](deploy-local-git.md) e a [implantação de FTP/S](deploy-ftp.md). Essas credenciais não são iguais às suas credenciais de assinatura do Azure.

[!INCLUDE [app-service-deploy-credentials](../../includes/app-service-deploy-credentials.md)]

## <a name="configure-user-level-credentials"></a><a name="userscope"></a>Configurar as credenciais de usuário

Você pode configurar as credenciais de usuário na [página de recursos](../azure-resource-manager/management/manage-resources-portal.md#manage-resources) de qualquer aplicativo. Independentemente de para qual aplicativo você configura essas credenciais, elas se aplicam a todos os aplicativos e a todas as assinaturas na conta do Azure. 

### <a name="in-the-cloud-shell"></a>No Cloud Shell

Para configurar o usuário de implantação no [Cloud Shell](https://shell.azure.com), execute o comando [az webapp deployment user set](/cli/azure/webapp/deployment/user#az-webapp-deployment-user-set). Substitua \<username> e \<password> pelo nome de usuário e a senha do usuário de implantação. 

- O nome de usuário deve ser exclusivo no Azure. Para envios por push do Git local, não deve conter o símbolo "\@". 
- A senha deve ter pelo menos oito caracteres, com dois destes três elementos: letras, números, símbolos. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

A saída JSON mostra a senha como `null`. Se receber um erro `'Conflict'. Details: 409`, altere o nome de usuário. Se receber um erro `'Bad Request'. Details: 400`, use uma senha mais forte. 

### <a name="in-the-portal"></a>No portal

No portal do Azure, você deve ter pelo menos um aplicativo antes de poder acessar a página de credenciais de implantação. Para configurar as credenciais de usuário:

1. No [portal do Azure](https://portal.azure.com), no menu à esquerda, selecione **serviços de aplicativo**  >  **\<any_app>**  >  **central de implantação**  >    >  **painel** de FTP.

    ![Mostra como você pode selecionar o painel de FTP na central de implantação no Azure App Services.](./media/app-service-deployment-credentials/access-no-git.png)

    Ou, se você já tiver configurado a implantação do Git, selecione **Serviços de Aplicativos** >  **&lt;qualquer_aplicativo>**  > **Centro de Implantação** > **FTP/Credenciais**.

    ![Mostra como você pode selecionar o painel de FTP na central de implantação no Azure App Services para sua implantação do git configurada.](./media/app-service-deployment-credentials/access-with-git.png)

2. Selecione **Credenciais de Usuário**, configure o nome do usuário e a senha e, em seguida, selecione **Salvar Credenciais**.

Depois que você tiver configurado suas credenciais de implantação, é possível encontrar o nome de usuário de implantação do *Git* na página **Visão geral** de seu aplicativo.

![Mostra como localizar o nome de usuário de implantação do git na página de visão geral do seu aplicativo.](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

Se a implantação do Git estiver configurada, a página mostrará um **Nome de usuário da implantação/Git**; caso contrário, um **Nome de usuário da implantação/FTP**.

> [!NOTE]
> O Azure não mostra sua senha de implantação de usuário. Se você esquecer a senha, poderá redefinir suas credenciais seguindo as etapas desta seção.
>
> 

## <a name="use-user-level-credentials-with-ftpftps"></a>Usar credenciais de usuário com FTP/FTPS

A autenticação em um terminal FTP/FTPS usando credenciais no nível do usuário requer um nome de usuário no seguinte formato: `<app-name>\<user-name>`

Como as credenciais de usuário estão vinculadas ao usuário e não a um recurso específico, o nome de usuário deve estar neste formato para direcionar a ação de entrada para o ponto de extremidade correto do aplicativo.

## <a name="get-and-reset-app-level-credentials"></a><a name="appscope"></a>Definir e redefinir credenciais de aplicativo
Para definir e redefinir credenciais de aplicativo:

1. No [portal do Azure](https://portal.azure.com), no menu à esquerda, selecione **Serviços de Aplicativos** >  **&lt;qualquer_aplicativo>**  > **Centro de Implantação** > **FTP/Credenciais**.

2. Selecione **Credenciais de Aplicativo** e clique no link **Copiar** para copiar o nome de usuário ou senha.

Para redefinir as credenciais de nível de aplicativo, selecione **Redefinir credenciais** na mesma caixa de diálogo.

## <a name="disable-basic-authentication"></a>Desabilitar a autenticação básica

Algumas organizações precisam atender aos requisitos de segurança e, em vez disso, desabilitar o acesso via FTP ou WebDeploy. Dessa forma, os membros da organização só podem acessar seus serviços de aplicativos por meio de APIs controladas pelo Azure Active Directory (AD do Azure).

### <a name="ftp"></a>FTP

Para desabilitar o acesso FTP ao site, execute o comando da CLI a seguir. Substitua os espaços reservados pelo seu grupo de recursos e nome do site. 

```bash
az resource update --resource-group <resource-group> --name ftp --namespace Microsoft.Web --resource-type basicPublishingCredentialsPolicies --parent sites/<site-name> --set properties.allow=false
```

Para confirmar se o acesso ao FTP está bloqueado, você pode tentar autenticar usando um cliente FTP, como o FileZilla. Para recuperar as credenciais de publicação, vá para a folha visão geral do seu site e clique em baixar perfil de publicação. Use o nome de host, o nome de usuário e a senha do FTP do arquivo para autenticar e você receberá uma resposta de erro 401, indicando que você não está autorizado.

### <a name="webdeploy-and-scm"></a>WebDeploy e SCM

Para desabilitar o acesso básico de autenticação para a porta WebDeploy e o site SCM, execute o comando da CLI a seguir. Substitua os espaços reservados pelo seu grupo de recursos e nome do site. 

```bash
az resource update --resource-group <resource-group> --name scm --namespace Microsoft.Web --resource-type basicPublishingCredentialsPolicies --parent sites/<site-name> --set properties.allow=false
```

Para confirmar se as credenciais do perfil de publicação estão bloqueadas no WebDeploy, tente [publicar um aplicativo Web usando o Visual Studio 2019](/visualstudio/deployment/quickstart-deploy-to-azure).

### <a name="disable-access-to-the-api"></a>Desabilitar o acesso à API

A API na seção anterior é apoiada no controle de acesso baseado em função do Azure (RBAC do Azure), o que significa que você pode [criar uma função personalizada](../role-based-access-control/custom-roles.md#steps-to-create-a-custom-role) e atribuir usuários de priveldged menores à função para que eles não possam habilitar a autenticação básica em nenhum site. Para configurar a função personalizada, [siga estas instruções](https://azure.github.io/AppService/2020/08/10/securing-data-plane-access.html#create-a-custom-rbac-role).

Você também pode usar [Azure monitor](https://azure.github.io/AppService/2020/08/10/securing-data-plane-access.html#audit-with-azure-monitor) para auditar quaisquer solicitações de autenticação bem-sucedidas e usar [Azure Policy](https://azure.github.io/AppService/2020/08/10/securing-data-plane-access.html#enforce-compliance-with-azure-policy) para impor essa configuração para todos os sites em sua assinatura.

## <a name="next-steps"></a>Próximas etapas

Saiba como usar essas credenciais para implantar seu aplicativo do [Git local](deploy-local-git.md) ou usando [FTP/S](deploy-ftp.md).
