<properties
	pageTitle="Notas de versão do HDInsight | Azure"
	description="Notas de versão do HDInsight."
	services="hdinsight"
	documentationCenter=""
	editor="cgronlun"
	manager="paulettm"
	authors="nitinme"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/27/2015"
	ms.author="nitinme"/>


#Notas de versão do Microsoft HDInsight

## Notas da versão de 27/04/2015 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795 - inalterado)
* HDInsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117 - inalterado)
* HDInsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329 - inalterado)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

Esta versão contém as atualizações a seguir.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Área afetada (por exemplo, serviço, componente ou SDK)</p></th>
<th>Tipo de cluster (por exemplo, Hadoop, HBase ou Storm)</th>
<th>JIRA (se aplicável)</th>
</tr>


<tr>
<td>Corrigir a dependência de DLL</td>
<td>Remove a dependência de HDInsight na estrutura de teste de unidade.</td>
<td>.</td>
<td>O Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Correção de bug para condição de corrida</td>
<td>Uma solicitação Criar cluster agora aguarda que a solicitação PUT seja aceita antes de realizar a sondagem do status</td>
<td>.</td>
<td>O Hadoop</td>
<td>N/D</td>
</tr>
</table>

## Notas da versão de 14/04/2015 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - inalterado)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - inalterado)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - inalterado)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6

Esta versão contém as atualizações a seguir.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Área afetada (por exemplo, serviço, componente ou SDK)</p></th>
<th>Tipo de cluster (por exemplo, Hadoop, HBase ou Storm)</th>
<th>JIRA (se aplicável)</th>
</tr>


<tr>
<td>Correções de bug Tez</td>
<td>Correções para Apache TEZ 2214 e TEZ 1923 estão incluídas nesta versão do HDI 3.2. Estas são necessárias especificamente para determinadas consultas Hive em Tez que requerem a movimentação de uma quantidade significativa de dados.
</td>
<td>HDP</td>
<td>O Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## Notas da versão de 06/04/2015 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - inalterado)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - inalterado)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - inalterado)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6

Esta versão contém as atualizações a seguir.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Área afetada (por exemplo, serviço, componente ou SDK)</p></th>
<th>Tipo de cluster (por exemplo, Hadoop, HBase ou Storm)</th>
<th>JIRA (se aplicável)</th>
</tr>


<tr>
<td>SDK 1.5.6 do .NET do HDInsight</td>
<td>Atualizações para remover algumas classes internas para HDInsight no Linux.</td>
<td>.</td>
<td>O Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Biblioteca de Avro 1.5.6</td>
<td>Adicionado <b>KnownTypeAttribute</b> para o método <b>GetAllKnownTypes</b>. Corrigida NullReferenceException quando um tipo é null para o método GetAllKnownTypes.</td>
<td>.</td>
<td>O Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Correções de bug</td>
<td>Correções de bug diversas para o serviço</td>
<td>O Barramento de</td>
<td>Todos</td>
<td>N/D</td>
</tr>

</table>
<br>

## Notas da versão de 01/04/2015 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

Esta versão contém as atualizações a seguir.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Área afetada (por exemplo, serviço, componente ou SDK)</p></th>
<th>Tipo de cluster (por exemplo, Hadoop, HBase ou Storm)</th>
<th>JIRA (se aplicável)</th>
</tr>


<tr>
<td>Capacidade de habilitar/desabilitar as credenciais da área de trabalho remota em clusters do Windows via SDK do .NET</td>
<td>Suporte programático para habilitar ou desabilitar as credenciais RDP em clusters do Windows.</td>
<td>.</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>Capacidade de habilitar credenciais de área de trabalho remota em clusters enquanto eles estão sendo provisionados</td>
<td>Suporte programático para habilitar as credenciais de área de trabalho remota enquanto o cluster está sendo criado. Isso remove o processo de duas etapas para provisionar primeiro o cluster e, em seguida, habilitar a área de trabalho remota.</td>
<td>.</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>Python atualizado para 2.7.8</td>
<td>Python atualizado em Clusters HDInsight para Python 2.7.8, que contém algumas correções importantes de segurança para versões 2.1, 3.0, 3.1 e 3.2 do HDInsight</td>
<td>O Barramento de</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>Alteração de configuração do YARN</td>
<td>Configuração YARN yarn.resourcemanager.max-completed-applications alterada para 1.000 para todos os tipos de clusterpar HDInsight versões 3.1 e 3.2. Esse valor controla apenas a lista de aplicativos concluídos na interface de usuário YARN. Para obter informações sobre os aplicativos que foram enviados antes da lista de aplicativos mostrada na interface do usuário, você pode ir diretamente para o Servidor de Histórico.</td>
<td>YARN</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>Redimensionamento de nós em um cluster HBase</td>
<td>Clusters HBase permitem redimensionamento de nós (aumento e redução) para as versões 3.1 e 3.2 do HDInsight</td>
<td>O Barramento de</td>
<td>HBase</td>
<td>N/D</td>
</tr>

<tr>
<td>Atualização do JDBC</td>
<td>O driver JDBC do SQL é atualizado para a versão sqljdbc_4.0.2206.100 para o HDInsight versão 3.2. Essa versão contém aprimoramentos de segurança importantes.</td>
<td>HDP</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>Atualização da configuração da JVM</td>
<td>Configuração networkaddress.cache.ttl da JVM atualizada para 300 segundos do valor padrão de -1 para HDInsight versões 3.1 e 3.2. Esse valor de configuração controla a política de caching para pesquisas de nome bem-sucedidas por meio do serviço de nome. Isso corrige um bug relacionado ao aumento e redução de clusters HBase.</td>
<td>O Barramento de</td>
<td>HBase</td>
<td>N/D</td>
</tr>

