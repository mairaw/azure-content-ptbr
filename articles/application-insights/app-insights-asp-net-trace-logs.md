<properties 
	pageTitle="Explorar os logs de rastreamento do .NET no Application Insights" 
	description="Pesquise logs gerados com Trace, NLog ou Log4Net." 
	services="application-insights" 
    documentationCenter=".net"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="11/25/2015" 
	ms.author="awills"/>
 
# Explorar os logs de rastreamento do .NET no Application Insights  

Se usa NLog, log4Net ou System.Diagnostics.Trace para rastreamento de diagnóstico em seu aplicativo ASP.NET, você pode fazer com que os logs sejam enviados ao [Visual Studio Application Insights][start], em que você pode explorá-los e pesquisá-los. Os logs serão mesclados à outra telemetria proveniente de seu aplicativo para que você possa identificar os rastreamentos associados ao atendimento de cada solicitação de usuário e correlacioná-los com outros relatórios de eventos e exceções.

> [AZURE.NOTE]Você precisa do módulo de captura de log? É um adaptador útil para agentes de terceiros, mas se você ainda não usa o NLog, log4Net ou System.Diagnostics.Trace, convém chamar apenas o [TrackTrace() do Application Insights](app-insights-api-custom-events-metrics.md#track-trace) diretamente.

Se você ainda não [configurou o Application Insights para seu projeto][start], faça isso agora. Seu projeto deve ter o arquivo `ApplicationInsights.config` e o pacote do NuGet `Microsoft.ApplicationInsights.Web`.


##  Instalar um adaptador para sua estrutura de log

Se usa uma estrutura de log (log4Net, NLog ou System.Diagnostics.Trace), você pode instalar um adaptador que envia esses logs ao Application Insights juntamente com outra telemetria.

1. Se você planeja usar o log4Net ou NLog, instale-o em seu projeto. 
2. No Gerenciador de Soluções, clique com o botão direito do mouse no seu projeto e escolha **Gerenciar Pacotes NuGet**.
3. Pesquise “Application Insights”

    ![Obtenha a versão de pré-lançamento do adaptador correto](./media/app-insights-asp-net-trace-logs/appinsights-36nuget.png)

4. Selecione o pacote apropriado entre:
  + Microsoft.ApplicationInsights.TraceListener (para capturar chamadas do System.Diagnostics.Trace)
  + Microsoft.ApplicationInsights.NLogTarget
  + Microsoft.ApplicationInsights.Log4NetAppender

O pacote NuGet instala os assemblies necessários e também modifica o app.config ou web.config.

#### Inserir chamadas de log de diagnóstico

Se você usa System.Diagnostics.Trace, uma chamada típica é semelhante a:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Se você preferir log4net ou NLog:

    logger.Warn("Slow response - database01");


## Usando a API de rastreamento diretamente

Você pode chamar a API de rastreamento do Application Insights diretamente. Os adaptadores de log usam essa API.

Por exemplo:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

Uma vantagem de TrackTrace é que você pode colocar dados relativamente compridos na mensagem. Por exemplo, você pode codificar dados POST.


## Explorar seus logs

Na folha de visão geral do aplicativo no [portal do Application Insights][portal], escolha [Pesquisar][diagnostic].

![No Application Insights, escolha Pesquisar](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Pesquisa de Diagnóstico](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

Por exemplo, você pode:

* Filtrar rastreamentos de log ou itens com propriedades específicas
* Inspecionar um item específico em detalhes.
* Localizar outra telemetria relacionada à mesma solicitação de usuário (ou seja, com o mesmo OperationId) 
* Salvar a configuração dessa página como um favorito

> [AZURE.NOTE]**Amostragem.** Se o aplicativo enviar muitos dados e se você estiver usando o SDK do Application Insights para o ASP.NET versão 2.0.0-beta3 ou posterior, o recurso de amostragem adaptável poderá operar e enviar apenas uma porcentagem de sua telemetria. [Saiba mais sobre amostragem.](app-insights-sampling.md)

## Próximas etapas

[Diagnosticar falhas e exceções no ASP.NET][exceptions]

[Saber mais sobre a Pesquisa de Diagnóstico][diagnostic].



## Solucionar problemas

### <a name="emptykey"></a>Recebo um erro "Chave de instrumentação não pode ser vazio"

Parece que você instalou o pacote de Nuget de adaptador para registro em log sem instalar o Application Insights.

No Gerenciador de Soluções, clique com o botão direito do mouse em `ApplicationInsights.config` e escolha **Atualizar o Application Insights**. Você obterá uma caixa de diálogo que solicita que você entre no Azure e crie um recurso de Application Insights, ou então reutilize um recurso existente. Isso deve corrigir o erro.

### Posso ver rastreamentos na pesquisa de diagnóstico, mas não os outros eventos

Às vezes, pode levar algum tempo para que todos os eventos e solicitações percorram o pipeline.

### <a name="limits"></a>Que quantidade de dados é mantida?

Até 500 eventos por segundo de cada aplicativo. Os eventos são retidos por sete dias.

### Não estou vendo algumas das entradas de log que eu esperava

Se o aplicativo enviar muitos dados e se você estiver usando o SDK do Application Insights para o ASP.NET versão 2.0.0-beta3 ou posterior, o recurso de amostragem adaptável poderá operar e enviar apenas uma porcentagem de sua telemetria. [Saiba mais sobre amostragem.](app-insights-sampling.md)

## <a name="add"></a>Próximas etapas

* [Configurar testes de disponibilidade e capacidade de resposta][availability]
* [Solucionar problemas][qna]





<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-web-failures-exceptions.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md

 

<!---HONumber=AcomDC_1203_2015-->