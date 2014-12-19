<properties title="Como configurar um conjunto de disponibilidade para m&aacute;quinas virtuais" pageTitle="Como configurar um conjunto de disponibilidade para m&aacute;quinas virtuais" description="Instrui a configurar um conjunto de disponibilidade para m&aacute;quinas virtuais no Azure" metaKeywords="" services="virtual-machines" solutions="" documentationCenter="" authors="kathydav" manager="timlt" videoId="" scriptId="" />

<tags ms.service="virtual-machines" ms.workload="infrastructure-services" ms.tgt_pltfrm="vm-multiple" ms.devlang="na" ms.topic="article" ms.date="09/30/2014" ms.author="kathydav" />

# Como configurar um conjunto de disponibilidade para máquinas virtuais

Os conjuntos de disponibilidade ajudam a manter as máquinas virtuais disponíveis em caso de tempo de inatividade (por exemplo, durante a manutenção). Ter duas ou mais máquinas virtuais com configurações parecidas em um conjunto de disponibilidade cria a redundância necessária para manter a disponibilidade dos aplicativos ou serviços executados em sua máquina virtual. Para obter mais detalhes, confira [Gerenciar a disponibilidade de máquinas virtuais][Gerenciar a disponibilidade de máquinas virtuais].

Você deve usar uma combinação de conjuntos de disponibilidade e pontos de extremidade de balanceamento de carga para ajudar a garantir que seu aplicativo esteja sempre disponível e sendo executado com eficiência. Para obter mais detalhes sobre pontos de extremidade de balanceamento de carga, confira [Balanceamento de carga de máquinas virtuais][Balanceamento de carga de máquinas virtuais].

Você pode escolher uma destas opções para colocar máquinas virtuais em um conjunto de disponibilidade:

-   [Opção 1: Crie uma máquina virtual e um conjunto de disponibilidade ao mesmo tempo][Opção 1: Crie uma máquina virtual e um conjunto de disponibilidade ao mesmo tempo]. Em seguida, adicione novas máquinas virtuais ao conjunto ao criá-las.
-   [Opção 2: Adicione uma máquina virtual já existente a um conjunto de disponibilidade][Opção 2: Adicione uma máquina virtual já existente a um conjunto de disponibilidade].

> [WACOM.NOTE] As máquinas virtuais que você pretende inserir em um mesmo conjunto de disponibilidade devem pertencer ao mesmo serviço de nuvem.

## <span id="createset"></span> </a>Opção 1: Crie uma máquina virtual e um conjunto de disponibilidade ao mesmo tempo

Você pode usar o Portal de Gerenciamento ou cmdlets do PowerShell do Azure para fazer isso.

Para usar o Portal de Gerenciamento:

1.  Se você ainda não fez isso, entre no [Portal de Gerenciamento do Azure][Portal de Gerenciamento do Azure].

2.  Na barra de comandos, clique em **Nova**.

3.  Clique em **Máquina Virtual** e, em seguida, clique em **Da Galeria**.

4.  Nas duas primeiras telas, escolha a imagem, o nome de usuário, a senha etc. Para obter mais detalhes, confira [Criar uma máquina virtual que execute o Windows][Criar uma máquina virtual que execute o Windows].

5.  A terceira tela permite configurar recursos para rede, armazenamento e disponibilidade. Faça o seguinte:

    1.  Escolha a opção de serviço de nuvem adequada. Selecione a opção **Criar um novo serviço de nuvem** (a menos que você esteja adicionando uma nova máquina virtual a um serviço de nuvem de máquina virtual existente). Insira um nome em **Nome DNS de serviço de nuvem**. O nome DNS torna-se parte do URI usado no contato com a máquina virtual. O serviço de nuvem atua como grupo de comunicação e isolamento. Todas as máquinas virtuais do mesmo serviço de nuvem podem comunicar-se entre si. Além disso, é possível configurá-las para balanceamento de carga e colocá-las no mesmo conjunto de disponibilidade.

    2.  Se você pretende usar uma rede virtual, especifique-a em **Região/Grupo de Afinidade/Rede Virtual**. **Importante**: Se quiser que uma máquina virtual use uma rede virtual, você deve associá-las ao criar a máquina virtual. Você não poderá inserir a máquina virtual em uma rede virtual depois de criar a VM. Para obter mais informações, consulte [Visão geral da Rede Virtual do Azure][Visão geral da Rede Virtual do Azure].

    3.  Crie o conjunto de disponibilidade. Em **Conjunto de Disponibilidade**, selecione a opção **Criar um conjunto de disponibilidade**. Em seguida, atribua um nome ao conjunto.
    4.  Crie os pontos de extremidade padrão e adicione mais pontos de extremidade se necessário. Também é possível adicionar os pontos de extremidade em outro momento.

    ![Criar um conjunto de disponibilidade para uma nova máquina virtual][Criar um conjunto de disponibilidade para uma nova máquina virtual]

6.  Na quarta tela, selecione as extensões que você deseja instalar. As extensões oferecem recursos que facilitam o gerenciamento das máquinas virtuais, como a execução de antimalware ou redefinição de senhas. Para obter detalhes, confira [Agente de máquina virtual Linux e extensões de máquina virtual do Azure][Agente de máquina virtual Linux e extensões de máquina virtual do Azure].