<tr>
<td>Atualização para SDK de Armazenamento do Azure para Java 2.0</td>
<td>O HDInsight versão 3.2 é atualizado para usar a versão mais recente do SDK do armazenamento do Azure para Java. Ela contém várias correções de bug importantes em relação à versão 0.6.0 atual.</td>
<td>HDP</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>Atualizado para o código-fonte WASB do Apache</td>
<td>O HDInsight versão 3.2 agora usa a versão mais recente de código para o driver de sistema de arquivos WASB do Apache Hadoop. Com essa alteração, o driver WASB agora é empacotado como um jar separado. Isso é apenas uma alteração de empacotamento e não contém quaisquer alterações ao comportamento do driver WASB. O nome deste arquivo JAR é hadoop-azure-2.6.0.jar.</td>
<td>HDP</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>Atualizações de nome de arquivo JAR no HDInsight 3.2</td>
<td>Esta atualização para HDInsight versão 3.2 contém várias correções de bugs e alguns jars internos, empacotados como parte do HDP, foram atualizados. Observe que esses arquivos JAR são internos ao pacote HDP e não para uso direto por aplicativos cliente. Aplicativos devem empacotar sua própria versão dos JARs para que uma atualização para os JARs internos HDP não interrompa os aplicativos cliente.</td>
<td>HDP</td>
<td>Todos</td>
<td>N/D</td>
</tr>

</table>
<br>

## Notas da versão de 03/03/2015 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351 - inalterado)
* HDInsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097 - inalterado)
* HDInsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290 - inalterado)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-2340 - inalterado)
* SDK 1.5.0 (inalterado)

Esta versão contém a seguinte atualização.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Área afetada (por exemplo, serviço, componente ou SDK)</p></th>
<th>Tipo de cluster (por exemplo, Hadoop, HBase ou Storm)</th>
<th>JIRA</th>
</tr>


<tr>
<td>Aprimoramentos de confiabilidade</td>
<td>Fizemos correções que permitem melhor escalabilidade do serviço com o aumento de carga relacionado à criação de cluster.</td>
<td>O Barramento de</td>
<td>Todos</td>
<td>N/D</td>
</tr>



</table>
<br>

## Notas da versão de 18/02/2015 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351 - inalterado)
* HDInsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097 - inalterado)
* HDInsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290 - inalterado)
* HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0-2340)
* SDK 1.5.0

Esta versão contém as atualizações a seguir.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Área afetada (por exemplo, serviço, componente ou SDK)</p></th>
<th>Tipo de cluster (por exemplo, Hadoop, HBase ou Storm)</th>
<th>JIRA (se aplicável)</th>
</tr>


<tr>
<td>Clusters do HDInsight 3.2</td>
<td>O Hadoop 2.6/HDP2.2 está disponível com clusters HDInsight 3.2. Ele contém atualizações importantes para todos os componentes de código-fonte aberto. Para obter mais detalhes, consulte <a href="http://azure.microsoft.com/documentation/articles/hdinsight-component-versioning/" target="_blank">o que há de novo no HDInsight</a> e <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">notas de versão do HDP 2.2.0.0</a>.</td>
<td>Software livre</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>HDInsight no Linux (Visualização)</td>
<td>Clusters podem ser implantados em execução no Ubuntu Linux. Para obter mais detalhes, consulte <a href="http://azure.microsoft.com/documentation/articles/hdinsight-hadoop-linux-get-started/" target ="_blank">Introdução ao HDInsight no Linux</a>.</td>
<td>O Barramento de</td>
<td>O Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Disponibilidade geral do Storm</td>
<td>Clusters do Apache tempestade estão geralmente disponíveis. Para obter mais detalhes, consulte <a href="http://azure.microsoft.com/documentation/articles/hdinsight-storm-getting-started/" target="_blank">Introdução ao uso do Storm no Linux</a>.</td>
<td>O Barramento de</td>
<td>Storm</td>
<td>N/D</td>
</tr>

<tr>
<td>Tamanhos de máquina virtual</td>
<td>HDInsight do Azure está disponível em mais tipos e tamanhos de máquina virtual. Os clusters do HDInsight podem utilizar tamanhos de A2 a A7 criados para fins gerais; os nós da Série D têm SSDs (unidades de estado sólido) e processadores 60% mais rápidos; nos tamanhos A8 e A9, há suporte para InfiniBand para rede rápida.</td>
<td>O Barramento de</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>Dimensionamento de cluster</td>
<td>Você pode alterar o número de nós de dados para um cluster HDInsight em execução sem precisar excluí-lo ou recriá-lo. Atualmente, somente os tipos de cluster Hadoop Query e Apache Storm têm essa capacidade, mas o Apache HBase logo deverá ter também. Para saber mais, confira <a href="http://azure.microsoft.com/documentation/articles/hdinsight-hadoop-cluster-scaling/" target="_blank">Dimensionamento de clusters no HDInsight</a>.</td>
<td>O Barramento de</td>
<td>Hadoop, Storm</td>
<td>N/D</td>
</tr>

<tr>
<td>Ferramentas do Visual Studio</td>
<td>Além de ferramentas completas para o Apache Storm, as ferramentas para Apache Hive no Visual Studio foram atualizadas para incluir o preenchimento de declaração, validação local e suporte aprimorado de depuração. Para obter mais informações, consulte <a href="http://azure.microsoft.com/documentation/articles/hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">Introdução ao uso das ferramentas do Hadoop HDInsight para Visual Studio</a>.</td>
<td>Ferramentas</td>
<td>O Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Conector de Hadoop para Banco de Dados de Documentos</td>
<td>Com o conector de Hadoop para Banco de Dados de Documentos, você pode executar agregações complexas, análise e manipulações de seus documentos JSON sem esquema armazenados nas coleções de Banco de Dados de Documentos ou entre contas de banco de dados. Para obter mais informações e um tutorial, consulte <a href="http://azure.microsoft.com/documentation/articles/documentdb-run-hadoop-with-hdinsight/" target="_blank">Executar trabalhos do Hadoop usando Banco de Dados de Documentos e HDInsight</a>.</td>
<td>O Barramento de</td>
<td>O Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Correções de bug</td>
<td>Fizemos várias correções de bug secundárias para serviços HDInsight. Não se espera nenhuma alteração no comportamento voltado ao cliente.</td>
<td>O Barramento de</td>
<td>Todos</td>
<td>N/D</td>
</tr>

</table>
<br>

## Notas da versão de 06/02/2015 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351 - inalterado)
* HDInsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097 - inalterado)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SDK N/D

Esta versão contém as atualizações a seguir.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Área afetada (por exemplo, serviço, componente ou SDK)</p></th>
<th>Tipo de cluster (por exemplo, Hadoop, HBase ou Storm)</th>
<th>JIRA (se aplicável)</th>
</tr>


