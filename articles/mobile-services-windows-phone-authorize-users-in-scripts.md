﻿<properties 
	pageTitle="Autorização no lado do serviço (Windows Phone) | Centro de desenvolvimento de Serviços Móveis" 
	description="Saiba como autorizar usuários no back-end do JavaScript dos Serviços Móveis do Azure." 
	services="" 
	documentationCenter="windows" 
	authors="ggailey777" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="mobile-services" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="mobile-windows-phone" 
	ms.devlang="dotnet" 
	ms.topic="article" 
	ms.date="09/29/2014" 
	ms.author="glenga"/>

# Autorização de atendimento a usuários dos Serviços Móveis

[AZURE.INCLUDE [mobile-services-selector-service-auth-users](../includes/mobile-services-selector-service-auth-users.md)]	


<div class="dev-onpage-video-clear clearfix">
<div class="dev-onpage-left-content">

<p>Este tópico mostra como usar scripts de servidor para autorizar usuários autenticados a acessar dados nos Serviços Móveis do Azure por meio de um aplicativo do Windows Phone 8. Neste tutorial, você registra scripts nos Serviços Móveis para filtrar consultas com base na userId de um usuário autenticado, garantindo que cada usuário possa ver apenas seus próprios dados.</p>
<p>Este tutorial baseia-se no início rápido dos Serviços Móveis e no tutorial anterior <a href="/pt-br/develop/mobile/tutorials/get-started-with-users-wp8">Introdução à autenticação</a>. Antes de iniciar este tutorial, você deve primeiro concluir o tutorial <a href="/pt-br/develop/mobile/tutorials/get-started-with-users-wp8">Introdução à autenticação</a>.</p>
</div>
<div class="dev-onpage-video-wrapper"><a href="http://go.microsoft.com/fwlink/?LinkId=298630" target="_blank" class="label">assista ao tutorial</a><a style="background-image: url('/media/devcenter/mobile/videos/mobile-wp8-scripts-for-authentication-180x120.png') !important;" href="http://go.microsoft.com/fwlink/?LinkId=298630" target="_blank" class="dev-onpage-video"><span class="icon">Executar o vídeo</span></a> <span class="time">15:00</span></div>
</div> 

## <a name="register-scripts"></a>Registrar scripts
Como o aplicativo Guia de início rápido lê e insere dados, você precisa registrar scripts para essas operações com base na tabela TodoItem.

1. Faça logon no [Portal de Gerenciamento do Azure], clique em **Serviços Móveis** e clique em seu aplicativo. 

   	![][0]

2. Clique na guia **Dados** e clique na tabela **TodoItem**.

   	![][1]

3. Clique em **Script** e selecione a operação **Inserir**.

   	![][2]

4. Substitua o script existente pela função a seguir e clique em **Salvar**.

        function insert(item, user, request) {
          item.userId = user.userId;    
          request.execute();
        }

    Este script adiciona um valor de userId ao item que é a ID de usuário autenticado, antes que ele seja inserido na tabela TodoItem. 

    > [AZURE.NOTE] O esquema dinâmico deve ser habilitado na primeira vez que esse script de inserção for executado. Com o esquema dinâmico habilitado, os Serviços Móveis automaticamente adicionam a coluna **userId** à tabela **TodoItem** na primeira execução. Por padrão, o esquema dinâmico é habilitado para um novo serviço móvel e deve ser desabilitado antes que o aplicativo seja publicado na Windows Phone Store.


5. Repita as etapas 3 e 4 para substituir a operação **Ler** pela função a seguir:

        function read(query, user, request) {
           query.where({ userId: user.userId });    
           request.execute();
        }

   	Esse script filtra os objetos TodoItem retornados para que cada usuário receba apenas os itens inseridos por ele.

## Testar o aplicativo

1. No Visual Studio 2012 Express para Windows Phone, abra o projeto que você criou quando concluiu o tutorial [Introdução à autenticação].

2. Pressione a tecla F5 para executar o aplicativo e fazer o logon com seu provedor de identidade. 

   	Observe que, desta vez, embora já existam itens na tabela TodoItem de tutoriais anteriores, nenhum item é retornado. Isso acontece porque os itens anteriores foram inseridos sem a coluna userId e agora têm valores nulos.

3. No aplicativo, insira o texto na caixa de texto e clique em **Salvar**.

   	![][3]

   	Isso insere o texto e a userId na tabela TodoItem no serviço móvel. Como o novo item tem o valor correto de userId, o mesmo é retornado pelo serviço móvel.

5. De volta à tabela **todoitem** no [Portal de Gerenciamento][Portal de Gerenciamento do Azure], clique em **Procurar** e verifique se cada item recém-adicionado agora tem um valor de userId associado.

## Próximas etapas

Isso conclui os tutoriais que demonstram os conceitos básicos de como trabalhar com autenticação. Considere a possibilidade de obter mais informações sobre os seguintes tópicos de Serviços Móveis:

* [Introdução aos dados]
  <br/>Saiba mais sobre como armazenar e consultar dados usando os Serviços Móveis.

* [Introdução às notificações por push] 
  <br/>Saiba como enviar uma notificação por push bastante básica a seu aplicativo.

* [Referência de script de servidor dos Serviços Móveis]
  <br/>Saiba mais sobre como registrar e usar scripts de servidor.

<!-- Anchors. -->
[Registrar scripts de servidor]: #register-scripts
[Próximas etapas]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-windows-phone-authorize-users-in-scripts/mobile-services-selection.png
[1]: ./media/mobile-services-windows-phone-authorize-users-in-scripts/mobile-portal-data-tables.png
[2]: ./media/mobile-services-windows-phone-authorize-users-in-scripts/mobile-insert-script-users.png
[3]: ./media/mobile-services-windows-phone-authorize-users-in-scripts/mobile-quickstart-startup-wp8.png

<!-- URLs. -->
[Referência de script de servidor dos Serviços Móveis]: http://go.microsoft.com/fwlink/?LinkId=262293
[Painel Meus Aplicativos]: http://go.microsoft.com/fwlink/?LinkId=262039
[Introdução aos Serviços Móveis]: /pt-br/develop/mobile/tutorials/get-started/#create-new-service
[Introdução aos dados]: /pt-br/develop/mobile/tutorials/get-started-with-data-wp8
[Introdução à autenticação]: /pt-br/develop/mobile/tutorials/get-started-with-users-wp8
[Introdução às notificações por push]: /pt-br/develop/mobile/tutorials/get-started-with-push-wp8

[Portal de Gerenciamento do Azure]: https://manage.windowsazure.com/



<!--HONumber=42-->