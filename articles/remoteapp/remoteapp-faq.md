<properties 
	pageTitle="Perguntas frequentes sobre o RemoteApp do Azure" 
	description="Perguntas frequentes sobre o RemoteApp do Azure." 
	services="remoteapp" 
	documentationCenter="" 
	authors="lizap" 
	manager="mbaldwin" 
	editor=""/>

<tags 
	ms.service="remoteapp" 
	ms.workload="compute" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/28/2015" 
	ms.author="elizapo"/>

# Perguntas frequentes sobre o RemoteApp do Azure
Ouvimos as seguintes perguntas sobre o RemoteApp do Azure. Você tem outras? Visite os [fóruns do RemoteApp](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) e diga-nos o que você precisa saber.

## O que é o RemoteApp do Azure? ##


- **O que é o RemoteApp do Azure?** O RemoteApp é um serviço do Azure que fornece a funcionalidade do Microsoft RemoteApp local, composto pelos Serviços de Área de Trabalho Remota do Azure. O RemoteApp o ajudará a fornecer acesso remoto seguro aos aplicativos de vários dispositivos de usuário diferentes. Saiba mais sobre o [RemoteApp do Azure](remoteapp-whatis.md).
- **Quais são os dois tipos de opções de implantação?** Há dois tipos de implantações de RemoteApp (ou coleções): híbrido e nuvem. Descubra qual [Opção de implantação](remoteapp-whatis.md) funciona melhor para a sua organização.

## Configurações com suporte ##


