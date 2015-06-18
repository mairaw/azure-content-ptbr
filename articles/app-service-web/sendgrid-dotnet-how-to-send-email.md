<properties 
	pageTitle="Como usar o serviço de email SendGrid (.NET) - Azure" 
	description="Saiba como enviar email com o serviço de email SendGrid no Azure. Exemplos de código escritos em c# e usam a API .NET." 
	services="app-service\web" 
	documentationCenter=".net" 
	authors="thinkingserious" 
	manager="sendgrid" 
	editor="erikre"/>

<tags 
	ms.service="app-service-web" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="dotnet" 
	ms.topic="article" 
	ms.date="02/24/2015" 
	ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork"/>





# Como enviar emails usando o SendGrid com o Azure

Última atualização: 24 de fevereiro de 2015

<h2><a name="overview"></a><span  class="short-header">Visão geral</span></h2>

Este guia demonstra como executar tarefas comuns de programação com o serviço de email SendGrid no Windows Azure. Os exemplos são escritos em código C# e utilizam a API .NET. Os cenários abordados incluem a **construção de email**, **envio de email**, **adição de anexos** e o **uso de filtros**. Para obter mais informações sobre o SendGrid e o envio de emails, consulte a seção [Próximas etapas][].

<h2><a name="whatis"></a><span  class="short-header">O que é o serviço de email SendGrid?</span></h2>

O SendGrid é um [serviço de e-mail baseado em nuvem] que oferece [entrega de e-mail transacional], escalabilidade e análise em tempo real confiáveis com APIsflexíveis que facilitam a integração personalizada. Os cenários comuns de uso do SendGrid incluem:

-   Envio automático de recibos para os clientes.
-   Administração de listas de distribuição para envio mensal de panfletos eletrônicos e ofertas especiais aos clientes.
-   Coleta de métricas em tempo real para, por exemplo, email bloqueado e capacidade de resposta do cliente.
-   Geração de relatórios para ajudar a identificar as tendências.
-   Encaminhamento de consultas dos clientes.
-   Processamento de emails de entrada.

