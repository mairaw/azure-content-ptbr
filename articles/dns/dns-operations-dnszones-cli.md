<properties 
   pageTitle="Operações em zonas DNS | Microsoft Azure" 
   description="Você pode gerenciar zonas DNS usando cmdlets do Powershell do Azure. Como atualizar, excluir e criar zonas DNS no DNS do Azure" 
   services="dns" 
   documentationCenter="na" 
   authors="joaoma" 
   manager="Adinah" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="07/28/2015"
   ms.author="joaoma"/>

# Como gerenciar as zonas DNS

Este guia mostrará como gerenciar sua zona DNS. Ele ajudará a entender a sequência de operações a serem realizadas para administrar sua zona DNS.

## Criar uma nova zona DNS

Para criar uma nova zona DNS para hospedar seu domínio, use o cmdlet New-AzureDnsZone:

		PS C:> $zone = New-AzureDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [–Tag $tags] 

A operação cria uma nova zona DNS no DNS do Azure e retorna um objeto local correspondente a essa zona. Opcionalmente, você pode especificar uma matriz de marcas do Gerenciador de Recursos do Azure. Para obter mais informações, consulte [Etags e marcas](../dns-getstarted-create-dnszone#Etags-and-tags).

O nome da zona deve ser exclusivo dentro do grupo de recursos e a zona não deve existir, caso contrário, a operação falhará.

O mesmo nome de zona pode ser reutilizado em outro grupo de recursos ou uma assinatura do Azure diferente. Quando várias zonas compartilharem o mesmo nome, a cada instância serão atribuídos endereços de servidor de nome diferentes, e somente uma instância pode ser delegada do domínio pai. Consulte [Delegar um domínio ao DNS do Azure](../dns-domain-delegation) para obter mais informações.

## Obter uma zona DNS

Para recuperar uma zona DNS, use o cmdlet Get-AzureDnsZone:

		PS C:> $zone = Get-AzureDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

A operação retornará um objeto de zona DNS correspondente a uma zona existente no DNS do Azure. Esse objeto contém dados sobre a zona (como o número de conjuntos de registros), mas não contém os conjuntos de registro em si.

## Listar as zonas DNS
Omitindo o nome da zona de Get-AzureDnsZone, você pode enumerar todas as zonas em um grupo de recursos:

	PS C:> $zoneList = Get-AzureDnsZone -ResourceGroupName MyAzureResourceGroup
Essa operação retorna uma matriz de objetos de zona.

## Atualizar uma zona DNS
As alterações a um recurso de zona DNS podem ser feitas usando Set-AzureDnsZone. Isso não atualiza nenhum dos conjuntos de registros DNS dentro da zona (consulte [Como gerenciar registros DNS](../dns-operations-recordsets)). Ele só é usado para atualizar as propriedades do recurso da zona em si. Isso é atualmente limitado às “marcas” do Gerenciador de Recursos do Azure para o recurso de zona. Consulte [Etags e marcas](../dns-getstarted-create-dnszone#Etags-and-tags) para obter mais informações.

Use um dos dois métodos a seguir para atualizar a zona DNS:

### Opção 1
 
Especifique a zona usando o nome da zona e o grupo de recursos.

	PS C:> Set-AzureDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### Opção 2
Especifique a zona usando um objeto de $zone de Get-AzureDnsZone:

	PS C:> $zone = Get-AzureDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
	PS C:> <..modify $zone.Tags here...>
	PS C:> Set-AzureDnsZone -Zone $zone [-Overwrite]

Ao usar Set-AzureDnsZone com um objeto $zone, verificações de “Etag” serão usadas para garantir que as alterações simultâneas não sejam substituídas. Você pode usar a opção “-Substituir” para suprimir essas verificações. Consulte [Etags e marcas](../dns-getstarted-create-dnszone#Etags-and-tags) para obter mais informações.

## Excluir uma zona DNS

As zonas DNS podem ser excluídas usando o cmdlet Remove-AzureDnsZone.
 
Antes de excluir uma zona DNS no DNS do Azure, você precisará excluir todos os conjuntos de registros, exceto os registros NS e SOA na raiz da zona, que foram criados automaticamente quando a zona foi criada.

Use um dos dois métodos a seguir para remover uma zona DNS:

### Opção 1

Especifique a zona usando o nome da zona e o nome do grupo de recursos:

	PS C:> Remove-AzureDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force] 

Esta operação tem um comutador opcional “-Forçar” que suprime o prompt para confirmar que deseja remover a zona DNS.
### Opção 2

Especifique a zona usando um objeto de $zone de Get-AzureDnsZone:

	PS C:> $zone = Get-AzureDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
	PS C:> Remove-AzureDnsZone -Zone $zone [-Force] [-Overwrite]

O comutador “-Forçar” é o mesmo da Opção 1.

Assim como com “Set-AzureDnsZone”, especificar a zona usando um objeto $zone habilita as verificações de “etag” para garantir que as alterações simultâneas não sejam excluídas. <BR> O sinalizador opcional '-Substituir ' suprime essas verificações. Consulte [Etags e marcas](../dns-getstarted-create-dnszone#Etags-and-tags) para obter mais informações.

O objeto de zona também pode ser redirecionado em vez de ser passado como um parâmetro:

	PS C:> Get-AzureDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureDnsZone [-Force] [-Overwrite]

## Próximas etapas


[Gerenciar registros DNS](../dns-operations-recordsets)

[Automatizar operações usando o SDK do .NET](../dns-sdk)

<!---HONumber=July15_HO5-->