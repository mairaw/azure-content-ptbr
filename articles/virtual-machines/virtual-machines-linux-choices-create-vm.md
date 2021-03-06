<properties
	pageTitle="Diferentes maneiras de criar uma VM do Linux | Microsoft Azure"
	description="Lista as diferentes maneiras de criar uma máquina virtual do Linux no Azure e fornece links para mais instruções."
	services="virtual-machines"
	documentationCenter=""
	authors="dsk-2015"
	manager="timlt"
	editor=""
	tags="azure-service-management,azure-resource-manager"/>

<tags
	ms.service="virtual-machines"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-linux"
	ms.workload="infrastructure-services"
	ms.date="08/12/2015"
	ms.author="dkshir"/>

# Diferentes maneiras de criar uma Máquina Virtual Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

O Azure oferece diferentes maneiras de criar uma máquina virtual, pois as VMs são adequadas para fins e usuários diferentes. Isso significa que você precisará fazer algumas escolhas sobre a máquina virtual e como a criará. Este artigo fornece um resumo dessas opções e links para instruções.

Os modelos do Gerenciador de Recursos do Azure foram introduzidos recentemente como uma maneira de criar e gerenciar uma máquina virtual e seus recursos diferentes, como uma unidade lógica de implantação. Instruções para essa abordagem são incluídas abaixo, quando disponíveis. Para saber mais sobre o Gerenciador de Recursos do Azure e como gerenciar recursos como uma unidade, confira o [visão geral][].

## Opções da ferramenta

### GUI: O portal clássico do Azure ou portal do Azure

A interface gráfica do usuário do portal é uma maneira fácil de experimentar uma máquina virtual, especialmente se você estiver iniciando no Azure. Use o [portal clássico do Azure](http://manage.windowsazure.com) ou o [portal do Azure](http://portal.azure.com) para criar a VM. Para obter instruções gerais, consulte [Criar uma máquina virtual personalizada][] e selecione qualquer imagem do Linux na **Galeria**. Observe que o [portal clássico do Azure](http://manage.windowsazure.com) cria máquinas virtuais usando o modelo de implantação clássica.

### Shell de comando: CLI do Azure ou PowerShell do Azure

Se você preferir trabalhar em um shell de comando, escolha entre a interface de linha de comando (CLI) do Azure para usuários do Mac e Linux ou o PowerShell do Azure, que tem os cmdlets do Windows PowerShell para Azure e um console personalizado.

Para a CLI do Azure, consulte [Criar uma máquina virtual que executa Linux][]. Para usar um modelo, confira [Implantar e gerenciar Máquinas Virtuais usando modelos de Gerenciador de Recursos do Azure e a CLI do Azure][].

Para o PowerShell do Azure, confira [Usar o PowerShell do Azure para criar e pré-configurar máquinas virtuais do Azure baseadas em Linux][]. Para usar um modelo, confira [Implantar e gerenciar Máquinas Virtuais usando modelos de Gerenciador de Recursos e o PowerShell][].

### Ambiente de desenvolvimento: Visual Studio

[Criando uma máquina virtual para um site com o Visual Studio][]

[Implantar recursos do Azure usando bibliotecas de computação, rede e armazenamento do .NET][]

## Sistema operacional e opções de imagem

Escolha uma imagem com base no sistema operacional que você deseja executar. A Azure e seus parceiros oferecem muitas imagens, algumas delas incluem aplicativos e ferramentas. Ou use uma das suas próprias imagens.

### Imagens do Azure

Estas instruções mostram como usar uma imagem do Azure para criar uma máquina virtual personalizada com opções de rede, balanceamento de carga e muito mais. Consulte [Como criar uma máquina virtual personalizada que executa Linux no Azure][].

### Usar sua própria imagem

Use uma imagem com base em uma máquina virtual do Azure existente *capturando* essa VM ou carregue uma imagem de sua preferência, armazenada em um disco rígido virtual (VHD):

- [Como capturar uma máquina virtual do Linux para usar como um modelo com a CLI][]
- [Criando e Carregando um Disco Rígido Virtual que Contém o Sistema Operacional Linux][]

## Próximas etapas

[Fazer logon na máquina virtual][]

[Anexar um disco de dados][]

## Recursos adicionais

[Ambiente de teste de configuração básica][]

[Ambientes de teste de nuvem híbrida do Azure][]

[Comandos equivalentes do Gerenciador de Recursos e de Gerenciamento de Serviços para operações de VM com a CLI do Azure para Mac, Linux e Windows][]

<!-- LINKS -->
[visão geral]: ../resource-group-overview.md

[Create a Virtual Machine Running Windows]: virtual-machines-windows-tutorial.md
[Create a Virtual Machine Running Linux]: virtual-machines-linux-tutorial.md

[Comandos equivalentes do Gerenciador de Recursos e de Gerenciamento de Serviços para operações de VM com a CLI do Azure para Mac, Linux e Windows]: xplat-cli-azure-manage-vm-asm-arm.md
[Implantar e gerenciar Máquinas Virtuais usando modelos de Gerenciador de Recursos do Azure e a CLI do Azure]: virtual-machines-deploy-rmtemplates-azure-cli.md
[Implantar e gerenciar Máquinas Virtuais usando modelos de Gerenciador de Recursos e o PowerShell]: virtual-machines-deploy-rmtemplates-powershell.md
[Usar o PowerShell do Azure para criar e pré-configurar máquinas virtuais do Azure baseadas em Linux]: virtual-machines-ps-create-preconfigure-linux-vms.md

[Como criar uma máquina virtual personalizada que executa Linux no Azure]: virtual-machines-linux-create-custom.md
[Como capturar uma máquina virtual do Linux para usar como um modelo com a CLI]: virtual-machines-linux-capture-image.md

[Criando e Carregando um Disco Rígido Virtual que Contém o Sistema Operacional Linux]: virtual-machines-linux-create-upload-vhd.md

[Criando uma máquina virtual para um site com o Visual Studio]: virtual-machines-dotnet-create-visual-studio-powershell.md
[Implantar recursos do Azure usando bibliotecas de computação, rede e armazenamento do .NET]: virtual-machines-arm-deployment.md

[Fazer logon na máquina virtual]: virtual-machines-linux-how-to-log-on.md

[Anexar um disco de dados]: virtual-machines-linux-how-to-attach-disk.md

[Ambiente de teste de configuração básica]: virtual-machines-base-configuration-test-environment.md
[Ambientes de teste de nuvem híbrida do Azure]: virtual-machines-hybrid-cloud-test-environments.md

[Criar uma máquina virtual que executa Linux]: virtual-machines-linux-tutorial.md
[Criar uma máquina virtual personalizada]: virtual-machines-create-custom.md

<!---HONumber=AcomDC_1203_2015-->