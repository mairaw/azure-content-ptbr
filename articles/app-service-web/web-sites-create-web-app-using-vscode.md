<properties
   pageTitle="Criar um aplicativo Web ASP.NET 5 no Visual Studio Code"
   description="Este tutorial mostra como criar um aplicativo Web ASP.NET 5 usando o código do Visual Studio."
   services="app-service\web"
   documentationCenter=".net"
   authors="erikre"
   manager="wpickett"
   editor="jimbe"/>

<tags
	ms.service="app-service-web" 
	ms.workload="web" 
	ms.tgt_pltfrm="dotnet" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/14/2015" 
	ms.author="erikre;tarcher"/>

# Criar um aplicativo Web ASP.NET 5 no Visual Studio Code

## Visão geral

Este tutorial mostra como criar um aplicativo Web ASP.NET 5 usando [Código do Visual Studio (Código do VS)](http://code.visualstudio.com//Docs/whyvscode) e implantá-lo no [Serviço de Aplicativo do Azure](../app-service/app-service-value-prop-what-is.md). O ASP.NET 5 é uma reestruturação significativa do ASP.NET. ASP.NET 5 é uma nova estrutura de código-fonte aberto entre plataformas para criar modernos aplicativos em nuvem da Web usando o .NET. Para obter mais informações, consulte [Introdução ao ASP.NET 5](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html). Para obter informações sobre aplicativos Web do Serviço de Aplicativo do Azure, consulte [Visão geral de aplicativos Web](app-service-web-overview.md).

[AZURE.INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## Pré-requisitos  

* Instale o [Código do VS](http://code.visualstudio.com/Docs/setup).
* Instale o [Node.js](http://nodejs.org/download/) - o [Node.js](http://nodejs.org/) é uma plataforma para a criação rápida e dimensionável de aplicativos de servidor usando JavaScript. O nó é o tempo de execução (Nó) e [npm](http://www.npmjs.com/) é o Gerenciador de Pacotes para os módulos do Nó. Você usará o npm para criar o scaffolding de um aplicativo Web ASP.NET 5 neste tutorial.
* Instale o Git - você pode instalá-lo de um destes locais: [Chocolatey](https://chocolatey.org/packages/git) ou [git scm.com](http://git-scm.com/downloads). Se você for iniciante no Git, escolha [git-scm.com](http://git-scm.com/downloads) e selecione a opção para usar Git com GitBash no prompt de comando do Windows. Depois de instalar o Git, também será necessário definir o nome de usuário e email do Git, pois eles são necessários posteriormente no tutorial (ao realizar uma confirmação do Código do VS).  

## Instalar o ASP.NET 5 e DNX
O ASP.NET 5/DNX é uma pilha enxuta do .NET para criar aplicativos da Web e de nuvem modernos e executados em OS X, Linux e Windows. Ele foi criado do zero para fornecer uma estrutura de desenvolvimento otimizada para aplicativos que são implantados na nuvem ou então executados localmente. Ele consiste em componentes modulares com sobrecarga mínima, para que você mantenha a flexibilidade durante a construção de suas soluções.

> [AZURE.NOTE]O ASP.NET 5 e o DNX (o Ambiente de Execução do .NET) em OS X e Linux estão em um estado inicial Beta/de Visualização.

Este tutorial é projetado para começar a criar aplicativos com as versões de desenvolvimento ASP.NET 5 e DNX mais recentes. As instruções a seguir são específicas do Windows. Para obter instruções de instalação mais detalhadas para OS X, Linux e Windows, consulte [Instalação do ASP.NET 5 e DNX](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).

1. Para instalar o Gerenciador de Versão do .NET (DNVM) no Windows, abra um prompt de comando e execute o comando a seguir.

		@powershell -NoProfile -ExecutionPolicy unrestricted -Command "&{$Branch='dev';iex ((new-object net.webclient).DownloadString('https://raw.githubusercontent.com/aspnet/Home/dev/dnvminstall.ps1'))}"

	Isso baixará o script DNVM e o colocará em seu diretório de perfil do usuário.

2. Reinicie o Windows para concluir a instalação do DNVM.

3. Abra um prompt de comando e verifique o local do DNVM digitando o seguinte.

		where dnvm

	O prompt de comando mostrará um caminho semelhante ao seguinte.

	![local dnvm](./media/web-sites-create-web-app-using-vscode/00-where-dnvm.png)

4. Agora que você tem o DNVM, você precisa usá-lo para baixar o DNX, para então executar os aplicativos. Execute o seguinte no prompt de comando.

		dnvm upgrade

5. Verifique o DNVM e exiba o tempo de execução ativo digitando o seguinte no prompt de comando:

		dnvm list

	O prompt de comando mostrará os detalhes do tempo de execução ativo.

	![Local DNVM](./media/web-sites-create-web-app-using-vscode/00b-dnvm-list.png)

6. Se mais de um tempo de execução DNX estiver listado, digite o seguinte no prompt de comando para definir o tempo de execução DNX ativo para a mesma versão que é usada pelo gerador de ASP.NET 5 quando você cria seu aplicativo Web mais adiante neste tutorial.

		dnvm use 1.0.0-beta4 –p

> [AZURE.NOTE]Para obter instruções de instalação mais detalhadas para OS X, Linux e Windows, consulte [Instalação do ASP.NET 5 e DNX](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).

## Criar o aplicativo Web 

Esta seção mostra como criar o scaffolding de um novo aplicativo Web do ASP.NET do aplicativo. Você usará o gerenciador de pacotes do nó (npm) para instalar o [Yeoman](http://yeoman.io/) (ferramenta de scaffolding do aplicativo - o equivalente em código VS à operação do Visual Studio **Arquivo > Novo Projeto**), [Grunt](http://gruntjs.com/) (executor de tarefa JavaScript) e [Bower](http://bower.io/) (gerenciador de pacotes no cliente).

1. Abra um prompt de comando com direitos de administrador e navegue até o local onde você deseja criar seu projeto ASP.NET.

2. Digite o seguinte no prompt de comando para instalar as ferramentas de suporte e o Yeoman.

		npm install -g yo grunt-cli generator-aspnet bower

3. Digite o seguinte no prompt de comando para criar a pasta do projeto e o scaffolding do aplicativo.

		yo aspnet

4. Use as teclas de direção para selecionar o tipo **Aplicativo Web** no menu do gerador de ASP.NET 5 e pressione &lt;Enter>.

	![Yoman - gerador do ASP.NET 5](./media/web-sites-create-web-app-using-vscode/01-yo-aspnet.png)

5. Defina o nome de seu novo aplicativo Web ASP.NET como **SampleWebApp**. Como esse nome é usado em todo o tutorial, se você selecionar um nome diferente, precisará substituir cada ocorrência de **SampleWebApp**. Quando você pressionar &lt;Enter>, Yeoman criará uma nova pasta chamada **SampleWebApp** e os arquivos necessários para seu novo aplicativo.

6. Abra o código do VS digitando o seguinte no prompt de comando.

		code .

7. No código do VS, selecione **Arquivo > Abrir Pasta** e selecione a pasta que contém seu aplicativo Web ASP.NET.

	![caixa de diálogo Selecionar Pasta](./media/web-sites-create-web-app-using-vscode/02-open-folder.png)

	O VSCode carregará o projeto e exibirá seus arquivos nas janelas **Explorar**.

	![VSCode exibindo projeto SampleWebApp](./media/web-sites-create-web-app-using-vscode/03-vscode-project.png)

8. Selecione **Exibir > Paleta de Comandos**.

9. Em **Paleta de Comandos**, digite os comandos a seguir.

		dnx:dnu restore - (SampleWebApp)

	Quando você começar a digitar, verá a linha de comando completa na lista.

	![Comando Restore](./media/web-sites-create-web-app-using-vscode/04-dnu-restore.png)

	O comando Restore instala os pacotes do NuGet necessários para executar o aplicativo. Um prompt de comando exibirá **Restauração concluída** quando a restauração estiver pronta.

## Executar o aplicativo Web localmente

Agora que criou o aplicativo Web e recuperou todos os pacotes do NuGet para o aplicativo, você pode executar o aplicativo Web localmente.

1. Na **Paleta de Comandos** no Código do VS, digite o seguinte para executar o comando localmente:

		dnx: kestrel - (SampleWebApp, Microsoft.AspNet.Hosting --server Kestrel --server.urls http://localhost:5001

	A janela de comando exibirá a mensagem *Iniciado*. Se a janela de comando não exibir a mensagem *Iniciado*, verifique o canto inferior esquerdo do Código do VS em busca de erros em seu projeto.

5. Abra uma janela de navegador e navegue até a URL a seguir.

	**http://localhost:5001**

	A página padrão do aplicativo Web será exibida da maneira a seguir.

	![Aplicativo Web local em um navegador](./media/web-sites-create-web-app-using-vscode/08-web-app.png)

## Criar um aplicativo Web no portal de visualização do Azure

As etapas a seguir o guiarão ao longo da criação de um aplicativo Web no portal de visualização do Azure.

1. Faça logon no [Portal de Visualização do Azure](https://portal.azure.com).

2. Clique em **NOVO** na parte superior esquerda do portal.

3. Clique em **Aplicativos Web > Aplicativo Web**.

	![Novo aplicativo Web do Azure](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)

4. Insira um valor para **Nome**, como **SampleWebAppDemo**. Observe que esse nome precisa ser exclusivo, e o portal imporá isso quando você tentar inserir o nome. Portanto, se você selecionar um valor diferente, precisará substituir esse valor para cada ocorrência do **SampleWebAppDemo** que você vê neste tutorial.

5. Selecione um **Plano de Serviço de Aplicativo** existente ou crie um novo. Se você criar um novo plano, selecione a camada de preços, localização e outras opções. Para obter mais informações sobre planos de serviço de aplicativo, consulte o artigo [Visão geral detalhada de planos de serviço de aplicativo do Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

	![Folha de novo aplicativo Web do Azure](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)

6. Clique em **Criar**.

	![folha de aplicativo Web](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## Habilitar a publicação de Git para o novo aplicativo Web

O Git é um sistema de controle de versão distribuído que você pode usar para implantar seu aplicativo Web do Serviço de Aplicativo do Azure. Você armazenará o código que você escreve para seu aplicativo Web em um repositório Git local e você implantará seu código no Azure enviando-o por push para um repositório remoto.

1. Entre no [portal de visualização do Azure](https://portal.azure.com).

2. Clique em **Procurar Tudo**.

3. Clique em **aplicativos Web** para exibir uma lista dos aplicativos Web associados à sua assinatura do Azure.

4. Selecione o aplicativo Web criado neste tutorial.

5. Na folha do aplicativo Web, role para baixo para localizar a seção **Implantação** e clique em **Configurar implantação contínua**.

	![host de aplicativo Web do Azure](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)

6. Clique em **Escolher fonte > Repositório Git local**.

7. Clique em **OK**.

	![Repositório do Git Local do Azure](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)

8. Se você não configurou previamente as credenciais de implantação para publicação de um Aplicativo Web ou outro aplicativo de Serviço de Aplicativo, configure-as agora:

	* Clique em **FTP** para definir credenciais de implantação.

	* Digite um nome de usuário e senha. Você precisará dessa senha posteriormente ao configurar o Git.

	* Clique em **Salvar**.

	![Credenciais de implantação do Azure](./media/web-sites-create-web-app-using-vscode/16-azure-credentials.png)

9. Na folha de seu aplicativo Web, clique em **Configurações > Propriedades**. A URL do repositório Git remoto na qual você vai implantar é mostrada em **
10.  URL**.

10. Copie o valor de **URL do GIT** para uso posterior no tutorial.

	![URL do Git do Azure](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## Publicar seu aplicativo Web no serviço de aplicativo do Azure

Nesta seção, você criará um repositório do Git local e enviará esse repositório por push ao Azure para implantar seu aplicativo Web no Azure.

1. No código do VS, selecione a opção **Git** na barra de navegação à esquerda.

	![Ícone do Git no Código do VS](./media/web-sites-create-web-app-using-vscode/git-icon.png)

2. Selecione **Inicializar repositório git** para assegurar que o espaço de trabalho está sob controle do código-fonte git.

	![Inicializar Git](./media/web-sites-create-web-app-using-vscode/19-initgit.png)

3. Adicione uma mensagem de confirmação e clique no ícone de verificação **Confirmar Tudo**.

	![Confirmar Tudo do Git](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)

4. Após a conclusão do processamento do Git, você verá que não há nenhum arquivo listado na janela do Git em **Alterações**.

	![Sem alterações do Git](./media/web-sites-create-web-app-using-vscode/no-changes.png)

5. Abra um prompt de comando e altere os diretórios para o diretório que hospeda seu aplicativo Web.

6. Crie uma referência remota para enviar atualizações para seu aplicativo Web usando a URL do Git (terminando em ".git") que você copiou anteriormente.

		git remote add azure [URL for remote repository]

7. Envie as alterações por push ao Azure usando o comando a seguir.

		git push azure master

	Será solicitada a senha que você criou anteriormente. **Observação: a senha não será visível.**

	A saída deste comando termina com uma mensagem de que a implantação foi bem-sucedida.

		remote: Deployment successful.
		To https://user@testsite.scm.azurewebsites.net/testsite.git
		[new branch]      master -> master

> [AZURE.NOTE]Se fizer alterações no aplicativo, você poderá republicá-lo selecionando a opção **Confirmar Tudo** no código do VS e, em seguida, inserindo o comando **git push azure master** no prompt de comando.

## Executar o aplicativo no Azure
Agora que você implantou seu aplicativo Web, vamos executar o aplicativo enquanto ele está hospedado no Azure.

Isso pode ser feito de duas maneiras:

* Abra um navegador e digite o nome do aplicativo Web da maneira a seguir.   

		http://SampleWebAppDemo.azurewebsites.net
 
* No portal de visualização do Azure, localize a folha de seu aplicativo Web e clique em **Procurar** para exibir seu aplicativo no navegador padrão.

![Aplicativo Web do Azure](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## Resumo
Neste tutorial, você aprendeu a criar um aplicativo Web no código do VS e implantá-lo no Azure. Para obter mais informações sobre o código do VS, consulte o artigo [Por que o Código do Visual Studio?](https://code.visualstudio.com/Docs/). Para obter informações sobre os aplicativos Web do Serviço de Aplicativo, consulte [Visão geral de aplicativos Web](app-service-web-overview.md).

<!---HONumber=62-->