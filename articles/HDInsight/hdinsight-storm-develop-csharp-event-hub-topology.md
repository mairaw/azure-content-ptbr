<properties
   pageTitle="Processar eventos de Hubs de Eventos com o Storm no HDInsight | Azure"
   description="Saiba como processar dados de Hubs de Eventos com uma topologia Storm C# criada no Visual Studio usando as Ferramentas do HDInsight para Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="04/28/2015"
   ms.author="larryfr"/>

# Processar eventos dos Hubs de Eventos do Azure com o Storm no HDInsight

Os Hubs de Eventos do Azure permitem processar grandes quantidades de dados de sites, aplicativos e dispositivos. O spout dos Hubs de Eventos facilita o uso do Apache Storm no HDInsight para analisar esses dados em tempo real. Você pode também gravar dados no Hub de Eventos usando o bolt dos Hubs de Eventos.

Neste tutorial, você aprenderá a usar as Ferramentas do HDInsight para Visual Studio, além do spout e do bolt dos Hubs de Eventos para criar duas topologias C#/Java híbridas:

* **EventHubWriter**: gera dados aleatoriamente e grava nos Hubs de Eventos

* **EventHubReader**: lê dados dos Hubs de Eventos e os armazena no armazenamento de Tabela do Azure

## Pré-requisitos

* An <a href="../hdinsight-storm-getting-started/" target="_blank">Apache Storm no cluster HDInsight</a>

* An <a href="../service-bus-event-hubs-csharp-ephcs-getstarted/" target="_blank">Hub de Eventos do Azure</a>

* The <a href="http://azure.microsoft.com/downloads/" target="_blank">SDK .NET do Azure</a>

* As <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">Ferramentas do HDInsight para Visual Studio</a> (a versão de 17/02/2015 ou posterior)

## Projeto concluído

