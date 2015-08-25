<properties pageTitle="Tutorial: Integração do Active Directory do Azure com o Jitbit Helpdesk | Microsoft Azure" description="Saiba como usar o Jitbit Helpdesk com o Active Directory do Azure para habilitar logon único, provisionamento automatizado e muito mais!" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#Tutorial: Integração do Active Directory do Azure com o Jitbit Helpdesk
>[AZURE.TIP]Para ver comentários, clique [aqui](http://go.microsoft.com/fwlink/?LinkId=524767).
  
O objetivo deste tutorial é mostrar a integração do Azure com o Jitbit Helpdesk. O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

-   Uma assinatura válida do Azure
-   Um locatário do Jitbit Helpdesk
  
Depois de concluir este tutorial, os usuários do AD do Azure que você atribuiu ao Jitbit Helpdesk poderão fazer um logon único no aplicativo do site da sua empresa Jitbit Helpdesk (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](https://msdn.microsoft.com/library/dn308586)
  
O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1.  Habilitando a integração de aplicativos com o Jitbit Helpdesk
2.  Configurando o logon único
3.  Configurando o provisionamento de usuários
4.  Atribuindo usuários

![Cenário](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Cenário")
##Habilitando a integração de aplicativos com o Jitbit Helpdesk
  
O objetivo desta seção é descrever como habilitar a integração de aplicativos com o Jitbit Helpdesk.

###Para habilitar a integração de aplicativos com o Jitbit Helpdesk, execute as seguintes etapas:

1.  No Portal de Gerenciamento do Azure, no painel navegação à esquerda, clique em **Active Directory**.

    ![Active Directory](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Active Directory")

2.  Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3.  Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

    ![Aplicativos](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Aplicativos")

4.  Clique em **Adicionar** na parte inferior da página.

    ![Adicionar aplicativo](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Adicionar aplicativo")

5.  Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

    ![Adicionar um aplicativo da galeria](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Adicionar um aplicativo da galeria")

6.  Na **caixa de pesquisa**, digite **Jitbit Helpdesk**.

    ![Galeria de aplicativos](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Galeria de aplicativos")

7.  No painel de resultados, selecione **Jitbit Helpdesk** e clique em **Concluir** para adicionar o aplicativo.

    ![JitBit](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
##Configurando o logon único
  
O objetivo desta seção é descrever como permitir que os usuários se autentiquem no Jitbit Helpdesk com sua conta do AD do Azure usando federação baseada em protocolo SAML. Como parte desse procedimento, é necessário criar um arquivo de certificado codificado em base 64. Se você não estiver familiarizado com esse procedimento, consulte [Como converter certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o).

>[AZURE.IMPORTANT]Para poder configurar o logon único em seu locatário Jitbit Helpdesk, você precisa primeiro contatar o suporte técnico Jitbit Helpdesk para que esse recurso seja habilitado.

###Para configurar o logon único, execute as seguintes etapas:

1.  No portal do AD do Azure, na página de integração do aplicativo **Jitbit Helpdesk**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar logon único**.

    ![Configurar o logon único](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Configurar o logon único")

2.  Na página **Como você deseja que os usuários façam logon no Jitbit Helpdesk**, selecione **Logon único do AD do Microsoft Azure** e clique em **Avançar**.

    ![Configurar o logon único](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Configurar o logon único")

3.  Na página **Configurar URL do aplicativo**, na caixa de texto **URL de entrada do Jitbit Helpdesk**, digite sua URL usando o seguinte padrão "*https://\<nome do locatário>.Jitbit.com*" e, em seguida, clique em **Próximo**.

    ![Configurar a URL do aplicativo](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "Configurar a URL do aplicativo")

4.  Na página **Configurar logon único no Jitbit Helpdesk**, clique em **Baixar certificado** e, em seguida, salve o arquivo do certificado em seu computador como **c:\\Jitbit Helpdesk.cer**.

    ![Configurar o logon único](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Configurar o logon único")

5.  Em uma janela diferente do navegador da Web, faça logon no site da sua empresa do Jitbit Helpdesk como administrador.

6.  Na barra de ferramentas na parte superior, clique em **Administração**.

    ![Administração](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Administração")

7.  Clique em **Configurações gerais**.

    ![Usuários, empresas e permissões](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Usuários, empresas e permissões")

8.  Na seção de configuração **Configurações de autenticação**, execute as seguintes etapas:

    ![Configurações da autenticação](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Configurações da autenticação")

    1.  Selecione **Habilitar logon único SAML 2.0** usando SSO (logon único) com **OneLogin**.
    2.  No portal do Azure, na página de diálogo **Configurar logon único no Jitbit Helpdesk**, copie o valor **Ponto de extremidade iniciado pelo SP (provedor de serviço)** e, em seguida, cole-o na caixa de texto **URL de ponto de extremidade**.
    3.  Crie um arquivo **codificado em base 64** usando o certificado baixado.
        
		>[AZURE.TIP]Para obter mais detalhes, consulte [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o)

    4.  Abra seu certificado codificado base 64, copie o conteúdo dele em sua área de transferência e, então, cole-o na caixa de texto **Certificate X.509**
    5.  Clique em **Salvar alterações**.

9.  No portal do AD do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar logon único**.

    ![Configurar o logon único](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Configurar o logon único")
##Configurando o provisionamento de usuários
  
Para permitir que os usuários do AD do Azure façam logon no Jitbit Helpdesk, eles devem ser provisionados no Helpdesk. No caso do Jitbit Helpdesk, o provisionamento é uma tarefa manual.

###Para provisionar contas de usuário, execute as seguintes etapas:

1.  Faça logon no seu locatário do **Jitbit Helpdesk**.

2.  No menu na parte superior, clique em **Administração**.

    ![Administração](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Administração")

3.  Clique em **Usuários, empresas e permissões**.

    ![Usuários, empresas e permissões](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Usuários, empresas e permissões")

4.  Clique em **Adicionar usuário**.

    ![Adicionar usuário](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Adicionar usuário")

5.  Na seção Criar, digite os dados da conta do AD do Azure que deseja provisionar nas seguintes caixas de texto: **Nome de usuário**, **Email**, **Nome**, **Sobrenome**

    ![Criar](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Criar")

6.  Clique em **Criar**.

>[AZURE.NOTE]É possível usar qualquer outra ferramenta de criação da conta de usuário do Jitbit Helpdesk ou APIs fornecidas pelo Jitbit Helpdesk para provisionar as contas de usuário do AAD.

##Atribuindo usuários
  
Para testar sua configuração, é necessário conceder aos usuários do AD do Azure que você deseja que usem seu aplicativo acesso a ele.

###Para atribuir usuários ao Jitbit Helpdesk, execute as seguintes etapas:

1.  No portal do AD do Azure, crie uma conta de teste.

2.  Na página de integração de aplicativos do **Jitbit Helpdesk**, clique em **Atribuir usuários**.

    ![Atribuir usuários](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Atribuir usuários")

3.  Selecione seu usuário de teste, clique em **Atribuir** e em **Sim** para confirmar a atribuição.

    ![Sim](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Sim")
  
Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, consulte [Introdução ao Painel de Acesso](https://msdn.microsoft.com/library/dn308586).

<!---HONumber=August15_HO7-->