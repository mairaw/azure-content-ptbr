<properties 
	pageTitle="Definir entradas | Microsoft Azure" 
	description="Noções básicas sobre entradas do Stream Analytics" 
	keywords="big data analytics,cloud service,internet of things,managed service,stream processing,streaming analytics,streaming data"
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
	ms.date="09/09/2015" 
	ms.author="jeffstok"/>

# Noções básicas sobre entradas do Stream Analytics

As entradas do Stream Analytics do Azure são definidas como uma conexão a uma fonte de dados. O Stream Analytics tem integração excelente com o Hub de Eventos de fontes do Azure e armazenamento de Blob de dentro e de fora da assinatura do Azure na qual o seu trabalho está sendo executado. Como os dados são enviados a essa fonte de dados, eles são consumidos pelo trabalho do Stream Analytics e processados em tempo real. As entradas são divididas em dois tipos distintos: entradas de fluxo de dados e entradas de dados de referência.

## Entradas de fluxo de dados

Um fluxo de dados é uma sequência ilimitada de eventos ao longo do tempo. Os trabalhos do Stream Analytics devem conter pelo menos uma fonte de entrada de fluxo de dados para ser consumida e transformada pelo trabalho. O Armazenamento de Blob do Azure e os Hubs de Eventos do Azure têm suporte como fontes de entrada de fluxo de dados. Os Hubs de Eventos do Azure são usados para coletar fluxos de eventos de vários dispositivos e serviços, como feeds de atividade de mídias sociais, informações sobre o mercado de ações ou dados de sensores. Como alternativa, o armazenamento Blob do Azure pode ser usado como uma fonte de entrada para ingerir dados em massa como um fluxo.

## Entradas de dados de referência

O Stream Analytics dá suporte a um segundo tipo de fonte de entrada conhecido como dados de referência. São dados auxiliares que são estáticos ou que estão sofrendo uma alteração lenta ao longo do tempo e normalmente são usados para executar a correlação e pesquisas. O Armazenamento de Blob do Azure é a única fonte de entrada com suporte para dados de referência. Os blobs de fonte de dados de referência estão limitados a 50MB de tamanho.

Para saber como criar entradas de dados de referência, consulte [Usar dados de referência](./articles/stream-analytics-use-reference-data.md).

## Criando um fluxo de entrada de dados de Hub de Eventos