Você pode baixar uma versão concluída do projeto criado neste tutorial do GitHub: [eventhub-storm-hybrid](https://github.com/Blackmist/eventhub-storm-hybrid). No entanto, você ainda precisa fornecer definições de configuração seguindo as etapas neste tutorial.

> [AZURE.NOTE]Ao usar o projeto concluído, você deverá usar o **Gerenciador de Pacotes NuGet** para restaurar pacotes exigidos pela solução.

## Bolt e spout dos Hubs de Eventos

O bolt e o spout dos Hubs de Eventos são componentes de Java que permitem trabalhar facilmente com o Hub de Eventos do Apache Storm. Embora esses componentes sejam escritos em Java, as Ferramentas do HDInsight para Visual Studio permitem que você crie topologias híbridas que combinem componentes C# e Java.

O bolt e o spout são distribuídos como um único arquivo Java (.jar) denominado **eventhubs-storm-spout-0.9-jar-with-dependencies.jar**.

### Baixar o arquivo .jar

A versão mais recente do arquivo **eventhubs-storm-spout-0.9-jar-with-dependencies.jar** foi incluída no projeto <a href="https://github.com/hdinsight/hdinsight-storm-examples" target="_blank">exemplos do Storm no HDInsight</a> na pasta **lib**. Para baixar o arquivo, use um dos métodos a seguir.

> [AZURE.NOTE]O bolt e spout foram enviados para inclusão no projeto Apache Storm. Para obter mais informações, consulte <a href="https://github.com/apache/storm/pull/336/files">STORM-583: check-in inicial para hubs de eventos do storm</a> no GitHub.

* **Baixar um arquivo ZIP**: do site <a href="https://github.com/hdinsight/hdinsight-storm-examples" target="_blank">Exemplos do Storm no HDInsight</a>, selecione **Baixar ZIP** no painel à direita para baixar um arquivo .zip que contém o projeto.

	![botão baixar zip](./media/hdinsight-storm-develop-csharp-event-hub-topology/download.png)

	Depois de baixar o arquivo, você poderá extrair o arquivo para o diretório **lib**.

* **Clonar o projeto**: se você tiver <a href="http://git-scm.com/" target="_blank">Git</a> instalado, use o comando a seguir para clonar o repositório localmente, em seguida, localize o arquivo no diretório **lib**.

		git clone https://github.com/hdinsight/hdinsight-storm-examples

## Configurar o Hub de Eventos

Hubs de Eventos é a fonte de dados para este exemplo. Use as seguintes etapas para criar um novo Hub de Eventos.

1. No [portal do Azure](https://manage.windowsazure.com), selecione **NOVO** > **Barramento de Serviço** > **Hub de Eventos** | **Criação Personalizada**.

2. Na tela **Adicionar um novo Hub de Eventos**, insira um **Nome do Hub de Eventos**, selecione a **Região** na qual criar o hub e crie um novo namespace ou selecione um existente. Clique na **Seta** para continuar.

	![página 1 do assistente](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

	> [AZURE.NOTE]Você deve selecionar o mesmo **Local** como seu Storm no servidor HDInsight para reduzir a latência e os custos.

2. Na tela **Configurar o Hub de Eventos**, insira os valores de **Contagem de partições** e **Retenção de Mensagem**. Para este exemplo, use uma contagem de partições de 10 e uma retenção de mensagens de 1. Observe a contagem de partições, pois você precisará desse valor posteriormente.

	![página 2 do assistente](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

3. Depois que o hub de eventos tiver sido criado, selecione o namespace, selecione **Hubs de Eventos** e, em seguida, selecione o hub de eventos criado anteriormente.

4. Selecione **Configurar** e crie duas novas políticas de acesso usando as informações a seguir.

	<table>
<tr><th>Nome</th><th>Permissões</th></tr>
<tr><td>Gravador</td><td>Enviar</td></tr>
<tr><td>Leitor</td><td>Escutar</td></tr>
</table>Depois de criar permissões, selecione o ícone **Salvar** na parte inferior da página. Isso cria políticas de acesso compartilhado que serão usadas para enviar (gravador) e escutar (leitor) esse Hub de Eventos.

	![políticas](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

5. Depois de salvar as políticas, use o **Gerador de chave de acesso compartilhado** na parte inferior da página para recuperar a chave para ambas as políticas **gravador** e **leitor**. Salve-as, pois serão usadas mais tarde.

## Configurar o armazenamento de Tabela

O armazenamento de Tabela será usado para armazenar valores lidos de Hubs de Eventos, pois você pode exibir facilmente o armazenamento de Tabela no Visual Studio por meio do **Gerenciador de Servidores**. Use as seguintes etapas para criar um novo armazenamento de Tabela.

1. No [portal do Azure](https://manage.windowsazure.com), selecione **NOVO** > **Serviços de Dados** > **Armazenamento** > **Criação Rápida**.

	![armazenamento de criação rápida](./media/hdinsight-storm-develop-csharp-event-hub-topology/storagecreate.png)

2. Insira um **Nome** para a conta de armazenamento, selecione um **Local** e, em seguida, clique na **marca de seleção** para criar a conta de armazenamento.

	> [AZURE.NOTE]Você deve selecionar o mesmo **Local** dos seus Hubs de Eventos e Storm no servidor HDInsight para reduzir a latência e os custos.

3. Depois que a nova conta de armazenamento for provisionada, selecione a conta e, em seguida, use o link **Gerenciar Chaves de Acesso** na parte inferior da página para recuperar o **Nome da Conta de Armazenamento** e a **Chave de Acesso Primária**. Salve essas informações porque elas serão usadas posteriormente.

	![teclas de acesso](./media/hdinsight-storm-develop-csharp-event-hub-topology/managekeys.png)

## Criar EventHubWriter

Nesta seção, você criará uma topologia que grava dados nos Hubs de Eventos usando o bolt dos Hubs de Eventos.

1. Se você ainda não instalou a versão mais recente das Ferramentas do HDInsight para Visual Studio, consulte <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">Introdução ao uso das Ferramentas do HDInsight para Visual Studio</a>.

2. Abra o Visual Studio, selecione **Arquivo** > **Novo** e **Projeto**.

3. Na caixa de diálogo **Novo Projeto**, expanda **Instalado** > **Modelos** e selecione **HDInsight**. Na lista de modelos, selecione **Aplicativo Storm**. Na parte inferior da caixa de diálogo, digite **EventHubWriter** como o nome do aplicativo.

	![imagem](./media/hdinsight-storm-develop-csharp-event-hub-topology/newproject.png)

4. Quando o projeto tiver sido criado, você deverá ter os seguintes arquivos:

	* **Program.cs**: define a topologia para seu projeto. Observe que uma topologia padrão que consiste em um spout e um bolt é criada por padrão.

	* **Spout.cs**: um spout de exemplo.

	* **Bolt.cs**: um bolt de exemplo. Isso será excluído porque você usará o bolt dos Hubs de Eventos para gravar no Hub de Eventos

### Configuração

1. No **Gerenciador de Soluções**, clique com botão direito do mouse em **EventHubWriter** e, em seguida, selecione **Propriedades**.

2. Nas propriedades do projeto, selecione **Configurações** e, em seguida, selecione **Este projeto não contém um arquivo de configurações padrão. Clique aqui para criar um.**

3. Insira as configurações a seguir. Use as informações para o Hub de Eventos criado anteriormente na coluna **Valor**.

	<table>
<tr><th style="text-align:left">Nome</th><th style="text-align:left">Tipo</th><th style="text-align:left">Escopo</th></tr>
<tr><td style="text-align:left">EventHubPolicyName</td><td style="text-align:left">string</td><td style="text-align:left">Aplicativo</td></tr>
<tr><td style="text-align:left">EventHubPolicyKey</td><td style="text-align:left">string</td><td style="text-align:left">Aplicativo</td></tr>
<tr><td style="text-align:left">EventHubNamespace</td><td style="text-align:left">string</td><td style="text-align:left">Aplicativo</td></tr>
<tr><td style="text-align:left">EventHubName</td><td style="text-align:left">string</td><td style="text-align:left">Aplicativo</td></tr>
<tr><td style="text-align:left">EventHubPartitionCount</td><td style="text-align:left">int</td><td style="text-align:left">Aplicativo</td></tr>
</table>

4. Salve e feche a página **Propriedades**.

### Definir a topologia

1. No **Gerenciador de Soluções**, clique com botão direito do mouse em **Bolt.cs** e selecione **Excluir**. Como você está usando o bolt dos Hubs de Eventos do Java, esse arquivo não será necessário.

2. Abra o arquivo** Program.cs** e adicione o seguinte código imediatamente após a linha `TopologyBuilder topologyBuilder = new TopologyBuilder("EventHubWriter");`.

		int partitionCount = Properties.Settings.Default.EventHubPartitionCount;
		List<string> javaDeserializerInfo =
            new List<string>() { "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer", "java.lang.String" };

	A primeira linha lê a contagem de partições de propriedades definidas anteriormente. A segunda linha define um desserializador usado para desserializar dados JSON produzidos pelo spout, em um`java.lang.String` para que os componentes Java possam consumir os dados.

4. Localize o código a seguir:

		topologyBuilder.SetSpout(
            "Spout",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"count"}}
            },
            1);

	Substitua-o pelo seguinte:

		topologyBuilder.SetSpout(
            "Spout",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"Event"}}
            },
            partitionCount).
            DeclareCustomizedJavaDeserializer(javaDeserializerInfo);

	Isso cria um spout e usa a contagem de partições dos Hubs de Eventos como a dica de paralelismo para este componente. Isso deve criar uma instância do spout para cada partição.

	Isso também associa o desserializador criado anteriormente ao fluxo de saída deste componente. Isso permite que o componente downstream EventHubSpout consuma os dados produzidos a partir do spout C#.

