<properties
	pageTitle="Crie seu primeiro pipeline usando o Azure Data Factory"
	description="Este tutorial mostra como criar um pipeline de dados de exemplo que transforma os dados usando o Azure HDInsight usando o Azure PowerShell"
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
	ms.topic="hero-article" 
	ms.date="07/27/2015"
	ms.author="spelluru"/>

# Crie seu primeiro pipeline do Azure Data Factory usando o Azure PowerShell
> [AZURE.SELECTOR]
- [Tutorial Overview](data-factory-build-your-first-pipeline.md)
- [Using Data Factory Editor](data-factory-build-your-first-pipeline-using-editor.md)
- [Using PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Using Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)


Neste artigo, você aprenderá a usar o Azure PowerShell para criar seu primeiro pipeline. Este tutorial consiste nas seguintes etapas:

1.	Criação da data factory
2.	Criação dos serviços vinculados (armazenamentos de dados, cálculos) e dos conjuntos de dados
3.	Criação do pipeline

Este artigo não fornece uma visão geral conceitual do serviço Azure Data Factory. Para obter uma visão geral detalhada do serviço, consulte o artigo [Introdução ao Azure Data Factory](data-factory-introduction.md).

## Etapa 1: Criação da data factory

Nesta etapa, é possível utilizar o PowerShell do Azure para criar uma Azure Data Factory denominada **ADFTutorialDataFactoryPSH**.

1. Inicie o **PowerShell do Azure** e execute os comandos a seguir. Mantenha o PowerShell do Azure aberto até o final deste tutorial. Se você fechá-lo e reabri-lo, será preciso executar esses comandos novamente.
	- Execute **Add-AzureAccount** e insira o nome de usuário e a senha que você usa para entrar no Portal de Visualização do Azure.  
	- Execute **Get-AzureSubscription** para exibir todas as assinaturas dessa conta.
	- Execute **Select-AzureSubscription** para selecionar a assinatura com a qual deseja trabalhar. Esta assinatura deve ser a mesma que você usou no Portal de Visualização do Azure. 
2. Alterne para o modo **AzureResourceManager**, pois os cmdlets da Azure Data Factory estão disponíveis nesse modo.

		Switch-AzureMode AzureResourceManager 
3. Crie um grupo de recursos do Azure denominado **ADFTutorialResourceGroup** executando o comando a seguir.
   
		New-AzureResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

	Algumas das etapas neste tutorial supõem que você utiliza o grupo de recursos denominado **ADFTutorialResourceGroup**. Se você utilizar um grupo de recursos diferente, será necessário usá-lo no lugar de ADFTutorialResourceGroup neste tutorial. 
4. Execute o cmdlet **New-AzureDataFactory** para criar uma data factory denominada **DataFactoryMyFirstPipelinePSH**.  

		New-AzureDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name DataFactoryMyFirstPipelinePSH –Location "West US"


	O nome da data factory do Azure deve ser globalmente exclusivo. Se você receber o erro: **O nome da data factory "DataFactoryMyFirstPipelinePSH" não está disponível**, altere o nome (por exemplo, seunomeADFTutorialDataFactoryPSH). Use esse nome em vez de ADFTutorialFactoryPSH ao executar as etapas neste tutorial.

Nas etapas subsequentes, você aprenderá a criar serviços vinculados, conjuntos de dados e o pipeline que você usará neste tutorial.

## Etapa 2: Criação de serviços vinculados e conjuntos de dados
Nesta etapa, você vinculará sua conta do Armazenamento do Azure e um cluster do Azure HDInsight sob demanda à sua data factory e, em seguida, criará um conjunto de dados para representar os dados de saída do processamento do Hive.

