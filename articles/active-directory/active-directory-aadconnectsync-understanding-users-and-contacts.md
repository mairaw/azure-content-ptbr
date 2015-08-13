<properties
	pageTitle="Azure AD Connect Sync: noções básicas de usuários e contatos"
	description="Explica usuários e contatos no Azure AD Connect Sync."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="swadhwa"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/27/2015"
	ms.author="markusvi"/>


# Azure AD Connect Sync: noções básicas de usuários e contatos

Há vários motivos diferentes de por que existem várias florestas do Active Directory e várias topologias de implantação diferentes. Os modelos comuns incluem uma implantação do recurso em conta e florestas sincronizadas de GAL (Lista de Endereços Global) após uma fusão e aquisição. Mas mesmo que haja modelos puros, modelos híbridos são comuns também. A configuração padrão do Azure AD Connect Sync não assume nenhum modelo específico, mas dependendo de como a compatibilidade de usuário foi selecionada na guia de instalação, comportamentos diferentes podem ser observados.

Neste tópico, veremos como a configuração padrão se comporta em certas topologias. Veremos que a configuração e o Editor de regras de sincronização podem ser usados para examinar a configuração.

A configuração pressupõe algumas regras gerais:

- Independentemente da ordem em que lemos os Active Directories de origem, o resultado final deve ser sempre o mesmo.
- Uma conta ativa sempre também contribuirá com informações de logon, incluindo **userPrincipalName** e **sourceAnchor**.
- Uma conta desabilitada contribuirá com userPrincipalName and sourceAnchor, a menos que seja uma caixa de correio vinculada.
- Uma conta com uma caixa de correio vinculada nunca será usada para userPrincipalName e sourceAnchor. Pressupõe-se que uma conta ativa será encontrada posteriormente.
- Um objeto de contato pode ser provisionado no AD do Azure como um contato ou como um usuário. Você realmente não sabe até que todas as florestas do Active Directory de origem sejam processadas.







## Contatos

Ter contatos que representam um usuário em uma floresta diferente é comum após uma fusão e aquisição em que uma solução GALSync está fazendo a ponte entre duas ou mais florestas do Exchange. O objeto de contato está sempre unindo o espaço do conector ao metaverso, usando o atributo de email. Se já houver um objeto de contato ou um objeto de usuário com o mesmo endereço de email, os objetos serão unidos. Isso é configurado na regra **Entrada do AD – Junção de Contato**. Há também uma regra denominada **Entrada no AD – Contato Comum** com um fluxo de atributos para o atributo de metaverso **sourceObjectType** com a constante **Contato**. Essa regra tem precedência muito baixa, portanto se nenhum objeto de usuário estiver associado ao mesmo objeto de metaverso, a regra Entrada do AD – Usuário Comum contribuirá com o valor Usuário para esse atributo. Com essa regra, esse atributo terá o valor Contato, se nenhum usuário tiver sido associado; e o valor Usuário se pelo menos um usuário tiver sido encontrado.

Para o provisionamento de um objeto no AD do Azure, a regra **Saída para o AAD – Junção de Contato** criará um objeto de contato se o atributo **sourceObjectType** estiver definido como **Contato**. Se esse atributo estiver definido como **Usuário**, a regra **Saída para o AAD – Junção de Usuário** criará um objeto de usuário, em vez disso. É possível que um objeto seja promovido de Contato para Usuário quando mais Active Directories de origem forem importados e sincronizados.

Por exemplo, em uma topologia de GALSync encontraremos objetos de contato para todos da segunda floresta quando importarmos a primeira floresta. Isso testará novos objetos de contato no conector AAD. Quando mais tarde, importarmos e sincronizarmos a segunda floresta, encontraremos os verdadeiros usuários e os uniremos aos objetos de metaverso existentes. Em seguida, excluiremos os objetos de contato no AAD e criaremos um novo objeto de usuário, em vez disso.

Se você tiver uma topologia em que usuários são representados como contatos, certifique-se de selecionar para combinar usuários no atributo de email no guia de instalação. Se você selecionar outra opção, terá uma configuração dependente de pedido. Objetos de contato sempre ingressarão no atributo de email, mas objetos de usuário só ingressarão no atributo de email se essa opção foi selecionada no guia de instalação. Você pode acabar com dois objetos diferentes no metaverso, com o mesmo atributo de email, se o objeto de contato tiver sido importado antes do objeto de usuário. Durante a exportação para o AD do Azure, um erro será gerado. Esse comportamento ocorre por design e indica dados incorretos ou que a topologia não foi identificada corretamente durante a instalação.





## Contas desabilitadas

Contas desabilitadas são sincronizadas também ao AD do Azure. Contas desabilitadas são comuns para representar recursos no Exchange, por exemplo, salas de conferência. A exceção são usuários com uma caixa de correio vinculada. Conforme mencionado anteriormente, eles nunca provisionarão uma conta no AD do Azure.

O pressuposto é que, se uma conta de usuário desabilitada for encontrada, não encontraremos outra conta ativa posteriormente e o objeto será provisionado no AD do Azure com o userPrincipalName e sourceAnchor encontrados. Caso outra conta ativa seja associada ao mesmo objeto de metaverso, seu userPrincipalName e sourceAnchor serão usados.





## Alterando o sourceAnchor

Quando um objeto é exportado para o AD do Azure, não é mais permitido alterar o sourceAnchor. Quando o objeto é exportado do atributo de metaverso **cloudSourceAnchor**, é definido com o valor **sourceAnchor** aceito pelo AD do Azure. Se **sourceAnchor** for alterado e não corresponder a **cloudSourceAnchor**, a regra \*\* Saída para o AAD – Junção de Usuário\*\* gerará o erro **O atributo sourceAnchor foi alterado**. Nesse caso, a configuração ou os dados ou devem ser corrigidos para que o mesmo sourceAnchor esteja presente no metaverso novamente antes que o objeto possa ser sincronizado outra vez.



## Recursos adicionais

* [Azure AD Connect Sync: personalizando opções de sincronização](active-directory-aadconnectsync-whatis.md)
* [Integração de suas identidades locais com o Active Directory do Azure](active-directory-aadconnect.md)

 
<!--Image references-->

<!---HONumber=July15_HO5-->