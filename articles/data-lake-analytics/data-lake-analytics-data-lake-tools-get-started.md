<properties 
   pageTitle="Desenvolver scripts U-SQL usando as Ferramentas do Data Lake para Visual Studio | Azure" 
   description="Saiba como instalar ferramentas do Data Lake para o Visual Studio, como desenvolver e testar scripts U-SQL." 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/28/2015"
   ms.author="jgao"/>

# Tutorial: desenvolver scripts U-SQL usando as Ferramentas do Data Lake para Visual Studio

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Saiba como instalar e usar as Ferramentas do Data Lake para Visual Studio para escrever e testar scripts U-SQL.

U-SQL é uma linguagem altamente extensível e hiperescalonável para preparação, transformação e análise de todos os dados no Data Lake e além dele. Para saber mais, consulte [Referência do U-SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).


**Pré-requisitos**

- **Visual Studio 2015, Visual Studio 2013 atualização 4 ou Visual Studio 2012 com Visual C++ instalado** 
- **SDK do Microsoft Azure para .NET versão 2.5 ou posterior**. Instale-o usando o [Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).
- **[Ferramentas do Data Lake para Visual Studio](http://aka.ms/adltoolsvs)**. 

    Após a instalação das Ferramentas do Data Lake para Visual Studio, você verá um menu do Data Lake no Visual Studio:
    
    ![Menu U-SQL do Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Explore as duas seções a seguir em [Introdução à Análise Azure Data Lake usando o Portal de Visualização do Azure](data-lake-analytics-get-started-portal.md)**.

	- [Criar uma conta da Análise Data Lake do Azure](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).
	- [Carregar SearchLog.tsv na conta padrão do Repositório Data Lake](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

	As Ferramentas do Data Lake não dão suporte à criação de contas da Análise Data Lake. Por isso, você precisa criá-la usando o Portal de Visualização do Azure, o Azure PowerShell, o SDK do .NET ou a CLI do Azure. Para executar o trabalho da Análise Data Lake, você precisará de alguns dados. Embora as Ferramentas do Data Lake deem suporte ao carregamento de dados, você poderá usar o portal para carregar os dados de exemplo para deixar o tutorial mais fácil de acompanhar.

## Conecte-se ao Azure

**Para conectar-se à Análise Data Lake**

1. Abra o Visual Studio.
2. No menu **U-SQL**, clique em **Opções e Configurações**.
4. Clique em **Entrar** ou em **Alterar Usuário** se alguém estiver conectado e siga as instruções para entrar.
5. Clique em **OK** para fechar a caixa de diálogo Opções e Configurações.

**Para procurar as suas contas de Análise Data Lake**

1. No Visual Studio, abra o **Gerenciador de Servidores** pressionando **CTRL+ALT+S**.
2. No **Gerenciador de Servidores**, expanda **Azure** e expanda **Análise Data Lake**. Você deverá ver uma lista das suas contas da Análise Data Lake, caso haja alguma. Não é possível criar contas de Análise Data Lake no Visual Studio. Para criar uma conta, consulte [Introdução à Análise Data Lake do Azure usando o Portal de Visualização do Azure](data-lake-analytics-get-started-portal.md) ou [Introdução à Análise Data Lake do Azure usando o Azure PowerShell](knoa-get-started-powershell.md).

## Carregar arquivos de dados de origem

Anteriormente neste tutorial, você carregou alguns dados na seção **Pré-requisito**.

Caso você queira usar seus próprios dados, estes são os procedimentos para carregar dados das Ferramentas do Data Lake.

**Para carregar o arquivo na conta dependente do Azure Data Lake**

1. No **Gerenciador de Servidores**, expanda **Azure**, expanda **Análise Data Lake**, expanda sua conta de Análise Data Lake, expanda **Contas de Armazenamento**. Você deverá ver a conta padrão do Repositório Data Lake, as contas vinculadas do Repositório Data Lake e as contas de Armazenamento do Azure vinculadas. A conta padrão do Data Lake tem um rótulo "Conta de Armazenamento Padrão".
2. Clique com o botão direito na conta padrão do Repositório Data Lake e clique em **Gerenciador**. Isso abrirá o painel Gerenciador das Ferramentas do Data Lake para Visual Studio. À esquerda, há um modo de exibição de árvore e o modo de exibição de conteúdo está à direita.
3. Navegue até a pasta na qual você deseja carregar arquivos, 
4. Clique com o botão direito do mouse em qualquer espaço em branco e clique em **Carregar**. 

	![U-SQL do Projeto de U-SQL do Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-upload-files.png)

**Para carregar o arquivo na conta de Armazenamento de Blob do Azure**

1. No **Gerenciador de Servidores**, expanda **Azure**, expanda **Análise Data Lake**, expanda sua conta de Análise Data Lake, expanda **Contas de Armazenamento**. Você deverá ver a conta padrão do Repositório Data Lake, as contas vinculadas do Repositório Data Lake e as contas de Armazenamento do Azure vinculadas. 
2. Expanda a conta de Armazenamento do Azure.
3. Clique com o botão direito no contêiner onde você deseja carregar arquivos e clique em **Gerenciador**.
4. Navegue até a pasta na qual você deseja carregar arquivos, 
5. Clique com o botão direito do mouse em qualquer espaço em branco e clique em **Carregar**. 

## Desenvolver e testar scripts U-SQL 

Os trabalhos de Análise Data Lake são escritos na linguagem U-SQL. Para saber mais sobre o U-SQL, consulte [Introdução à linguagem U-SQL](data-lake-analytics-u-sql-get-started.md) e [Referência da linguagem U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**Para criar e enviar um trabalho da Análise Data Lake**

1. No menu **Arquivo**, clique em **Novo** e em **Projeto**.
2. Selecione o tipo do **Projeto de U-SQL**.

	![novo projeto de U-SQL do Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Clique em **OK**. O Visual Studio cria uma solução com um arquivo **Script.usql**.
4. Insira o seguinte script em **Script.usql**:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

	Este script U-SQL lê o arquivo de dados de origem usando **Extractors.Tsv()** e cria um arquivo .csv usando **Outputters.Csv()**.
    
    Não modifique os dois caminhos, a menos que você tenha copiado o arquivo de origem em um local diferente. A Análise Data Lake criará a pasta de saída se ela não existir.
	
	É mais simples usar caminhos relativos para arquivos armazenados em contas padrão do Data Lake. Você também pode usar caminhos absolutos. Por exemplo,
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Você deve usar caminhos absolutos para acessar os arquivos em contas do Armazenamento vinculadas. A sintaxe para os arquivos armazenados na conta do Armazenamento do Azure vinculada é:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE]Atualmente, não há suporte para o contêiner de Blob do Azure com permissões de acesso de blobs públicos ou de contêineres públicos.

	Perceba os seguintes recursos:

	- **IntelliSense**
	 
		Preenchimento automático de nomes e membros exibidos para o Conjunto de linhas, Classes, Bancos de dados, Esquemas e Objetos definidos pelo usuário (UDOs).
		 
		IntelliSense para entidades do catálogo (Bancos de dados, Esquemas, Tabelas, UDOs etc.) tem relação à sua conta de computação. Você pode verificar a conta de computação ativa atual, o banco de dados e o esquema na barra de ferramentas superior e alterne entre elas nas listas suspensas.
 
	- **Formatação Automática**
	
		Os usuários podem alterar o recuo do script Escopo com base na estrutura de código em Editar -> Avançado:
  
		- Formatar Documento (Ctrl + E, D): Formata o documento inteiro   
		- Formatar Seleção (Ctrl + K, Ctrl + F): Formata a seleção. Se nenhuma seleção for feita, esse atalho formatará a linha na qual o cursor está posicionado.  
		
		Todas as regras de formatação podem ser configuradas em Ferramentas -> Opções -> Editor de Texto - > SIP -> Formatação.  
	- **Recuo Inteligente**
	 
		As Ferramentas do Data Lake para Visual Studio recuam automaticamente as expressões enquanto você escreve os scripts. Esse recurso está desabilitado por padrão, e os usuários precisam habilitá-lo marcando a opção U-SQL -> Opções e Configurações -> Opções -> Habilitar o Recuo Inteligente.

	- **Ir para Definição e Localizar Todas as Referências**
	
		Clicar com o botão direito no nome de um Conjunto de linhas/parâmetro/coluna/UDO etc e clicar em Ir para Definição (F12) permite que você navegue até a definição. Ao clicar em Localizar Todas as Referências (Shift+F12), todas as referências serão exibidas.

	- **Inserir Caminho do Azure**
		
		Em vez de memorizar o caminho do arquivo do Azure e digitá-lo manualmente ao escrever o script, as Ferramentas do Data Lake para Visual Studio fornecem uma maneira mais fácil: clique com o botão direito no editor e clique em Inserir Caminho do Azure. Navegue até o arquivo na caixa de diálogo do Navegador de Blob do Azure. Clique no botão OK e o caminho do arquivo será inserido em seu código.

5. Especifique a conta de Análise Data Lake, o Banco de dados e o Esquema:

	![Enviar projeto de U-SQL do Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

    Para saber mais, consulte [Usar o Catálogo do U-SQL](data-lake-analytics-use-u-sql-catalog.md).

5. No **Gerenciador de Soluções**, clique com botão direito do mouse em **Script.usql** e clique em **Criar Script**. Verifique o resultado no painel Saída.
6. No **Gerenciador de Soluções**, clique com botão direito do mouse em **Script.usql** e clique em **Enviar Script**. Como opção, também é possível clicar em **Enviar** no painel Script.usql. Consulte a captura de tela anterior. Clique na seta para baixo ao lado do botão Enviar para enviar usando as opções avançadas:
7. Especifique o **Nome do Trabalho**, verifique a **Conta de Análise** e clique em **Enviar**. Os resultados do envio e o link do trabalho ficarão disponíveis na janela de resultados das Ferramentas do Date Lake para Visual Studio quando o envio for concluído.

	![Enviar projeto de U-SQL do Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)

8. Clique no botão Atualizar para ver o status do trabalho mais recente e atualizar a tela. Após a conclusão bem-sucedida do trabalho, ele exibirá **Gráfico do Trabalho**, **Operações de Metadados**, **Histórico de Estado**, **Diagnósticos**:

	![Gráfico de desempenho de trabalho de Análise Data Lake do SQL-U do Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

	* Resumo do Trabalho. Mostra as informações resumidas do trabalho atual, por exemplo: Estado, Progresso, Tempo de Execução, Nome do Tempo de Execução, Emissor etc.   
	* Detalhes do Trabalho. Inclui informações detalhadas sobre esse trabalho, incluindo o script, recursos, Exibição de Execuções de Vértice.
	* Gráfico do Trabalho. Há quatro gráficos para visualização das informações do trabalho: Progresso, Leitura de Dados, Dados Gravados, Tempo de Execução, Tempo de Execução Médio por Nó, Taxa de Transferência de Entrada, Taxa de Transferência de Saída.
	* Operações de Metadados. Mostra todas as operações de metadados.
	* Histórico de Estado. 
	* Diagnósticos. As Ferramentas do Data Lake para Visual Studio diagnosticarão automaticamente a execução do trabalho. Você receberá alertas quando houver algum erro ou problemas de desempenho em seus trabalhos. Consulte a parte Diagnósticos do Trabalho (link a ser definido) para saber mais. 
	
**Para verificar o estado do trabalho**

1. No Gerenciador de Servidores, expanda **Azure**, expanda **Análise Data Lake**, expanda o nome de sua conta de Análise Data Lake.
2. Clique duas vezes em **Trabalhos** para listar os trabalhos.
2. Clique em um trabalho para ver o status.

**Para ver a saída do trabalho**

1. No **Gerenciador de Servidores**, expanda o **Azure**, expanda a **Análise Data Lake**, expanda sua conta da Análise Data Lake, expanda **Contas de Armazenamento**, clique com o botão direito do mouse na conta padrão do Repositório Data Lake e clique em **Gerenciador**. 
2.  Clique duas vezes em **saída** para abrir a pasta
3.  Clique duas vezes em **SearchLog-From-adltools.csv**.


###Reprodução do Trabalho 

A Reprodução do Trabalho permite que você assista ao progresso da execução do trabalho e detecte visualmente os gargalos e anomalias no desempenho. Esse recurso pode ser usado antes da conclusão da execução do trabalho (por exemplo, durante o tempo em que o trabalho está sendo executado ativamente), bem como após a execução. A reprodução durante a execução do trabalho permitirá que o usuário reproduza o andamento até o momento atual.

**Para exibir o progresso da execução de trabalho**

1. Clique em **Carregar Perfil** no canto superior direito. Consulte a captura de tela anterior.
2. Clique no botão Reproduzir no canto inferior esquerdo para analisar o progresso de execução do trabalho. 
3. Durante a reprodução, clique em **Pausar** para interrompê-la ou arraste diretamente a barra de progresso para posições específicas. 


###Mapa de Calor 

As Ferramentas do Data Lake para Visual Studio fornecem sobreposições de cores selecionáveis pelo usuário na exibição do trabalho a fim de indicar o progresso, a E/S de dados, o tempo de execução, a taxa de transferência de E/S de cada estágio. Com isso, os usuários podem descobrir possíveis problemas e distribuir as propriedades do trabalho de forma direta e intuitiva. Você pode escolher uma fonte de dados para exibir na lista suspensa.



##Consulte também

Para começar a usar a Análise Data Lake com ferramentas diferentes, consulte:

- [Introdução à Análise Data Lake usando o Portal de Visualização do Azure](data-lake-analytics-get-started-portal.md)
- [Introdução à Análise Data Lake usando o Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [Introdução à Análise Data Lake usando o SDK do .NET](data-lake-analytics-get-started-net-sdk.md)

Para ver mais tópicos de desenvolvimento:

- [Analisar os weblogs usando a Análise Data Lake](data-lake-analytics-analyze-weblogs.md)
- [Desenvolver scripts U-SQL usando as Ferramentas do Data Lake para Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Introdução à linguagem U-SQL da Análise Data Lake do Azure](data-lake-analytics-u-sql-get-started.md)
- [Desenvolver operadores do U-SQL definidos pelo usuário para trabalhos da Análise Data Lake](data-lake-analytics-u-sql-user-defined-operators.md)

<!---HONumber=Nov15_HO1-->