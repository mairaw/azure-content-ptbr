<properties 
    pageTitle="Integração de controle de origem na Automação do Azure | Microsoft Azure"
    description="Este artigo descreve a integração de controle de origem com o GitHub na Automação do Azure."
    services="automation"
    documentationCenter=""
    authors="SnehaGunda"
    manager="stevenka"
    editor="tysonn" />    
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="11/10/2015"
    ms.author="sngun" />

# Integração de controle de origem na Automação do Azure

A integração de controle de origem permite que você associe runbooks em sua conta de Automação a um repositório de controle de origem do GitHub. O controle de origem permite que você colabore com facilidade com sua equipe, controle alterações e reverta para versões anteriores de seus runbooks. Por exemplo, o controle de origem permite a sincronização de ramificações diferentes no controle de origem para suas contas de desenvolvimento, teste ou produção da Automação, facilitando a promoção do código testado em seu ambiente de desenvolvimento para o ambiente de produção da Automação.

O controle do código-fonte permite que você envie código por push da Automação do Azure para o controle do código-fonte ou efetue o pull de seus runbooks do controle do código-fonte para a Automação do Azure. Este artigo descreve como configurar o controle de origem no seu ambiente da Automação do Azure. Vamos começar configurando a Automação do Azure para acessar seu repositório do GitHub e analisar operações diferentes que podem ser feitas usando a integração de controle de origem.


