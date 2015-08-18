<properties 
	pageTitle="Criação de pipelines" 
	description="Entenda os pipelines do Azure Data Factory e aprenda como criá-los para mover e transformar dados para produzir informações que podem ser usadas para obter insights" 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" y
	ms.date="07/28/2015" 
	ms.author="spelluru"/>

# Noções básicas sobre Pipelines e Atividades
Este artigo o ajudará a compreender pipelines e atividades no Azure Data Factory e como aproveitá-los para construir fluxos de trabalho orientados a dados de ponta a ponta para seu cenário ou negócio. Este artigo pressupõe que você verificou os artigos [Visão geral](data-factory-introduction.md) e [Criando conjuntos de dados](data-factory-create-datasets.md).

## O que é um pipeline?
**Pipelines são um agrupamento lógico de Atividades**. Eles são usados para agrupar atividades em uma unidade que executa uma tarefa. Para entender melhor os pipelines, você precisa entender uma atividade primeiro.

### O que é uma atividade?
As Atividades definem as ações a serem realizadas em seus dados. Cada atividade leva zero ou mais [conjuntos de dados](data-factory-create-datasets.md) como entrada e produz um ou mais conjuntos de dados como saída. **Uma atividade é uma unidade de orquestração no Azure Data Factory.**

Por exemplo, você pode usar uma atividade de Cópia para orquestrar a cópia de dados de um conjunto de dados para outro. Da mesma forma, você pode usar uma atividade do Hive que executará uma consulta de Hive em um cluster do Azure HDInsight para transformar ou analisar seus dados. O Azure Data Factory fornece uma ampla gama de atividades de [movimentação, análise](data-factory-data-transformation-activities.md) e [transformação de dados](data-factory-data-movement-activities.md). Você também pode optar por criar uma atividade personalizada do .NET para executar seu próprio código.

Considere os 2 seguintes conjuntos de dados:

**Conjunto de dados do SQL Azure**

A tabela 'MyTable' contém uma coluna 'timestampcolumn', que ajuda a especificar a data e hora de quando os dados foram inseridos no banco de dados.

	{
	  "name": "AzureSqlInput",
	  "properties": {
	    "type": "AzureSqlTable",
	    "linkedServiceName": "AzureSqlLinkedService",
	    "typeProperties": {
	      "tableName": "MyTable"
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    },
	    "policy": {
	      "externalData": {
	        "retryInterval": "00:01:00",
	        "retryTimeout": "00:10:00",
	        "maximumRetry": 3
	      }
	    }
	  }
	}

**Conjunto de dados de Blob do Azure**

Dados são copiados para um novo blob a cada hora com o caminho para o blob que reflete a data e hora específicas com granularidade de hora.

	{
	  "name": "AzureBlobOutput",
	  "properties": {
	    "type": "AzureBlob",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
	      "partitionedBy": [
	        {
	          "name": "Year",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "yyyy"
	          }
	        },
	        {
	          "name": "Month",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%M"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%d"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%H"
	          }
	        }
	      ],
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": "\t",
	        "rowDelimiter": "\n"
	      }
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}


A atividade de cópia no pipeline abaixo copiará os dados do SQL Azure para o armazenamento de Blob do Azure. Ele usa a tabela do SQL Azure como o conjunto de dados de entrada com frequência por hora e grava os dados para o armazenamento de Blob do Azure representado pelo conjunto de dados 'AzureBlobOutput'. O conjunto de dados de saída também tem uma frequência por hora. Consulte a seção Agendamento e execução para compreender como os dados são copiados na unidade de tempo. Esse pipeline tem um período ativo de 3 horas "2015-01-01T08:00:00" para "2015-01-01T11:00:00".

**Pipeline:**
	
	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2015-01-01T08:00:00",
	    "end":"2015-01-01T11:00:00",
	    "description":"pipeline for copy activity",
	    "activities":[  
	      {
	        "name": "AzureSQLtoBlob",
	        "description": "copy activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "AzureSQLInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "AzureBlobOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "SqlSource",
	            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \'{0:yyyy-MM-dd HH:mm}\' AND timestampcolumn < \'{1:yyyy-MM-dd HH:mm}\'', WindowStart, WindowEnd)"
	          },
	          "sink": {
	            "type": "BlobSink"
	          }
	        },
	       "scheduler": {
	          "frequency": "Hour",
	          "interval": 1
	        },
	        "policy": {
	          "concurrency": 1,
	          "executionPriorityOrder": "OldestFirst",
	          "retry": 0,
	          "timeout": "01:00:00"
	        }
	      }
	     ]
	   }
	}

Agora que temos uma breve compreensão sobre o que uma atividade é, vamos ver novamente o pipeline.
 
