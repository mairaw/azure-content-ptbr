﻿<properties 
   pageTitle="Criar e carregar um VHD FreeBSD para o Azure" 
   description="Saiba como criar e carregar um VHD (disco rígido virtual) do Azure que contenha um sistema operacional FreeBSD" 
   services="virtual-machines" 
   documentationCenter="" 
   authors="KylieLiang" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services" 
   ms.date="02/05/2015"
   ms.author="kyliel"/>

# Criar e carregar um VHD FreeBSD para o Azure 

Esse artigo mostra como criar e carregar um disco rígido virtual (VHD) que contém o sistema operacional FreeBSD para que você o use como sua própria imagem para criar outras máquinas virtuais no Azure. 

##Pré-requisitos##
Este artigo pressupõe que você tenha os seguintes itens:

- **Uma assinatura do Azure** - Se você não tiver uma, você poderá criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [Criar uma conta do Azure](http://azure.microsoft.com/documentation/articles/php-create-account/). 

- **Ferramentas Azure PowerShell** - Você tem o módulo do Microsoft Azure PowerShell instalado e configurado para usar sua assinatura. Para baixar o módulo, consulte [Downloads do Azure](http://azure.microsoft.com/downloads/) (a página pode estar em inglês). Um tutorial para instalar e configurar o módulo está disponível aqui. Você usará o cmdlet [Downloads do Azure](http://azure.microsoft.com/downloads/) para carregar o VHD.

- **Sistema operacional FreeBSD instalado em um arquivo .vhd**  - Você instalou um sistema operacional FreeBSD com suporte em um disco rígido virtual. Existem várias ferramentas para criar arquivos .vhd - por exemplo, você pode usar uma solução de virtualização como o Hyper-V para criar o arquivo .vhd e instalar o sistema operacional. Para obter instruções, consulte [Instalar a função Hyper-V e configurar uma máquina virtual](http://technet.microsoft.com/library/hh846766.aspx). 

> [AZURE.NOTE] Não há suporte para o formato VHDX mais recente no Azure. Você pode converter o disco em formato VHD usando o Gerenciador do Hyper-V ou o cmdlet [convert-VHD](https://technet.microsoft.com/pt-br/library/hh848454.aspx).

Esta tarefa inclui as cinco etapas a seguir.

## Etapa 1: Preparar a imagem a ser carregada ##

Para instalação FreeBSD no Hyper-v, um tutorial está disponível [aqui](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).

Na máquina virtual na qual o sistema operacional FreeBSD foi instalado, conclua o procedimento a seguir:

1. **Habilitar DHCP**

		# echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
		# service netif restart

2. **Habilitar SSH**

    O SSH é habilitado por padrão após a instalação do disco. Se não for, ou se você usar o VHD FreeBSD diretamente, digite:

		# echo 'sshd_enable="YES"' >> /etc/rc.conf 
		# ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key 
		# ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key 
		# service sshd restart

3. **Instalar console serial**

		# echo 'console="comconsole vidconsole"' >> /boot/loader.conf
		# echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. **Instalar sudo**

    A conta raiz será desabilitada no Azure e você precisará utilizar sudo de um usuário sem privilégios para executar comandos com privilégios elevados.

		# pkg install sudo

5. Pré-requisitos para o agente do Azure

    5.1 **Instalar o python**

		# pkg install python27 py27-asn1
		# ln -s /usr/local/bin/python2.7 /usr/bin/python

    5.2 **Instalar o wget**

		# pkg install wget 

6. **Instalar o agente do Azure**

    A versão mais recente do agente do Azure sempre pode ser encontrada no [github](https://github.com/Azure/WALinuxAgent/releases). A Versão 2.0.10 e posterior dá suporte oficialmente ao FreeBSD 10 e a versões posteriores.

		# wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.10/waagent --no-check-certificate
		# mv waagent /usr/sbin
		# chmod 755 /usr/sbin/waagent
		# /usr/sbin/waagent -install

    **Importante**: Após a instalação, verifique novamente se ele está em execução.

		# service -e | grep waagent
		/etc/rc.d/waagent
		# cat /var/log/waagent.log

    Só então você poderá **desligar** sua VM. Você também pode executar a etapa 7 antes do desligamento, mas isso é opcional.

7. O cancelamento de provisionamento é opcional. Ele serve para limpar o sistema e torná-lo adequado para reprovisionamento.

    O comando abaixo também exclui a última conta de usuário provisionada e dados associados.

		# waagent -deprovision+user

## Etapa 2: Criar uma conta de armazenamento no Azure ##

Você precisa de uma conta de armazenamento no Azure para carregar um arquivo .vhd para que ele possa ser usado no Azure para criar uma máquina virtual. Entre no Portal de Gerenciamento do Azure para criar uma conta de armazenamento.

1. Entre no Portal de Gerenciamento do Azure.

2. Na barra de comandos, clique em **Novo**.

3. Clique em **Serviços de Dados** > **Armazenamento** > **Criação Rápida**.

	![Quick create a storage account](./media/virtual-machines-create-upload-vhd-windows-server/Storage-quick-create.png)

4. Preencha os campos da seguinte maneira:
	
	- Em **URL**, digite um nome de subdomínio para usar no URL para a conta de armazenamento. A entrada pode conter de 3 a 24 letras minúsculas e números. Esse nome se torna o nome do host na URL que é usada para lidar com os recursos Blob, Fila ou Tabela da assinatura.
			
	- Escolha o **local ou o grupo de afinidade** da conta de armazenamento. Um grupo de afinidade permite colocar seus serviços de nuvem e armazenamento no mesmo data center.
		 
	- Escolha entre usar a **replicação geográfica** ou não para a conta de armazenamento. A replicação geográfica é ativada por padrão. Essa opção replica os dados para um local secundário, sem nenhum custo para você, para que o armazenamento efetue o failover para o local se ocorrer uma falha grave no local principal. O local secundário é atribuído automaticamente e não pode ser alterado. Se você precisar de mais controle sobre o local do armazenamento baseado em nuvem devido a requisitos legais ou política organizacional, você pode desligar a replicação geográfica. No entanto, saiba que ao ativar a replicação geográfica posteriormente, você deverá pagar uma taxa de transferência de dados uma única vez para replicar seus dados existentes para o local secundário. Serviços de armazenamento sem replicação geográfica são oferecidos com desconto. Você pode encontrar mais detalhes sobre como gerenciar contas de armazenamento sem replicação geográfica aqui: [Criar, gerenciar ou excluir uma conta de armazenamento](../storage-create-storage-account/#replication-options).

	![Enter storage account details](./media/virtual-machines-create-upload-vhd-windows-server/Storage-create-account.png)


5. Clique em **Criar Conta de Armazenamento**. A conta aparece agora em **armazenamento**.

	![Storage account successfully created](./media/virtual-machines-create-upload-vhd-windows-server/Storagenewaccount.png)

6. Em seguida, crie um contêiner para os seus VHDs carregados. Clique no nome da conta de armazenamento e, em seguida, clique em **Contêineres**.

	![Storage account detail](./media/virtual-machines-create-upload-vhd-windows-server/storageaccount_detail.png)

7. Clique em **Criar um contêiner**.

	![Storage account detail](./media/virtual-machines-create-upload-vhd-windows-server/storageaccount_container.png)

8. Digite um **Nome** para o seu contêiner e selecione a Política de **Acesso**.

	![Container name](./media/virtual-machines-create-upload-vhd-windows-server/storageaccount_containervalues.png)

    > [AZURE.NOTE] Por padrão, o contêiner é privado e pode ser acessado apenas pelo proprietário da conta. Para permitir acesso de leitura público dos blobs no contêiner, mas não das propriedades ou metadados do contêiner, use a opção "Public Blob". Para permitir o acesso de leitura público completo dos dados do contêiner e de blobs, use a opção "Public Container".

## Etapa 3: Prepare a conexão com o Microsoft Azure ##

Para poder carregar um arquivo .vhd, você precisa estabelecer uma conexão segura entre seu computador e sua assinatura do Azure. No entanto, você pode usar o método Active Directory do Microsoft Azure ou o método de certificado para realizar esse tipo de trabalho.

<h3>Use o método do AD do Microsoft Azure</h3>

1. Abra o console do PowerShell do Azure.

2. Digite o seguinte comando:  
	`Add-AzureAccount`
	
	Esse comando abre uma janela de entrada para que você possa conectar-se com sua conta do trabalho ou escola.

	![PowerShell Window](./media/virtual-machines-create-upload-vhd-windows-server/add_azureaccount.png)

3. O Azure autentica e salva as informações de credenciais e, em seguida, fecha a janela.

<h3>Usar o método de certificado</h3> 

1. Abra o console do PowerShell do Azure. 

2. Digite: 
	`Get-AzurePublishSettingsFile`.

3. Uma janela de navegador será aberta e baixará automaticamente um arquivo .publishsettings. Esse arquivo contém informações e um certificado de sua assinatura do Microsoft Azure.

	![Browser download page](./media/virtual-machines-create-upload-vhd-windows-server/Browser_download_GetPublishSettingsFile.png)

3. Salve o arquivo .publishsettings. 

4. Digite: 
	`Import-AzurePublishSettingsFile <CaminhoParaOArquivo>`

	Em que `<CaminhoParaOArquivo>` é o caminho completo para o arquivo .publishsettings. 

   Para obter mais informações, consulte [Introdução aos cmdlets do Microsoft Azure](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx) 
	
   Para obter mais informações sobre instalação e configuração do PowerShell, consulte [Como instalar e configurar o PowerShell do Microsoft Azure](http://azure.microsoft.com/documentation/articles/install-configure-powershell/). 

## Etapa 4: Carregar o arquivo .vhd ##

Quando carrega o arquivo .vhd, você pode colocá-lo em qualquer lugar no seu armazenamento de blob. Nos exemplos de comando a seguir, **BlobStorageURL** é a URL da conta de armazenamento que você criou na Etapa 2, **YourImagesFolder** é o contêiner no armazenamento de blob onde você deseja armazenar as imagens. **VHDName** é o rótulo que aparece no Portal de Gerenciamento para identificar o disco rígido virtual. **PathToVHDFile** é o caminho completo e o nome do arquivo .vhd. 


1. Na janela PowerShell do Azure que você usou na etapa anterior, digite:

		Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>		

## Etapa 5: Criar uma máquina virtual com o VHD carregado ##
Depois de carregar o VHD, você pode adicioná-lo como uma imagem à lista de imagens personalizadas associadas à sua assinatura e criar uma máquina virtual com essa imagem personalizada.

1. Na janela PowerShell do Azure que você usou na etapa anterior, digite:

		Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    **Importante**: Use o Linux como sistema operacional por enquanto, pois a versão atual do PowerShell do Azure aceita apenas "Linux" ou "Windows" como parâmetro.

2. Depois de concluir as etapas anteriores, a nova imagem será listada ao escolher a guia **Imagens** no portal de gerenciamento do Azure.  

    ![add image](./media/virtual-machines-freebsd-create-upload-vhd/addfreebsdimage.png)

3. Crie uma máquina virtual na galeria. Essa nova imagem agora está disponível em **Minhas imagens**. Selecione a nova imagem e percorra as instruções para configurar um nome de host, a chave SSH/senha, etc. 

	![custom image](./media/virtual-machines-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. Quando o provisionamento for concluído, você verá sua VM FreeBSD em execução no Azure. 

	![freebsd image in azure](./media/virtual-machines-freebsd-create-upload-vhd/freebsdimageinazure.png)

<!--HONumber=45--> 