7.  Clique na seta para criar a máquina virtual e o grupo de disponibilidade.

    No painel da nova máquina virtual, você pode clicar em **Configurar** e verificar que a máquina virtual pertence ao novo conjunto de disponibilidade.

Para usar os cmdlets do Azure:

1.  Abra uma sessão do PowerShell do Azure e execute os comandos como nos exemplos a seguir. Esses exemplos mostram a criação da máquina virtual, do serviço de nuvem e do conjunto de disponibilidade.

2.  Obtenha o nome da imagem que será usada para criar a máquina virtual e armazene-o em uma variável. O comando usa o número do índice e a propriedade ImageName do objeto de imagem:

    `C:\PS> $image = (Get-AzureVMImage)[4].ImageName`

    > [WACOM.NOTE] Para obter uma lista com todas as imagens adequadas a sua assinatura, execute `Get-AzureVMImage` sem parâmetros.

3.  Especifique a configuração da nova máquina virtual e use o pipeline para transmitir um objeto de configuração ao cmdlet que cria a máquina virtual. Não se esqueça de substituir seus próprios valores por espaços reservados, como \<VmName\> e \<VmSize\>.

    `C:\PS> New-AzureVMConfig -Name "<VmName>" -InstanceSize <VmSize> -AvailabilitySetName "<SetName>" -ImageName $image | Add-AzureProvisioningConfig -Windows -AdminUsername "<UserName>" -Password "<MyPassword>" | Add-AzureDataDisk -CreateNew -DiskSizeInGB 50 -DiskLabel 'datadisk1' -LUN 0 | New-AzureVM –ServiceName "<MySvc1>"`

## <span id="addmachine"></span> </a>Opção 2: Adicione uma máquina virtual já existente a um conjunto de disponibilidade

No Portal de gerenciamento é possível adicionar máquinas virtuais existentes a um conjunto de disponibilidade existente ou criar um novo conjunto para essas máquinas. Lembre-se de que as máquinas virtuais devem pertencer ao mesmo serviço de nuvem. As etapas são praticamente as mesmas. No PowerShell do Azure é possível máquinas virtuais a um conjunto de disponibilidade existente.

1.  Se você ainda não fez isso, entre no [Portal de Gerenciamento][Portal de Gerenciamento do Azure] do Azure.

2.  Na barra de navegação, clique em **Máquinas Virtuais**.

3.  Selecione uma das máquinas virtuais que você deseja adicionar ao conjunto. Clique na linha da máquina virtual para abrir seu painel.

4.  Dentre as guias abaixo do nome da máquina virtual, clique em **Configurar**.

5.  Na seção Configurações, localize a opção **Conjunto de Disponibilidade**. Faça uma das opções a seguir:

    A. Escolha **Criar um conjunto de disponibilidade** e atribua um nome ao conjunto.

    B. Escolha **Selecionar um conjunto de disponibilidade** e selecione um dos conjuntos da lista.

    ![Criar um conjunto de disponibilidade uma máquina virtual existente][Criar um conjunto de disponibilidade uma máquina virtual existente]

6.  Clique em **Salvar**.

Para usar os cmdlets do Azure:

Abra uma sessão do PowerShell do Azure e execute o comando a seguir. Não se esqueça de substituir seus próprios valores por espaços reservados, como \<VmCloudServiceName\> e \<VmName\>.

    C:\PS> Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<MyAvSet>" | Update-AzureVM

> [WACOM.NOTE] A máquina virtual pode ser reiniciada para concluir a adição ao conjunto de disponibilidade.

## Recursos adicionais

[Sobre as definições de configuração da VM do Azure][Sobre as definições de configuração da VM do Azure]

<!-- LINKS -->

  [Gerenciar a disponibilidade de máquinas virtuais]: ../virtual-machines-manage-availability
  [Balanceamento de carga de máquinas virtuais]: ../virtual-machines-load-balance
  [Opção 1: Crie uma máquina virtual e um conjunto de disponibilidade ao mesmo tempo]: #createset
  [Opção 2: Adicione uma máquina virtual já existente a um conjunto de disponibilidade]: #addmachine
  [Portal de Gerenciamento do Azure]: http://manage.windowsazure.com
  [Criar uma máquina virtual que execute o Windows]: ../virtual-machines-windows-tutorial
  [Visão geral da Rede Virtual do Azure]: http://go.microsoft.com/fwlink/p/?linkid=294063
  [Criar um conjunto de disponibilidade para uma nova máquina virtual]: ./media/virtual-machines-how-to-configure-availability/VMavailabilityset.png
  [Agente de máquina virtual Linux e extensões de máquina virtual do Azure]: http://go.microsoft.com/fwlink/p/?LinkId=XXX
  [Criar um conjunto de disponibilidade uma máquina virtual existente]: ./media/virtual-machines-how-to-configure-availability/VMavailabilityExistingVM.png
  [Sobre as definições de configuração da VM do Azure]: http://msdn.microsoft.com/pt-br/library/azure/dn763935.aspx