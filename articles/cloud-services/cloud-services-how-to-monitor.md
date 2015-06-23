<properties 
	pageTitle="Como monitorar um serviço de nuvem - Azure" 
	description="Saiba como monitorar serviços de nuvem usando o Portal de Gerenciamento do Azure." 
	services="cloud-services" 
	documentationCenter="" 
	authors="rboucher" 
	manager="jwhit" 
	editor=""/>

<tags 
	ms.service="cloud-services" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="10/23/2014" 
	ms.author="robb"/>


#Como monitorar serviços de nuvem

[AZURE.INCLUDE [isenção de responsabilidade](../../includes/disclaimer.md)]

Você pode monitorar métricas do desempenho de seus serviços de nuvem no Portal de Gerenciamento do Azure. Você pode definir o nível de monitoramento como mínimo e detalhado para cada função do serviço, além de poder personalizar as exibições de monitoramento. Os dados de monitoramento detalhados são armazenados em uma conta do armazenamento, que você poderá acessar fora do portal.

O monitoramento das exibições no Portal de Gerenciamento são altamente configuráveis. Você pode escolher as métricas que você deseja monitorar na lista de métricas na página **Monitoramento** e pode escolher quais métricas para plotar em gráficos de métricas na página **Monitoramento** e no painel.

##Sumário##
* [Conceitos](#concepts)
* [Como: configurar o monitoramento de serviços de nuvem](#verbose)
* [Como: receber alertas para métricas dos serviços de nuvem](#receivealerts)
* [Como: adicionar métricas à tabela de métricas](#addmetrics)
* [Como: personalizar o gráfico de métricas](#customizechart)
* [Como: acessar dados detalhados de monitoramento fora do Portal de Gerenciamento](#accessverbose)

<h2><a id="concepts"></a>Conceitos</h2>

Por padrão, o monitoramento mínimo é fornecido para um novo serviço de nuvem usando os contadores do desempenho coletados do sistema operacional do host para as instâncias de função (máquinas virtuais). As métricas mínimas estão limitadas a percentual de CPU, entrada de dados, saída de dados, transferência de leitura e gravação de disco. Configurando o monitoramento detalhado, você pode receber métricas adicionais com base nos dados de desempenho em máquinas virtuais (instâncias de função). As métricas detalhadas permitem uma análise mais próxima dos problemas que ocorrem durante operações de aplicativo.

> [AZURE.NOTE]Se você usar o monitoramento detalhado, é possível adicionar mais contadores de desempenho na inicialização da instância de função por meio de um arquivo de configuração de diagnóstico. Para monitorar essas métricas no Portal de Gerenciamento, você deve adicionar esses contadores de desempenho antes de configurar o monitoramento detalhado. Para obter mais informações, consulte [Habilitando o diagnóstico nos serviços de nuvem e máquinas virtuais do Azure](cloud-services-dotnet-diagnostics.md).

Por padrão, os dados do contador de desempenho das instâncias de função passam por amostragem e são transferidos da instância de função em intervalos de 3 minutos. Quando você habilita o monitoramento detalhado, os dados brutos do contador de desempenho são agregados para cada instância de função e por instâncias de função para cada função em intervalos de 5 minutos, 1 hora e 12 horas. Os dados agregados são limpos após 10 dias.

Após habilitar o monitoramento detalhado, os dados de monitoramento agregados são armazenados nas tabelas em sua conta de armazenamento. Para habilitar o monitoramento detalhado para uma função, você deve configurar uma cadeia de conexão de diagnóstico vinculada à sua conta de armazenamento. Você pode usar contas de armazenamento diferentes para funções diferentes.

Observe que habilitar o monitoramento detalhado aumentará os custos de armazenamento relacionados ao armazenamento de dados, à transferência de dados e às transações do armazenamento. O monitoramento mínimo não requer uma conta de armazenamento. Os dados para as métricas, expostos no nível mínimo do monitoramento, não são armazenados na sua conta de armazenamento, mesmo que você defina o nível de monitoramento para detalhado.


<h2><a id="verbose"></a>Como: configurar o monitoramento de serviços de nuvem</h2>

Use os procedimentos a seguir para configurar o monitoramento mínimo ou detalhado no Portal de Gerenciamento. Não é possível ativar o monitoramento detalhado até que você habilite Diagnóstico do Azure e configurar cadeias de conexão de diagnóstico para habilitar Diagnóstico do Azure e acessar contas de armazenamento para armazenar os dados do monitoramento detalhado.

###Antes de começar###

- Crie uma conta de armazenamento para armazenar os dados de monitoramento. Você pode usar contas de armazenamento diferentes para funções diferentes. Para obter mais informações, consulte a ajuda para as **Contas de Armazenamento** ou consulte [Como criar uma conta de armazenamento](/manage/services/storage/how-to-create-a-storage-account/).


- Habilite o Diagnóstico do Azure para suas funções do serviço de nuvem. <br /><br />Para obter mais informações, consulte [Habilitando o diagnóstico nos serviços de nuvem e máquinas virtuais do Azure](cloud-services-dotnet-diagnostics.md).

No Portal de Gerenciamento, você pode adicionar ou modificar as cadeias de conexão de diagnóstico que Diagnóstico do Azure usa para acessar as contas do armazenamento que armazenam os dados do monitoramento detalhado, além de poder definir o nível de monitoramento como detalhado ou mínimo. Como o monitoramento detalhado armazena dados em uma conta de armazenamento, você deve configurar as cadeias de conexão de diagnóstico antes de definir o nível de monitoramento como detalhado.

###Para configurar cadeias de conexões de diagnóstico para o monitoramento detalhado###

1. Copie uma chave de acesso de armazenamento para a conta de armazenamento que você usará para armazenar os dados do monitoramento detalhado. No [Portal de Gerenciamento do Azure](https://manage.windowsazure.com/), você pode usar **Gerencia Chaves** na página **Contas de Armazenamento**. Para obter mais informações, consulte [Como gerenciar os serviços de nuvem](cloud-services-how-to-manage.md) ou consulte a ajuda para a página **Contas de Armazenamento**. 

2. Abra **Serviços de Nuvem**. Em seguida, para abrir o painel, clique no nome do serviço de nuvem que você deseja configurar.

3. Clique em **Produção** ou **Preparo** para exibir a implantação que você deseja configurar.

4. Clique em **Configurar**.

	Você editará as configurações de **monitoramento** na parte superior da página **Configurar**, mostrada abaixo. Se você não habilitou o Diagnóstico do Azure para o serviço de nuvem, a opção de **Nível** não estará disponível. Você não pode alterar a diretiva de retenção dos dados. Os dados do monitoramento detalhado para um serviço de nuvem são armazenados por 10 dias.

	![Opções de monitoramento](./media/cloud-services-how-to-monitor/CloudServices_MonitoringOptions.png)

5. Em **Cadeias de Conexão de Diagnóstico**, complete a cadeia de conexão de diagnóstico para cada função que você deseja monitorar detalhadamente.
	
	As cadeias de conexão têm o seguinte formato. (O exemplo é um serviço de nuvem que usa pontos de extremidade padrões.) Para atualizar uma cadeia de conexão, digite um nome de conta de armazenamento e uma chave de acesso de armazenamento válidos para a conta de armazenamento que você deseja usar.
         
 	DefaultEndpointsProtocol=https;AccountName=StorageAccountName;AccountKey=StorageAccountKey

6. Clique em **Salvar**.

Se você está ativando o monitoramento detalhado, execute o procedimento a seguir após configurar as cadeias de conexão de diagnóstico para as funções de serviço.


###Para alterar o nível de monitoramento para detalhado ou mínimo###

1. No [Portal de Gerenciamento](https://manage.windowsazure.com/), abra a página **Configurar** para implantar o serviço de nuvem.

2. Em **Nível**, clique em **Detalhado** ou **Mínimo**.

3. Clique em **Salvar**.

Após ativar o monitoramento detalhado, deve ser possível ver os dados de monitoramento no Portal de Gerenciamento dentro de uma hora.

Os dados brutos do contador de desempenho e os dados de monitoramento agregados são armazenados na conta do armazenamento, em tabelas qualificadas pela ID da implantação para as funções.

<h2><a id="receivealerts"></a>Como: receber alertas para métricas de serviço de nuvem</h2>

Você pode receber alertas baseados nas métricas de monitoramento do serviço de nuvem. Na página **Serviços de Gerenciamento** do Portal de Gerenciamento do Azure, você pode criar uma regra para acionar uma alerta quando a métrica escolhida alcançar um valor especificado. Você também pode escolher que um email seja enviado quando o alerta é disparado. Para obter mais informações, consulte [Como: receber notificações de alerta e gerenciar regras de alerta no Azure](http://go.microsoft.com/fwlink/?LinkId=309356).

<h2><a id="addmetrics"></a>Como: adicionar métricas à tabela de métricas</h2>

1. No [Portal de Gerenciamento](http://manage.windowsazure.com/), abra a página **Monitoramento** para o serviço de nuvem.

	Por padrão, a tabela de métricas exibe um subconjunto de métricas disponíveis. A ilustração mostra as métricas detalhadas padrão para um serviço de nuvem, que estão limitadas à contador de desempenho de memória\\MBytes disponíveis, com os dados agregados à nível de função. Use **Adicionar Métricas** para selecionar métricas agregadas e de nível de função adicionais para monitorar o Portal de Gerenciamento.

	![Exibição detalhada](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
 
2. Para adicionar métricas à tabela de métricas:

	a. Clique em **Adicionar Métricas** para abrir **Escolher Métricas**, conforme mostrado abaixo. A primeira métrica disponível é expandida para mostrar as opções disponíveis. Para cada métrica, a primeira opção exibe os dados agregados de monitoramento para todas as funções. Além disso, você pode escolher funções individuais para a exibição de dados.

	![Adicionar métricas](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)


	b. Para selecionar métricas para exibir:

	- Clique na seta para baixo próxima à métrica para expandir as opções de monitoramento.
	- Marque a caixa de seleção para cada opção de monitoramento que você deseja exibir.

	Você pode exibir até 50 métricas na tabela de métricas.

	> [AZURE.TIP]No monitoramento detalhado, a lista de métricas pode conter dezenas de métricas. Para exibir uma barra de rolagem, passe o cursor do mouse pelo lado direito da caixa de diálogo. Para filtrar a lista, clique no ícone da busca e digite o texto na caixa da busca, conforme mostrado abaixo.
 
	![Pesquisa de métricas adicionadas](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)

3. Após concluir a seleção de métricas, clique em OK (marca de seleção).

	As métricas selecionadas são adicionadas à tabela de métricas, conforme mostrado abaixo.

	![métricas de monitoramento](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)

 
4. Para excluir uma métrica da tabela de métricas, clique na métrica para selecioná-la e clique em **Excluir Métricas**. (Você poderá ver **Excluir Métricas** somente quando uma métrica for selecionada).


<h2><a id="customizechart"></a>Como: personalizar o gráfico de métricas</h2>

1. Na tabela de métricas, selecione até 6 métricas para plotar no gráfico de métricas. Para selecionar uma métrica, clique na caixa de seleção à esquerda. Para remover uma métrica do gráfico de métricas, desmarque a caixa de seleção na tabela de métricas.

	Conforme você seleciona métricas da tabela de métricas, elas são adicionadas ao gráfico de métricas. Em uma exibição estreita, uma lista suspensa de **n mais** contém os cabeçalhos de métrica que não caberão na exibição.

 
2. Para alternar entre a exibição dos valores relativos (somente valor final para cada métrica) e os valores absolutos (eixo Y exibido), selecione Relativo ou Absoluto na parte superior do gráfico.

	![Relativo ou Absoluto](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)

3. Para alterar o intervalo de tempo de exibição do gráfico de métricas, selecione 1 hora, 24 horas ou 7 dias na parte superior do gráfico.

	![Período de exibição do monitoramento](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)

	No gráfico de métricas do painel, o método para plotar métricas é diferente. Um conjunto padrão de métricas está disponível e métricas são adicionadas ou removidas selecionando o cabeçalho da métrica.

###Para personalizar o gráfico de métricas no painel###

1. Abra o painel do serviço de nuvem.

2. Adicionar ou remover métricas do gráfico:

	- Para plotar uma nova métrica, marque a caixa de seleção da métrica nos cabeçalhos do gráfico. Em uma exibição estreita, clique na seta para baixo por ***n*??métricas** métricas para plotar uma métrica que a área do cabeçalho do gráfico não pode exibir.

	- Para excluir uma métrica que seja plotado no gráfico, desmarque a caixa de seleção próxima ao cabeçalho.

3. Alterne entre exibições **Relativas** e **Absoluta**.

4. Escolha 1 hora, 24, horas ou 7 dias para exibição dos dados.

<h2><a id="accessverbose"></a>Como: acessar dados detalhados de monitoramento fora do Portal de Gerenciamento</h2>

Os dados do monitoramento detalhado são armazenados em tabelas nas contas de armazenamento que você especifica para cada função. Para cada implantação do serviço de nuvem, seis tabelas são criadas para a função. Duas tabelas são criadas para cada (5 minutos, 1 hora e 12 horas). Uma dessas tabelas armazena as agregações de nível de função; outra tabela armazena as agregações para as instâncias de função.

Os nomes das tabelas têm o formato a seguir:

	WAD*deploymentID*PT*aggregation_interval*[R|RI]Table

onde:

- *deploymentID* é a GUID atribuída à implantação do serviço de nuvem

- *aggregation_interval* = 5M, 1H ou 12H

- agregações de nível de função = R

- agregações para instâncias de função = RI

Por exemplo, as tabelas a seguir armazenariam os dados agregados do monitoramento detalhado em intervalos de 1 hora:

	WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

	WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)


<!--HONumber=54--> 