Para obter mais informações, consulte [https://sendgrid.com](https://sendgrid.com).

<h2><a name="createaccount"></a>Criar uma conta do SendGrid</h2>

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

<h2><a name="reference"></a>Referência à biblioteca de classes do .NET do SendGrid</h2>

O [pacote NuGet do SendGrid](https://www.nuget.org/packages/Sendgrid) é a maneira mais fácil de obter a API do SendGrid e para configurar seu aplicativo com todas as dependências. O NuGet é uma extensão do Visual Studio incluído com o Microsoft Visual Studio 2012 que facilita a instalação e a atualização de bibliotecas e ferramentas.

> [AZURE.NOTE]Para instalar o NuGet se você estiver executando uma versão do Visual Studio anterior ao Visual Studio 2012, visite [http://www.nuget.org](http://www.nuget.org) e clique no botão **Instalar NuGet**.

Para instalar o pacote NuGet do SendGrid no seu aplicativo, faça o seguinte:

1.  No **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e clique em **Gerenciar Pacotes NuGet**.

2.  No painel esquerdo da caixa de diálogo **Gerenciar Pacotes NuGet**, clique em **Online**.

3.  Procure **SendGrid** e selecione o item **SendGrid** na lista de resultados (a versão atual é 5.0.0).

    ![Pacote NuGet do SendGrid][SendGrid-NuGet-package]

4.  Clique em **Instalar** para concluir a instalação e, em seguida, feche essa caixa de diálogo.

A biblioteca de classes do .NET do SendGrid é denominada **SendGridMail**. Ela contém os seguintes namespaces:

-   **SendGridMail** para criar e trabalhar com itens de email.
-   **SendGridMail.Transport** para enviar email usando o protocolo **SMTP** ou o protocolo HTTP 1.1 com **Web/REST**.

Adicione as declarações de namespace de código a seguir à parte superior de qualquer arquivo C# em que queira acessar o serviço de email SendGrid de forma programática. **System.Net** e **System.Net.Mail** são namespaces do .NET Framework que estão incluídos porque eles incluem tipos que você normalmente usará com as APIs do SendGrid.

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

<h2><a name="createemail"></a>Como criar um email</h2>

Use o objeto **SendGridMessage** para criar uma mensagem de email. Quando o objeto de mensagem for criado, você poderá definir as propriedades e os métodos, incluindo o remetente do email, o destinatário do email e o assunto e o corpo do email.

O exemplo a seguir demonstra como criar um objeto de email totalmente preenchido:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

Para obter mais informações sobre todas as propriedades e métodos com suporte do tipo **SendGrid**, consulte [sendgrid-csharp][] no GitHub.

<h2><a name="sendemail"></a>Como enviar um email</h2>

Após criar uma mensagem de email, você poderá enviá-la usando a API da Web fornecida pelo SendGrid. Alternativamente, você pode [usar a compilação do .NET na biblioteca](https://sendgrid.com/docs/Code_Examples/csharp.html).

O envio de email requer que você forneça suas credenciais de conta do SendGrid (nome de usuário e senha). O código a seguir demonstra como encapsular suas credenciais em um objeto **NetworkCredential**:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    /* Alternatively, you may store these credentials via your Azure portal
       by clicking CONFIGURE and adding the key/value pairs under "app settings".
       Then, you may access them as follows: 
       var username = System.Environment.GetEnvironmentVariable("SENDGRID_USER"); 
       var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASS");
       assuming you named your keys SENDGRID_USER and SENDGRID_PASS */

    var credentials = new NetworkCredential(username, pswd);

Os exemplos a seguir mostram como enviar umamensagem usando a API da Web.

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email.
    // You can also use the **DeliverAsync** method, which returns an awaitable task.
    transportWeb.Deliver(myMessage);

<h2><a name="addattachment"></a>Como adicionar um anexo</h2>

É possível adicionar anexos a uma mensagem, chamando o método **AddAttachment** e especificando o nome e o caminho do arquivo que você deseja anexar. Você pode incluir vários anexos, chamando esse método uma vez para cada arquivo que quiser anexar. O exemplo a seguir demonstra como adicionar um anexo a uma mensagem:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
Você também pode adicionar anexos a partir do **Stream** de dados. Isso pode ser feito ao chamar o mesmo método acima, **AddAttachment**, mas ao passar no Stream dos dados, e o nome do arquivo que você quer mostrar na mensagem. Nesse caso, você precisará adicionar a biblioteca System.IO.

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


<h2><a name="usefilters"></a><span  class="short-header">Como usar aplicativos para habilitar rodapés, rastreamento e análise</span></h2>

O SendGrid fornece a funcionalidade adicional de email por meio do uso de aplicativos. Essas são as configurações que podem ser adicionadas a uma mensagem de email para habilitar uma funcionalidade específica, como habilitar acompanhamento de clique, Googleanalytics, acompanhamento de assinatura e assim por diante. Para obter uma lista completa de aplicativos, consulte [Configurações do aplicativo][].

É possível aplicar filtros às mensagens de email do **SendGrid** usando os métodos implementados como parte da classe do **SendGrid**.

Os exemplos a seguir demonstram os filtros de rodapé e de acompanhamento de cliques:

### Rodapé

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### Rastreamento de cliques

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href="http://www.example.com">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

<h2><a name="useservices"></a>Como usar serviços adicionais do SendGrid</h2>

O SendGrid oferece API e Ganchos da web baseados na web, que você pode usar para aproveitar a funcionalidade adicional do SendGrid do aplicativo do Azure. Para obter detalhes completos, consulte a [Documentação da API do SendGrid][].

<h2><a name="nextsteps"></a>Próximas etapas</h2>

Agora que você já conhece as noções básicas do serviço de email SendGrid, siga estes links para saber mais.

* Repositório de biblioteca C# do SendGrid: [sendgrid-csharp][]
*   Documentação da API do SendGrid: <https://sendgrid.com/docs>
*   Oferta especial de SendGrid para clientes Azure: [https://sendgrid.com](https://sendgrid.com)

[Próximas etapas]: #nextsteps
[What is the SendGrid Email Service?]: #whatis
[Create a SendGrid Account]: #createaccount
[Reference the SendGrid .NET Class Library]: #reference
[How to: Create an Email]: #createemail
[How to: Send an Email]: #sendemail
[How to: Add an Attachment]: #addattachment
[How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
[How to: Use Additional SendGrid Services]: #useservices
[special offer]: https://www.sendgrid.com/windowsazure.html
[SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid01.png
[sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
[SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
[Configurações do aplicativo]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[Documentação da API do SendGrid]: https://sendgrid.com/docs
[serviço de e-mail baseado em nuvem]: https://sendgrid.com/email-solutions
[entrega de e-mail transacional]: https://sendgrid.com/transactional-email

<!--HONumber=54--> 