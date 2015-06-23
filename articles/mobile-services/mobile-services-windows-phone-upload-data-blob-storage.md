<properties 
	pageTitle="Use Serviços Móveis para carregar imagens no armazenamento de blob (Windows Phone) | Serviços Móveis" 
	description="Aprenda a usar os Serviços Móveis para carregar imagens no Armazenamento de blobs do Azure." 
	documentationCenter="windows" 
	authors="wesmc7777" 
	writer="wesmc" 
	services="mobile-services" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="mobile-services" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="" 
	ms.devlang="dotnet" 
	ms.topic="article" 
	ms.date="02/23/2015" 
	ms.author="wesmc"/>

# Fazer upload de imagens no Armazenamento do Azure usando Serviços Móveis

[AZURE.INCLUDE [mobile-services-selector-upload-data-blob-storage](../../includes/mobile-services-selector-upload-data-blob-storage.md)]

##Visão geral

Este tópico mostra como usar os Serviços Móveis do Azure para habilitar seu aplicativo para carregar e armazenar as imagens geradas pelo usuário no Armazenamento do Azure. Os Serviços Móveis usam um banco de dados SQL para armazenar dados. No entanto, os dados de BLOB (objeto binário grande) são armazenados com mais eficiência no serviço de armazenamento de Blob do Azure.

Você não pode distribuir as credenciais necessárias de maneira segura usando o aplicativo cliente para carregar dados com segurança para o serviço de Armazenamento de Blob. Nesse caso, você deve armazenar essas credenciais em seu serviço móvel e usá-las para gerar uma SAS (assinatura de acesso compartilhado) usada para carregar uma nova imagem. A SAS, uma credencial com expiração curta – nesse caso, 5 minutos – será retornada com segurança pelos Serviços Móveis para o aplicativo cliente. O aplicativo usará, então, essa credencial temporária para carregar a imagem. Neste exemplo, os downloads do serviço Blob são públicos.

Neste tutorial, você adicionará funcionalidade ao [projeto de aplicativo de exemplo GetStartedWithData](mobile-services-windows-phone-get-started-data.md) para tirar fotos e carregar as imagens no Azure usando uma SAS gerada pelos Serviços Móveis.


##Pré-requisitos

Este tutorial exige o seguinte:

+ Microsoft Visual Studio 2012 Express para Windows 8 ou uma versão posterior
+ [SDK do Windows Phone 8.0] ou superior
+ Gerenciador de Pacotes Nuget instalado para o Microsoft Visual Studio.
+ [Conta de Armazenamento do Azure][How To Create a Storage Account]
+ Conclua o tutorial [Adicionar os Serviços Móveis a um aplicativo existente](mobile-services-windows-phone-get-started-data.md)   


##Instalar o cliente Storage para aplicativos do Windows Phone

Para poder usar uma SAS para carregar imagens para o armazenamento de Blob, você deve primeiro adicionar o pacote NuGet que instala a biblioteca do cliente de armazenamento para aplicativos do Windows Phone.

1. No **Gerenciador de Soluções** do Visual Studio, clique com o botão direito do mouse no nome do projeto e selecione **Gerenciar Pacotes NuGet**.

2. No painel esquerdo, selecione a categoria **Online**, selecione **Incluir Pré-liberação**, pesquise **WindowsAzure.Storage-Preview**, clique em **Instalar** no pacote **Armazenamento do Azure**, em seguida, aceite os acordos de licença.

  	![][2]

  	Isso adicionará a biblioteca de cliente para os serviços de armazenamento do Azure ao projeto.

Em seguida, você atualizará o aplicativo quickstart para capturar e carregar imagens.

##Atualizar o script de inserção registrados no Portal de Gerenciamento


[AZURE.INCLUDE [mobile-services-configure-blob-storage](../../includes/mobile-services-configure-blob-storage.md)]

>[AZURE.NOTE]Para adicionar novas propriedades ao objeto TodoItem, você deve ter o Esquema Dinâmico habilitado no seu serviço móvel. Quando o Esquema Dinâmico estiver habilitado, novas colunas serão automaticamente adicionadas à tabela TodoItem que mapeará para essas novas propriedades.

[AZURE.INCLUDE [mobile-services-windows-phone-upload-to-blob-storage](../../includes/mobile-services-windows-phone-upload-to-blob-storage.md)]


##Próximas etapas

Agora que você conseguiu carregar as imagens com segurança, integrando seu serviço móvel com o serviço de Blob, confira alguns dos outros tópicos de serviço de back-end e integração:

+ [Enviar email dos Serviços Móveis com SendGrid]
 
  Saiba como adicionar funcionalidade de email ao Serviço Móvel usando o serviço de email SendGrid. Este tópico demonstra como adicionar scripts do lado do servidor para enviar email usando o SendGrid.

+ [Agendar trabalhos de back-end nos Serviços Móveis]

  Saiba como usar a funcionalidade de agendador de trabalhos dos Serviços Móveis para definir o código de script de servidor executado em um agendamento definido por você.

+ [Referência de script de servidor dos Serviços Móveis]

  Tópicos de referência para o uso de scripts de servidor para executar tarefas no lado do servidor e integração com outros componentes do Azure e recursos externos.
 
+ [Referência conceitual do tutorial do .NET dos Serviços Móveis]

  Saiba mais sobre como usar os Serviços Móveis com o .NET
  
 

<!-- Images. -->


[2]: ./media/mobile-services-windows-phone-upload-data-blob-storage/mobile-add-storage-nuget-package-dotnet.png


[5]: ./media/mobile-services-windows-phone-upload-data-blob-storage/mobile-upload-blob-app-WMAppmanifest-wp8.png
[6]: ./media/mobile-services-windows-phone-upload-data-blob-storage/mobile-upload-blob-app-view-wp8.png
[7]: ./media/mobile-services-windows-phone-upload-data-blob-storage/mobile-upload-blob-app-view-camera-wp8.png
[8]: ./media/mobile-services-windows-phone-upload-data-blob-storage/mobile-upload-blob-app-view-save-wp8.png
[9]: ./media/mobile-services-windows-phone-upload-data-blob-storage/mobile-upload-blob-app-view-final-wp8.png

[11]: ./media/mobile-services-windows-phone-upload-data-blob-storage/mobile-upload-blob-app-view-camera-accept-wp8.png

<!-- URLs. -->
[Enviar email dos Serviços Móveis com SendGrid]: /develop/mobile/tutorials/send-email-with-sendgrid/
[Agendar trabalhos de back-end nos Serviços Móveis]: /develop/mobile/tutorials/schedule-backend-tasks/
[Referência de script de servidor dos Serviços Móveis]: http://go.microsoft.com/fwlink/p/?LinkId=262293
[Get started with Mobile Services]: ../mobile-services-windows-phone-get-started.md

[Azure Management Portal]: https://manage.windowsazure.com/
[How To Create a Storage Account]: /manage/services/storage/how-to-create-a-storage-account
[Azure Storage Client library for Store apps]: http://go.microsoft.com/fwlink/p/?LinkId=276866
[Referência conceitual do tutorial do .NET dos Serviços Móveis]: /develop/mobile/how-to-guides/work-with-net-client-library
[SDK do Windows Phone 8.0]: http://www.microsoft.com/download/details.aspx?id=35471



<!--HONumber=54--> 