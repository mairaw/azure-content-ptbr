<properties pageTitle="Tutorial: Integração do Active Directory do Azure com o xMatters OnDemand | Microsoft Azure" description="Saiba como usar o xMatters OnDemand com o Active Directory do Azure para habilitar logon único, provisionamento automatizado e muito mais." services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#Tutorial: Integração do Active Directory do Azure com o xMatters OnDemand
>[AZURE.TIP]Para comentários, clique [aqui](http://go.microsoft.com/fwlink/?LinkId=524330).
  
O objetivo deste tutorial é mostrar a integração do Azure com o xMatters OnDemand. O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

-   Uma assinatura válida do Azure
-   Um locatário do xMatters OnDemand
  
Depois de concluir este tutorial, os usuários do AD do Azure atribuídos ao xMatters OnDemand poderão fazer logon único no aplicativo, em seu site de empresa xMatters OnDemand (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](https://msdn.microsoft.com/library/dn308586)
  
O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1.  Habilitando a integração de aplicativos com o xMatters OnDemand
2.  Configurando o logon único
3.  Configurando o provisionamento de usuários
4.  Atribuindo usuários

![Cenário](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Cenário")

##Habilitando a integração de aplicativos com o xMatters OnDemand
  
O objetivo desta seção é descrever como habilitar a integração de aplicativos com o xMatters OnDemand.

###Para habilitar a integração de aplicativos com o xMatters OnDemand, execute as seguintes etapas:

1.  No Portal de Gerenciamento do Azure, no painel navegação à esquerda, clique em **Active Directory**.

    ![Active Directory](./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Active Directory")

2.  Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3.  Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

    ![Aplicativos](./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Aplicativos")

4.  Clique em **Adicionar** na parte inferior da página.

    ![Adicionar aplicativo](./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Adicionar aplicativo")

5.  Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

    ![Adicionar um aplicativo da galeria](./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Adicionar um aplicativo da galeria")

6.  Na **caixa de pesquisa**, digite **xMatters OnDemand**.

    ![Galeria de aplicativos](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Galeria de aplicativos")

7.  No painel de resultados, selecione **xMatters OnDemand** e clique em **Concluir** para adicionar o aplicativo.

    ![xMatters OnDemand](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters OnDemand")

##Configurando o logon único
  
O objetivo desta seção é descrever como permitir que os usuários se autentiquem no xMatters OnDemand com sua conta do AD do Azure usando federação baseada em protocolo SAML.

###Para configurar o logon único, execute as seguintes etapas:

1.  No portal do AD do Azure, na página de integração do aplicativo **xMatters OnDemand**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar logon único **.

    ![Configurar logon único](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Configurar logon único")

2.  Na página **Como você deseja que os usuários façam logon no XMatters OnDemand**, selecione **Logon Único do AD do Microsoft Azure** e clique em **Avançar**.

    ![Configurar logon único](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Configurar logon único")

3.  Na página **Configurar URL do aplicativo**, na caixa de texto **URL de entrada do XMatters OnDemand**, digite sua URL usando o seguinte padrão "*https://\<nome do locatário>.XMattersOnDemand.com*" e, em seguida, clique em **Próximo**.

    ![Configurar a URL do aplicativo](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Configurar a URL do aplicativo")

4.  Na página **Configurar logon único no XMatters OnDemand**, clique em **Baixar certificado** e, em seguida, salve o arquivo do certificado em seu computador como **c:\\XMatters OnDemand.cer**.

    >[AZURE.IMPORTANT]Você precisa encaminhar o certificado para a equipe de suporte do xMatters. O certificado precisa ser carregado pela equipe de suporte do xMatters antes de você finalizar a configuração de logon único.

    ![Configurar logon único](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Configurar logon único")

5.  Em uma janela diferente do navegador da Web, faça logon em seu site de empresa XMatters OnDemand como administrador.

6.  Na barra de ferramentas na parte superior, clique em **Admin** e, em seguida, clique em **Detalhes da Empresa** na barra de navegação à esquerda.

    ![Administrador](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Administrador")

7.  Na página **Configuração do SAML**, realize as seguintes etapas:

    ![Configuração de SAML](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "Configuração de SAML")

    1.  Selecione **Habilitar SAML**.
    2.  No portal do Azure, na página de diálogo **Configurar logon único no XMatters OnDemand**, copie o valor da **ID do Provedor de Identidade** e, em seguida, cole-o na caixa de texto **ID do Provedor de Identidade**.
    3.  No portal do Azure, na página de diálogo **Configurar logon único no XMatters OnDemand**, copie o valor da **URL de Serviço de Logon Único** e, em seguida, cole-o na caixa de texto **URL de Logon Único**.
    4.  No portal do Azure, na página de diálogo **Configurar logon único no XMatters OnDemand**, copie o valor da **URL de Serviço de Logout Único** e, em seguida, cole-o na caixa de texto **URL de Logout Único**.
    5.  Na página Detalhes da Empresa, na parte superior, clique em **Salvar alterações**. ![Detalhes da empresa](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Detalhes da empresa")

8.  No portal do AD do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar Logon Único**.

    ![Configurar logon único](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Configurar logon único")

##Configurando o provisionamento de usuários
  
Para permitir que os usuários do AD do Azure façam logon no XMatters OnDemand, eles devem ser provisionados no XMatters OnDemand. No caso do XMatters OnDemand, o provisionamento é uma tarefa manual.

###Para provisionar contas de usuário, execute as seguintes etapas:

1.  Faça logon no seu locatário do **XMatters OnDemand**.

2.  Clique na guia **Usuários**.

3.  Clique em **Adicionar Usuário**.

    ![Usuários](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Usuários")

4.  Selecione **Ativo**.

5.  Na seção **Adicionar um Usuário**, execute as seguintes etapas:

    ![Adicionar um Usuário](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Adicionar um Usuário")

    1.  Insira **UserID**, **Nome**, **Sobrenome**, **Site** de uma conta válida do AAD que você deseja provisionar.
    2.  Clique em **Salvar**.

>[AZURE.NOTE]É possível usar qualquer outra ferramenta de criação da conta de usuário do XMatters OnDemand ou APIs fornecidas pelo XMatters OnDemand para provisionar as contas de usuário do AAD.

##Atribuindo usuários
  
Para testar sua configuração, é necessário atribuir acesso ao aplicativo aos usuários do AD do Azure que deseja que usem seu aplicativo.

###Para atribuir usuários ao XMatters OnDemand, execute as seguintes etapas:

1.  No portal do AD do Azure, crie uma conta de teste.

2.  Na página de integração de aplicativos do **XMatters OnDemand **, clique em **Atribuir usuários**.

    ![Atribuir usuários](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Atribuir usuários")

3.  Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.

    ![Sim](./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Sim")
  
Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, consulte [Introdução ao Painel de Acesso](https://msdn.microsoft.com/library/dn308586).

<!---HONumber=August15_HO7-->