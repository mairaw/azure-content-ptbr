<properties
	pageTitle="Introdução aos Serviços Móveis do Azure para aplicativos iOS"
	description="Siga este tutorial para começar a usar os serviços móveis do Azure para desenvolvimento do iOS."
	services="mobile-services"
	documentationCenter="ios"
	authors="krisragh"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.workload="mobile"
	ms.tgt_pltfrm=""
	ms.devlang="objective-c"
	ms.topic="hero-article"
	ms.date="2/11/2015"
	ms.author="krisragh"/>

# <a name="getting-started"> </a>Introdução aos Serviços Móveis

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

Este tutorial mostra como adicionar um serviço de back-end baseado em nuvem a um aplicativo iOS usando os Serviços Móveis do Azure. Neste tutorial, você criará um novo serviço móvel e um aplicativo simples _To do list_ que armazena dados do aplicativo no novo serviço móvel. O serviço móvel usa o .NET e o Visual Studio para lógica de negócios do lado do servidor. Para criar um serviço móvel com lógica de negócios do servidor no JavaScript, consulte a [versão de back-end do JavaScript] deste tópico.

> [AZURE.NOTE] Para concluir este tutorial, você precisa de uma conta do Azure. Se não tiver uma conta, você poderá se inscrever para uma avaliação do Azure e obter [serviços móveis gratuitos que você pode continuar usando mesmo depois do fim de sua avaliação.](http://azure.microsoft.com/pricing/details/mobile-services/).  Para obter detalhes, consulte [Avaliação Gratuita do Azure](http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=AE564AB28&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fpt-br%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-ios%2F%20target="_blank").

## <a name="create-new-service"> </a>Criar um novo serviço móvel

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## Baixe o serviço móvel e o aplicativo no computador local

Agora que você criou o serviço móvel, baixe projetos que podem ser executados localmente.

1. Clique no serviço móvel que você acabou de criar e, em seguida, na guia Início Rápido, clique em **iOS** em **Escolher plataforma** e expanda **Criar um novo aplicativo iOS**.

2. No seu PC do Windows, clique em **Baixar** em **Baixar e publicar seu serviço na nuvem**. Isso baixa o projeto do Visual Studio que implementa o serviço móvel. Salve o arquivo de projeto compactado em seu computador local e anote onde ele foi salvo.

3. Em seu Mac, clique em **Baixar** em **Baixar e executar seu aplicativo**. Será baixado o projeto para o aplicativo _To do list_ que está conectado ao serviço móvel, juntamente com os Serviços Móveis iOS SDK. Salve o arquivo de projeto compactado em seu computador local e anote onde ele foi salvo.

## Testar o serviço móvel

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## Publicar o serviço móvel

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]


## Executar seu novo aplicativo iOS

[AZURE.INCLUDE [mobile-services-ios-run-app](../../includes/mobile-services-ios-run-app.md)]


## <a name="next-steps"> </a>Próximas etapas

Isso mostra como executar seu novo aplicativo cliente no serviço móvel em execução no Azure. Antes de testar o aplicativo iOS com o serviço móvel em execução em um computador local, você deve configurar o servidor Web e o firewall para permitir acesso por meio de seu computador de desenvolvimento do iOS. Para obter mais informações, consulte [Configurar o servidor Web local para permitir conexões para um serviço móvel local](mobile-services-dotnet-backend-how-to-configure-iis-express.md).

Saiba como executar tarefas adicionais importantes nos Serviços Móveis:

* [Adicionar Serviços Móveis a um aplicativo existente]
  <br/>Saiba mais sobre como armazenar e consultar dados usando os Serviços Móveis.

* [Introdução à sincronização de dados offline]
  <br/>Saiba como usar a sincronização de dados offline para tornar seu aplicativo ágil e robusto.

* [Adicionar autenticação a um aplicativo existente]
  <br/>Aprenda a autenticar usuários do aplicativo com um provedor de identidade.

* [Adicionar notificações por push a um aplicativo existente]
  <br/>Saiba como enviar uma notificação por push bastante básica a seu aplicativo.

* [Solucionar problemas de back-end do .NET dos Serviços Móveis]
  <br/> Saiba como diagnosticar e corrigir problemas que podem surgir com um back-end do .NET dos serviços móveis.

<!-- Anchors. -->
[Introdução aos Serviços Móveis]:#getting-started
[Criar um novo serviço móvel]:#create-new-service
[Definir a instância do serviço móvel]:#define-mobile-service-instance
[Próximas etapas]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-completed-ios.png
[1]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-steps-vs.png

[6]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-portal-quickstart-ios.png
[7]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-steps-ios.png
[8]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-xcode-project.png

[10]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-data-browse.png


<!-- URLs. -->
[Adicionar Serviços Móveis a um aplicativo existente]: mobile-services-dotnet-backend-ios-get-started-data.md
[Introdução à sincronização de dados offline]: mobile-services-ios-get-started-offline-data.md
[Adicionar autenticação a um aplicativo existente]: mobile-services-dotnet-backend-ios-get-started-users.md
[Adicionar notificações por push a um aplicativo existente]: mobile-services-dotnet-backend-ios-get-started-push.md
[Solucionar problemas de back-end do .NET dos Serviços Móveis]: mobile-services-dotnet-backend-how-to-troubleshoot.md

[SDK do iOS dos Serviços Móveis]: https://go.microsoft.com/fwLink/p/?LinkID=266533

[Portal de Gerenciamento]: https://manage.windowsazure.com/
[XCode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Versão de back-end do JavaScript]: mobile-services-ios-get-started.md


<!--HONumber=52--> 