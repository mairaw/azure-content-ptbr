<properties 
	pageTitle="Noções básicas sobre monitoramento de trabalho do Stream Analytics | Microsoft Azure" 
	description="Noções básicas sobre monitoramento de trabalho do Stream Analytics" 
	keywords="big data analytics,cloud service,internet of things,managed service,stream processing,streaming analytics,streaming data"
	services="stream-analytics" 
	documentationCenter="" 
	authors="jeffstokes72" 
	manager="paulettm" 
	editor="cgronlun"/>

<tags 
	ms.service="stream-analytics" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.workload="data-services" 
	ms.date="09/09/2015" 
	ms.author="jeffstok"/>

# Noções básicas sobre monitoramento de trabalho do Stream Analytics

## Página do Monitor

A página do Monitor contém as principais métricas de desempenho que podem ser usadas para monitorar e solucionar problemas de seu trabalho. Clique na guia do Monitor de um trabalho do Stream Analytics de execução para ver essas métricas. Há um atraso de no máximo 1 minuto nas métricas de desempenho exibidas na página do Monitor.

  ![Painel de monitoramento](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)

## Métricas disponíveis para o Stream Analytics

| Métrica | Definição |
|--------|-------------|
| % de utilização do SU | A utilização das unidades de Streaming é atribuída a um trabalho na guia Escala do trabalho. Se esse indicador alcançar 80% ou acima, há grande probabilidade de que o processamento de eventos se atrase ou pare. |
| Eventos de entrada | Quantidade de dados recebidos pelo trabalho do Stream Analytics, em termos de contagem de eventos. Isso pode ser usado para validar que os eventos estão sendo enviados para a fonte de entrada. |
| Bytes de evento de entrada | Quantidade de dados recebidos pelo trabalho do Stream Analytics, em termos de taxa de transferência em bytes |
| Eventos de saída | Quantidade de dados enviados pelo trabalho do Stream Analytics para o destino de saída, em termos de contagem de evento. |
| Eventos fora de ordem | Número de eventos recebidos fora de ordem que foram descartados ou que receberam um carimbo de data/hora ajustado, com base na Política de ordenação de evento. Isso pode ser afetado pela configuração da definição da Janela de tolerância fora de ordem. |
| Erros de conversão de dados | Número de erros de conversão de dados gerado por um trabalho do Stream Analytics. |
| Eventos de entrada atrasados | Número de eventos que chegam atrasados da fonte que podem ter sido descartados ou ter tido o carimbo de data/hora ajustado, com base na configuração de Política de ordenação de eventos da configuração da definição da Janela de tolerância de chegada atrasada. |
Até 6 métricas podem ser exibidas em um gráfico

Para alternar entre a exibição dos valores relativos (somente valor final para cada métrica) e os valores absolutos (eixo Y exibido), selecione Relativo ou Absoluto na parte superior do gráfico.

  ![Absoluto relativo](./media/stream-analytics-monitoring/02-stream-analytics-monitoring.png)

As métricas podem ser exibidas no gráfico do Monitor em agregações de 1 hora, 12 horas, 24 horas ou 7 dias.

Para alterar o intervalo de tempo de exibição do gráfico de métricas, selecione 1 hora, 24 horas ou 7 dias na parte superior do gráfico.

  ![Escala de tempo](./media/stream-analytics-monitoring/03-stream-analytics-monitoring.png)

Você pode definir regras que podem notificá-lo por e-mail caso o trabalho passe de um limite definido.

## Status do trabalho

O status dos trabalhos de Stream Analytics pode ser visualizado no portal do Azure onde é exibida uma lista dos trabalhos. Você pode ver a lista de trabalhos, clicando no ícone do Stream Analytics no Portal do Azure.

| Status | Definição |
|--------|------------|
| Criado | Um trabalho foi criado, porém não foi iniciado. |
| Iniciando | Um usuário clicou em Iniciar o trabalho e o trabalho está iniciando |
| Executando | O trabalho foi alocado, está processando dados ou aguardando para processar a entrada. Se um mostrar o estado Em execução sem produzir saída, é provável que a janela de tempo de processamento de dados seja grande ou que a lógica de consulta seja complicada. Outro motivo pode ser que atualmente não há qualquer dado sendo enviados para o trabalho. |
| Parando | Um usuário clicou em Iniciar o trabalho e o trabalho está sendo interrompido. |
| Parada | O trabalho foi interrompido. |
| Degradado | Este estado indica que um trabalho do Stream Analytics está encontrando erros transitórios (por ex. erros de entrada/saída, erros de processamento, erros de conversão, etc.). O trabalho ainda está em execução, no entanto, há muitos erros que estão sendo gerados. Esse trabalho precisa de atenção do cliente e o cliente pode ver os Logs de operações para os erros. |
| Falha | Isso indica que o trabalho falhou devido a erros e o processamento foi interrompido. O cliente precisa examinar os logs de operações para depurar os erros. |
| Excluindo | Isso indica que o trabalho está sendo excluído. |

## Diagnóstico

O painel de trabalho fornece informações sobre onde você precisa procurar o diagnóstico, ou seja, entradas, saídas e/ou log de operações. Você pode clicar no link para acessar o local apropriado para examinar o diagnóstico.

  ![Erro](./media/stream-analytics-monitoring/04-stream-analytics-monitoring.png)

Clicar no recurso de entrada ou saída fornece informações detalhadas de diagnóstico. A atualização é feita com as últimas informações de diagnóstico, enquanto o trabalho está em execução.

  ![Diagnostics](./media/stream-analytics-monitoring/05-stream-analytics-monitoring.png)

## Obter ajuda
Para obter mais assistência, experimente nosso [Fórum do Stream Analytics do Azure](https://social.msdn.microsoft.com/Forums/pt-BR/home?forum=AzureStreamAnalytics)

## Próximas etapas

- [Introdução ao Stream Analytics do Azure](stream-analytics-introduction.md)
- [Introdução ao uso do Stream Analytics do Azure](stream-analytics-get-started.md)
- [Dimensionar trabalhos do Stream Analytics do Azure](stream-analytics-scale-jobs.md)
- [Referência de Linguagem de Consulta do Stream Analytics do Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Referência da API REST do Gerenciamento do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!---HONumber=Sept15_HO2-->