Pipelines são um agrupamento lógico de Atividades. Eles são usados para agrupar atividades em uma unidade que executa uma tarefa. **Um pipeline também é a unidade de implantação e gerenciamento de atividades.** Por exemplo, você poderá reunir atividades relacionadas logicamente como um pipeline, que podem estar em estado ativo ou pausado.

Um conjunto de dados de saída de uma atividade em um pipeline pode ser o conjunto de dados de entrada para outra atividade no mesmo pipeline/pipeline diferente, definindo as dependências entre atividades. A seção [Agendamento e execução](#scheduling-and-execution) aborda esse assunto em detalhes.

As etapas comuns ao criar um pipeline no Azure Data Factory são:

1.	Criar uma fábrica de dados (caso não tenha criado).
2.	Criar um serviço vinculado para cada armazenamento de dados ou serviço de computação.
3.	Criar conjuntos de dados de entrada e saída.
4.	Criar um pipeline com atividades que operam em conjuntos de dados definidos acima.

![Entidades de Data Factory](./media/data-factory-create-pipelines/entities.png)

Vamos dar uma olhada mais próxima em como um pipeline é definido.

## Anatomia de um Pipeline  

A estrutura genérica para um pipeline será semelhante ao seguinte:

	{
	    "name": "PipelineName",
	    "properties": 
	    {
	        "description" : "pipeline description",
	        "activities":
	        [
	
	        ],
			"start": "<start date-time>",
			"end": "<end date-time>"
	    }
	}

A seção **Atividades** pode ter uma ou mais atividades definidas dentro dela. Cada atividade tem a seguinte estrutura de nível superior:

	{
	    "name": "ActivityName",
	    "description": "description", 
	    "type": "<ActivityType>",
	    "inputs":  "[]",
	    "outputs":  "[]",
	    "linkedServiceName": "MyLinkedService",
	    "typeProperties":
	    {
	
	    },
	    "policy":
	    {
	    }
	    "scheduler":
	    {
	    }
	}

A tabela a seguir descreve as propriedades nas definições de JSON de atividade e pipeline:

Marca | Descrição | Obrigatório
--- | ----------- | --------
name | Nome da atividade ou pipeline. Especifique um nome que representa a ação que a atividade ou o pipeline é configurado para fazer<br/><ul><li>Número máximo de caracteres: 260</li><li>Deve começar com um número de letra ou um sublinhado (\_)</li><li>Os seguintes caracteres não são permitidos: “.”, “+”, “?”, “/”, “<”,”>”,”*”,”%”,”&”,”:”,”\\”</li></ul> | Descrição de Sim | Texto que descreve para o que a atividade ou o pipeline é usado | Tipo de Sim | Especifica o tipo de atividade. Consulte os artigos [Atividades de movimentação de dados](data-factory-data-movement-activities.md) e [Atividades de transformação de dados](data-factory-data-transformation-activities.md) para diferentes tipos de atividades. | Entradas de Sim | Tabelas de entrada usadas pela atividade<p>// one input table<br/>"inputs": [ { "name": "inputtable1" } ],</p><p>// two input tables <br/>"inputs": [ { "name": "inputtable1" }, { "name": "inputtable2" } ],</p> | Yes outputs | Output tables used by the activity.<p>// one output table<br/>"outputs": [ { "name": “outputtable1” } ],</p><p>//two output tables<br/>"outputs": [ { "name": “outputtable1” }, { "name": “outputtable2” } ],</p> | Yes linkedServiceName | Nome do serviço vinculado usado pela atividade. <p>Uma atividade pode exigir que você especifique o vinculado de serviço que se vincula ao ambiente de computação necessário.</p> | Sim para Atividade HDInsight e Atividade de Pontuação de Lote de Aprendizado de Máquina do Azure <p>Não para todos os outros</p> typeProperties | Propriedades na seção typeProperties dependem do tipo de atividade. Consulte o artigo sobre cada atividade individual para saber mais sobre isso | Nenhuma política | Políticas que afetam o comportamento de tempo de execução da atividade. Se não for especificado, as políticas padrão serão utilizadas. Rolar abaixo para obter detalhes | Não iniciar | Data-hora de início do pipeline. Deve estar no [formato ISO](http://en.wikipedia.org/wiki/ISO_8601). Por exemplo: 2014-10-14T16:32:41Z. As propriedades de início e término especificam o período ativo para o pipeline. Divisões de saída são produzidas somente neste período ativo. | Não<p>O período ativo para um pipeline também pode ser definido usando o Set-AzureDataFactoryPipelineActivePeriod cmdlet</p> End | End date-time para o pipeline. Se especificado, deve estar no formato ISO. Por exemplo: 2014-10-14T17:32:41Z <p>Se não for especificado, ele é calculado como "+ 48 horas de início". Para executar o pipeline indefinidamente, especifique 9999-09-09 como o valor para a propriedade final.</p>| Nenhum isPaused | Se definido como verdadeiro, o pipeline não será executado. Valor padrão = falso. Você pode usar essa propriedade para habilitar ou desabilitar. | Nenhum Agendador | A propriedade "Agendador" é usada para definir o agendamento desejado para a atividade. Suas subpropriedades são as mesmas na [propriedade de disponibilidade em um conjunto de dados](data-factory-create-datasets.md#Availability). | Não | 

### Tipos de atividade
O Azure Data Factory fornece uma ampla gama de atividades de [movimentação de dados](data-factory-data-movement-activities.md) e [transformação de dados](data-factory-data-transformation-activities.md).

### Políticas
As políticas afetam o comportamento de tempo de execução de uma atividade, especialmente quando a divisão de uma tabela é processada. A tabela a seguir fornece os detalhes.

Propriedade | Valores permitidos | Valor Padrão | Descrição
-------- | ----------- | -------------- | ---------------
simultaneidade | Inteiro <p>Valor máximo: 10</p> | 1 | Número de execuções concorrentes da atividade.<p>Determina o número de execuções de atividade paralela que podem ocorrer em intervalos diferentes. Por exemplo, se uma atividade precisa passar por um grande conjunto de dados disponíveis, ter uma maior simultaneidade acelera o processamento de dados.</p> 
executionPriorityOrder | NewestFirst<p>OldestFirst</p> | OldestFirst | Determina a ordem das divisões de dados que estão sendo processadas.<p>Por exemplo, se você tiver 2 divisões (uma ocorre às 16h e a outra às 17h), e ambas estão aguardando execução. Se você definir o executionPriorityOrder como NewestFirst, a divisão às 17h será processada primeiro. De modo semelhante, se você definir o executionPriorityORder como OldestFIrst, a divisão às 16h será processada.</p> 
tentar novamente | Inteiro<p>Valor máx. pode ser 10</p> | 3 | Número de novas tentativas antes do processamento de dados da divisão ser marcado como Com falha. A execução da atividade para uma divisão de dados é repetida até a contagem de repetição especificada. A nova tentativa é feita logo após a falha.
Tempo limite | TimeSpan | 00:00:00 | Tempo limite para a atividade. Exemplo: 00:10:00 (implica o tempo limite de 10 minutos)<p>Se um valor não for especificado ou for 0, o tempo limite é infinito.</p><p>Se o tempo de processamento de dados em uma divisão exceder o valor de tempo limite, ele será cancelado e o sistema tentará repetir o processamento. O número de repetições depende da propriedade de repetição. Quando o tempo limite ocorre, o status será TimedOut.</p>
atrasar | TimeSpan | 00:00:00 | Especifique o atraso antes do processamento de dados da divisão começar.<p>A execução da atividade de uma divisão de dados é iniciada após o atraso passar do tempo de execução esperado.</p><p>Exemplo: 00:10:00 (implica o atraso de 10 minutos)</p>
longRetry | Inteiro<p>Valor máximo: 10</p> | 1 | O número de longas novas tentativas antes que a execução da divisão falhe.<p>Tentativas longRetry são espaçadas por longRetryInterval. Portanto, se você precisar especificar um tempo entre tentativas de repetição, use longRetry. Se Retry e longRetry forem especificados, cada tentativa de longRetry incluirá novas tentativas de Retry e o número máximo de tentativas será Retry * longRetry.</p><p>Por exemplo, se tivermos o seguinte na política de atividade:<br/>Retry: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/></p><p>Pressupomos que haja apenas uma divisão para executar (o status é PendingExecution) e sempre ocorre falha na execução de atividade. Inicialmente haveria três tentativas consecutivas de execução. Após cada tentativa, o status de divisão seria Retry. Depois de três primeiras tentativas, o status da divisão seria LongRetry.</p><p>Depois de uma hora (por exemplo, valor do longRetryInteval), deve haver outro conjunto de três tentativas consecutivas de execução. Depois disso, o status da divisão seria Com falha e não haveria nova tentativa. Portanto, em geral, foram feitas 6 tentativas.</p><p>Observação: Se qualquer execução for bem-sucedida, o status da divisão seria Pronto e não haveria nova tentativa.</p><p>longRetry pode ser usado em situações em que dados dependentes chegam em horários não determinísticos ou o ambiente geral está bastante instável nos dados em que o processamento ocorre. Nesses casos, fazer novas tentativas uma após a outra pode não ajudar e fazer isso após um intervalo de tempo resulta na saída desejada.</p><p>Nota de advertência: não definir valores altos para longRetry ou longRetryInterval. Geralmente, valores mais altos implicam em outros problemas sistêmicos que estão sendo tratados em</p> 
longRetryInterval | TimeSpan | 00:00:00 | O intervalo entre tentativas de repetição longa 

## Criação e gerenciamento de um pipeline
O Azure Data Factory fornece vários mecanismos para criar e implantar pipelines (que, por sua vez, contêm uma ou mais atividades neles).

### Uso do Portal de Visualização do Azure

1. Faça logon no [Portal de Visualização do Azure](https://portal.azure.com/).
2. Navegue até sua instância do Azure Data Factory na qual você deseja criar um pipeline
3. Clique no bloco **Criar e implantar** na lente **Resumo**. 
 
	![Bloco Criar e implantar](./media/data-factory-create-pipelines/author-deploy-tile.png)

4. Clique em **Novo pipeline** na barra de comandos.

	![Botão Novo pipeline](./media/data-factory-create-pipelines/new-pipeline-button.png)

5. Você deve ver a janela do editor com um modelo JSON de pipeline.

	![Editor de pipeline](./media/data-factory-create-pipelines/pipeline-in-editor.png)

6. Depois de concluir a criação do pipeline, em seguida, clique em **Implantar** na barra de comando para implantar o pipeline.

	**Observação:** durante a implantação, o serviço Azure Data Factory executa algumas verificações de validação para ajudar a corrigir alguns problemas comuns. Caso haja um erro, as informações correspondentes serão exibidas. Execute ações corretivas e, em seguida, reimplante o pipeline criado. Você pode usar o editor para atualizar e excluir um pipeline.

### Usando o plug-in do Visual Studio
Você pode usar o Visual Studio para criar e implantar pipelines no Azure Data Factory. Para obter mais informações, consulte [Tutorial: Copiar dados de Armazenamento do Azure para SQL Azure (Visual Studio)](data-factory-get-started-using-vs.md).

### Usando o PowerShell do Azure
Você pode usar o Azure PowerShell para criar pipelines no Azure Data Factory. Digamos que você definiu o pipeline JSON em um arquivo em c:\\DPWikisample.json. Você pode carregá-lo na sua instância do Azure Data Factory, conforme mostrado no exemplo a seguir.

	New-AzureDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

Para saber mais sobre esse cmdlet, consulte [New-AzureDataFactoryPipeline cmdlet](https://msdn.microsoft.com/library/dn820227.aspx).

### Usando a API REST
Você também pode criar e implantar o pipeline usando APIs REST. Esse mecanismo pode ser utilizado para criar pipelines programaticamente. Para saber mais sobre isso, consulte [Criar ou atualizar um Pipeline](https://msdn.microsoft.com/library/azure/dn906741.aspx).

### Usando o SDK .NET
Você também pode criar e implantar o pipeline usando SDK .NET. Esse mecanismo pode ser utilizado para criar pipelines programaticamente. Para saber mais sobre isso consulte, [Criar, gerenciar e monitorar as fábricas de dados programaticamente](data-factory-create-data-factories-programmatically.md).


## Planejamento e execução
Até agora você entendeu o que são pipelines e atividades. Você também viu como eles são definidos e uma exibição de alto nível das atividades no Azure Data Factory. Agora, vamos dar uma olhada em como eles são executados.

Um pipeline está ativo somente entre sua hora de início e de término. Ele não é executado antes da hora de início ou após a hora de término. Se o pipeline for pausado, ele não será executado independentemente da sua hora de início e término. Para um pipeline ser executado, ele não deve estar pausado.

Na verdade, não é o pipeline que é executado. São as atividades no pipeline que são executadas. Entretanto, elas fazem isso no contexto geral do pipeline. Consulte [Agendamento e execução](data-factory-scheduling-and-execution.md) para compreender como funciona a programação e a execução no Azure Data Factory.

## Gerenciar e monitorar  
Quando um pipeline é implantado, você pode gerenciar e monitorar seu pipeline, divisões e execuções. Leia mais sobre isso aqui: [Monitorar e gerenciar pipelines](data-factory-monior-manage-pipelines.md).

## Próximas etapas

- Noções básicas sobre [agendamento e execução no Azure Data Factory](data-factory-scheduling-and-execution.md).  
- Leia sobre o [movimento de dados](data-factory-data-movement-activities.md) e [recursos de transformação de dados](data-factory-data-transformation-activities.md) no Azure Data Factory
- Noções básicas sobre o [gerenciamento e monitoramento no Azure Data Factory](data-factory-monitor-manage-pipelines.md).
- [Criar e implantar seu primeiro pipeline](data-factory-build-your-first-pipeline.md). 


 

   













 
 


 

 

<!---HONumber=August15_HO6-->