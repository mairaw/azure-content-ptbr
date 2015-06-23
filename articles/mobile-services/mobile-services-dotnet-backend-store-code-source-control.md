<properties 
	pageTitle="Armazenar código de projeto no controle do código-fonte - Serviços Móveis do Azure" 
	description="Saiba como armazenar seu projeto de back-end do .NET no e publicar por meio de um repositório Git local no computador." 
	services="mobile-services" 
	documentationCenter="" 
	authors="ggailey777" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="mobile-services" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="na" 
	ms.devlang="multiple" 
	ms.topic="article" 
	ms.date="04/13/2015" 
	ms.author="glenga"/>

<div class="dev-center-tutorial-subselector">
	<a href="/documentation/articles/mobile-services-dotnet-backend-store-code-source-control/" title="Back-end do .NET" class="current">Back-end do .NET</a> | <a href="/documentation/articles/mobile-services-store-scripts-source-control/"  title="Back-end do JavaScript">Back-end do JavaScript</a>
</div>

# Armazenar o código do projeto no controle de origem

Este tópico mostra como usar o controle de origem fornecido pelos Serviços Móveis do Azure para armazenar o seu projeto de serviço de back-end do .NET. Seu projeto pode ser publicado simplesmente carregando do repositório Git local para o seu serviço móvel de produção.

O tutorial o orientará por estas etapas:

1. [Habilitar o controle do código-fonte no seu serviço móvel].
2. [Instalar o Git e criar o repositório local].
3. [Publicar seu projeto usando o Git].

Para concluir este tutorial, você deve já ter criado um serviço móvel ao concluir a [Introdução aos Serviços Móveis] ou ao concluir o tutorial [Adicionar Serviços Móveis a um aplicativo existente].

##<a name="enable-source-control"></a>Habilitar o controle do código-fonte no seu serviço móvel.

[AZURE.INCLUDE [mobile-services-enable-source-control](../../includes/mobile-services-enable-source-control.md)]

##<a name="clone-repo"></a>Instalar o Git e criar o repositório local

1. Instale o Git no seu computador local. 

	As etapas necessárias para instalar o Git variam de acordo com o sistema operacional. Consulte [Instalando o Git] para distribuições específicas de sistemas operacionais e orientações de instalação.

	> [AZURE.NOTE]Em alguns sistemas operacionais, uma versão da linha de comando e uma versão da GUI do Git estão disponíveis. As instruções fornecidas neste artigo usam a versão de linha de comando.

2. Abra uma linha de comando, como **GitBash** (Windows) ou **Bash** (shell Unix). Nos sistemas OS X, você pode acessar a linha de comando por meio do aplicativo **Terminal**.

3. Na linha de comando, altere para o diretório no qual você armazenará seus scripts. Por exemplo, `cd SourceControl`.

4. Use o comando a seguir para criar uma cópia local do seu novo repositório Git, substituindo `<your_git_URL>` pela URL do repositório Git para seu serviço móvel:

		git clone <your_git_URL>

5. Quando solicitado, digite o nome de usuário e a senha definidos no momento em que habilitou o controle do código-fonte no seu serviço móvel. Após a autenticação bem-sucedida, você verá uma série de respostas, como esta:

		remote: Counting objects: 8, done.
		remote: Compressing objects: 100% (4/4), done.
		remote: Total 8 (delta 1), reused 0 (delta 0)
		Unpacking objects: 100% (8/8), done.

6. Navegue até o diretório que executou o comando `git clone` e observe que um novo diretório é criado com o nome do serviço móvel. Para um serviço móvel back-end do .NET, o repositório git está vazio no início.

Agora que você criou seu repositório local, você pode publicar seu projeto de serviço de back-end do .NET deste repositório.

##<a name="deploy-scripts"></a>Publicar seu projeto usando o Git

1. Crie um novo projeto de serviço móvel de back-end do .NET no Visual Studio 2013 ou mova um projeto existente para o novo repositório local.  

	Para um teste rápido, baixe e salve o projeto quickstart de Serviços Móveis para essa pasta.

2. Remova quaisquer pastas de pacote NuGet, deixando o arquivo packages.config.

	Os Serviços Móveis irão automaticamente restaurar seus pacotes do NuGet com base no arquivo packages.confign. Você também pode definir um arquivo. gitignore para evitar que os diretórios de pacote sejam adicionados.
 
3. No prompt de comando do Git, digite o comando a seguir para iniciar o acompanhamento do novo arquivo de script:

		$ git add .
	
4. Digite o comando a seguir para confirmar as alterações:

		$ git commit -m "adding the .NET backend service project"

5. Digite o seguinte comando para carregar as alterações para o repositório remoto e fornecer suas credenciais:

		$ git push origin master
	
	Você deve ver uma série de comandos que indica que o projeto foi implantado para os Serviços Móveis, que os pacotes foram adicionados e o serviço foi reiniciado.

6. Navegue até a URL do seu serviço móvel de back-end do .NET e você verá o seguinte:

	![Página de inicialização dos Serviços Móveis](./media/mobile-services-dotnet-backend-store-code-source-control/mobile-service-startup.png)

Agora, seu projeto de serviço móvel é mantido no controle de origem e você pode publicar atualizações de serviço simplesmente ao enviar atualizações no seu repositório local. Para saber mais sobre como fazer alterações no modelo de dados em um serviço móvel de back-end do .NET que usa um banco de dados SQL, consulte [Como fazer alterações no modelo de dados para um serviço móvel de back-end do .NET].

## <a name="next-steps"> </a>Próximas etapas

Agora que concluiu este tutorial, você sabe como armazenar seus scripts no controle do código-fonte. Considere aprender mais sobre os serviços móveis:

+ [Como fazer alterações de modelos de dados a um serviço móvel de back-end .NET] <br/> Mostra como usar Migrações Code First do Entity Framework para fazer alterações de modelo de dados a um Banco de Dados SQL existente para evitar perder os dados existentes. 	

+ [Chamar uma API personalizada a partir do cliente] <br/> Mostra como criar APIs personalizadas que podem ser chamadas  partir do cliente.

<!-- Anchors. -->
[Habilitar o controle do código-fonte no seu serviço móvel]: #enable-source-control
[Instalar o Git e criar o repositório local]: #clone-repo
[Publicar seu projeto usando o Git]: #deploy-scripts

<!-- Images. -->

<!-- URLs. -->
[Git website]: http://git-scm.com
[Source control]: http://msdn.microsoft.com/library/windowsazure/c25aaede-c1f0-4004-8b78-113708761643
[Instalando o Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Introdução aos Serviços Móveis]: mobile-services-dotnet-backend-ios-get-started.md
[Adicionar Serviços Móveis a um aplicativo existente]: mobile-services-dotnet-backend-ios-get-started-data.md
[Azure Management Portal]: https://manage.windowsazure.com/
[Chamar uma API personalizada a partir do cliente]: mobile-services-dotnet-backend-ios-call-custom-api.md
[Como fazer alterações de modelos de dados a um serviço móvel de back-end .NET]: mobile-services-dotnet-backend-how-to-use-code-first-migrations.md
[Como fazer alterações no modelo de dados para um serviço móvel de back-end do .NET]: mobile-services-dotnet-backend-how-to-use-code-first-migrations.md

<!--HONumber=54--> 