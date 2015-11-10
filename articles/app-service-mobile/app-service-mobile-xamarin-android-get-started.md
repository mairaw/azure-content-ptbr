<properties
	pageTitle="Introdução aos aplicativos móveis do Azure para aplicativos Xamarin Android"
	description="Siga este tutorial para começar a usar os Aplicativos Móveis do Azure para desenvolvimento Android Xamarin"
	services="app-service\mobile"
	documentationCenter="xamarin"
	authors="wesmc7777"
	manager="dwrede"
	editor="" />

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-xamarin-android"
	ms.devlang="dotnet"
	ms.topic="hero-article"
	ms.date="10/20/2015"
	ms.author="normesta" />

#Criar um Aplicativo Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]
 
##Visão geral

Este tutorial mostra como adicionar um serviço de back-end baseado em nuvem a um back-end do Aplicativo Móvel do Azure. Você criará um novo back-end do Aplicativo Móvel e um aplicativo Xamarin.Android simples _Tarefas pendentes_ que armazena dados do aplicativo no Azure.

Uma captura de tela do aplicativo completo está disponível abaixo:

![][0]

A conclusão deste tutorial é um pré-requisito para todos os outros tutoriais de Aplicativos Móveis para aplicativos Xamarin.Android.
 
##Pré-requisitos

Para concluir este tutorial, você precisará do seguinte:

* Uma conta ativa do Azure. Se você não tem uma conta, você pode se inscrever para uma avaliação do Azure e obter até 10 aplicativos móveis gratuitos que você pode continuar a usar mesmo após o fim do seu período de avaliação. Para obter detalhes, consulte [Avaliação gratuita do Azure](http://azure.microsoft.com/pricing/free-trial/).
 
* [Visual Studio Community 2013] ou posterior. Se você instalar o Visual Studio Community 2013, instale o [Xamarin] separadamente. Você pode instalar as ferramentas Xamarin quando você instala o Visual Studio de 2015.
 
>[AZURE.NOTE]Se você deseja começar com o serviço de aplicativo do Azure antes de inscrever-se em uma conta do Azure, vá até [Experimentar o serviço de aplicativo](http://go.microsoft.com/fwlink/?LinkId=523751&appServiceName=mobile), onde você pode criar imediatamente um aplicativo móvel inicial de curta duração no serviço de aplicativo. Nenhum cartão de crédito é exigido, sem compromissos.


## Criar um novo back-end de aplicativo móvel do Azure

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## Baixar o projeto de servidor

1. Em seu PC, visite o [Portal do Azure]. Clique em **Procurar Tudo** > **Aplicativos Móveis** e, em seguida, clique no back-end de aplicativo móvel que você acabou de criar.

2. Na folha do aplicativo móvel, clique em **Configurações** e, em **Aplicativo Móvel**, clique **Início rápido** > **Xamarin.Android**.
 
3. Em **Baixar e executar seu projeto de servidor**, clique em **Baixar**. Extraia os arquivos compactados do projeto em seu computador e abra a solução no Visual Studio.
 
## Teste seu projeto de back-end localmente

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-test-local-service](../../includes/app-service-mobile-dotnet-backend-test-local-service.md)]

## Publicar o projeto de servidor no Azure

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service](../../includes/app-service-mobile-dotnet-backend-publish-service.md)]

## Baixar e executar o aplicativo Xamarin.Android

1. Em **Baixe e execute seu projeto Xamarin.Android**, clique no botão **Baixar**.

  	Isso baixa um projeto que contém uma aplicação de cliente que está conectada ao seu aplicativo móvel. Salve o arquivo do projeto compactado em seu computador local e anote onde ele foi salvo.

	![][8]

	![][9]

2. Pressione a tecla **F5** para recompilar o projeto e iniciar o aplicativo.

3. No aplicativo, digite um texto significativo, como _Concluir o tutorial_ e clique no botão **Adicionar**.

	![][10]

	Isso envia uma solicitação de POST para o novo back-end de aplicativo móvel hospedado no Azure. Os dados da solicitação são inseridos na tabela TodoItem. Itens armazenados na tabela são retornados pelo back-end do aplicativo móvel e os dados aparecem na lista.

	> [AZURE.NOTE]Você pode examinar o código que acessa o back-end do aplicativo móvel para consultar e inserir dados que estão localizados no arquivo ToDoActivity.cs C#.

##Próximas etapas

* [Adicionar autenticação ao seu aplicativo](app-service-mobile-xamarin-android-get-started-users.md) <br/>Saiba como autenticar usuários de seu aplicativo com um provedor de identidade.


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Portal do Azure]: https://azure.portal.com/
[Xamarin]: http://xamarin.com/download
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532&clcid=0x409
[Xamarin for Windows]: https://go.microsoft.com/fwLink/?LinkID=330242&clcid=0x409
 
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203

<!---HONumber=Nov15_HO1-->