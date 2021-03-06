<properties 
    pageTitle="Tutorial: Integração do Active Directory do Azure com o Mimecast Personal Portal | Microsoft Azure" 
    description="Saiba como usar o Mimecast Personal Portal com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!" 
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

#Tutorial: Integração do Active Directory do Azure com o Mimecast Personal Portal
  
O objetivo deste tutorial é mostrar a integração do Azure com o Mimecast Personal Portal. O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

-   Uma assinatura válida do Azure
-   Uma assinatura do Mimecast Personal Portal com logon único habilitado
  
Depois de concluir este tutorial, os usuários do Azure AD atribuídos ao Mimecast Personal Portal poderão fazer logon único no aplicativo em seu site de empresa do Mimecast Personal Portal (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).
  
O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1.  Habilitando a integração de aplicativos com o Mimecast Personal Portal
2.  Configurando o logon único
3.  Configurando o provisionamento de usuários
4.  Atribuindo usuários

![Cenário](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794991.png "Cenário")
##Habilitando a integração de aplicativos com o Mimecast Personal Portal
  
O objetivo desta seção é descrever como habilitar a integração de aplicativos com o Mimecast Personal Portal.

###Para habilitar a integração de aplicativos com o Mimecast Personal Portal, execute as seguintes etapas:

1.  No Portal de Gerenciamento do Azure, no painel navegação à esquerda, clique em **Active Directory**.

    ![Active Directory](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700993.png "Active Directory")

2.  Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3.  Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

    ![Aplicativos](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700994.png "Aplicativos")

4.  Clique em **Adicionar** na parte inferior da página.

    ![Adicionar aplicativo](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749321.png "Adicionar aplicativo")

5.  No diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

    ![Adicionar um aplicativo da galeria](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749322.png "Adicionar um aplicativo da galeria")

6.  Na **caixa de pesquisa**, digite **Mimecast Personal Portal**.

    ![Galeria de Aplicativos](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794992.png "Galeria de Aplicativos")

7.  No painel de resultados, selecione **Mimecast Personal Portal** e clique em **Concluir** para adicionar o aplicativo.

    ![Mimecast Personal Portal](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794993.png "Mimecast Personal Portal")
##Configurando o logon único
  
O objetivo desta seção é descrever como permitir que os usuários se autentiquem no Mimecast Personal Portal com sua conta do AD do Azure usando federação baseada em protocolo SAML. Como parte deste procedimento, será necessário criar um arquivo de certificado codificado em base 64. Se você não estiver familiarizado com esse procedimento, veja [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o).

###Para configurar o logon único, execute as seguintes etapas:

1.  No portal do Azure AD, na página de integração de aplicativos do **Mimecast Personal Portal**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.

    ![Configurar o logon único](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794994.png "Configurar o logon único")

2.  Na página **Como você deseja que os usuários façam logon no Mimecast Personal Portal**, selecione **Logon Único do AD do Microsoft Azure** e clique em **Avançar**.

    ![Configurar o logon único](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794995.png "Configurar o logon único")