5. Logo após o código anterior, adicione o seguinte:

		JavaComponentConstructor constructor =
            JavaComponentConstructor.CreateFromClojureExpr(
            String.Format(@"(com.microsoft.eventhubs.bolt.EventHubBolt. (com.microsoft.eventhubs.bolt.EventHubBoltConfig. " +
            @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
            Properties.Settings.Default.EventHubPolicyName,
            Properties.Settings.Default.EventHubPolicyKey,
            Properties.Settings.Default.EventHubNamespace,
            "servicebus.windows.net", //suffix for servicebus fqdn
            Properties.Settings.Default.EventHubName,
			"true"));

	Isso cria um novo construtor para o bolt Java, que é usado em tempo de execução para configurar uma nova instância do bolt. Nesse caso, você está usando o <a href="http://storm.apache.org/documentation/Clojure-DSL.html" target="_blank">Apache Storm Clojure DSL</a> para configurar o spout com as informações de configuração dos Hubs de Eventos adicionadas anteriormente. Mais especificamente, esse código é usado pelo HDInsight em tempo de execução para fazer o seguinte:

	* Criar uma nova instância de **com.microsoft.eventhubs.bolt.EventHubBoltConfig** usando as informações dos Hubs de Eventos que você fornecer.
	* Criar uma nova instância de **com.microsoft.eventhubs.bolt.EventHubBolt**, passando na instância **EventHubBoltConfig**.

