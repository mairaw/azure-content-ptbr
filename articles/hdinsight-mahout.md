<properties title="Generate movie recommendations using Mahout" pageTitle="Generate movie recommendations using Mahout with Microsoft Azure HDInsight (Hadoop)" description="Learn how to use the Apache Mahout machine learning library to generate movie recommendations with HDInsight (Hadoop)" metaKeywords="Azure hdinsight mahout, Azure hdinsight machine learning, azure hadoop mahout, azure hadoop machine learning" services="hdinsight" solutions="" documentationCenter="big-data" authors="larryfr" videoId="" scriptId="" />

<tags ms.service="hdinsight" ms.workload="big-data" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="01/01/1900" ms.author="larryfr"></tags>

# Gerar recomendações de vídeo usando o Apache Mahout com o HDInsight (Hadoop)

Aprenda como usar a biblioteca de aprendizado da máquina [Apache Mahout][Apache Mahout] para gerenciar recomendações de filmes com o Microsoft Azure HDInsight (Hadoop).

> [WACOM.NOTE] Você deve ter um cluster do HDInsight para usar as informações presentes neste artigo. Para obter informações sobre como criar um, consulte [Introdução ao uso do Hadoop no HDInsight][Introdução ao uso do Hadoop no HDInsight].

> [WACOM.NOTE] O Mahout é fornecido com clusters do HDInsight 3.1. Se você estiver usando uma versão anterior do HDInsight, consulte [Instalar o Mahout][Instalar o Mahout] antes de continuar.

## <a name="learn"></a>O que você aprenderá

O Mahout é uma biblioteca de [aprendizado de máquina][aprendizado de máquina] para o Apache Hadoop. O Mahout contém algoritmos para processamento de dados, como filtragem, classificação e clustering. Neste artigo, você utilizará um mecanismo de recomendação para gerar recomendações de filmes baseadas nos vídeos que seus amigos assistiram. Você também aprenderá como realizar classificações com uma árvore de decisões. Isso ensinará a você o que é descrito a seguir.

-   Como executar trabalhos do Mahout por meio do PowerShell

-   Como executar trabalhos do Mahout por meio da linha de comando do Hadoop

-   Como instalar o Mahout em clusters do HDInsight 2.0 e 3.0

## Neste artigo

-   [Gerar recomendações usando o PowerShell][Gerar recomendações usando o PowerShell]
-   [Classificar dados usando a linha de comando do Hadoop][Classificar dados usando a linha de comando do Hadoop]
-   [Solucionar problemas][Solucionar problemas]

## <a name="recommendations"></a>Gerar recomendações usando o PowerShell

> [WACOM.NOTE] Enquanto o trabalho usado nesta seção funciona com o PowerShell, muitas das classes fornecidas com o Mahout não funcionam atualmente com o PowerShell e precisam ser executadas usando a linha de comando do Hadoop. Para obter uma lista de classes que não funcionam com o PowerShell, consulte a seção [Resolução de problemas][Solucionar problemas].
>
> Para obter um exemplo de uso da linha de comando do Hadoop para executar trabalhos no Mahout, consulte [Classificar dados usando a linha de comando do Hadoop][Classificar dados usando a linha de comando do Hadoop].

Uma das funções oferecidas pelo Mahout é um mecanismo de recomendação. Ele aceita dados no for</code>mato de `userID, itemId`, `prefValue` (os usuários escolhem o item de sua preferência). O Mahout, então, pode realizar análises de co-ocorrência, para determinar que *usuários que têm preferência por um item também têm preferência por esses outros itens*. O Mahout determinará, então, usuários com preferências de item similares, que podem ser utilizadas para fazer recomendações.

A seguir está um exemplo extremamente simples usando filmes:

-   **Co-ocorrência** - Joe, Alice e Bob, todos gostavam de *Guerra nas Estrelas*, *O Império Contra-ataca* e *O Retorno de Jedi*. O Mahout determinaria que usuários que gostam de qualquer um desses filmes também gostam dos outros dois.