- **Há suporte para aplicativos personalizados da linha de negócios (LOB)?** Sim. Para usar um aplicativo personalizado no RemoteApp do Azure, crie uma [imagem personalizada de modelo](remoteapp-create-custom-image.md) e, em seguida, carregue-a para a coleção do RemoteApp.
- **Meu aplicativo de LOB personalizado funcionará no RemoteApp do Azure?** A melhor maneira de avaliar isso é testando-o. Examine os [requisitos de compatibilidade do aplicativo](http://www.microsoft.com/download/details.aspx?id=18704) e verifique o [Centro de compatibilidade de área de trabalho remota](http://www.rdcompatibility.com/compatibility/default.aspx).
- **Qual método de implantação (nuvem ou híbrido) é o melhor para a minha organização?** As coleções híbridas fornecem a experiência mais completa se você deseja integração total com o logon único (SSO) e conectividade segura de rede local. As coleções de nuvem fornecem uma maneira fácil e ágil de isolar a sua implantação usando vários métodos de autenticação. Leia mais sobre as [Opções de implantação](remoteapp-whatis.md).
- **A coleção híbrida requer uma VNET. Podemos usar nosso NET existente?** Não neste momento, mas nós sabemos que você deseja. Enquanto estamos trabalhando nisso, você pode conectar a sua VNET existente ao Azure RemoteApp VNET seguindo [estas instruções](http://blogs.msdn.com/b/rds/archive/2014/07/21/how-to-link-azure-remoteapp-to-an-existing-vnet.aspx).
- **Posso usar uma nuvem ou máquina virtual existente como modelo para a Minha coleção de RemoteApp?** Sim! Você pode criar uma imagem com base em uma VM do Azure, use uma das imagens incluídas com a sua assinatura ou crie uma imagem personalizada. Confira as [Opções de imagem do RemoteApp](remoteapp-imageoptions.md).
- **Temos SQL ou outro banco de dados local ou no Azure. Qual o tipo de implantação que nós usamos?** Isso depende de onde está o seu banco de dados SQL ou back-end. Se o banco de dados estiver em uma rede privada, use a coleção híbrida. Se o banco de dados estiver exposto à Internet e permite ao cliente conexões para se conectar a ele, você pode usar a coleção na nuvem.
- **E o mapeamento de unidade, USB e porta serial, o compartilhamento de área de transferência e o redirecionamento de impressora?** Todos esses recursos têm suporte no RemoteApp do Azure. O redirecionamento de impressora e o compartilhamento de área de transferência estão habilitados por padrão. Você pode aprender mais sobre redirecionamento [aqui](remoteapp-redirection.md). 


- **E quanto à autenticação? Há suporte para quais métodos?** A coleção na nuvem oferece suporte a contas da Microsoft e contas do Active Directory do Azure, que são contas do Office 365 também. A coleção híbrida dá suporte somente às contas do Active Directory do Azure que foram sincronizadas (usando uma ferramenta como o [Azure Active Directory Sync](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) em uma implantação do Active Directory do Windows Server. Especificamente, ou sincronizado com a opção de sincronização de senha ou sincronizado com a federação de Serviços de Federação do Active Directory (AD FS) configurada. Você também pode configurar a [Autenticação multifator (MFA)](../../services/multi-factor-authentication/).

	**Observação:** os usuários do Active Directory do Azure devem ser do locatário que está associado à sua assinatura. (Você pode exibir e modificar a sua assinatura na guia **Configurações** no portal. Consulte [Alterar o locatário do Active Directory do Azure usado pelo RemoteApp](remoteapp-changetenant.md) para obter mais informações.)

- **Por que não posso dar o meu acesso de conta do Active Directory do Azure?** Os usuários do Active Directory do Azure devem ser do diretório que está associado à sua assinatura. Você pode exibir ou modificar esse diretório na guia Configurações no portal. Consulte [Alterar o locatário do Active Directory do Azure usado pelo RemoteApp](remoteapp-changetenant.md) para obter mais informações.
- **Para quais dispositivos e sistemas operacionais os aplicativos cliente dão suporte?** Os aplicativos cliente estão disponíveis para Windows 8.1, Windows 8, Windows 7 Service Pack 1, iOS, Mac OS X, Windows RT, dispositivos com Android e Windows Phone. Também há suporte para a visualização do Windows 10.
 
	[Baixe](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) um cliente RemoteApp agora.
- **O RemoteApp do Azure dá suporte a clientes finos?** Sim, há suporte para os seguintes clientes finos do Windows Embedded:
	- Windows Embedded Standard 7 com Service Pack 1
	- Windows Embedded POSReady7 
	- Windows Embedded Thin PC 
	- Windows Embedded 8.1 Industry

- **Qual versão do Windows Server tem suporte para o Host da Sessão da Área de Trabalho Remota (RDSH)?** Windows Server 2012 R2.

##Suporte e comentários

- **Posso testar esse serviço gratuitamente?** Sim. Há uma avaliação gratuita disponível por 30 dias. Após o término da avaliação, você poderá fazer a transição para uma conta paga (que pode ser usada na produção) ou parar de usar o serviço. Inicie a sua avaliação gratuita acessando [manage.windowsazure.com](http://manage.windowsazure.com) - crie uma nova instância do RemoteApp. Com a versão de avaliação gratuita, você pode criar duas (2) instâncias do RemoteApp com 10 usuários por instância. Lembre-se de que esta avaliação dura 30 dias.
- **O que é o plano de suporte para o RemoteApp?** O suporte para gerenciamento de assinaturas e cobrança é fornecido sem custo adicional. O suporte técnico está disponível por meio de [planos de serviço do Azure](../../../support/plans/). Você também pode obter suporte gratuito para a comunidade por meio do nosso [Fórum de discussão do Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp). 
- **Qual é o custo do RemoteApp?** Verifique os [Detalhes de preços do Azure RemoteApp](../../../pricing/details/remoteapp/).
- **Como posso enviar comentários?** Visite o [Fórum de comentários](http://feedback.azure.com/forums/247748-azure-remoteapp).
- **Com quem eu posso falar para saber mais sobre o Azure RemoteApp?** Além do nosso [Fórum de discussão](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), que é um ótimo lugar para publicar perguntas, você pode se associar ao webinar semanal [Pergunte aos especialistas](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), onde podemos falar sobre tudo acerca do RemoteApp.
- **E a documentação do RemoteApp?** Estamos felizes com a pergunta. Além do conteúdo da Ajuda na gaveta de Ajuda do portal (basta clicar no **?** em qualquer página do portal), os artigos a seguir estão disponíveis para ensinar a você tudo sobre o RemoteApp:
	- **Introdução:**
		- [O que é o RemoteApp?](remoteapp-whatis.md)
		- [O que são as imagens de modelo do RemoteApp?](remoteapp-images.md)
		- [Como funciona o licenciamento?](remoteapp-licensing.md)
		- [Como o RemoteApp e o Office funcionam juntos?](remoteapp-o365.md)
		- [Como o redirecionamento funciona no RemoteApp](remoteapp-redirection.md)?
	- **Implantar:**
		- [Criar uma imagem personalizada de modelo](remoteapp-create-custom-image.md)
		- [Criar uma coleção híbrida](remoteapp-create-hybrid-deployment.md)
		- [Criar uma coleção na nuvem](remoteapp-create-cloud-deployment.md)
		- [Configurar o Active Directory do Azure para o RemoteApp](remoteapp-ad.md)
		- [Publicar um aplicativo no RemoteApp](remoteapp-publish.md)
	- **Gerenciar:**
		- [Adicionar usuários](remoteapp-user.md)
		- [Práticas recomendadas para configurar e usar o RemoteApp](remoteapp-bestpractices.md)	

	Vídeos! Também temos inúmeros vídeos sobre o RemoteApp. Alguns fornecem a Introdução ([Introdução ao RemoteApp do Azure](http://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)) enquanto outros o orientam por meio da implantação ([implantação de nuvem](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) e [implantação híbrida](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Experimente-os!


<!--HONumber=54--> 