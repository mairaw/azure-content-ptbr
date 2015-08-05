<properties
   pageTitle="Modelo de aplicativo Malha do Serviço"
   description="Como modelar um aplicativo na Malha do Serviço"
   services="service-fabric"
   documentationCenter=".net"
   authors="alexwun"
   manager="timlt"
   editor="mani-ramaswamy"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/23/2015"
   ms.author="alexwun"/>

# Modelar um aplicativo no Malha do Serviço

Este artigo fornece uma visão geral do modelo de aplicativo da Malha do Serviço e descreve como definir um aplicativo e um serviço por meio de arquivos de manifesto e ter os aplicativos empacotados e prontos para implantação.

## Entenda o modelo de aplicativo

Um aplicativo é uma coleção de serviços membros que executam determinadas funções. Um serviço executa uma função autônoma completa (que pode iniciar e ser executada independentemente de outros serviços) e é composto de código, configuração e dados. Para cada serviço, o código consiste em binários executáveis, a configuração consiste em configurações do serviço que podem ser carregadas no tempo de execução e os dados consistem em dados estáticos arbitrários a serem consumidos pelo serviço. Cada componente nesse modelo hierárquico de aplicativo pode ser atualizado e transformado em outra versão independentemente.

![][1]


Um tipo de aplicativo é uma categorização de um aplicativo, que consiste em um conjunto de tipos de serviço. Um tipo de serviço é uma categorização de um serviço, que pode ter diferentes definições e configurações, mas a funcionalidade básica permanece a mesma. As instâncias de um serviço são as diferentes variações de configuração de serviço de um mesmo tipo de serviço.

Classes (ou "tipos") de aplicativos e de serviços são descritos usando arquivos XML (manifestos de aplicativo e manifestos do serviço), que são os modelos em relação ao qual os aplicativos podem ser instanciados. O código para instâncias do aplicativo diferentes será executado como processos separados, mesmo quando hospedadas pelo mesmo nó da Malha do Serviço. Além disso, o ciclo de vida de cada instância do aplicativo pode ser gerenciado (ou seja, atualizado) independentemente. O diagrama a seguir mostra como os tipos de aplicativo são compostos de tipos de serviço, que, por sua vez, são compostos de código, configuração e pacotes.

![TiposdeServiço e TiposdeAplicativo da Malha do Serviço][Image1]

Dois arquivos de manifesto diferentes são usados para descrever aplicativos e serviços: o manifesto do serviço e o manifesto do aplicativo, que é abordado em detalhes nas seções a seguir.

Pode haver uma ou mais instâncias de um tipo de serviço ativo no cluster. Por exemplo, instâncias de serviço com monitoração de estado, ou réplicas, alcançam alta confiabilidade replicando o estado entre as réplicas localizadas em diferentes nós do cluster (que essencialmente fornecem redundância para que o serviço esteja disponível mesmo se um nó em um cluster falhar). Um [serviço particionado](service-fabric-concepts-partitioning.md) divide ainda mais seu estado (e padrões de acesso a esse estado) pelos nós do cluster.

O diagrama a seguir mostra a relação entre aplicativos e instâncias de serviço, partições e réplicas.

![Partições e réplicas dentro de um Serviço][Image2]


## Descrever um serviço

O manifesto do serviço define declarativamente o tipo de serviço, a versão e especifica os metadados de serviço, como o tipo de serviço, as propriedades de integridade, as medidas e os arquivos de configuração de balanceamento de carga e binários de serviço. Trocando em miúdos, ele descreve os pacotes de código, de configuração e de dados que compõem um pacote de serviço para dar suporte a um ou mais tipos de serviço. Aqui está um manifesto do serviço de exemplo simples:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
~~~

Os atributos **Version** são cadeias de caracteres não estruturadas e não analisadas pelo sistema. Eles são usados para fazer a versão de cada componente para atualizações.

**ServiceTypes** declara que tipos de serviço são suportados pelos **CodePackages** nesse manifesto. Quando um serviço é instanciado em relação a um desses tipos de serviço, todos os pacotes de código declarados nesse manifesto são ativados com a execução de seus pontos de entrada. Os processos resultantes devem registrar os tipos de serviço com suporte no tempo de execução. Observe que os tipos de serviço são declarados no nível do manifesto e não no nível do pacote de código. Assim, quando há vários pacotes de código, eles são todos ativados sempre que o sistema procurar por qualquer um dos tipos de serviço declarados.

**SetupEntryPoint** é um ponto de entrada privilegiado que é executado com as mesmas credenciais da Malha do Serviço (normalmente, a conta *LocalSystem*) antes de qualquer outro ponto de entrada. O executável especificado pelo **EntryPoint** normalmente é o host de serviço de longa execução; portanto, ter um ponto de entrada de instalação separado evita a necessidade de executar o host de serviço com altos privilégios por longos períodos de tempo. O executável especificado pelo **EntryPoint** é executado depois que o **SetupEntryPoint** é encerrado com êxito. O processo resultante é monitorado e reiniciado (começando novamente com **SetupEntryPoint**) se ele terminar ou falhar.