>[AZURE.NOTE]O controle de origem dá suporte a efetuar pull e a enviar por push de [Runbooks de fluxo de trabalho do PowerShell](automation-runbook-types.md#powershell-workflow-runbooks), bem como de [Runbooks do PowerShell](automation-runbook-types.md#powershell-runbooks), mas os [runbooks do Gráfico](automation-runbook-types.md#graphical-runbooks) ainda não têm suporte.<br><br>


Há duas etapas simples necessárias para configurar o controle de origem para sua Conta de Automação, e somente uma se você já tiver uma conta do GitHub. Eles são:
## Etapa 1: Criar um repositório do GitHub

Se você já tiver uma conta do GitHub e um repositório que deseja vincular à Automação do Azure, faça logon em sua conta existente e comece da etapa 2 abaixo. Caso contrário, navegue até o [GitHub](https://github.com/), inscreva-se em uma nova conta e [crie um novo repositório](https://help.github.com/articles/create-a-repo/).


## Etapa 2: Configurar o controle de origem na Automação do Azure

1. Na folha Conta de Automação do portal de visualização do Azure, clique em **Configurar Controle de Origem**. 
 
    ![Configurar Controle de Origem](media/automation-source-control-integration/automation_01_SetUpSourceControl.png)

2. A folha **Controle de Origem** será aberta e você poderá configurar os detalhes da conta GitHub. A seguir, a lista de parâmetros a serem configurados:

    |**Parâmetro** |**Descrição** |
    |:---|:---| 
    |Escolher Origem | Selecione a origem. No momento, apenas **GitHub** tem suporte. |
    |Autorização | Clique no botão **Autorizar** para conceder acesso da Automação do Azure a seu repositório do GitHub. Se você já estiver conectado à sua conta do GitHub em uma janela diferente, as credenciais da conta serão usadas. Quando a autorização for bem-sucedida, a folha mostrará o seu nome de usuário do GitHub em **Propriedade de Autorização**. |
    |Escolher o repositório | Selecione um repositório do GitHub na lista de repositórios disponíveis. |
    |Escolher a ramificação | Selecione uma ramificação na lista de ramificações disponíveis. Somente a ramificação **mestre** será mostrada se você ainda não tiver criado quaisquer ramificações. |
    |Caminho da pasta do runbook | O caminho da pasta do runbook especifica o caminho no repositório do GitHub do qual você deseja enviar seu código por push ou por pull. Ele deve ser inserido no formato **/nomedapasta/nomedasubpasta**. Somente os runbooks no caminho da pasta do runbook serão sincronizados com sua conta da Automação. Os runbooks nas subpastas do caminho da pasta do runbook serão **NÃO** serão sincronizados. Use **/** para sincronizar todos os runbooks do repositório. |


3. Por exemplo, se você tiver um repositório chamado **PowerShellScripts** com uma pasta chamada **RootFolder** que contenha uma pasta chamada **SubFolder**. Você pode usar as seguintes cadeias de caracteres para sincronizar cada nível de pasta:

    1. Para sincronizar os runbooks do **repositório**, o caminho da pasta do runbook será */*
    2. Para sincronizar os runbooks de **RootFolder**, o caminho da pasta do runbook será */RootFolder*
    3. Para sincronizar os runbooks de **SubFolder**, o caminho da pasta do runbook será */RootFolder/SubFolder*.
  

4. Depois de configurar os parâmetros, eles serão exibidos na folha **Configurar Controle de Origem**.
 
    ![Configurar Folha](media/automation-source-control-integration/automation_02_SourceControlConfigure.png)


5. Depois de clicar em OK, a integração do controle do código-fonte estará configurada para a sua conta de automação e deverá ser atualizada com as informações do GitHub. Agora você pode clicar nessa parte para exibir todo o seu histórico de trabalhos de sincronização de controle do código-fonte.

    ![Valores de Repositório](media/automation-source-control-integration/automation_03_RepoValues.png)

6. Depois de configurar o controle de origem, os seguintes recursos da Automação serão criados na sua conta da Automação: dois [ativos de variáveis](automation-variables.md) são criados.
      
    * A variável **Microsoft.Azure.Automation.SourceControl.Connection** contém os valores da cadeia de conexão, como mostrado abaixo.  

    |**Parâmetro** |**Valor** |
    |:---|:---|
    | Nome | Microsoft.Azure.Automation.SourceControl.Connection |
    | Tipo | Cadeia de caracteres |
    | Valor | {"Branch":<*Nome da sua ramificação*>,"RunbookFolderPath":<*Caminho da pasta do runbook*>,"ProviderType":<*tem um valor 1 para o GitHub*>,"Repository":<*Nome do seu repositório*>,"Username":<*O nome do seu usuário no GitHub*>} | <br>


    * A variável **Microsoft.Azure.Automation.SourceControl.OauthToken** contém o valor criptografado seguro do seu OAuthToken.  

    |**Parâmetro** |**Valor** |
    |:---|:---|
    | Nome | Microsoft.Azure.Automation.SourceControl.OauthToken |
    | Tipo | Unknown(Encrypted) |
    | Valor | <*OAuthToken Criptografado*> |  

    ![Variáveis](media/automation-source-control-integration/automation_04_Variables.png)

    * O **Controle de Origem de Automação** é adicionado como um aplicativo autorizado à sua conta do GitHub. Para exibir o aplicativo: na home page do GitHub, navegue até o **perfil** > **Configurações** > **Aplicativos**. Esse aplicativo permite que a Automação do Azure sincronize seu repositório do GitHub para uma conta da Automação.  

    ![Aplicativo do Git](media/automation-source-control-integration/automation_05_GitApplication.png)


## Usando o controle de origem na Automação


### Fazer check-in de um runbook da Automação do Azure no controle de origem

O check-in de runbook permite que você envie por push as alterações feitas em um runbook na Automação do Azure para o repositório do controle de origem. A seguir, as etapas para fazer check-in de um runbook:

1. De sua Conta de Automação, [crie um novo runbook textual](automation-first-runbook-textual.md) ou [edite um runbook textual existente](automation-edit-textual-runbook.md). Esse runbook pode ser um Fluxo de Trabalho do PowerShell ou um runbook de script do PowerShell.  

2. Depois de editar o runbook, salve-o e clique em **check-in** na folha **Editar**.

    ![Botão Check-in](media/automation-source-control-integration/automation_06_CheckinButton.png)


     >[AZURE.NOTE]O check-in da Automação do Azure substituirá o código que existe atualmente no controle de origem. A instrução de linha de comando equivalente do Git para fazer check-in é **git add + git commit + git push**

3. Quando você clicar em **check-in**, uma mensagem de confirmação será exibida, clique em sim para continuar.

    ![Mensagem de Check-in](media/automation-source-control-integration/automation_07_CheckinMessage.png)

4. O check-in inicia o runbook do controle de origem: **Sync-MicrosoftAzureAutomationAccountToGitHubV1**. Esse runbook se conecta ao GitHub e envia por push alterações da Automação do Azure para seu repositório. Para exibir o histórico de trabalho de check-in, volte para a guia **Integração de Controle de Origem** e clique para abrir a folha Sincronização de Repositório. Essa folha mostra todos os seus trabalhos de controle de origem. Selecione o trabalho que você deseja exibir e clique para exibir os detalhes.

    ![Fazer Check-in de Runbook](media/automation-source-control-integration/automation_08_CheckinRunbook.png)

    >[AZURE.NOTE]Runbooks de controle de origem são runbooks especiais de Automação que você não pode exibir ou editar. Embora eles não apareçam em sua lista de runbooks, você verá os trabalhos de sincronização em sua lista de trabalhos.
 
5. O nome do runbook modificado é enviado como um parâmetro de entrada para o runbook de check-in. Você pode [exibir os detalhes do trabalho](automation-runbook-execution.md#viewing-job-status-using-the-azure-management-portal) expandindo o runbook na folha **Sincronização de Repositórios**.

    ![Entrada de Check-in](media/automation-source-control-integration/automation_09_CheckinInput.png)

6. Atualize seu repositório do GitHub depois que o trabalho for concluído para exibir as alterações. Deve haver uma confirmação em seu repositório com uma mensagem de confirmação: ***Nome do Runbook* Atualizado na Automação do Azure.**



### Sincronizar runbooks do controle de origem para a Automação do Azure 

O botão de sincronização na folha Sincronização do repositório permite efetuar pull em todos os runbooks no caminho da pasta do runbook do seu repositório para sua Conta de Automação. O mesmo repositório pode ser sincronizado em mais de uma Conta de Automação. A seguir, as etapas para sincronizar um runbook:

1. Na Conta de Automação onde você configura o controle de origem, abra a **folha Integração de Controle de Origem/Sincronização de Repositórios** e clique em **Sincronizar** e será exibida uma mensagem de confirmação, clique em **Sim** para continuar.  

    ![Botão Sincronizar](media/automation-source-control-integration/automation_10_SyncButtonwithMessage.png)

2. A sincronização inicia o runbook: **Sync-MicrosoftAzureAutomationAccountFromGitHubV1**. Esse runbook se conecta ao GitHub e efetua pull das alterações do seu repositório para a Automação do Azure. Você verá um novo trabalho na folha **Sincronização de Repositório** para esta ação. Para exibir detalhes sobre o trabalho de sincronização, clique para abrir a folha de detalhes do trabalho.
 
    ![Sincronizar Runbook](media/automation-source-control-integration/automation_11_SyncRunbook.png)

 
    >[AZURE.NOTE]Uma sincronização do controle de origem substitui a versão de rascunho dos runbooks que existem atualmente em sua conta da Automação para **TODOS** os runbooks atualmente no controle de origem. A instrução de linha de comando equivalente do Git para sincronizar é **git pull**


## Solucionando problemas do controle de origem

Se houver erros com um trabalho de check-in ou de sincronização, o status do trabalho deverá ser suspenso e você poderá exibir mais detalhes sobre o erro na folha do trabalho. A parte **Todos os Logs** mostrará todos os fluxos do PowerShell associados ao trabalho. Isso fornecerá os detalhes necessários para ajudar com a correção de quaisquer problemas com seu check-in ou sincronização. Também mostrará a sequência de ações que ocorreram durante a sincronização ou verificação em um runbook.

![Imagem AllLogs](media/automation-source-control-integration/automation_13_AllLogs.png)

## Desconectando o controle de origem

Para se desconectar de sua conta do GitHub, abra a folha Sincronização de Repositórios e clique em **Desconectar**. Depois de se desconectar do controle de origem, os runbooks sincronizados anteriormente ainda permanecerão em sua conta da Automação, mas a folha Sincronização de Repositórios não será habilitada.

  ![Botão Desconectar](media/automation-source-control-integration/automation_12_Disconnect.png)



## Próximas etapas

Para saber mais sobre a integração de controle de origem, confira os seguintes recursos:- [Automação do Azure: integração de controle de origem na Automação do Azure](https://azure.microsoft.com/blog/azure-automation-source-control-13/) - [Vote em seu sistema de controle de origem favorito](https://www.surveymonkey.com/r/?sm=2dVjdcrCPFdT0dFFI8nUdQ%3d%3d) - [Automação do Azure: integrando o controle do código-fonte de runbook usando o Visual Studio Team Services](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)

<!---HONumber=AcomDC_1217_2015-->