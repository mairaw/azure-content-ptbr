<properties
	pageTitle="Desenvolver programas Java MapReduce para Hadoop | Microsoft Azure"
	description="Saiba como desenvolver programas MapReduce em Java no emulador do HDInsight e como implantá-los no HDInsight."
	services="hdinsight"
	editor="cgronlun"
	manager="paulettm"
	authors="nitinme"
	documentationCenter=""/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="Java"
	ms.topic="article"
	ms.date="07/11/2015"
	ms.author="nitinme"/>

# Desenvolver programas Java MapReduce para Hadoop no HDInsight

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-maven-mapreduce-selector.md)]

Este tutorial explica um cenário completo de desenvolvimento de um trabalho de contagem de palavras do MapReduce do Hadoop em Java usando o Apache Maven. O tutorial também mostra como testar o aplicativo no Emulador do HDInsight para Azure e depois implantá-lo e executá-lo em um cluster do HDInsight baseado no Windows.

##<a name="prerequisites"></a>Pré-requisitos

Antes de começar este tutorial, você deve ter concluído o seguinte:

- Instale o Emulador do HDInsight. Para obter instruções, consulte [Introdução ao Emulador do HDInsight][hdinsight-emulator]. Verifique se todos os serviços necessários estão em execução. No computador com o emulador do HDInsight instalado, inicie a linha de comando do Hadoop de um atalho da área de trabalho, navegue até **C:\\hdp** e execute o comando **start\_local\_hdp\_services.cmd**.
- Instale o PowerShell do Azure no computador do emulador. Para obter instruções, consulte [Instalar e configurar o PowerShell do Azure][powershell-install-configure].
- Instale a plataforma Java JDK 7 ou superior no computador emulador. Ela já está disponível no computador emulador.
- Instale e configure o [Apache Maven](http://maven.apache.org/).
- Obtenha uma assinatura do Azure. Para obter instruções, consulte [Opções de compra][azure-purchase-options], [Ofertas para membros][azure-member-offers] ou [Avaliação gratuita][azure-free-trial].


##<a name="develop"></a>Use o Apache Maven para criar um programa MapReduce em Java

Crie um aplicativo MapReduce de contagem de palavras. É um aplicativo simples que conta as ocorrências de cada palavra em um determinado conjunto de entrada. Nesta seção, realizaremos as seguintes tarefas:

1. Criar um projeto usando o Apache Maven
2. Atualizar o modelo do objeto do projeto (POM)
3. Criar um aplicativo MapReduce de contagem de palavras
4. Compilar e criar o pacote do aplicativo

**Para criar um projeto usando o Maven**

1. Crie um diretório **C:\\Tutorials\\WordCountJava**.
2. Na linha de comando em seu ambiente de desenvolvimento, mude os diretórios para o local criado.
3. Use o comando __mvn__, que é instalado com o Maven, para gerar o scaffolding para o projeto.

		mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

	Isso criará um novo diretório dentro do diretório atual, com o nome especificado pelo parâmetro __artifactID__ (**wordcountjava** neste exemplo). Esse diretório conterá os itens a seguir:

	* __pom.xml__ - o [POM (Modelo de Objeto de Projeto](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) contém informações e detalhes de configuração usados para criar o projeto.

	* __src__ - O diretório contém o diretório __main\\java\\org\\apache\\hadoop\\examples__, em que você criará o aplicativo.
3. Exclua o arquivo __src\\test\\java\\org\\apache\\hadoop\\examples\\apptest.java__, já que ele não será utilizado neste exemplo.

**Para atualizar o POM**

1. Edite o arquivo __pom.xml__ e adicione o seguinte dentro da seção `<dependencies>`:

		<dependency>
		  <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
        </dependency>
    	<dependency>
      	  <groupId>org.apache.hadoop</groupId>
      	  <artifactId>hadoop-mapreduce-client-common</artifactId>
      	  <version>2.5.1</version>
    	</dependency>
    	<dependency>
      	  <groupId>org.apache.hadoop</groupId>
      	  <artifactId>hadoop-common</artifactId>
      	  <version>2.5.1</version>
    	</dependency>

	Isso informa ao Maven que o projeto exige as bibliotecas (listadas em <artifactId>) com uma versão específica (listada em <version>). No momento da compilação, ele será baixado do repositório padrão do Maven. Você pode usar a [pesquisa de repositório do Maven](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) para exibir mais informações.

2. Adicione o conteúdo a seguir ao arquivo __pom.xml__. Isso precisa estar dentro das tags `<project>...</project>` no arquivo; por exemplo, entre `</dependencies>` e `</project>`.

		<build>
  		  <plugins>
    		<plugin>
      		  <groupId>org.apache.maven.plugins</groupId>
      		  <artifactId>maven-shade-plugin</artifactId>
      		  <version>2.3</version>
      		  <configuration>
        		<transformers>
          		  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
		          </transformer>
        		</transformers>
      		  </configuration>
      		  <executions>
        		<execution>
          		  <phase>package</phase>
          			<goals>
            		  <goal>shade</goal>
          			</goals>
        	    </execution>
      		  </executions>
      	    </plugin>
  		  </plugins>
	    </build>

	Isso configura o [Plug-in Maven Shade](http://maven.apache.org/plugins/maven-shade-plugin/), que é utilizado para evitar duplicação de licenças no arquivo JAR que é compilado pelo Maven. O motivo do uso desse recurso é que os arquivos de licença duplicados causam um erro no momento da execução no cluster do HDInsight. Utilizar o plug-in Maven Shade com a implementação `ApacheLicenseResourceTransformer` evita esse erro.

	O plug-in Maven Shade também produzirá um uberjar (às vezes chamado de fatjar), que contém todas as dependências exigidas pelo aplicativo.

3. Salve o arquivo __pom.xml__.

**Para criar o aplicativo de contagem de palavras**

1. Vá até o diretório __wordcountjava\\src\\main\\java\\org\\apache\\hadoop\\examples__ e renomeie o arquivo __app.java__ como __WordCount.java__.
2. Abra o Bloco de Notas.
2. Copie e cole o programa a seguir no Bloco de Notas:

		package org.apache.hadoop.examples;

		import java.io.IOException;
		import java.util.StringTokenizer;
		import org.apache.hadoop.conf.Configuration;
		import org.apache.hadoop.fs.Path;
		import org.apache.hadoop.io.IntWritable;
		import org.apache.hadoop.io.Text;
		import org.apache.hadoop.mapreduce.Job;
		import org.apache.hadoop.mapreduce.Mapper;
		import org.apache.hadoop.mapreduce.Reducer;
		import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
		import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
		import org.apache.hadoop.util.GenericOptionsParser;

		public class WordCount {

		  public static class TokenizerMapper
		       extends Mapper<Object, Text, Text, IntWritable>{

		    private final static IntWritable one = new IntWritable(1);
		    private Text word = new Text();

		    public void map(Object key, Text value, Context context
		                    ) throws IOException, InterruptedException {
		      StringTokenizer itr = new StringTokenizer(value.toString());
		      while (itr.hasMoreTokens()) {
		        word.set(itr.nextToken());
		        context.write(word, one);
		      }
		    }
		  }

		  public static class IntSumReducer
		       extends Reducer<Text,IntWritable,Text,IntWritable> {
		    private IntWritable result = new IntWritable();

		    public void reduce(Text key, Iterable<IntWritable> values,
		                       Context context
		                       ) throws IOException, InterruptedException {
		      int sum = 0;
		      for (IntWritable val : values) {
		        sum += val.get();
		      }
		      result.set(sum);
		      context.write(key, result);
		    }
		  }

		  public static void main(String[] args) throws Exception {
		    Configuration conf = new Configuration();
		    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
		    if (otherArgs.length != 2) {
		      System.err.println("Usage: wordcount <in> <out>");
		      System.exit(2);
		    }
		    Job job = new Job(conf, "word count");
		    job.setJarByClass(WordCount.class);
		    job.setMapperClass(TokenizerMapper.class);
		    job.setCombinerClass(IntSumReducer.class);
		    job.setReducerClass(IntSumReducer.class);
		    job.setOutputKeyClass(Text.class);
		    job.setOutputValueClass(IntWritable.class);
		    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
		    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
		    System.exit(job.waitForCompletion(true) ? 0 : 1);
		  }
		}

	Observe que o nome do pacote é **org.apache.hadoop.examples** e o nome da classe é **WordCount**. Você usará esses nomes quando enviar o trabalho MapReduce.

3. Salve o arquivo.

**Para compilar e empacotar o aplicativo**

1. Abra um prompt de comando e mude os diretórios para o diretório __wordcountjava__.

2. Use o comando a seguir para compilar um arquivo JAR contendo o aplicativo:

		mvn clean package

	Isso limpará quaisquer artefatos de compilações anteriores, baixará quaisquer dependências que ainda não tiverem sido instaladas e então compilará e criará o pacote do aplicativo.

3. Assim que o comando for concluído, o diretório __wordcountjava\\target__ conterá um arquivo chamado __wordcountjava-1.0-SNAPSHOT.jar__.

	> [AZURE.NOTE]O arquivo __wordcountjava-1.0-SNAPSHOT.jar__ é um uberjar.


##<a name="test"></a>Testar o programa no emulador

Testar o trabalho do MapReduce no emulador HDInsight inclui os seguintes procedimentos:

1. Carregar os arquivos de dados para o HDFS (Sistema de Arquivos Distribuído Hadoop) no emulador
2. Criar um grupo de usuários local
3. Executar um trabalho do MapReduce de contagem de palavras
4. Recupere os resultados do trabalho

Por padrão, o Emulador do HDInsight usa o HDFS como o sistema de arquivos. Opcionalmente, você pode configurar o Emulador do HDInsight para usar o Armazenamento de Blob do Azure. Para obter detalhes, consulte [Introdução ao Emulador do HDInsight][hdinsight-emulator-wasb].

Neste tutorial, você usará o comando **copyFromLocal** do HDFS para carregar os arquivos de dados no HDFS. A próxima seção mostra como carregar arquivos para o armazenamento de Blob do Azure usando o PowerShell do Azure. Para obter outros métodos de carregamento de arquivos no armazenamento de Blob do Azure, consulte [Carregar dados no HDInsight][hdinsight-upload-data].

Este tutorial usa a seguinte estrutura de pastas HDFS:

Pasta|Observação
---|---
/WordCount|A pasta raiz do projeto de contagem de palavras. 
/WordCount/Apps|A pasta dos executáveis do mapeador e do redutor.
/WordCount/Input|A pasta do arquivo de origem do MapReduce.
/WordCount/Output|A pasta do arquivo de saída do MapReduce.
/WordCount/MRStatusOutput|A pasta de saída do trabalho.


Este tutorial usa os arquivos .txt localizados no diretório %hadoop\_home% como os arquivos de dados.

> [AZURE.NOTE]Os comandos de HDFS do Hadoop diferenciam maiúsculas de minúsculas.

**Para copiar os arquivos de dados no emulador do HDFS**

1. Abra a linha de comando do Hadoop na área de trabalho. A linha de comando do Hadoop é instalada pelo instalador do emulador.
2. Na janela de linha de comando do Hadoop, execute o seguinte comando para criar um diretório para os arquivos de entrada:

		hadoop fs -mkdir /WordCount
		hadoop fs -mkdir /WordCount/Input

	O caminho usado aqui é o caminho relativo. É equivalente ao seguinte:

		hadoop fs -mkdir hdfs://localhost:8020/WordCount/Input

3. Execute o comando a seguir para copiar alguns arquivos de texto na pasta de entrada no HDFS:

		hadoop fs -copyFromLocal C:\hdp\hadoop-2.4.0.2.1.3.0-1981\share\doc\hadoop\common*.txt /WordCount/Input

	O trabalho MapReduce contará as palavras desses arquivos.

4. Execute o comando a seguir para listar e verificar os arquivos carregados:

		hadoop fs -ls /WordCount/Input

**Para criar um grupo de usuários local**

Para executar com sucesso o trabalho do MapReduce no cluster, você deve criar um grupo de usuários chamado hdfs. Nesse grupo, você também deve adicionar um hadoop chamado pelo usuário e um usuário local com o qual faz logon no emulador. Use os comandos a seguir de um prompt de comando elevado:

		# Add a user group called hdfs
		net localgroup hdfs /add

		# Adds a user called hadoop to the group
		net localgroup hdfs hadoop /add

		# Adds the local user to the group
		net localgroup hdfs <username> /add

**Para executar o trabalho MapReduce usando a linha de comando do Hadoop**

1. Abra a linha de comando do Hadoop na área de trabalho.
2. Execute o seguinte comando para excluir a estrutura de pastas /WordCount/Output do HDFS. /WordCount/Output é a pasta de saída do trabalho MapReduce de contagem de palavras. O trabalho MapReduce falhará se a pasta já existir. Essa etapa será necessária se esta for a segunda vez que você está executando o trabalho.

		hadoop fs -rm - r /WordCount/Output

2. Execute o comando a seguir:

		hadoop jar C:\Tutorials\WordCountJava\wordcountjava\target\wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /WordCount/Input /WordCount/Output

	Se o trabalho for concluído com êxito, você verá uma saída semelhante à seguinte captura de tela:

	![HDI.EMulator.WordCount.Run][image-emulator-wordcount-run]

	Na captura de tela, você pode ver o mapa e a redução 100% concluídos. Também lista a ID do trabalho. O mesmo relatório pode ser recuperado ao abrir o atalho **Status do MapReduce do Hadoop** em sua área de trabalho e procurar a ID do trabalho.

A outra opção para executar um trabalho MapReduce é usar o PowerShell do Azure. Para obter instruções, consulte [Introdução ao emulador do HDInsight][hdinsight-emulator].

**Para exibir a saída do HDFS**

1. Abra a linha de comando do Hadoop.
2. Execute os comandos a seguir para exibir a saída:

		hadoop fs -ls /WordCount/Output/
		hadoop fs -cat /WordCount/Output/part-r-00000

	Você pode acrescentar "|more" no final do comando para obter a exibição da página. Ou usar o comando **findstr** para localizar um padrão de cadeia de caracteres:

		hadoop fs -cat /WordCount/Output/part-r-00000 | findstr "there"

Você agora desenvolveu um trabalho de contagem de palavras MapReduce e o testou com sucesso no emulador. A próxima etapa é implantá-lo e executá-lo no Azure HDInsight.



##<a id="upload"></a>Carregar os dados e o aplicativo no armazenamento de Blob do Azure
O Azure HDInsight usa o armazenamento de Blob do Azure para armazenar dados. Quando um cluster HDInsight é provisionado, um contêiner de armazenamento de Blob do Azure é usado para armazenar os arquivos do sistema. Você pode usar esse contêiner padrão ou outro contêiner (na mesma conta de armazenamento do Azure ou em outra conta de armazenamento localizada no mesmo data center que o cluster) para armazenar os arquivos de dados.

Neste tutorial, você criará um contêiner em uma conta de armazenamento separada para os arquivos de dados e o aplicativo MapReduce. Os arquivos de dados são arquivos de texto no diretório **C:\\hdp\\hadoop-2.4.0.2.1.3.0-1981\\share\\doc\\hadoop\\common** em sua estação de trabalho do emulador.

**Para criar um contêiner e uma conta de armazenamento de Blob**

1. Abra o PowerShell do Azure.
2. Defina as variáveis e execute os comandos:

		$subscriptionName = "<AzureSubscriptionName>"
		$storageAccountName_Data = "<AzureStorageAccountName>"  
		$containerName_Data = "<ContainerName>"
		$location = "<MicrosoftDataCenter>"  # For example, "East US"

	A variável **$subscripionName** está associada à sua assinatura do Azure. Você deve atribuir um nome a **$storageAccountName\_Data** e **$containerName\_Data**. Para as restrições de nomes, consulte [Nomenclatura e referência a contêineres, Blobs e metadados](http://msdn.microsoft.com/library/windowsazure/dd135715.aspx).

3. Execute os seguintes comandos para criar uma conta do Armazenamento e um contêiner de armazenamento de Blob na conta:

		# Select an Azure subscription
		Select-AzureSubscription $subscriptionName

		# Create a Storage account
		New-AzureStorageAccount -StorageAccountName $storageAccountName_Data -location $location

		# Create a Blob storage container
		$storageAccountKey = Get-AzureStorageKey $storageAccountName_Data | %{ $_.Primary }
		$destContext = New-AzureStorageContext –StorageAccountName $storageAccountName_Data –StorageAccountKey $storageAccountKey  
		New-AzureStorageContainer -Name $containerName_Data -Context $destContext

4. Execute os seguintes comandos para verificar a conta e o contêiner do Armazenamento:

		Get-AzureStorageAccount -StorageAccountName $storageAccountName_Data
		Get-AzureStorageContainer -Context $destContext

**Para carregar os arquivos de dados**

1. Abra o PowerShell do Azure.
2. Defina as primeiras três variáveis e execute os comandos:

		$subscriptionName = "<AzureSubscriptionName>"
		$storageAccountName_Data = "<AzureStorageAccountName>"  
		$containerName_Data = "<ContainerName>"

		$localFolder = "C:\hdp\hadoop-2.4.0.2.1.3.0-1981\share\doc\hadoop\common"
		$destFolder = "WordCount/Input"

	As variáveis **$storageAccountName\_Data** e **$containerName\_Data** são as mesmas que você definiu no último procedimento.

	Observe que a pasta do arquivo de origem é **c:\\Hadoop\\hadoop-1.1.0-SNAPSHOT**, e que a pasta de destino é **WordCount/Input**.

3. Execute os comandos a seguir para obter uma lista dos arquivos .txt na pasta do arquivo de origem:

		# Get a list of the .txt files
		$filesAll = Get-ChildItem $localFolder
		$filesTxt = $filesAll | where {$_.Extension -eq ".txt"}

4. Execute os comandos a seguir para criar um objeto de contexto de armazenamento:

		# Create a storage context object
		Select-AzureSubscription $subscriptionName
		$storageaccountkey = get-azurestoragekey $storageAccountName_Data | %{$_.Primary}
		$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName_Data -StorageAccountKey $storageaccountkey

5. Execute os comandos a seguir para copiar os arquivos:

		# Copy the files from the local workstation to the Blob container
		foreach ($file in $filesTxt){

		    $fileName = "$localFolder\$file"
		    $blobName = "$destFolder/$file"

		    write-host "Copying $fileName to $blobName"

		    Set-AzureStorageBlobContent -File $fileName -Container $containerName_Data -Blob $blobName -Context $destContext
		}

6. Execute os seguintes comandos para listar os arquivos carregados:

		# List the uploaded files in the Blob storage container
        Write-Host "The Uploaded data files:" -BackgroundColor Green
		Get-AzureStorageBlob -Container $containerName_Data -Context $destContext -Prefix $destFolder

	Você deve ver os arquivos de dados de texto carregados.

**Para carregar o aplicativo de contagem de palavras**

1. Abra o PowerShell do Azure.
2. Defina as primeiras três variáveis e execute os comandos:

		$subscriptionName = "<AzureSubscriptionName>"
		$storageAccountName_Data = "<AzureStorageAccountName>"  
		$containerName_Data = "<ContainerName>"

		$jarFile = "C:\Tutorials\WordCountJava\wordcountjava\target\wordcountjava-1.0-SNAPSHOT.jar"
		$blobFolder = "WordCount/jars"

	As variáveis **$storageAccountName\_Data** e o **$containerName\_Data** são as mesmas que você definiu no último procedimento, o que significa que você carregará o arquivo de dados e o aplicativo no mesmo contêiner da mesma conta de armazenamento.

	Observe que a pasta de destino é **WordCount/jars**.

4. Execute os comandos a seguir para criar um objeto de contexto de armazenamento:

		# Create a storage context object
		Select-AzureSubscription $subscriptionName
		$storageaccountkey = get-azurestoragekey $storageAccountName_Data | %{$_.Primary}
		$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName_Data -StorageAccountKey $storageaccountkey

5. Execute o comando a seguir para copiar os aplicativos:

		Set-AzureStorageBlobContent -File $jarFile -Container $containerName_Data -Blob "$blobFolder/WordCount.jar" -Context $destContext

6. Execute os seguintes comandos para listar os arquivos carregados:

		# List the uploaded files in the Blob storage container
        Write-Host "The Uploaded application file:" -BackgroundColor Green
		Get-AzureStorageBlob -Container $containerName_Data -Context $destContext -Prefix $blobFolder

	Você deve consultar o arquivo JAR listado aqui.

##<a name="run"></a>Executar o trabalho do MapReduce no Azure HDInsight

Nesta seção, você criará um script do PowerShell do Azure que realize as seguintes tarefas:

1. Provisiona um cluster HDInsight

	1. Criar uma conta do Armazenamento que será usada como o sistema de arquivos padrão do cluster HDInsight
	2. Crie um contêiner de armazenamento de Blob
	3. Crie um cluster HDInsight

2. Envia o trabalho de MapReduce

	1. Crie uma definição do trabalho MapReduce
	2. Envie um trabalho MapReduce
	3. Aguarde a conclusão do trabalho
	4. Exiba o erro padrão
	5. Exiba a saída padrão

3. Exclui o cluster

	1. Exclua o cluster HDInsight
	2. Exclua a conta do Armazenamento usada como sistema de arquivos de cluster HDInsight padrão


**Para executar o script do PowerShell do Azure**

1. Abra o Bloco de Notas.
2. Copie e cole o seguinte código:

		# The Storage account and the HDInsight cluster variables
		$subscriptionName = "<AzureSubscriptionName>"
		$stringPrefix = "<StringForPrefix>"
		$location = "<MicrosoftDataCenter>"     ### Must match the data Storage account location
		$clusterNodes = <NumberOFNodesInTheCluster>

		$storageAccountName_Data = "<TheDataStorageAccountName>"
		$containerName_Data = "<TheDataBlobStorageContainerName>"

		$clusterName = $stringPrefix + "hdicluster"

		$storageAccountName_Default = $stringPrefix + "hdistore"
		$containerName_Default =  $stringPrefix + "hdicluster"

		# The MapReduce job variables
		$jarFile = "wasb://$containerName_Data@$storageAccountName_Data.blob.core.windows.net/WordCount/jars/WordCount.jar"
		$className = "org.apache.hadoop.examples.WordCount"
		$mrInput = "wasb://$containerName_Data@$storageAccountName_Data.blob.core.windows.net/WordCount/Input/"
		$mrOutput = "wasb://$containerName_Data@$storageAccountName_Data.blob.core.windows.net/WordCount/Output/"
		$mrStatusOutput = "wasb://$containerName_Data@$storageAccountName_Data.blob.core.windows.net/WordCount/MRStatusOutput/"

		# Create a PSCredential object. The user name and password are hardcoded here. You can change them if you want.
		$password = ConvertTo-SecureString "Pass@word1" -AsPlainText -Force
		$creds = New-Object System.Management.Automation.PSCredential ("Admin", $password)

		Select-AzureSubscription $subscriptionName

		#=============================
		# Create a Storage account used as the default file system
		Write-Host "Create a storage account" -ForegroundColor Green
		New-AzureStorageAccount -StorageAccountName $storageAccountName_Default -location $location

		#=============================
		# Create a Blob storage container used as the default file system
		Write-Host "Create a Blob storage container" -ForegroundColor Green
		$storageAccountKey_Default = Get-AzureStorageKey $storageAccountName_Default | %{ $_.Primary }
		$destContext = New-AzureStorageContext –StorageAccountName $storageAccountName_Default –StorageAccountKey $storageAccountKey_Default

		New-AzureStorageContainer -Name $containerName_Default -Context $destContext

		#=============================
		# Create an HDInsight cluster
		Write-Host "Create an HDInsight cluster" -ForegroundColor Green
		$storageAccountKey_Data = Get-AzureStorageKey $storageAccountName_Data | %{ $_.Primary }

		$config = New-AzureHDInsightClusterConfig -ClusterSizeInNodes $clusterNodes |
		    Set-AzureHDInsightDefaultStorage -StorageAccountName "$storageAccountName_Default.blob.core.windows.net" -StorageAccountKey $storageAccountKey_Default -StorageContainerName $containerName_Default |
		    Add-AzureHDInsightStorage -StorageAccountName "$storageAccountName_Data.blob.core.windows.net" -StorageAccountKey $storageAccountKey_Data

		New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $creds -Config $config

		#=============================
		# Create a MapReduce job definition
		Write-Host "Create a MapReduce job definition" -ForegroundColor Green
		$mrJobDef = New-AzureHDInsightMapReduceJobDefinition -JobName mrWordCountJob  -JarFile $jarFile -ClassName $className -Arguments $mrInput, $mrOutput -StatusFolder /WordCountStatus

		#=============================
		# Run the MapReduce job
		Write-Host "Run the MapReduce job" -ForegroundColor Green
		$mrJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $mrJobDef
		Wait-AzureHDInsightJob -Job $mrJob -WaitTimeoutInSeconds 3600

		Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $mrJob.JobId -StandardError
		Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $mrJob.JobId -StandardOutput

		#=============================
		# Delete the HDInsight cluster
		Write-Host "Delete the HDInsight cluster" -ForegroundColor Green
		Remove-AzureHDInsightCluster -Name $clusterName  

		# Delete the default file system Storage account
		Write-Host "Delete the storage account" -ForegroundColor Green
		Remove-AzureStorageAccount -StorageAccountName $storageAccountName_Default

3. Defina as primeiras seis variáveis no script. A variável **$stringPrefix** é usada como prefixo da cadeia especificada para o nome do cluster HDInsight, o nome da conta de armazenamento e o nome do contêiner de armazenamento de Blob. Como esses nomes devem ter de 3 a 24 caracteres, garanta que a cadeia de caracteres que você especificar e os nomes que esse script usa, juntos, não excedam o limite de caracteres para o nome. Você deve usar letras minúsculas para **$stringPrefix**.

	As variáveis **$storageAccountName\_Data** e **$containerName\_Data** são a conta e o contêiner de armazenamento usados para armazenar os arquivos de dados e os aplicativos. A variável **$location** deve corresponder ao local da conta de armazenamento de dados.

4. Verifique o restante das variáveis.
5. Salve o arquivo de script.
6. Abra o PowerShell do Azure.
7. Execute o seguinte comando para definir a política de execução para RemoteSigned:

		PowerShell -File <FileName> -ExecutionPolicy RemoteSigned

8. Quando solicitado, insira o nome de usuário e a senha do cluster HDInsight. Como você excluirá o cluster no final do script e você não precisará mais do nome de usuário e da senha, o nome de usuário e a senha podem ser qualquer cadeia de caracteres. Se você não desejar que as credenciais sejam solicitadas, consulte [Trabalhando com senhas, cadeias de caracteres e credenciais seguras no Windows PowerShell][powershell-PSCredential].


##<a name="retrieve"></a>Recuperar a saída do trabalho de MapReduce
Esta seção mostra como baixar e exibir a saída. Para obter informações sobre como exibir os resultados no Excel, consulte [Conectar o Excel ao HDInsight com o Driver ODBC do Microsoft Hive][hdinsight-ODBC] e [Conectar o Excel ao HDInsight com o Power Query][hdinsight-power-query].


**Para recuperar a saída**

1. Abra a janela do PowerShell do Azure.
2. Altere o diretório para **C:\\Tutorials\\WordCountJava**. A pasta padrão do PowerShell do Azure é **C:\\Windows\\System32\\WindowsPowerShell\\v1.0**. Os cmdlets que você executará baixarão o arquivo de saída na pasta atual. Você não tem permissões para baixar os arquivos nas pastas do sistema.
2. Execute os comandos a seguir para definir os valores:

		$subscriptionName = "<AzureSubscriptionName>"
		$storageAccountName_Data = "<TheDataStorageAccountName>"
		$containerName_Data = "<TheDataBlobStorageContainerName>"
		$blobName = "WordCount/Output/part-r-00000"

3. Execute os seguintes comandos para criar um objeto de contexto de armazenamento do Azure:

		Select-AzureSubscription $subscriptionName
		$storageAccountKey = Get-AzureStorageKey $storageAccountName_Data | %{ $_.Primary }
		$storageContext = New-AzureStorageContext –StorageAccountName $storageAccountName_Data –StorageAccountKey $storageAccountKey  

4. Execute os comandos a seguir para baixar e exibir a saída:

		Get-AzureStorageBlobContent -Container $containerName_Data -Blob $blobName -Context $storageContext -Force
		cat "./$blobName" | findstr "there"

Depois que o trabalho for concluído, você terá as opções de exportar os dados para SQL Server ou para o Banco de dados SQL do Azure usando [Sqoop][hdinsight-use-sqoop] ou de exportar os dados para o Excel.

##<a id="nextsteps"></a>Próximas etapas
Neste tutorial, você aprendeu a desenvolver um trabalho MapReduce em Java, a testar o aplicativo no emulador do HDInsight e a gravar um script do Azure PowerShell para provisionar um cluster HDInsight e executar um trabalho do MapReduce no cluster. Para saber mais, consulte os seguintes artigos:

- [Desenvolver programas MapReduce de streaming do Hadoop em C# para o HDInsight][hdinsight-develop-streaming]
- [Introdução ao Azure HDInsight][hdinsight-get-started]
- [Introdução ao Emulador do HDInsight][hdinsight-emulator]
- [Usar o armazenamento de Blob do Azure com o HDInsight][hdinsight-storage]
- [Administrar o HDInsight usando o PowerShell do Azure][hdinsight-admin-powershell]
- [Carregar dados no HDInsight][hdinsight-upload-data]
- [Usar o Hive com o HDInsight][hdinsight-use-hive]
- [Usar o Pig com o HDInsight][hdinsight-use-pig]
- [Conectar o Excel ao HDInsight com o Power Query][hdinsight-power-query]
- [Conectar o Excel ao HDInsight com o driver ODBC do Microsoft Hive][hdinsight-ODBC]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-develop-streaming]: hdinsight-hadoop-develop-deploy-streaming-jobs.md

[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-emulator]: ../hdinsight-get-started-emulator.md
[hdinsight-emulator-wasb]: ../hdinsight-get-started-emulator.md#blobstorage
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: ../hdinsight-use-blob-storage.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx
[powershell-install-configure]: ../install-configure-powershell.md



[image-emulator-wordcount-compile]: ./media/hdinsight-develop-deploy-java-mapreduce/HDI-Emulator-Compile-Java-MapReduce.png
[image-emulator-wordcount-run]: ./media/hdinsight-develop-deploy-java-mapreduce/HDI-Emulator-Run-Java-MapReduce.png

<!---HONumber=Oct15_HO3-->