-   **Co-ocorrência** - Bob e Alice também gostavam de *A Ameaça Fantasma*, *A Guerra dos Clones* e *A Vingança dos Sith*. O Mahout determinaria que usuários que gostam de qualquer um dos três filmes anteriores também gostam desses três

-   **Recomendação por similaridade** - Como Joe gostou dos primeiros três, o Mahout pesquisará os filmes que outros com preferências similares gostaram, mas que Joe não assistiu (curtiu/classificou). Nesse caso, o Mahout recomendaria *A Ameaça Fantasma*, *A Guerra dos Clones* e *A Vingança dos Sith*.

### Carregar os dados

Convenientemente, o GroupLens Research oferece [dados de classificação para filmes][dados de classificação para filmes] em um formato compatível com o Mahout.

1.  Baixe o arquivo [MovieLens 100k][MovieLens 100k], que contém 100.000 classificações de 1.000 usuários sobre 1.700 filmes.

2.  Extraia o arquivo. Ele deve conter um diretório **ml-100k**, que contém muitos arquivos de dados com o prefixo **u.**. O arquivo que será analisado pelo Mahout é **u.data**. A estrutura de dados desse arquivo é `userID`, `movieID`, `userRating` e `timestamp`. Aqui está um exemplo dos dados.

        196 242 3   881250949
        186 302 3   891717742
        22  377 1   878887116
        244 51  2   880606923
        166 346 1   886397596

3.  Carregue o arquivo **u.data** em **example/data/u.data** no seu cluster HDInsight. Se você tem o [Azure PowerShell][Azure PowerShell], pode usar o módulo do PowerShell [HDInsight-Tools][HDInsight-Tools] para carregar o arquivo. Para obter outros meios de carregar arquivos, consulte [Carregar dados para trabalhos do Hadoop no HDInsight][Carregar dados para trabalhos do Hadoop no HDInsight]. A seguir, uma demonstração do uso de `Add-HDInsightFile` para carregar o arquivo

    	PS C:\> Add-HDInsightFile -LocalPath "path\to\u.data" -DestinationPath "example/data/u.data" -ClusterName "your cluster name"

    Isso carregará o arquivo **u.data** para **example/data/u.data** no armazenamento padrão de seu cluster. Então, poderemos acessar esses dados usando o URI **wasb:///example/data/u.data** por meio de trabalhos do HDInsight.

### Executar o trabalho

Use o seguinte script do PowerShell para executar um trabalho usando o mecanismo de recomendação do Mahout com o arquivo **u.data** carregado anteriormente.

    # The HDInsight cluster name.
    $clusterName = "the cluster name"

    # The location of the Mahout jar file.
    $jarFile = "C:\apps\dist\mahout-0.9.0.2.1.3.0-1887\examples\target\mahout-examples-0.9.0.2.1.3.0-1887-job.jar"
    # NOTE: The version number portion of the file path
    # may change in future versions of HDInsight.
    # Use the following to find the location and name
    # of the Mahout jar on HDInsight 3.1, and modify
    # the $jarFile= line to point to it.
    #
    # Use-AzureHDInsightCluster -Name $clusterName
    # $jarFile = Invoke-Hive -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'
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
    $jobArguments = "-s", "SIMILARITY_COOCCURRENCE",
                    "--input", "wasb:///example/data/u.data",
                    "--output", "wasb:///example/out",
                    "--tempDir", "wasb:///temp/mahout"

    # Create the job definition
    $jobDefinition = New-AzureHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job

    # Write out any error information
    Write-Host "STDERR"
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError

