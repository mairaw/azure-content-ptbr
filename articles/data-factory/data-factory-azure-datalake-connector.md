<properties
	pageTitle="Mover dados do e para o Repositório Data Lake do Azure | Azure Data Factory"
	description="Saiba como mover dados para/do Repositório Data Lake do Azure usando o Azure Data Factory"
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
	ms.topic="article"
	ms.date="10/13/2015"
	ms.author="spelluru"/>

# Mover dados para e do Repositório Data Lake do Azure usando o Azure Data Factory
Este artigo descreve como você pode usar a Atividade de Cópia em uma fábrica de dados Azure para mover dados para o Repositório Data Lake do Azure de outro repositório de dados e mover dados de outro repositório de dados para o Repositório Data Lake do Azure. Este artigo se baseia no artigo [atividades de movimentação de dados](data-factory-data-movement-activities.md), que apresenta uma visão geral de movimentação de dados com a atividade de cópia e combinações de armazenamento de dados para as quais há suporte.

> [AZURE.NOTE]Você deve criar uma conta do Repositório Data Lake do Azure antes de criar um pipeline com uma Atividade de Cópia para mover dados de ou para um repositório do Azure Data Lake. Para saber mais sobre o Repositório Data Lake do Azure, consulte [Introdução ao Repositório Data Lake do Azure](../data-lake-store/data-lake-store-get-started-portal.md).
>  
> Consulte o tutorial [Criar seu primeiro pipeline](data-factory-build-your-first-pipeline.md) para obter etapas detalhadas para criar uma fábrica de dados, serviços vinculados, conjuntos de dados e um pipeline. Use os trechos de código JSON com o Editor do Data Factory, Visual Studio ou Azure PowerShell para criar as entidades do Data Factory.

## Exemplo: copiar dados do Blob do Azure para o Repositório Data Lake do Azure
O exemplo a seguir mostra:

1.	Um serviço vinculado do tipo [AzureStorage](#azure-storage-linked-service-properties).
2.	Um serviço vinculado do tipo [AzureDataLakeStore](#azure-data-lake-linked-service-properties).
3.	Um [conjunto de dados](data-factory-create-datasets.md) de entrada do tipo [AzureBlob](#azure-blob-dataset-type-properties).
4.	Um [conjunto de dados](data-factory-create-datasets.md) de saída do tipo [AzureDataLakeStore](#azure-data-lake-dataset-type-properties).
4.	Um [pipeline](data-factory-create-pipelines.md) com Atividade de cópia que usa [BlobSource](#azure-blob-copy-activity-type-properties) e [AzureDataLakeStoreSink](#azure-data-lake-copy-activity-type-properties).

O exemplo copia os dados pertencentes a uma série temporal de um Armazenamento do Blob do Azure para um Repositório Data Lake do Azure a cada hora. As propriedades JSON usadas nesses exemplos são descritas nas seções após os exemplos.


**Serviço vinculado de armazenamento do Azure:**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

**Serviço vinculado do Azure Data Lake:**

	{
	    "name": "AzureDataLakeStoreLinkedService",
	    "properties": {
	        "type": "AzureDataLakeStore",
	        "typeProperties": {
	            "dataLakeUri": "https://<accountname>.azuredatalake.net/webhdfs/v1",
				"sessionId": "<session ID>",
	            "authorization": "<authorization URL>"
	        }
	    }
	}

### Criar um Serviço Vinculado do Azure Data Lake usando o Editor do Data Factory
O procedimento a seguir lista as etapas para criar um serviço vinculado do Repositório Data Lake do Azure usando o Editor do Data Factory.

1. Clique em **Novo repositório de dados** na barra de comandos e escolha **Repositório Data Lake do Azure**.
2. No editor de JSON, para a propriedade **datalakeUri**, insira o URI do Data Lake.
3. Clique no botão **Autorizar** na barra de comandos. Você deverá ver uma janela pop-up.

	![Botão Autorizar](./media/data-factory-azure-data-lake-connector/authorize-button.png)
4. Use as suas credenciais para entrar. Agora, a propriedade **authorization** no JSON deve ser atribuída a um valor.
5. (opcional) Especifique valores para parâmetros opcionais, como **accountName**, **subscriptionID** e **resourceGroupName** no JSON (ou) exclua essas propriedades do JSON.
6. Clique em **Implantar** na barra de comandos para implantar o serviço vinculado.


**Conjunto de dados de entrada de Blob do Azure:**

Os dados são coletados de um novo blob a cada hora (frequência: hora, intervalo: 1). O caminho de pasta e nome de arquivo para o blob são avaliados dinamicamente com base na hora de início da fatia que está sendo processada. O caminho da pasta usa parte da hora de início do dia, mês e ano e nome de arquivo usa a parte de hora da hora de início. A configuração “external”: ”true” informa o serviço Data Factory que essa é uma tabela externa à data factory e não é produzida por uma atividade na data factory.

	{
	  "name": "AzureBlobInput",
	  "properties": {
	    "type": "AzureBlob",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "Path": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
	            "format": "MM"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "dd"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "HH"
	          }
	        }
	      ]
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


**Conjunto de dados de saída do Azure Data Lake:**

O exemplo copia dados para um repositório do Azure Data Lake. Os novos dados são copiados para o repositório Data Lake a cada hora.

	{
		"name": "AzureDataLakeStoreOutput",
	  	"properties": {
			"type": "AzureDataLakeStore",
		    "linkedServiceName": " AzureDataLakeStoreLinkedService",
		    "typeProperties": {
				"folderPath": "datalake/output/"
		    },
	    	"availability": {
	      		"frequency": "Hour",
	      		"interval": 1
	    	}
	  	}
	}



**Pipeline com a Atividade de cópia:**

O pipeline contém uma Atividade de Cópia que está configurada para usar os conjuntos de dados de entrada e saída acima e agendada para ser executada a cada hora. Na definição JSON do pipeline, o tipo **source** está definido como **BlobSource** e o tipo **sink** está definido como **AzureDataLakeStoreSink**.

	{  
	    "name":"SamplePipeline",
	    "properties":
		{  
	    	"start":"2014-06-01T18:00:00",
	    	"end":"2014-06-01T19:00:00",
	    	"description":"pipeline with copy activity",
	    	"activities":
			[  
	      		{
	        		"name": "AzureBlobtoDataLake",
		        	"description": "Copy Activity",
		        	"type": "Copy",
		        	"inputs": [
		          	{
			            "name": "AzureBlobInput"
		          	}
		        	],
		        	"outputs": [
		          	{
			            "name": "AzureDataLakeStoreOutput"
		          	}
		        	],
		        	"typeProperties": {
			        	"source": {
		            		"type": "BlobSource",
			    			"treatEmptyAsNull": true,
		            		"blobColumnSeparators": ","
		          		},
		          		"sink": {
		            		"type": "AzureDataLakeStoreSink"
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

## Exemplo: copiar dados do Repositório Data Lake do Azure para o Blob do Azure
O exemplo a seguir mostra:

1.	Um serviço vinculado do tipo [AzureDataLakeStore](#azure-data-lake-linked-service-properties).
2.	Um serviço vinculado do tipo [AzureStorage](#azure-storage-linked-service-properties).
3.	Um [conjunto de dados](data-factory-create-datasets.md) de entrada do tipo [AzureDataLakeStore](#azure-data-lake-dataset-type-properties).
4.	Um [conjunto de dados](data-factory-create-datasets.md) do tipo [AzureBlob](#azure-blob-dataset-type-properties).
5.	Um [pipeline](data-factory-create-pipelines.md) com uma Atividade de cópia que usa [AzureDataLakeStoreSource](#azure-data-lake-copy-activity-type-properties) e [BlobSink](#azure-blob-copy-activity-type-properties).

O exemplo copia os dados pertencentes a uma série temporal de um repositório do Azure Data Lake para um blob do Azure a cada hora. As propriedades JSON usadas nesses exemplos são descritas nas seções após os exemplos.

**Exemplo de serviço vinculado do Repositório Data Lake do Azure:**

	{
	    "name": "AzureDataLakeStoreLinkedService",
	    "properties": {
	        "type": "AzureDataLakeStore",
	        "typeProperties": {
	            "dataLakeUri": "https://<accountname>.azuredatalake.net/webhdfs/v1",
				"sessionId": "<session ID>",
	            "authorization": "<authorization URL>"
	        }
	    }
	}

> [AZURE.NOTE]Veja as etapas no exemplo anterior para obter a URL de autorização.

**Serviço vinculado de armazenamento do Azure:**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

**Conjunto de dados de entrada do Azure Data Lake:**

Definir **"external": true** e especificar a política **externalData** informa o serviço Azure Data Factory que essa é uma tabela externa à fábrica de dados e não é produzida por uma atividade dessa fábrica de dados.

	{
		"name": "AzureDataLakeStoreInput",
	  	"properties":
		{
	    	"type": "AzureDataLakeStore",
	    	"linkedServiceName": "AzureDataLakeStoreLinkedService",
		    "typeProperties": {
				"folderPath": "datalake/input/",
            	"fileName": "SearchLog.tsv",
            	"format": {
                	"type": "TextFormat",
	                "rowDelimiter": "\n",
    	            "columnDelimiter": "\t"
    	        }
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

**Conjunto de dados de saída de Blob do Azure:**

Os dados são gravados em um novo blob a cada hora (frequência: hora, intervalo: 1). O caminho de pasta para o blob é avaliado dinamicamente com base na hora de início da fatia que está sendo processada. O caminho da pasta usa as partes ano, mês, dia e horas da hora de início.

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
	            "format": "MM"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "dd"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "HH"
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

**Pipeline com Atividade de cópia:**

O pipeline contém uma Atividade de Cópia que está configurada para usar os conjuntos de dados de entrada e saída acima e agendada para ser executada a cada hora. Na definição JSON do pipeline, o tipo **source** está definido como **AzureDataLakeStoreSource** e o tipo **sink** está definido como **BlobSink**.


	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    	"start":"2014-06-01T18:00:00",
	    	"end":"2014-06-01T19:00:00",
	    	"description":"pipeline for copy activity",
	    	"activities":[  
	      		{
	        		"name": "AzureDakeLaketoBlob",
		    	    "description": "copy activity",
		    	    "type": "Copy",
		    	    "inputs": [
		    	      {
		    	        "name": "AzureDataLakeStoreInput"
		    	      }
		    	    ],
		    	    "outputs": [
		    	      {
		    	        "name": "AzureBlobOutput"
		    	      }
		    	    ],
		    	    "typeProperties": {
		    	    	"source": {
		            		"type": "AzureDataLakeStoreSource",
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


## Propriedades do Serviço Vinculado do Repositório Data Lake do Azure

Você pode vincular uma conta de armazenamento do Azure a uma Azure Data Factory usando um serviço de armazenamento do Azure vinculado. A tabela a seguir fornece a descrição para elementos JSON específicas para o serviço de Armazenamento do Azure vinculado.

| Propriedade | Descrição | Obrigatório |
| -------- | ----------- | -------- |
| type | A propriedade type deve ser definida como: **AzureDataLakeStore** | Sim |
| dataLakeUri | Especifica informações sobre a conta do Repositório Data Lake do Azure. Ela está no seguinte formato: https://<Azure Data Lake account name>.azuredatalake.net/webhdfs/v1 | Sim |
| authorization | Clique no botão **Autorizar** no **Editor do Data Factory** e insira as suas credenciais. Isso atribui a URL de autorização gerada automaticamente a essa propriedade. | Sim |
| sessionId | A ID de sessão OAuth da sessão de autorização OAuth. Cada ID de sessão é exclusiva e pode ser usada somente uma vez. Ela é gerada automaticamente quando você usa o Editor do Data Factory. | Sim |  
| accountName | Nome de conta do Data Lake | Não |
| subscriptionId | ID de assinatura do Azure. | Não (se não for especificado, a assinatura da data factory será usada). |
| resourceGroupName | Nome do grupo de recursos do Azure | Não (se não for especificado, o grupo de recursos da data factory será usado). |


## Propriedades de tipo de Conjunto de Dados do Azure Data Lake

Para obter uma lista completa das seções e propriedades JSON disponíveis para definir conjuntos de dados, consulte o artigo [Criando conjuntos de dados](data-factory-create-datasets.md). Seções como structure, availability e policy de um conjunto de dados JSON são similares para todos os tipos de conjunto de dados (SQL Azure, Blob do Azure, Tabela do Azure etc.).

A seção **typeProperties** é diferente para cada tipo de conjunto de dados e fornece informações sobre o local, formato, etc dos dados no repositório de dados. A seção typeProperties do conjunto de dados do tipo **AzureDataLakeStore** tem as seguintes propriedades.

| Propriedade | Descrição | Obrigatório |
| -------- | ----------- | -------- |
| folderPath | Caminho para o contêiner e a pasta no repositório do Azure Data Lake. | Sim |
| fileName | <p>Nome do arquivo no repositório do Azure Data Lake. fileName é opcional. </p><p>Se você especificar um nome de arquivo, a atividade (incluindo a cópia) funciona no arquivo específico.</p><p>Quando fileName não for especificado, a cópia incluirá todos os arquivos em folderPath no conjunto de dados de entrada.</p><p>Quando fileName não for especificado para um conjunto de dados de saída, o nome do arquivo gerado estaria no seguinte formato: Data.<Guid>.txt (por exemplo: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</p> | Não |
| partitionedBy | partitionedBy é uma propriedade opcional. Você pode usá-lo para especificar um folderPath dinâmico e o nome de arquivo para dados de série temporal. Por exemplo, folderPath pode ser parametrizado para cada hora dos dados. Consulte Utilizando a seção da propriedade partitionedBy abaixo para obter detalhes e exemplos. | Não |

### Utilizando a propriedade partitionedBy
Como mencionado acima, você pode especificar um folderPath dinâmico e um nome de arquivo para os dados de série temporal com a seção **partitionedBy**, as macros do Data Factory e as variáveis de sistema SliceStart e SliceEnd, que indicam as horas de início e término para uma determinada fatia de dados.

Consulte os artigos [Criando conjuntos de dados](data-factory-create-datasets.md) e [Agendamento e Execução](data-factory-scheduling-and-execution.md) para saber mais detalhes sobre os conjuntos de dados de série temporal, agendamento e fatias.

#### Exemplo 1

	"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
	"partitionedBy":
	[
	    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
	],

No exemplo acima, {Slice} é substituído pelo valor da variável de sistema SliceStart da Data Factory no formato (AAAAMMDDHH) especificado. O SliceStart refere-se à hora de início da fatia. O folderPath é diferente para cada fatia. Por exemplo: wikidatagateway/wikisampledataout/2014100103 ou wikidatagateway/wikisampledataout/2014100104

#### Exemplo 2

	"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
	"fileName": "{Hour}.csv",
	"partitionedBy":
	 [
	    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
	    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
	    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
	    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
	],

No exemplo acima, ano, mês, dia e hora do SliceStart são extraídos em variáveis separadas que são usadas pelas propriedades folderPath e fileName.

## Propriedades de tipo de Atividade de Cópia do Azure Data Lake  
Para ver uma lista completa das seções e das propriedades disponíveis para definir atividades, consulte o artigo [Criando pipelines](data-factory-create-pipelines.md). Propriedades, como nome, descrição, tabelas de entrada e saída, várias políticas, etc. estão disponíveis para todos os tipos de atividades.

Propriedades disponíveis na seção typeProperties da atividade, por outro lado, variam de acordo com cada tipo de atividade e, no caso de atividade de cópia, variam dependendo dos tipos de fontes e coletores

**AzureDataLakeStoreSource** dá suporte à seção **typeProperties** das seguintes propriedades:

| Propriedade | Descrição | Valores permitidos | Obrigatório |
| -------- | ----------- | -------------- | -------- |
| recursiva | Indica se os dados são lidos recursivamente por meio de subpastas ou somente da pasta especificada. | True (valor padrão), False | Não |



**AzureDataLakeStoreSink** dá suporte à seção **typeProperties** das seguintes propriedades:

| Propriedade | Descrição | Valores permitidos | Obrigatório |
| -------- | ----------- | -------------- | -------- |
| copyBehavior | Especifica o comportamento da cópia. | <p>**PreserveHierarchy:** preserva a hierarquia de arquivos na pasta de destino, ou seja, o caminho relativo do arquivo de origem para a pasta de origem é idêntico ao caminho relativo do arquivo de destino para a pasta de destino.</p><p>**FlattenHierarchy:** todos os arquivos da pasta de origem estarão no primeiro nível da pasta de destino. Os arquivos de destino terão o nome gerado automaticamente.</p><p>**MergeFiles:** mescla todos os arquivos da pasta de origem em um arquivo. Se o nome do arquivo/blob for especificado, o nome do arquivo mesclado será o nome especificado; caso contrário, será o nome de arquivo gerado automaticamente.</p> | Não |


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

<!---HONumber=Nov15_HO1-->