<properties
  pageTitle="Controle de versão de cliente e servidor em Aplicativos Móveis e Serviços Móveis | Serviço de Aplicativo do Azure"
  description="Lista dos SDKs clientes e compatibilidade com versões do SDK do servidor para os Serviços Móveis e Aplicativos Móveis do Azure"
  services="app-service\mobile"
  documentationCenter=""
  authors="lindydonna" 
  manager="dwrede"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="12/15/2015"
  ms.author="donnam"/>

# Controle de versão de cliente e servidor em Aplicativos Móveis e Serviços Móveis

A versão mais recente dos Serviços Móveis do Azure é o recurso **Aplicativos Móveis** do Serviço de Aplicativo do Azure.

<!-- Azure App Service offers a number of platform benefits over Mobile Services, including continuous integration and deployment, staging lots, and VNET support.
 -->

Os SDKs de cliente e servidor de Aplicativos Móveis são baseados originalmente nos Serviços Móveis, mas *não* são compatíveis entre si. Ou seja, você deve usar um SDK de cliente de *Aplicativos Móveis* com SDK de servidor de *Aplicativos Móveis* e da mesma forma para *Serviços Móveis*. Esse contrato é imposto por meio de um valor de cabeçalho especial usado pelos SDKs de cliente e servidor, `ZUMO-API-VERSION`.

Observação: sempre que este documento se refere a um back-end de *Serviços Móveis*, ele não necessariamente precisa estar hospedados nos Serviços Móveis. Agora é possível migrar um serviço móvel para ser executado em um Serviço de Aplicativo sem qualquer alteração de código, mas o serviço ainda estaria usando versões de SDK de *Serviços Móveis*.

Para saber mais sobre a migração para o Serviço de Aplicativo sem qualquer alteração de código, consulte o artigo [Migrar um Serviço Móvel para o Serviço de Aplicativo do Azure].

## Especificação de cabeçalho

A chave `ZUMO-API-VERSION` pode ser especificada no cabeçalho HTTP ou na cadeia de consulta. O valor é uma cadeia de caracteres de versão no formulário **x.y.z**.

Por exemplo:

GET https://service.azurewebsites.net/tables/TodoItem

CABEÇALHOS: ZUMO-API-VERSION: 2.0.0

POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## Recusando a verificação de versão

Você pode recusar a verificação de versão definindo um valor **verdadeiro** para a configuração do aplicativo **MS\_SkipVersionCheck**. Especifique isso no seu web.config ou na seção Configurações do Aplicativo do Portal do Azure.

> [AZURE.NOTE]Há algumas alterações de comportamento entre os Serviços Móveis e os Aplicativos Móveis, especialmente nas áreas de sincronização offline, autenticação e notificações por push. Você deve recusar verificação somente após o teste completo para garantir que essas alterações de comportamento não interrompam a funcionalidade do aplicativo.

## Resumo de compatibilidade para todas as versões

A tabela abaixo mostra a compatibilidade entre todos os tipos de cliente e de servidor. Um back-end é classificado como **Serviços** Móveis ou **Aplicativos** Móveis baseado no SDK de servidor que ele usa.

| | Node.js ou .NET de **Serviços Móveis** | Node.js ou .NET de **Aplicativos Móveis** |
| ----------                | -----------------------             |   ----------------              |
| [Clientes de Serviços Móveis] | OK | Erro* |
| [Clientes de Aplicativos Móveis] | Erro* | OK |

*Isso pode ser controlado especificando **MS\_SkipVersionCheck**.


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Servidor e cliente de Serviços Móveis

Os SDKs de cliente na tabela a seguir são compatíveis com **Serviços Móveis**.

Observação: as SDKs do cliente dos Serviços Móveis *não* enviam um valor de cabeçalho para `ZUMO-API-VERSION`. Se o serviço receber esse cabeçalho ou o valor de cadeia de consulta, um erro será retornado, a menos que você tenha recusado expressamente conforme descrito acima.

### <a name="MobileServicesClients"></a> SDKs de clientes de *Serviços* Móveis

| Plataforma cliente | Versão | Valor de cabeçalho de versão |
| -------------------               | ------------------------                                                  | -------------------  |
| Cliente gerenciado (Windows, Xamarin) | [1\.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | n/d |
| iOS | [2\.2.2](http://aka.ms/gc6fex) | n/d |
| Android | [2\.0.3](https://go.microsoft.com/fwLink/?LinkID=280126) | n/d |
| HTML | [1\.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | n/d |

### SDKs de servidor de *Serviços* Móveis

| Plataforma servidor | Versão | Cabeçalho de versão aceito |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET | [WindowsAzure.MobileServices.Backend.* versão 1.0. x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | **Nenhum cabeçalho de versão**|
| Node.js | (em breve) | **Nenhum cabeçalho de versão** |

<!-- TODO: add Node npm version -->

### Comportamento dos back-ends de Serviços Móveis

| ZUMO-API-VERSION | Valor de MS\_SkipVersionCheck | Resposta |
| ---------------- | ---------------------------- | -------- |
| Não especificado | Qualquer | 200 - OK |
| Qualquer valor | Verdadeiro | 200 - OK |
| Qualquer valor | Falso/não especificado | 400 - solicitação inválida | 

## <a name="2.0.0"></a>Servidor e cliente de Aplicativos Móveis do Azure

### <a name="MobileAppsClients"></a> SDKs de cliente de *Aplicativos* Móveis

A verificação de versão foi introduzida começando com as seguintes versões do SDK do cliente para **Aplicativos Móveis do Azure**:

| Plataforma cliente | Versão | Valor de cabeçalho de versão |
| -------------------               | ------------------------  | -----------------    |
| Cliente gerenciado (Windows, Xamarin) | [2\.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2\.0.0 |
| iOS | [3\.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2\.0.0 |
| Android | [3\.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3\.0.0 |

<!-- TODO: add HTML version when released -->

### SDKs de servidor dos *Aplicativos* Móveis

A verificação de versão está incluída nas seguintes versões do SDK do servidor:

| Plataforma servidor | . | Cabeçalho de versão aceito |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET | [Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2\.0.0 |
| Node.js | [azure-mobile-apps Versão 1.0-beta1 (ou posterior)](https://www.npmjs.com/package/azure-mobile-apps) | 2\.0.0 |

### Comportamento dos back-ends de Aplicativos Móveis

| ZUMO-API-VERSION | Valor de MS\_SkipVersionCheck | Resposta |
| ---------------- | ---------------------------- | -------- |
| x.y.z ou Null | Verdadeiro | 200 - OK |
| Nulo | Falso/não especificado | 400 - solicitação inválida |
| 1\.x.y | Falso/não especificado | 400 - solicitação inválida |
| 2\.0.0-2.x.y | Falso/não especificado | 200 - OK |
| 3\.0.0-3.x.y | Falso/não especificado | 400 - solicitação inválida |


## Próximas etapas

- [Migrar um Serviço Móvel para o Serviço de Aplicativo do Azure]


[Clientes de Serviços Móveis]: #MobileServicesClients
[Clientes de Aplicativos Móveis]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Migrar um Serviço Móvel para o Serviço de Aplicativo do Azure]: app-service-mobile-migrating-from-mobile-services.md


<!-----HONumber=AcomDC_1217_2015-->

