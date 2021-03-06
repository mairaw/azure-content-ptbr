<properties
   pageTitle="Serviço de comunicação com a API Web do ASP.NET | Microsoft Azure"
   description="Saiba como implementar passo a passo a comunicação de serviço usando o API Web do ASP.NET com auto-hospedagem OWIN na API de Serviços Confiáveis."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="11/13/2015"
   ms.author="vturecek"/>

# Introdução aos serviços de API Web da malha de serviço do Microsoft Azure com auto-hospedagem do OWIN

A Malha de Serviço lhe dá o poder de decisão sobre como você deseja que seus serviços se comuniquem com os usuários e como eles devem se comunicar entre si. Este tutorial se concentra na implementação da comunicação de serviço usando o API Web ASP.NET com auto-hospedagem OWIN no API de *Serviços Confiáveis* da malha de serviços. Vamos detalhadamente o API de comunicação conectável dos *Serviços Confiáveis* e mostrar passo a passo como configurar um ouvinte de comunicação personalizado para o serviço com o API Web usado como exemplo. Para ver um exemplo completo de um ouvinte de comunicação de API Web, confira o [exemplo de WebApplication da Malha de Serviço no GitHub](https://github.com/Azure/servicefabric-samples/tree/master/samples/Services/VS2015/WebApplication).


## Introdução ao API Web na Malha de Serviço

O API Web ASP.NET é uma estrutura popular e poderosa para criar APIs HTTP sobre o .NET Framework. Vá diretamente para [www.asp.net/webapi](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) para saber mais sobre API Web caso não esteja familiarizado com ele.

A API Web na Malha de Serviço é o mesma API Web ASP.NET que você conhece e adora. A diferença está em como você *hospeda* um aplicativo de API Web (dica: você não estará usando o IIS). Para compreender melhor a diferença, vamos dividir isso em duas partes:

 1. O aplicativo de API Web (seus controladores, modelos, etc.)
 2. O host (o servidor web, geralmente IIS)

O próprio aplicativo API Web não muda aqui - não é diferente de aplicativos de API Web que você possa ter desenvolvido no passado e você poderá simplesmente mover a maioria do código do aplicativo diretamente. A hospedagem do aplicativo pode ser um pouco diferente da que você está acostumado, caso normalmente use a hospedagem no IIS. Mas antes de entrar na parte de hospedagem, vamos começar com a parte mais familiar: o aplicativo API Web.


## Criar o aplicativo

Comece criando um novo aplicativo do Service Fabric, com um único serviço sem estado no Visual Studio 2015:

![Criar um novo aplicativo da Malha de Serviço](media/service-fabric-reliable-services-communication-webapi/webapi-newproject.png)

![Criar um único serviço sem estado](media/service-fabric-reliable-services-communication-webapi/webapi-newproject2.png)

Isso nos dá um serviço sem estado vazio que hospedará o aplicativo API Web. Vamos configurar o aplicativo do zero para ver como juntar tudo isso.

A primeira etapa é obter alguns pacotes NuGet para o API Web. O pacote que queremos usar é o **Microsoft.AspNet.WebApi.OwinSelfHost**. Este pacote inclui todos os pacotes de API Web necessários e os pacotes do *host* - isso será importante mais tarde.

![](media/service-fabric-reliable-services-communication-webapi/webapi-nuget.png)

Com os pacotes instalados, podemos começar a criar a estrutura de projeto básico de API Web. Se você usou o API Web, a estrutura do projeto deve ser bastante familiar. Comece criando os diretórios de API Web básicos:

 + App\_Start
 + Controladores
 + Modelos

Adicione as classes de configuração básicas do API Web no diretório App\_Start. Agora vamos adicionar uma configuração do formatador de tipo de mídia vazia:

 + FormatterConfig.cs

```csharp

namespace WebApiService
{
    using System.Net.Http.Formatting;

    public static class FormatterConfig
    {
        public static void ConfigureFormatters(MediaTypeFormatterCollection formatters)
        {
        }
    }
}

```

Adicione um controlador padrão no diretório de controladores:

 + DefaultController.cs

```csharp

namespace WebApiService.Controllers
{
    using System.Collections.Generic;
    using System.Web.Http;

    [RoutePrefix("api")]
    public class DefaultController : ApiController
    {
        // GET api/values
        [Route("values")]
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5
        [Route("values/{id}")]
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values
        [Route("values")]
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5
        [Route("values/{id}")]
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5
        [Route("values/{id}")]
        public void Delete(int id)
        {
        }
    }
}

```

Finalmente, adicione uma classe de Inicialização à raiz do projeto para registrar o roteamento, formatadores e qualquer outra configuração. Este também é o local onde o API Web se conecta ao *host*, que será revisto novamente mais tarde. Ao configurar a classe Startup, crie uma interface chamada *IOwinAppBuilder* para a classe Startup que define o método de configuração. Embora tecnicamente não seja necessário que o API Web funcione, ele permitirá o uso mais flexível do uso da classe Startup posteriormente.

 + Startup.cs

```csharp

namespace WebApiService
{
    using Owin;
    using System.Web.Http;

    public class Startup : IOwinAppBuilder
    {
        public void Configuration(IAppBuilder appBuilder)
        {
            HttpConfiguration config = new HttpConfiguration();

            config.MapHttpAttributeRoutes();
            FormatterConfig.ConfigureFormatters(config.Formatters);

            appBuilder.UseWebApi(config);
        }
    }
}

```

 + IOwinAppBuilder.cs

```csharp

namespace WebApiService
{
    using Owin;

    public interface IOwinAppBuilder
    {
        void Configuration(IAppBuilder appBuilder);
    }
}

```

Isso é tudo para a parte do aplicativo. Neste ponto, apenas configuramos o layout básico do projeto de API Web. Comparado aos projetos de API Web que você possa ter desenvolvido no passado ou ao modelo de API Web básico, esse processo não parece muito diferente até agora. A lógica de negócios vai para os controladores e modelos como de costume.

Agora, o que fazemos a respeito da hospedagem para que seja possível realmente executá-lo?


## Serviço de hospedagem

Na Malha de Serviço, o serviço é executado em um *processo de host do serviço* - um executável que executa seu código de serviço. Ao desenvolver um serviço usando o API de Serviços Confiáveis e, na maioria dos casos, quando você estiver desenvolvendo um serviço na malha de serviço no .NET, seu projeto de serviço apenas compila para um .EXE que registra o tipo de serviço e executa o seu código. Na verdade, se você abrir **Program.cs** no projeto de serviço sem estado, deve ver:

```csharp

public class Program
{
    public static void Main(string[] args)
    {
        try
        {
            using (FabricRuntime fabricRuntime = FabricRuntime.Create())
            {
                fabricRuntime.RegisterServiceType("WebApiServiceType", typeof(Service));

                Thread.Sleep(Timeout.Infinite);
            }
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e);
            throw;
        }
    }
}

```

Se parecer anormalmente o ponto de entrada para um aplicativo de console, é porque é.

Os detalhes sobre o processo de hospedagem do serviço e o registro do serviço estão além do escopo deste artigo, mas é importante saber por enquanto que **seu código de serviço está sendo executado em seu próprio processo**.

## API Web auto-hospedagem com um host OWIN

Considerando que o código do aplicativo API Web esteja hospedado em seu próprio processo, como podemos vinculá-lo a um servidor web? Digite [OWIN](http://owin.org/). OWIN é simplesmente um contrato entre aplicativos web do .NET e servidores web. Tradicionalmente com ASP.NET - até o MVC 5 - o aplicativo web era bem acoplado ao IIS por meio de System.Web. No entanto, o API Web implementa o OWIN, que permite o desenvolvimento de um aplicativo web que é dissociado do servidor web que o hospeda. Isso permite que você use um servidor web OWIN com *auto-hospedagem* que pode ser iniciado em seu próprio processo, que se ajusta perfeitamente ao modelo de hospedagem de Malha de Serviço que acabamos de descrever.

Neste artigo, vamos usar Katana como host OWIN para o aplicativo API Web. Katana é uma implementação de host OWIN de código aberto.

> [AZURE.NOTE]Para saber mais sobre Katana, vá até o [site Katana](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana), e para uma visão geral de como usar Katana para a auto-hospedagem da API Web, leia este artigo sobre como [Usar OWIN para a auto-hospedagem do API Web ASP.NET 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).


## Configuração do servidor Web

A API do Reliable Services fornece um ponto de entrada de comunicação no qual você pode conectar pilhas de comunicação para permitir que os usuários e clientes se conectem ao serviço:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

O servidor web, e qualquer pilha de comunicação que você possa usar no futuro, como WebSockets, deve usar a interface ICommunicationListener para integração correta com o sistema. As razões para isso ficarão mais evidentes nas etapas a seguir.

Primeiro, crie uma classe chamada OwinCommunicationListener que implementa ICommunicationListener:

 + OwinCommunicationListener.cs:

```csharp

namespace WebApi
{
    using System;
    using System.Fabric;
    using System.Fabric.Description;
    using System.Globalization;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Owin.Hosting;
    using Microsoft.ServiceFabric.Services.Communication.Runtime;

    public class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}

```

A interface ICommunicationListener fornece três métodos para gerenciar um ouvinte de comunicação para seu serviço:

 + **OpenAsync**: começar a ouvir solicitações.
 + **CloseAsync**: parar de ouvir solicitações, concluir todas as solicitações em andamento e desligar normalmente.
 + **Abort**: cancelar tudo e parar imediatamente.

Para começar, adicionar membros de classe privada a um prefixo de caminho de URL e a classe **Startup** criada anteriormente. Elas serão inicializados por meio do construtor e usadas posteriormente ao configurar a URL de escuta. Adicione também membros de classe privada para salvar o endereço de escuta e o indicador do servidor que são criados durante a inicialização e mais tarde, quando o servidor é iniciado, respectivamente.

```csharp

public class OwinCommunicationListener : ICommunicationListener
{
    private readonly IOwinAppBuilder startup;
    private readonly string appRoot;
    private IDisposable serverHandle;
    private string listeningAddress;
    private readonly ServiceInitializationParameters serviceInitializationParameters;

    public OwinCommunicationListener(string appRoot, IOwinAppBuilder startup, ServiceInitializationParameters serviceInitializationParameters)
    {
        this.startup = startup;
        this.appRoot = appRoot;
        this.serviceInitializationParameters = serviceInitializationParameters;
    }        

    ...

```

### Implementação

Para configurar o servidor Web, precisamos de duas partes de informações:

 + **Um prefixo de caminho de URL**. Embora seja opcional, é bom configurar isso agora para que você possa hospedar vários serviços Web com segurança em seu aplicativo.
 + **Uma porta**.

Antes de capturamos uma porta para o servidor Web, é importante compreender que a Malha de Serviço fornece uma camada de aplicativo que age como um buffer entre seu aplicativo e o sistema operacional subjacente em que é executado. Dessa forma, a Malha de Serviços fornece uma maneira de configurar *pontos de extremidade* para seus serviços. A Malha de Serviço se encarrega de se certificar de que o ponto de extremidade esteja disponível para o seu serviço usar para que você não precise configurá-lo com o ambiente do sistema operacional subjacente. Isso lhe permite hospedar facilmente seu aplicativo de Malha de Serviço em diferentes ambientes sem precisar fazer nenhuma alteração ao seu aplicativo (por exemplo, você pode hospedar o mesmo aplicativo no Azure ou em seu próprio data center).

Configure um ponto de extremidade de HTTP em PackageRoot\\ServiceManifest.xml:

```xml

<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="80" />
    </Endpoints>
</Resources>

```

Essa etapa é importante porque o processo de hospedagem de serviço é executado sob credenciais restritas (serviço de rede no Windows), o que significa que o serviço não terá acesso para configurar um ponto de extremidade HTTP por conta própria. Ao usar a configuração de ponto de extremidade, a Malha de Serviços sabe como definir a ACL apropriada para a URL que o serviço escutará enquanto fornece um local padrão para configurar pontos de extremidade.


Em OwinCommunicationListener.cs, podemos começar a implementar o OpenAsync. É onde podemos iniciar o servidor Web. Primeiro, obtenha as informações do ponto de extremidade e crie a URL que o serviço escutará.

```csharp

public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceInitializationParameters.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = String.Format(
        CultureInfo.InvariantCulture,
        "http://+:{0}/{1}",
        port,
        String.IsNullOrWhiteSpace(this.appRoot)
            ? String.Empty
            : this.appRoot.TrimEnd('/') + '/');
    ...

```

Observe que "http://+" é usado aqui. Isso é para garantir que o servidor Web esteja escutando em todos os endereços disponíveis, incluindo localhost, FQDN e o IP da máquina.

Implementar o OpenAsync é uma das razões mais importantes pelas quais o servidor Web (ou qualquer pilha de comunicação) seja implementada como um ICommunicationListener em vez de apenas abri-lo diretamente do RunAsync() no serviço. O valor de retorno de OpenAsync é o endereço que o servidor Web está escutando. Quando esse endereço é retornado ao sistema, ele registra o endereço com o serviço. A Malha de Serviço fornece um API que permite aos clientes ou outros serviços pedir esse endereço pelo nome do serviço. Isso é importante porque o endereço do serviço não é estático, já que os serviços são movidos no cluster para fins de disponibilidade e balanceamento de recursos. Esse é o mecanismo que permite aos clientes resolver o endereço de escuta de um serviço.

Com isso em mente, OpenAsync inicia o servidor Web e retorna o endereço em que está escutando. Observe que ele escuta em "http://+", mas antes de retornar o endereço, o "+" é substituído pelo IP ou o FQDN do nó em que ele está no momento. A razão disso é que esse endereço que está sendo retornado pelo método é o que está registrado com o sistema e é o que os clientes e outros serviços verão quando pedirem o endereço do serviço. Para que os clientes se conectem corretamente a ele, eles precisam de um IP ou FQDN real no endereço.

```csharp

    ...

    this.serverHandle = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Configuration(appBuilder));
    string publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    ServiceEventSource.Current.Message("Listening on {0}", publishAddress);

    return Task.FromResult(publishAddress);
}

```

Observe que isso faz referência à classe **Startup** que foi passado para OwinCommunicationListener no construtor. Esta instância de Startup é usada pelo servidor Web para inicializar o aplicativo API Web.

A linha ServiceEventSource.Current.Message() aparecerá na janela de eventos de diagnóstico mais tarde, quando você executar o aplicativo que informará se o servidor Web foi iniciado com êxito.

### CloseAsync e Abort

Finalmente, implemente o CloseAsync e o Abort para interromper o servidor Web. O servidor Web pode ser interrompido eliminando-se o indicador de servidor que foi criado durante o OpenAsync.

```csharp

public Task CloseAsync(CancellationToken cancellationToken)
{
    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.serverHandle != null)
    {
        try
        {
            this.serverHandle.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}

```

Nesse exemplo de implementação, o CloseAsync e o Abort simplesmente interrompem o servidor Web. Você pode optar por executar um desligamento coordenado mais normalmente do servidor Web em CloseAsync; por exemplo, aguardando que solicitações em andamento sejam concluídas antes de retornar.

## Inicie o servidor Web.

Agora você está pronto para criar e retornar uma instância de OwinCommunicationListener para iniciar o servidor web. De volta para a classe de serviço (Service.cs), substitua o método **CreateServiceInstanceListeners()**:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new[]
    {
        new ServiceInstanceListener(initParams => new OwinCommunicationListener("webapp", new Startup(), initParams))
    };
}

```

É aí que o *aplicativo* da API Web e o *host* OWIN finalmente se encontram: o *host* (**OwinCommunicationListener**) recebe uma instância do *aplicativo* (API Web por meio de **Startup**), e o Service Fabric gerencia o ciclo de vida. Esse mesmo padrão normalmente pode ser seguido com qualquer pilha de comunicação.

## Juntando as peças

Neste exemplo, você não precisa fazer nada no método RunAsync(), de forma que a substituição pode ser simplesmente removida.

A implementação do serviço final deve ser muito simples, já que ele só precisa criar o ouvinte de comunicação:

```csharp

namespace WebApiService
{
    using System.Collections.Generic;
    using Microsoft.ServiceFabric.Services.Communication.Runtime;
    using Microsoft.ServiceFabric.Services.Runtime;

    public class WebApiService : StatelessService
    {
        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            return new[]
            {
                new ServiceInstanceListener(initParams => new OwinCommunicationListener("webapp", new Startup(), initParams))
            };
        }
    }
}

