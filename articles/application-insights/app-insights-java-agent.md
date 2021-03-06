<properties 
	pageTitle="Monitorar dependências, exceções e tempos de execução em aplicativos Web em Java" 
	description="Monitoramento estendido do seu site Java com o Application Insights" 
	services="application-insights" 
    documentationCenter="java"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="11/03/2015" 
	ms.author="awills"/>
 
# Monitorar dependências, exceções e tempos de execução em aplicativos Web em Java

*O Application Insights está em modo de visualização.*

Se você [instrumentou seu aplicativo Web em Java com o Application Insights][java], será possível usar o Agente Java para obter insights mais aprofundados, sem nenhuma alteração de código:


* **Dependências:** dados sobre chamadas de seu aplicativo a outros componentes, incluindo:
 * **Chamadas REST** feitas por meio de HttpClient, OkHttp e RestTemplate (Spring).
 * Chamadas **Redis** feitas por meio do cliente Jedis. Se a chamada levar mais de 10 segundos, o agente também buscará os argumentos da chamada.
 * **[Chamadas JDBC](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** – MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB ou Apache Derby DB. Há suporte para chamadas “executeBatch”. Para MySQL e PostgreSQL, se a chamada levar mais de 10 segundos, o agente relatará o plano de consulta. 
* **Exceções capturadas:** dados sobre exceções que são manipuladas pelo código.
* **Tempo de execução do método:** dados sobre o tempo necessário para executar métodos específicos.

Para usar o agente Java, instale-o no servidor. Seus aplicativos Web devem ser instrumentados com o [SDK do Java do Application Insights][java].

## Instalar o agente do Application Insights para Java

1. No computador que está executando o servidor Java, [baixe o agente](https://azuredownloads.blob.core.windows.net/applicationinsights/sdk.html).
2. Edite o script de inicialização do servidor de aplicativos e adicione a seguinte JVM:

    `javaagent:`*caminho completo para o arquivo JAR do agente*

    Por exemplo, no Tomcat em um computador Linux:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. Reinicie o servidor de aplicativos.

## Configurar o agente

Crie um arquivo chamado `AI-Agent.xml` e coloque-o na mesma pasta que o arquivo JAR do agente.

Defina o conteúdo do arquivo xml. Edite o exemplo a seguir para incluir ou omitir os recursos desejados.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments will be sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan will be reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String:I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

Você precisa habilitar a exceção de relatórios e o tempo de método para métodos individuais.

Por padrão, `reportExecutionTime` é true e `reportCaughtExceptions` é false.

## Exibir os dados

No recurso do Application Insights, a dependência remota e os tempos de execução do método agregados aparecerão [no bloco Desempenho][metrics].

Para procurar instâncias individuais de dependência, exceções e relatórios de método, abra [Pesquisar][diagnostic].

[Diagnosticando problemas de dependência – Saiba mais](app-insights-dependencies.md#diagnosis).



## Perguntas? Problemas?

[Solucionar problemas de Java](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 

<!---HONumber=Nov15_HO2-->