**DataPackage** declara uma pasta nomeada pelo atributo **Name** que contém dados estáticos arbitrários a serem consumidos pelo processo no tempo de execução.

**ConfigPackage** declara uma pasta nomeada pelo atributo **Name** que contém um arquivo *Settings*. Esse arquivo contém seções de configurações de par chave-valor, definido pelo usuário, que o processo pode ler de volta no tempo de execução. Durante a atualização, se apenas a **version** **ConfigPackage** foi alterada, o processo em execução não é reiniciado. Em vez disso, um retorno de chamada notifica o processo de que as definições de configuração foram alteradas para que possam ser recarregadas dinamicamente. Aqui está um exemplo do arquivo *Settings.xml*:

~~~
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSecion">
    <Parameter Name="MySettingA" Value="Foo" />
    <Parameter Name="MySettingB" Value="Bar" />
  </Section>
</Settings>
~~~

> [AZURE.NOTE]Um manifesto do serviço pode conter vários pacotes de código, de configuração e de dados e cada um deles pode ser transformado em versão independentemente.

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## Descrever um aplicativo


O manifesto do aplicativo descreve declarativamente o tipo de aplicativo e a versão e especifica os metadados de composição de serviço, como nomes estáveis, esquema de particionamento, fator de replicação/contagem de instância, política de segurança/isolamento, restrições de posicionamento, substituições de configuração e tipos de serviço membro. Também são descritos os domínios de balanceamento de carga no qual o aplicativo é colocado. Portanto, um manifesto de aplicativo descreve os elementos no nível do aplicativo e faz referência a um ou mais manifestos do serviço para compor um tipo de aplicativo. Aqui está um manifesto de aplicativo de exemplo simples:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
~~~

Manifestos de serviço, como atributos **Version**, são cadeias de caracteres não estruturadas e não analisadas pelo sistema. Eles também são usados para a fazer a versão de cada componente para atualizações.

**ServiceManifestImport** contém referências a manifestos de serviço que compõem esse tipo de aplicativo. Os manifestos de serviço importados determinam quais tipos de serviço são válidos nesse tipo de aplicativo.

**DefaultServices** declara as instâncias de serviço que são criadas automaticamente sempre que um aplicativo é instanciado em relação a esse tipo de aplicativo. Serviços padrão são apenas uma conveniência e se comportam como serviços normais em todos os aspectos após terem sido criados. Eles são atualizados com qualquer outro serviço na instância do aplicativo e também podem ser removidos.

> [AZURE.NOTE]Um manifesto de aplicativo pode conter várias importações de manifesto do serviço e serviços padrão. Cada importação de manifesto do serviço pode ser transformada em versão independentemente.

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## Preparar um aplicativo

### Layout do pacote

O manifesto do aplicativo, os manifestos do serviço e outros arquivos de pacote necessários devem ser organizados em um layout específico para a implantação em um cluster Malha do Serviço. Os manifestos de exemplo neste artigo precisariam estar organizados na seguinte estrutura de diretórios:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
~~~

As pastas são nomeadas para corresponder a atributos **Name** de cada elemento correspondente. Por exemplo, se o manifesto do serviço contiver dois pacotes de código com nomes **MyCodeA** e **MyCodeB** seria necessário haver duas pastas com os mesmos nomes que contêm os binários necessários para cada pacote de código.

### Criar um pacote usando o Visual Studio

Se você usar o Visual Studio 2015 para criar o seu aplicativo, pode usar o comando Package para criar automaticamente um pacote que corresponda ao layout descrito acima.

Para criar um pacote, clique com o botão direito do mouse no projeto de aplicativo no Gerenciador de Soluções e escolha o comando de pacote, conforme mostrado abaixo:

![][2]

Quando o empacotamento estiver concluído, você encontrará o local do pacote na janela Saída. Observe que a etapa de empacotamento ocorre automaticamente quando você implanta ou depura seu aplicativo no Visual Studio.

### Testando o pacote

A estrutura do pacote pode ser verificada através do PowerShell localmente usando o comando **Test-ServiceFabricApplicationPackage**, que irá verificar se há problemas de análise de manifesto e checar todas as referências. Observe que esse comando só verifica a correção estrutural de diretórios e arquivos no pacote; ele não irá verificar qualquer conteúdo do pacote de códigos ou de dados para além da presença dos arquivos necessários:

~~~
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
~~~

Esse erro mostra que o arquivo *MySetup.bat* referenciado no manifesto do serviço **SetupEntryPoint** está ausente do pacote de código. Depois de adicionar o arquivo que está faltando, a verificação do aplicativo é aprovada:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
~~~

Depois que o aplicativo é empacotado corretamente e passa pela verificação, ele está pronto para implantação.

## Próximas etapas

[Implantar e remover aplicativos][10]

<!--Image references-->
[1]: ./media/service-fabric-application-model/application-model.jpg
[2]: ./media/service-fabric-application-model/vs-package-command.png
[Image1]: media/service-fabric-application-model/Service1.jpg
[Image2]: media/service-fabric-application-model/Service2.jpg

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md

 

<!---HONumber=July15_HO4-->