```

E complete a `OwinCommunicationListener` classe:

```csharp

namespace WebApiService
{
    using System;
    using System.Fabric;
    using System.Fabric.Description;
    using System.Globalization;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Owin.Hosting;
    using Microsoft.ServiceFabric.Services.Communication.Runtime;

    public class OwinCommunicationListener : ICommunicationListener
    {
        private readonly IOwinAppBuilder startup;
        private readonly string appRoot;
        private readonly ServiceInitializationParameters serviceInitializationParameters;
        private IDisposable serverHandle;
        private string listeningAddress;
        
        public OwinCommunicationListener(string appRoot, IOwinAppBuilder startup, ServiceInitializationParameters serviceInitializationParameters)
        {
            this.startup = startup;
            this.appRoot = appRoot;
            this.serviceInitializationParameters = serviceInitializationParameters;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            EndpointResourceDescription serviceEndpoint = serviceInitializationParameters.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
            int port = serviceEndpoint.Port;

            this.listeningAddress = String.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/{1}",
                port,
                String.IsNullOrWhiteSpace(this.appRoot)
                    ? String.Empty
                    : this.appRoot.TrimEnd('/') + '/');

            this.serverHandle = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Configuration(appBuilder));
            string publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            ServiceEventSource.Current.Message("Listening on {0}", publishAddress);