6. Localize o código a seguir:

		topologyBuilder.SetBolt(
            "Bolt",
            Bolt.Get,
            new Dictionary<string, List<string>>(),
            1).shuffleGrouping("Spout");

	Substitua-o pelo seguinte:

		topologyBuilder.SetJavaBolt(
            "EventHubBolt",
            constructor,
            partitionCount).
            shuffleGrouping("Spout");

	Isso instrui a topologia a usar o **JavaComponentConstructor** da etapa anterior como o bolt. O componente pode ser chamado nessa topologia pelo nome amigável de "EventHubBolt". A dica de paralelismo é definida como o número de partições para o Hub de Eventos, e ele assina dados produzidos pelo spout ("Spout").

Neste ponto, você concluiu o **Program.cs**. A topologia foi definida, mas agora modifique **Spout.cs** para que ele produza dados em um formato que o bolt dos Hubs de Eventos possa usar.

> [AZURE.NOTE]Essa topologia terá como padrão a criação de um processo de trabalho, que é suficiente para fins de exemplo. Se você estiver adaptando isso para um cluster de produção, deverá adicionar o seguinte e ajustar o número de trabalhos que deseja criar.
>
> ```topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
                {
                    {"topology.workers", "1"}  //Change to set the number of workers to create
                });```

### Modificar o spout

O bolt dos Hubs de Evento espera um valor único de cadeia de caracteres, que ele roteará para o Hub de Eventos. No exemplo a seguir, você modificará o arquivo padrão **Spout.cs** para produzir uma cadeia de caracteres JSON.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **EventHubWriter** e escolha **Gerenciar Pacotes Nuget**. Pesquise o pacote **Json.NET** e adicione-o à solução. Isso nos permitirá criar facilmente dados JSON que serão enviados aos Hubs de Eventos usando o bolt.

1. Abra **Spout.cs** e adicione o seguinte à parte superior do arquivo:

		using Newtonsoft.Json;
		using Newtonsoft.Json.Linq;

	Isso permite que você trabalhe com mais facilidade com dados JSON.

3. Localize o código a seguir:

		Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        outputSchema.Add("default", new List<Type>() { typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));

	Substitua-o pelo seguinte:

		Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer());

	Isso altera a definição de dados criados pelo spout para usar dados de **cadeia de caracteres** e o **CustomizedInteropJSONSerializer** declarado anteriormente na topologia (em program.cs).

2. Substitua o método **NextTuple** pelo seguinte:

		public void NextTuple(Dictionary<string, Object> parms)
        {
            JObject eventData = new JObject();
            eventData.Add("deviceId", r.Next(10));
            eventData.Add("deviceValue", r.Next());
            ctx.Emit(new Values(eventData.ToString(Formatting.None)));
        }

	Isso vai gerar aleatoriamente uma identificação de dispositivo, um valor e então usará Json.NET para emitir um objeto JSON usando esses valores.

3. Salve o arquivo **Spout.cs**.

Neste ponto, você tem uma topologia básica que vai gerar dados aleatórios e armazená-los em Hubs de Eventos usando o bolt dos Hubs de Eventos. Em seguida, você criará o leitor.

## Criar EventHubReader

Nesta seção, você criará uma topologia que lê dados de Hubs de Eventos usando o bolt dos Hubs de Eventos.

2. Abra o Visual Studio, selecione **Arquivo** > **Novo** > **Projeto**.

3. Na caixa de diálogo **Novo Projeto**, expanda **Instalado** > **Modelos** e selecione **HDInsight**. Na lista de modelos, selecione **Aplicativo Storm**. Na parte inferior da caixa de diálogo, insira **EventHubReader** como o nome do aplicativo.

### Configuração

1. No **Gerenciador de Soluções**, clique com botão direito do mouse em **EventHubReader** e, em seguida, selecione **Propriedades**.

2. Nas propriedades do projeto, selecione **Configurações** e, em seguida, selecione **Este projeto não contém um arquivo de configurações padrão. Clique aqui para criar um.**

