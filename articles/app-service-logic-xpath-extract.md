<properties 
   pageTitle="Extrator de XPath do BizTalk" 
   description="Extrator de XPath do BizTalk" 
   services="app-service\logic" 
   documentationCenter=".net,nodejs,java" 
   authors="prkumar" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="03/20/2015"
   ms.author="rajram"/>

#Extrator de XPath do BizTalk

O conector do Extrator de XPath do BizTalk ajuda seu aplicativo a pesquisar e extrair dados de conteúdo XML com base em determinado XPath.

##Usando o Extrator de Xpath do BizTalk
1. Para usar o Extrator de Xpath do BizTalk, primeiro você precisa criar uma instância do aplicativo de API do Extrator de Xpath do BizTalk. Isso pode ser feito de forma embutida durante a criação de um aplicativo lógico ou com a seleção do aplicativo de API do Extrator de Xpath do BizTalk do Azure Marketplace.

		NOTE: There are no configuration settings associated with BizTalk Xpath Extractor.
2. O designer mostra a ação associada ao aplicativo de API do Extrator de XPath do BizTalk.
	
![Escolher Ação do Extrator de XPath do BizTalk][1]

3. Escolha "Extrair Usando XPath"

"Extrair Usando XPath" avalia a expressão xpath de entrada em determinado Xml de entrada.

![Entrada do Extrator de XPath do BizTalk][2]

<table>
	<tr>
		<th>Parâmetro</th>
		<th>Tipo</th>
		<th>Descrição do parâmetro</th>
	</tr>
	<tr>
		<td>XPath</td>
		<td>string</td>
		<td>Caminho de consulta dentro do xml.</td>
	</tr>
	<tr>
		<td>Xml de entrada</td>
		<td>string</td>
		<td>Conteúdo do Xml de entrada.</td>
	</tr>
</table>

A ação retorna a saída como uma cadeia de caracteres ‒ Resultado. O Resultado contém o valor do caminho de consulta dentro do Xml.

<!-- References -->
[1]: ./media/app-service-logic-xpath-extract/ChooseAction.PNG
[2]: ./media/app-service-logic-xpath-extract/ConfigureInput.PNG

<!---HONumber=58-->