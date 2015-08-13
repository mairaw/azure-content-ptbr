<properties
   pageTitle="Como marcar uma Máquina Virtual no Azure"
   description="Saiba mais sobre a marcação de uma Máquina Virtual no Azure"
   services="virtual-machines"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure-services"
   ms.date="07/23/2015"
   ms.author="dkshir;memccror"/>

# Como marcar uma máquina virtual no Azure

Este artigo descreve as diferentes maneiras de marcar uma máquina virtual no Azure. As marcas são pares de chave/valor definidos pelo usuário que podem ser colocados diretamente em um recurso ou grupo de recursos. Atualmente, o Azure oferece suporte a até 15 marcas por recurso e grupo de recursos. As marcas podem ser colocadas em um recurso no momento da criação ou adicionadas a um recurso existente.

## Marcando uma máquina virtual por meio de modelos

Primeiramente, vamos observar uma marcação por meio de modelos. [Este modelo](https://github.com/Azure/azure-quickstart-templates/tree/master/101-tags-vm) coloca marcas nos seguintes recursos: Computação (Máquina Virtual), Armazenamento (Conta de Armazenamento) e Rede (Endereço IP Público, Rede Virtual e Interface de Rede).

Clique no botão **Implantar no Azure** no [link do modelo](https://github.com/Azure/azure-quickstart-templates/tree/master/101-tags-vm). Isso o levará ao [Portal de Visualização do Azure](http://portal.azure.com/), onde é possível implantar esse modelo.

![Implantação simples com marcas](./media/virtual-machines-tagging-arm/deploy-to-azure-tags.png)

Esse modelo inclui as seguintes marcas: *Department*, *Application* e *Created By*. Você pode adicionar/editar essas marcas diretamente no modelo se desejar diferentes nomes de marca.

![Marcas do Azure em um modelo](./media/virtual-machines-tagging-arm/azure-tags-in-a-template.png)

Como pode ser visto, as marcas são pares de chave/valor definidos, separados por dois-pontos (:). As marcas devem ser definidas neste formato:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Salve o arquivo de modelo quando terminar de editá-lo com as marcas de sua escolha.

Em seguida, na seção **Editar Parâmetros**, você pode preencher os valores de suas marcas.

![Editar marcas no Portal do Azure](./media/virtual-machines-tagging-arm/edit-tags-in-azure-portal.png)

Clique em **Criar** para implantar esse modelo com seus valores de marca.


## Marcando por meio do portal

Depois de criar seus recursos com marcas, você pode exibir, adicionar e excluir as marcas no portal.

Selecione o ícone de marcas para exibir suas marcas:

![Ícone de marcas no Portal do Azure](./media/virtual-machines-tagging-arm/azure-portal-tags-icon.png)

Adicione uma nova marca por meio do portal definindo seu próprio par de chave/valor e salve-a.

![Adicionar nova marca no Portal do Azure](./media/virtual-machines-tagging-arm/azure-portal-add-new-tag.png)

Agora, a nova marca deve aparecer na lista de marcas do seu recurso.

![Nova marca salva no Portal do Azure](./media/virtual-machines-tagging-arm/azure-portal-saved-new-tag.png)


## Marcação com o PowerShell

Para criar, adicionar e excluir marcas pelo PowerShell, primeiramente, você precisa configurar o [ambiente do PowerShell com o Gerenciador de Recursos do Azure][]. Depois de concluir a instalação, você pode colocar marcas em recursos de Computação, Rede e Armazenamento na criação ou depois que o recurso for criado via PowerShell. Este artigo se concentra na exibição/edição de marcas colocadas nas Máquinas Virtuais.

Em primeiro lugar, navegue para uma Máquina Virtual usando o cmdlet `Get-AzureVM`.

        PS C:> Get-AzureVM -ResourceGroupName "MyResourceGroup" -Name "MyWindowsVM"

Se sua Máquina Virtual já contiver marcas, você verá todas elas no seu recurso:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Se desejar adicionar marcas por meio do PowerShell, você poderá usar o comando `Set-AzureResource`. Observe que, ao atualizar marcas pelo PowerShell, as marcas são atualizadas como um todo. Desse modo, se estiver adicionando uma marca a um recurso que já tenha marcas, você precisará incluir todas as marcas que deseja que sejam colocadas no recurso. Veja abaixo um exemplo de como adicionar mais marcas a um recurso usando cmdlets do PowerShell.

Este primeiro cmdlet define todas as marcas colocadas em *MyWindowsVM* para a variável *tags* usando as funções `Get-AzureResource` e `Tags`.

        PS C:> $tags = (Get-AzureResource -Name MyWindowsVM -ResourceGroupName MyResourceGroup -ResourceType "Microsoft.Compute/virtualmachines" -ApiVersion 2015-05-01-preview).Tags

O segundo comando exibe as marcas para a variável fornecida.

        PS C:> $tags

        Name		Value
        ----                           -----
        Value		MyDepartment
        Name		Department
        Value		MyApp1
        Name		Application
        Value		MyName
        Name		Created By
        Value		Production
        Name		Environment

O terceiro comando adiciona uma marca extra à variável *tags*. Observe o uso de **+=** para acrescentar o novo par de chave/valor à lista *tags*.

        PS C:> $tags +=@{Name="Location";Value="MyLocation"}

O quarto comando define todas as marcas definidas na variável *tags* para o recurso fornecido. Nesse caso, é MyWindowsVM.

        PS C:> Set-AzureResource -Name MyWindowsVM -ResourceGroupName MyResourceGroup -ResourceType "Microsoft.Compute/VirtualMachines" -ApiVersion 2015-05-01-preview -Tag $tags

O quinto comando exibe todas as marcas no recurso. Como pode ser visto, *Location* agora está definido como uma marca com *MyLocation* como o valor.

        PS C:> (Get-AzureResource -ResourceName "MyWindowsVM" -ResourceGroupName "MyResourceGroup" -ResourceType "Microsoft.Compute/VirtualMachines" -ApiVersion 2015-05-01-preview).Tags

        Name		Value
        ----                           -----
        Value		MyDepartment
        Name		Department
        Value		MyApp1
        Name		Application
        Value		MyName
        Name		Created By
        Value		Production
        Name		Environment
        Value		MyLocation
        Name		Location

Para saber mais sobre como marcar usando o PowerShell, confira [Cmdlets de recurso do Azure][].


## Marcando com a CLI do Azure

A marcação também tem suporte de recursos que já foram criados por meio da CLI do Azure. Para começar, configure seu [ambiente CLI do Azure][]. Faça logon na sua assinatura usando a CLI do Azure e alterne para o modo ARM. É possível exibir todas as propriedades de uma determinada Máquina Virtual, incluindo as marcas, usando este comando:

        azure vm show -g MyResourceGroup -n MyVM

Diferentemente do PowerShell, se estiver adicionando marcas a um recurso que já contém marcas, você não precisará especificar todas as marcas (antigas e novas) antes de usar o comando `azure vm set`. Em vez disso, esse comando permite acrescentar uma marca ao recurso. Para adicionar uma nova marca VM usando a CLI do Azure, você pode usar o comando `azure vm set` juntamente com o parâmetro de marca **-t**:

        azure vm set -g MyResourceGroup -n MyVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Para remover todas as marcas, é possível usar o parâmetro **–T** no comando `azure vm set`.

        azure vm set – g MyResourceGroup –n MyVM -T


Agora que aplicamos marcas aos nossos recursos por meio do PowerShell, da CLI do Azure e do Portal, vamos examinar os detalhes de uso para ver as marcas no portal de cobrança.


## Exibindo suas marcas nos detalhes de uso

As marcas colocadas nos recursos de Computação, Rede e Armazenamento por meio do Gerenciador de Recursos do Azure serão populadas nos detalhes de uso no portal de cobrança.

Clique em **Baixar detalhes de uso** para exibir os detalhes de uso na sua assinatura.

![Detalhes de uso no Portal do Azure](./media/virtual-machines-tagging-arm/azure-portal-tags-usage-details.png)

Selecione o extrato de cobrança e os detalhes de uso da **Versão 2**:

![Detalhes de uso da Versão 2 Preview no Portal do Azure](./media/virtual-machines-tagging-arm/azure-portal-version2-usage-details.png)

Nos detalhes de uso, você pode ver todas as marcas na coluna **Marcas**:

![Coluna Marcas no Portal do Azure](./media/virtual-machines-tagging-arm/azure-portal-tags-column.png)

Ao analisar essas marcas juntamente com o uso, as organizações poderão adquirir novas percepções sobre os respectivos dados de consumo.


## Recursos adicionais

* [Visão Geral do Gerenciador de Recursos do Azure][]
* [Usando marcas para organizar os recursos do Azure][]
* [Noções básicas de sua fatura do Azure][]
* [Obtenha informações sobre o consumo de recursos do Microsoft Azure][]




[ambiente do PowerShell com o Gerenciador de Recursos do Azure]: ../powershell-azure-resource-manager.md
[Cmdlets de recurso do Azure]: https://msdn.microsoft.com/pt-br/library/azure/dn757692.aspx
[ambiente CLI do Azure]: ./xplat-cli-azure-resource-manager.md
[Visão Geral do Gerenciador de Recursos do Azure]: ../resource-group-overview.md
[Usando marcas para organizar os recursos do Azure]: ../resource-group-using-tags.md
[Noções básicas de sua fatura do Azure]: ../billing-understand-your-bill.md
[Obtenha informações sobre o consumo de recursos do Microsoft Azure]: ../billing-usage-rate-card-overview.md

<!---HONumber=July15_HO5-->