<properties linkid="manage-windows-common-task-mongodb-vm" urlDisplayName="Install MongoDB" pageTitle="Install MongoDB on a Windows Server virtual machine" metaKeywords="Azure vm, Azure MongoDB, Azure remote desktop" description="Learn how to install MongoDB on an Azure VM running Windows Server." metaCanonical="" services="virtual-machines" documentationCenter="" title="Install MongoDB on a virtual machine running Windows Server in Azure" authors="kathydav" solutions="" manager="dongill" editor="tysonn" />

<tags ms.service="virtual-machines" ms.workload="infrastructure-services" ms.tgt_pltfrm="vm-windows" ms.devlang="na" ms.topic="article" ms.date="01/01/1900" ms.author="kathydav"></tags>

# Instalar o MongoDB em uma máquina virtual na qual o Windows Server está em execução

[O MongoDB][O MongoDB] é um banco de dados NoSQL de código-fonte aberto e com alto desempenho. Usando o [Portal de Gerenciamento Azure][Portal de Gerenciamento Azure], você pode criar uma máquina virtual que executa o Windows Server da galeria de imagens. Em seguida, você pode instalar e configurar um banco de dados MongoDB na máquina virtual.

Este artigo aborda como:

-   Usar o Portal de Gerenciamento para criar uma máquina virtual do Windows Server por meio da galeria
-   Conectar-se à máquina virtual usando a Área de trabalho Remota
-   Anexar um disco de dados à máquina virtual
-   Instalar o MongoDB na máquina virtual

## Criar uma máquina virtual na qual o Windows Server está em execução

Seguindo as instruções gerais, você pode modificá-las criando um ponto de extremidade para permitir que o MongoDB seja acessado remotamente. (Também é possível criá-lo depois, descrito após as instruções de instalação do MongoDB.) Na última página do assistente, adicione um ponto de extremidade e o configure assim:

-   Chamá-lo de **Mongo**
-   Usar **TCP** como protocolo
-   Defina as portas públicas e privadas como **27017**.

[WACOM.INCLUDE [virtual-machines-create-WindowsVM][virtual-machines-create-WindowsVM]]

## Anexar um disco de dados

Para fornecer um armazenamento para a máquina virtual, anexe um disco de dados e inicialize-o de forma que o Windows possa usá-lo. É possível anexar um disco existente caso você já tenha dados que deseja usar ou anexar um disco vazio.

[WACOM.INCLUDE [howto-attach-disk-windows-linux][howto-attach-disk-windows-linux]]

Para obter instruções sobre como inicializar o disco, consulte “Como: Inicializar um novo disco de dados no Windows Server" em [Como anexar um disco de dados a uma Máquina Virtual do Windows][Como anexar um disco de dados a uma Máquina Virtual do Windows].

## Instalar e executar o MongoDB na máquina virtual

[WACOM.INCLUDE [install-and-run-mongo-on-win2k8-vm][install-and-run-mongo-on-win2k8-vm]]

## Resumo

Neste tutorial você aprendeu a criar uma Máquina Virtual do Windows Server e a se conectar remotamente a ela, além de anexar um disco de dados. Você também aprendeu como instalar e configurar o MongoDB na máquina virtual Windows. Para obter mais informações sobre o MongoDB, consulte [Documentação do MongoDB][Documentação do MongoDB].

  [O MongoDB]: http://www.mongodb.org/
  [Portal de Gerenciamento Azure]: http://manage.windowsazure.com
  [virtual-machines-create-WindowsVM]: ../includes/virtual-machines-create-WindowsVM.md
  [howto-attach-disk-windows-linux]: ../includes/howto-attach-disk-windows-linux.md
  [Como anexar um disco de dados a uma Máquina Virtual do Windows]: http://azure.microsoft.com/pt-br/documentation/articles/storage-windows-attach-disk/
  [install-and-run-mongo-on-win2k8-vm]: ../includes/install-and-run-mongo-on-win2k8-vm.md
  [Documentação do MongoDB]: http://www.mongodb.org/display/DOCS/Home