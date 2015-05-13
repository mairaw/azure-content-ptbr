<properties 
	pageTitle="Painel Power BI do Stream Analytics | Azure" 
	description="Saiba como preencher o painel dinâmico do Power BI com dados de um trabalho do Stream Analytics." 
	services="stream-analytics" 
	documentationCenter="" 
	authors="jeffstokes72" 
	manager="paulettm" 
	editor="cgronlun"/>

<tags 
	ms.service="stream-analytics" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.workload="data-services" 
	ms.date="04/24/2015" 
	ms.author="jeffstok"/>
	
#Azure Stream Analytics e Power BI: Painel Dinâmico em Análise em Tempo real de Fados de Streaming

Um dos casos de uso comuns do Azure Stream Analytics é analisar os dados de streaming de alto volume em tempo real e colocar as informações em um painel dinâmico \(um painel atualizado em tempo real sem que o usuário precise atualizar o navegador\). O [Microsoft Power BI](https://powerbi.com/) é perfeito para criar o painel dinâmico em pouco tempo. [Veja um vídeo de exemplo para ilustrar o cenário](https://www.youtube.com/watch?v=SGUpT-a99MA). Neste artigo, saiba como usar o Power BI como uma saída para o trabalho do Azure Stream Analytics. Observação: o Azure Stream Analytics está disponível em geral, mas neste ponto, a saída do Power BI é um recurso de visualização do Azure Stream Analytics.

##Pré-requisitos

* Conta do Microsoft Azure usando a Id da Org \(o Power BI só funciona com a ID da Org. A ID da Org é o seu endereço de email de trabalho ou comercial, por exemplo, xyz@mycompany.com. Emails pessoais, como xyz@hotmail.com não são ids da org. [Você pode aprender mais sobre id da org aqui](https://www.arin.net/resources/request/org.html)\).
* Um fluxo de entrada para o trabalho do ASA \(Azure Stream Analytics\) de onde serão consumidos dados de streaming. Neste ponto, o ASA aceita entrada de um Hub de Eventos do Azure ou um Armazenamento de Blob do Azure.  

##Criar Trabalho do Azure Stream Analytics

No [Portal do Azure](https://manage.windowsazure.com), clique em **Novo, Serviços de Dados, Stream Analytics, Criação Rápida**.

Especifique os valores a seguir, então clique em **Criar trabalho do Stream Analytics**:

* **Nome do Trabalho** - Insira um nome de trabalho. Por exemplo, **Temperaturas do Dispositivo**.
* **Região** - Selecione a região em que você deseja que o trabalho esteja localizado. Considere posicionar o trabalho e o hub de eventos na mesma região para garantir um desempenho ideal e evitar custos de transferência de dados entre regiões.
* **Conta de Armazenamento** - Escolha a Conta de Armazenamento que você deseja usar para armazenar dados de monitoramento de todos os trabalhos do Stream Analytics executados nesta região. Você tem a opção de escolher uma Conta de Armazenamento existente ou criar uma nova.

Clique em **Stream Analytics** no painel esquerdo para listar os trabalhos do Stream Analytics.

![elementográfico1][graphic1]

> [AZURE.TIP]O novo trabalho será listado com um status **Não Iniciado**. Observe que o botão **Iniciar** na parte inferior da página está desabilitado. Esse é o comportamento esperado, já que você deve configurar a entrada do trabalho, a saída, a consulta e assim por diante antes de poder iniciar o trabalho.

##Especificar entrada de trabalho

Para este tutorial, estamos supondo que você está usando o Hub de Eventos como uma entrada com serialização JSON e codificação utf-8.

* Clique no nome do trabalho.
* Clique em **Entradas** na parte superior da página e então clique em **Adicionar Entrada**. A caixa de diálogo que se abre vai orientar você por algumas etapas para configurar sua entrada.
*	Selecione **Fluxo de Dados** e então clique no botão direito.
*	Selecione **Hub de Eventos** e então clique no botão direito.
*	Digite ou selecione os seguintes valores na terceira página:
  *	**Alias de Entrada** - Insira um nome amigável para essa entrada de trabalho. Observe que você usará esse nome na consulta posteriormente.
  * **Hub de Eventos** - Se o Hub de Eventos que você criou estiver na mesma assinatura que o trabalho do Stream Analytics, selecione o namespace que contém o hub de eventos.
*	Se o hub de eventos estiver em uma assinatura diferente, selecione **Usar Hub de Eventos de Outra Assinatura** e insira manualmente informações para **Namespace do Service Bus**, **Nome do Hub de Eventos**, **Nome da Política do Hub de Eventos**, **Chave de Política do Hub de Eventos** e **Contagem de Partições do Hub de Eventos**.

> [AZURE.NOTE]Este exemplo usa o número de partições padrão, que é 16.

* **Nome do Hub de Eventos** - Selecione o nome do Hub de eventos do Azure que você tem.
* **Nome de Política do Hub de Eventos** -Selecione a política de hub de eventos para o hub de eventos que você está usando. Verifique se essa política tem permissões de gerenciamento.
*	**Grupo de Consumidores do Hub de Eventos** – Você pode deixar esta opção vazia ou especificar um grupo de consumidores que você tem em seu hub de eventos. Observe que cada grupo de consumidores de um hub de eventos pode ter apenas cinco leitores por vez. Dessa forma, decida qual é o grupo de consumidores certo para seu trabalho. Se você deixar o campo em branco, ele usará o grupo de consumidores padrão.

*	Clique no botão direito.
*	Especifique os seguintes valores:
  *	**Formato do Serializador de Eventos** - JSON
  *	**Codificação** - UTF8
*	Clique no botão de verificação para adicionar essa fonte e para verificar se o Stream Analytics pode se conectar com êxito ao hub de eventos.

##Adicionar saída do Power BI

1.  Clique em **Saída** na parte superior da página e então clique em **Adicionar Saída**. Você verá o Power BI listado como uma opção de saída.

![elementográfico2][graphic2]

> [AZURE.NOTE]Observação - A saída do Power BI só estará disponível para contas do Azure usando Ids de Org. Se você não estiver usando uma Id da Org para sua conta do Azure \(por exemplo, seu live id/conta pessoal da Microsoft\), não verá uma opção de saída do Power BI.

2.  Selecione **Power BI** e então clique no botão direito.
3.  Você verá uma tela como esta:

![elementográfico3][graphic3]

4.  Nesta etapa, você precisa ter cuidado para usar a mesma Id da Org que está usando para o trabalho do ASA. Neste ponto, a saída de Power BI deve usar a mesma Id da Org usada pelo trabalho do ASA. Se você já tiver uma conta do Power BI usando a mesma Id da Org, selecione "Autorizar Agora". Caso contrário, escolha “Inscrever-se agora” e use a mesma Id da Org como sua conta do Azure para inscrever-se no Power BI. [Aqui você encontrará um blog com detalhes de orientação para a inscrição no Power BI](http://blogs.technet.com/b/powerbisupport/archive/2015/02/06/power-bi-sign-up-walkthrough.aspx).
5.  Em seguida, você verá uma tela como esta:

![elementográfico4][graphic4]

Forneça valores como a seguir:

* **Alias de Saída** – Você pode colocar qualquer alias de saída que seja fácil de lembrar. Esse alias de saída será particularmente útil se você optar por ter várias saídas para seu trabalho. Nesse caso, você precisa se referir a essa saída em sua consulta. Por exemplo, vamos usar o valor do alias de saída = "OutPbi".
* **Nome do Conjunto de Dados** -Forneça um nome de conjunto de dados que você deseja que a saída do Power BI tenha. Por exemplo, usaremos “pbidemo”.
*	**Nome da Tabela** - Forneça um nome de tabela sob o conjunto de dados da sua saída do Power BI. Digamos que nós a chamaremos de “pbidemo”. Atualmente, a saída do Power BI de trabalhos do ASA só podem ter uma tabela em um conjunto de dados.

>	[AZURE.NOTE] Note - You should not explicitly create this dataset and table in your Power BI account. They will be automatically created when you start your ASA job and the job starts pumping output into Power BI. If your ASA job query doesn’t return any results, the dataset and table will not be created.

*	Clique em **OK**, **Testar Conexão** e agora sua configuração de saída está concluída.

>	[AZURE.WARNING] Also be aware that if Power BI already had a dataset and table with the same name as the one you provided in this ASA job, the existing data will be overwritten.


##Gravar Consultas

Vá para a guia **Consulta** do seu trabalho. Escreva sua consulta, a saída da qual você deseja em seu Power BI. Por exemplo, ela poderia ser algo como a seguinte consulta SQL:

    SELECT
    	MAX(hmdt) AS hmdt,
    	MAX(temp) AS temp,
    	System.TimeStamp AS time,
    	dspl
    INTO
        OutPBI
    FROM
    	Input TIMESTAMP BY time
    GROUP BY
    	TUMBLINGWINDOW(ss,1),
    	dspl

    
    
Inicie o trabalho. Valide se o seu hub de eventos está recebendo eventos e se sua consulta gera os resultados esperados. Se a sua consulta tiver como saída 0 linhas, o conjunto de dados e as tabelas do Power BI não serão criados automaticamente.

##Criar o Painel no Power BI

Acesse [Powerbi.com](https://powerbi.com) e faça logon com a sua Id da Org. Se a consulta do trabalho do ASA tiver como saída resultados, você verá seu conjunto de dados já criado:

![elementográfico5][graphic5]

Para a criação do painel, vá para a opção de Painéis e crie um novo Painel.

![elementográfico6][graphic6]

Neste exemplo, vamos rotulá-lo como “Painel de Demonstração”.

Agora, clique no conjunto de dados criado por seu trabalho do ASA \(pbidemo, em nosso exemplo atual\). Você será levado a uma página para criar um gráfico na parte superior desse conjunto de dados. A seguir, um exemplo dos relatórios que você pode criar:

Selecione os campos Σ temporário e hora. Eles irão automaticamente para Valor e Eixo no gráfico:

![elementográfico7][graphic7]

Com isso, você obterá automaticamente um gráfico como a seguir:

![elementográfico8][graphic8]

Na seção de valor, clique na lista suspensa de temporários e escolha **média** para a temperatura e, no gráfico, clique em **visualização** e escolha **gráfico de linhas**:

![elementográfico9][graphic9]

Agora, você obterá um gráfico de linhas de média ao longo do tempo. Usando a opção de fixação como mostrado abaixo, você poderá fixar isso em seu painel criado anteriormente:

![elementográfico10][graphic10]

Agora quando você exibir o painel com esse relatório fixado, você verá a atualização do relatório em tempo real. Tente alterar os dados em seus eventos – Temp especial ou algo parecido e você verá o efeito em tempo real disso refletido no painel.

Observe que este tutorial demonstrou como criar um tipo de gráfico para um conjunto de dados. No entanto, as possibilidades com o Power BI são ilimitadas. Para obter outro exemplo de um painel do Power BI, assista ao vídeo [Introdução ao Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s).

Outro recurso útil para saber mais sobre como criar Painéis com o Power BI é [Painéis no Power BI Preview](http://support.powerbi.com/knowledgebase/articles/424868-dashboards-in-power-bi-preview).

## Obter ajuda
Para obter mais assistência, experimente nosso [Fórum do Stream Analytics do Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## Próximas etapas

- [Introdução ao Stream Analytics do Azure](stream-analytics-introduction.md)
- [Introdução ao uso do Stream Analytics do Azure](stream-analytics-get-started.md)
- [Dimensionar trabalhos do Stream Analytics do Azure](stream-analytics-scale-jobs.md)
- [Referência de Linguagem de Consulta do Stream Analytics do Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Referência da API REST do Gerenciamento do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-power-bi-dashboard/1-stream-analytics-power-bi-dashboard.png
[graphic2]: ./media/stream-analytics-power-bi-dashboard/2-stream-analytics-power-bi-dashboard.png
[graphic3]: ./media/stream-analytics-power-bi-dashboard/3-stream-analytics-power-bi-dashboard.png
[graphic4]: ./media/stream-analytics-power-bi-dashboard/4-stream-analytics-power-bi-dashboard.png
[graphic5]: ./media/stream-analytics-power-bi-dashboard/5-stream-analytics-power-bi-dashboard.png
[graphic6]: ./media/stream-analytics-power-bi-dashboard/6-stream-analytics-power-bi-dashboard.png
[graphic7]: ./media/stream-analytics-power-bi-dashboard/7-stream-analytics-power-bi-dashboard.png
[graphic8]: ./media/stream-analytics-power-bi-dashboard/8-stream-analytics-power-bi-dashboard.png
[graphic9]: ./media/stream-analytics-power-bi-dashboard/9-stream-analytics-power-bi-dashboard.png
[graphic10]: ./media/stream-analytics-power-bi-dashboard/10-stream-analytics-power-bi-dashboard.png

<!--HONumber=52-->