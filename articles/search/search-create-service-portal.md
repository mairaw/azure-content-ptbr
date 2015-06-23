<properties 
	pageTitle="Criar um serviço de Pesquisa do Azure no portal" 
	description="Adicionar um serviço de Pesquisa do Azure gratuito ou padrão para uma assinatura existente usando o portal de gerenciamento" 
	services="search" 
	documentationCenter="" 
	authors="HeidiSteen" 
	manager="mblythe" 
	editor=""
    tags="azure-portal"/>

<tags 
	ms.service="search" 
	ms.devlang="rest-api" 
	ms.workload="search" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.date="04/27/2015" 
	ms.author="heidist"/>

# Criar um serviço de Pesquisa do Azure no portal

A Pesquisa do Microsoft Azure é um novo serviço que permite que você incorpore a funcionalidade de pesquisa em aplicativos personalizados. Ele fornece um mecanismo de pesquisa e armazenamento para seus dados de pesquisa, os quais você acessa e gerencia usando o portal, um SDK do .NET ou uma API REST. Os principais recursos incluem consultas de preenchimento automático, correspondência difusa, realce de ocorrências, navegação facetada e suporte a vários idiomas. Para obter mais informações sobre o que a Pesquisa faz, consulte [Apresentando a Pesquisa do Azure](../fundamentals-azure-search-chappell/).

##Adicionar a Pesquisa à sua assinatura  

Como administrador, você pode adicionar a Pesquisa a uma assinatura existente sem custos adicionais ao selecionar o serviço compartilhado, ou por um valor padrão ao optar por recursos dedicados.

