<properties 
    pageTitle="Tutorial: Integração do AD do Azure com o Sciforma | Microsoft Azure" 
    description="Saiba como usar o Sciforma com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" 
    manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/22/2015" 
    ms.author="markvi" />

#Tutorial: Integração do AD do Azure com o Sciforma
  
O objetivo deste tutorial é mostrar a integração do Azure ao Sciforma. O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

-   Uma assinatura válida do Azure
-   Um locatário do Sciforma
  
Depois de concluir este tutorial, os usuários do Azure AD atribuídos ao Sciforma poderão fazer logon único no aplicativo em seu site de empresa do Sciforma (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).
  
O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1.  Habilitando a integração de aplicativos para o Sciforma
2.  Configurando o logon único
3.  Configurando o provisionamento de usuários
4.  Atribuindo usuários

![Cenário](./media/active-directory-saas-sciforma-tutorial/IC777369.png "Cenário")
##Habilitando a integração de aplicativos para o Sciforma
  
O objetivo desta seção é descrever como habilitar a integração de aplicativos para o Sciforma.

###Para habilitar a integração de aplicativos com o Sciforma, execute as seguintes etapas:

1.  No Portal de Gerenciamento do Azure, no painel navegação à esquerda, clique em **Active Directory**.

    ![Active Directory](./media/active-directory-saas-sciforma-tutorial/IC700993.png "Active Directory")

2.  Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3.  Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

    ![Aplicativos](./media/active-directory-saas-sciforma-tutorial/IC700994.png "Aplicativos")

4.  Clique em **Adicionar** na parte inferior da página.

    ![Adicionar aplicativo](./media/active-directory-saas-sciforma-tutorial/IC749321.png "Adicionar aplicativo")

5.  No diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

    ![Adicionar um aplicativo da galeria](./media/active-directory-saas-sciforma-tutorial/IC749322.png "Adicionar um aplicativo da galeria")

6.  Na **caixa de pesquisa**, digite **Sciforma**.

    ![Galeria de aplicativos](./media/active-directory-saas-sciforma-tutorial/IC777370.png "Galeria de aplicativos")

7.  No painel de resultados, selecione **Sciforma** e clique em **Concluir** para adicionar o aplicativo.

    ![Sciforma](./media/active-directory-saas-sciforma-tutorial/IC777371.png "Sciforma")
##Configurando o logon único
  
O objetivo desta seção é descrever como permitir que os usuários se autentiquem no Sciforma com a respectiva conta do AD do Azure usando federação baseada no protocolo SAML.

###Para configurar o logon único, execute as seguintes etapas:

1.  No portal do Azure AD, na página de integração de aplicativos do **Sciforma**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.

    ![Configurar o logon único](./media/active-directory-saas-sciforma-tutorial/IC777372.png "Configurar logon único")

2.  Na página **Como você deseja que os usuários façam logon no Sciforma**, selecione **Logon Único do AD do Microsoft Azure** e clique em **Avançar**.

    ![Configurar logon único](./media/active-directory-saas-sciforma-tutorial/IC777373.png "Configurar logon único")

3.  Na página **Configurar URL do Aplicativo**, na caixa de texto **URL de Entrada do Sciforma**, digite a URL usando o padrão "*https://\<nome-locatário>.Sciforma.com*" e clique em **Avançar**.

    ![Configurar a URL do aplicativo](./media/active-directory-saas-sciforma-tutorial/IC777374.png "Configurar a URL do aplicativo")

4.  Na página **Configurar logon único no Sciforma**, para baixar os metadados, clique em **Baixar metadados** e salve o arquivo de dados localmente como **c:\\SciformaMetaData.xml**.

    ![Configurar logon único](./media/active-directory-saas-sciforma-tutorial/IC777375.png "Configurar logon único")

5.  Encaminhe esse arquivo de metadados à equipe de suporte do Sciforma. A equipe de suporte precisa configurar o logon único para você.

6.  Selecione a confirmação de configuração de logon único e clique em **Concluir** para fechar o diálogo **Configurar Logon Único**.

    ![Configurar logon único](./media/active-directory-saas-sciforma-tutorial/IC777376.png "Configurar o logon único")
##Configurando o provisionamento de usuários
  
Não há nenhum item de ação para a configuração de provisionamento de usuário para o Sciforma. Quando um usuário atribuído tentar fazer logon no Sciforma usando o painel de acesso, o Sciforma verificará se o usuário existe. Se ainda não houver uma conta de usuário disponível, ela será automaticamente criada pelo Sciforma.
##Atribuindo usuários
  
Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do AD do Azure que você deseja que usem seu aplicativo.

###Para atribuir usuários ao Sciforma, execute as seguintes etapas:

1.  No portal do AD do Azure, crie uma conta de teste.

2.  Na página de integração de aplicativos do **Sciforma**, clique em **Atribuir usuários**.

    ![Atribuir usuários](./media/active-directory-saas-sciforma-tutorial/IC777377.png "Atribuir usuários")

3.  Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.

    ![Sim](./media/active-directory-saas-sciforma-tutorial/IC767830.png "Sim")
  
Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

<!---HONumber=Nov15_HO1-->