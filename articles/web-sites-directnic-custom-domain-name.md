<properties 
	pageTitle="Configurar um nome de domínio Directnic para um Website do Azure" 
	description="Saiba como configurar um site do Azure para usar um nome de domínio registrado com o Directnic" 
	services="web-sites" 
	documentationCenter="" 
	authors="blackmist" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="web-sites" 
	ms.workload="web" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/17/2014" 
	ms.author="larryfr,jroth"/>

#Configurando um nome de domínio personalizado para um Site do Azure (Directnic)

<div class="dev-center-tutorial-selector sublanding"><a href="/pt-br/documentation/articles/web-sites-custom-domain-name" title="Custom Domain">Domínio personalizado</a><a href="/pt-br/documentation/articles/web-sites-godaddy-custom-domain-name" title="GoDaddy">GoDaddy</a><a href="/pt-br/documentation/articles/web-sites-network-solutions-custom-domain-name" title="Network Solutions">soluções de rede</a><a href="/pt-br/documentation/articles/web-sites-registerdotcom-custom-domain-name" title="Register.com">Register.com</a><a href="/pt-br/documentation/articles/web-sites-enom-custom-domain-name" title="Enom">Enom</a><a href="/pt-br/documentation/articles/web-sites-moniker-custom-domain-name" title="Moniker">Moniker</a><a href="/pt-br/documentation/articles/web-sites-dotster-custom-domain-name" title="Dotster">Dotster</a><a href="/pt-br/documentation/articles/web-sites-domaindiscover-custom-domain-name" title="DomainDiscover">DomainDiscover</a><a href="/pt-br/documentation/articles/web-sites-directnic-custom-domain-name" title="Directnic" class="current">Directnic</a></div>
<div class="dev-center-tutorial-subselector"><a href="/pt-br/documentation/articles/web-sites-directnic-custom-domain-name/" title="Websites" class="current">Website</a> | <a href="/pt-br/documentation/articles/web-sites-directnic-traffic-manager-custom-domain-name/" title="Website using Traffic Manager"> Website usando o Gerenciador de tráfego</a></div>

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../includes/websites-cloud-services-css-guided-walkthrough.md)]


[AZURE.INCLUDE [Introdução](../includes/custom-dns-web-site-intro.md)]

Este artigo fornece instruções sobre como utilizar um nome de domínio personalizado adquirido do [Directnic.com](https://directnic.com) com os Sites do Azure.

[AZURE.INCLUDE [introfooter](../includes/custom-dns-web-site-intro-notes.md)]

Neste artigo:

-   [Compreendendo os registros DNS](#understanding-records)
-   [Configurar seus sites para modo básico, compartilhado ou padrão](#bkmk_configsharedmode)
-   [Adicionar um registro DNS a seu domínio personalizado](#bkmk_configurecname)
-   [Habilitar o domínio no site](#enabledomain)

<h2><a name="understanding-records"></a>Compreendendo os registros DNS</h2>

[AZURE.INCLUDE [understandingdns](../includes/custom-dns-web-site-understanding-dns-raw.md)]

<h2><a name="bkmk_configsharedmode"></a>Configurar seus sites para modo básico, compartilhado ou padrão</h2>

[AZURE.INCLUDE [modos](../includes/custom-dns-web-site-modes.md)]

<a name="bkmk_configurecname"></a><h2>Adicionar um registro DNS a seu domínio personalizado</h2>

Para associar seu domínio personalizado a um Site do Azure, você deve adicionar uma nova entrada na tabela DNS para seu domínio personalizado utilizando as ferramentas fornecidas pelo Directnic. Use as seguintes etapas para localizar as ferramentas DNS para o Directnic.com

1. Faça logon na sua conta do Directnic.com e selecione **Meus Serviços** e, em seguida, **Domínios**.

    ![Menu de serviços do Directnic](.\media\web-sites-directnic-custom-domain-name\Directnic_DomainMenu.png)

2. Clique no nome do domínio que você deseja usar com o Site do Azure.

2. Na página de gerenciamento de seu domínio, clique no botão **Gerenciar** do **DNS** no painel **Serviços**.

    ![Painel Serviços](.\media\web-sites-directnic-custom-domain-name\Directnic_DomainManagement.png)

4. Adicione registros DNS preenchendo os campos **Tipo**, **Nome** e **Dados**. Ao concluir, clique no botão **Adicionar Registro**.   

    ![](.\media\web-sites-directnic-custom-domain-name\Directnic_DNS.png)

    * Ao adicionar um registro CNAME, você deve definir o campo **Nome** como o subdomínio que deseja usar. Por exemplo, **www**. Você deve definir o campo **Dados** como o nome do domínio **.azurewebsites.net** do Site do Azure. Por exemplo, **contoso.azurwebsites.net**.

    *  Ao adicionar um registro A, você deve definir o campo **Nome** como **@** (isso representa o nome do domínio raiz, como **contoso.com**) ou o subdomínio que você deseja usar (por exemplo, **www**). É necessário definir o campo **Dados** como o endereço IP do seu Site do Azure.

		> [AZURE.NOTE] Se estiver usando um registro A, você também deverá adicionar um registro CNAME com uma das seguintes configurações:
		> 
		> *Um valor **nome** de **www** com um valor **dados** de **&lt;yourwebsitename&gt;. azurewebsites.net**.
		> 
		> OU
		> 
		> *Um valor **nome** de **awverify** com um valor **dados** de **awverify.&lt;yourwebsitename&gt;.azurewebsites.net**.
		> 
		> Esse registro CNAME é usado pelo Azure para validar que você possui o domínio descrito pelo registro A


<h2><a name="enabledomain"></a>Habilitar o nome do domínio no seu site</h2>

[AZURE.INCLUDE [modos](../includes/custom-dns-web-site-enable-on-web-site.md)]



<!--HONumber=42-->