## Configurar uma cadeia de conexão de armazenamento

A Biblioteca de Cliente de Armazenamento do Azure para .NET oferece suporte ao uso de uma cadeia de conexão de armazenamento para configurar pontos de extremidade e credenciais para acesso a serviços de armazenamento. Recomendamos a você colocar a cadeia de conexão de armazenamento em um arquivo de configuração, em vez de embuti-la no código de seu aplicativo. Você tem duas opções para salvar sua cadeia de conexão:

- Ao usar os Serviços de Nuvem do Azure, é recomendável armazenar a cadeia de conexão usando o sistema de configuração de serviço do Azure (arquivos`*.csdef` e `*.cscfg`).
- Se seu aplicativo é executado em máquinas virtuais do Azure, ou se você está compilando aplicativos .NET destinados a serem executados fora do Azure, é recomendável armazenar a cadeia de conexão usando o sistema de configuração do .NET (por exemplo, arquivo `web.config` ou `app.config`).

Posteriormente neste guia, lhe mostraremos como obter sua cadeia de conexão a partir de seu código.

### Configurar sua cadeia de conexão por meio de um serviço de nuvem do Azure

Os Serviços de Nuvem do Azure têm um mecanismo de configuração de serviço exclusivo, que permite que você altere dinamicamente os parâmetros de configuração no Portal de Gerenciamento do Azure sem reimplantar o aplicativo.

Para configurar a cadeia de conexão na configuração de serviço do Azure:

1.  No Gerenciador de Soluções do Visual Studio, na pasta **Funções**
    do seu Projeto de implantação do Azure, clique com o botão direito do mouse em sua
    função web ou função de trabalho e clique em **Propriedades**.  
       ![Select the properties on a Cloud Service role in Visual Studio][connection-string1]]

2.  Clique na guia **Configurações** e pressione o botão **Adicionar Configuração**.  
    ![Add a Cloud Service setting in visual Studio][connection-string2

    Uma nova entrada **Configuração1** será mostrada na grade de configurações.

3.  No menu suspenso **Tipo** da nova entrada **Configuração1**, escolha
    **Cadeia de conexão**.  
    ![Set connection string type][connection-string3]

4.  Clique no botão **...** na extremidade direita da entrada **Setting1**.
    A caixa de diálogo **Cadeia de Conexão da Conta de Armazenamento** será aberta.

5.  Escolha se deseja ou não direcionar o emulador de armazenamento (armazenamento
    do Microsoft Azure simulado em seu computador local) ou uma conta
    de armazenamento na nuvem. O código neste guia funciona com qualquer uma dessas
    opções. 

	> [AZURE.NOTE] Você pode utilizar o emulador de armazenamento como destino para evitar quaisquer custos associados ao Armazenamento do Microsoft Azure. Todavia, se você escolher utilizar como destino uma conta de armazenamento do Azure na nuvem, os custos para realização das operações neste tutorial serão insignificantes.

	Se você tiver como alvo uma conta de armazenamento na nuvem, insira a chave de acesso primário para essa conta de armazenamento. Para saber como copiar a chave de acesso primária por meio do Portal de Gerenciamento do Azure, consulte 	
	[Exibir, copiar e regenerar chaves de acesso de armazenamento](http://azure.microsoft.com/documentation/articles/storage-create-storage-account/#view-copy-and-regenerate-storage-access-keys).
	
    ![Select target environment][connection-string4]

6.  Altere a entrada **Nome** em **Configuração1** para um nome mais amigável,
    como **CadeiaDeConexãoDeArmazenamento**. Você fará referência a essa
    cadeia de conexão posteriormente no código deste guia.  
    ![Change connection string name][connection-string5]
	
### Configurando sua cadeia de conexão usando configuração do .NET

Se você estiver escrevendo um aplicativo que não seja um serviço de nuvem do Azure (consulte a seção anterior), será recomendável usar o sistema de configuração do .NET (por exemplo, `web.config` ou `app.config`). Isso inclui sites ou máquinas virtuais do Azure, bem como aplicativos projetados para serem executados fora do Azure. Você armazena a cadeia de conexão usando o elemento `<appSettings>` conforme descrito a seguir. Substitua `account-name` pelo nome da sua conta de armazenamento e `account-key` pela chave de acesso da conta de armazenamento:

	<configuration>
  		<appSettings>
    		<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
  		</appSettings>
	</configuration>

Por exemplo, o parâmetro de configuração em seu arquivo de configuração pode ser similar a:

	<configuration>
    	<appSettings>
      		<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=nYV0gln9fT7bvY+rxu2iWAEyzPNITGkhM88J8HUoyofpK7C8fHcZc2kIZp6cKgYRUM74lHI84L50Iau1+9hPjB==" />
    	</appSettings>
	</configuration>

Agora você está pronto para executar as tarefas das instruções deste guia.

[connection-string1]: ./media/storage-configure-connection-string-include/connection-string1.png
[connection-string2]: ./media/storage-configure-connection-string-include/connection-string2.png
[connection-string3]: ./media/storage-configure-connection-string-include/connection-string3.png
[connection-string4]: ./media/storage-configure-connection-string-include/connection-string4.png
[connection-string5]: ./media/storage-configure-connection-string-include/connection-string5.png

[Configurando cadeias de conexão (a página pode estar em inglês)]: http://msdn.microsoft.com/library/azure/ee758697.aspx

<!--HONumber=49-->