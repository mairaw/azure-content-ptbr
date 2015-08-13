<properties
	pageTitle="Provisionamento de aplicativo com base em atributo com filtros de escopo"
	description="Saiba como usar filtros de escopo para impedir que objetos em aplicativos, que dão suporte a provisionamento automatizado de usuários, sejam provisionados, caso um objeto não satisfaça suas necessidades de negócios."
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


# Provisionamento de aplicativo com base em atributo com filtros de escopo

O objetivo desta seção é explicar como usar filtros de escopo para definir regras baseadas em atributo que determinam quais usuários serão provisionados no aplicativo.





## Cláusulas e grupos de escopo


![Filtro de Escopo][1]




Filtros de escopo são definidos por um ou mais **grupos de escopo** e cada um deles contém uma ou mais **cláusulas**. Para ver as cláusulas de determinado grupo de escopo, expanda-o clicando na seta à esquerda do nome do grupo.

Uma **cláusula** determina quais usuários têm permissão para passar pelo filtro de escopo, avaliando os atributos de cada usuário. Por exemplo, pode haver uma cláusula que requer que o atributo de “estado” de um usuário seja igual a Nova York, o que significa que somente os usuários de Nova York serão provisionados no aplicativo.

![Nome do grupo de escopo][2]



Cada **grupo de escopo** começa com uma **cláusula** obrigatória conforme mostrado na captura de tela acima. Essa cláusula simplesmente informa que o usuário deve primeiro ser atribuído ao aplicativo antes de ser avaliado por seus filtros de escopo. Essa cláusula não pode ser excluída nem modificada.

Você pode adicionar novas cláusulas ou novos grupos de escopo, pressionando o botão adequado. Você pode dar um nome a cada grupo de escopo editando sua propriedade **Nome do Grupo de Escopo**.





## Como os filtros de escopo são avaliados

Durante o provisionamento, testamos cada usuário atribuído em relação a seus filtros de escopo para determinar se esse usuário merece acesso ao aplicativo. Você pode pensar em cada cláusula como sendo um teste que o usuário deve passar para evitar ser filtrado.

Se houver vários grupos de escopo definidos, cada usuário deverá passar em pelo menos um deles para acessar o aplicativo. Em cada grupo de escopo, no entanto, o usuário deve passar por cada cláusula única, a fim de passar para aquele grupo de escopo específico.

Em outras palavras, você pode pensar nos grupos de escopo como sendo agrupados por OR e pode considerar as cláusulas dentro deles como sendo agrupadas por AND. Por exemplo, considere o filtro de escopo abaixo:


![Nome do grupo de escopo][2]


De acordo com esse filtro de escopo, os usuários devem atender aos seguintes critérios para serem provisionados:

1. Eles devem ser atribuídos ao aplicativo.

2. Eles devem trabalhar no departamento de engenharia

3. Eles devem trabalhar em São Francisco ou no Canadá.


## Recursos adicionais

* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png

 

<!---HONumber=July15_HO5-->