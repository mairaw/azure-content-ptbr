﻿<properties 
	pageTitle="Como configurar a autenticação de conta da Microsoft para o seu aplicativo de Serviços de Aplicativos"
	description="Saiba como configurar a autenticação de conta da Microsoft para o seu aplicativo de Serviços de Aplicativos." 
	authors="mattchenderson,ggailey777" 
	services="app-service\mobile" 
	documentationCenter="" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="app-service-mobile" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="na" 
	ms.devlang="multiple" 
	ms.topic="article" 
	ms.date="02/19/2015" 
	ms.author="mahender"/>

# Como configurar seu aplicativo para usar o logon da conta da Microsoft

Este tópico mostra como configurar os Serviços de Aplicativos do Azure para usar a conta da Microsoft como um provedor de autenticação. 

## <a name="register"> </a>Registrar seu aplicativo com a conta da Microsoft

1. Navegue até a página [meus aplicativos] no Centro de Desenvolvedoras da conta da Microsoft e faça logon com sua conta da Microsoft, se necessário. 

2. Clique em **Criar aplicativo**, digite um **Nome de aplicativo** e clique em **Aceito**.

3. Clique em **Configurações da API**.  Selecione **Sim** para **Aplicativo cliente de celular ou desktop**.  No campo **URL de redirecionamento**, insira a URL do seu gateway anexada ao caminho, _/signin-microsoft_.  Por exemplo, `https://contosogateway.azurewebsites.net/signin-microsoft`.  Certifique-se de estar usando o esquema HTTPS.  Depois de inserir a URL de redirecionamento, clique em **Salvar**.

	![][0]

	>[AZURE.NOTE]Para um registro de aplicativo da conta da Microsoft existente, você precisará primeiro habilitar a **Segurança de redirecionamento avançada**.

4. Clique em **Configurações do Aplicativo** e anote os valores de **ID do Cliente** e **Segredo do Cliente**. 

    > [AZURE.NOTE] O segredo do cliente é uma credencial de segurança importante.  Não compartilhe o segredo do cliente com ninguém nem distribua-o em um aplicativo cliente.


## <a name="secrets"> </a>Adicionar informações da conta da Microsoft ao seu aplicativo móvel

5. Faça logon no [Portal de Gerenciamento do Azure] e navegue para o gateway de Serviços de Aplicativos.

6. Em **Configurações**, escolha **Identidade**e, em seguida, selecione **Conta da Microsoft**.  Cole os valores de ID do Aplicativo e Segredo do Aplicativo que você obteve anteriormente.  Em seguida, clique em **Salvar**.

    ![][1]

Agora você está pronto para usar uma conta da Microsoft para autenticação em seu aplicativo.

## <a name="related-content"> </a>Conteúdo relacionado

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Authenticate your app with Live Connect Single Sign-On: [Windows](windows-liveconnect) -->



<!-- Images. -->

[0]: ./media/app-service-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-how-to-configure-microsoft-authentication/app-service-microsoftaccount-settings.png

<!-- URLs. -->

[Meus Aplicativos]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Portal de Gerenciamento do Azure]: https://portal.azure.com/

<!--HONumber=49-->