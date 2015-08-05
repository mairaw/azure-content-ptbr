<properties
   pageTitle="Desenvolver um serviço da Malha do Serviço"
   description="Informações conceituais e tutoriais que ajudarão você a entender como desenvolver um serviço da Malha do Serviço usando os modelos de programação Ator Confiável ou Serviços Confiáveis."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/14/2015"
   ms.author="ryanwi"/>

# Desenvolver um serviço da Malha do Serviço
Esta página contém links para artigos conceituais e de visão geral, bem como para tutoriais que ajudam você a aprender a desenvolver um serviço da Malha do Serviço. A Malha do Serviço oferece dois modelos de programação de nível alto para criação de serviços: as APIs de ator confiável e as APIs de serviços confiáveis. Embora ambos se baseiem no mesmo núcleo da Malha do Serviço, eles apresentam diferentes compensações entre simplicidade e flexibilidade em termos de simultaneidade, particionamento e comunicação. É útil entender ambos os modelos para que você possa escolher a estrutura apropriada para um determinado serviço no seu aplicativo.

- [Escolher um modelo de programação](service-fabric-choose-framework.md)
- [Introdução ao modelo de Ator da Malha do Serviço](service-fabric-reliable-actors-introduction.md)
- [Introdução do modelo de programação de Serviço Confiável](../Service-Fabric/service-fabric-reliable-services-introduction.md)

## Modelo de programação de Ator Confiável
 Os Atores Confiáveis fornecem um modelo de ator assíncrono single-threaded. Os atores representam a unidade de estado e a computação que são distribuídas em todo o cluster para alcançar alta escalabilidade. O modelo de Ator Confiável aproveita o repositório distribuído fornecido pela plataforma subjacente da Malha do Serviço para fornecer gerenciamento de estado altamente disponível e consistente aos desenvolvedores de aplicativo. Para saber mais, leia:

- [Introdução aos Atores Confiáveis](service-fabric-reliable-actors-get-started.md)
- [Ciclo de vida do ator e coleta de lixo](service-fabric-reliable-actors-lifecycle.md)
- [Como os Atores da Malha usam a plataforma da Malha do Serviço](service-fabric-reliable-actors-platform.md)
- [Observações sobre a serialização do tipo de Atores da Malha do Serviço do Azure](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)

A comunicação com Atores é descrita em:

- [Introdução ao modelo de Ator da Malha do Serviço](service-fabric-reliable-actors-introduction.md#actor-communication).
- [Comunicando-se com serviços](service-fabric-connect-and-communicate-with-services.md)

Estes artigos abordam padrões e cenários úteis de design:

- [Padrões de design do modelo de Ator](service-fabric-reliable-actors-patterns-introduction.md)  
- [Padrão: cache inteligente](service-fabric-reliable-actors-pattern-smart-cache.md)
- [Padrão: redes e gráficos distribuídos](service-fabric-reliable-actors-pattern-distributed-networks-and-graphs.md)
- [Padrão: controle de recursos](service-fabric-reliable-actors-pattern-resource-governance.md)
- [Padrão: composição de serviço com estado](service-fabric-reliable-actors-pattern-stateful-service-composition.md)
- [Padrão: Internet das Coisas](service-fabric-reliable-actors-pattern-internet-of-things.md)
- [Padrão: computação distribuída](service-fabric-reliable-actors-pattern-distributed-computation.md)
- [Alguns antipadrões](service-fabric-reliable-actors-anti-patterns.md)

Uma simultaneidade simples baseada em turno é fornecida para métodos de Ator Confiável. Simultaneidade, temporizadores e lembretes, e reentrância são descritos nestes artigos:

- [Simultaneidade](service-fabric-reliable-actors-introduction.md#concurrency)
- [Eventos e contadores de desempenho relacionados à simultaneidade](service-fabric-reliable-actors-diagnostics.md)
- [Reentrância de Ator](service-fabric-reliable-actors-reentrancy.md)
- [Temporizadores de ator](service-fabric-reliable-actors-timers-reminders.md)
 
Informações sobre como configurar Atores Confiáveis são encontradas aqui:

- [Configuração de KVSActorStateProvider](../Service-Fabric/service-fabric-reliable-actors-KVSActorstateprovider-configuration.md)  
- [Configurando Atores Confiáveis — ReliableDictionaryActorStateProvider](../service-fabric-reliable-actors-reliabledictionarystateprovider-configuration.md)

Os Atores Confiáveis emitem eventos e contadores de desempenho, que podem ser usados para diagnosticar e monitorar seu serviço:

- [Diagnóstico de Ator](service-fabric-reliable-actors-diagnostics.md)
- [Eventos de Ator](service-fabric-reliable-actors-events.md)


## Modelo de programação de Serviço Confiável
Os Serviços Confiáveis fornecem um modelo de programação simples, potente e de nível superior que ajuda você a demonstrar o que é importante para seu aplicativo. Para saber mais, leia:

- [Introdução aos Serviços Confiáveis](service-fabric-reliable-services-quick-start.md)
- [Visão geral do modelo de programação](../service-fabric-reliable-services-service-overview.md)  
- [Arquitetura](service-fabric-reliable-services-platform-architecture.md)
- [Coleções Confiáveis](service-fabric-reliable-services-reliable-collections.md)
- [Configurando Serviços Confiáveis com estado](../Service-Fabric/service-fabric-reliable-services-configuration.md)
- [Uso avançado do modelo de programação de Serviços Confiáveis](../Service-Fabric/service-fabric-reliable-services-advanced-usage.md)

A comunicação com Serviços Confiáveis e as abstrações que os clientes podem usar para descobrir e se comunicar com os pontos de extremidade de serviço são descritas nos artigos a seguir:

- [Comunicando-se com serviços](service-fabric-connect-and-communicate-with-services.md)
- [Modelo de comunicação de serviço](service-fabric-reliable-services-communication.md)
- [Pilha de comunicação padrão fornecida pela Estrutura de Serviços Confiáveis](service-fabric-reliable-services-communication-default.md)
- [Pilha de comunicação baseada no WCF para Serviços Confiáveis](service-fabric-reliable-services-communication-wcf.md)
- [Introdução à API da Web da Malha do Serviço do Microsoft Azure com host automático OWIN (VS 2015 RC)](service-fabric-reliable-services-communication-webapi.md)

Os Serviços Confiáveis emitem eventos e contadores de desempenho, que podem ser usados para diagnosticar e monitorar seu serviço:

- [Diagnóstico de Serviços Confiáveis com estado](service-fabric-reliable-services-diagnostics.md)
 

<!---HONumber=July15_HO4-->