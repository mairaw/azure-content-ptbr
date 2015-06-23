<properties 
	pageTitle="Distribuições endossadas do Linux no Azure" 
	description="Saiba mais sobre o Linux nas distribuições endossadas do Azure, incluindo diretrizes para Ubuntu, OpenLogic e SUSE." 
	services="virtual-machines" 
	documentationCenter="" 
	authors="szarkos" 
	manager="timlt" 
	editor="tysonn"/>

<tags 
	ms.service="virtual-machines" 
	ms.workload="infrastructure-services" 
	ms.tgt_pltfrm="vm-linux" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/15/2015" 
	ms.author="szark"/>



#Linux em distribuições endossadas pelo Azure

As imagens Linux na Galeria do Azure são fornecidas por alguns parceiros, e estamos trabalhando com várias comunidades Linux para adicionar ainda mais opções à lista de Distribuições Endossadas. Enquanto isso, para distribuições não disponíveis na Galeria, você sempre pode Trazer o Próprio Linux seguindo as diretrizes [nesta página](virtual-machines-linux-create-upload-vhd.md).


## Distribuições e versões com suporte ##

A tabela a seguir lista as distribuições e versões do Linux com suporte no Azure.

Os drivers LIS (Serviços de Integração do Linux) para Hyper-V e Azure são módulos de kernel que a Microsoft contribui diretamente para o kernel upstream do Linux. Os drivers LIS também são internos ao kernel da distribuição por padrão ou, para distribuições com base em RHEL/CentOS mais antigas estão disponíveis como um download separado [aqui](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409). Confira [este artigo](virtual-machines-linux-create-upload-vhd-generic.md#linux-kernel-requirements) para saber mais sobre os drivers LIS.

O Agente Linux do Azure já vem pré-instalado nas imagens da Galeria do Linux e normalmente está disponível no repositório de pacotes de distribuição. O código-fonte pode ser encontrado no [GitHub](https://github.com/azure/walinuxagent).

<table border="1" width="600">
  <tr bgcolor="#E9E7E7">
		<th>Distribuição</th>		
	    <th>Versão</th>
	    <th>Drivers</th>
		<th>Agente</th>
			</tr>
	<tr>
		<th>  Canonical Ubuntu </th>
		<td> Ubuntu 12.04.1+, 14.04 e 14.10 </td>
		<td>No kernel</td>
		<td>Pacote: no repositório em "walinuxagent" <br />
			Fonte: <a href="http://go.microsoft.com/fwlink/p/?LinkID=250998">GitHub</a></td>
			</tr>
	<tr>
		<th> CentOS by OpenLogic </th>
		<td> CentOS 6.3+</td>
	    <td> CentOS 6.3: <a href="http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409">download do LIS</a><br/>
	        CentOS 6.4+: no Kernel</td>
		<td>Pacote: no repositório do <a href="http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/">OpenLogic</a> em "WALinuxAgent"<br />
			Fonte: <a href="http://go.microsoft.com/fwlink/p/?LinkID=250998">GitHub</a></td>
 		
	</tr>
	<tr>
		<th> <a href="https://coreos.com/docs/running-coreos/cloud-providers/azure/">CoreOS</a> </th>
		<td> 494.4.0+ </td>
        <td> No kernel </td>
		<td> Fonte: <a href="https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent">GitHub</a></td>
		
	</tr>
	<tr>
		<th> Oracle Linux </th>
		<td> 6.4+</td>
        <td>No kernel</td>
		<td>Pacote: no repositório em "WALinuxAgent"<br />
			Fonte: <a href="http://go.microsoft.com/fwlink/p/?LinkID=250998">GitHub</a></td>
		
	</tr>
	<tr>
		<th> SUSE Linux Enterprise </th>
		<td> SLES 11 SP3+</td>
        <td>No kernel</td>
		<td>Pacote: no repositório <a href="https://build.opensuse.org/project/show/Cloud:Tools" >Cloud:Tools</a> em "WALinuxAgent"<br />
			Fonte: <a href="http://go.microsoft.com/fwlink/p/?LinkID=250998">GitHub</a></td>
		
	</tr>
	<tr>
		<th> openSUSE </th>
		<td> openSUSE 13.1+</td>
		<td>No kernel</td>
		<td>Pacote: no repositório <a href="https://build.opensuse.org/project/show/Cloud:Tools" >Cloud:Tools</a> em "WALinuxAgent"<br />
			Código-fonte: <a href="http://go.microsoft.com/fwlink/p/?LinkID=250998">GitHub</a></td>
		
	</tr>
</table>

## Parceiros

### Canonical
[http://www.ubuntu.com/cloud/azure](http://www.ubuntu.com/cloud/azure)

O controle aberto da comunidade e a engenharia da Canonical impulsionam o sucesso do Ubuntu no cliente, no servidor e na computação em nuvem, incluindo serviços de nuvem pessoais para os consumidores. A visão da Canonical de uma plataforma livre unificada no Ubuntu, do telefone à nuvem, com uma família de interfaces coerentes para telefone, tablet, TV e área de trabalho, torna o Ubuntu a primeira opção para diversas instituições que vão desde provedores de nuvem pública até fabricantes de aparelhos eletrônicos e um favorito entre tecnólogos individuais.

Com desenvolvedores e centros de engenharia no mundo inteiro, a Canonical está posicionada exclusivamente para fazer parceria com fabricantes de hardware, provedores de conteúdo e desenvolvedores de software para oferecer soluções de Ubuntu no mercado, de PCs a servidores e dispositivos portáteis.


### CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

No site CoreOS:

*O CoreOS foi projetado para segurança, consistência e confiabilidade. Em vez de instalar os pacotes por meio de yum ou apt, o CoreOS usa contêineres do Linux para gerenciar seus serviços em um nível mais alto de abstração. Um único código de serviço e todas as dependências são empacotados em um contêiner que pode ser executado em uma ou várias máquinas CoreOS.*


### OpenLogic
[http://www.openlogic.com/azure](http://www.openlogic.com/azure)

O OpenLogic é líder no fornecimento de soluções empresariais de software livre para a nuvem e o data center. Ele ajuda centenas de empresas líderes de inúmeros setores a adquirir, dar suporte e controlar com segurança o software livre. O OpenLogic oferece suporte técnico de nível comercial e indenização para 600 pacotes de software livre com suporte da OpenLogic Expert Community, inclusive suporte de nível empresarial para CentOS, bem como o parceiro de lançamento para fornecer imagens baseadas no CentOS no Azure.


### Oracle
[http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

A estratégia da Oracle consiste em oferecer um amplo portfólio de soluções para nuvens públicas e privadas, ao mesmo tempo em que dá aos clientes opção e flexibilidade na maneira como eles implantam um software Oracle em nuvens Oracle, bem como em outras nuvens. A parceria da Oracle com a Microsoft permite aos clientes implantar um software Oracle em nuvens públicas e privadas da Microsoft com a certeza da certificação e do suporte da Oracle. O compromisso e o investimento da Oracle em soluções de nuvem pública e privada Oracle permanecem inalterados.


### SUSE
[http://www.suse.com/suse-linux-enterprise-server-on-azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

O SUSE Linux Enterprise Server no Azure é uma plataforma testada que fornece confiabilidade e segurança superiores para a computação em nuvem. A plataforma Linux versátil do SUSE se integra perfeitamente aos serviços de nuvem do Azure para oferecer um ambiente de nuvem facilmente gerenciável. E, com mais de 9.200 aplicativos certificados de mais de 1.800 fornecedores de software independentes para SUSE Linux Enterprise Server, o SUSE garante que as cargas de trabalho em execução compatíveis no datacenter podem ser implantadas com segurança no Azure.

<!---HONumber=58--> 