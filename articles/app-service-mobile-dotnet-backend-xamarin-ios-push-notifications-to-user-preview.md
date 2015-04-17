﻿<properties 
	pageTitle="Enviar notificações por push para um usuário específico com o cliente Xamarin iOS" 
	description="Saiba como enviar notificações por push para todos os dispositivos de um usuário" 
	services="app-service\mobile" 
	documentationCenter="windows" 
	authors="yuaxu" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="app-service-mobile" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="mobile-xamarin-ios" 
	ms.devlang="dotnet" 
	ms.topic="article" 
	ms.date="03/17/2015"
	ms.author="yuaxu"/>

# Enviar notificações de plataforma cruzada para um usuário específico

[AZURE.INCLUDE [app-service-mobile-selector-push-users-preview](../includes/app-service-mobile-selector-push-users-preview.md)]

Este tópico mostra como enviar notificações para todos os dispositivos registrados de um usuário específico do seu back-end móvel.  Ele introduziu o conceito de [modelos], que oferece a aplicativos cliente a liberdade de especificar formatos de carga e espaços reservados de variáveis no registro.  O envio, em seguida, atinge todas as plataformas com esses espaços reservados, habilitando notificações de plataforma cruzada.

> [AZURE.NOTE] Para fazer o push funcionar com clientes multiplataforma, você precisará concluir este tutorial para cada plataforma que desejar habilitar.  Você só precisa fazer a [atualização do back-end móvel](#backend) uma vez para clientes que compartilhem o mesmo back-end móvel.
 
##Pré-requisitos 

Antes de iniciar este tutorial, você já deve ter concluído esses tutoriais de Serviço de Aplicativo para cada plataforma cliente com que desejar trabalhar:

+ [Introdução à autenticação]<br/>Adiciona um requisito de logon ao aplicativo de exemplo TodoList.

+ [Introdução às notificações por push]<br/>Configura o aplicativo de exemplo TodoList tarefas para notificações por push.

##<a name="client"></a>Atualize o cliente para se registrar para modelos para lidar com pushes de plataforma cruzada

1. Mova os fragmentos de código de registro APNs de **FinishedLaunching** em **AppDelegate.cs** para a definição de tarefa **RefreshAsync** em **QSTodoListViewController.cs**.  Os registros devem acontecer após a conclusão da autenticação.

        ...
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }

            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes (
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound, 
                new NSSet ());

            UIApplication.SharedApplication.RegisterUserNotificationSettings (settings); 
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        ...

2. Em **AppDelegate.cs**, substitua a chamada **RegisterAsync** em **RegisteredForRemoteNotifications** pelo seguinte para trabalhar com modelos:

        // delete await push.RegisterAsync (deviceToken);
        
        var notificationTemplate = "{\"aps\":  {\"alert\":\"$(message)\"}}";

        JObject templateBody = new JObject();
        templateBody["body"] = notificationTemplate;

        JObject templates = new JObject();
        templates["testApnsTemplate"] = templateBody;

        // register with templates
        await push.RegisterAsync (deviceToken, templates);

##<a name="backend"></a>Atualize o back-end de serviço para enviar notificações para um usuário específico

1. No Visual Studio, atualize a `PostTodoItem` definição de método com o seguinte código:  

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);

            // get notification hubs credentials associated with this mobile app
            string notificationHubName = this.Services.Settings.NotificationHubName;
            string notificationHubConnection = this.Services.Settings.Connections[ServiceSettingsKeys.NotificationHubConnectionString].ConnectionString;

            // connect to notification hub
            NotificationHubClient Hub = NotificationHubClient.CreateClientFromConnectionString(notificationHubConnection, notificationHubName)

            // get the current user id and create given user tag
            ServiceUser authenticatedUser = this.User as ServiceUser;
            string userTag = "_UserId:"  + authenticatedUser.Id;

            var notification = new Dictionary<string, string>{{"message", item.Text}};

            try
            {
                await Hub.Push.SendTemplateNotificationAsync(notification, userTag);
            }
            catch (System.Exception ex)
            {
                Throw;
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

##<a name="test"></a>Testar o aplicativo

Publique novamente o projeto de back-end móvel e execute qualquer aplicativo cliente que tenha configurado.  Na inserção do item, o back-end enviará as notificações para todos os aplicativos cliente em que o usuário esteja conectado.

<!-- URLs. -->
[Introdução à autenticação]: app-service-mobile-dotnet-backend-xamarin-ios-get-started-users-preview.md
[Introdução às notificações por push]: app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview.md
[modelos]: https://msdn.microsoft.com/pt-br/library/dn530748.aspx

<!--HONumber=49-->