            return Task.FromResult(publishAddress);
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            ServiceEventSource.Current.Message("Close");

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            ServiceEventSource.Current.Message("Abort");

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.serverHandle != null)
            {
                try
                {
                    this.serverHandle.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}

```

Com todas as peças no lugar, seu projeto agora deve se parecer com um aplicativo típico de API Web com os pontos de entrada do API de Serviços Confiáveis e um host OWIN:


![](media/service-fabric-reliable-services-communication-webapi/webapi-projectstructure.png)

## Execução e conexão por meio de um navegador da Web

[Configure seu ambiente de desenvolvimento](service-fabric-get-started.md), caso ainda não tenha feito isso.


Agora você pode criar e implantar seu serviço. Pressione **F5** no Visual Studio para compilar e implantar o aplicativo. Na janela Eventos de Diagnóstico, você deverá ver uma mensagem indicando o servidor Web aberto em **http://localhost:80/webapp/api**


![](media/service-fabric-reliable-services-communication-webapi/webapi-diagnostics.png)

> [AZURE.NOTE]Se a porta já tiver sido aberta por outro processo em seu computador, talvez você receba um erro nesse ponto indicando que não foi possível abrir o ouvinte. Se esse for o caso, tente usar uma porta diferente na configuração do ponto de extremidade em ServiceManifest.xml.


Quando o serviço estiver em execução, abra um navegador e navegue para [http://localhost/webapp/api/values](http://localhost/webapp/api/values) para testá-lo.

## Escalar horizontalmente

Escalar horizontalmente aplicativos Web sem estado geralmente significa adicionar mais computadores e fazer um rodízio do aplicativo Web neles. O mecanismo de orquestração da Malha de Serviço pode fazer isso para você sempre que novos nós forem adicionados a um cluster. Ao criar instâncias de um serviço sem estado, você pode especificar o número de instâncias que deseja criar. A Malha de Serviço colocará esse número de instâncias em nós de cluster da mesma forma, certificando-se de não criar mais de uma instância em nenhum nó. Você também pode instruir a Malha de Serviço para sempre criar uma instância em cada nó, especificando "-1" para a contagem de instâncias. Isso garante que sempre que você adicionar nós para escalar horizontalmente o cluster, uma instância do seu serviço sem estado será criada em novos nós. Esse valor é uma propriedade da instância do serviço, para que ela seja definida ao criar uma instância de serviço por meio do PowerShell:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

Ou, ao definir um serviço padrão em um projeto de serviço sem estado do Visual Studio:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Para obter mais informações sobre a criação de instâncias de aplicativos e de serviço, consulte [como implantar e remover aplicativos](service-fabric-deploy-remove-applications.md).

## ASP.NET 5

No ASP.NET 5, o conceito e o modelo de programação de separação do *aplicativo* do *host* em aplicativos Web permanecem os mesmos. Ele também pode ser aplicado a outras formas de comunicação. Além disso, embora o *host* possa ser diferente no ASP.NET 5, a camada de *aplicativo* do API Web permanece a mesma, que é onde reside de fato a maior parte da lógica do aplicativo.

## Próximas etapas

[Depurando seu aplicativo da Malha de Serviços no Visual Studio](service-fabric-debugging-your-application.md)

<!----HONumber=Nov15_HO4-->