3. Insira as configurações a seguir. Use as informações para o Hub de Eventos e a Conta de armazenamento que você criou anteriormente na coluna **Valor**.

	<table>
<tr><th style="text-align:left">Nome</th><th style="text-align:left">Tipo</th><th style="text-align:left">Escopo</th></tr>
<tr><th style="text-align:left">EventHubPolicyName</th><th style="text-align:left">string</th><th style="text-align:left">Aplicativo</th></tr>
<tr><th style="text-align:left">EventHubPolicyKey</th><th style="text-align:left">string</th><th style="text-align:left">Aplicativo</th></tr>
<tr><th style="text-align:left">EventHubNamespace</th><th style="text-align:left">string</th><th style="text-align:left">Aplicativo</th></tr>
<tr><th style="text-align:left">EventHubName</th><th style="text-align:left">string</th><th style="text-align:left">Aplicativo</th></tr>
<tr><th style="text-align:left">EventHubPartitionCount</th><th style="text-align:left">int</th><th style="text-align:left">Aplicativo</th></tr>
<tr><th style="text-align:left">StorageConnection</th><th style="text-align:left">(Cadeia de Conexão)</th><th style="text-align:left">Aplicativo</th></tr>
<tr><th style="text-align:left">TableName</th><th style="text-align:left">string</th><th style="text-align:left">Aplicativo</th></tr>
</table>Para **TableName**, insira o nome da tabela em que você deseja que eventos sejam armazenados.

  Para **StorageConnection**, insira um valor `DefaultEndpointsProtocol=https;AccountName=myAccount;AccountKey=myKey;`. Substitua **myAccount** e **myKey** pelo nome da conta de armazenamento e pela chave obtida anteriormente.

	These values will be used by the topology to communicate with Event Hubs and Table Storage.

4. Salve e feche a página **Propriedades**.

### Definir a topologia

1. No **Gerenciador de Soluções**, clique com botão direito do mouse em **Spout.cs** e selecione **Excluir**. Como você está usando o spout dos Hubs de Eventos do Java, esse arquivo não será necessário.

