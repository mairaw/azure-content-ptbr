<properties 
	pageTitle="Como o Azure AD Connect funciona" 
	description="Saiba como o Azure AD Connect funciona" 
	services="active-directory" 
	documentationCenter="" 
	authors="billmath" 
	manager="terrylan" 
	editor="lisatoft"/>

<tags 
	ms.service="active-directory" 
	ms.workload="identity" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/02/2015" 
	ms.author="billmath"/>

# Como o Azure AD Connect funciona


<div class="dev-center-tutorial-selector sublanding">
<a href="/pt-br/documentation/articles/active-directory-aadconnect/" title="O que é" class="current">O que é</a> <a href="/pt-br/documentation/articles/active-directory-aadconnect-how-it-works/" title="Como funciona">Como funciona</a> <a href="/pt-br/documentation/articles/active-directory-aadconnect-get-started/" title="Introdução">Introdução</a> <a href="/pt-br/documentation/articles/active-directory-aadconnect-whats-next/" title="O que vem a seguir">O que vem a seguir</a> <a href="/pt-br/documentation/articles/active-directory-aadconnect-learn-more/" title="Saiba mais">Saiba mais</a>
</div>

O Azure Active Directory Connect é constituído de três partes principais que compõem o Azure Active Directory Connect. São elas os serviços de sincronização, a parte opcional de serviços de federação do Active Directory e a parte de monitoramento que é feita usando o Azure AD Connect Health.


<center>![Azure AD Connect Stack](./media/active-directory-aadconnect-how-it-works/AADConnectStack.png) </center>

- Sincronização - Esta parte é composta de componentes e funcionalidades lançados anteriormente como Dirsync e AAD Sync. Essa é a parte responsável pela criação de usuários e grupos. Também é responsável por garantir que as informações sobre usuários e grupos em seu ambiente local correspondam às da nuvem.
- AD FS - Esta é uma parte opcional do Azure AD Connect e pode ser usada para configurar um ambiente híbrido usando uma infraestrutura de AD FS local. Esta parte pode ser usada pela organização para abordar implantações complexas que incluem coisas como SSO de ingresso em domínio, imposição de política de logon de AD e cartão inteligente ou MFA de terceiros. Para obter informações adicionais sobre como configurar SSO, consulte [DirSync com Logon Único](https://msdn.microsoft.com/library/azure/dn441213.aspx).
- Monitoramento de integridade - Para implantações complexas usando AD FS, o Azure AD Connect Health pode fornecer monitoramento robusto de seus servidores de federação e uma localização central no portal do Azure para exibir essa atividade. Para obter informações adicionais, consulte [Azure Active Directory Connect Health](https://msdn.microsoft.com/library/azure/dn906722.aspx).






**Recursos adicionais**

* [Usar sua infraestrutura de identidade local na nuvem](active-directory-aadconnect.md)
* [Introdução ao Azure AD Connect](active-directory-aadconnect-get-started.md)
* [O que vem a seguir com o Azure AD Connect](active-directory-aadconnect-whats-next.md)
* [Saiba mais](active-directory-aadconnect-learn-more.md)
* [Azure AD Connect no MSDN](https://msdn.microsoft.com/library/azure/dn832695.aspx)

<!---HONumber=58-->