3.  Na página **Configurar URL do Aplicativo**, na caixa de texto **URL de Logon do Mimecast Personal Portal**, digite a URL usada pelos usuários para fazer logon no seu aplicativo do Mimecast Personal Portal (por exemplo, “https://webmail-uk.mimecast.com” ou “https://webmail-us.mimecast.com”) e clique em **Avançar**.

    >[AZURE.NOTE]A URL de logon é específica para a região.

    ![Configurar a URL do Aplicativo](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794996.png "Configurar a URL do Aplicativo")

4.  Na página **Configurar logon único no Mimecast Personal Portal**, para baixar seu certificado, clique em **Baixar certificado** e salve o arquivo de certificado localmente no computador.

    ![Configurar o logon único](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794997.png "Configurar o logon único")

5.  Em outra janela do navegador da Web, faça logon em seu Mimecast Personal Portal como um administrador.

6.  Vá para **Serviços > Aplicativo**.

    ![Aplicativos](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794998.png "Aplicativos")

7.  Clique em **Perfis de Autenticação**.

    ![Perfis de Autenticação](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794999.png "Perfis de Autenticação")

8.  Clique em **Novo Perfil de Autenticação**.

    ![Novo Perfil de Autenticação](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795000.png "Novo Perfil de Autenticação")

9.  Na seção **Perfil de Autenticação**, realize as seguintes etapas:

    ![Perfil de Autenticação](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795001.png "Perfil de Autenticação")

    1.  Na caixa de texto **Descrição**, digite um nome para a sua configuração.
    2.  Selecione **Impor Autenticação SAML para o Mimecast Personal Portal**.
    3.  Como **Provedor**, selecione **Active Directory do Azure**.
    4.  No portal do Azure, na página do diálogo **Configurar logon único no Mimecast Personal Portal**, copie o valor da **URL do Emissor** e cole-o na caixa de texto **URL do Emissor**.
    5.  No portal do Azure, na página do diálogo **Configurar logon único no Mimecast Personal Portal**, copie o valor da **URL do Emissor** e cole-o na caixa de texto **URL de Logon**.
    6.  No portal do Azure, na página do diálogo **Configurar logon único no Mimecast Personal Portal**, copie o valor da **URL do Emissor** e cole-o na caixa de texto **URL de Logoff**.  

        >[AZURE.NOTE]Os valores da URL de logon e da URL de logoff para o Mimecast Personal Portal são os mesmos.

    7.  Crie um arquivo **codificado em Base 64** por meio do certificado baixado.

        >[AZURE.TIP]Para obter mais detalhes, veja [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o).

    8.  Abra seu certificado codificado em Base 64 no bloco de notas, remova a primeira linha (“*--*”) e a última linha (“*--*”), copie o conteúdo restante na área de transferência e cole-o na caixa de texto **Certificado do Provedor de Identidade (Metadados)**.
    9.  Selecione **Permitir Logon Único**.
    10. Clique em **Salvar**.

10. No portal do AD do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar Logon Único**.

    ![Configurar o logon único](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795002.png "Configurar o logon único")
##Configurando o provisionamento de usuários
  
Para permitir que os usuários do AD do Azure façam logon no Mimecast Personal Portal, eles devem ser provisionados no Mimecast Personal Portal. No caso do Mimecast Personal Portal, o provisionamento é uma tarefa manual.
  
Você precisa registrar um domínio antes de criar usuários.

###Para configurar o provisionamento de usuários, execute as seguintes etapas:

1.  Faça logon no **Mimecast Personal Portal** como administrador.

2.  Vá para **Diretórios > Interno**.

    ![Diretórios](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795003.png "Diretórios")

3.  Clique em **Registrar Novo Domínio**.

    ![Registrar Novo Domínio](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795004.png "Registrar Novo Domínio")

4.  Depois de criar o novo domínio, clique em **Novo Endereço**.

    ![Novo Endereço](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795005.png "Novo Endereço")

5.  Na caixa de diálogo do novo endereço, execute as seguintes etapas:

    ![Salvar](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795006.png "Salvar")

    1.  Digite os atributos de **Endereço de Email**, **Nome Global**, **Senha** e **Confirmar Senha** de uma conta válida do AAD que você deseja provisionar nas caixas de texto relacionadas.
    2.  Clique em **Salvar**.

>[AZURE.NOTE]É possível usar qualquer outra ferramenta de criação da conta de usuário do Mimecast Personal Portal ou as APIs fornecidas pelo Mimecast Personal Portal para provisionar as contas de usuário do AAD.

##Atribuindo usuários

Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do AD do Azure que você deseja que usem seu aplicativo.

###Para atribuir usuários ao Mimecast Personal Portal, execute as seguintes etapas:

1.  No portal do AD do Azure, crie uma conta de teste.

2.  Na página de integração de aplicativos do **Mimecast Personal Portal**, clique em **Atribuir usuários**.

    ![Atribuir Usuários](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795007.png "Atribuir Usuários")

3.  Selecione seu usuário de teste, clique em **Atribuir** e em **Sim** para confirmar a atribuição.

    ![Sim](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC767830.png "Sim")
  
Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

<!---HONumber=Nov15_HO1-->