### Criar o serviço vinculado do armazenamento do Azure
1.	Crie um arquivo JSON chamado **StorageLinkedService.json** em **C:\\ADFGetStartedPSH** com o conteúdo a seguir. Crie a pasta ADFGetStartedPSH se ela ainda não existir.

		{
		    "name": "StorageLinkedService",
		    "properties": {
		        "type": "AzureStorage",
		        "description": "",
		        "typeProperties": {
		            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
		        }
		    }
		}

	Substitua **account name** pelo nome da sua conta de armazenamento do Azure e **account key** pela chave de acesso da sua conta de armazenamento do Azure. Para saber como obter a chave de acesso de armazenamento, consulte [Exibir, copiar e regenerar chaves de acesso de armazenamento](http://azure.microsoft.com/documentation/articles/storage-create-storage-account/#view-copy-and-regenerate-storage-access-keys)

2.	No **PowerShell do Azure**, alterne para a pasta **ADFGetStartedPSH**.
3.	É possível usar o cmdlet **New-AzureDataFactoryLinkedService** para criar um serviço vinculado. Esse cmdlet e outros cmdlets de Data Factory que você usa neste tutorial exigem que os valores sejam passados aos parâmetros **ResourceGroupName** e **DataFactoryName**. Como alternativa, é possível usar **Get-AzureDataFactory** para obter um objeto DataFactory e passar o objeto sem digitar ResourceGroupName e DataFactoryName toda vez que você executa um cmdlet. Execute o comando a seguir para atribuir a saída do cmdlet **Get-AzureDataFactory** a uma variável: **$df**. 

		$df=Get-AzureDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name DataFactoryMyFirstPipelinePSH

4.	Agora, execute o cmdlet **New-AzureDataFactoryLinkedService** para criar o serviço vinculado: **StorageLinkedService**.

		New-AzureDataFactoryLinkedService $df -File .\StorageLinkedService.json

	Se você não tivesse executado o cmdlet **Get-AzureDataFactory** e atribuído a saída à variável **$df**, você precisaria especificar valores para os parâmetros ResourceGroupName e DataFactoryName como demonstrado a seguir.
		
		New-AzureDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactoryPSH -File .\StorageLinkedService.json

	Se você fechar o PowerShell do Azure no meio do tutorial, será preciso executar o cmdlet Get-AzureDataFactory da próxima vez que iniciar o PowerShell do Azure para concluir o tutorial.

### Criar o serviço vinculado do Azure HDInsight
Agora, você criará um serviço vinculado para um cluster HDInsight sob demanda que será usado para executar o script Hive.

1.	Crie um arquivo JSON chamado **HDInsightOnDemandLinkedService.json** em **C:\\ADFGetStartedPSH** com o conteúdo a seguir. 


		{
		  "name": "HDInsightOnDemandLinkedService",
		  "properties": {
		    "type": "HDInsightOnDemand",
		    "typeProperties": {
		      "version": "3.1",
		      "clusterSize": 1,
		      "timeToLive": "00:05:00",
		      "jobsContainer": "adfjobs",
		      "linkedServiceName": "StorageLinkedService"
		    }
		  }
		}

	A tabela a seguir fornece descrições das propriedades de JSON usadas no trecho de código:
		
	Propriedade | Descrição
	-------- | -----------
	Versão | Especifica a versão do HDInsight criada como 3.1. 
	ClusterSize | Cria um cluster do HDInsight de um nó. 
	TimeToLive | Especifica tempo ocioso de cluster HDInsight antes de ser excluído.
	JobsContainer | Especifica o nome do contêiner de trabalho que será criado para armazenar os logs gerados pelo HDInsight
	linkedServiceName | Especifica a conta de armazenamento que será usada para armazenar os logs gerados pelo HDInsight
2. Execute o cmdlet **New-AzureDataFactoryLinkedService** para criar o serviço vinculado: **HDInsightOnDemandLinkedService**.

		New-AzureDataFactoryLinkedService $df -File .\HDInsightOnDemandLinkedService.json


### Criar o conjunto de dados de saída
Agora, você criará o conjunto de dados de saída para representar os dados armazenados no armazenamento de Blob do Azure.

1.	Crie um arquivo JSON denominado **OutputTable.json** na pasta **C:\\ADFGetStartedPSH** com o conteúdo a seguir:

		{
		  "name": "AzureBlobOutput",
		  "properties": {
		    "type": "AzureBlob",
		    "linkedServiceName": "StorageLinkedService",
		    "typeProperties": {
		      "folderPath": "data/partitioneddata",
		      "format": {
		        "type": "TextFormat",
		        "columnDelimiter": ","
		      }
		    },
		    "availability": {
		      "frequency": "Month",
		      "interval": 1
		    }
		  }
		}
	No trecho de JSON, você cria um conjunto de dados chamado **AzureBlobOutput** e especifica a estrutura dos dados que serão produzidos pelo script do Hive. Além disso, você especifica que os resultados são armazenados no contêiner de blob denominado **data** e na pasta chamada **partitioneddata**. A seção **availability** especifica que o conjunto de dados de saída é produzido mensalmente.

2. Execute o comando a seguir no Azure PowerShell para criar a tabela do Data Factory.

		New-AzureDataFactoryTable $df -File .\OutputTable.json

## Etapa 3: Criação do seu primeiro pipeline
Nesta etapa, você criará seu primeiro pipelines.

1.	Crie um arquivo JSON denominado **MyFirstPipelinePSH.json** na pasta **C:\\ADFGetStartedPSH** com o conteúdo a seguir: 

	> [AZURE.IMPORTANT]Substitua **storageaccountname** pelo nome da sua conta de armazenamento no JSON.

		{
		  "name": "MyFirstPipeline",
		  "properties": {
		    "description": "My first Azure Data Factory pipeline",
		    "activities": [
		      {
		        "type": "HDInsightHive",
		        "typeProperties": {
		          "scriptPath": "script/partitionweblogs.hql",
		          "scriptLinkedService": "StorageLinkedService",
		          "defines": {
		            "partitionedtable": "wasb://data@<storageaccountname>.blob.core.windows.net/partitioneddata"
		          }
		        },
		        "outputs": [
		          {
		            "name": "AzureBlobOutput"
		          }
		        ],
		        "policy": {
		          "concurrency": 1,
		          "retry": 3
		        },
		        "name": "RunSampleHiveActivity",
		        "linkedServiceName": "HDInsightOnDemandLinkedService"
		      }
		    ],
		    "start": "2014-01-01",
		    "end": "2014-01-02"
		  }
		}
		
	No trecho de JSON, você cria um pipeline que consiste de uma única atividade que usa o Hive para processar dados em um cluster HDInsight.
	
	O arquivo de script do Hive, **partitionweblogs.hql**, é armazenado na conta de armazenamento do Azure (especificada por scriptLinkedService, chamada **StorageLinkedService**) e em um contêiner chamado **script**.

	A seção **extendedProperties** é usada para especificar as configurações de tempo de execução que serão passadas para o script do hive como valores de configuração de Hive (como ${hiveconf:PartitionedData}).

	As propriedades **start** e **end** do pipeline especificam o período de atividade do pipeline.

	Na atividade de JSON, você especifica que o script do Hive seja executado na computação especificada pelo serviço vinculado –**HDInsightOnDemandLinkedService**.
2. Execute o comando a seguir para criar a tabela de Data Factory. 
	
		New-AzureDataFactoryPipeline $df -File .\MyFirstPipelinePSH.json
5. Parabéns, você criou com sucesso seu primeiro pipeline usando o Azure PowerShell!

### <a name="MonitorDataSetsAndPipeline"></a> Monitorar os conjuntos de dados e o pipeline
Nesta etapa, você usará o PowerShell do Azure para monitorar o que está acontecendo em uma Azure Data Factory.

1.	Execute **Get-AzureDataFactory** e atribua o resultado a uma variável $df.

		$df=Get-AzureDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name DataFactoryMyFirstPipelinePSH
 
2.	Execute **Get-AzureDataFactorySlice** para obter detalhes sobre todas as fatias do **EmpSQLTable**, que é a tabela de saída do pipeline.

		Get-AzureDataFactorySlice $df -TableName AzureBlobOutput -StartDateTime 2014-01-01

	Observe que o valor de StartDateTime especificado aqui é a mesma hora de início especificada no pipeline de JSON. Você deverá ver uma saída semelhante ao seguinte:

		ResourceGroupName : ADFTutorialResourceGroup
		DataFactoryName   : DataFactoryMyFirstPipelinePSH
		TableName         : AzureBlobOutput
		Start             : 1/1/2014 12:00:00 AM
		End               : 2/1/2014 12:00:00 AM
		RetryCount        : 0
		Status            : InProgress
		LatencyStatus     :
		LongRetryCount    : 0
	
3.	Executar **Get-AzureDataFactoryRun** para obter os detalhes de execuções de atividade para uma fatia **específica**.

		Get-AzureDataFactoryRun $df -TableName AzureBlobOutput -StartDateTime 2014-01-01

	Você deverá ver uma saída semelhante ao seguinte:

		Id                  : 4dbc6a07-537d-4005-a53e-6b9a4b844089_635241312000000000_635268096000000000_AzureBlobOutput
		ResourceGroupName   : ADFTutorialResourceGroup
		DataFactoryName     : DataFactoryMyFirstPipelinePSH
		TableName           : AzureBlobOutput
		ProcessingStartTime : 7/7/2015 1:14:18 AM
		ProcessingEndTime   : 12/31/9999 11:59:59 PM
		PercentComplete     : 0
		DataSliceStart      : 1/1/2014 12:00:00 AM
		DataSliceEnd        : 2/1/2014 12:00:00 AM
		Status              : AllocatingResources
		Timestamp           : 7/7/2015 1:14:18 AM
		RetryAttempt        : 0
		Properties          : {}
		ErrorMessage        :
		ActivityName        : RunSampleHiveActivity
		PipelineName        : MyFirstPipeline
		Type                : Script

	Pode continuar executando este cmdlet até ver a fatia no estado Ready ou Failed. Quando a fatia estiver no estado Ready, verifique a pasta partitioneddata no contêiner de dados em seu armazenamento de blob para os dados de saída. Observe que a criação de um cluster do HDInsight sob demanda geralmente leva algum tempo.

Consulte [Referência de cmdlet de Data Factory](https://msdn.microsoft.com/library/azure/dn820234.aspx) para obter uma documentação abrangente sobre os cmdlets de Data Factory.

 

## Próximas etapas
Neste artigo, você criou um pipeline com uma atividade de transformação (atividade do HDInsight) que executa um script Hive em um cluster do HDInsight sob demanda. Para ver como usar uma Atividade de cópia para copiar dados de um Blob do Azure para o SQL do Azure, consulte [Tutorial: Copiar dados de um blob do Azure para o SQL do Azure](./data-factory-get-started.md).
  

<!---HONumber=July15_HO5-->