<tr>
<td>Correções de bug</td>
<td>Fizemos várias correções de bug secundárias para serviços HDInsight. Não se espera nenhuma alteração no comportamento voltado ao cliente.</td>
<td>O Barramento de</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>Atualização de manutenção HDP 2.1</td>
<td>O HDInsight 3.1 é atualizado para implantar o HDP 2.1.10.0. Para obter mais informações, consulte <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">Notas de versão do HDP-2.1.10</a>. </td>
<td>Software livre</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>Atualizações binárias HDP</td>
<td>Existem alguns arquivos JAR no HBase para os quais os nomes de arquivo foram atualizados. Esses arquivos JAR são usados internamente pelo HBase, portanto, não se espera que os clientes tenham uma dependência nos nomes desses arquivos JAR. Estão incluídos:
<ul>
<li>./lib/jetty-6.1.26.hwx.jar</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.jar</li>
<li>./lib/jetty-util-6.1.26.hwx.jar</li>
</ul>
</td>
<td>Software livre</td>
<td>HBase</td>
<td>N/D</td>
</tr>

</table>
<br>

## Notas da versão de 29/01/2015 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351 - inalterado)
* HDInsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097 - inalterado)
* HDInsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196 - inalterado)
* SDK N/D

Esta versão contém a seguinte atualização.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Área afetada (por exemplo, serviço, componente ou SDK)</p></th>
<th>Tipo de cluster (por exemplo, Hadoop, HBase ou Storm)</th>
<th>JIRA (se aplicável)</th>
</tr>


<tr>

<td>Correções de bug</td>
<td>Fizemos algumas correções de bugs importantes que melhoram a confiabilidade dos Clusters HDInsight durante as atualizações do Azure.</td>
<td>O Barramento de</td>
<td>Todos</td>
<td>N/D</td>
</tr>



</table>
<br>

## Notas da versão de 05/01/2015 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351 - inalterado)
* HDInsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097 - inalterado)
* HDInsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196 - inalterado)


Esta versão contém as atualizações a seguir.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Componente</th>
<th>Tipo de cluster</th>
<th>JIRA (se aplicável)</th>
</tr>


<tr>
<td>Exemplos para análise de tendência do Twitter e recomendações de vídeos baseadas no Mahout</td>
<td><p>Nesta versão, o console de consulta do HDInsight tem dois exemplos adicionais:</p>

<p><b>Análise de tendências do Twitter</b><br>
APIs públicas fornecidas por sites, como o Twitter, são uma fonte útil de dados para analisar e compreender as tendências populares. Neste tutorial, Aprenda a usar o Hive para obter uma lista dos usuários do Twitter que enviaram a maioria dos tweets que contêm uma palavra específica. </p>

<p><b>Recomendação de vídeos do Mahout</b><br>
O Apache Mahout é uma biblioteca de aprendizado de máquina de Apache Hadoop. O Mahout contém algoritmos para processamento de dados (como filtragem, classificação e clustering). Neste tutorial, você utilizará um mecanismo de recomendação para gerar recomendações de filmes baseadas nos vídeos que seus amigos assistiram.</p></td>
<td>Console de consulta</td>
<td>O Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Mude para o valor padrão para configuração de Hive: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Essa configuração de tamanho aplica-se a associações de mapa convertidas automaticamente. O valor representa a soma dos tamanhos das tabelas que podem ser convertidas em hashmaps que cabem na memória. Em uma versão anterior, esse valor aumentava do valor padrão de 10 MB para 128 MB. No entanto, o novo valor de 128 MB estava causando falhas nos trabalhos devido à falta de memória. Esta versão reverte o valor padrão de volta para 10 MB. Os clientes ainda podem optar por substituir esse valor durante a criação do cluster, considerando suas consultas e tamanhos de tabela. Para obter mais informações sobre essa configuração e sobre como substituí-la, consulte <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">Otimizar conversão de junção automática</a> na documentação da Hortonworks. </p></td>
<td>Hive</td>
<td>Hadoop, HBase</td>
<td>N/D</td>
</tr>

</table>
<br>

## Notas da versão de 23/12/2014 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.10.420.1246783 (versão HDP inalterada)
* HDInsight 3.0.6.420.1246783 (versão HDP inalterada)
* HDInsight 3.1.1.420.1246783 (versão HDP inalterada)

Esta versão contém a seguinte atualização.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Componente</th>
<th>Tipo de cluster</th>
<th>JIRA (se aplicável)</th>
</tr>


<tr>
<td>Falhas intermitentes de criação de Cluster causadas por excesso de carga</td>
<td><p>Algoritmo aprimorado para baixar os pacotes HDP durante a criação do cluster permite a manipulação mais robusta de falhas causadas por excesso de carga.</p></td>
<td>O Barramento de</td>
<td>Hadoop, Hbase, Storm</td>
<td>N/D</td>
</tr>



</table>
<br>

## Notas da versão de 18/12/2014 do HDInsight ##

Esta versão contém a atualização de componentes a seguir.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Componente</th>
<th>Tipo de cluster</th>
<th>JIRA (se aplicável)</th>
</tr>

<tr>
<td><a href = "http://azure.microsoft.com/documentation/articles/hdinsight-hadoop-customize-cluster/" target="_blank">Disponibilidade geral de personalização de cluster</a></td>
<td><p>A personalização oferece a capacidade de personalizar os clusters de HDInsight do Azure com projetos disponíveis por meio do ecossistema do Apache Hadoop. Com esse novo recurso, agora você pode testar e implantar projetos de Hadoop no HDInsight do Azure. Isso é habilitado por meio do recurso **Ação de Script**, que pode modificar clusters Hadoop de modos arbitrários usando scripts personalizados. Esse recurso de personalização está disponível em todos os tipos de clusters do HDInsight, incluindo Hadoop, HBase e Storm. Para demonstrar o poder dessa funcionalidade, documentamos o processo para instalar os populares módulos <a href = "http://azure.microsoft.com/documentation/articles/hdinsight-hadoop-spark-install/" target="_blank">Spark</a>, <a href = "http://azure.microsoft.com/documentation/articles/hdinsight-hadoop-r-scripts/" target="_blank">R</a>, <a href = "http://azure.microsoft.com/documentation/articles/hdinsight-hadoop-solr-install/" target="_blank">Solr</a> e <a href = "http://azure.microsoft.com/documentation/articles/hdinsight-hadoop-giraph-install/" target="_blank">Giraph</a>.&#160;Esta versão também adiciona a funcionalidade para os clientes especificarem suas ações de script personalizadas por meio do Portal do Azure, fornece diretrizes e práticas recomendadas de como criar ações de script personalizadas usando métodos auxiliares e fornece diretrizes sobre como testar a ação de script. </p></td>
<td>Disponibilidade geral dos recursos</td>
<td>Todos</td>
<td>N/D</td>
</tr>


