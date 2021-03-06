<properties 
    pageTitle="Configurando o logon único para aplicativos que não estejam na galeria de aplicativos do Active Directory do Azure | Microsoft Azure" 
    description="Saiba como conectar aplicativos ao Active Directory do Azure por conta própria usando SAML e SSO baseado em senha" 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="11/18/2015" 
    ms.author="asmalser" />

#Configurando logon único para aplicativos que não estão na galeria de aplicativo do Active Directory do Azure

A galeria de aplicativos do Active Directory do Azure fornece uma lista de aplicativos que reconhecidamente dão suporte a um formulário de logon único com o Active Directory do Azure, conforme descrito [neste artigo](https://azure.microsoft.com/documentation/articles/active-directory-appssoaccess-whatis/). Como especialista em TI ou integrador de sistema em sua organização, quando você tiver localizado o aplicativo que deseja conectar, poderá começar seguindo as instruções apresentadas no portal de gerenciamento do Azure para habilitar o logon único.

Os clientes com licenças [Active Directory Premium do Azure](https://msdn.microsoft.com/library/azure/dn532272.aspx) tem esses recursos adicionais, que podem ser chamados da categoria **Personalizado** da galeria de aplicativos do AD do Azure:

* Conexão de autoatendimento de qualquer aplicativo com suporte a provedores de identidade SAML 2.0
* Conexão de autoatendimento de qualquer aplicativo Web que tenha uma página de entrada baseada em HTML usando o SSO baseado em senha
* Capacidade de adicionar links aos aplicativos no [inicializador de aplicativos do Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) ou no [Painel de acesso do AD do Azure](https://azure.microsoft.com/documentation/articles/active-directory-appssoaccess-whatis/#deploying-azure-ad-integrated-applications-to-users)
* Conexão de autoatendimento de aplicativos que usam o protocolo SCIM para provisionamento de usuários ([descrito aqui](active-directory-scim-provisioning))

Isso pode incluir não apenas aplicativos SaaS usados por você que ainda não foram integrados à galeria de aplicativos do AD do Azure, mas também aplicativos Web de terceiros que sua organização implantou em servidores sob seu controle, seja na nuvem ou locais.

Observação: os desenvolvedores de aplicativos que estejam tentando para testar a compatibilidade entre seu aplicativo e esse recurso podem fazer isso usando uma [avaliação gratuita do Active Directory Premium do Azure](https://azure.microsoft.com/trial/get-started-active-directory/), mas são incentivados a adquirir uma [licença de Direitos de Uso Interno](https://mspartner.microsoft.com/en/us/pages/membership/internal-use-software.aspx).

##Adicionando um aplicativo personalizado ou não listado 

Para configurar um aplicativo, entre no portal de gerenciamento do Azure usando sua conta de administrador do Active Directory do Azure e navegue até a seção **Active Directory > [Diretório] > Aplicativos**, selecione **Adicionar** e **Adicionar um aplicativo da galeria**.

![][1]

Na galeria de aplicativos, você pode adicionar um aplicativo personalizado usando a categoria **Personalizado** à esquerda ou selecionando o link **Adicionar um aplicativo não listado** mostrado nos resultados da pesquisa se seu aplicativo desejado não foi encontrado. Depois de inserir um nome para seu aplicativo, você pode configurar as opções de logon único e comportamento.

**Dica**: como uma prática recomendada, use a função de pesquisa para verificar se o aplicativo já existe na galeria de aplicativos. Se o aplicativo for encontrado e sua descrição mencionar "logon único", o aplicativo já tem suporte para logon único federado.

![][2]

A adição de um aplicativo personalizado oferece uma experiência muito semelhante àquela disponível para aplicativos previamente integrados. Para iniciar, selecione **Configurar Logon Único**. A próxima tela apresenta três opções para configurar o logon único, descritas nas seções a seguir.

![][3]


##Logon único do AD do Azure

Selecione esta opção para configurar a autenticação baseada em SAML no aplicativo. Isso requer que o aplicativo dê suporte a SAML 2.0, e você deve coletar informações sobre como usar os recursos SAML do aplicativo antes de continuar. Depois de selecionar **Próximo**, você será solicitado a inserir três URLs diferentes correspondentes aos pontos de extremidade SAML para o aplicativo.

![][4]

Os ícones de dica de ferramenta no diálogo fornecem detalhes sobre o que é cada URL e como elas são usadas. Depois que eles tiverem sido inseridos, clique em **Próximo** para prosseguir para a próxima tela. Esta tela fornece informações sobre o que precisa ser configurado no lado do aplicativo para habilitá-lo a aceitar um token SAML do AD do Azure.

![][5]

Os valores necessários variam de acordo com o aplicativo; portanto, verifique a documentação do SAML do aplicativo para obter detalhes. As URLs de serviço **logon** e **saída** resolvem para o mesmo ponto de extremidade, que é o ponto de extremidade de tratamento de solicitação SAML para a instância do AD do Azure. A URL de emissor é o valor que aparece como o "emissor" dentro do token SAML emitido para o aplicativo.

Depois que o aplicativo tiver sido configurado, clique no botão **Próximo** e em **Concluir** para fechar a caixa de diálogo.

##Atribuindo usuários e grupos a seu aplicativo SAML 

Quando o aplicativo estiver configurado para usar o AD do Azure como um provedor de identidade baseado em SAML, estará quase pronto para teste. Como controle de segurança, o AD do Azure somente emitirá um token permitindo que eles entrem no aplicativo se receberem acesso usando o AD do Azure. Os usuários podem ter acesso diretamente ou através de um grupo do qual sejam membros.

Para atribuir um usuário ou grupo ao seu aplicativo, clique no botão **Atribuir Usuários**. Selecione o usuário ou grupo que você deseja atribuir e selecione o botão **Atribuir**.

![][6]

A atribuição de um usuário permite que o AD do Azure emita um token para o usuário e faz aparecer um bloco para esse aplicativo no Painel de Acesso do usuário. Um bloco do aplicativo também aparece no iniciador do aplicativo do Office 365 se o usuário estiver usando o Office 365.

Você pode carregar um logotipo de bloco para o aplicativo usando o botão **Carregar Logotipo** na guia **Configurar** do aplicativo.

###Personalizando as declarações emitidas no token SAML 

Quando um usuário é autenticado no aplicativo, o AD do Azure emite um token SAML para o aplicativo que contém informações (ou declarações) sobre o usuário que o identifica com exclusividade. Por padrão, isso inclui o nome de usuário, o endereço de email, o nome e sobrenome do usuário.

Você pode exibir ou editar as declarações enviadas no token SAML para o aplicativo na guia **Atributos**.

![][7]

Há duas razões possíveis para você precisar editar as declarações emitidas no token SAML: • O aplicativo foi escrito para exigir um conjunto diferente de URIs de declaração ou valores de declaração • Seu aplicativo foi implantado de uma forma que requer que a declaração NameIdentifier seja algo diferente do nome de usuário (conhecido como UPN) armazenado no Active Directory do Azure.

Para saber mais sobre como adicionar e editar declarações para esses cenários, confira este [artigo sobre a personalização de declarações](active-directory-saml-claims-customization.md).

###Testando o aplicativo SAML 

Depois que o certificado e as URLs de SAML tiverem sido configurados no AD do Azure e no aplicativo, que usuários ou grupos forem atribuídos ao aplicativo no Azure e que as declarações forem examinadas e editadas, se necessário, o usuário estará pronto para entrar no aplicativo.

Para testar, basta entrar no painel de acesso do AD do Azure em https://myapps.microsoft.com usando uma conta de usuário atribuída ao aplicativo e clicar no bloco do aplicativo para iniciar o processo de logon único. Como alternativa, você pode navegar diretamente para a URL de logon do aplicativo e entrar a partir daí.

Para obter dicas de depuração, confira este [artigo sobre como depurar logon único baseado em SAML para aplicativos](active-directory-saml-debugging.md)

##Logon único com senha

Selecione esta opção para configurar o [logon único baseado em senha](active-directory-appssoaccess-whatis.md) para um aplicativo Web que possua uma página de entrada em HTML. O SSO baseado em senha, também conhecido como armazenamento de senha em cofre, permite que você gerencie o acesso de usuários e senhas para aplicativos Web que não dão suporte a federação de identidades. Também é útil para cenários onde vários usuários precisam compartilhar uma única conta, como contas de aplicativo de mídia social da sua organização.

Depois de selecionar **Próximo**, você deverá inserir a URL da página de entrada baseada na Web do aplicativo. Observe que deve ser a página que inclui os campos de entrada de nome de usuário e senha. Depois de inserida, o AD do Azure iniciará um processo para analisar a página de entrada em busca de entradas de nome de usuário e de senha. Se o processo não for bem-sucedido, ele o orientará por um processo alternativo de instalação de uma extensão de navegador (requer o Internet Explorer, o Chrome ou o Firefox) que permite que você capture os campos manualmente.

Depois que a página for capturada, os usuários e grupos podem ser atribuídos e as políticas de credencial podem ser definidas como [aplicativos de SSO de senha](active-directory-appssoaccess-whatis.md) normais.

Observação: você pode carregar um logotipo de bloco para o aplicativo usando o botão **Carregar Logotipo** na guia **Configurar** do aplicativo.

##Logon único existente

Selecione esta opção para adicionar um link para um aplicativo no Painel de Acesso do AD do Azure ou no portal do Office 365 da sua organização. Você pode usar isso para adicionar links para aplicativos Web personalizado que atualmente usam o Serviços de Federação do Active Directory do Azure (ou outro serviço de federação) em vez do AD do Azure para autenticação. Ou você pode adicionar links profundos para páginas específicas do SharePoint ou outras páginas da Web que você queira que apareçam somente nos Painéis de Acesso do usuário.

Depois de selecionar **Próximo**, você deverá inserir a URL do aplicativo a ser vinculado. Depois de concluído, os usuários e os grupos poderão ser atribuídos ao aplicativo, o que faz com que o aplicativo seja exibido no [inicializador de aplicativos do Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) ou no [painel de acesso do AD do Azure](https://azure.microsoft.com/documentation/articles/active-directory-appssoaccess-whatis/#deploying-azure-ad-integrated-applications-to-users) desses usuários.

Observação: você pode carregar um logotipo de bloco para o aplicativo usando o botão **Carregar Logotipo** na guia **Configurar** do aplicativo.

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png

<!---HONumber=Nov15_HO4-->