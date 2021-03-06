<properties
	pageTitle="Gerar recomendações usando o Mahout e o HDInsight baseado em Windows | Microsoft Azure"
	description="Saiba como usar a biblioteca de aprendizado de máquina do Apache Mahout para gerar recomendações de filmes com o HDInsight para Windows (Hadoop)."
	services="hdinsight"
	documentationCenter=""
	authors="Blackmist"
	manager="paulettm"
	editor="cgronlun"
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="11/03/2015"
	ms.author="larryfr"/>

#Gerar recomendações de vídeo usando o Apache Mahout com o Hadoop no HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Saiba como usar a biblioteca de aprendizado de máquina do [Apache Mahout](http://mahout.apache.org) com o Azure HDInsight para gerar recomendações de vídeos.

> [AZURE.NOTE]As etapas deste documento exigem um cliente do Windows e um cluster HDInsight baseado em Windows. Para obter informações sobre como usar o Mahout em um Linux, OS X ou cliente Unix, com um cluster do HDInsight baseado em Linux, consulte [Gerar recomendações de filmes usando o Apache Mahout com Hadoop baseado em Linux no HDInsight](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>O que você aprenderá

O Mahout é uma biblioteca de [aprendizado de máquina][ml] para o Apache Hadoop. O Mahout contém algoritmos para processamento de dados, como filtragem, classificação e clustering. Neste artigo, você utilizará um mecanismo de recomendação para gerar recomendações de filmes baseadas nos vídeos que seus amigos assistiram. Você também aprenderá como realizar classificações com uma árvore de decisões. Isso ensinará a você o que é descrito a seguir.

* Como executar trabalhos do Mahout usando o Windows PowerShell

* Como executar trabalhos do Mahout por meio da linha de comando do Hadoop

* Como instalar o Mahout em clusters do HDInsight 3.0 e 2.0

	> [AZURE.NOTE]O Mahout é fornecido com a versão HDInsight 3.1 dos clusters. Se você estiver usando uma versão anterior do HDInsight, consulte [Instalar o Mahout](#install) antes de continuar.

##pré-requisitos

- **Um cluster Hadoop baseado em Windows no HDInsight**. Para obter informações sobre como criar um, consulte [Introdução ao uso do Hadoop no HDInsight][getstarted]
- **Uma estação de trabalho com o PowerShell do Azure**. Veja [Instalar o Azure PowerShell 1.0 e superior](hdinsight-administer-use-powershell.md#install-azure-powershell-10-and-greater).


##<a name="recommendations"></a>Gerar recomendações usando o Windows PowerShell

> [AZURE.NOTE]Enquanto o trabalho usado nesta seção funciona pelo uso do Windows PowerShell, muitas das classes fornecidas com o Mahout não funcionam atualmente com o Windows PowerShell e precisam ser executadas usando a linha de comando do Hadoop. Para obter uma lista de classes que não funcionam com o Windows PowerShell, consulte a seção [Solucionar problemas](#troubleshooting).
>
> Para obter um exemplo de uso da linha de comando do Hadoop para executar trabalhos no Mahout, consulte [Classificar dados usando a linha de comando do Hadoop](#classify).

Uma das funções oferecidas pelo Mahout é um mecanismo de recomendação. Ele aceita dados no formato de `userID`, `itemId` e `prefValue` (os usuários escolhem o item de sua preferência). O Mahout, então, pode realizar análises de coocorrência, para determinar que _usuários que têm preferência por um item também têm preferência por esses outros itens_. O Mahout determinará, então, usuários com preferências de item similares, que podem ser utilizadas para fazer recomendações.

A seguir está um exemplo extremamente simples usando filmes:

* __Coocorrência__: Joe, Alice e Bob, todos gostaram de _Guerra nas Estrelas_, _O Império Contra-ataca_ e _O Retorno de Jedi_. O Mahout determina que usuários que gostam de qualquer um desses filmes também gostam dos outros dois.

* __Coocorrência__: Bob e Alice também gostaram de _A Ameaça Fantasma_, _A Guerra dos Clones_ e _A Vingança dos Sith_. O Mahout determina que usuários que gostam de qualquer um dos três filmes citados anteriormente também gostam destes últimos três.

* __Recomendação por similaridade__: já que Joe gostou dos primeiros três, o Mahout pesquisará os filmes que outros com preferências similares gostaram, mas que Joe não assistiu (curtiu/classificou). Nesse caso, o Mahout recomendaria _A Ameaça Fantasma_, _A Guerra dos Clones_ e _A Vingança dos Sith_.

###Carregar os dados

Convenientemente, a [GroupLens Research][movielens] oferece dados de classificação para filmes em um formato compatível com o Mahout.

1. Baixe o arquivo [MovieLens 100k][100k], que contém 100.000 classificações de 1.700 filmes feitas por 1.000 usuários.

2. Extraia o arquivo. Ele deve conter um diretório __ml-100k__, que contém muitos arquivos de dados com o prefixo __u.__. O arquivo que será analisado pelo Mahout é __u.data__. A estrutura de dados desse arquivo é `userID`, `movieID`, `userRating` e `timestamp`. Aqui está um exemplo dos dados:


		196	242	3	881250949
		186	302	3	891717742
		22	377	1	878887116
		244	51	2	880606923
		166	346	1	886397596


3. Carregue o arquivo __u.data__ em __example/data/u.data__ no seu cluster HDInsight. O comando a seguir usa PowerShell para carregar os dados. Para obter outros meios de carregar arquivos, consulte [Carregar dados para trabalhos do Hadoop no HDInsight][upload].

        # Put your cluster name below
        $clusterName="Your HDInsight cluster name"
        # Put the path to the u.data file below
        $fileToUpload="The path to the u.data file"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup `
            | %{ $_.Key1 }
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/data/u.data" `
            -Container $container `
            -Context $context
    
    Isso carregará o arquivo __u.data__ para __example/data/u.data__ no armazenamento padrão em seu cluster. Então, você poderá acessar esses dados usando o URI \_\___wasb:///example/data/u.data__ de trabalhos do HDInsight.

###Executar o trabalho

Use o script do Windows PowerShell a seguir para executar um trabalho usando o mecanismo de recomendação do Mahout com o arquivo __u.data__ carregado anteriormente:

	# The HDInsight cluster name.
	$clusterName = "the cluster name"
    
    #Get HTTPS/Admin credentials for submitting the job later
    $creds = Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup `
        | %{ $_.Key1 }
            
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
            
	# NOTE: The version number portion of the file path
	# may change in future versions of HDInsight.
	$jarFile = "file:///C:/apps/dist/mahout-0.9.0.2.2.7.1-33/examples/target/mahout-examples-0.9.0.2.2.7.1-33-job.jar"
    #
	# If you are using an earlier version of HDInsight,
	# set $jarFile to the jar file you
	# uploaded.
	# For example,
	# $jarFile = "wasb:///example/jars/mahout-core-0.9-job.jar"

	# The arguments for this job
	# * input - the path to the data uploaded to HDInsight
	# * output - the path to store output data
	# * tempDir - the directory for temp files
	$jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
	                "--input", "wasb:///example/data/u.data",
	                "--output", "wasb:///example/out",
	                "--tempDir", "wasb:///example/temp"

	# Create the job definition
	$jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
	  -JarFile $jarFile `
	  -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
	  -Arguments $jobArguments

	# Start the job
	$job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

	# Wait on the job to complete
	Write-Host "Wait for the job to complete ..." -ForegroundColor Green
	Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
	# Write out any error information
	Write-Host "STDERR"
	Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE]Os trabalhos do Mahout não removem dados temporários criados durante o processamento do trabalho. O parâmetro `--tempDir` é especificado no trabalho de exemplo para isolar os arquivos temporários em um caminho específico.

O trabalho do Mahout não retorna a saída para STDOUT. Em vez disso, ele armazena no diretório de saída especificado como __part-r-00000__. O script baixa esse arquivo em __output.txt__ no diretório atual em sua estação de trabalho.

Veja a seguir um exemplo do conteúdo do arquivo:

	1	[234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
	2	[282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
	3	[284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
	4	[690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

A primeira coluna é a `userID`. Os valores contidos em '[' and ']' são `movieId`:`recommendationScore`.

###Exibir a saída

Apesar de a saída gerada talvez ser adequada para uso em um aplicativo, ela não pode ser lida com facilidade. Alguns dos arquivos extraídos para a pasta __ml-100k__ anteriormente podem ser usados para resolver `movieId` para um nome de filme, que é o que faz o seguinte script do PowerShell:

	<#
	.SYNOPSIS
	    Displays recommendations for movies.
	.DESCRIPTION
	    Displays recommendations generated by Mahout
	    with HDInsight example in a human readable format.
	.EXAMPLE
	    .\Show-Recommendation -userId 4
	        -userDataFile "u.data"
	        -movieFile "u.item"
	        -recommendationFile "output.txt"
	#>

	[CmdletBinding(SupportsShouldProcess = $true)]
	param(
	    #The user ID
	    [Parameter(Mandatory = $true)]
	    [String]$userId,

	    [Parameter(Mandatory = $true)]
	    [String]$userDataFile,

	    [Parameter(Mandatory = $true)]
	    [String]$movieFile,

	    [Parameter(Mandatory = $true)]
	    [String]$recommendationFile
	)
	# Read movie ID & description into hash table
	Write-Host "Reading movies descriptions" -ForegroundColor Green
	$movieById = @{}
	foreach($line in Get-Content $movieFile)
	{
	    $tokens = $line.Split("|")
	    $movieById[$tokens[0]] = $tokens[1]
	}
	# Load movies user has already seen (rated)
	# into a hash table
	Write-Host "Reading rated movies" -ForegroundColor Green
	$ratedMovieIds = @{}
	foreach($line in Get-Content $userDataFile)
	{
	    $tokens = $line.Split("`t")
	    if($tokens[0] -eq $userId)
	    {
	        # Resolve the ID to the movie name
	        $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
	    }
	}
	# Read recommendations generated by Mahout
	Write-Host "Reading recommendations" -ForegroundColor Green
	$recommendations = @{}
	foreach($line in get-content $recommendationFile)
	{
	    $tokens = $line.Split("`t")
	    if($tokens[0] -eq $userId)
	    {
	        #Trim leading/treailing [] and split at ,
	        $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
	        foreach($movieIdAndScore in $movieIdAndScores)
	        {
	            #Split at : and store title and score in a hash table
	            $idAndScore = $movieIdAndScore.Split(":")
	            $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
	        }
	        break
	    }
	}

	Write-Host "Rated movies" -ForegroundColor Green
	Write-Host "---------------------------" -ForegroundColor Green
	$ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
	               @{Expression={$_.Value};Label="Rating"}
	$ratedMovieIds | format-table $ratedFormat
	Write-Host "---------------------------" -ForegroundColor Green

	write-host "Recommended movies" -ForegroundColor Green
	Write-Host "---------------------------" -ForegroundColor Green
	$recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
	                        @{Expression={$_.Value};Label="Score"}
	$recommendations | format-table $recommendationFormat

Para usar esse script, você já deve ter extraído a pasta __ml-100k__. A seguir está um exemplo de execução do script:

	PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\ml-100k\u.data -movieFile .\ml-100k\u.item -recommendationFile .\output.txt

A saída deve ter aparência similar à exibida a seguir:

	Reading movies descriptions
	Reading rated movies
	Reading recommendations
	Rated movies
	---------------------------
	Movie                                    Rating
	-----                                    ------
	Devil's Own, The (1997)                  1
	Alien: Resurrection (1997)               3
	187 (1997)                               2
	(lines ommitted)

	---------------------------
	Recommended movies
	---------------------------

	Movie                                    Score
	-----                                    -----
	Good Will Hunting (1997)                 4.6504064
	Swingers (1996)                          4.6862745
	Wings of the Dove, The (1997)            4.6666665
	People vs. Larry Flynt, The (1996)       4.834559
	Everyone Says I Love You (1996)          4.707071
	Secrets & Lies (1996)                    4.818182
	That Thing You Do! (1996)                4.75
	Grosse Pointe Blank (1997)               4.8235292
	Donnie Brasco (1997)                     4.6792455
	Lone Star (1996)                         4.7099237  

##<a name="classify"></a>Classificar dados usando a linha de comando do Hadoop

Um dos métodos de classificação disponíveis com o Mahout é criar uma [floresta aleatória][forest]. Esse é um processo de várias etapas que envolve dados de treinamento de usuários para gerar árvores de decisões, que devem, então, ser usadas para classificar dados. Ele utiliza a classe __org.apache.mahout.classifier.df.tools.Describe__ fornecida pelo Mahout. Atualmente, ele deve ser executado usando a linha de comando do Hadoop.

###Carregar os dados

1. Baixe os arquivos a seguir por meio do [Conjunto de Dados NDL-KDD](http://nsl.cs.unb.ca/NSL-KDD/).

  * [KDDTrain+.ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): o arquivo de treinamento

  * [KDDTest+.ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): os dados de teste

2. Abra cada arquivo e remova as linhas no topo que começam com '@' e, em seguida, salve os arquivos. Se eles não forem removidos, você receberá mensagens de erro ao usar esses dados com o Mahout.

2. Carregue os arquivos para __example/data__. Faça isso usando o script a seguir. Substitua __NOMEDOCLUSTER__ pelo nome do cluster HDInsight. Substitua NOMEDOARQUIVO pelo nome do arquivo a ser carregado.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup `
            | %{ $_.Key1 }
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context

###Executar o trabalho

1. Este trabalho requer a linha de comando do Hadoop. Habilite a área de trabalho remota para o cluster HDInsight e conecte-se a ele seguindo as instruções em [Conectar aos clusters HDInsight usando o RDP](hdinsight-administer-use-management-portal.md#rdp).

3. Após conectar-se, utilize o ícone __Linha de Comando do Hadoop__ para abrir a Linha de comando do Hadoop:

	![hadoop cli][hadoopcli]

3. Use o comando a seguir para gerar o descritor do arquivo (__KDDTrain+.info__), que usa o Mahout.

		hadoop jar "c:/apps/dist/mahout-0.9.0.2.1.3.0-1887/examples/target/mahout-examples-0.9.0.2.1.3.0-1887-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasb:///example/data/KDDTrain+.arff" -f "wasb:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

	O `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` descreve os atributos dos dados no arquivo. Por exemplo, L indica um rótulo.

4. Crie uma floresta de árvores de decisão usando o comando a seguir:

		hadoop jar c:/apps/dist/mahout-0.9.0.2.1.3.0-1887/examples/target/mahout-examples-0.9.0.2.1.3.0-1887-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasb:///example/data/KDDTrain+.arff -ds wasb:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    A saída dessa operação está armazenada no diretório __nsl-forest__, que está localizado no armazenamento de seu cluster HDInsight em \_\___wasb://user/&lt;username>/nsl-forest/nsl-forest.seq. O &lt;username> é o nome de usuário usado para a sua sessão da área de trabalho remota. Esse arquivo não pode ser lido por pessoas.

5. Teste a floresta classificando o conjunto de dados __KDDTest+.arff__. Use o seguinte comando:

    	hadoop jar c:/apps/dist/mahout-0.9.0.2.1.3.0-1887/examples/target/mahout-examples-0.9.0.2.1.3.0-1887-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasb:///example/data/KDDTest+.arff -ds wasb:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasb:///example/data/predictions

    Esse comando retornará informações resumidas sobre o processo de classificação similares às descritas a seguir:

	    14/07/02 14:29:28 INFO mapreduce.TestForest:

	    =======================================================
	    Summary
	    -------------------------------------------------------
	    Correctly Classified Instances          :      17560       77.8921%
	    Incorrectly Classified Instances        :       4984       22.1079%
	    Total Classified Instances              :      22544

	    =======================================================
	    Confusion Matrix
	    -------------------------------------------------------
	    a       b       <--Classified as
	    9437    274      |  9711        a     = normal
	    4710    8123     |  12833       b     = anomaly

	    =======================================================
	    Statistics
	    -------------------------------------------------------
	    Kappa                                       0.5728
	    Accuracy                                   77.8921%
	    Reliability                                53.4921%
	    Reliability (standard deviation)            0.4933

  Esse trabalho também produz um arquivo localizado em \_\___wasb:///example/data/predictions/KDDTest+.arff.out__. Todavia, esse arquivo não pode ser lido por pessoas.

> [AZURE.NOTE]Trabalhos do Mahout não sobrescrevem arquivos. Se você deseja executar esses trabalhos novamente, é preciso excluir os arquivos criados pelos trabalhos anteriores.

##<a name="troubleshooting"></a>Solucionar problemas

###<a name="install"></a>Instalar o Mahout

O Mahout é instalado nos clusters do HDInsight 3.1 e pode ser instalado manualmente em clusters 3.0 ou 2.1, usando as etapas a seguir:

1. A versão do Mahout a ser usada depende da versão do HDInsight do seu cluster. Você pode encontrar a versão do cluster exibindo as propriedades do cluster no portal do Azure.

  * __Para HDInsight 2.1__, você pode baixar um arquivo do arquivo JAR (Java) que contém o [Mahout 0.9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar).

  * __Para o HDInsight 3.0__, você precisa [compilar o Mahout por meio da origem][build] e especificar a versão do Hadoop fornecida pelo HDInsight. Instale os pré-requisitos listados na página de compilação, baixe a origem e, então, use o comando a seguir para criar os arquivos .jar do Mahout:

			mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

    	Após a conclusão da compilação, você encontrará o arquivo JAR em __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__. 

    	> [AZURE.NOTE] No lançamento do Mahout 1.0, você poderá usar os pacotes de pré-compilação com o HDInsight 3.0.

2. Carregue o arquivo jar no __exemplo/jars__, no armazenamento padrão para seu cluster. Substitua CLUSTERNAME no script a seguir pelo nome do cluster HDInsight e substitua FILENAME pelo caminho até o arquivo __mahout-coure-0.9-job.jar__.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup `
            | %{ $_.Key1 }
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###Não foi possível sobrescrever arquivos

Os trabalhos do Mahout não limpam arquivos temporários criados durante o processamento. Além disso, os trabalhos não sobrescreverão um arquivo de saída existente.

Para evitar erros ao executar trabalhos do Mahout, exclua arquivos temporários e de saída entre execuções, ou então use nomes de diretório de saída e temporário únicos.

###Não foi possível encontrar o arquivo JAR

Clusters de HDInsight 3.1 incluem o Mahout. O caminho e o nome de arquivo incluem o número de versão do Mahout instalado no cluster. O script de exemplo do Windows PowerShell neste tutorial usa um caminho válido desde novembro de 2015, mas o número de versão mudará em atualizações futuras do HDInsight. Para determinar o caminho atual para o arquivo JAR do Mahout para seu cluster, use os comandos do Windows PowerShell a seguir, então, modifique o script para fazer referência ao caminho de arquivo que ele retorna como resultado:

	Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup `
            | %{ $_.Key1 }
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasb:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target*-job.jar'

###<a name="nopowershell"></a>Classes que não funcionam com o Windows PowerShell

Os trabalhos do Mahout que usam as classes a seguir retornarão uma série de erros, se utilizados por meio do Windows PowerShell:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Para executar trabalhos que usam essas classes, conecte-se ao cluster do HDInsight e execute os trabalhos usando a linha de comando do Hadoop. Veja [Classificar os dados usando a linha de comando do Hadoop](#classify) para obter um exemplo.

##Próximas etapas

Agora que você aprendeu como usar o Mahout, descubra outras maneiras de trabalhar com dados no HDInsight:

* [Hive com o HDInsight](../hadoop-use-hive.md)
* [Pig com o HDInsight](../hadoop-use-pig.md)
* [MapReduce com o HDInsight](../hadoop-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: ../hdinsight-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 

<!---HONumber=AcomDC_1203_2015-->