</table>
<br>

## Notas da versão de 21/11/2014 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* SDK do HDInsight N/D

Esta versão contém as seguintes atualizações de componentes.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Componente</th>
<th>Tipo de cluster</th>
<th>JIRA (se aplicável)</th>
</tr>

<tr>
<td>Correção de bug: erro intermitente ao adicionar grande número de partições a uma tabela na DDL do Hive </td>
<td><p>Se existir um erro de conexão intermitente com o banco de dados do metastore do Hive ao adicionar diversas partições a uma tabela Hive, a DDL do Hive poderá falhar. A seguinte instrução será vista no log de erros do Hive se essa falha ocorrer: </p><p>"ERROR [main]: ql.Driver (SessionState.java:printError(547)) - FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. MetaException(message:java.lang.RuntimeException: commitTransaction was called but openTransactionCalls = 0. This probably indicates that there are unbalanced calls to openTransaction/commitTransaction)"</p></td>
<td>Hive</td>
<td>Hadoop, HBase</td>
<td>HIVE-482 (Este é um JIRA interno de modo que não é possível fazer um orçamento externamente. Mencionado aqui para referência.)</td>
</tr>

<tr>
<td>Correção de bug: interrupção de funcionamento ocasional no console de consulta do HDInsight</td>
<td>Quando isso acontece, a seguinte instrução pode ser vista no log do WebHCat para o trabalho do inicializador do WebHCat: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): Não foi possível carregar arquivo de histórico {url wasb para o arquivo de histórico}"</p></td>
<td>WebHCat</td>
<td>O Hadoop</td>
<td>HIVE-482 (Este é um JIRA interno de modo que não é possível fazer um orçamento externamente. Mencionado aqui para referência.)</td>
</tr>

<tr>
<td>Correção de bug: pico ocasional na latência das consultas do HBase</td>
<td>Se isso acontecer, os usuários notarão um pico ocasional de 3 segundos na latências das consultas do HBase.&#160;</td>
<td>Gateway do cluster HDInsight</td>
<td>HBase</td>
<td>N/D</td>
</tr>

<tr>
<td>Mudanças de nomes dos arquivos JAR HDP</td>
<td>Para HDI cluster versão 3.0, há algumas alterações para os arquivos JAR internos instalados por HDP. jetty-6.1.26.jar foi substituído por jetty-6.1.26.hwx.jar. jetty-util-6.1.26.jar foi substituído pelo jetty-util-6.1.26.hwx.jar. Essas mudanças se aplicam aos projetos do Hadoop, Mahout, WebHCat e Oozie.</td>
<td>Hadoop, Mahout, WebHCat, Oozie</td>
<td>Hadoop, HBase</td>
<td>N/D</td>
</tr>

</table>
<br>


## Notas da versão de 21/11/2014 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.9.382.1169709 (sem alteração desde 14/11/2014)
* HDInsight 3.0.5.382.1169709 (sem alteração desde 14/11/2014)
* HDInsight 3.1.1.382.1169709 (sem alteração desde 14/11/2014)
* SDK do HDINsight 1.4.0

Esta versão contém as seguintes atualizações de componentes.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Componente</th>
<th>Tipo de cluster</th>
<th>JIRA (se aplicável)</th>
</tr>

<tr>
<td>Acesso a logs de aplicativos</td>
<td>Capacidade para enumerar de modo programático aplicativos que foram executados em seus clusters e para baixar logs relevantes de aplicativos ou de contêineres específicos para ajudar a depurar aplicativos problemáticos.</td>
<td>.</td>
<td>O Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Capacidade para especificar o nome da região no IHdInsightClient.DeleteCluster </td>
<td>O SDK do Azure HDInsight fornece a capacidade de especificar um nome de região ao usar o **DeleteCluster**. Isso ajuda a desbloquear clientes com dois recursos de mesmo nome em regiões diferentes que não conseguiam excluir nenhum deles.</td>
<td>.</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td>O objeto ClusterDetails agora retorna um campo **DeploymentID** que representa um identificador exclusivo para o cluster. É garantida a exclusividade entre tentativas de criação de clusters com os mesmos nomes.</td>
<td>.</td>
<td>Todos</td>
<td>N/D</td>
</tr>
</table>
<br>

## Notas da versão de 14/11/2014 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

Esta versão contém os novos recursos, atualizações de componentes e correções de bug a seguir.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Componente</th>
<th>Tipo de cluster</th>
<th>JIRA (se aplicável)</th>
</tr>

<tr>
<td>Ação de script (visualização)</td>
<td>Visualização do recurso de personalização de cluster, que habilita a modificação dos clusters Hadoop de maneiras arbitrárias pelo uso de scripts personalizados. Com esse recurso, os usuários podem experimentar e implantar projetos disponíveis no ecossistema do Apache Hadoop nos clusters do Azure HDInsight. O recurso de personalização está disponível em todos os tipos de clusters do HDInsight, incluindo Hadoop, HBase e Storm.</td>
<td>New recurso</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>Trabalhos pré-compilados para sites do Azure e análise de logs de armazenamento</td>
<td>O Console de Consulta do HDInsight tem uma galeria de introdução que dá suporte a soluções que funcionam com seus dados ou com dados de exemplo.
<p>**Soluções que funcionam com seus dados**:<br>
Criamos trabalhos para alguns dos cenários de análise de dados mais comuns para fornecer um ponto de partida para você criar as suas próprias soluções. Você pode usar seus próprios dados com o trabalho para ver como funciona. Em seguida, quando estiver pronto, use o que aprendeu para criar uma solução modelada segundo o trabalho pré-compilado.</p>
<p>**Soluções que funcionam com dados de exemplo**:<br>
Aprenda como trabalhar com o HDInsight ao analisar alguns cenários básicos (como analisar los da Web e dados de sensores). Além de aprender como usar o HDInsight para analisar tais dados, você também verá como é possível conectar outros aplicativos e serviços a esses dados. Visualizar dados ao conectar ao Microsoft Excel fornecerá um exemplo da eficiência dessa abordagem.</p></td>
<td>Console de consulta</td>
<td>O Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Correção de perda de memória no Templeton</td>
<td>Foi tratada a perda de memória no Templeton que afetava os clientes que tinham um cluster em execução há tempos ou que estavam enviando 100s de solicitações de trabalhos por segundo. O problema se manifestava como erros 5xx do Templeton e a solução alternativa era reiniciar o serviço. A solução alternativa não é mais necessária.</td>
<td>Templeton</td>
<td>Todos</td>
<td>https://issues.apache.org/jira/browse/HADOOP-11248</td>
</tr>
</table>
<br>


