<properties
	pageTitle="Documentação de implantação do Serviço de Aplicativo do Azure"
	description="Encontre a documentação, que explica como implantar seu aplicativo no Serviço de Aplicativo do Azure."
	services="app-service"
	documentationCenter=""
	authors="tdykstra"
	manager="wpickett"
	editor="mollybos"/>

<tags
	ms.service="app-service"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="11/06/2015"
	ms.author="tdykstra"/>

# Documentação de implantação do Serviço de Aplicativo do Azure

## Visão geral

Este artigo lista os métodos disponíveis para implantar seu próprio conteúdo para o [Serviço de Aplicativo do Azure](http://go.microsoft.com/fwlink/?LinkId=529714), incluindo links para artigos e blogs que contêm instruções. Conforme artigos adicionais forem publicados, eles serão adicionados a essa lista.

A melhor maneira de implantar um aplicativo Web é configurando um [fluxo de trabalho de entrega contínua](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) integrado com seu [sistema de controle do código-fonte](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control). A automação não só torna o processo de desenvolvimento mais eficiente, mas também pode tornar os processos de backup e restauração mais gerenciáveis e confiáveis.

##### Implantar por meio de sistemas de controle do código-fonte hospedados em nuvem

* [Entrega contínua usando o Visual Studio Team Services](#vsts)
* [Sites de repositório usando o Git](#git)
* [Sites de repositório usando o Mercurial](#mercurial)
* [Automatizar a implantação por meio do Dropbox](#dropbox)

##### Implantar por meio de sistemas locais de controle do código-fonte

* [Configurar a entrega contínua com o TFS (Team Foundation Service)](#tfs)
* [Repositórios locais do Git ou do Mercurial](#onpremises)

##### Automatizar a implantação usando ferramentas de linha de comando

* [Automatizar a implantação com o MSBuild](#msbuild)
* [Copiar arquivos com scripts e ferramentas de FTP](#ftp)
* [Automatizar a implantação com o Windows PowerShell](#powershell)
* [Automatizar a implantação com a API de gerenciamento do .NET](#api)
* [Implantar por meio da CLI (Interface de Linha de Comando) do Azure](#cli)
* [Implantar por meio da linha de comando de Implantação da Web](#webdeploy)
 
##### Implantar por meio do IDE (Ambiente de Desenvolvimento Integrado)

* [Implantar diretamente por meio do Visual Studio](#vs)
* [Implantar diretamente por meio do WebMatrix](#webmatrix)

Outra opção de implantação é usar um serviço baseado em nuvem, como o [Octopus Deploy](http://en.wikipedia.org/wiki/Octopus_Deploy). Para obter mais informações, consulte [Implantar aplicativos ASP.NET em Sites do Azure](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

##<a name="vsts"></a>Entrega contínua usando o Visual Studio Team Services

O [Visual Studio Team Services](http://www.visualstudio.com/) (anteriormente conhecido como Team Foundation Service) é a solução baseada em nuvem da Microsoft para controle do código-fonte e colaboração em equipe. O serviço é grátis para uma equipe de até 5 desenvolvedores. Você pode realizar a entrega contínua para um aplicativo da Web nos Serviços de Aplicativo e seu repositório pode usar [Git ou TFVC](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#gittfs).

Para obter mais informações, consulte os recursos a seguir:

* [Entrega contínua no Azure usando Visual Studio Team Services e TFVC](../cloud-services-continuous-delivery-use-vso.md). Tutorial passo a passo que mostra como configurar o fornecimento contínuo por meio do Visual Studio Team Services em um aplicativo Web, usando o TFVC. O TFVC é a opção de controle do código-fonte centralizada, ao contrário do Git, que é a opção de controle do código-fonte distribuída.
* [Entrega contínua no Azure usando o Visual Studio Team Services e Git](../cloud-services-continuous-delivery-use-vso-git.md). Semelhante ao tutorial anterior, porém usando o Git em vez do TFVC.

##<a name="git"></a>Sites de repositório usando o Git

O [Git](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#gittfs) é um sistema de controle do código-fonte distribuído popular. O Azure tem recursos integrados que facilitam a automação da implantação de um aplicativo Web por meio de sites de repositório populares baseados na Web que armazenam repositórios Git, incluindo o [GitHub](http://www.github.com), [CodePlex](http://www.codeplex.com/) e [BitBucket](https://bitbucket.org/). Uma vantagem de usar do Git para a implantação é a reversão relativamente fácil para uma implantação anterior se isso for necessário em algum momento.

Para obter mais informações, consulte os recursos a seguir:

* [Publicando por meio do Controle do Código-fonte em Aplicativos Web com Git](web-sites-publish-source-control.md). Como usar o Git para publicar diretamente do computador local em aplicativos Web (no Azure, esse método de publicação é denominado Git local). Ele também mostra como habilitar a implantação contínua de repositórios Git a partir do GitHub, do CodePlex ou do BitBucket.
* [Implantar em Aplicativos Web com o GitHub usando o Kudu](http://azure.microsoft.com/documentation/videos/deploying-to-azure-from-github/). Vídeo de Scott Hanselman e David Ebbo que mostra como implantar um aplicativo Web diretamente do GitHub para Aplicativos Web.
* [Implantar em um botão do Azure para Aplicativos Web](http://azure.microsoft.com/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/). Blog sobre um método para acionar a implantação por meio de um repositório Git.
* [Fórum do Azure para Git, Mercurial e Dropbox](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azuregit).

##<a name="mercurial"></a>Sites de repositório usando o Mercurial

Se usar o [Mercurial](http://mercurial.selenic.com/) como seu sistema de controle do código-fonte e armazenar seu repositório no [CodePlex](http://www.codeplex.com/) ou no [BitBucket](https://bitbucket.org/), você poderá usar recursos integrados do Serviço de Aplicativo do Azure para implantar automaticamente seu conteúdo.

Para obter informações sobre como implantar usando o Mercurial, consulte os recursos a seguir:

* [Publicando por meio do Controle do Código-fonte em Aplicativos Web com Git](web-sites-publish-source-control.md). Embora este tutorial mostre como publicar um repositório Git, o processo para repositórios Mercurial hospedados no CodePlex ou nonBitBucket é semelhante.
* [Fórum do Azure para Git, Mercurial e Dropbox](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azuregit).

##<a name="dropbox"></a>Automatizar a implantação do Dropbox

O [Dropbox](https://www.dropbox.com/) não é um sistema de controle do código-fonte, mas, se você armazenar seu código-fonte nele, poderá automatizar a implantação da sua conta.

* [Implantar em Aplicativos Web por meio do Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). Como usar o [Portal do Azure](http://go.microsoft.com/fwlink/?LinkId=529715) para configurar a implantação do Dropbox.
* [Implantação do Dropbox em Aplicativos Web](http://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Dropbox-Deployment-to-Windows-Azure-Web-Sites). Este vídeo o orientará pelo processo de conexão de uma pasta DropBox a um aplicativo Web do Azure e mostrará com que rapidez você pode colocar um aplicativo Web em funcionamento ou mantê-lo, usando a implantação simples do tipo "arrastar e soltar".
* [Fórum do Azure para Git, Mercurial e Dropbox](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azuregit).

##<a name="vs"></a>Implantar diretamente do Visual Studio

Para obter informações sobre como implantar aplicativos Web por meio do Visual Studio, consulte os recursos a seguir:

* [Introdução ao Microsoft Azure e ao ASP.NET](web-sites-dotnet-get-started.md). Como criar e implantar um projeto da web ASP.NET MVC simples usando o Visual Studio e a Implantação da Web.
* [Como implantar Trabalhos Web do Azure usando o Visual Studio](websites-dotnet-deploy-webjobs.md). Como configurar projetos de Aplicativo de console para que sejam implantados como Trabalhos Web.  
* [Implantar um aplicativo ASP.NET MVC 5 Seguro com Associação, OAuth e Banco de Dados SQL em Aplicativos Web](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Como criar e implantar um projeto da web ASP.NET MVC com um banco de dados SQL, usando o Visual Studio, a Implantação da Web e as Migrações do Entity Framework Code First.
* [Visão geral sobre a implantação da Web para o Visual Studio e o ASP.NET](http://msdn.microsoft.com/library/dd394698.aspx). Uma introdução básica à implantação da web usando o Visual Studio. Desatualizado, mas inclui informações que ainda são relevantes, incluindo uma visão geral das opções para a implantação de um banco de dados junto com o aplicativo da web e uma lista de tarefas de implantação adicionais que você talvez tenha que executar ou configurar o Visual Studio manualmente para executá-las para você. Este tópico é sobre implantação em geral, e não apenas sobre a implantação em aplicativos Web.
* [Implantação da Web do ASP.NET usando o Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). Uma série de tutoriais de 12 partes que abrange uma gama mais completa de tarefas de implantação do que os outros nessa lista. Alguns recursos de implantação do Azure foram adicionados desde que o tutorial foi escrito, mas anotações adicionadas posteriormente explicam o que está faltando.
* [Implantação direta de um site do ASP.NET no Azure no Visual Studio 2012 a partir de um repositório Git](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Explica como implantar um projeto da web ASP.NET no Visual Studio, usando o plug-in do Git para confirmar o código para o Git e conectar o Azure ao repositório Git. A partir do Visual Studio 2013, o suporte ao Git é integrado não requer a instalação de um plug-in.

##<a name="webmatrix"></a>Implantar diretamente do WebMatrix

Para obter informações sobre como implantar aplicativos Web por meio do WebMatrix, consulte os recursos a seguir:

* [Compilar e implantar um site do node.js no Azure usando o WebMatrix](web-sites-nodejs-use-webmatrix.md).
* [Criar e implantar um aplicativo Web do PHP-MySQL usando o WebMatrix](web-sites-php-mysql-use-webmatrix.md).
* [WebMatrix 3: Git Integrado e Implantação no Azure](http://www.codeproject.com/Articles/577581/Webmatrixplus3-3aplusIntegratedplusGitplusandplusD). Como usar o WebMatrix para implantar de um repositório de controle do código-fonte do Git.

Para obter mais informações, consulte os recursos a seguir:

* [Criar um aplicativo Web do PHP-MySQL e implantá-lo usando o FTP](web-sites-php-mysql-deploy-use-ftp.md).

##<a name="tfs"></a>Entrega contínua com o TFS (Team Foundation Server)

O Team Foundation Server é uma solução local da Microsoft para colaboração de equipe e controle do código-fonte. Você pode configurar o TFS para fazer entrega contínua para um aplicativo Web.

Para obter mais informações, consulte o recurso a seguir:

* [Entrega contínua de serviços de nuvem no Azure](../cloud-services-dotnet-continuous-delivery.md). Este documento é destinado a um serviço de nuvem do Azure, mas parte do conteúdo é relevante a aplicativos Web.

##<a name="gitmercurial"></a>Repositórios locais do Git ou do Mercurial

No Azure, você pode inserir a URL de qualquer repositório que usa o Git ou o Mercurial para implantar desse local. Você também pode enviar diretamente por push de um repositório Git local para um aplicativo Web.

Para obter mais informações, consulte os recursos a seguir:

* [Publicando por meio do Controle do Código-fonte em Aplicativos Web com Git](web-sites-publish-source-control.md). Como usar o Git para publicar diretamente do computador local em um aplicativo Web (no Azure, esse método de publicação é denominado Git local). Ele também mostra como habilitar a implantação contínua de repositórios Git a partir do GitHub, do CodePlex ou do BitBucket.
* [Publicando em Aplicativos Web por meio de qualquer repositório git/hg](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html). Blog que explica o recurso "Repositório Externo" em Aplicativos Web.
* [Fórum do Azure para Git, Mercurial e Dropbox](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azuregit).
* [Implantando DOIS sites no Azure de um repositório Git](http://www.hanselman.com/blog/DeployingTWOWebsitesToWindowsAzureFromOneGitRepository.aspx) Postagem de blog de Scott Hanselman.

##<a name="msbuild"></a>Automatizar a implantação com o MSBuild

Se você usar o [IDE do Visual Studio](#vs) para desenvolvimento, poderá usar o [MSBuild](http://msbuildbook.com/) para automatizar qualquer tarefa que possa ser executada no seu IDE. Você pode configurar o MSBuild para usar a [Implantação da Web](#webdeploy) ou o [FTP/FTPS](#ftp) para copiar arquivos. A Implantação da Web também pode automatizar várias outras tarefas relacionadas à implantação tarefas, como a implantação de bancos de dados.

Para obter mais informações sobre a implantação de linha de comando usando o MSBuild, consulte os recursos a seguir:

* [Implantação da Web do ASP.NET usando o Visual Studio: implantação de linha de comando](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). O décimo de uma série de tutoriais sobre implantação no Azure usando o Visual Studio. Mostra como usar a linha de comando para implantar após a configuração de perfis de publicação no Visual Studio.
* [Por dentro do Microsoft Build Engine: usando o MSBuild e o Team Foundation Build](http://msbuildbook.com/). Livro impresso que contém capítulos sobre como usar o MSBuild para implantação.

##<a name="ftp"></a>Copiar arquivos com scripts e ferramentas de FTP

Você pode implantar conteúdo em seu aplicativo usando [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) para copiar arquivos. É fácil criar credenciais de FTP para um aplicativo Web, e você pode usá-las em scripts ou aplicativos que funcionem com o FTP, incluindo navegadores como o Internet Explorer e utilitários gratuitos completos, como o [FileZilla](https://filezilla-project.org/). Aplicativos Web também dão suporte ao protocolo FTPS, que é mais seguro.

Embora seja fácil copiar os arquivos do seu aplicativo Web para o Azure usando os utilitários FTP, estes não são capazes de, automaticamente, cuidar das tarefas de implantação relacionadas ou coordená-las; tarefas estas como, por exemplo, implantar um banco de dados ou alterar as cadeias de conexão. Além disso, várias ferramentas de FTP não comparam os arquivos de origem com os arquivos de destino para ignorar a cópia dos arquivos inalterados. Para aplicativos grandes, copiar todos os arquivos pode resultar em longos tempos de implantação até mesmo para atualizações menores, já que todos os arquivos serão sempre copiados.

Para obter mais informações, consulte o recurso a seguir:

* [Usando scripts em lotes FTP](http://support.microsoft.com/kb/96269).

##<a name="powershell"></a>Automatizar a implantação com o Windows PowerShell

Você pode executar funções de implantação do MSBuild ou de FTP a partir do [Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx). Se fizer isso, você também poderá usar uma coleção de cmdlets do Windows PowerShell que facilitam chamar a API de gerenciamento do REST do Azure.

Para saber mais, consulte os recursos a seguir:

* [Implantar um aplicativo Web vinculado a um repositório GitHub](app-service-web-arm-from-github-provision.md)
* [Provisionar um aplicativo Web com um banco de dados SQL](app-service-web-arm-with-sql-database-provision.md)
* [Provisionar e implantar microsserviços previsíveis no Azure](app-service-deploy-complex-application-predictably.md)
* [Compilando aplicativos de nuvem do mundo real com o Azure - automatizar tudo](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). Capítulo de livro eletrônico que explica como o aplicativo de amostra mostrado no livro eletrônico usa scripts do Windows PowerShell para criar um ambiente de teste do Azure e implantá-lo. Consulte a seção [Recursos](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) para obter links para a documentação adicional do PowerShell do Azure.
* [Usando scripts do Windows PowerShell para publicar em ambientes de desenvolvimento e de teste](http://msdn.microsoft.com/library/dn642480.aspx). Como usar os scripts de implantação do Windows PowerShell gerados pelo Visual Studio.

##<a name="api"></a>Automatizar a implantação com a API de gerenciamento do .NET

Você pode escrever um código C# para executar as funções do MSBuild ou do FTP para implantação. Se fizer isso, você poderá acessar a API de gerenciamento do REST do Azure para executar as funções de gerenciamento de site.

Para obter mais informações, consulte o recurso a seguir:

* [Automatizando tudo com as bibliotecas de gerenciamento do Azure e o .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) Introdução à API de gerenciamento do .NET e links para mais documentação.

##<a name="cli"></a>Implantar da CLI (Interface de Linha de Comando) do Azure

É possível utilizar a linha de comando em máquinas Windows, Mac ou Linux para implantar utilizando o FTP. Se fizer isso, você também poderá acessar a API de gerenciamento do REST do Azure usando a CLI do Azure.

Para obter mais informações, consulte o recurso a seguir:

* [Ferramentas de linha de comando do Azure.](/downloads/#cmd-line-tools). Página de portal em Azure.com que fornece informações da ferramenta de linha de comando.

##<a name="webdeploy"></a>Implantar da linha de comando da Implantação na Web

A [Implantação da Web](http://www.iis.net/downloads/microsoft/web-deploy) é o software da Microsoft para a implantação do IIS que não só fornece recursos de sincronização de arquivos inteligentes, mas também pode executar ou coordenar várias outras tarefas relacionadas à implantação que não poderão ser automatizadas quando você usar o FTP. Por exemplo, a Implantação da Web pode implantar um novo banco de dados ou atualizações de banco de dados junto com seu aplicativo Web. A Implantação da Web também pode minimizar o tempo necessário para atualizar um site existente, pois ele pode copiar apenas os arquivos alterados de forma inteligente. O Microsoft WebMatrix, o Visual Studio, o Visual Studio Team Services e o Team Foundation Server têm suporte para a Implantação da Web interna, mas você também pode usar a Implantação da Web diretamente da linha de comando para automatizar a implantação. Os comandos de Implantação da Web são muito avançados, mas a curva de aprendizado pode ser acentuada.

Para obter mais informações, consulte o recurso a seguir:

* [Aplicativos Web simples: implantação](http://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog de David Ebbo sobre uma ferramenta que ele escreveu para facilitar o uso da Implantação da Web.
* [Ferramenta de Implantação de Web](http://technet.microsoft.com/library/dd568996). Documentação oficial no site do Microsoft TechNet. Desatualizada, mas ainda um bom ponto de partida.
* [Usando a Implantação da Web](http://www.iis.net/learn/publish/using-web-deploy). Documentação oficial no site do Microsoft IIS.NET. Também desatualizada, mas ainda um bom ponto de partida.
* [StackOverflow](http://www.stackoverflow.com). O melhor lugar para obter informações mais atuais sobre como usar a Implantação da Web da linha de comando.
* [Implantação da Web do ASP.NET usando o Visual Studio: implantação de linha de comando](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). O MSBuild é o mecanismo de compilação usado pelo Visual Studio e também pode ser usado na linha de comando para implantar aplicativos Web em "Aplicativos Web". Este tutorial faz parte de uma série que trata principalmente da implantação do Visual Studio.

##<a name="nextsteps"></a>Próximas etapas

Em algumas situações, você provavelmente desejará ser capaz de alternar facilmente entre a versão de preparo e a versão de produção do seu aplicativo Web. Para obter mais informações, consulte [Implantação preparada em Aplicativos Web](web-sites-staged-publishing.md).

Uma parte importante de qualquer fluxo de trabalho de implantação é ter um plano de backup e restauração. Para obter informações sobre o recurso de backup e restauração dos Aplicativos Web, consulte [Backups de Aplicativos Web](web-sites-backup.md).

Para obter informações sobre como usar o Controle de Acesso Baseado em Função do Azure para gerenciar o acesso à implantação de Aplicativos Web, consulte [RBAC e a publicação de Aplicativos Web](http://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing).

Para obter mais informações sobre outros tópicos de implantação, consulte a seção Implantar na [Documentação de Aplicativos Web](/documentation/services/web-sites/).

## O que mudou
* Para obter um guia sobre a alteração de Sites para o Serviço de Aplicativo, consulte: [Serviço de Aplicativo do Azure e seu impacto sobre os serviços do Azure existentes](http://go.microsoft.com/fwlink/?LinkId=529714)
 

<!----HONumber=AcomDC_1217_2015-->