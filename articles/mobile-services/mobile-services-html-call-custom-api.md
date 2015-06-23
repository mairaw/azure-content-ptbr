<properties 
	pageTitle="Chamar uma API personalizada em um cliente HTML - Serviços Móveis" 
	description="Saiba como definir uma API personalizada e chamá-la de um aplicativo HTML que usa os Serviços Móveis do Azure." 
	services="mobile-services" 
	documentationCenter="" 
	authors="ggailey777" 
	writer="jparrel" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="mobile-services" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="" 
	ms.devlang="javascript" 
	ms.topic="article" 
	ms.date="02/26/2015" 
	ms.author="glenga"/>

# Chamar uma API personalizada a partir de um aplicativo HTML

[AZURE.INCLUDE [mobile-services-selector-call-custom-api](../../includes/mobile-services-selector-call-custom-api.md)]

Este tópico mostra como chamar uma API personalizada a partir de um aplicativo HTML. Uma API personalizada permite que você defina pontos de extremidade personalizados que expõem a funcionalidade do servidor que não mapeia para uma inserção, atualização, exclusão ou operação de leitura. Usando uma API personalizada, você pode ter mais controle sobre mensagens, incluindo ler e definir cabeçalhos de mensagens HTTP e definir um formato de corpo de mensagem diferente do JSON.

A API personalizada criada neste tópico fornece a capacidade de enviar uma única solicitação POST que define o sinalizador concluído como `true` para todos os itens de tarefas pendentes na tabela. Sem essa API personalizada, o cliente precisa enviar solicitações individuais para atualizar o sinalizador para cada item todo na tabela.

Você adicionará essa funcionalidade ao aplicativo que você criou quando concluiu o tutorial [Introdução aos Serviços Móveis] ou [Introdução a dados]. Para fazer isso, você executará as seguintes etapas:

1. [Definir a API personalizada]
2. [Atualizar o aplicativo para chamar a API personalizada]
3. [Testar o aplicativo] 

Este tutorial baseia-se no quickstart dos Serviços Móveis. Antes de iniciar este tutorial, você deve primeiro concluir o tutorial [Introdução aos Serviços Móveis] ou [Introdução aos dados].

## <a name="define-custom-api"></a>Definir a API personalizada

[AZURE.INCLUDE [mobile-services-create-custom-api](../../includes/mobile-services-create-custom-api.md)]

<h2><a name="update-app"></a>Atualizar o aplicativo para chamar a API personalizada</h2>

1. Usando seu editor de texto, abra o arquivo index.html, localize o elemento **botão** chamado `buttonRefresh` e adicione o novo elemento abaixo logo depois dele: 

		<button id="buttonCompleteAll">Complete All</button> 

	Isso adiciona um novo botão à página.

2. Em page.js e após a função **refreshTodoItems**, inclua o código a seguir:

		var completeAllTodoItems = function () {
			// Asynchronously call the custom API using the POST method.
			client.invokeApi("completeall", {
				body: null,
				method: "post"
			}).done(function (results) {
				var message = results.result.count + " item(s) marked as complete.";
				alert(message);
				refreshTodoItems();
			}, function(error) {
				alert(error.message);
			});
		};

		$('#buttonCompleteAll').click(function () {
			completeAllTodoItems();
		});

	Esse método identifica o evento **Click** do novo botão. O método **invokeApi** é chamado no cliente, que envia uma solicitação POST para a nova API personalizada. O resultado retornado pela API personalizada é exibido em uma caixa de diálogo de mensagem, se houver erros.

## <a name="test-app"></a>Testar o aplicativo

1. Atualize seu navegador.

2. No aplicativo, digite um texto em **Inserir um TodoItem** e, em seguida, clique em **Salvar**.

3. Repita a etapa anterior até que tenha incluído vários itens pendentes na lista.

4. Clique no botão **Concluir Tudo**. É exibida uma caixa de diálogo de mensagem que indica o número de itens marcados como concluídos, e a consulta filtrada é executada novamente, o que limpa todos os itens da lista.

## Próximas etapas

Agora que você criou uma API personalizada e a chamou a partir de seu aplicativo HTML, saiba mais sobre os tópicos de Serviços Móveis a seguir:

* [Referência de script de servidor de Serviços Móveis]<br/>Saiba mais sobre como criar APIs personalizadas.

<!-- Anchors. -->
[Definir a API personalizada]: #define-custom-api
[Atualizar o aplicativo para chamar a API personalizada]: #update-app
[Testar o aplicativo]: #test-app
[Next Steps]: #next-steps

<!-- URLs. -->
[Referência de script de servidor de Serviços Móveis]: http://go.microsoft.com/fwlink/?LinkId=262293
[My Apps dashboard]: http://go.microsoft.com/fwlink/?LinkId=262039
[Introdução aos Serviços Móveis]: mobile-services-html-get-started.md
[Introdução a dados]: mobile-services-html-get-started-data.md
[Introdução aos dados]: mobile-services-html-get-started-data.md
<!--HONumber=54--> 