**Observação**: para demonstrar os novos recursos disponíveis pela personalização de clusters, os procedimentos que usam as Ações de Script para instalar ps módulos Spark e R em um cluster foram documentados. Para obter mais informações, consulte:

* [Instalar e usar o Spark 1.0 em clusters HDInsight](hdinsight-hadoop-spark-install.md)
* [Instalar e usar R em clusters Hadoop do HDInsight](hdinsight-hadoop-r-scripts.md)




## Notas da versão de 07/11/2014 do HDInsight ##

Os números completos da versão dos clusters HDInsight implantados com essa versão são:

* HDInsight 2.1 2.1.9.374.1153876
* HDInsight 3.0 3.0.5.374.1153876
* HDInsight 3.1 3.1.1.374.1153876

Esta versão contém as seguintes atualizações de componentes.

<table border="1">
<tr>
<th>Title</th>
<th>Descrição</th>
<th>Componente</th>
<th>Tipo de cluster</th>
<th>JIRA (se aplicável)</th>
</tr>

<tr>
<td>HDP 2.1.7</td>
<td>Esta versão é baseada no Hortonworks Data Platform (HDP) 2.1.7. Para obter mais informações, consulte <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">Notas de versão do HDP 2.1.7</a>.</td>
<td>HDP</td>
<td>Todos</td>
<td>N/D</td>
</tr>

<tr>
<td>Servidor de linha do tempo do YARN</td>
<td>O YARN Timeline Server (também conhecido como Generic Application History Server) foi habilitado por padrão. O Servidor de linha do tempo fornece informações genéricas sobre aplicativos concluídos, como ID do aplicativo, nome do aplicativo, status do aplicativo, hora de envio do aplicativo e hora de conclusão do aplicativo. Essas informações do aplicativo podem ser recuperadas do headnode tanto ao acessar o URI http://headnodehost:8188 quanto pela execução do comando YARN: yarn application –list –appStates ALL. Essas informações também podem ser recuperadas remotamente por uma API REST em https://{ClusterDnsName}. azurehdinsight.net/ws/v1/applicationhistory/. Para obter mais informações, consulte <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Servidor de linha do tempo do YARN</a>.</td>
<td>Serviço, YARN</td>
<td>Hadoop, HBase</td>
<td>N/D</td>
</tr>

<tr>
<td>ID de Implantação de Cluster</td>
<td>Iniciando pela versão 1.3.3.1.5426.29232 do SDK, os usuários podem acessar uma ID exclusiva para cada cluster, emitida pelo HDInsight. Isso permite que os clientes tomem conhecimento das instâncias exclusivas de clusters quando um dnsname está sendo reutilizado entre cenários de criação/cancelamento.</td>
<td>.</td>
<td>Todos</td>
<td>N/D</td>
</tr>
</table>
<br>

**Observação**: o bug que não permitia que o número completo da versão fosse mostrado no portal ou retornado pelo SDK ou pelo Windows PowerShell foi corrigido nesta versão.

## Notas para a versão de 15/10/2014 ##

A liberação desse hotfix corrige um vazamento de memória no Templeton que afetou os usuários mais ativos do Templeton. Em alguns casos, os usuários que haviam usado o Templeton com frequência viam erros manifestados como códigos de erro 500, pois as solicitações não tinham memória suficiente para serem executadas. A solução alternativa para esse problema era reiniciar o serviço do Templeton. Esse problema foi corrigido.


## Notas para a versão de 07/10/2014 ##

* Ao usar o ponto de extremidade do Ambari, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", o campo *host_name* retorna o nome de domínio totalmente qualificado (FQDN) do nó em vez de somente o nome do host. Por exemplo, ao invés de retornar "**headnode0**", você obtém o FQDN “**headnode0.{ClusterDNS}.azurehdinsight.net**”. Essa alteração foi necessária para facilitar cenários em que vários tipos de cluster, como HBase e Hadoop, podem ser implementados em uma rede virtual. Isso acontece, por exemplo, ao usar HBase como uma plataforma de back-end para o Hadoop.

* Fornecemos novas configurações de memória para a implantação padrão do cluster do HDInsight. As configurações de memória padrão anteriores não atentavam adequadamente às diretrizes referente ao número de núcleos de CPU implantados. Essas novas configurações de memória devem fornecer melhores padrões, de acordo com as recomendações da Hortonworks. Para alterá-las, consulte a documentação de referência do SDK sobre alteração da configuração de clusters. As novas configurações de memória usadas pelo cluster do HDInsight padrão com 4 núcleos de CPU (8 contêineres) são detalhadas na tabela a seguir. (Os valores usados anteriormente a essa versão também são fornecidos entre parênteses).

<table border="1"> <tr><th>Componente</th><th>alocação de memória</th></tr> <tr><td> yarn.scheduler.minimum-allocation</td><td>768 MB (anteriormente 512 MB)</td></tr> <tr><td> yarn.scheduler.maximum-allocation</td><td>6.144 MB (inalterada)</td></tr> <tr><td>yarn.nodemanager.resource.memory</td><td>6.144 MB (inalterada)</td></tr> <tr><td>mapreduce.map.memory</td><td>768 MB (anteriormente 512MB)</td></tr> <tr><td>mapreduce.map.java.opts</td><td>opts=-Xmx512m (anteriormente -Xmx410m)</td></tr> <tr><td>mapreduce.reduce.memory</td><td>1.536 MB (anteriormente 1.024 MB)</td></tr> <tr><td>mapreduce.reduce.java.opts</td><td>opts=-Xmx1024m (anteriormente -Xmx819m)</td></tr> <tr><td>yarn.app.mapreduce.am.resource</td><td>768 MB (anteriormente 1.024 MB)</td></tr> <tr><td>yarn.app.mapreduce.am.command</td><td>opts=-Xmx512m (anteriormente -Xmx819m)</td></tr> <tr><td>mapreduce.task.io.sort</td><td>256 MB (anteriormente 200 MB)</td></tr> <tr><td>tez.am.resource.memory</td><td>1.536 MB (inalterada)</td></tr>

