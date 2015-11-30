<properties 
	pageTitle="Criar e carregar um VHD do RedHat Linux no Azure" 
	description="Saiba como criar e carregar um disco rígido virtual (VHD) do Azure que contém um sistema operacional RedHat Linux." 
	services="virtual-machines" 
	documentationCenter="" 
	authors="SuperScottz" 
	manager="timlt" 
	editor="tysonn"/>

<tags 
	ms.service="virtual-machines" 
	ms.workload="infrastructure-services" 
	ms.tgt_pltfrm="vm-linux" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="10/28/2015" 
	ms.author="mingzhan"/>


# Preparar uma Máquina Virtual baseada no RedHat para o Azure
Neste artigo, você aprenderá como preparar uma Máquina Virtual do Red Hat Enterprise Linux (RHEL) para usar no Azure. As versões do RHEL abordadas neste artigo são 6.6, 6.7, 7.0 e 7.1 e os hipervisores de preparação abordados neste artigo são o Hyper-V, KVM e VMWare.




##Preparar uma imagem a partir do Gerenciador do Hyper-V 
###Pré-requisitos
Esta seção pressupõe que você já instalou uma imagem RHEL a partir de um arquivo ISO obtido no site RedHats em um disco rígido virtual (VHD). Para obter mais detalhes sobre como usar o gerenciador do Hyper-V para instalar uma imagem do sistema operacional, confira [Instalar a Função Hyper-V e Configurar uma Máquina Virtual](http://technet.microsoft.com/library/hh846766.aspx).

**Notas de Instalação do RHEL**

- Não há suporte para o formato VHDX mais recente no Azure. Você pode converter o disco no formato VHD usando o Gerenciador do Hyper-V ou o cmdlet convert-vhd powershell.

- Ao instalar o sistema Linux, é recomendável que você use partições padrão em vez de LVM (geralmente o padrão para muitas instalações). Isso irá evitar conflitos de nome LVM com VMs clonadas, especialmente se um disco do sistema operacional precisar ser anexado a outra VM para solução de problemas. Se você preferir, é possível usar LVM ou RAID em discos de dados.

- Não configure uma partição de permuta no disco do SO. O agente Linux pode ser configurado para criar um arquivo de permuta no disco de recursos temporários. Verifique as etapas a seguir para obter mais informações a esse respeito.

- Todos os VHDs devem ter tamanhos que sejam múltiplos de 1 MB.

###RHEL 6.6/6.7

1.	No Gerenciador do Hyper-V, selecione a máquina virtual.

2.	Clique em **Conectar** para abrir a janela do console para a máquina virtual.

3.	Desinstale o NetworkManager executando o seguinte comando:

        # sudo rpm -e --nodeps NetworkManager

    **Observação:** se o pacote ainda não estiver instalado, esse comando irá falhar com uma mensagem de erro. Isso é esperado.

4.	Crie um arquivo chamado **rede** no diretório `/etc/sysconfig/` que contém o seguinte texto:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.	Crie um arquivo chamado **ifcfg-eth0** no diretório `/etc/sysconfig/network-scripts/` que contém o seguinte texto:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.	Mova (ou remova) as regras de udev para evitar a geração de regras estáticas da interface Ethernet. Essas regras provocam problemas ao clonar uma máquina virtual no Microsoft Azure ou no Hyper-V:
            
        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.	Certifique-se de que o serviço de rede será iniciado na inicialização executando o seguinte comando:

        # sudo chkconfig network on

8.	Registre a assinatura do Red Hat para habilitar a instalação de pacotes do repositório RHEL:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9.	Habilite o repositório epel, uma vez que o pacote WALinuxAgent `WALinuxAgent-<version>` foi enviado para o repositório Fedora EPEL 6:

        # wget http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
        # rpm -ivh epel-release-6-8.noarch.rpm

10.	Modifique a linha de inicialização do kernel em sua configuração de grub para incluir parâmetros adicionais de kernel para o Azure. Para fazer isso, abra `/boot/grub/menu.lst` em um editor de texto e verifique se o kernel padrão inclui os seguintes parâmetros:

        console=ttyS0 
        earlyprintk=ttyS0 
        rootdelay=300 
        numa=off

    Isso também garantirá que todas as mensagens do console sejam enviadas para a primeira porta serial, que pode auxiliar o suporte do Azure com problemas de depuração. Essa ação desabilitará o NUMA devido a um erro na versão do kernel usada pelo RHEL 6.

    Além disso, recomendamos que você remova os seguintes parâmetros:

        rhgb quiet crashkernel=auto

    As inicializações gráfica e silenciosa não são úteis em ambientes de rede, quando queremos que todos os logs sejam enviados para a porta serial.

    Você pode configurar a opção crashkernel, mas note que esse parâmetro reduzirá a memória disponível na VM em 128 MB ou mais, o que pode ser um problema nas VMs menores.

11.	Confira se o servidor SSH está instalado e configurado para iniciar no tempo de inicialização. Geralmente, esse é o padrão. Modifique /etc/ssh/sshd\_config para incluir a seguinte linha:

        ClientAliveInterval 180

12.	Instale o Agente Linux do Azure executando o seguinte comando:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

    **Observação:** a instalação do pacote WALinuxAgent removerá os pacotes NetworkManager e NetworkManager-gnome se eles não foram removidos conforme descrito na etapa 2.

13.	Não crie um espaço de permuta no disco do sistema operacional. O Agente Linux do Azure pode configurar automaticamente o espaço de permuta usando o disco de recurso local anexado à VM após o provisionamento no Azure. Observe que o disco de recurso local é um disco temporário e pode ser esvaziado quando a VM é desprovisionada. Depois de instalar o Agente Linux do Azure (consulte a etapa anterior), modifique os seguintes parâmetros em /etc/waagent.conf de maneira apropriada:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14.	Cancele o registro da assinatura (se necessário) executando o seguinte comando:

        # sudo subscription-manager unregister

15.	Execute os comandos a seguir para desprovisionar a máquina virtual e prepará-la para provisionamento no Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16.	Clique em **Ação -> Desligar** no Gerenciador do Hyper-V. Agora, seu VHD Linux está pronto para ser carregado no Azure.
###RHEL 7.0/7.1

1.	No Gerenciador do Hyper-V, selecione a máquina virtual.

2.	Clique em Conectar para abrir a janela do console para a máquina virtual.

3.	Crie um arquivo chamado **rede** no diretório `/etc/sysconfig/` que contém o seguinte texto:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.	Crie um arquivo chamado **ifcfg-eth0** no diretório `/etc/sysconfig/network-scripts/` que contém o seguinte texto:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

5.	Certifique-se de que o serviço de rede será iniciado na inicialização executando o seguinte comando:

        # sudo chkconfig network on

6.	Registre a assinatura do Red Hat para habilitar a instalação de pacotes do repositório RHEL:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.	Modifique a linha de inicialização do kernel em sua configuração de grub para incluir parâmetros adicionais de kernel para o Azure. Para fazer isso, abra `/etc/default/grub` em um editor de texto e edite o parâmetro **GRUB\_CMDLINE\_LINUX**, por exemplo:

        GRUB_CMDLINE_LINUX="rootdelay=300 
        console=ttyS0 
        earlyprintk=ttyS0"

    Isso também garantirá que todas as mensagens do console sejam enviadas para a primeira porta serial, que pode auxiliar o suporte do Azure com problemas de depuração. Além disso, recomendamos que você remova os seguintes parâmetros:

        rhgb quiet crashkernel=auto
    As inicializações gráfica e silenciosa não são úteis em ambientes de rede, quando queremos que todos os logs sejam enviados para a porta serial. Você pode configurar a opção crashkernel, mas note que esse parâmetro reduzirá a memória disponível na VM em 128 MB ou mais, o que pode ser um problema nas VMs menores.

8.	Depois de editar `/etc/default/grub` como mostrado acima, execute o comando a seguir para recompilar a configuração do grub:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9.	Confira se o servidor SSH está instalado e configurado para iniciar no tempo de inicialização. Geralmente, esse é o padrão. Modifique o `/etc/ssh/sshd_config` para incluir a seguinte linha:

        ClientAliveInterval 180

10.	Habilite o repositório epel, uma vez que o pacote WALinuxAgent `WALinuxAgent-<version>` foi enviado para o repositório Fedora EPEL 7.

        # wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
        # rpm -ivh epel-release-7-5.noarch.rpm

11.	Instale o Agente Linux do Azure executando o seguinte comando:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service 

12.	Não crie espaço de permuta no disco do SO. O Agente Linux do Azure pode configurar automaticamente o espaço de permuta usando o disco de recurso local que é anexado à VM após o provisionamento no Azure. Observe que o disco de recurso local é um disco temporário e pode ser esvaziado quando a VM é desprovisionada. Depois de instalar o Agente Linux do Azure (consulte a etapa anterior), modifique os seguintes parâmetros em `/etc/waagent.conf` adequadamente:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13.	Se você quiser cancelar o registro da assinatura, execute o seguinte comando:

        # sudo subscription-manager unregister

14.	Execute os comandos a seguir para desprovisionar a máquina virtual e prepará-la para provisionamento no Azure:
        
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15.	Clique em **Ação -> Desligar** no Gerenciador do Hyper-V. Agora, seu VHD Linux está pronto para ser carregado no Azure.


##Preparar uma imagem a partir do KVM 
###RHEL 6.6/6.7

1.	Baixe a imagem KVM do RHEL 6.6/6.7 no site do Red Hat.

2.	Definir uma senha raiz

    Gere a senha criptografada e copie a saída do comando:

        # openssl passwd -1 changeme
    Defina uma senha raiz com guestfish:
       
        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit
    Altere o segundo campo do usuário raiz de “!!” para a senha criptografada.

3.	Crie uma máquina virtual no KVM a partir da imagem qcow2, defina o tipo de disco para **qcow2** e defina o modelo do dispositivo da Interface da Rede Virtual para **virtio**. Em seguida, inicie a máquina virtual e faça logon como raiz.

4.	Crie um arquivo chamado **rede** no diretório `/etc/sysconfig/` que contém o seguinte texto:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.	Crie um arquivo chamado **ifcfg-eth0** no diretório `/etc/sysconfig/network-scripts/` que contém o seguinte texto:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.	Mova (ou remova) as regras de udev para evitar a geração de regras estáticas da interface Ethernet. Essas regras provocam problemas ao clonar uma máquina virtual no Microsoft Azure ou no Hyper-V:

        # mkdir -m 0700 /var/lib/waagent
        # mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.	Certifique-se de que o serviço de rede será iniciado na inicialização executando o seguinte comando:

        # chkconfig network on

8.	Registre a assinatura do Red Hat para habilitar a instalação de pacotes do repositório RHEL:

        # subscription-manager register –auto-attach --username=XXX --password=XXX

9.	Modifique a linha de inicialização do kernel em sua configuração de grub para incluir parâmetros adicionais de kernel para o Azure. Para fazer isso, abra `/boot/grub/menu.lst` em um editor de texto e verifique se o kernel padrão inclui os seguintes parâmetros:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Isso também garantirá que todas as mensagens do console sejam enviadas para a primeira porta serial, que pode auxiliar o suporte do Azure com problemas de depuração. Essa ação desabilitará o NUMA devido a um erro na versão do kernel usada pelo RHEL 6.

    Além disso, recomendamos que você remova os seguintes parâmetros:

        rhgb quiet crashkernel=auto

    As inicializações gráfica e silenciosa não são úteis em ambientes de rede, quando queremos que todos os logs sejam enviados para a porta serial. Você pode configurar a opção crashkernel, mas note que esse parâmetro reduzirá a memória disponível na VM em 128 MB ou mais, o que pode ser um problema nas VMs menores.

10.	Desinstale cloud-init:

        # yum remove cloud-init

11.	Verifique se o servidor SSH está instalado e configurado para começar na hora da inicialização:
 
        # chkconfig sshd on

    Modifique /etc/ssh/sshd\_config para incluir as seguintes linhas:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Reinicie o sshd:

		# service sshd restart

12.	Habilite o repositório epel, uma vez que o pacote WALinuxAgent `WALinuxAgent-<version>` foi enviado para o repositório **Fedora EPEL 6**:

        # wget http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
        # rpm -ivh epel-release-6-8.noarch.rpm

13.	Instale o Agente Linux do Azure executando o seguinte comando:

        # yum install WALinuxAgent
        # chkconfig waagent on

14.	O Agente Linux do Azure pode configurar automaticamente o espaço de permuta usando o disco de recurso local que é anexado à VM após o provisionamento no Azure. Observe que o disco de recurso local é um disco temporário e pode ser esvaziado quando a VM é desprovisionada. Depois de instalar o Agente Linux do Azure (consulte a etapa anterior), modifique os seguintes parâmetros em **/etc/waagent.conf** de maneira apropriada:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15.	Cancele o registro da assinatura (se necessário) executando o seguinte comando:
        
        # subscription-manager unregister

16.	Execute os comandos a seguir para desprovisionar a máquina virtual e prepará-la para provisionamento no Azure:

        # waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17.	Finalize a VM no KVM.

18.	Converta a imagem qcow2 no formato vhd: primeiro converta a imagem no formato bruto:
         
         # qemu-img convert -f qcow2 –O raw rhel-6.6.qcow2 rhel-6.6.raw
    Verifique se o tamanho da imagem bruta está alinhado com 1 MB, caso contrário, arredonde o tamanho para se alinhar com 1 MB:

         # qemu-img resize rhel-6.6.raw $rounded_size

    Converta o disco bruto no vhd de tamanho fixo:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.6.raw rhel-6.6.vhd
 
###RHEL 7.0/7.1

1.	Baixe a imagem KVM do RHEL 7.0 no site do Red Hat.

2.	Definir uma senha raiz

    Gerar a senha criptografada e copiar a saída do comando

        # openssl passwd -1 changeme

    Definir uma senha raiz com guestfish

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Alterar o segundo campo do usuário raiz de “!!” para a senha criptografada

3.	Crie uma máquina virtual no KVM a partir da imagem qcow2, defina o tipo de disco para **qcow2** e defina o modelo do dispositivo da Interface da Rede Virtual para **virtio**. Em seguida, inicie a máquina virtual e faça logon como raiz.

4.	Crie um arquivo chamado **rede** no diretório `/etc/sysconfig/` que contém o seguinte texto:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.	Crie um arquivo chamado **ifcfg-eth0** no diretório `/etc/sysconfig/network-scripts/` que contém o seguinte texto:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.	Certifique-se de que o serviço de rede será iniciado na inicialização executando o seguinte comando:

        # chkconfig network on

7.	Registre a assinatura do Red Hat para habilitar a instalação de pacotes do repositório RHEL:

        # subscription-manager register –auto-attach --username=XXX --password=XXX

8.	Modifique a linha de inicialização do kernel em sua configuração de grub para incluir parâmetros adicionais de kernel para o Azure. Para fazer isso, abra `/etc/default/grub` em um editor de texto e edite o parâmetro **GRUB\_CMDLINE\_LINUX**, por exemplo:

        GRUB_CMDLINE_LINUX="rootdelay=300 
        console=ttyS0 
        earlyprintk=ttyS0"

    Isso também garantirá que todas as mensagens do console sejam enviadas para a primeira porta serial, que pode auxiliar o suporte do Azure com problemas de depuração. Além disso, recomendamos que você remova os seguintes parâmetros:

        rhgb quiet crashkernel=auto

    As inicializações gráfica e silenciosa não são úteis em ambientes de rede, quando queremos que todos os logs sejam enviados para a porta serial. Você pode configurar a opção crashkernel, mas note que esse parâmetro reduzirá a memória disponível na VM em 128 MB ou mais, o que pode ser um problema nas VMs menores.

9.	Depois de editar `/etc/default/grub` como mostrado acima, execute o comando a seguir para recompilar a configuração do grub:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10.	Desinstale cloud-init:

        # yum remove cloud-init

11.	Verifique se o servidor SSH está instalado e configurado para começar na hora da inicialização:

        # systemctl enable sshd

    Modifique /etc/ssh/sshd\_config para incluir as seguintes linhas:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Reinicie o sshd:

        systemctl restart sshd	

12.	Habilite o repositório epel, uma vez que o pacote WALinuxAgent `WALinuxAgent-<version>` foi enviado para o repositório **Fedora EPEL 7**:

        # wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
        # rpm -ivh epel-release-7-5.noarch.rpm

13.	Instale o Agente Linux do Azure executando o seguinte comando:

        # yum install WALinuxAgent

    Habilite o serviço de waagent:

        # systemctl enable waagent.service

14.	Não crie espaço de permuta no disco do SO. O Agente Linux do Azure pode configurar automaticamente o espaço de permuta usando o disco de recurso local que é anexado à VM após o provisionamento no Azure. Observe que o disco de recurso local é um disco temporário e pode ser esvaziado quando a VM é desprovisionada. Depois de instalar o Agente Linux do Azure (consulte a etapa anterior), modifique os seguintes parâmetros em `/etc/waagent.conf` adequadamente:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15.	Cancele o registro da assinatura (se necessário) executando o seguinte comando:

        # subscription-manager unregister

16.	Execute os comandos a seguir para desprovisionar a máquina virtual e prepará-la para provisionamento no Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17.	Finalize a máquina virtual no KVM.

18.	Converta a imagem qcow2 no formato vhd:

    Primeiro, converta a imagem no formato bruto:

         # qemu-img convert -f qcow2 –O raw rhel-7.0.qcow2 rhel-7.0.raw

    Verifique se o tamanho da imagem bruta está alinhado com 1 MB, caso contrário, arredonde o tamanho para se alinhar com 1 MB:

         # qemu-img resize rhel-7.0.raw $rounded_size

    Converta o disco bruto no vhd de tamanho fixo:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.0.raw rhel-7.0.vhd


##Preparar uma imagem a partir do VMWare
###Pré-requisitos
Esta seção pressupõe que você já instalou uma máquina virtual RHEL no VMWare. Para obter detalhes sobre como instalar um sistema operacional no VMWare, consulte o [Guia de Instalação do Sistema Operacional Convidado do VMWare](http://partnerweb.vmware.com/GOSIG/home.html).
 
- Ao instalar o sistema Linux, é recomendável que você use partições padrão em vez de LVM (geralmente o padrão para muitas instalações). Isso irá evitar conflitos de nome LVM com VMs clonadas, especialmente se um disco do sistema operacional precisar ser anexado a outra VM para solução de problemas. Se você preferir, é possível usar LVM ou RAID em discos de dados.

- Não configure uma partição de permuta no disco do SO. O agente Linux pode ser configurado para criar um arquivo de permuta no disco de recursos temporários. Verifique as etapas a seguir para obter mais informações a esse respeito.

- Ao criar o disco rígido virtual, selecione **Armazenar disco virtual como um único arquivo**.

###RHEL 6.6/6.7
1.	Desinstale o NetworkManager executando o seguinte comando:

         # sudo rpm -e --nodeps NetworkManager

    **Observação:** se o pacote ainda não estiver instalado, esse comando irá falhar com uma mensagem de erro. Isso é esperado.

2.	Crie um arquivo chamado **network** no diretório /etc/sysconfig/ que contém o seguinte texto:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3.	Crie um arquivo chamado **ifcfg-eth0** no diretório /etc/sysconfig/network-scripts/ que contém o seguinte texto:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4.	Mova (ou remova) as regras de udev para evitar a geração de regras estáticas da interface Ethernet. Essas regras provocam problemas ao clonar uma máquina virtual no Microsoft Azure ou no Hyper-V:

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

5.	Certifique-se de que o serviço de rede será iniciado na inicialização executando o seguinte comando:

        # sudo chkconfig network on

6.	Registre a assinatura do Red Hat para habilitar a instalação de pacotes do repositório RHEL:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.	Habilite o repositório epel, uma vez que o pacote WALinuxAgent `WALinuxAgent-<version>` foi enviado para o repositório Fedora EPEL 6:

        # wget http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
        # rpm -ivh epel-release-6-8.noarch.rpm

8.	Modifique a linha de inicialização do kernel em sua configuração de grub para incluir parâmetros adicionais de kernel para o Azure. Para fazer isso, abra "/boot/grub/menu.lst" em um editor de texto e verifique se o kernel padrão inclui os seguintes parâmetros:

        console=ttyS0 
        earlyprintk=ttyS0 
        rootdelay=300 
        numa=off

    Isso também garantirá que todas as mensagens do console sejam enviadas para a primeira porta serial, que pode auxiliar o suporte do Azure com problemas de depuração. Essa ação desabilitará o NUMA devido a um erro na versão do kernel usada pelo RHEL 6. Além disso, recomendamos que você remova os seguintes parâmetros:

        rhgb quiet crashkernel=auto

    As inicializações gráfica e silenciosa não são úteis em ambientes de rede, quando queremos que todos os logs sejam enviados para a porta serial. Você pode configurar a opção crashkernel, mas note que esse parâmetro reduzirá a memória disponível na VM em 128 MB ou mais, o que pode ser um problema nas VMs menores.

9.	Confira se o servidor SSH está instalado e configurado para iniciar no tempo de inicialização. Geralmente, esse é o padrão. Modifique o `/etc/ssh/sshd_config` para incluir a seguinte linha:

        ClientAliveInterval 180

10.	Instale o Agente Linux do Azure executando o seguinte comando:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

11.	Não crie um espaço de permuta no disco do sistema operacional:
    
    O Agente Linux do Azure pode configurar automaticamente o espaço de permuta usando o disco de recurso local que é anexado à VM após o provisionamento no Azure. Observe que o disco de recurso local é um disco temporário e pode ser esvaziado quando a VM é desprovisionada. Depois de instalar o Agente Linux do Azure (consulte a etapa anterior), modifique os seguintes parâmetros em `/etc/waagent.conf` adequadamente:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12.	Cancele o registro da assinatura (se necessário) executando o seguinte comando:

        # sudo subscription-manager unregister

13.	Execute os comandos a seguir para desprovisionar a máquina virtual e prepará-la para provisionamento no Azure:

        # sudo waagent -force -deprovision 
        # export HISTSIZE=0
        # logout

14.	Finalize a VM e converta o arquivo VMDK no arquivo VHD.

    Primeiro, converta a imagem no formato bruto:

        # qemu-img convert -f vmdk –O raw rhel-6.6.vmdk rhel-6.6.raw

    Verifique se o tamanho da imagem bruta está alinhado com 1 MB, caso contrário, arredonde o tamanho para se alinhar com 1 MB:

        # qemu-img resize rhel-6.6.raw $rounded_size

    Converta o disco bruto no vhd de tamanho fixo:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.6.raw rhel-6.6.vhd

###RHEL 7.0/7.1

1.	Crie um arquivo chamado **network** no diretório /etc/sysconfig/ que contém o seguinte texto:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2.	Crie um arquivo chamado **ifcfg-eth0** no diretório /etc/sysconfig/network-scripts/ que contém o seguinte texto:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

3.	Certifique-se de que o serviço de rede será iniciado na inicialização executando o seguinte comando:

        # sudo chkconfig network on

4.	Registre a assinatura do Red Hat para habilitar a instalação de pacotes do repositório RHEL:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5.	Modifique a linha de inicialização do kernel em sua configuração de grub para incluir parâmetros adicionais de kernel para o Azure. Para fazer isso, abra `/etc/default/grub` em um editor de texto e edite o parâmetro **GRUB\_CMDLINE\_LINUX**, por exemplo:

        GRUB_CMDLINE_LINUX="rootdelay=300 
        console=ttyS0 
        earlyprintk=ttyS0"

    Isso também garantirá que todas as mensagens do console sejam enviadas para a primeira porta serial, que pode auxiliar o suporte do Azure com problemas de depuração. Além disso, recomendamos que você remova os seguintes parâmetros:

        rhgb quiet crashkernel=auto

    As inicializações gráfica e silenciosa não são úteis em ambientes de rede, quando queremos que todos os logs sejam enviados para a porta serial. Você pode configurar a opção crashkernel, mas note que esse parâmetro reduzirá a memória disponível na VM em 128 MB ou mais, o que pode ser um problema nas VMs menores.

6.	Depois de editar `/etc/default/grub` como mostrado acima, execute o comando a seguir para recompilar a configuração do grub:

         # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7.	Adicione os módulos do Hyper-V em initramfs:

    Edite `/etc/dracut.conf` e adicione o conteúdo:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Recompile o initramfs:

        # dracut –f -v

8.	Confira se o servidor SSH está instalado e configurado para iniciar no tempo de inicialização. Geralmente, esse é o padrão. Modifique o `/etc/ssh/sshd_config` para incluir a seguinte linha:

        ClientAliveInterval 180

9.	Habilite o repositório epel, uma vez que o pacote WALinuxAgent `WALinuxAgent-<version>` foi enviado para o repositório Fedora EPEL 7:


        # wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
        # rpm -ivh epel-release-7-5.noarch.rpm

10.	Instale o Agente Linux do Azure executando o seguinte comando:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

11.	Não crie espaço de permuta no disco do SO. O Agente Linux do Azure pode configurar automaticamente o espaço de permuta usando o disco de recurso local que é anexado à VM após o provisionamento no Azure. Observe que o disco de recurso local é um disco temporário e pode ser esvaziado quando a VM é desprovisionada. Depois de instalar o Agente Linux do Azure (consulte a etapa anterior), modifique os seguintes parâmetros em `/etc/waagent.conf` adequadamente:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12.	Se você quiser cancelar o registro da assinatura, execute o seguinte comando:

        # sudo subscription-manager unregister

13.	Execute os comandos a seguir para desprovisionar a máquina virtual e prepará-la para provisionamento no Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14.	Finalize a VM e converta o arquivo VMDK no formato VHD.

    Primeiro, converta a imagem no formato bruto:

        # qemu-img convert -f vmdk –O raw rhel-7.0.vmdk rhel-7.0.raw

    Verifique se o tamanho da imagem bruta está alinhado com 1 MB, caso contrário, arredonde o tamanho para se alinhar com 1 MB:

        # qemu-img resize rhel-7.0.raw $rounded_size

    Converta o disco bruto no vhd de tamanho fixo:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.0.raw rhel-7.0.vhd


##Preparar a partir de um ISO usando o arquivo de início rápido automaticamente
###RHEL 7.0/7.1

1.	Crie o arquivo de início rápido com o conteúdo abaixo e salve o arquivo. Para obter detalhes sobre a instalação de início rápido, consulte o [Guia de Instalação de Início Rápido](https://access.redhat.com/documentation/pt-BR/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).


        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
        auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff

        # Primary Fedora repo
        repo --name="epel7" --baseurl="http://dl.fedoraproject.org/pub/epel/7/x86_64/"

        %packages
        @base
        @console-internet
        chrony
        sudo
        python-pyasn1
        parted
        ntfsprogs
        WALinuxAgent
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^(ResourceDisk\.EnableSwap)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^(ResourceDisk\.SwapSizeMB)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^(GRUB_CMDLINE_LINUX)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#(ClientAliveInterval).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=yes
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

 

2.	Coloque o arquivo de início rápido em um local acessível no sistema de instalação.
 
3.	No Gerenciador do Hyper-V, crie uma nova VM. Na página **Conectar Disco Rígido Virtual**, selecione **Anexar um disco rígido virtual posteriormente** e conclua o Assistente de Nova Máquina Virtual.

4.	Abra as configurações da VM:

    a. Anexe um novo disco rígido virtual à VM, selecione **Formato VHD** e **Tamanho Fixo**.
    
    b. Anexe o ISO de instalação ISO à unidade de DVD.

    c. Configure o BIOS para inicializar no CD.

5.	Inicie a VM e quando o guia de instalação aparecer, pressione **Tab** para configurar as opções de inicialização.

6.	Insira `inst.ks=<the location of the Kickstart file>` no final das opções de inicialização e pressione **Enter**.

7.	Aguarde a instalação terminar e quando for concluída, a VM desligará automaticamente. Agora, seu VHD Linux está pronto para ser carregado no Azure.

##Problemas conhecidos:
Há dois problemas conhecidos quando você está usando o RHEL 6.6, 7.0 e 7.1 no Hyper-V e no Azure.

###Problema 1: Tempo limite de provisionamento
Esse problema pode ocorrer durante a inicialização com o RHEL no Hyper-V e no Azure. Isso é mais comum com o RHEL 6.6.

Taxa de Reprodução:

O problema é intermitente. Quase sempre é reproduzido nas VMs menores com um único vCPU e com mais frequência nos servidores mais ocupados.


###Problema 2: Congelamento da E/S do Disco 

Esse problema pode ocorrer durante as atividades de E/S do disco de armazenamento frequentes com o RHEL 6.6, 7.0 e 7.1 no Hyper-V e no Azure.

Taxa de Reprodução:

O problema é intermitente, porém ocorre com mais frequência durante as operações de E/S do disco frequentes no Hyper-V e no Azure.

    
[AZURE.NOTE]Esses dois problemas conhecidos já são abordados no Red Hat. Para instalar as correções associadas, você pode executar o comando abaixo:

    # sudo yum update

<!---HONumber=Nov15_HO3-->