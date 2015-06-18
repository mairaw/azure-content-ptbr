<properties 
	pageTitle="Adicionar referências de Escala Elástica do banco de dados SQL do Azure a um projeto do Visual Studio" 
	description="Como adicionar referências do .NET a APIs de escala elástica para projetos do Visual Studio usando o Nuget." 
	services="sql-database" 
	documentationCenter="" 
	manager="stuartozer" 
	authors="stuartozer" 
	editor=""/>

<tags 
	ms.service="sql-database" 
	ms.workload="sql-database" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/16/2015" 
	ms.author="stuartozer@microsoft.com"/>

#Como: Adicionar referências de Escala Elástica do banco de dados SQL do Azure a um projeto do Visual Studio 

###Pré-requisitos: 

- Instalar a [Galeria de extensões do NuGet Visual Studio](http://docs.nuget.org/docs/start-here/installing-nuget) para Visual Studio 

###Para adicionar uma referência de Escala Elástica no Visual Studio 

1. Abrir um projeto existente ou criar um novo projeto usando o novo projeto caixa de diálogo localizada em **Arquivo** --> **Novo** --> **Projeto** 
2. No Gerenciador de Soluções, clique com o botão direito em **Referências** e selecione **Gerenciar Pacotes NuGet**
3. No menu no lado esquerdo da janela Gerenciar Pacotes NuGet, selecione **Online** e depois **nuget.org** ou "Tudo" 
4. Na caixa de diálogo de **Pesquisa Online**, digite **Escala Elástica**, selecione as **bibliotecas do cliente de Escala Elástica** e clique em **Instalar**.
4. Examine a licença e clique em **Eu Aceito**. 

As referências de Escala Elástica do banco de dados SQL do Azure agora foram adicionadas ao seu projeto. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--HONumber=47-->
 