</table><br>

Para obter mais informações sobre as configurações de memória usadas pelo YARN e pelo MapReduce na plataforma de dados Hortonworks usada pelo HDInsight, consulte [Determinar as configurações de memória de HDP](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). A Hortonworks também oferece uma ferramenta para calcular as configurações adequadas de memória.

Sobre o PowerShell do Azure e a mensagem de erro do SDK do HDInsight: "*O cluster não foi configurado para acessar os serviços HTTP*":

* Esse erro é um [problema de compatibilidade](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) conhecido que pode ocorrer devido a uma diferença entre a versão do SDK do HDInsight ou PowerShell do Azure e a versão do cluster. Clusters criados a partir de 15/08 dão suporte à nova funcionalidade de provisionamento em Redes Virtuais. Mas essa funcionalidade não é interpretada corretamente por versões mais antigas do SDK do HDInsight ou PowerShell do Azure. O resultado é uma falha em algumas operações de envio de trabalho. Se você usar APIs do SDK do HDInsight ou cmdlets do PowerShell do Azure (**Use-AzureHDInsightCluster**, **Invoke-Hive**) para enviar trabalhos, essas operações podem falhar com a mensagem de erro “O cluster <clustername> não está configurado para acessar serviços HTTP*”. Ou, dependendo da operação, você pode receber outras mensagens de erro como “*Não é possível se conectar ao cluster*”.

