<properties 
   pageTitle="Introdução à criação de um balanceador de carga para a Internet no modelo de implantação clássico usando a CLI do Azure | Microsoft Azure"
   description="Saiba como criar um balanceador de carga para a Internet no modelo de implantação clássico usando a CLI do Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="carolz"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="11/06/2015"
   ms.author="joaoma" />

# Introdução à criação de um balanceador de carga para a Internet (clássico) na CLI do Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Este artigo aborda o modelo de implantação clássico. Também é possível [Saber como criar um balanceador de carga para a Internet usando o Gerenciador de Recursos do Azure](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## Passo a passo para criação de um balanceador de carga para a Internet usando a CLI

Este guia mostra como criar um balanceador de carga de Internet com base no cenário acima.

1. Se você nunca usou a CLI do Azure, consulte [Instalar e configurar a CLI do Azure](xplat-cli.md) e siga as instruções até o ponto em que você seleciona sua conta e assinatura do Azure.

2. Execute o comando **azure config mode** para alternar para o modo clássico, como mostrado abaixo.

		azure config mode asm

	Saída esperada:

		info:    New mode is asm


## Criar ponto de extremidade e conjunto de balanceadores de carga 

O cenário pressupõe que as máquinas virtuais "web1" e "web2" foram criadas. Este guia criará um conjunto de balanceadores de carga usando a porta 80 como porta pública e a porta 80 como porta local. Uma porta de investigação também é configurada na porta 80 e nomeou o conjunto de balanceadores de carga como "lbset".


### Etapa 1 

Criar o primeiro ponto de extremidade e conjunto de balanceadores de carga usando `azure network vm endpoint create` para a máquina virtual "web1".

	azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset 

Parâmetros usados:

**-k** -porta da máquina virtual local<br> **-o** -protocolo<BR> **-t** -porta de investigação<BR> **-b** -nome do balanceador de carga<BR>
 
## Etapa 2 

Adicione uma segunda máquina virtual "web2" ao conjunto de balanceadores de carga.

	azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## Etapa 3 

Verificar a configuração do balanceador de carga usando `azure vm show`.

	azure vm show web1

A saída será:

	data:    DNSName "contoso.cloudapp.net"
	data:    Location "East US"
	data:    VMName "web1"
	data:    IPAddress "10.0.0.5"
	data:    InstanceStatus "ReadyRole"
	data:    InstanceSize "Standard_D1"
	data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
	6-en.us-127GB.vhd"
	data:    OSDisk hostCaching "ReadWrite"
	data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
	data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
	/vhds/joaomatest-web1-2015-09-25.vhd"
	data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
	r-2012-R2-20150916-en.us-127GB.vhd"
	data:    OSDisk operatingSystem "Windows"
	data:    OSDisk iOType "Standard"
	data:    ReservedIPName ""
	data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
	data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
	data:    VirtualIPAddresses 0 isDnsProgrammed true
	data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
	data:    Network Endpoints 0 localPort 80
	data:    Network Endpoints 0 name "tcp-80-80"
	data:    Network Endpoints 0 port 80
	data:    Network Endpoints 0 loadBalancerProbe port 80
	data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
	data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
	data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
	data:    Network Endpoints 0 protocol "tcp"
	data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
	data:    Network Endpoints 0 enableDirectServerReturn false
	data:    Network Endpoints 1 localPort 5986
	data:    Network Endpoints 1 name "PowerShell"
	data:    Network Endpoints 1 port 5986
	data:    Network Endpoints 1 protocol "tcp"
	data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
	data:    Network Endpoints 1 enableDirectServerReturn false
	data:    Network Endpoints 2 localPort 3389
	data:    Network Endpoints 2 name "Remote Desktop"
	data:    Network Endpoints 2 port 58081
	info:    vm show command OK

## Criar um ponto de extremidade da área de trabalho remota para uma máquina virtual

Você pode criar um ponto de extremidade da área de trabalho remota para encaminhar o tráfego de rede de uma porta pública para uma porta local, para uma máquina virtual específica, usando `azure vm endpoint create`.

	azure vm endpoint create web1 54580 -k 3389 


## Remover máquina virtual do balanceador de carga

Você precisa excluir o ponto de extremidade associado ao conjunto de balanceadores de carga da máquina virtual. Depois que o ponto de extremidade é removido, a máquina virtual não pertence mais ao conjunto de balanceadores de carga.

 Usando o exemplo acima, você pode remover o ponto de extremidade criado para a máquina virtual "web1" do balanceador de carga usando o comando "lbset" `azure vm endpoint delete`.

	azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE]Você pode explorar mais opções para gerenciar pontos de extremidade usando o comando `azure vm endpoint --help`


## Próximas etapas

[Introdução à configuração de um balanceador de carga interno](load-balancer-internal-getstarted.md)

[Configurar um modo de distribuição do balanceador de carga](load-balancer-distribution-mode.md)

[Definir configurações de tempo limite de TCP ocioso para o balanceador de carga](load-balancer-tcp-idle-timeout.md)

 

<!---HONumber=AcomDC_1203_2015-->