> [WACOM.NOTE] Os trabalhos do Mahout não removem dados temporários criados durante o processamento do trabalho. É por isso que o parâmetro `--tempDir` é especificado no trabalho de exemplo - para isolar os arquivos temporários em um caminho específico para fácil exclusão.
>
> Para remover esses arquivos, você pode utilizar uma das ferramentas mencionadas em [Carregar dados para trabalhos do Hadoop no HDInsight][Carregar dados para trabalhos do Hadoop no HDInsight]. Ou, então, use a função `Remove-HDInsightFile` no script de PowerShell [HDInsight-Tools][HDInsight-Tools] .
>
> Se você não remover os arquivos temporários ou o arquivo de saída, receberá uma mensagem de erro se tentar executar o trabalho novamente.

O trabalho Mahout não retorna a mesma saída para o STDOUT, mas em vez disso, armazena-a no diretório de saída especificado como **part-r-00000**. Para baixar e ver o arquivo, use a função `Get-HDInsightFile` no módulo do PowerShell [HDInsight-Tools][HDInsight-Tools].

A seguir está um exemplo dos conteúdos do arquivo:

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

A primeira coluna é a `userID`. Os valores contidos em '[' e ']' são `movieId`:`recommendationScore`.

### Exibir a saída

Enquanto a saída gerada pode ser adequada para uso em um aplicativo, ela não pode ser lida com facilidade. Alguns dos outros arquivos extraídos para a pasta **ml-100k** mais cedo podem ser usados para resolver o `movieId` para um nome de filme. Enquanto há um script Python que fará isso incluso na pasta **ml-100k** (**show\_recommendations.py**), você também pode usar o script de PowerShell a seguir.

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

Para usar este script, você precisa extrair a pasta **ml-100k** anteriormente, junto com uma cópia local do arquivo de saída **part-r-00000** gerado pelo trabalho do Mahout. A seguir está um exemplo de execução do script.

	PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\ml-100k\u.data -movieFile .\ml-100k\u.item -recommendationFile .\output.txt

> [WACOM.NOTE] O script Python de exemplo, **show\_recommendations.py**, utiliza os mesmos parâmetros.

A saída deve ter aparência similar à exibida a seguir.

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

## <a name="classify"></a>Classificar dados usando a linha de comando do Hadoop

Um dos métodos de classificação disponíveis com o Mahout é criar uma [floresta aleatória][floresta aleatória]. Esse é um processo de várias etapas que envolve dados de treinamento de usuários para gerar árvores de decisões, que devem, então, ser usadas para classificar dados. Isso utiliza a classe **org.apache.mahout.classifier.df.tools.Describe** fornecida pelo Mahout e, atualmente, precisa ser executado utilizando a linha de comando do Hadoop.

### Carregar os dados

A implementação atual do Mahout é compatível com o formato de repositório da Universidade da Califórnia, Irvine (UCI) [porque isso é importante, qual é esse formato]

1.  Baixe os arquivos a seguir por meio de [][]<http://nsl.cs.unb.ca/NSL-KDD/></a>.

	-   [KDDTrain+.ARFF][KDDTrain+.ARFF] - o arquivo de treinamento

	-   [KDDTest+.ARFF][KDDTest+.ARFF] - os dados de teste

1.  Abra cada arquivo e remova as linhas na parte superior, que começam com '@', então, salve os arquivos. Se eles não forem removidos, você receberá erros ao usar esses dados com o Mahout.

2.  Carregue o arquivo para **example/data**. Você pode fazer isso usando a função `Add-HDInsightFile` no módulo do PowerShell [HDInsight-Tools][HDInsight-Tools].

### Executar o trabalho

1.  Já que esse trabalho requer a linha de comando do Hadoop, você precisa primeiro habilitar a área de trabalho remota pelo [Portal de Gerenciamento do Azure][Portal de Gerenciamento do Azure]. No portal, selecione seu cluster HDInsight, então, selecione **Habilitar Acesso Remoto** na parte inferior da página **Configurações**.

    ![habilitar acesso remoto][habilitar acesso remoto]

    Quando solicitado, insira um nome de usuário e uma senha para uso em sessões de acesso remoto.

2.  Após o acesso remoto ser estabelecido, selecione **Conectar** para iniciar a conexão. Isso baixará um arquivo **.rdp**, que poderá ser usado para iniciar uma sessão de área de trabalho remota.

    ![conectar][conectar]

