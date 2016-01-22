<properties
	pageTitle="Solução de problemas de falha na alocação do serviço de nuvem | Microsoft Azure"
	description="Solução de problemas de falha de alocação ao implantar Serviços de Nuvem no Azure"
	services="azure-service-management, cloud-services"
	documentationCenter=""
	authors="kenazk"
	manager="drewm"
	editor=""
	tags="top-support-issue"/>

<tags
	ms.service="cloud-services"
	ms.workload="na"
	ms.tgt_pltfrm="ibiza"
	ms.devlang="na"
	ms.topic="article"
	ms.date="11/04/2015"
	ms.author="kenazk"/>



# Solução de problemas de falha de alocação ao implantar Serviços de Nuvem no Azure

## Resumo
Quando você implanta instâncias em um Serviço de Nuvem ou adiciona nova instâncias de função da Web ou de trabalho, o Microsoft Azure aloca recursos de computação. Eventualmente, você pode receber mensagens de erro durante a execução dessas operações antes de alcançar os limites da assinatura do Microsoft Azure. Este artigo explica as causas das falhas de alocação mais comuns e sugere possíveis correções. As informações também poderão ser úteis caso você pretenda implantar serviços.

Se precisar de mais ajuda em qualquer momento neste artigo, você pode entrar em contato com os especialistas do Azure nos [fóruns do Azure e do Stack Overflow no MSDN](http://azure.microsoft.com/support/forums/). Como alternativa, você também pode registrar um incidente de suporte do Azure. Acesse o [site de suporte do Microsoft Azure](http://azure.microsoft.com/support/options/) e clique em **Obter Suporte**.

### Histórico – Como funciona a alocação
Os servidores são particionados em clusters nos datacenters do Microsoft Azure. Ocorre uma nova tentativa de solicitação de alocação de serviço de nuvem em vários clusters. Quando a primeira instância é implantada em um serviço de nuvem (em preparo ou produção), esse serviço de nuvem é fixado a um cluster. Quaisquer implantações adicionais do serviço de nuvem ocorrerão no mesmo cluster. Neste artigo, chamaremos isso de "fixada a um cluster". O diagrama 1 a seguir exemplifica o caso de uma tentativa de alocação normal feita em vários clusters; o diagrama 2 demonstra o caso de uma alocação que foi fixada ao Cluster 2 por se tratar do local de hospedagem do Serviço de Nuvem CS\_1.

![Diagrama de alocação](./media/cloud-services-allocation-failure/Allocation1.png)

### Motivos das falhas de alocação
Quando a solicitação de alocação é fixada a um cluster, é mais provável que haja falha na localização de recursos livres, pois o pool de recursos disponíveis está limitado a um cluster. Além disso, quando a solicitação de alocação é fixada a um cluster, mas não há suporte para o tipo de recurso solicitado ao cluster, a solicitação falha mesmo que o cluster tenha recursos gratuitos. O diagrama 3 a seguir demonstra o caso em que uma alocação fixada falha porque o único cluster candidato não dispõe de recursos gratuitos. O diagrama 4 demonstra o caso em que uma alocação fixada falha porque o único cluster candidato não é compatível com o tamanho da VM solicitado, mesmo que o cluster disponha de recursos livres.

![Falha na alocação fixada](./media/cloud-services-allocation-failure/Allocation2.png)

## Solucionando problemas de falha de alocação para serviços de nuvem
### Mensagem de erro
Você pode ver a seguinte mensagem de erro:

	"Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### Problemas comuns
Estes são os cenários comuns de alocação que causam a fixação de uma solicitação de alocação em um único cluster.

- Implantação em um Slot de Preparo: se um serviço de nuvem tiver uma implantação em cada slot, todo o serviço de nuvem será fixado em um cluster específico. Isso significa que se uma implantação já existir no slot de produção, uma nova implantação de preparo só pode ser alocada no mesmo cluster, como o slot de produção. Se o cluster estiver se aproximando da capacidade, a solicitação poderá falhar. 
 
- Dimensionamento: a adição de novas instâncias a um serviço de nuvem existente deve alocar no mesmo cluster. Solicitações pequenas de dimensionamento normalmente podem ser alocadas, mas nem sempre. Se o cluster estiver se aproximando da capacidade, a solicitação poderá falhar.
	
- Grupo de afinidade: uma nova implantação para um serviço de nuvem vazio pode ser alocada pela malha em qualquer cluster nessa região, a menos que o serviço de nuvem esteja fixado em um grupo de afinidade. As implantações no mesmo grupo de afinidade serão tentadas no mesmo cluster. Se o cluster estiver se aproximando da capacidade, a solicitação poderá falhar.
	
- Grupo de Afinidade vNet: redes virtuais mais antigas foram vinculadas a grupos de afinidade em vez de regiões, e os serviços de nuvem nessas Redes Virtuais poderiam ser fixados ao cluster do grupo de afinidade. As implantações para esse tipo de rede virtual serão tentadas no cluster fixado. Se o cluster estiver se aproximando da capacidade, a solicitação poderá falhar.

## Soluções

1. Reimplantar em um novo serviço de nuvem: essa solução provavelmente será a mais bem-sucedida, pois permite que a plataforma escolha entre todos os clusters nessa região.
	
	- Implantar a carga de trabalho em um novo serviço de nuvem  
	
	- Atualizar o CNAME ou um registro A para apontar o tráfego para o novo serviço de nuvem
		
	- Quando nenhum tráfego estiver sendo encaminhado ao local antigo, você poderá excluir o serviço de nuvem antigo. Essa solução não causará qualquer tempo de inatividade.

2. Excluir os slots de produção e de preparo: essa solução preservará seu nome DNS existente, mas causará um tempo de inatividade para seu aplicativo.
	
	- Exclua os slots de produção e de preparo de um serviço de nuvem existente para que o serviço de nuvem fique vazio e, em seguida, 
	
	- Crie uma nova implantação no serviço de nuvem existente. Isso causará uma nova tentativa de alocação em todos os clusters na região. Certifique-se de que o serviço de nuvem não esteja associado a um grupo de afinidade.

3. IP reservado: essa solução preservará o endereço IP existente, mas causará um tempo de inatividade para seu aplicativo.
	
	- Criar um IP reservado para sua implantação existente usando o Powershell 

	```
	New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
	```
		
	- Siga o nº 2 acima, lembrando de especificar o novo IP reservado no CSCFG do serviço.

4. Remover o grupo de afinidade para novas implantações: grupos de afinidade não são mais recomendados. Execute as etapas para o nº 1 acima a fim de implantar um novo serviço de nuvem. Certifique-se de que o serviço de nuvem não esteja em um grupo de afinidade.

5. Converter para uma rede virtual regional: confira [Como migrar de grupos de afinidades para uma rede virtual regional (VNet)](https://azure.microsoft.com/documentation/articles/virtual-networks-migrate-to-regional-vnet/).

## Recursos adicionais
### Contate o Suporte ao Cliente do Azure

Caso este artigo não o ajude a resolver seu problema com o Azure, navegue pelos fóruns do Azure em [MSDN e Excedente de Pilha](http://azure.microsoft.com/support/forums/). Você pode também registrar uma ocorrência de suporte do Microsoft Azure sobre o problema. Acesse o site [Suporte do Azure](http://azure.microsoft.com/support/options/) e clique em Obter Suporte. Para saber mais sobre como usar o Suporte do Azure, leia as [ Perguntas frequentes de suporte do Microsoft Azure](http://azure.microsoft.com/support/faq/).

<!---HONumber=Nov15_HO4-->