2. Abra o arquivo **Program.cs** e adicione o seguinte código imediatamente após a linha `TopologyBuilder topologyBuilder = new TopologyBuilder("EventHubReader");`.

		int partitionCount = Properties.Settings.Default.EventHubPartitionCount;
		JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
            String.Format(@"(com.microsoft.eventhubs.spout.EventHubSpout. (com.microsoft.eventhubs.spout.EventHubSpoutConfig. " +
                @"""{0}"" ""{1}"" ""{2}"" ""{3}"" {4} ""{5}""))",
                Properties.Settings.Default.EventHubPolicyName,
                Properties.Settings.Default.EventHubPolicyKey,
                Properties.Settings.Default.EventHubNamespace,
                Properties.Settings.Default.EventHubName,
                partitionCount,
                "")); //Last value is the zookeeper connection string - leave empty

	A contagem de partições é lida e atribuída a uma variável local. Ela será usada várias vezes.

	O `JavaComponentConstructor` define como o spout do Java será criado em tempo de execução. Nesse caso, você está usando o <a href="http://storm.apache.org/documentation/Clojure-DSL.html" target="_blank">Apache Storm Clojure DSL</a> para configurar o spout com as informações de configuração dos Hubs de Eventos adicionadas anteriormente. Mais especificamente, esse código é usado pelo HDInsight em tempo de execução para fazer o seguinte:

	* Crie uma nova instância de **com.microsoft.eventhubs.spout.EventHubBoltConfig** usando as informações dos Hubs de Eventos que você fornecer.

	* Crie uma nova instância de **com.microsoft.eventhubs.spout.EventHubBoltConfig** passando na instância **EventHubSpoutConfig**.

5. Localize o código a seguir:

		topologyBuilder.SetSpout(
            "Spout",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"count"}}
            },
            1);

	Substitua-o pelo seguinte:

        topologyBuilder.SetJavaSpout(
            "EventHubSpout",
            constructor,
            partitionCount);

	Isso instrui a topologia a usar o **JavaComponentConstructor** da etapa anterior como o spout e a chamá-lo de "EventHubSpout". Isso também define a dica paralelismo deste componente para o número de partições no Hub de Eventos.

2. Adicione o seguinte logo após o código anterior:

         List<string> javaSerializerInfo = new List<string>() { "microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer" };

	Isso cria um serializador personalizado, que será usado para serializar informações geradas pelos componentes Java (como EventHubSpout) em um formato JSON que possa ser usado por componentes C# downstream.

3. Localize o código a seguir:

		topologyBuilder.SetBolt(
            "Bolt",
            Bolt.Get,
            new Dictionary<string, List<string>>(),
            1).shuffleGrouping("Spout");

	Substitua-o pelo seguinte:

		topologyBuilder.SetBolt(
            "Bolt",
            Bolt.Get,
            new Dictionary<string, List<string>>(),
            partitionCount,
            true).
            DeclareCustomizedJavaSerializer(javaSerializerInfo).
            shuffleGrouping("EventHubSpout");

	Este código instrui a topologia a usar um bolt (definido em Bolt.cs). O serializador personalizado definido anteriormente é usado aqui para que este bolt possa consumir dados produzidos pelos componentes Java upstream. Nesse caso, o EventHubSpout.

    > [AZURE.IMPORTANT]O último parâmetro para SetBolt, (com um valor `true`), habilita a funcionalidade ACK para este bolt. Isso é necessário, já que o componente EventHubSpout espera uma ACK para os dados que ele emite. Se ACKs não forem retornados por componentes downstream, o spout terá a recepção interrompida após o processamento de cerca de 1000 mensagens.

Neste ponto, você concluiu **Program.cs**. A topologia foi definida, mas agora você deve criar uma classe auxiliar para gravar dados no armazenamento de Tabela. Você deve modificar **Bolt.cs** para que ele possa entender os dados produzidos pelo spout.

> [AZURE.NOTE]Essa topologia terá como padrão a criação de um processo de trabalho, que é suficiente para fins de exemplo. Se você estiver adaptando isso para um cluster de produção, deverá adicionar o seguinte e ajustar o número de trabalhos que deseja criar.
>
> ```topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
                {
                    {"topology.workers", "1"}  //Change to set the number of workers to create
                });```

### Criar uma classe auxiliar

Ao gravar dados no armazenamento de Tabela, você deve criar uma classe que descreve os dados que serão gravados.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **EventHubReader** e escolha **Adicionar** e **Nova Classe**. Chame a nova classe de **Devices.cs**.

2. Abra **Devices.cs** e substitua o código padrão pelo seguinte:

		using System;
		using System.Collections.Generic;
		using System.Linq;
		using System.Text;
		using System.Threading.Tasks;
		using Microsoft.WindowsAzure.Storage.Table;

		namespace EventHubReader
		{
		    class Device : TableEntity
		    {
		        public int value { get; set; }

		        public Device() { }
		        public Device(int id)
		        {
		            this.PartitionKey = id.ToString();
		            this.RowKey = System.Guid.NewGuid().ToString();
		        }
		    }
		}

	Isso criará entidades no armazenamento de Tabela que consistem em uma chave de partição (que será definida como a identificação do dispositivo para ler do Hub de Eventos), uma chave exclusiva da linha e um valor que é lido do Hub de Eventos. Cada entidade também terá um carimbo de data/hora, que será criado automaticamente quando a entidade for inserida na tabela.

### Modificar o bolt

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **EventHubReader** e escolha **Gerenciar Pacotes Nuget**. Pesquise o pacote **Json.Net** e adicione-o à solução. Isso nos permitirá processar com facilidade os dados JSON recebidos do spout. Adicione também o pacote **WindowsAzure.Storage**, que nos permitirá gravar para o armazenamento de tabela.

1. Abra **Bolt.cs** e adicione o seguinte à parte superior do arquivo:

		using Newtonsoft.Json.Linq;
		using Microsoft.WindowsAzure.Storage;
		using Microsoft.WindowsAzure.Storage.Table;

	Isso nos permitirá trabalhar mais facilmente com dados JSON do bolt e gravar dados no armazenamento de Tabela.

2. Localize a instrução `private int count;` e substitua-a pelo seguinte:

        private CloudTable table;

	Isso será usado ao conectar-se à tabela.

4. Localize o código a seguir:

		Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        inputSchema.Add("default", new List<Type>() { typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));

	Substitua-o pelo seguinte:

		Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
        this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());

	Isso instrui o bolt que estará recebendo um valor **cadeia de caracteres** em vez de um **int**, e que os dados devem ser desserializados usando o **CustomizedInteropJSONDeserialzer** declarado anteriormente na topologia (no arquivo program.cs).

3. Logo após o código anterior, adicione o seguinte:

		CloudStorageAccount storageAccount = CloudStorageAccount.Parse(Properties.Settings.Default.StorageConnection);
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
        table = tableClient.GetTableReference(Properties.Settings.Default.TableName);
        table.CreateIfNotExists();

	Isso se conectará à tabela **eventos** usando a cadeia de conexão configurada anteriormente. Se não existir, ela será criada.

2. Localize o método **Execute** e substitua-o pelo seguinte:

		public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Processing events");
            string eventValue = (string)tuple.GetValue(0);
            if (eventValue != null)
            {
                JObject eventData = JObject.Parse(eventValue);

                Device device = new Device((int)eventData["deviceId"]);
                device.value = (int)eventData["deviceValue"];

                TableOperation insertOperation = TableOperation.Insert(device);

                table.Execute(insertOperation);
                this.ctx.Ack(tuple);
            }
        }

	Isso usa Json.NET para analisar os dados JSON do spout e, em seguida, seleciona os campos **deviceId** e **deviceValue**. Um novo objeto **Dispositivo** é criado, usando a **deviceId** durante a inicialização para definir a chave de partição da tabela. O valor é definido como **deviceValue** e, por fim, a entidade é inserida na tabela.

    Depois que a entidade for inserida na tabela, `Ack()` será chamado para a tupla, para informar o spout de que os dados foram processados com êxito.

    > [AZURE.IMPORTANT]O componente EventHubSpout espera um ACK para cada tupla de componentes downstream, como este bolt. Se ACKS não forem recebidas, o EventHubSpout suporá que o processamento da tupla falhou.

Neste ponto, você tem uma topologia completa que lê dados de Hub de Eventos e os armazena no Armazenamento de Tabela em uma tabela chamada **eventos**.

## Implantar as topologias

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **EventHubReader** e escolha **Enviar para o Storm no HDInsight**.

	![enviar para o storm](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. Na caixa de diálogo **Enviar Topologia**, selecione seu **Cluster Storm**. Expanda **Configurações Adicionais**, selecione **Caminhos de Arquivo Java**, selecione **...** e o diretório que contém o arquivo **eventhubs-storm-spout-0.9-jar-with-dependencies.jar** baixado anteriormente. Por fim, clique em **Enviar**.

	![Imagem da caixa de diálogo de envio](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. Depois que a topologia tiver sido enviada, o **Visualizador de Topologias Storm** será exibido. Selecione a topologia **EventHubReader** do lado esquerdo da caixa de diálogo para exibir estatísticas da topologia. Atualmente, nada deveria estar acontecendo, pois nenhum evento foi gravado no Hub de Eventos ainda.

	![exibição de armazenamento de exemplo](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **EventHubWriter** e escolha **Enviar para o Storm no HDInsight**.

2. Na caixa de diálogo **Enviar Topologia**, selecione seu **Cluster Storm**. Expanda **Configurações Adicionais**, selecione **Caminhos de Arquivo Java**, selecione **...** e o diretório que contém o arquivo **eventhubs-storm-spout-0.9-jar-with-dependencies.jar** baixado anteriormente. Por fim, clique em **Enviar**.

5. Depois que a topologia tiver sido enviada, atualize a lista de topologia no **Visualizador de Topologias Storm** para verificar se ambas estão em execução no cluster.

6. Quando ambas as topologias estiverem em execução, selecione **Gerenciador de Servidores**, expanda **Azure** > **Armazenamento** e selecione a Conta de Armazenamento criada anteriormente. Na Conta de Armazenamento, expanda **Tabelas**. Por fim, clique duas vezes na tabela **eventos** para abrir a tabela. Você deve ver os dados que foram armazenados na tabela por meio da topologia **EventHubReader**.

	* Os eventos são gerados pela topologia **EventHubWriter**, que os grava no Hub de Eventos.

	* Em seguida, o **EventHubReader** lê os eventos de Hubs de Eventos e os armazena no armazenamento de Tabela na tabela **eventos**.

## Interromper as topologias

Para interromper as topologias, selecione cada topologia no **Visualizador de Topologia Storm** e selecione **Eliminar**.

![imagem de eliminação de uma topologia](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

## Observações

### Configuração

Há vários métodos sobrecarregados na criação de EventHubSpoutConfig. Use as informações abaixo para encontrar aquele que melhor atenda às suas necessidades.

* EventHubSpoutConfig(String PolicyName, String PolicyKey, String Namespace, String HubName, Int PartitionCount)

    * PolicyName: o nome da Política de Acesso Compartilhado que pode ler o hub especificado.

    * PolicyKey: a chave para a Política de Acesso Compartilhado.

    * Namespace: o namespace ServiceBus que contém o hub.

    * HubName: o nome do Hub de Eventos a ser lido

    * PartitionCount: o número de partições para o hub

* EventHubSpoutConfig(String PolicyName, String PolicyKey, String Namespace, String HubName, Int PartitionCount, String ZooKeeperConnection)

    Além das propriedades descritas anteriormente:

    * ZooKeeperConnection: a cadeia de conexão para o nó ZooKeeper. Deixe em branco para Storm em servidores HDInsight.

* EventHubSpoutConfig(String PolicyName, String PolicyKey, String Namespace, String HubName, Int PartitionCount, String ZooKeeperConnection, Int CheckPointIntervalInSeconds,Int ReceiverCredits)

    Além das propriedades descritas anteriormente:

    * CheckPointIntervalInSeconds: com que frequência o estado é mantido para o Zookeeper

    * ReceiverCredits: o número de eventos divididos em lotes antes de serem liberados para a topologia Storm.

* EventHubSpoutConfig(String PolicyName, String PolicyKey, String Namespace, String HubName, Int PartitionCount, String ZooKeeperConnection, Int CheckPointIntervalInSeconds, Int ReceiverCredits, Int MaxPendingMsgsPerPartition, Long EnqueueTimeFilter)

    Além das propriedades descritas anteriormente:

    * MaxPendingMsgsPerPartition: o número máximo de eventos buscados do hub. O padrão é 1024.

    * EnqueueTimeFilter: filtra eventos de acordo com o carimbo de data/hora em que o evento foi enfileirado.

### Ponto de verificação

O EventHubSpout cria periodicamente pontos de verificação para seu estado para o nó Zookeeper, que salva o deslocamento atual para mensagens lidas da fila. Isso permite que o componente comece a receber mensagens no deslocamento salvo nos seguintes cenários:

* A instância de componente falha e é reiniciada.

* Você aumenta ou reduz o cluster ao adicionar ou remover nós.

* A topologia é interrompida e reiniciada **com o mesmo nome**.

Você também pode exportar e importar os pontos de verificação persistentes para WASB (Armazenamento do Azure usado pelo seu cluster HDInsight). Os scripts para fazer isso estão localizados no Storm no cluster HDInsight, em **c:\\apps\\dist\\storm-0.9.3.2.2.1.0-2340\\zkdatatool-1.0\\bin**.

>[AZURE.NOTE]O número de versão no caminho pode ser diferente, já que a versão do Storm instalada no cluster pode ser alterada no futuro.

Os scripts que estão nesse diretório são:

* **stormmeta_import.cmd**: importe todos os metadados do Storm do contêiner de armazenamento de cluster padrão para o Zookeeper.

* **stormmeta_export.cmd**: exporte todos os metadados do Storm do Zookeeper para o contêiner de armazenamento do cluster padrão.

* **stormmeta_delete.cmd**: exclua todos os metadados do Storm do Zookeeper.

Exportar uma importação permite que você mantenha os dados de ponto de verificação quando precisar excluir o cluster, mas quiser retomar o processamento do deslocamento atual no hub quando colocar um novo cluster novamente online.

> [AZURE.NOTE]Como os dados são mantidos para o contêiner de armazenamento padrão, o novo cluster **deverá** usar a mesma conta de armazenamento e o contêiner do cluster anterior.

## Resumo

Neste documento, você aprendeu a usar o Bolt e o Spout dos Hub de Eventos Java de uma topologia C# para trabalhar com dados no Hub de Eventos do Azure. Para saber mais sobre a criação de topologias de C#, consulte o seguinte.

* [Desenvolver topologias C# para o Apache Storm no HDInsight usando o Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [Topologias de exemplo para Storm no HDInsight](hdinsight-storm-example-topology.md)

<!--HONumber=54--> 