3.  Após conectar-se, utilize o ícone **Linha de comando do Hadoop** para abrir a Linha de comando do Hadoop.

    ![hadoop cli][hadoop cli]

4.  Use o comando a seguir para gerar o descritor do arquivo (**KDDTrain+.info**,) usando o Mahout.

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.1.3.0-1887/examples/target/mahout-examples-0.9.0.2.1.3.0-1887-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasb:///example/data/KDDTrain+.arff" -f "wasb:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    O `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` descreve os atributos dos dados no arquivo. Um atributo numérico, dois por categoria, etc. L indica um rótulo.

5.  Crie uma floresta de árvores de decisão usando o comando a seguir.

        hadoop jar c:/apps/dist/mahout-0.9.0.2.1.3.0-1887/examples/target/mahout-examples-0.9.0.2.1.3.0-1887-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasb:///example/data/KDDTrain+.arff -ds wasb:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    A saída dessa operação é armazenada no diretório **nsl-forest**, que está localizado no armazenamento para seu cluster do HDInsight em \_\_wasb://user/\<username\>/nsl-forest/nsl-forest.seq. O \<nome de usuário\> é o nome de usuário utilizado para sua sessão de área de trabalho remota. Esse arquivo não pode ser lido por pessoas.

6.  Teste a floresta classificando o conjunto de dados **KDDTest+.arff** usando o comando a seguir.

        hadoop jar c:/apps/dist/mahout-0.9.0.2.1.3.0-1887/examples/target/mahout-examples-0.9.0.2.1.3.0-1887-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasb:///example/data/KDDTest+.arff -ds wasb:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasb:///example/data/predictions

    Esse comando retornará informações resumidas sobre o processo de classificação similares às descritas a seguir.

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

	Esse trabalho também produz um arquivo localizado em **wasb:///example/data/predictions/KDDTest+.arff.out**, no entanto, esse arquivo não pode ser lido por pessoas.

> [WACOM.NOTE] Trabalhos do Mahout não sobrescrevem arquivos. Se você deseja executar esses trabalhos novamente, é preciso excluir os arquivos criados pelos trabalhos anteriores.

## <a name="troubleshooting"></a>Solução de problemas

### <a name="install"></a>Instalar o Mahout

O Mahout é instalado nos clusters do HDInsight 3.1 e pode ser instalado manualmente em clusters 3.0 ou 2.1 usando as etapas a seguir.

1.  A versão do Mahout a ser usada depende da versão do HDInsight do seu cluster. Você pode encontrar a versão do cluster usando o seguinte com o [PowerShell do Azure][Azure PowerShell]:

    	PS C:\> Get-AzureHDInsightCluster -Name YourClusterName | Select version

	-   **Para o HDInsight 2.1**, você pode baixar um arquivo jar contendo o [Mahout 0.9][Mahout 0.9].

	-   **Para o HDInsight 3.0**, você precisa [compilar o Mahout por meio da origem][compilar o Mahout por meio da origem] e especificar a versão do Hadoop fornecida pelo HDInsight. Instale os pré-requisitos listados na página de compilação, baixe a origem e, então, use o comando a seguir para criar os arquivos .jar do Mahout.
	
            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        Once the build completes, the jar file will be created at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [WACOM.NOTE] Once Mahout 1.0 is released, you should be able to use the pre-built packages with HDInsight 3.0.

1.  Carregue o arquivo jar no **exemplo/jars**, no armazenamento padrão para seu cluster. O exemplo a seguir usa o script [send-hdinsight][sendhdinsight] para carregar o arquivo.

        PS C:PS C:\> .\Send-HDInsight -LocalPath "path\to\mahout-core-0.9-job.jar" -DestinationPath "example/jars/mahout-core-0.9-job.jar" -ClusterName "your cluster name"gt; .\Send-HDInsight -LocalPath "path\to\mahout-core-0.9-job.jar" -DestinationPath "example/jars/mahout-core-0.9-job.jar" -ClusterName "your cluster name"