[Hubs de eventos](https://azure.microsoft.com/services/event-hubs/) é um ingestor de eventos altamente escalonável de publicação / assinatura. Ele pode incluir milhões de eventos por segundo, para que você possa processar e analisar grandes quantidades de dados produzidos por seus aplicativos e dispositivos conectados. É uma das entradas mais comumente usadas para Stream Analytics. Hubs de eventos e Stream Analytics juntos fornecem aos clientes uma solução de ponta a ponta para análise em tempo real. Hubs de eventos permitem que os clientes alimentem eventos no Azure em tempo real e trabalhos do Stream Analytics podem processá-los em tempo real. Por exemplo, os clientes podem enviar cliques da Web, leituras de sensor, eventos de log online para Hubs de Eventos, e criar trabalhos do Stream Analytics que usem os Hubs de Eventos como os fluxos de dados de entrada para filtragem, agregação e correlação em tempo real.

É importante observar que o carimbo de hora padrão de eventos provenientes de Hubs de eventos no Stream Analytics é o carimbo de hora de que o evento foi recebido no Hub de eventos que é *EventEnqueuedUtcTime*. Para processar os dados como um fluxo usando um carimbo de data/hora na carga útil do evento, use a palavra-chave [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx).

## Grupos de consumidores

Cada entrada do trabalho do Hub de Eventos do Stream Analytics deve ser configurada para ter seu próprio grupo de consumidores. Quando um trabalho contiver uma autojunção ou várias entradas, algumas entradas poderão ser lidas por mais de um downstream de leitor, causando impacto no número de leitores de um único grupo de consumidores. Para evitar exceder o limite de Hub de Eventos dos 5 leitores por grupo de consumidores por partição, recomenda-se designar um grupo de consumidores para cada trabalho do Stream Analytics. Observe que também há um limite de 20 grupos de consumidores por Hub de Eventos. Para saber detalhes, confira o [Guia de programação de Hubs de Eventos](./articles/event-hubs-programming-guide.md).

## Configurando o Hub de eventos como um fluxo de dados de entrada

A tabela a seguir explica cada propriedade na guia de entrada de do Hub de eventos com sua descrição:

| Nome da Propriedade | Descrição |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alias de entrada | Um nome amigável que será usado na consulta de trabalho para fazer referência a essa entrada |
| Namespace do Barramento de Serviço | Um namespace Barramento de Serviço é um contêiner para um conjunto de entidades de mensagens. Ao criar um novo Hub de Eventos, você também criou um namespace Barramento de Serviço. |
| Hub de evento | O nome da sua entrada de Hub de eventos |
| Nome da política do Hub de Eventos. | A política de acesso compartilhada, que pode ser criada na guia Configurar o Hub de Eventos. Cada política de acesso compartilhado terá um nome, as permissões definidas por você e as chaves de acesso. |
| Chave de política do Hub de eventos | A chave de acesso compartilhado usada para autenticar o acesso ao namespace do Barramento de Serviço |
| Grupo de consumidores de Hub de Eventos (Opcional) | O Grupo de Consumidores para ingerir dados do Hub de Eventos. Se não for especificado, os trabalhos do Stream Analytics usarão o Grupo de Consumidores Padrão para ingerir dados do Hub de Eventos. É recomendável usar um Grupo de consumidores distinto para cada trabalho do Stream Analytics. |
| Formato de serialização do evento | Para verificar se as consultas funcionam da maneira esperada, o Stream Analytics precisa saber qual formato de serialização (JSON, CSV ou Avro) você está usando para os fluxos de dados de entrada |
| Codificação | UTF-8 é o único formato de codificação com suporte no momento. |

Quando seus dados forem provenientes de uma fonte de Hub de eventos, você pode acessar alguns campos de metadados em sua consulta do Stream Analytics. A tabela a seguir lista os campos e suas descrições.

| Propriedade | Descrição |
|------------------------------|--------------------------------------------------------------------|
| System.EventProcessedUtcTime | A data e a hora em que o evento foi processado pelo Stream Analytics |
| System.EventEnqueuedUtcTime | A data e a hora em que o evento foi recebido pelos Hubs de eventos. |
| System.PartitionId | A ID de partição com base em zero para o adaptador de entrada |

Por exemplo, você pode escrever uma consulta semelhante à seguinte:


    SELECT
    	System. EventProcessedUtcTime,
    	System. EventEnqueuedUtcTime,
    	System.PartitionId
    FROM Input

## Como criar uma entrada do fluxo de dados do armazenamento de Blob

Para cenários com grandes quantidades de dados não estruturados para armazenamento na nuvem, o Armazenamento de Blob oferece uma solução econômica e escalonável. Os dados no [armazenamento de Blob](http://azure.microsoft.com/services/storage/blobs/) são geralmente considerados como dados "em repouso", mas podem ser processados como um fluxo de dados pelo Stream Analytics. Um cenário comum para entradas do armazenamento de Blob com o Stream Analytics é o processamento de log, no qual a telemetria é capturada de um sistema e precisa ser analisada e processada para extrair dados significativos.

É importante observar que o carimbo de data/hora padrão de eventos de armazenamento de Blob no Stream Analytics é o carimbo de data/hora em que o blob foi modificado pela última vez que é *BlobLastModifiedUtcTime*. Para processar os dados como um fluxo usando um carimbo de data/hora na carga útil do evento, use a palavra-chave [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx).

## Configurando o armazenamento de Blob como um fluxo de dados de entrada

A tabela a seguir explica cada propriedade na guia de entrada do armazenamento de Blob com sua descrição:

<table>
<tbody>
<tr>
<td>Nome da Propriedade</td>
<td>Descrição</td>
</tr>
<tr>
<td>Alias de entrada</td>
<td>Um nome amigável que será usado na consulta de trabalho para fazer referência a essa entrada.</td>
</tr>
<tr>
<td>Conta de armazenamento</td>
<td>O nome da conta de armazenamento onde estão localizados os arquivos de blob.</td>
</tr>
<tr>
<td>Chave da conta de armazenamento</td>
<td>A chave secreta associada à conta de armazenamento.</td>
</tr>
<tr>
<td>Contêiner de armazenamento</td>
<td>Os contêineres fornecem um agrupamento lógico de blobs armazenados no serviço Blob do Microsoft Azure. Quando você carrega um blob no serviço Blob, você deve especificar um contêiner para aquele blob.</td>
</tr>
<tr>
<td>Padrão de prefixo do caminho [opcional]</td>
<td>O caminho do arquivo usado para localizar seus blobs no contêiner especificado.<BR>No caminho, você pode optar por especificar uma ou mais instâncias das três variáveis a seguir:<BR>{data}, {hora}, {partição}<BR>Exemplo 1: cluster1/logs/{data}/{hora}/{partição}<BR>Exemplo 2: cluster1/logs/{data}</td>
</tr>
<tr>
<td>Formato de data [opcional]</td>
<td>Se o token de data for usado no caminho do prefixo, você pode selecionar o formato de data na qual os arquivos são organizados. Exemplo: AAAA/MM/DD</td>
</tr>
<tr>
<td>Formato de hora [opcional]</td>
<td>Se o token de hora for usado no caminho do prefixo, você pode selecionar o formato de hora na qual os arquivos são organizados. Atualmente, o único valor aceito é HH.</td>
</tr>
<tr>
<td>Formato de serialização do evento</td>
<td>Para verificar se as consultas funcionam da maneira esperada, o Stream Analytics precisa saber qual formato de serialização (JSON, CSV ou Avro) você está usando para os fluxos de dados de entrada.</td>
</tr>
<tr>
<td>Codificação</td>
<td>Para CSV e JSON, UTF-8 é o único formato de codificação com suporte no momento.</td>
</tr>
<tr>
<td>Delimitador</td>
<td>O Stream Analytics é compatível com vários delimitadores comuns para serialização de dados no formato CSV. Os valores suportados são vírgula, ponto e vírgula, espaço, tab e barra vertical.</td>
</tr>
</tbody>
</table>

Quando seus dados forem provenientes de uma fonte de armazenamento de Blob, você pode acessar alguns campos de metadados em sua consulta do Stream Analytics. A tabela a seguir lista os campos e suas descrições.

| Propriedade | Descrição |
|--------------------------------|--------------------------------------------------------------------|
| System.BlobName | O nome do blob de entrada de onde o evento veio. |
| System.EventProcessedUtcTime | A data e a hora em que o evento foi processado pelo Stream Analytics |
| System.BlobLastModifiedUtcTime | A data e hora da última modificação do blob |
| System.PartitionId | A ID de partição com base em zero para o adaptador de entrada |

Por exemplo, você pode escrever uma consulta semelhante à seguinte:


    SELECT
    	System.BlobName,
    	System.EventProcessedUtcTime,
    	System.BlobLastModifiedUtcTime
    FROM Input


## Obter ajuda
Para obter mais assistência, experimente nosso [Fórum do Stream Analytics do Azure](https://social.msdn.microsoft.com/Forums/pt-BR/home?forum=AzureStreamAnalytics)

## Próximas etapas
Você foi apresentado ao Stream Analytics, um serviço gerenciado para análise de streaming em dados da Internet das coisas. Para saber mais sobre esse serviço, consulte:

- [Introdução ao uso do Stream Analytics do Azure](stream-analytics-get-started.md)
- [Dimensionar trabalhos do Stream Analytics do Azure](stream-analytics-scale-jobs.md)
- [Referência de Linguagem de Consulta do Stream Analytics do Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Referência da API REST do Gerenciamento do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

<!---HONumber=Sept15_HO2-->