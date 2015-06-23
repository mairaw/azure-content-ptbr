<properties 
	pageTitle="Configurar uma máquina virtual SQL Server do Azure para ciência de dados" 
	description="Configurar uma Máquina Virtual de Ciência de Dados com o SQL Server e o IPython Server." 
	services="machine-learning" 
	solutions="" documentationCenter="" 
	authors="msolhab,xibingaomsft" 
	manager="paulettm" 
	editor="cgronlun" />

<tags 
	ms.service="machine-learning" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/18/2015" 
	ms.author="mohabib;xibingao" />

# Configurar uma máquina virtual SQL Server do Azure para ciência de dados

Este tópico mostra como provisionar e configurar uma máquina virtual do SQL Server para ser usada como parte de um ambiente de ciência de dados baseado em nuvem.  A máquina virtual do Windows está configurada com ferramentas de suporte como o IPython Notebook, o Azure Storage Explorer e o AzCopy, bem como outros utilitários que são úteis para projetos de ciência de dados.  O Azure Storage Explorer e o AzCopy, por exemplo, fornecem maneiras convenientes para carregar dados no armazenamento de blob do Azure em seu computador local ou baixá-lo em seu computador local por meio do armazenamento de blob.

A galeria de Máquinas Virtuais do Azure inclui várias imagens que contêm o Microsoft SQL Server.  Selecione uma imagem de VM do SQL Server que seja adequada para as suas necessidades de dados.  As imagens recomendadas são:

- SQL Server 2012 SP2 Enterprise para dados de pequeno a médio porte
- SQL Server 2012 SP2 Enterprise otimizado para cargas de trabalho de DataWarehousing para dados de grande a muito grande porte

 > [AZURE.NOTE] A imagem do SQL Server 2012 SP2 Enterprise **não inclui um disco de dados**.  Você precisará adicionar e/ou anexar um ou mais discos rígidos virtuais para armazenar seus dados.  Quando você cria uma máquina virtual do Azure, ela tem um disco para o sistema operacional mapeado para a unidade C e um disco temporário mapeado para a unidade D.  Não utilize a unidade D para armazenar dados.  Como seu nome quer dizer, ele oferece armazenamento apenas temporariamente.  Não oferece redundância nem backup porque eles não residem no armazenamento do Azure.


##<a name="Provision"></a>Conecte-se ao portal de gerenciamento do Azure e forneça uma máquina virtual do SQL Server