### Não foi possível sobrescrever arquivos

Os trabalhos do Mahout não limpam arquivos temporários criados durante o processamento. Além disso, os trabalhos não sobrescreverão um arquivo de saída existente.

Para evitar erros ao executar trabalhos do Mahout, exclua arquivos temporários e de saída entre execuções ou, então, use nomes de diretório de saída e temporário únicos.

### Não foi possível encontrar o arquivo .jar

Enquanto o HDInsight 3.1 inclui o Mahout, o caminho e o nome de arquivo incluem o número da versão do Mahout instalado no cluster. O script PowerShell de exemplo neste tutorial usa um caminho válido a partir de julho de 2014, mas o número de versão mudará em atualizações futuras do HDInsight. Para determinar o caminho atual para o arquivo .jar do Mahout para seu cluster, use os comandos do PowerShell a seguir, então, modifique o script para fazer referência ao caminho de arquivo que ele retorna como resultado.

    Use-AzureHDInsightCluster -Name $clusterName
    $jarFile = Invoke-Hive -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

### <a name="nopowershell"></a>Classes que não funcionam com o PowerShell

Os trabalhos do Mahout que usam as classes a seguir retornarão uma série de erros, se utilizados por meio do PowerShell.

-   org.apache.mahout.utils.clustering.ClusterDumper
-   org.apache.mahout.utils.SequenceFileDumper
-   org.apache.mahout.utils.vectors.lucene.Driver
-   org.apache.mahout.utils.vectors.arff.Driver
-   org.apache.mahout.text.WikipediaToSequenceFile
-   org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
-   org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
-   org.apache.mahout.classifier.sgd.TrainLogistic
-   org.apache.mahout.classifier.sgd.RunLogistic
-   org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
-   org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
-   org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
-   org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
-   org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
-   org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
-   org.apache.mahout.classifier.df.tools.Describe

Para executar trabalhos que usam essas classes, conecte-se ao cluster do HDInsight e execute os trabalhos usando a linha de comando do Hadoop. Veja [Classificar os dados usando a linha de comando do Hadoop][Classificar dados usando a linha de comando do Hadoop] para obter um exemplo.

  [Apache Mahout]: http://mahout.apache.org
  [Introdução ao uso do Hadoop no HDInsight]: http://azure.microsoft.com/pt-br/documentation/articles/hdinsight-get-started/
  [Instalar o Mahout]: #install
  [aprendizado de máquina]: http://en.wikipedia.org/wiki/Machine_learning
  [Gerar recomendações usando o PowerShell]: #recommendations
  [Classificar dados usando a linha de comando do Hadoop]: #classify
  [Solucionar problemas]: #troubleshooting
  [dados de classificação para filmes]: http://grouplens.org/datasets/movielens/
  [MovieLens 100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
  [Azure PowerShell]: http://azure.microsoft.com/pt-br/documentation/articles/install-configure-powershell/
  [HDInsight-Tools]: https://github.com/Blackmist/hdinsight-tools
  [Carregar dados para trabalhos do Hadoop no HDInsight]: http://azure.microsoft.com/pt-br/documentation/articles/hdinsight-upload-data/
  [floresta aleatória]: http://en.wikipedia.org/wiki/Random_forest
  []: http://nsl.cs.unb.ca/NSL-KDD/
  [KDDTrain+.ARFF]: http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff
  [KDDTest+.ARFF]: http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff
  [Portal de Gerenciamento do Azure]: https://manage.windowsazure.com/
  [habilitar acesso remoto]: ./media/hdinsight-mahout/enableremote.png
  [conectar]: ./media/hdinsight-mahout/connect.png
  [hadoop cli]: ./media/hdinsight-mahout/hadoopcli.png
  [Mahout 0.9]: http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar
  [compilar o Mahout por meio da origem]: http://mahout.apache.org/developers/buildingmahout.html