<properties 
	pageTitle="Adicionas entradas | Microsoft Azure" 
	description="Segmento do roteiro de aprendizagem Adicionar entradas."
	documentationCenter=""
	services="stream-analytics"
	authors="jeffstokes72" 
	manager="paulettm" 
	editor="cgronlun"/>

<tags 
	ms.service="stream-analytics" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.workload="data-services" 
	ms.date="09/09/2015" 
	ms.author="jeffstok"/>

# Adicionar Entradas

Os trabalhos do Stream Analytics do Azure podem ser conectados a uma ou mais entradas, o que define uma conexão com uma fonte de dados existente. Como os dados são enviados a essa fonte de dados, eles são consumidos pelo trabalho do Stream Analytics e processados em tempo real. O Stream Analytics tem integração excelente com os [Hubs de Eventos do Azure](http://azure.microsoft.com/services/event-hubs/) e o [armazenamento de Blob do Azure](./storage/storage-dotnet-how-to-use-blobs.md), ambos dentro e fora da assinatura de trabalho. Há dois tipos de entradas no Stream Analytics: fluxos de dados e dados de referência.

- **Fluxos de Dados**: os trabalhos do Stream Analytics devem incluir pelo menos uma entrada de fluxo de dados a ser consumida e transformada pelo trabalho. O Armazenamento de Blob do Azure e os Hubs de Eventos do Azure têm suporte como fontes de entrada de fluxo de dados. Os Hubs de Eventos do Azure são usados para coletar fluxos de eventos de dispositivos conectados, serviços e aplicativos. O armazenamento Blob do Azure pode ser usado como uma fonte de entrada para ingerir dados em massa como um fluxo.  
- **Dados de referência**: o Stream Analytics oferece suporte a um segundo tipo de entrada auxiliar chamado de dados de referência. Ao contrário dos dados em movimento, esses dados são estáticos ou se modificam lentamente. Geralmente, são usados para executar pesquisas e correlações com fluxos de dados a fim de criar um conjunto de dados mais rico. O Armazenamento de Blob do Azure é a única fonte de entrada com suporte para dados de referência.  

Para adicionar uma entrada ao trabalho do Stream Analytics:

1. No trabalho do Stream Analytics, clique em **Entradas** e em **Adicionar uma Entrada**.

    ![Adicionar Entradas](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)

2. Especifique o tipo de entrada: ou **Fluxo de dados**, ou **Dados de referência**.

    ![Adicionar dados](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)

3. Se criar uma entrada de Fluxo de Dados, especifique o tipo de fonte para a entrada. Essa tela é ignorada durante a criação de Dados de Referência, pois somente o armazenamento de Blob tem suporte.

    ![Adicionar fluxo de dados](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)

4. Forneça um nome amigável para essa entrada na caixa Alias de Entrada. Esse nome será usado na consulta do trabalho posteriormente para fazer referência à entrada.

    Preencha o restante das propriedades de conexão necessárias para se conectar à fonte de dados. Esses campos variam de acordo com o tipo de entrada e fonte e são definidos em detalhes [aqui](stream-analytics-create-a-job.md.)

    ![Adicionar hub de eventos](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)

5. Especifique as configurações de serialização para os dados de entrada:
	- Para verificar se suas consultas funcionam da maneira esperada, especifique o **Formato de Serialização de Evento** dos dados de entrada. Os formatos de serialização com suporte são JSON, CSV e Avro.
	- Verifique a **Codificação** dos dados. UTF-8 é o único formato de codificação com suporte no momento.

    ![Configurações da serialização de dados](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)

6. Depois de concluir a criação da entrada, o Stream Analytics verificará se ele pode se conectar à fonte de entrada. Você pode exibir o status da operação Testar Conexão no Hub de notificação.

    ![Testar conexão](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)


## Obter ajuda
Para obter mais assistência, experimente nosso [Fórum do Stream Analytics do Azure](https://social.msdn.microsoft.com/Forums/pt-BR/home?forum=AzureStreamAnalytics)

## Próximas etapas

- [Introdução ao Stream Analytics do Azure](stream-analytics-introduction.md)
- [Introdução ao uso do Stream Analytics do Azure](stream-analytics-get-started.md)
- [Dimensionar trabalhos do Stream Analytics do Azure](stream-analytics-scale-jobs.md)
- [Referência de Linguagem de Consulta do Stream Analytics do Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Referência da API REST do Gerenciamento do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!---HONumber=Sept15_HO2-->