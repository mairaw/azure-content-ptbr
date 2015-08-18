<properties 
	pageTitle="Conector do sistema de arquivos: mover dados para e do sistema de arquivos" 
	description="Saiba mais sobre o conector do sistema de arquivos para o serviço Data Factory que permite mover dados de/para o sistema de arquivos local" 
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
	ms.date="07/27/2015" 
	ms.author="spelluru"/>

# Conector do sistema de arquivos: mover dados para e do sistema de arquivos local

Este artigo descreve como você pode usar a atividade de cópia da data factory para mover dados para e do sistema de arquivos local. Este artigo se baseia no artigo [atividades de movimentação de dados](data-factory-data-movement-activities.md), que apresenta uma visão geral de movimentação de dados com a atividade de cópia e combinações de armazenamento de dados com suporte.

A data factory dá suporte à conexão com e do sistema de arquivos local por meio do Gateway de Gerenciamento de Dados. Consulte o artigo sobre [movimentação de dados entre pontos locais e na nuvem](data-factory-move-data-between-onprem-and-cloud.md) para saber mais sobre o Gateway de Gerenciamento de Dados e obter instruções passo a passo de como configurar o gateway.

**Observação:** exceto o Gateway de Gerenciamento de Dados, não é necessário instalar nenhum outro binário para se comunicar com e do sistema de arquivos local.

## Compartilhamento de arquivos do Linux 

Execute as duas etapas a seguir para usar um compartilhamento de arquivos do Linux com o serviço vinculado do servidor de arquivos:

- Instale o [Samba](https://www.samba.org/) no servidor Linux.
- Instale e configure o Gateway de Gerenciamento de Dados em um servidor Windows. Não há suporte para a instalação do gateway em um servidor Linux. 
 
## Exemplo: copiar dados de um sistema de arquivos local para um Blob do Azure

O exemplo a seguir mostra:

1.	Um serviço vinculado do tipo OnPremisesFileServer.
2.	Um serviço vinculado do tipo AzureStorage.
3.	Um conjunto de dados de entrada do tipo FileShare.
4.	Um conjunto de dados de saída do tipo AzureBlob.
4.	O pipeline com a atividade de cópia que usa FileSystemSource e BlobSink. 

O exemplo a seguir copia dados pertencentes a uma série temporal do sistema de arquivos local para o blob do Azure a cada hora. As propriedades JSON usadas nesses exemplos são descritas nas seções após os exemplos.

Como uma primeira etapa, configure o gateway de gerenciamento de dados de acordo com as instruções no artigo sobre [movimentação de dados entre pontos locais e na nuvem](data-factory-move-data-between-onprem-and-cloud.md).

**Serviço vinculado de servidor de arquivos local:**

	{
	  "Name": "OnPremisesFileServerLinkedService",
	  "properties": {
	    "type": "OnPremisesFileServer",
	    "typeProperties": {
	      "host": "\\\\Contosogame-Asia",
	      "userid": "Admin",
	      "password": "123456",
	      "gatewayName": "mygateway"
	    }
	  }
	}

**Serviço vinculado do armazenamento de Blob do Azure:**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

**Conjunto de dados de entrada do sistema de arquivos local:**

Os dados são obtidos de um novo arquivo a cada hora com o caminho e nome do arquivo refletindo a data e hora específicas com granularidade de hora.

Configurar “external”: “true” e especificar a política externalData informa ao serviço Azure Data Factory que essa a tabela é externa à data factory e não é produzida por uma atividade na data factory.

	{
	  "name": "OnpremisesFileSystemInput",
	  "properties": {
	    "type": " FileShare",
	    "linkedServiceName": " OnPremisesFileServerLinkedService ",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
	      "fileName": "{Hour}.csv",
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
	            "format": "%HH"
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

**Atividade de cópia:**

O pipeline contém uma Atividade de Cópia que está configurada para usar os conjuntos de dados de entrada e saída acima e agendada para ser executada a cada hora. Na definição JSON do pipeline, o tipo de **source** está definido como **FileSystemSource** e o tipo de **sink** está definido como **BlobSink**.
	
	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2015-06-01T18:00:00",
	    "end":"2015-06-01T19:00:00",
	    "description":"Pipeline for copy activity",
	    "activities":[  
	      {
	        "name": "OnpremisesFileSystemtoBlob",
	        "description": "copy activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "OnpremisesFileSystemInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "AzureBlobOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "FileSystemSource"
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

##Exemplo: copiar dados do SQL do Azure pra o sistema de arquivos local 

O exemplo a seguir mostra:

1.	Um serviço vinculado do tipo AzureSqlDatabase.
2.	Um serviço vinculado do tipo OnPremisesFileServer.
3.	Um conjunto de dados de entrada do tipo AzureSqlTable. 
3.	Um conjunto de dados de saída do tipo FileShare.
4.	Um pipeline com a atividade de cópia que usa SqlSource e FileSystemSink.

O exemplo copia a cada hora dados pertencentes a uma série temporal de uma tabela no banco de dados SQL do Azure para um sistema de arquivos local. As propriedades JSON usadas nesses exemplos são descritas nas seções após os exemplos.

**Serviço vinculado do SQL Azure:**

	{
	  "name": "AzureSqlLinkedService",
	  "properties": {
	    "type": "AzureSqlDatabase",
	    "typeProperties": {
	      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
	    }
	  }
	}

**Serviço vinculado de servidor de arquivos local:**

	{
	  "Name": "OnPremisesFileServerLinkedService",
	  "properties": {
	    "type": "OnPremisesFileServer",
	    "typeProperties": {
	      "host": "\\\\Contosogame-Asia",
	      "userid": "Admin",
	      "password": "123456",
	      "gatewayName": "mygateway"
	    }
	  }
	}

**Conjunto de dados de entrada do SQL Azure:**

O exemplo supõe que você criou uma tabela "MyTable" no SQL Azure e que ela contém uma coluna chamada "timestampcolumn" para dados de série temporal.

Configurar “external”: “true” e especificar a política externalData informa ao serviço Data Factory que essa a tabela é externa à data factory e não é produzida por uma atividade na data factory.

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

**Conjunto de dados de saída do sistema de arquivos local:**

Os dados são copiados para um novo arquivo a cada hora com o caminho do blob refletindo a data e hora específicas com granularidade de hora.

	{
	  "name": "OnpremisesFileSystemOutput",
	  "properties": {
	    "type": "FileShare",
	    "linkedServiceName": " OnPremisesFileServerLinkedService ",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
	      "fileName": "{Hour}.csv",
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
	            "format": "%HH"
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

**Pipeline com uma atividade de cópia:** o pipeline contém uma atividade de cópia que está configurada para usar os conjuntos de dados de entrada e saída acima e agendada para ser executada a cada hora. Na definição JSON do pipeline, o tipo de **source** está definido como **SqlSource** e o tipo de **sink** está definido como **FileSystemSink**. A consulta SQL especificada para a propriedade **SqlReaderQuery** seleciona os dados na última hora a serem copiados.

	
	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2015-06-01T18:00:00",
	    "end":"2015-06-01T20:00:00",
	    "description":"pipeline for copy activity",
	    "activities":[  
	      {
	        "name": "AzureSQLtoOnPremisesFile",
	        "description": "copy activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "AzureSQLInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "OnpremisesFileSystemOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "SqlSource",
	            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \'{0:yyyy-MM-dd}\' AND timestampcolumn < \'{1:yyyy-MM-dd}\'', WindowStart, WindowEnd)"
	          },
	          "sink": {
	            "type": "FileSystemSink"
	          }
	        },
	       "scheduler": {
	          "frequency": "Hour",
	          "interval": 1
	        },
	        "policy": {
	          "concurrency": 1,
	          "executionPriorityOrder": "OldestFirst",
	          "retry": 3,
	          "timeout": "01:00:00"
	        }
	      }
	     ]
	   }
	}

## Propriedades do serviço vinculado OnPremisesFileServer

Você pode vincular um sistema de arquivos local a uma Azure Data Factory com o serviço vinculado de servidor de arquivos local. A tabela a seguir fornece a descrição para elementos JSON específicos para o serviço vinculado do servidor de arquivos local.

Propriedade | Descrição | Obrigatório
-------- | ----------- | --------
type | A propriedade type deve ser configurada como **OnPremisesFileServer**. | Sim 
host | Nome do host do servidor. Use ' \\ ' como o caractere de escape, como no exemplo a seguir: se o compartilhamento for: \\servername, especifique \\\\servername.<p>Se o sistema de arquivos for local no computador do gateway, use localhost ou Local. Se o sistema de arquivos estiver em um servidor diferente do computador do gateway, use \\\\servername.</p> | Sim
userid | Especifique a ID do usuário que tem acesso ao servidor | Não (se você escolher encryptedcredential)
Senha | Especifique a senha para o usuário (userid) | Não (se você escolher encryptedcredential) 
Encryptedcredential | Especifique as credenciais criptografadas que você pode obter executando o cmdlet New-AzureDataFactoryEncryptValue<p>**Observação:** você deve usar o Azure PowerShell de versão 0.8.14 ou superior para usar cmdlets como New-AzureDataFactoryEncryptValue com parâmetro type definido como OnPremisesFileSystemLinkedService</p> | Não (se você optar por especificar userid e password em texto sem formatação)
Gatewayname | O nome do gateway que o serviço Data Factory deve usar para se conectar ao servidor de arquivos local | Sim

**Exemplo: usando username e password em texto sem formatação**
	
	{
	  "Name": "OnPremisesFileServerLinkedService",
	  "properties": {
	    "type": "OnPremisesFileServer",
	    "typeProperties": {
	      "host": "\\\\Contosogame-Asia",
	      "userid": "Admin",
	      "password": "123456",
	      "gatewayName": "mygateway"
	    }
	  }
	}
	
**Exemplo: usando encryptedcredential**

	{
	  "Name": " OnPremisesFileServerLinkedService ",
	  "properties": {
	    "type": "OnPremisesFileServer",
	    "typeProperties": {
	      "host": "localhost",
	      "encryptedcredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
	      "gatewayName": "mygateway"
	    }
	  }
	}

## Propriedades type do conjunto de dados de sistema de arquivos local

Para obter uma lista completa das seções e propriedades disponíveis para definição de conjuntos de dados, consulte o artigo [Criando conjuntos de dados](data-factory-create-datasets.md). Seções como structure, availability e policy de um conjunto de dados JSON são similares para todos os tipos de conjunto de dados (SQL do Azure, Blob do Azure, tabela do Azure, sistema de arquivos local etc.).

A seção typeProperties é diferente para cada tipo de conjunto de dados e fornece informações sobre o local, formato etc. de dados no armazenamento de dados. A seção typeProperties para o conjunto de dados do tipo **FileShare** tem as seguintes propriedades.

Propriedade | Descrição | Obrigatório
-------- | ----------- | --------
folderPath | Caminho para a pasta. Exemplo: myfolder<p>Use o caractere de escape ' \\ ' para caracteres especiais na cadeia de caracteres. Por exemplo: para pasta\\subpasta, especifique pasta\\subpasta e para d:\\samplefolder, especifique d:\\samplefolder.</p><p>Você pode combinar isso com **partitionBy** para ter caminhos de pasta baseados na fatia de data-horário de início/fim.</p> | Sim
fileName | Especifique o nome do arquivo no **folderPath** se você deseja que a tabela se refira a um arquivo específico na pasta. Se você não especificar nenhum valor para essa propriedade, a tabela aponta para todos os arquivos na pasta.<p>Quando fileName não for especificado para um conjunto de dados de saída, o nome do arquivo gerado estaria no seguinte formato:</p><p>Data.<Guid>. txt (por exemplo: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</p>) | Não
partitionedBy | partitionedBy pode utilizado para especificar um filename, folderPath dinâmico para dados de série temporal. Por exemplo, folderPathparametrizado para cada hora dos dados. | Não
Formatar | Há suporte para dois tipos de formatos: **TextFormat**, **AvroFormat**. Você precisa definir a propriedade type no formato para qualquer um desses valores. Quando o forAvroFormatmat é TextFormat, você pode especificar as propriedades opcionais adicionais para o formato. Consulte a seção abaixo para obter mais detalhes. | Não
fileFilter | Especifique um filtro a ser usado para selecionar um subconjunto de arquivos no folderPath em vez de todos os arquivos. <p>Os valores permitidos são: * (vários caracteres) e ? (um único caractere).</p><p>Exemplo 1: "fileFilter": "*.log"</p>Exemplo 2: "fileFilter": 2014-1-?.txt"</p><p>**Observação: fileFilter é aplicável a um conjunto de dados FileShare de entrada</p> | Não

> [AZURE.NOTE]filename e fileFilter não podem ser usados simultaneamente.

### Utilizando a propriedade partionedBy

Conforme mencionado acima, você pode especificar um filename, folderPath dinâmico para dados de série temporal com partitionedBy. Você pode fazer isso com as macros de Data Factory e as variáveis de sistema SliceStart e SliceEnd que indicam o período de tempo lógico para uma determinada fatia de dados.

Consulte os artigos [Creating datasets (Criando conjuntos de dados)](data-factory-create-datasets.md), [Scheduling & Execution (Agendamento e execução)](data-factory-scheduling-and-execution.md) e [Creating Pipelines (Criando pipelines)](data-factory-create-pipelines.md) para saber mais detalhes sobre conjuntos de dados de série temporal, agendamentos e fatias.

#### Exemplo 1:

	"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
	"partitionedBy": 
	[
	    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
	],

No exemplo acima, {Slice} é substituído pelo valor da variável de sistema SliceStart da Data Factory no formato (AAAAMMDDHH) especificado. O SliceStart refere-se à hora de início da fatia. O folderPath é diferente para cada fatia. Por exemplo: wikidatagateway/wikisampledataout/2014100103 ou wikidatagateway/wikisampledataout/2014100104.

#### Exemplo 2:

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

### Especificando TextFormat

Se o formato for definido como **TextFormat**, você pode especificar as seguintes propriedades **opcionais** na seção **Format** dentro da seção **typeProperties**.

Propriedade | Descrição | Obrigatório
-------- | ----------- | --------
columnDelimiter | Os caracteres usados como um separador de coluna em um arquivo. O valor padrão é vírgula (,). | Não
rowDelimiter | Os caracteres usados como um separador bruto no arquivo. O valor padrão é qualquer um dos seguintes: [“\\r\\n”, “\\r”,” \\n”]. | Não
escapeChar | O caractere especial usado como escape do delimitador de coluna mostrado no conteúdo. Nenhum valor padrão. Você deve especificar não mais de um caractere para essa propriedade.<p>Por exemplo, se você tiver a vírgula (,) como o delimitador de coluna, mas desejar ter o caractere de vírgula no texto (exemplo: "Hello, world"), você pode definir '$' como o caractere de escape e usar a cadeia de caracteres "Hello$, world" na fonte.</p><p>Observe que não é possível especificar escapeChar e quoteChar para uma tabela.</p> | Não
quoteChar | O caractere especial é usado como o caractere no qual colocar o valor de cadeia de caracteres. Os delimitadores de linha e coluna dos caracteres de aspas seriam tratados como parte do valor de cadeia de caracteres. Nenhum valor padrão. Você deve especificar não mais de um caractere para essa propriedade.<p>Por exemplo, se você tiver a vírgula (,) como o delimitador de coluna, mas desejar ter o caractere de vírgula no texto (exemplo: <Hello  world>), você pode definir ‘"’ como o caractere de citação e usar a cadeia de caracteres <"Hello, world"> na fonte. Essa propriedade é aplicável às tabelas de entrada e saída.</p><p>Observe que não é possível especificar escapeChar e quoteChar para uma tabela.</p> | Não
nullValue | Os caracteres usados para representar um valor nulo no conteúdo do arquivo de blob. O valor padrão é “\\N”.> | Não
encodingName | Especifique o nome de codificação. Para obter a lista de nomes de codificação válidos, consulte: Propriedade Encoding.EncodingName. <p>Por exemplo: windows-1250 ou shift\_jis. O valor padrão é: UTF-8.</p> | Não

#### Exemplos:

O exemplo a seguir mostra algumas das propriedades de formato para **TextFormat**.

	"typeProperties":
	{
	    "folderPath": "MyFolder",
	    "fileName": "MyFileName"
	    "format":
	    {
	        "type": "TextFormat",
	        "columnDelimiter": ",",
	        "rowDelimiter": ";",
	        "quoteChar": """,
	        "NullValue": "NaN"
	    }
	},

Para usar um escapeChar em vez de quoteChar, substitua a linha com quoteChar pelo seguinte:

	"escapeChar": "$",

### Especificando AvroFormat

Se o formato é definido como **AvroFormat**, não é necessário especificar nenhuma propriedade na seção Formato dentro da seção typeProperties. Exemplo:

	"format":
	{
	    "type": "AvroFormat",
	}
	
Para usar o formato Avro em uma tabela de Hive subsequente, consulte o [Tutorial do Apache Hive](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

## Propriedades de tipo da atividade de cópia do compartilhamento de arquivos

No momento, **FileSystemSource** e **FileSystemSink** não dão suporte a nenhuma propriedade.

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]








 

<!---HONumber=August15_HO6-->