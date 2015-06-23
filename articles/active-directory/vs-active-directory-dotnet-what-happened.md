<properties 
	pageTitle="Introdução à Autenticação do Active Directory - O que aconteceu" 
	description="Descreve o que aconteceu no seu projeto do Active Directory do Azure no Visual Studio" 
	services="active-directory" 
	documentationCenter="" 
	authors="kempb" 
	manager="douge" 
	editor="tglee"/>
  
<tags 
	ms.service="active-directory" 
	ms.workload="web" 
	ms.tgt_pltfrm="vs-what-happened" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/02/2015" 
	ms.author="kempb"/>

# O que aconteceu com meu projeto?

> [AZURE.SELECTOR]
> - [Introdução](vs-active-directory-dotnet-getting-started.md)
> - [O que aconteceu](vs-active-directory-dotnet-what-happened.md)

###<span id="whathappened">O que aconteceu com meu projeto?</span>
 
Referências foram adicionadas.

#####Referências do pacote NuGet

- `Microsoft.IdentityModel.Protocol.Extensions`
- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.Cookies`
- `Microsoft.Owin.Security.OpenIdConnect`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

#####Referências .NET

- `Microsoft.IdentityModel.Protocol.Extensions`
- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.Cookies`
- `Microsoft.Owin.Security.OpenIdConnect`
- `Owin`
- `System`
- `System.Data`
- `System.Drawing`
- `System.IdentityModel`
- `System.IdentityModel.Tokens.Jwt`
- `System.Runtime.Serialization`

#####Arquivos de código foram adicionados ao seu projeto 

Uma classe de inicialização de autenticação,  `App_Start/Startup.Auth.cs` foi adicionada ao seu projeto contendo a lógica de inicialização para autenticação do AD do Azure.  Também, uma classe de controlador, Controllers/AccountController.cs, foi adicionada e contém os métodos  `SignIn()` e  `SignOut()`.  Finalmente, uma exibição parcial,  `Views/Shared/_LoginPartial.cshtml`, foi adicionada contendo um link de ação para Efetuar login/logout. 

#####O código de inicialização foi adicionado ao seu projeto
 
Se você já tiver uma classe de Inicialização em seu projeto, o método **Configuração** terá sido atualizado para incluir uma chamada para  `ConfigureAuth(app)`, que foi incluída nesse método.  Caso contrário, uma classe Startup foi adicionada ao projeto. 

#####Seu app.config ou web.config tem novos valores de configuração 

As entradas de configuração a seguir foram adicionadas. 
	<pre>
	`<appSettings>
	    <add key="ida:ClientId" value="ClientId from the new Azure AD App" /> 
	    <add key="ida:Tenant" value="Your selected Azure AD Tenant" /> 
	    <add key="ida:AADInstance" value="https://login.windows.net/{0}" /> 
	    <add key="Ida:PostLogoutRedirectURI" value="Your project start page" /> 
	</appSettings>` </pre>

#####Um aplicativo do Azure Active Directory (AD) foi criado 
Um Aplicativo Azure AD foi criado no diretório selecionado no assistente. 

[Saiba mais sobre o Active Directory do Azure](http://azure.microsoft.com/services/active-directory/)

<!--HONumber=49--> 