* Esses problemas de compatibilidade são resolvidos nas versões mais recentes do SDK do HDInsight e do PowerShell do Azure. Recomendamos atualizar o SDK do HDInsight para a versão 1.3.1.6 ou posterior e o Azure PowerShell Tools para a versão 0.8.8 ou posterior. Você pode obter acesso ao HDInsight SDK mais recente no [NuGet](http://nuget.codeplex.com/wikipage?title=Getting%20Started) e às Ferramentas do PowerShell do Azure em [Como instalar e configurar o PowerShell do Azure](../powershell-install-configure/).



## Notas da versão de 12/09/2014 do HDInsight 3.1##

* Esta versão é baseada na HDP (plataforma de dados Hortonworks) 2.1.5. Para obter uma lista de bugs corrigidos nesta versão, consulte a página [Corrigido nesta versão](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) no site da Hortonworks.
* Na pasta de bibliotecas do Pig, o arquivo “avro-mapred-1.7.4.jar” foi alterado para “avro-mapred-1.7.4-hadoop2.jar”. Esse arquivo contém uma pequena correção de bug que é não separável. Recomenda-se que os clientes não assumam uma dependência direta no nome do arquivo JAR para evitar separações quando os arquivos forem renomeados.


## Notas para a versão de 21/08/2014 ##

* Estamos adicionando a configuração WebHCat a seguir (HIVE-7155), que define o limite de memória padrão para um trabalho de controlador Templeton como 1 GB. (O valor padrão anterior era 512 MB.)

	 templeton.mapper.memory.mb (=1024)

	* Essa mudança trata do erro a seguir, que determinadas consultas de Hive encontraram devido a limites mais baixos de memória: “O contêiner está sendo executado além dos limites de memória física”.
	* Para voltar aos padrões antigos, você pode definir o valor dessa configuração para 512 por meio do PowerShell do Azure no momento da criação do cluster, usando o comando a seguir:

		Add-AzureHDInsightConfigValues -Core @{"templeton.mapper.memory.mb"="512";}


* O nome de host da função zookeeper foi alterado para *zookeeper*. Isso afeta o modo como o nome é resolvido no cluster, mas não afeta as APIs REST externas. Se você tem componentes que utilizam nome de host de *nó zookeeper*, você precisará atualizá-los com o novo nome. Os novos nomes para os três nós zookeeper são:
	* zookeeper0
	* zookeeper1
	* zookeeper2
* A matriz de suporte de versão do HBase é atualizada. Somente a versão HDInsight 3.1 (HBase versão 0.98) tem suporte para cargas de trabalho de produção no HBase. A versão 3.0, que estava disponível para visualização, não terá suporte se você prosseguir.

## Observações sobre os clusters criados antes de 15/8/2014 ##

Uma mensagem de erro do Azure PowerShell ou do SDK do HDInsight, "Cluster <clustername> não está configurado para acessar serviços HTTP" (ou dependendo da operação, outras mensagens de erro, como: "Não é possível conectar ao cluster") podem ser encontradas devido à diferença entre a versão do PowerShell do Azure ou do SDK do HDInsight e aquela de um cluster. Clusters criados a partir de 15/08 dão suporte à nova funcionalidade de provisionamento em Redes Virtuais. Essa funcionalidade não é interpretada corretamente por versões mais antigas do PowerShell do Azure ou do SDK do HDInsight, o que resulta em falhas nas operações de envio de trabalhos. Se você usar APIs do SDK do HDInsight ou cmdlets do PowerShell do Azure (como Use-AzureHDInsightCluster ou Invoke-AzureHDInsightHiveJob) para enviar trabalhos, essas operações podem falhar com uma das mensagens de erro descritas.

Esses problemas de compatibilidade são resolvidos nas versões mais recentes do SDK do HDInsight e do PowerShell do Azure. Recomendamos atualizar o SDK do HDInsight para a versão 1.3.1.6 ou posterior e o Azure PowerShell Tools para a versão 0.8.8 ou posterior. Você pode obter acesso ao SDK do HDInsight mais recente no [NuGet][nuget-link]. Você pode acessar as ferramentas do PowerShell do Azure usando o [Microsoft Web Platform Installer][webpi-link].


## Notas para a versão de 28/07/2014 ##

* **O HDInsight está disponível em novas regiões**: expandimos a presença geográfica do HDInsight para três regiões. Os clientes que usam o HDInsight agora podem criar clusters nessas regiões:
	* Ásia Oriental
	* Centro-Norte dos EUA
	* Centro-Sul dos Estados Unidos
* O HDInsight versão 1.6 (HDP 1.1 e Hadoop 1.0.3) e HDInsight versão 2.1 (HDP 1.3 e Hadoop 1.2) estão sendo removidos do portal do Azure. Você pode continuar a criar clusters Hadoop para essas versões usando o cmdlet do PowerShell do Azure ([New-AzureHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx)) ou usando o [SDK do HDInsight](http://msdn.microsoft.com/library/azure/dn469975.aspx). Consulte a página [Controle de versão do componente HDInsight](../hdinsight-component-versioning/) para obter mais informações.
* Alterações ao Hortonworks Data Platform (HDP) nesta versão:

<table border="1"> <tr><th>HDP</th><th>Alterações</th></tr> <tr><td>HDP 1.3 / HDI 2.1</td><td>nenhuma alteração</td></tr> <tr><td>HDP 2.0 / HDI 3.0</td><td>Nenhuma alteração</td></tr> <tr><td>HDP 2.1 / HDI 3.1</td><td>zookeeper: ["3.4.5.2.1.3.0-1948"] -> ["3.4.5.2.1.3.2-0002"]</td></tr>


</table><br>

## Notas para a versão de 24/06/2014 ##

Essa versão contém aprimoramentos para o serviço HDInsight:

* **Disponibilidade do HDP 2.1**: O HDInsight 3.1 (que contém o HDP 2.1) agora está disponível de modo geral e é a versão padrão para novos clusters.
* **HBase - aprimoramentos do portal do Azure**: estamos tornando os clusters HBase disponíveis em modo de visualização. Você pode criar clusters HBase por meio do portal com três cliques:

![](http://i.imgur.com/cmOl5fM.png)

Com o HBase, você pode criar uma variedade de cargas de trabalho em tempo real no HDInsight, por meio de sites interativos que funcionem com conjuntos de dados grandes para dados para o sensor de armazenamento de serviços e dados de telemetria de milhões de pontos de extremidade. A próxima etapa seria analisar os dados nessas cargas de trabalho com trabalhos do Hadoop e isso é possível no HDInsight, por meio do PowerShell do Azure e do painel de cluster do Hive.

### Apache Mahout pré-instalado no HDInsight 3.1 ###

 O [Mahout](http://hortonworks.com/hadoop/mahout/) é pré-instalado em clusters Hadoop do HDInsight 3.1, de modo que você pode executar trabalhos Mahout sem a necessidade de configuração de cluster adicionais. Por exemplo, você pode acessar remotamente um cluster do Hadoop usando o protocolo RDP e, sem etapas adicionais, executar o comando Olá Mundo do Mahout a seguir:

		mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

		mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Para obter uma explicação mais completa sobre este procedimento, consulte a documentação do [Exemplo Breiman](https://mahout.apache.org/users/classification/breiman-example.html) no site do Apache Mahout.


### Consultas de Hive podem usar o Tez no HDInsight 3.1 ###

O Hive 0.13 está disponível no HDInsight 3.1 e é capaz de executar consultas usando Tez, que pode ser utilizado para melhorias de desempenho significativas. O Tez não está habilitado por padrão para consultas de Hive. Para usá-lo, você deve escolher a opção de fazê-lo. Você pode habilitar o Tez executando o trecho de código a seguir:

		set hive.execution.engine=tez;
		select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

A Hortonworks publicou uma demonstração detalhada das melhorias nas consultas do Hive com o Tez, conforme fornecido em medições de desempenho padrão. Para obter mais informações, consulte [Medindo o desempenho do Hive 13 Apache para Hadoop Enterprise](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Para obter mais detalhes sobre como usar o Hive com Tez, consulte [Hive em Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

###Disponibilidade global
Com a versão do HDInsight no Hadoop 2.2, a Microsoft disponibilizou o HDInsight em todas as principais regiões geográficas onde o Azure está disponível. Especificamente, os datacenters da Europa Ocidental e do Sudeste Asiático foram colocados online. Isso permite que os clientes localizem clusters em um datacenter próximo e possivelmente em uma zona com requisitos de conformidade semelhantes.


###Prós e contras entre as versões de clusters

**Os metastores do Oozie usados com um cluster do HDInsight 3.1 não são compatíveis com as versões anteriores dos clusters do HDInsight 2.1 e não podem ser usados com elas**.

Um banco de dados do metastore do Oozie personalizado implantado com um cluster do HDInsight 3.1 não pode ser reutilizado com um cluster do HDInsight 2.1. Isso acontece mesmo se o metastore tiver sido originado com um cluster HDInsight 2.1. Esse cenário não tem suporte, já que o esquema do metastore é atualizado quando utilizado com um cluster HDInsight 3.1 e, portanto, não é mais compatível com o metastore exigido pelos clusters HDInsight 2.1. Qualquer tentativa de reutilizar um metastore do Oozie que tenha sido usado com um cluster do HDInsight 3.1 fará com que o cluster HDInsight 2.1 seja inutilizado.

**Os metastores do Oozie não podem ser compartilhados entre clusters diferentes.**

Metastores do Oozie estão associados a clusters específicos e não podem ser compartilhados entre clusters.

###Alterações de última hora

**Sintaxe de prefixo**: apenas a sintaxe "wasb://" tem suporte em clusters HDInsight 3.0 e 3.1. A antiga sintaxe "asv://" tem suporte em clusters HDInsight 1.6 e 2.1, mas não tem suporte em clusters HDInsight 3.0 ou 3.1. Isso significa que qualquer trabalho enviado para um cluster HDInsight 3.0 ou 3.1 que use explicitamente a sintaxe “asv://” falhará. Em vez disso, a sintaxe “wasb://” deve ser usada. Os trabalhos enviados para qualquer cluster HDInsight 3.0 ou 3.1, criados com um metastore existente e contendo referências explícitas a recursos usando a sintaxe “asv://” também falharão. Esses metastores precisarão ser recriados usando a sintaxe “wasb://” para endereçar recursos.


**Portas:** as portas usadas pelo serviço HDInsight foram alteradas. Os números de porta que estavam sendo usados estavam dentro do intervalo de portas efêmero do SO Windows. As portas são alocadas automaticamente por meio de um intervalo efêmero, para comunicações baseadas em protocolo de Internet de curta duração. O novo conjunto de números de porta de serviço do HDP (plataforma de dados Hortonworks) permitido está agora fora desse intervalo, para evitar conflitos que poderiam surgir com as portas usadas pelos serviços que estão sendo executados no headnode. Os novos números de porta não devem causar nenhuma mudança de última hora. Os números usados são conforme descrito a seguir:

 **HDInsight 1.6 (HDP 1.1)** <table border="1"> <tr><th>Nome</th><th>Valor</th></tr> <tr><td>dfs.http.address</td><td>namenodehost:30070</td></tr> <tr><td>dfs.datanode.address</td><td>0.0.0.0:30010</td></tr> <tr><td>dfs.datanode.http.address</td><td>0.0.0.0:30075</td></tr> <tr><td>dfs.datanode.ipc.address</td><td>0.0.0.0:30020</td></tr> <tr><td>dfs.secondary.http.address</td><td>0.0.0.0:30090</td></tr> <tr><td>mapred.job.tracker.http.address</td><td>jobtrackerhost:30030</td></tr> <tr><td>mapred.task.tracker.http.address</td><td>0.0.0.0:30060</td></tr> <tr><td>mapreduce.history.server.http.address</td><td>0.0.0.0:31111</td></tr> <tr><td>templeton.port</td><td>30111</td></tr> </table><br>

 **HDInsight 3.1 e 3.0 (HDP 2.1 e 2.0)** <table border="1"> <tr><th>Nome</th><th>Valor</th></tr> <tr><td>dfs.namenode.http-address</td><td>namenodehost:30070</td></tr> <tr><td>dfs.namenode.https-address</td><td>headnodehost:30470</td></tr> <tr><td>dfs.datanode.address</td><td>0.0.0.0:30010</td></tr> <tr><td>dfs.datanode.http.address</td><td>0.0.0.0:30075</td></tr> <tr><td>dfs.datanode.ipc.address</td><td>0.0.0.0:30020</td></tr> <tr><td>dfs.namenode.secondary.http-address</td><td>0.0.0.0:30090</td></tr> <tr><td>yarn.nodemanager.webapp.address</td><td>0.0.0.0:30060</td></tr> <tr><td>templeton.port</td><td>30111</td></tr> </table><br>

###Dependências

As seguintes dependências foram acrescentadas ao HDInsight 3.x (HDP2.x):

* guice-servlet
* optiq-core
* javax.inject
* activation
* jsr305
* geronimo-jaspic_1.0_spec
* jul-to-slf4j
* java-xmlbuilder
* ant
* commons-compiler
* jdo-api
* commons-math3
* paranamer
* jaxb-impl
* stringtemplate
* eigenbase-xom
* jersey-servlet
* commons-exec
* jaxb-api
* jetty-all-server
* janino
* xercesImpl
* optiq-avatica
* jta
* eigenbase-properties
* groovy-all
* hamcrest-core
* mail
* linq4j
* jpam
* jersey-client
* aopalliance
* geronimo-annotation_1.0_spec
* ant-launcher
* jersey-guice
* xml-apis
* stax-api
* asm-commons
* asm-tree
* wadl
* geronimo-jta_1.1_spec
* guice
* leveldbjni-all
* velocity
* jettison
* snappy-java
* jetty-all
* commons-dbcp

As seguintes dependências não existem mais no HDInsight 3.x (HDP2.x):

* jdeb
* kfs
* sqlline
* ivy
* aspectjrt
* json
* core
* jdo2-api
* avro-mapred
* datanucleus-enhancer
* jsp
* commons-logging-api
* commons-math
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* snappy

###Mudanças de versão

As mudanças de versão a seguir foram feitas entre o HDInsight 2.x (HDP1.x) e o HDInsight 3.x (HDP2.x):

* metrics-core: ['2.1.2'] -> ['3.0.0']
* derbynet: ['10.4.2.0'] -> ['10.10.1.1']
* datanucleus: ['rdbms-3.0.8'] -> ['rdbms-3.2.9']
* jasper-compiler: ['5.5.12'] -> ['5.5.23']
* log4j: ['1.2.15', '1.2.16'] -> ['1.2.16', '1.2.17']
* derbyclient: ['10.4.2.0'] -> ['10.10.1.1']
* httpcore: ['4.2.4'] -> ['4.2.5']
* hsqldb: ['1.8.0.10'] -> ['2.0.0']
* jets3t: ['0.6.1'] -> ['0.9.0']
* protobuf-java: ['2.4.1'] -> ['2.5.0']
* derby: ['10.4.2.0'] -> ['10.10.1.1']
* jasper: ['runtime-5.5.12'] -> ['runtime-5.5.23']
* commons-daemon: ['1.0.1'] -> ['1.0.13']
* datanucleus-core: ['3.0.9'] -> ['3.2.10']
* datanucleus-api-jdo: ['3.0.7'] -> ['3.2.6']
* zookeeper: ['3.4.5.1.3.9.0-01320'] -> ['3.4.5.2.1.3.0-1948']
* bonecp: ['0.7.1.RELEASE'] -> ['
* 0.8.0.RELEASE']


###Drivers
O driver JDBC (conectividade de banco de dados Java) do SQL Server é usado internamente pelo HDInsight e não é usado para operações externas. Se você desejar se conectar ao HDInsight usando o ODBC (conectividade aberta de banco de dados), use o driver ODBC do Microsoft Hive. Para obter mais informações, consulte [Conectar o Excel ao HDInsight com o driver ODBC do Microsoft Hive](../hdinsight-connect-excel-hive-odbc-driver.md).


### Correções de bug ###

Com essa versão, atualizamos as versões do HDInsight a seguir com diversas correções de bug:

* HDInsight 2,1 (HDP 1,3)
* HDInsight 3,0 (HDP 2,0)
* HDInsight 3.1 (HDP 2.1)


## Notas de versão Hortonworks ##

As notas de versão para os HDPs (plataformas de dados Hortonworks) que são usados pelos clusters de versão do HDInsight estão disponíveis nos locais listados a seguir:

* O HDInsight versão 3.1 usa uma distribuição do Hadoop baseada na [Plataforma de Dados Hortonworks 2.1.7][hdp-2-1-7]. Esse é o cluster do Hadoop padrão criado ao usar o portal do Azure após 07/11/2014. Os clusters do HDInsight 3.1 criados antes de 07/11/2014 eram baseados na [Plataforma de Dados Hortonworks 2.1.1][hdp-2-1-1]

* O HDInsight versão 3.0 usa uma distribuição do Hadoop baseada na [Plataforma de Dados Hortonworks 2.0][hdp-2-0-8].

* O HDInsight versão 2.1 usa uma distribuição do Hadoop baseada na [Plataforma de Dados Hortonworks 1.3][hdp-1-3-0].

* O HDInsight versão 1.6 usa uma distribuição do Hadoop baseada na [Plataforma de Dados Hortonworks 1.1][hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/

<!--HONumber=54--> 