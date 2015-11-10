<properties 
	pageTitle="Tutorial de visualização do Banco de Dados de Documentos e do Power BI | Microsoft Azure" 
	description="Saiba como usar o Power BI Desktop para criar relatórios interessantes com dados JSON do Banco de Dados de Documentos, um banco de dados de documentos NoSQL totalmente gerenciado." 
	services="documentdb" 
	authors="h0n" 
	manager="jhubbard" 
	editor="mimig" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="10/22/2015" 
	ms.author="hawong"/>

# Visualizando dados do Banco de Dados de Documentos com o Power BI
[PowerBI.com](https://powerbi.microsoft.com/) é um serviço online em que você pode criar e compartilhar painéis e relatórios com dados que são importantes para você e sua organização. O Power BI Desktop é uma ferramenta dedicada para a criação de relatórios que permite recuperar dados de várias fontes de dados, mesclar e transformar os dados, criar relatórios e visualizações avançados e publicar os relatórios no Power BI. Com a versão mais recente do Power BI Desktop, agora você pode se conectar à sua conta do Banco de Dados de Documentos por meio do conector do Banco de Dados de Documentos para o Power BI.

Neste tutorial, percorreremos as etapas para nos conectarmos a uma conta do Banco de Dados de Documentos no Power BI Desktop, navegaremos até uma coleção em que desejamos extrair os dados usando o Navegador, transformaremos os dados JSON em formato tabular usando o Editor de Consultas do Power BI Desktop e criaremos e publicaremos um relatório no PowerBI.com.

Após ler este artigo, você poderá responder as perguntas a seguir:

-	Como posso criar relatórios com dados do Banco de Dados de Documentos usando o Power BI Desktop? 
-	Como posso me conectar a uma conta do Banco de Dados de Documentos no Power BI Desktop?
-	Como posso recuperar dados de uma coleção no Power BI Desktop?
-	Como posso transformar dados JSON aninhados no Power BI Desktop?
-	Como posso publicar e compartilhar meus relatórios no PowerBI.com? 

## Pré-requisitos

Antes de seguir as instruções deste artigo, verifique se você tem os seguintes itens instalados:

- [A versão mais recente do Power BI Desktop](https://powerbi.microsoft.com/desktop).
- Acesse nossa conta ou dados de demonstração em sua conta do Banco de Dados de Documentos do Azure. 
	- A conta de demonstração é populada com os dados do vulcão mostrados neste tutorial. Essa conta de demonstração não está vinculada por nenhum SLA e se destina apenas a fins de demonstração. Reservamos o direito de fazer modificações a essa conta de demonstração, incluindo, entre outros, encerramento da conta, alteração da chave, restrição do acesso, alteração e exclusão dos dados, a qualquer momento, sem aviso prévio ou justificativa. 
		- URL: https://analytics.documents.azure.com
		- Chave somente leitura: MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==
	- Ou, para criar sua própria conta, veja [Criar uma conta de banco de dados do Banco de Dados de Documentos usando o portal de visualização do Azure](https://azure.microsoft.com/documentation/articles/documentdb-create-account/). Em seguida, para obter dados de vulcão de exemplo semelhantes aos que são usados neste tutorial (mas que não contêm os blocos GeoJSON), confira o [site da NOAA](https://www.ngdc.noaa.gov/nndc/struts/form?t=102557&s=5&d=5) e importe os dados usando a [ferramenta de migração de dados do Banco de Dados de Documentos](https://azure.microsoft.com/documentation/articles/documentdb-import-data/).


Para compartilhar seus relatórios no PowerBI.com, você deve ter uma conta no PowerBI.com. Para saber mais sobre o Power BI Gratuito e o Power BI Pro, visite [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing).

## Vamos começar
Neste tutorial, vamos imaginar que você é um geólogo que estuda os vulcões em todo o mundo. Os dados de vulcão são armazenados em uma conta do Banco de Dados de Documentos e os documentos JSON são semelhantes ao exemplo mostrado abaixo.

	{
    	"Volcano Name": "Rainier",
   		"Country": "United States",
  		"Region": "US-Washington",
  		"Location": {
    		"type": "Point",
    		"coordinates": [
      		-121.758,
      		46.87
    		]
  		},
  		"Elevation": 4392,
  		"Type": "Stratovolcano",
  		"Status": "Dendrochronology",
  		"Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
	}	

Você deseja recuperar os dados de vulcão da conta do Banco de Dados de Documentos e produzir um relatório interativo do Power BI como o mostrado abaixo.

![Relatório sobre Vulcões do Power BI Desktop](./media/documentdb-powerbi-visualize/pbireportfinal.png)

Pronto para experimentar? Vamos começar.

## Conectando-se a uma conta do Banco de Dados de Documentos

1. Execute o Power BI Desktop em sua estação de trabalho.
2. Depois de iniciar o Power BI Desktop, uma tela de *Boas-Vindas* é exibida.

	![Tela de boas-vindas do Power BI Desktop](./media/documentdb-powerbi-visualize/welcomescreen.png)

3. Você pode **Obter Dados**, ver as **Fontes Recentes** ou **Abrir Outros Relatórios** diretamente na tela de *Boas-vindas*. Clique no X no canto superior direito para fechar a tela. A exibição de **Relatório** do Power BI Desktop é exibida.

	![Exibição de Relatório do Power BI Desktop](./media/documentdb-powerbi-visualize/pbireportview.png)

4. Selecione a faixa de opções **Página Inicial** e clique em **Obter Dados**. A janela **Obter Dados** deve ser exibida.

5. Clique em **Azure**, selecione **Banco de Dados de Documentos do Microsoft Azure (Beta)** e clique em **Conectar**. A janela **Conexão do Banco de Dados de Documentos do Microsoft Azure** deve ser exibida.

	![Obter Dados do Power BI Desktop](./media/documentdb-powerbi-visualize/pbigetdata.png)

6. Especifique a URL do ponto de extremidade da conta do Banco de Dados de Documentos da qual você gostaria de recuperar os dados, como mostrado abaixo, e clique em **OK**. Você pode recuperar a URL na caixa de URI na folha **Chaves** do portal de visualização do Azure ou usar as informações da conta de demonstração fornecidas acima. Para obter mais informações, veja [Chaves](documentdb-manage-account.md#keys).


	*Observação. Neste tutorial, não especificaremos o nome do Banco de Dados, o nome da Coleção ou uma instrução SQL, já que esses campos são opcionais. Em vez disso, usaremos o Navegador para selecionar o Banco de Dados e a Coleção para identificar a origem dos dados.*

    ![Janela de Conexão do Power BI Desktop](./media/documentdb-powerbi-visualize/pbiconnectwindow.png)

7. Se estiver se conectando a este ponto de extremidade pela primeira vez, você será solicitado a fornecer a chave de conta. Insira a chave de conta e clique em **Conectar**.
	
	*Observação. Recomendamos que você use a chave somente leitura ao criar relatórios. Isso impedirá a exposição desnecessária da chave mestra a possíveis riscos de segurança. A chave somente leitura está disponível na folha Chaves Somente Leitura do portal de visualização do Azure, ou você pode usar as informações da conta de demonstração fornecidas acima.*

    ![Chave de Conta do Power BI Desktop](./media/documentdb-powerbi-visualize/pbidocumentdbkey.png)

8. Quando a conta for conectada com êxito, o **Navegador** será exibido. O **Navegador** mostrará uma lista de bancos de dados na conta.
9. Clique e expanda o banco de dados de origem dos dados do relatório. Uma lista de coleções no banco de dados será exibida.  

10. Agora, selecione uma coleção da qual você recuperará os dados, por exemplo, volcano1.

	*Observação. O painel Visualização mostra uma lista dos itens de **Registro**. Um Documento é representado como um tipo **Registro** no Power BI. Da mesma forma, um bloco JSON aninhado dentro de um documento é também um **Registro**.*

    ![Navegador do Power BI Desktop](./media/documentdb-powerbi-visualize/pbinavigator.png)

11. Clique em **Editar** para iniciar o Editor de Consultas para que possamos transformar os dados.

## Nivelando e transformando documentos JSON
1. No Editor de Consultas do Power BI, você deve ver uma coluna **Documento** no painel central. ![Editor de Consultas do Power BI Desktop](./media/documentdb-powerbi-visualize/pbiqueryeditor.png)

2. Clique no expansor no lado direito do cabeçalho da coluna **Documento**. O menu de contexto com uma lista de campos será exibido. Selecione os campos necessários para o relatório, por exemplo, Nome do Vulcão, País, Região, Local, Elevação, Tipo, Status e Última Erupção Conhecida e clique em **OK**.
    
	![Expandir Documentos do Power BI Desktop](./media/documentdb-powerbi-visualize/pbiqueryeditorexpander.png)

3. O painel central exibirá uma visualização do resultado com os campos selecionados.

	![Nivelar Resultado do Power BI Desktop](./media/documentdb-powerbi-visualize/pbiresultflatten.png)

4. Em nosso exemplo, a propriedade Location é um bloco GeoJSON em um documento. Como você pode ver, o Local é representado como um tipo **Registro** no Power BI Desktop.
5. Clique no expansor no lado direito do cabeçalho da coluna Local. O menu de contexto com os campos de tipo e coordenadas será exibido. Vamos selecionar o campo de coordenadas e clicar em **OK**.

    ![Registro de Local do Power BI Desktop](./media/documentdb-powerbi-visualize/pbilocationrecord.png)

6. Agora, o painel central mostra uma coluna de coordenadas do tipo **Lista**. Como mostrado no início do tutorial, os dados GeoJSON neste tutorial são do tipo Ponto com valores de Latitude e Longitude gravados na matriz de coordenadas.

	*Observação: o elemento coordinates[0] representa a Longitude, enquanto que coordinates[1] representa a Latitude.* ![Lista de Coordenadas do Power BI Desktop](./media/documentdb-powerbi-visualize/pbiresultflattenlist.png)

7. Para nivelar a matriz de coordenadas, criaremos uma **Coluna Personalizada** chamada LatLong. Selecione a faixa de opções **Adicionar Coluna** e clique em **Adicionar Coluna Personalizada**. A janela **Adicionar Coluna Personalizada** deve ser exibida.

8. Forneça um nome para a nova coluna, por exemplo, LatLong.

9. Em seguida, especifique a fórmula personalizada para a nova coluna. Para nosso exemplo, os valores de Latitude e Longitude, separados por uma vírgula, serão concatenados com a seguinte fórmula, como mostrado abaixo: Text.From([coordinates]{1})&","&Text.From([coordinates]{0}). Clique em **OK**.
	
    *Observação. Para obter mais informações sobre o DAX (Data Analysis Expressions), incluindo as funções DAX, visite [Noções básicas do DAX no Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/554619-dax-basics-in-power-bi-desktop).*

    ![Coluna Personalizada do Power BI Desktop](./media/documentdb-powerbi-visualize/pbicustomlatlong.png)

10. Agora, o painel central mostrará a nova coluna LatLong populada com os valores de Latitude e Longitude, separados por uma vírgula.

	![LatLong Personalizado do Power BI Desktop](./media/documentdb-powerbi-visualize/pbicolumnlatlong.png)

11. Agora concluímos o nivelamento dos dados em formato tabular. Você pode aproveitar todos os recursos disponíveis no Editor de Consultas para formatar e transformar dados no Banco de Dados de Documentos. Por exemplo, é possível alterar o tipo de dados de Elevação para **Número Decimal** alterando o **Tipo de Dados** na faixa de opções **Página Inicial**.

    ![Tipo de Alteração do Power BI Desktop](./media/documentdb-powerbi-visualize/pbichangetype.png)

12. Clique em **Fechar e Aplicar** para salvar o modelo de dados.
    
    ![Fechar e Aplicar do Power BI Desktop](./media/documentdb-powerbi-visualize/pbicloseapply.png)

## Compilar os relatórios
A exibição de Relatório do Power BI Desktop é em que você pode começar a criar relatórios. Você pode criar relatórios arrastando e soltando campos na tela **Relatório**.

![Exibição de Relatório do Power BI Desktop](./media/documentdb-powerbi-visualize/pbireportview2.png)
 
Na exibição de Relatório, você deve encontrar:

 1. O painel **Campos** é onde você verá uma lista de modelos de dados com campos que podem ser usados para seus relatórios.

 2. O painel **Visualizações**. Um relatório pode conter uma ou várias visualizações. Selecione os tipos de elementos visuais que atendem às suas necessidades no painel **Visualizações**.

 3. A tela **Relatório** é onde você criará os elementos visuais para seu relatório.

 4. A página **Relatório**. Você pode adicionar várias páginas do relatório no Power BI Desktop.

O exemplo a seguir mostra as etapas básicas da criação de um relatório interativo simples da exibição de Mapa.

1. Para nosso exemplo, criaremos uma exibição de mapa mostrando o local de cada vulcão. No painel **Visualizações**, clique no tipo de elemento visual Mapa, como destacado na captura de tela acima. Você deverá ver o tipo de elemento visual Mapa pintado na tela **Relatório**. O painel **Visualização** também deve exibir um conjunto de propriedades relacionadas ao tipo de elemento visual Mapa.

2. Agora, arraste e solte o campo LatLong do painel **Campos** para a propriedade **Location** no painel **Visualizações**.
3. Em seguida, arraste e solte o campo Nome do Vulcão para a propriedade **Legend**.  

4. Em seguida, arraste e solte o campo Elevação para a propriedade **Values**.

5. Agora você deverá ver o elemento visual Mapa mostrando um conjunto de bolhas que indica o local de cada vulcão com o tamanho da bolha correlacionando à elevação do vulcão.

6. Agora você criou um relatório básico. É possível personalizar ainda mais o relatório adicionando mais visualizações. Em nosso caso, adicionamos uma segmentação de dados do Tipo de Vulcão para tornar o relatório interativo.

    ![Relatório Final do Power BI Desktop](./media/documentdb-powerbi-visualize/pbireportfinal.png)

## Publicar e compartilhar seu relatório
Para compartilhar seu relatório, você deve ter uma conta no PowerBI.com.

1. No Power BI Desktop, clique na faixa de opções **Página Inicial**.
2. Clique em **Publicar**. Você será solicitado a inserir o nome de usuário e a senha da sua conta do PowerBI.com.
3. Depois que a credencial foi autenticada, o relatório é publicado em sua conta do PowerBI.com.
4. Em seguida, você poderá compartilhar seu relatório no PowerBI.com.

## Próximas etapas
- Para saber mais sobre o Power BI, clique [aqui](https://support.powerbi.com/knowledgebase)
- Para saber mais sobre o Banco de Dados de Documentos, clique [aqui](https://azure.microsoft.com/documentation/services/documentdb/).

<!---HONumber=Oct15_HO4-->