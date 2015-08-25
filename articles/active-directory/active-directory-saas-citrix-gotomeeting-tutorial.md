<properties pageTitle="Tutorial: integração do Active Directory do Azure ao Citrix GoToMeeting | Microsoft Azure" description="Saiba como usar o Citrix GoToMeeting com o Active Directory do Azure para habilitar o logon único, o provisionamento automatizado e muito mais!" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#Tutorial: integração do Active Directory do Azure ao Citrix GoToMeeting  
Aplica-se ao Azure

>[AZURE.TIP]Para ver comentários, clique [aqui](http://go.microsoft.com/fwlink/?LinkId=522412).

O objetivo deste tutorial é mostrar a integração do Azure ao Citrix GoToMeeting. O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

-   Uma assinatura válida do Azure
-   Um locatário no Citrix GoToMeeting

O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1.  Habilitando a integração de aplicativos para o Citrix GoToMeeting
2.  Configurando o logon único
3.  Configurando o provisionamento de usuários
4.  Atribuindo usuários

![Configuração](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "Configuração")
##Habilitando a integração de aplicativos para o Citrix GoToMeeting

O objetivo desta seção é descrever como habilitar a integração de aplicativos para o Citrix GoToMeeting.

###Para habilitar a integração de aplicativos para o Citrix GoToMeeting, execute as seguintes etapas:

1.  No Portal de Gerenciamento do Azure, no painel navegação à esquerda, clique em **Active Directory**.

    ![Active Directory](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "Active Directory")

2.  Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3.  Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

    ![Aplicativos](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "Aplicativos")

4.  Clique em **Adicionar** na parte inferior da página.

    ![Adicionar aplicativo](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "Adicionar aplicativo")

5.  Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

    ![Adicionar um aplicativo da galeria](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "Adicionar um aplicativo da galeria")

6.  Na **caixa de pesquisa**, digite **Citrix GoToMeeting**.

    ![Citrix GoToMeeting](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  No painel de resultados, selecione **Citrix GoToMeeting** e clique em **Concluir** para adicionar o aplicativo.

    ![Citrix GoToMeeting](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##Configurando o logon único

O objetivo desta seção é descrever como permitir que os usuários se autentiquem no Citrix GoToMeeting com sua própria conta do Azure AD usando federação baseada no protocolo SAML. Como parte deste procedimento, será necessário carregar um certificado codificado em base-64 no locatário do Citrix GoToMeeting. Se você não estiver familiarizado com este procedimento, consulte [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o).

###Para configurar o logon único, execute as seguintes etapas:

1.  Na página de integração do aplicativo **Citrix GoToMeeting**, clique em **Configurar logon único**, para abrir a caixa de diálogo **CONFIGURAR LOGON ÚNICO**.

    ![Habilitar logon único](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "Habilitar logon único")

2.  Na página **Como você deseja que os usuários façam logon no Citrix GoToMeeting**, selecione **Logon Único do AD do Microsoft Azure**.

    ![Configurar logon único](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "Configurar logon único")

3.  Na página **Configurar logon único no Citrix GoToMeeting**, clique em **Baixar certificado** e salve o arquivo de certificado no computador.

    ![Configurar logon único](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "Configurar logon único")

4.  Em uma janela diferente do navegador, faça logon como administrador em seu locatário do Citrix GoToMeeting.

5.  Abra a página [Definir a configuração de logon único (SSO) de SAML 2.0](https://login.citrixonline.com/saml/settings.html) e execute as seguintes etapas:

    ![Configuração de SAML](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC689232.png "Configuração de SAML")

    1.  Selecione **Configurar manualmente**.
    2.  No portal do Azure, na página da caixa de diálogo **Configurar logon único no Citrix GoToMeeting**, copie o valor da **URL do Serviço de Saída** e cole-o na caixa de texto **URL da página de saída**.
    3.  No portal do Azure, na página da caixa de diálogo **Configurar logon único no Citrix GoToMeeting**, copie o valor da **URL do Serviço de Entrada** e cole-o na caixa de texto **URL da página de entrada**.
    4.  Crie um arquivo **codificado em base-64** usando o certificado baixado.  

        >[AZURE.TIP]Para obter mais detalhes, consulte [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o)

    5.  Clique em **Substituir certificado** e carregue seu **arquivo de certificado codificado em base-64**.
    6.  Clique em **Salvar**.

6.  No portal do Azure AD, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar Logon Único**.

    ![Configurar logon único](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "Configurar logon único")
##Configurando o provisionamento de usuários

O objetivo desta seção é descrever como habilitar o provisionamento de contas de usuário do Active Directory no Citrix GoToMeeting.

###Para configurar o provisionamento de usuários, execute as seguintes etapas:

1.  No Portal de Gerenciamento do Azure, na página de integração de aplicativos do **Citrix GoToMeeting**, clique em **Configurar o provisionamento de usuário** para abrir a caixa de diálogo **Configurar Provisionamento de Usuário**.

    ![Configure o provisionamento do usuário](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "Configure o provisionamento do usuário")

2.  Na página **Nome de usuário de administrador do Citrix GotoMeeting**, digite um nome de conta do Citrix GoToMeeting que tenha direitos administrativos em seu locatário do Citrix GoToMeeting e clique em **Avançar**.

    ![Configure o provisionamento do usuário](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "Configure o provisionamento do usuário")

3.  Na página **Confirmação**, clique na marca de seleção para salvar sua configuração.

4.  Clique no botão **Validar** para verificar sua configuração.
##Atribuindo usuários

Para testar sua configuração, será necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que o utilizem.

###Para atribuir usuários ao Citrix GoToMeeting, execute as seguintes etapas:

1.  No portal do Azure AD, crie uma conta de teste.

2.  Na página de integração de aplicativos do **Citrix GoToMeeting**, clique em **Atribuir usuários**.

    ![Atribuir usuários](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "Atribuir usuários")

3.  Selecione seu usuário de teste, clique em **Atribuir** e em **Sim** para confirmar sua atribuição.

    ![Sim](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "Sim")

Agora você deve aguardar 10 minutos e verificar se a conta foi sincronizada com o Dropbox for Business.

Como uma primeira etapa de verificação, você pode conferir o status de provisionamento clicando em Dashboard (Painel de Controle) no D na página de integração de aplicativos do **Citrix GoToMeeting** no Portal de Gerenciamento do Azure.

![Painel](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "Painel")

Um ciclo de provisionamento de usuário concluído com êxito é indicado por um status relacionado:

![Status da integração](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "Status da integração")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso.

Para obter mais detalhes sobre o Painel de Acesso, consulte [Introdução ao Painel de Acesso](https://msdn.microsoft.com/library/dn308586).

<!---HONumber=August15_HO7-->