1.  Faça logon no [Portal de Gerenciamento do Azure](http://manage.windowsazure.com/) (a página pode estar em inglês) usando sua conta. 
	Se você não tiver uma conta do Azure, visite [Avaliação gratuita do Azure](http://www.windowsazure.com/pricing/free-trial/).

2.  No Portal de Gerenciamento do Azure, na parte inferior esquerda da página da Web, clique em **+NOVO**, **COMPUTAÇÃO**, **MÁQUINA VIRTUAL** e, em seguida, em **DA GALERIA**.

3.  Na página **Criar uma Máquina Virtual**, selecione uma imagem de máquina virtual que contenha o SQL Server com base nas suas necessidades de dados e clique na seta de avanço na parte inferior à direita da página.  Para obter as informações mais atualizadas sobre as imagens do SQL Server com suporte no Azure, consulte o tópico [Introdução ao SQL Server em Máquinas Virtuais do Azure](http://go.microsoft.com/fwlink/p/?LinkId=294720) (a página pode estar em inglês) no conjunto de documentação [SQL Server em Máquinas Virtuais do Azure](http://go.microsoft.com/fwlink/p/?LinkId=294719).

	![Select SQL Server VM][1]

4.  Na primeira página de **Configuração da Máquina Virtual**, forneça as seguintes informações:

    -   Forneça um **NOME DE MÁQUINA VIRTUAL**.
    -   Na caixa **NOVO NOME DE USUÁRIO**, digite um nome de usuário exclusivo para a conta de administrador local da VM.
    -   Na caixa **NOVA SENHA**, digite uma senha forte.  Para obter mais informações, consulte [Senhas fortes (a página pode estar em inglês)](http://msdn.microsoft.com/library/ms161962.aspx).
    -   Na caixa **CONFIRMAR SENHA**, digite a senha novamente.
    -   Selecione o **Tamanho** apropriado na lista suspensa.

     > [AZURE.NOTE] O tamanho da máquina virtual é especificado durante o provisionamento:  A2 é o menor tamanho recomendado para cargas de trabalho de produção.  O tamanho mínimo recomendado para uma máquina virtual é A3 ao usar o SQL Server Enterprise Edition.  Selecione A3 ou maior ao usar o SQL Server Enterprise Edition.  Selecione A4 ao usar o SQL Server 2012 ou 2014 Enterprise otimizado para imagens de cargas de trabalho transacionais.  Selecione A7 ao usar o SQL Server 2012 ou 2014 Enterprise otimizado para imagens de cargas de trabalho do Data Warehouse.  O tamanho selecionado limita o número de discos de dados que você pode configurar.  Para obter as informações mais atualizadas sobre os tamanhos de máquina virtual disponíveis e o número de discos de dados que você pode anexar a uma máquina virtual, consulte [Tamanhos de Máquina Virtual para o Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx).  Para obter informações sobre preços, consulte [Preços de Máquinas Virtuais](http://azure.microsoft.com/pricing/details/virtual-machines/).

    Clique na seta de avanço na parte inferior à direita para continuar.

    ![VM Configuration][2]

5.  Na segunda página **Configuração da máquina virtual**, configure recursos para a rede, o armazenamento e a disponibilidade:

    -   Na caixa **Serviço de Nuvem**, selecione **Criar um novo serviço de nuvem**.
    -   Na caixa **Nome DNS do Serviço de Nuvem**, forneça a primeira parte de um nome DNS de sua escolha, para que ele complete um nome no formato **TESTNAME.cloudapp.net**
    -   Na caixa **REGIÃO/GRUPO DE AFINIDADE/REDE VIRTUAL**, selecione uma região em que esta imagem virtual será hospedada.
    -   Na **Conta de Armazenamento**, selecione uma conta de armazenamento existente ou selecione uma gerada automaticamente.
    -   Na caixa **CONJUNTO DE DISPONIBILIDADE**, selecione **(nenhum)**.
    -   Leia e aceite as informações de preços.

6.	Na seção **PONTOS DE EXTREMIDADE**, clique na lista suspensa vazia em **NOME** e selecione **MSSQL**, em seguida, digite o número da porta da instância do mecanismo de banco de dados (**1433** para a instância padrão).

7.  A sua VM do SQL Server também pode funcionar como um IPython Notebook Server, que será configurado em uma etapa posterior. 
	Adicione um novo ponto de extremidade para especificar a porta a ser usada para o IPython Notebook Server.  Insira um nome na coluna **NAME**,	selecione um número da porta de sua preferência para a porta pública e 9999 para a porta privada.

	Clique na seta de avanço na parte inferior à direita para continuar.

	![Select MSSQL and IPython ports][3]

8.  Aceite a opção padrão **Instalar o Agente de VM** marcada e clique na marca de seleção no canto inferior direito do assistente para concluir o processo de provisionamento da VM.

	![VM Final Options][4]

9.  Aguarde enquanto o Azure prepara sua máquina virtual.  Espere que o status da máquina virtual prossiga:

    -   Iniciando (Provisionamento)
    -   Parada
    -   Iniciando (Provisionamento)
    -   Executando (Provisionamento)
    -   Executando

##<a name="RemoteDesktop"></a>Abrir a máquina virtual usando a Área de Trabalho Remota e concluir a instalação

1.  Quando o provisionamento for concluído, clique no nome da sua máquina virtual para ir para a página PAINEL.  Na parte inferior da página, clique em **Conectar**.

2.  Escolha abrir o arquivo .rpd usando o programa Área de Trabalho Remota do Windows (`%windir%\system32\mstsc.exe`).

3.  Na caixa de diálogo **Segurança do Windows**, forneça a senha da conta do administrador local que você especificou em uma etapa anterior.  (Pode ser solicitado que você verifique as credenciais da máquina virtual.)

4.  Na primeira vez que você fizer logon nessa máquina virtual, talvez seja necessário concluir vários processos, incluindo a configuração da sua área de trabalho, atualizações do Windows e a conclusão das tarefas de configuração inicial do Windows (sysprep).  Depois que o sysprep do Windows é concluído, a configuração do SQL Server conclui as tarefas de configuração.  Essas tarefas podem causar um atraso de alguns minutos enquanto são concluídas. `SELECT @@SERVERNAME` pode não retornar o nome correto até a conclusão da instalação do SQL Server, e o SQL Server Management Studio pode não estar visível na página inicial.

Quando você está conectado à máquina virtual com a Área de Trabalho Remota do Windows, a máquina virtual funciona de maneira muito semelhante à qualquer outro computador.  Conecte-se à instância padrão do SQL Server com o SQL Server Management Studio (em execução na máquina virtual) da maneira normal.

##<a name="InstallIPython"></a>Instalar o IPython Notebook e outras ferramentas de suporte

Para configurar a nova VM do SQL Server para servir como um servidor do IPython Notebook e instalar as ferramentas de suporte adicionais, como AzCopy, Azure Storage Explorer, pacotes de Python de ciência de dados úteis e outros, um script de personalização especial é fornecido para você.  Para instalar:

- Clique no ícone Iniciar do Windows e clique em **Prompt de comando (Admin)**
- Copie os seguintes comandos e cole no prompt de comando. 

    	set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
    	@powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

- Quando solicitado, digite uma senha de sua escolha para o servidor do IPython Notebook.
- O script de personalização automatiza vários procedimentos de pós-instalação, que incluem:
	+ Instalação e configuração do servidor do IPython Notebook
	+ Abertura de portas TCP no firewall do Windows para os pontos de extremidade criados anteriormente:
	+ Para conectividade remota do SQL Server
	+ Para a conectividade remota do servidor IPython Notebook
	+ Busca de scripts Python Notebooks e SQL de amostra
	+ Baixar e instalar pacotes úteis de Python para ciência dados
	+ Baixar e instalar ferramentas do Azure como o AzCopy e o Azure Storage Explorer  

- Você pode acessar e executar o IPython Notebook em qualquer navegador local ou remoto usando uma URL do formulário `https://<virtual_machine_DNS_name>:<port>`, em que a porta é a porta pública do IPython que você selecionou ao provisionar a máquina virtual.
- O servidor do IPython Notebook é executado como um serviço em segundo plano e será reiniciado automaticamente quando você reiniciar a máquina virtual.

##<a name="Optional"></a>Anexar disco de dados conforme necessário

Se a sua imagem de VM não incluir discos de dados, ou seja, discos que não sejam a unidade C (disco do sistema operacional) e a unidade D (disco temporário), você precisa adicionar um ou mais discos de dados para armazenar dados.  A imagem VM para SQL Server 2012 SP2 Enterprise Optimized for DataWarehousing Workloads vem pré-configurada com discos adicionais para arquivos de log e de dados do SQL Server.

 > [AZURE.NOTE] Não utilize a unidade D para armazenar dados.  Como seu nome quer dizer, ele oferece armazenamento apenas temporariamente.  Não oferece redundância nem backup porque eles não residem no armazenamento do Azure.

Para anexar discos de dados adicionais, siga as etapas descritas em [Como anexar um disco de dados a uma Máquina Virtual Windows](../storage-windows-attach-disk.md), que vai guiá-lo ao:

1. Anexar discos vazios à máquina virtual fornecida em etapas anteriores
2. Inicialização dos novos discos na máquina virtual


##<a name="SSMS"></a>Conecte-se ao SQL Server Management Studio e habilite a autenticação de modo misto

O Mecanismo de Banco de Dados do SQL Server não pode usar a Autenticação do Windows sem um ambiente de domínio.  Para conectar-se ao Mecanismo de Banco de Dados de outro computador, configure o SQL Server para a autenticação de modo misto.  A autenticação de modo misto permite a Autenticação do SQL Server e a Autenticação do Windows.  O modo de autenticação do SQL é necessário para receber dados diretamente de seus bancos de dados de VM do SQL Server no [Estúdio de Aprendizado de Máquina do Microsoft Azure](https://studio.azureml.net) usando o módulo de leitor.

1.  Enquanto estiver conectado à máquina virtual usando a Área de Trabalho Remota, use o painel **Pesquisar** do Windows e digite **SQL Server Management Studio** (SMSS).  Clique para iniciar o SQL Server Management Studio (SSMS).  Convém adicionar um atalho para o SSMS em sua área de trabalho para uso futuro.

    ![Start SSMS][5]

    Na primeira vez que você abrir o Management Studio ele deve criar o ambiente do Management Studio dos usuários.  Isso pode demorar alguns instantes.

2.  Ao abrir, o Management Studio apresenta a caixa de diálogo **Conectar ao Servidor**.  Na caixa **Nome do servidor**, digite o nome da máquina virtual para se conectar ao Mecanismo de Banco de Dados com o Pesquisador de Objetos.  (Em vez do nome da máquina virtual, também é possível usar **(local)** ou um único ponto como o **Nome do Servidor**.  Selecione a **Autenticação do Windows**e deixe *** *your_VM_name*\\your_local_administrator** na caixa **Nome de usuário**.  Clique em **Conectar**.

    ![Connect to Server][6]

	<br>

	 > [AZURE.TIP] Você pode alterar o modo de autenticação do SQL Server usando uma alteração de chave do Registro do Windows ou usando o SQL Server Management Studio.  Para alterar o modo de autenticação usando a alteração da chave do Registro, inicie uma **Nova consulta** e execute o seguinte script:
	
		USE master
    	go
    	
    	EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
    	go


	Para alterar o modo de autenticação usando o SQL Server Management Studio:

3.  No SQL Server Management Studio Object Explorer, clique com o botão direito do mouse no nome da instância do SQL Server (o nome da máquina virtual) e, em seguida, clique em **Propriedades**.

    ![Server Properties][7]

4.  Na página **Segurança**, em **Autenticação do Servidor**, selecione **SQL Server e Modo de Autenticação do Windows** e, em seguida, clique em **OK**.

    ![Select Authentication Mode][8]

5.  Na caixa de diálogo do SQL Server Management Studio, clique em **OK** para confirmar a necessidade de reiniciar o SQL Server.

6.  No Pesquisador de Objetos, clique com o botão direito do mouse no seu servidor e, em seguida, clique em **Reiniciar**.  (Se o SQL Server Agent estiver em execução, ele também deverá ser reinicializado.)

    ![Restart][9]

7.  Na caixa de diálogo do SQL Server Management Studio, clique em **Sim** para concordar que você deseja reiniciar o SQL Server.

##<a name="Logins"></a>Criar logons de autenticação do SQL Server

Para conectar-se ao Mecanismo de Banco de Dados de outro computador, você deve criar pelo menos um logon de autenticação do SQL Server.  

> [AZURE.TIP] Você pode criar novos logons do SQL Server programaticamente ou usando o SQL Server Management Studio.  Para criar um novo usuário sysadmin com autenticação do SQL de forma programática, inicie uma **Nova consulta** e execute o script a seguir.  Substitua <novo nome de usuário> e <nova senha> pelo nome de usuário e senha de sua escolha.  Ajuste a política de senha conforme necessário (o código de amostra desativa a expiração de senha e a verificação de política).  Para obter mais informações sobre logons do SQL Server, consulte [Criar um logon (a página pode estar em inglês)](http://msdn.microsoft.com/library/aa337562.aspx).  
	
    USE master
    go
    
    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
    	CHECK_POLICY = OFF,
    	CHECK_EXPIRATION = OFF;
    
    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';

Para criar novos logons do SQL Server usando o SQL Server Management Studio:

1.  No SQL Server Management Studio Object Explorer, expanda a pasta da instância do servidor na qual você deseja criar o novo logon.

2.  Clique com o botão direito do mouse na pasta **Segurança**, aponte para **Novo** e selecione **Logon...**.

    ![New Login][10]

3.  Na caixa de diálogo **Logon - Novo**, na página **Geral**, digite o nome do novo usuário na caixa **Nome de Logon**.

4.  Selecione **Autenticação do SQL Server**.

5.  Na caixa **Senha**, digite uma senha para o novo usuário.  Insira novamente essa senha na caixa **Confirmar Senha**.

6.  Para impor as opções da política de senha sobre complexidade e imposição, selecione **Impor Política de Senha** (recomendado).  Essa é uma opção padrão quando a autenticação do SQL Server está selecionada.

7.  Para impor as opções de expiração da política de senha, selecione **Impor Expiração de Senha** (recomendado).  A opção Impor a Política de Senha deve estar selecionada para habilitar essa caixa de seleção.  Essa é uma opção padrão quando a autenticação do SQL Server está selecionada.

8.  Para forçar o usuário a criar uma nova senha após a primeira vez que o logon é usado, selecione **O usuário deve alterar a senha no próximo logon** (recomendado se esse logon for para mais alguém usar).  Se o logon for para seu próprio uso, não selecione essa opção.)  A opção Impor a Expiração de Senha deve estar selecionada para habilitar essa caixa de seleção.  Essa é uma opção padrão quando a autenticação do SQL Server está selecionada.

9.  Na lista **Banco de dados padrão**, selecione um banco de dados padrão para o logon.  **mestre** é o padrão para essa opção.  Se você ainda não tiver criado um usuário de banco de dados, mantenha essa definição como **mestre**.

10. Na lista **Idioma padrão**, mantenha **padrão** como o valor.

    ![Login Properties][11]

11. Se esse for o primeiro logon que você está criando, talvez você queira designá-lo como um administrador do SQL Server.  Em caso afirmativo, na página **Funções de Servidor**, marque **sysadmin**.

    **Observação de segurança:** Os membros da função de servidor fixa sysadmin têm controle total sobre o Mecanismo de Banco de Dados. Você deve restringir cuidadosamente a associação nessa função.

    ![sysadmin][12]

12. Clique em OK.

##<a name="DNS"></a>Determinar o nome DNS da máquina virtual

Para conectar-se ao Mecanismo de Banco de Dados do SQL Server em outro computador, você deve saber o nome DNS (Sistema de Nome de Domínio) da máquina virtual.  (Esse é o nome que a Internet usa para identificar a máquina virtual.  Você pode usar o endereço IP, mas o endereço IP pode ser alterado quando o Azure mover os recursos para redundância ou manutenção.  O nome DNS será estável porque pode ser redirecionado para um novo endereço IP.)

1.  No Portal de Gerenciamento do Azure (ou na etapa anterior), selecione **MÁQUINAS VIRTUAIS**.

2.  Na página **INSTÂNCIAS DA MÁQUINA VIRTUAL**, na coluna **NOME DNS**, localize e copie o nome DNS para a máquina virtual que aparece precedido por **http://**.  (A interface do usuário pode não exibir o nome completo, mas você pode clicar com o botão direito do mouse nele e selecionar Copiar.)

##<a name="cde"></a>Conectar-se ao Mecanismo de Banco de Dados de outro computador

1.  Em um computador conectado à Internet, abra o SQL Server Management Studio.

2.  Na caixa de diálogo **Conectar-se ao Servidor** ou **Conectar-se ao Mecanismo de Banco de Dados**, na caixa **Nome do servidor**, insira o nome DNS da máquina virtual (determinado na tarefa anterior) e um número da porta pública do ponto de extremidade no formato *DNSName,portnumber*, como **tutorialtestVM.cloudapp.net,57500**.

3.  Na caixa **Autenticação**, selecione **Autenticação do SQL Server**.

4.  Na caixa **Logon**, digite o nome de um logon que você criou em uma tarefa anterior.

5.  Na caixa **Senha**, digite a senha do logon que você criou em uma tarefa anterior.

6.  Clique em **Conectar**.

##<a name="amlconnect"></a>Conectar-se ao mecanismo de banco de dados de aprendizado de máquina do Azure

Nos estágios posteriores do Processo de ciência de dados de nuvem, você usará o [Estúdio de Aprendizado de Máquina do Azure](https://studio.azureml.net) para compilar e implantar modelos de aprendizado de máquina.  Para incluir dados de seus bancos de dados do SQL Server VM diretamente no Aprendizado de máquina do Azure para treinamento ou pontuação, use o módulo de Leitor em um novo experimento do [Estúdio de Aprendizado de Máquina do Azure](https://studio.azureml.net).  Este tópico é abordado em mais detalhes usando os links de mapa do Processo de ciência de dados de nuvem.  Para obter uma introdução, consulte [O que é o Estúdio de Aprendizado de Máquina do Azure?](machine-learning-what-is-ml-studio.md).

2.	No painel **Propriedades** do [módulo Leitor](https://msdn.microsoft.com/library/azure/dn905997.aspx), selecione **Banco de dados SQL do Azure** na lista suspensa **Fonte de dados**.

3.	Na caixa de texto **Nome do servidor de banco de dados**, digite `tcp:<nome DNS da sua máquina virtual>,1433`

4.	Insira o nome de usuário do SQL na caixa de texto **Nome de conta de usuário do servidor**.

5.	Digite a senha do usuário do SQL na caixa de texto **Senha de conta de usuário do servidor**.

	![Azure ML Reader][13]

##<a name="shutdown"></a>Desligar e desalocar a máquina virtual quando ela não estiver em uso

As máquinas virtuais do Azure são cobradas como **pague somente pelo que usa**.  Para garantir que você não esteja sendo cobrado quando não estiver usando sua máquina virtual, ela deverá estar no estado **Parado (desalocado)**.

> [AZURE.NOTE] Ao desligar a máquina virtual de dentro (usando as opções de energia do Windows), a VM é interrompida, mas permanece alocada.  Para garantir que você não esteja sendo cobrado, sempre pare as máquinas virtuais no [Portal de Gerenciamento do Azure](http://manage.windowsazure.com/).  Você também pode interromper a VM por meio do Powershell chamando ShutdownRoleOperation com "PostShutdownAction" igual a "StoppedDeallocated".

Desligar e desalocar a máquina virtual:

1. Faça logon no [Portal de Gerenciamento do Azure](http://manage.windowsazure.com/) (a página pode estar em inglês) usando sua conta.  

2. Selecione **MÁQUINAS VIRTUAIS** na barra de navegação à esquerda.

3. Na lista de máquinas virtuais, clique no nome da máquina virtual e acesse a página **PAINEL**.

4. Na parte inferior da página, clique em **DESLIGAR**. 

![VM Shutdown][15]

A máquina virtual será desalocada, mas não excluída.  Você pode reiniciar a máquina virtual a qualquer momento no Portal de Gerenciamento do Azure.

## A sua VM SQL Server do Azure está pronta para uso: o que vem a seguir?

Agora, a sua máquina virtual está pronta para ser usada em seus exercícios de ciência de dados.  A máquina virtual também está pronta para uso como um servidor do IPython Notebook para a exploração e o processamento de dados e outras tarefas em conjunto com o Aprendizado de Máquina do Azure e o Processo de Ciência de Dados de Nuvem. 

As próximas etapas do processo de ciência de dados são mapeadas no [Guia de aprendizagem:  O processamento avançado de dados no Azure](machine-learning-data-science-advanced-data-processing.md) e pode incluir etapas que movem dados para o HDInsight, o processe-as e demonstre-as na preparação de aprendizado com os dados no Aprendizado de Máquina do Azure.

* Para obter informações sobre como mover dados para HDInsight por meio do armazenamento de blob do Azure, consulte [Criar e carregar dados em tabelas de Hive do armazenamento de blob do Azure](machine-learning-data-science-hive-tables.md).
* Para obter informações sobre o processamento de dados no HDInsight com consultas de Hive, consulte [Enviar consultas Hive para os clusters do HDInsight Hadoop no processo de ciência de dados de nuvem](machine-learning-data-science-hive-queries.md).
* Para obter informações sobre os dados de amostragem no HDInsight, consulte [Dados de amostra nas tabelas do Azure HDInsight Hive](machine-learning-data-science-sample-data-hive.md).



[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png


<!--HONumber=49--> 