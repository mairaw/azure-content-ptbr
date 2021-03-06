<properties 
	pageTitle="Habilitar área de trabalho remota para serviços de nuvem (Node.js)" 
	description="Saiba como habilitar o acesso à área de trabalho remota para as máquinas virtuais que hospedam o aplicativo Azure Node.js." 
	services="cloud-services" 
	documentationCenter="nodejs" 
	authors="rmcmurray" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="cloud-services" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="na" 
	ms.devlang="nodejs" 
	ms.topic="article" 
	ms.date="11/20/2015" 
	ms.author="robmcm"/>


# Habilitando a Área de Trabalho Remota no Azure

A Área de Trabalho Remota permite que você acesse a área de trabalho da instância de função em execução no Azure. É possível usar uma conexão de área de trabalho remota para configurar a máquina virtual ou solucionar problemas do seu aplicativo.

> [AZURE.NOTE]Este artigo se aplica a aplicativos Node.js hospedados como um Serviço de Nuvem do Azure.


## Pré-requisitos

- Instalar e configurar o [Powershell do Azure](../install-configure-powershell.md).
- Implantar um aplicativo Node.js para um Serviço de Nuvem do Azure Para saber mais, consulte [Criar e implantar um aplicativo Node.js em um Serviço de Nuvem do Azure](cloud-services-nodejs-develop-deploy-app.md).


## Etapa 1: Usar o PowerShell do Azure para configurar o serviço para acesso à área de trabalho remota

Para usar a Área de Trabalho Remota, você precisa atualizar a definição e a configuração de serviço do Azure com um nome de usuário, senha e certificado.

Execute as etapas a seguir em um computador que contém os arquivos de origem de seu aplicativo.

1. Execute o **PowerShell do Azure** como Administrador. (No **menu Iniciar** ou **tela Iniciar**, pesquise por **PowerShell do Azure**.)

2.  Navegue até o diretório que contém a definição de serviço (.csdef) e os arquivos de configuração de serviço (.cscfg).

3. Insira o seguinte cmdlet do PowerShell:

		Enable-AzureServiceProjectRemoteDesktop

4. No prompt, insira um nome de usuário e senha.

	![enable-azureserviceprojectremotedesktop][enable-rdp]

3.  Insira o seguinte cmdlet do PowerShell para publicar as alterações:

    	Publish-AzureServiceProject

	![publish-azureserviceproject][publish-project]

## Etapa 2: Conectar-se à instância da função

Depois de publicar a definição do serviço de atualização, você pode se conectar à instância da função.

1.  No [portal clássico do Azure], selecione **Serviços de Nuvem ** e selecione o serviço.

	![portal clássico do Azure][cloud-services]

2.  Clique em **Instâncias** e, em seguida, clique em **Produção** ou **Preparo** para ver as instâncias de seu serviço. Selecione uma instância e clique em **Conectar** na parte inferior da página.

    ![A página de instâncias][3]

2.  Quando você clica em **Conectar**, o navegador da Web solicita que você salve um arquivo .rdp. Abra este arquivo. (Por exemplo, se você estiver usando o Internet Explorer, clique em **Abrir**.)

    ![solicitação para abrir ou salvar o arquivo .rdp][4]

3.  Quando o arquivo é aberto, o seguinte prompt de segurança é exibido:

    ![Prompt de segurança do Windows][5]

4.  Clique em **Conectar** e será exibido um prompt de segurança para a inserção das credenciais para acessar a instância. Digite a senha criada na [Etapa1]\[Etapa 1: Configurar o serviço para acesso à Área de Trabalho Remota usando o PowerShell do Azure] e, em seguida, clique em **OK**.

    ![prompt de nome de usuário/senha][6]

Quando a conexão for feita, a Conexão de Área de Trabalho Remota exibe a área de trabalho da instância no Azure.

![Sessão de área de trabalho remota][7]

## Etapa 3: Configurar o serviço para desabilitar o acesso à Área de Trabalho Remota 

Quando você não precisar mais de conexões de área de trabalho remota para as instâncias de função na nuvem, desabilite o acesso à área de trabalho remota usando o [PowerShell do Azure].

1.  Insira o seguinte cmdlet do PowerShell:

    	Disable-AzureServiceProjectRemoteDesktop

2.  Insira o seguinte cmdlet do PowerShell para publicar as alterações:

    	Publish-AzureServiceProject

## Recursos adicionais

- [Acessando instâncias de função remotamente no Azure] 
- [Usando a Área de Trabalho Remota com as funções do Azure]
- [Centro de desenvolvedores do Node. js](/develop/nodejs/)

  [PowerShell do Azure]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[portal clássico do Azure]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
  [3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
  [4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
  [5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
  [6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
  [7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
  [Acessando instâncias de função remotamente no Azure]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
  [Usando a Área de Trabalho Remota com as funções do Azure]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 

<!-----HONumber=AcomDC_1203_2015-->
