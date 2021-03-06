<properties
   pageTitle="Alterações de atributo do objeto de usuário externo para a visualização da colaboração B2B do Active Directory do Azure | Microsoft Azure"
   description="A B2B do Active Directory do Azure dá suporte a relações entre empresas, permitindo que os parceiros de negócios acessem de maneira seletiva seus aplicativos corporativos"
   services="active-directory"
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/27/2015"
   ms.author="viviali"/>

# Alterações de atributo do objeto de usuário externo para a visualização da colaboração B2B do AD do Azure (Active Directory do Azure)
Cada usuário em um diretório do AD do Azure é representado por um objeto de usuário. O objeto de usuário no AD do Azure passa por alterações de atributo em vários estágios do fluxo de convite-resgate da colaboração B2B. O objeto de usuário que representa o usuário do parceiro no diretório tem atributos que são alterados no momento do resgate, quando o usuário do parceiro clica no link no email de convite. Especificamente:

- Os atributos **SignInName** e **AltSecId** são populados
- O atributo **DisplayName** é alterado do nome UPN (user\_fabrikam.com#EXT#@contoso.com) para o nome de entrada (user@fabrikam.com)

O acompanhamento desses atributos no AD do Azure pode ajudar a solucionar problemas referentes a se um usuário do parceiro resgatou ou não seu convite da colaboração B2B.

## Artigos relacionados
Confira nossos outros artigos sobre a colaboração B2B do Azure:

- [O que é a colaboração AD B2B do Azure?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Como funciona](active-directory-b2b-how-it-works.md)
- [Passo a passo detalhado](active-directory-b2b-detailed-walkthrough.md)
- [Referência do formato de arquivo CSV](active-directory-b2b-references-csv-file-format.md)
- [Formato do token de usuário externo](active-directory-b2b-references-external-user-token-format.md)
- [Limitações atuais da visualização](active-directory-b2b-current-preview-limitations.md)

<!---HONumber=Nov15_HO1-->