1. Entre no [Portal do Azure](https://portal.azure.com).

2. Na barra de navegação rápida, clique em **Novo** | **Dados + armazenamento** | **Pesquisa**.
 
     ![][1]

3. Configure o nome do serviço, camada de preços, grupo de recursos, assinatura e local. Essas configurações são necessárias e não podem ser alteradas após o início do fornecimento do serviço.

     ![][2]

	- O **nome do serviço** deve ser exclusivo, com letras minúsculas, menos de 15 caracteres e sem espaços. Esse nome se torna parte do ponto de extremidade de seu serviço de Pesquisa do Azure. Consulte [Regras de nomenclatura](https://msdn.microsoft.com/library/azure/dn857353.aspx) para saber mais sobre as convenções de nomenclatura. 
	
	- A **Camada de preços** determina a capacidade e a cobrança. Ambas as camadas fornecem os mesmos recursos, mas em níveis diferentes de recursos.
	
		- Um serviço na camada **Gratuito** é executado em clusters compartilhados com outros assinantes. Essa camada oferece capacidade suficiente para testar tutoriais e escrever código de prova de conceito, mas não deve ser utilizada para aplicativos de produção. A implantação de um serviço gratuito geralmente demora apenas alguns minutos.
		- Um serviço na camada **Padrão** é executado em recursos dedicados e é altamente escalonável. Inicialmente, um serviço padrão é configurado com uma réplica e uma partição, mas você pode ajustar a capacidade após o serviço ser criado. A implantação de um serviço padrão demora mais tempo, normalmente cerca de quinze minutos.
	
	- **Grupos de recursos** são contêineres para serviços e contêineres usados para um fim comum. Por exemplo, se for compilar um aplicativo de pesquisa personalizado baseado na Pesquisa do Azure, Sites do Azure e armazenamento em Blob do Azure, você pode criar um grupo de recursos que mantém esses serviços juntos nas páginas de gerenciamento do portal.
	
	- **Assinatura** permite que você escolha entre várias assinaturas, se você tiver mais de uma.
	
	- **Local** é a região do datacenter. Atualmente, todos os recursos devem ser executados no mesmo datacenter. Não há suporte para a distribuição de recursos em vários datacenters.

4. Clique em **Criar** para provisionar o serviço.

Observe as notificações na barra de navegação rápida. Um aviso será exibido quando o serviço estiver pronto para uso.

<a id="sub-2"></a>
##Localizar o nome do serviço e chaves de Api do serviço de Pesquisa do Azure

Depois que o serviço for criado, você poderá retornar ao portal para obter a URL ou `api-key`. As conexões com seu serviço de Pesquisa requerem que você tenha tanto uma URL e um `api-key` para autenticar a chamada.

1. Na barra de navegação rápida, clique em **Home** e clique no Serviço de Pesquisa para abrir o painel do serviço. 

2. No painel de serviço, você verá blocos com as informações essenciais, bem como o ícone de chave para acessar as chaves de administrador.

  	![][3]

3. Copie a URL do serviço e uma chave de administrador. Você precisará para a próxima tarefa, [Operações de serviço de teste](#sub-4).

<a id="sub-3"></a>
##Atualizar para a camada padrão

Muitos clientes iniciam com o serviço gratuito e, em seguida, atualizam para a camada padrão quando armazenamento ou o desempenho de consulta se torna inadequado para as cargas de trabalho que você precisa executar. A camada padrão oferece a você recursos dedicados em um datacenter do Azure, que só você pode utilizar. Operações de pesquisa requerem tanto réplicas de serviço quanto de armazenamento. Quando se inscreve para a pesquisa padrão, você pode otimizar a configuração de serviço para usar mais de qualquer um dos recursos que for mais importante no seu contexto.

Para usar a camada padrão, repita as etapas anteriores deste artigo para criar um novo serviço de Pesquisa, escolhendo a camada de preços Padrão. Observe que configurar recursos dedicados pode levar algum tempo, 15 minutos ou mais.

Não há nenhuma atualização in-loco da versão gratuita. A alteração para padrão, com seu potencial de dimensionamento, requer um novo serviço. Você precisará recarregar os índices e documentos usados por seu aplicativo de pesquisa.

Cada serviço de Pesquisa na camada padrão é criado com uma réplica e uma partição, mas pode ser redimensionado facilmente para níveis de recursos maiores.

1.	Após o serviço ser criado, volte para o painel do serviço. 

2.	Clique no bloco **Escala**.

3.	Use os controles deslizantes para adicionar réplicas, partições ou ambas.

Réplicas e partições adicionais são cobradas segundo as unidades de pesquisa. O total de unidades de pesquisa necessárias para dar suporte a qualquer configuração de recursos em particular é mostrado na página, conforme você adiciona recursos.

Você pode verificar os [Detalhes de Preços](http://go.microsoft.com/fwlink/p/?LinkID=509792) para obter as informações de cobrança por unidade. Consulte [Limites e restrições](http://msdn.microsoft.com/library/azure/dn798934.aspx) para ajudá-lo a decidir como configurar combinações de partição e de réplica.

<a id="sub-4"></a>
##Testar operações de serviço

Confirmar que seu serviço está funcionando e pode ser acessado por meio de um aplicativo cliente é a etapa final da configuração da Pesquisa. Você pode usar qualquer um dos links abaixo para uma abordagem sem código para verificar a disponibilidade do serviço.

- [Como usar o Chrome Postman com a Pesquisa do Azure](../search-chrome-postman/)
- [Como usar o Telerik Fiddler com a Pesquisa do Azure](../search-fiddler/)

<!--Next steps and links -->
<a id="next-steps"></a>
##Próximas etapas

Os materiais adicionais a seguir que mostram como compilar e gerenciar aplicativos de pesquisa que usam a Pesquisa do Azure.

- [Como usar a pesquisa do Azure no .NET](../search-howto-dotnet-sdk/)

- [Gerenciar sua solução de pesquisa no Microsoft Azure](../search-manage/)

- [Pesquisa do Azure no MSDN](http://msdn.microsoft.com/library/dn798933.aspx)

- [Vídeo do Canal 9: Introdução à pesquisa do Azure](http://channel9.msdn.com/Shows/Data-Exposed/Introduction-To-Azure-Search)


<!--Anchors-->
[Find the service name and api-keys of your Azure Search service]: #sub-2
[Upgrade to standard tier]: #sub-3
[Test service operations]: #sub-4
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/search-create-service-portal/create-search-portal-1.PNG
[2]: ./media/search-create-service-portal/create-search-portal-2.PNG
[3]: ./media/search-create-service-portal/create-search-portal-3.PNG


<!--HONumber=54--> 