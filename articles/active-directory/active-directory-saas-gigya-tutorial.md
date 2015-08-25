<properties pageTitle="Tutorial: integração do Active Directory do Azure ao Gigya | Microsoft Azure" description="Saiba como usar o Gigya com o Active Directory do Azure para habilitar o logon único, o provisionamento automatizado e muito mais!" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#Tutorial: integração do Active Directory do Azure ao Gigya
>[AZURE.TIP]Para ver comentários, clique [aqui](http://go.microsoft.com/fwlink/?LinkId=528575).
  
O objetivo deste tutorial é mostrar a integração do Azure ao Gigya. O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

-   Uma assinatura válida do Azure
-   Uma assinatura habilitada para logon único do Gigya
  
Depois de concluir este tutorial, os usuários do Azure AD que você atribuiu ao Gigya poderão fazer logon único no aplicativo em seu site de empresa do Gigya (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](https://msdn.microsoft.com/library/dn308586)
  
O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1.  Habilitando a integração de aplicativos para Gigya
2.  Configurando o logon único
3.  Configurando o provisionamento de usuários
4.  Atribuindo usuários

![Configurar o logon único](./media/active-directory-saas-gigya-tutorial/IC789512.png "Configurar o logon único")
##Habilitando a integração de aplicativos para Gigya
  
O objetivo desta seção é descrever como habilitar a integração de aplicativos para o Gigya.

###Para habilitar a integração de aplicativos para o Gigya, execute as seguintes etapas:

1.  No Portal de Gerenciamento do Azure, no painel navegação à esquerda, clique em **Active Directory**.

    ![Active Directory](./media/active-directory-saas-gigya-tutorial/IC700993.png "Active Directory")

2.  Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3.  Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

    ![Aplicativos](./media/active-directory-saas-gigya-tutorial/IC700994.png "Aplicativos")

4.  Clique em **Adicionar** na parte inferior da página.

    ![Adicionar aplicativo](./media/active-directory-saas-gigya-tutorial/IC749321.png "Adicionar aplicativo")

5.  Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

    ![Adicionar um aplicativo da galeria](./media/active-directory-saas-gigya-tutorial/IC749322.png "Adicionar um aplicativo da galeria")

6.  Na **caixa de pesquisa**, digite **Gigya**.

    ![Galeria de Aplicativos](./media/active-directory-saas-gigya-tutorial/IC789513.png "Galeria de Aplicativos")

7.  No painel de resultados, selecione **Gigya** e clique em **Concluir** para adicionar o aplicativo.

    ![Gigya](./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##Configurando o logon único
  
O objetivo desta seção é descrever como permitir que os usuários se autentiquem no Gigya com a respectiva conta do Azure AD usando a federação baseada no protocolo SAML. Como parte deste procedimento, é necessário criar um arquivo de certificado codificado em base-64. Se você não estiver familiarizado com este procedimento, consulte [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o).

###Para configurar o logon único, execute as seguintes etapas:

1.  No portal do Azure AD, na página de integração de aplicativos do **Gigya**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único**.

    ![Configurar o logon único](./media/active-directory-saas-gigya-tutorial/IC789528.png "Configurar o logon único")

2.  Na página **Como você deseja que os usuários façam logon no Gigya**, selecione **Logon Único do AD do Microsoft Azure** e clique em **Avançar**.

    ![Configurar o logon único](./media/active-directory-saas-gigya-tutorial/IC789529.png "Configurar o logon único")

3.  Na página **Configurar URL do aplicativo**, na caixa de texto **URL de logon no Gigya**, digite sua URL usando o seguinte padrão "**http://company.gigya.com*" e clique em **Avançar**.

    ![Configurar a URL do Aplicativo](./media/active-directory-saas-gigya-tutorial/IC789530.png "Configurar a URL do Aplicativo")

4.  Na página **Configurar logon único no Gigya**, clique em **Baixar certificado** e salve o arquivo de certificado em seu computador.

    ![Configurar o logon único](./media/active-directory-saas-gigya-tutorial/IC789531.png "Configurar o logon único")

5.  Em outra janela do navegador da Web, faça logon em seu site de empresa do Gigya como administrador.

6.  Acesse **Configurações > Logon SAML** e clique no botão **Adicionar**.

    ![Logon no SAML](./media/active-directory-saas-gigya-tutorial/IC789532.png "Logon no SAML")

7.  Na seção **Logon SAML**, execute as seguintes etapas:

    ![Configuração de SAML](./media/active-directory-saas-gigya-tutorial/IC789533.png "Configuração de SAML")

    1.  Na caixa de texto **Nome**, digite um nome para sua configuração.
    2.  No portal do Azure, na página da caixa de diálogo **Configurar logon único no Gigya**, copie o valor da **URL do Emissor** e cole-o na caixa de texto **Emissor**.
    3.  No portal do Azure, na página da caixa de diálogo **Configurar logon único no Gigya**, copie o valor da **URL de Serviço de Logon Único** e cole-o na caixa de texto **URL de Serviço de Logon Único**.
    4.  No portal do Azure, na página da caixa de diálogo **Configurar logon único no Gigya**, copie o valor da **Formato de Identificador de Nome** e cole-o na caixa de texto **Formato de Identificação de Nome**.
    5.  Crie um arquivo **codificado em base-64** usando o certificado baixado.
        
		>[AZURE.TIP]Para obter mais detalhes, consulte [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o)

    6.  Abra seu certificado codificado em base-64 no bloco de notas, copie o conteúdo dele na área de transferência e cole-o na caixa de texto **Certificado X.509**.
    7.  Clique em **Salvar Configurações**.

8.  No portal do Azure AD, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar Logon Único**.

    ![Configurar o logon único](./media/active-directory-saas-gigya-tutorial/IC789534.png "Configurar o logon único")
##Configurando o provisionamento de usuários
  
Para permitir que os usuários do Azure AD façam logon no Gigya, eles devem ser provisionados no Gigya. No caso do Gigya, o provisionamento é uma tarefa manual.

###Para provisionar contas de usuário, execute as seguintes etapas:

1.  Faça logon em seu site de empresa do **Gigya** como administrador.

2.  Acesse **Administração > Gerenciar Usuários** e clique em **Convidar Usuários**.

    ![Gerenciar Usuários](./media/active-directory-saas-gigya-tutorial/IC789535.png "Gerenciar Usuários")

3.  Na caixa de diálogo Convidar Usuários, execute as seguintes etapas:

    ![Convidar Usuários](./media/active-directory-saas-gigya-tutorial/IC789536.png "Convidar Usuários")

    1.  Na caixa de texto **Email**, digite o alias de email de uma conta válida do Active Directory do Azure que você deseja provisionar.
    2.  Clique em **Convidar Usuário**.
    
        >[AZURE.NOTE]O titular da conta do Active Directory do Azure receberá um email com um link para confirmar a conta antes que ela se torne ativa.

>[AZURE.NOTE]É possível usar qualquer outra ferramenta de criação da conta de usuário do Gigya ou as APIs fornecidas pelo Gigya para provisionar as contas de usuário do AAD.

##Atribuindo usuários
  
Para testar sua configuração, será necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que o utilizem.

###Para atribuir usuários ao Gigya, execute as seguintes etapas:

1.  No portal do Azure AD, crie uma conta de teste.

2.  Na página de integração de aplicativos do **Gigya**, clique em **Atribuir usuários**.

    ![Atribuir Usuários](./media/active-directory-saas-gigya-tutorial/IC789537.png "Atribuir Usuários")

3.  Selecione seu usuário de teste, clique em **Atribuir** e em **Sim** para confirmar sua atribuição.

    ![Sim](./media/active-directory-saas-gigya-tutorial/IC767830.png "Sim")
  
Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, consulte [Introdução ao Painel de Acesso](https://msdn.microsoft.com/library/dn308586).

<!---HONumber=August15_HO7-->