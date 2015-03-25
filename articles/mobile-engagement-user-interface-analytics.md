﻿<properties 
   pageTitle="Interface do usuário do Mobile Engagement do Azure - Análise" 
   description="Visão geral da Interface de usuário para o Mobile Engagement do Azure" 
   services="mobile-engagement" 
   documentationCenter="mobile" 
   authors="v-micada" 
   manager="mattgre" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="mobile"
   ms.workload="required" 
   ms.date="02/17/2015"
   ms.author="v-micada"/>

# Mobile Engagement do Azure - Interface do usuário

<div class="dev-center-tutorial-selector sublanding">
<a href="../mobile-engagement-user-interface" title="Introduction">Introdução</a>
<a href="../mobile-engagement-user-interface-navigation" title="Navigation">Navegação</a>
<a href="../mobile-engagement-user-interface-home/" title="Home">Página inicial</a>
<a href="../mobile-engagement-user-interface-my-account" title="My Account">Minha Conta</a>
<a href="../mobile-engagement-user-interface-analytics" title="Analytics">Análise</a>
<a href="../mobile-engagement-user-interface-monitor" title="Monitor">Monitoramento</a>
<a href="../mobile-engagement-user-interface-reach" title="Reach">Reach</a>
<a href="../mobile-engagement-user-interface-segments" title="Segments">Segmentos</a>
<a href="../mobile-engagement-user-interface-dashboard" title="Dashboard">Painel</a>
<a href="../mobile-engagement-user-interface-settings" title="Settings">Configurações</a>
</div>

# Análise
 
A seção de análise da interface do usuário fornece informações agregadas sobre seu aplicativo com base em dados históricos que são atualizados a cada 24 horas. As informações são exibidas em diferentes painéis compostos de mapas, grades e gráficos de barra/linha/pizza. Os dados também podem ser baixados como arquivos .csv. A maioria das mesmas informações está disponível em tempo real na seção de Monitoramento da interface do usuário e também pode ser acessada da API de análise. O "Glossário" em "Conceitos" possui definições de termos e abreviações na análise e monitoramento, como os seguintes: Usuário ativo, novo usuário, usuário retidos, sessão, gráficos de caminho do usuário, Mapa de usuários, Rastreamento de URLs, tendências, atividade, eventos, trabalho, erro, informações adicionais, falha e informações do aplicativo.

**Consulte também:** 

-  [Conceitos - Glossário][Link 6], [Guia solução de problemas - Análise][Link 2]

## Análise padrão e personalizada:
O Mobile Engagement do Azure fornece um conjunto de informações analíticas padrão básicas sobre seus aplicativos que podem ser representadas graficamente assim como integrar seu aplicativo com o SDK. O Mobile Engagement do Azure também fornece a capacidade de reunir informações de análises personalizadas adicionais que você deseja sobre o comportamento de seus usuários finais. Você pode fazer isso criando um plano de marca de "marcas de informações de aplicativo" personalizado, criado a partir de "Configurações" para que o Mobile Engagement do Azure possa coletar esses dados adicionais para você.

**Consulte também:** 
-  [Documentação de interface do usuário - Configurações][Link 20]
 
## Cabeçalho de análise:
- Nome do item: Rótulos do item sendo contado
- Exibir a Ajuda: Exibe informações contextuais sobre esta seção
- Versões: Permite que você mostre informações de análise diferentes sobre cada versão do seu aplicativo ou para exibir informações para todas as versões. (Observação: Filtrar seus dados de análise na interface do usuário mostrará todos os exemplos desse tipo, independentemente da versão do seu aplicativo. Por exemplo, "Falhas" filtradas por nome mostrará a partir da versão 1 e 2 do seu aplicativo.)
- Período de tempo: Últimos 7 dias, últimos 30 dias, o tempo todo, personalizado
- Avaliação: Por hora, dia, semana, mês
- Exibição: Gráfico de linha, grade, Envio para o painel, Baixar arquivo .csv
 
![Analytics1][10] 

## Análise:
- Painel: Mostra informações gerais sobre os usuários novos e ativos e suas tendências.
- Usuários: Os usuários são identificados por seu identificador de dispositivo: esse identificador é exclusivo para cada dispositivo (um novo usuário é, na verdade, um novo dispositivo). Um usuário é considerado como novo em um intervalo de tempo se ele realizou sua primeira sessão durante este intervalo de tempo. Um usuário é considerado como retido se ele tiver executado pelo menos uma sessão durante os últimos 7 dias. Usuários ativos são usuários que fizeram pelo menos uma sessão durante um determinado período. Você pode classificar por mensal, semanal, diária ou períodos de tempo por hora. Todos os gráficos parecem semelhantes, mas você pode filtrar por diferentes recursos, como a versão do seu aplicativo e, em seguida, classificar por um período de tempo. As informações padrão coletadas pela integração ao SDK incluem o seguinte: Usuários ativos, Novo usuário, número de sessões, duração de cada sessão, informações técnicas sobre o país, locais, localização, idioma da operadora, dispositivos, firmware, rede (WIFI), versões do aplicativo e do SDK usados pelos clientes. Essas informações podem ser exibidas em tempo real da seção de monitoramento. 
- Controle de código-fonte exibe o número de novos usuários que baixaram o aplicativo como resultado de uma determinada campanha de promoção. Os usuários são identificados por seu identificador de dispositivo: esse identificador é exclusivo para cada dispositivo (um novo usuário é, na verdade, um novo dispositivo). Um usuário é considerado como novo em um intervalo de tempo se ele realizou sua primeira sessão durante este intervalo de tempo.
- Controle por loja exibe o número de novos usuários que baixaram o aplicativo de uma determinada loja. Os usuários são identificados por seu identificador de dispositivo: esse identificador é exclusivo para cada dispositivo (um novo usuário é, na verdade, um novo dispositivo). Um usuário é considerado como novo em um intervalo de tempo se ele realizou sua primeira sessão durante este intervalo de tempo.

> Observação: O período de tempo se baseia na data das configurações do dispositivo dos usuários, dessa forma, um usuário cujo telefone tem a data definida incorretamente pode ser exibido no período de tempo incorreto.

- Retenção: Um usuário é considerado como retido em um determinado intervalo de tempo se ele realizou sua primeira sessão durante este intervalo de tempo. Você pode alterar os intervalos de tempo durante o qual os usuários retidos (e novos usuários) são contados para horas, dias, semanas ou meses. A análise de retenção de usuário é criada sobre cohorts. Um cohort é o conjunto de todos os novos usuários detectado em um determinado período (por exemplo, o conjunto de usuários que executarão a primeira sessão durante esse período). Usamos cohorts de 1 dia, 2 dias, 4 dias, 7 dias ou 1 mês. Dado um cohort, a cada 1 dia, 2, 4 ou 7 dias, ou 1 mês, o Mobile Engagement do Azure calcula o conjunto de todos os usuários que pertencem ao cohort e ainda estão ativos (ou seja, o conjunto de usuários que executaram pelo menos uma sessão durante o período). Este conjunto de usuários é chamado uma versão cohort. (O Mobile Engagement do Azure pode mostrar quantos de seus usuários ainda estão usando seu aplicativo, mas somente a loja específica da plataforma pode informar quantos de seus usuários desinstalaram seu aplicativo - por exemplo, GooglePlay, iTunes, Windows Store, etc.). 
- Sessões: Um uso do aplicativo por um usuário. As sessões são geradas a partir da sequência de atividades executadas pelos usuários (uma atividade geralmente está associada ao uso de uma tela do aplicativo, mas isso pode variar dependendo do modo como o SDK foi integrado no aplicativo). Um usuário pode executar apenas uma atividade por vez: uma sessão é iniciada assim que o usuário começa a sua primeira atividade e é interrompida quando ele termina sua última atividade. Se um usuário permanecer mais de alguns segundos sem executar qualquer atividade, sua sequência de atividades é dividida em duas sessões distintas.
- Atividades: Os nomes de cada tela em seu aplicativo e quanto tempo os usuários gastam em cada tela. As atividades são uma opção analítica personalizada que corresponderão às marcas de "informações do aplicativo" que você configurar para o seu próprio aplicativo:
- Caminho do usuário:  Mostra como os usuários navegam por meio de atividades do aplicativo (telas). Você pode mover o controle deslizante para ajustar o nível de detalhes. Nós azuis representam as atividades do seu aplicativo. Seu tamanho é proporcional ao tempo gasto pelos usuários nele. Nós brancos representam o início e o final da sessão. Nós vermelhos representam falhas. Os links representam as transições entre as atividades do aplicativo (ou entre atividades e falhas). Clique em um nó ou um link para exibir uma dica de ferramenta com mais informações sobre os dados: o tempo gasto em uma determinada tela, a contagem de transições e a porcentagem de transições de atividade de origem para a atividade de destino. (Um ---60%---> B significa que os usuários que estão em uma atividade A vão para a atividade B em 60% do tempo.) Você pode reorganizar o gráfico como desejar para esclarecê-lo. Sua posição é salva toda vez que você fizer uma alteração. Você pode mostrar ou ocultar as falhas para clarear o gráfico.
- Eventos: Ações específicas executadas por um usuário no aplicativo. A distribuição de eventos é mostrada como a contagem de eventos por usuário por sessão. Um evento representa uma ação instantânea, por exemplo, um clique em um botão ou a recepção de uma notificação. (O significado de eventos depende de como o SDK foi integrado no aplicativo.) Um evento pode ocorrer durante uma sessão ou um trabalho ou pode ser autônomo.
- Trabalhos: Semelhante aos eventos, exceto que eles se concentram na duração da ação. Por exemplo, trabalhos poderiam contar informações técnicas sobre o tempo de carregamento do conteúdo ou uma chamada ao serviço web. Ele também pode mostrar quanto tempo leva um usuário para preencher um formulário, criar uma conta ou fazer uma compra. Um trabalho representa a duração de uma tarefa, por exemplo, a duração de uma tarefa de download ou o tempo no qual um banner foi exibido na tela. (O significado dos trabalhos depende de como o SDK foi integrado no aplicativo.) Trabalhos estão geralmente associados a tarefas em segundo plano que são executadas fora do escopo de uma sessão (ou seja, sem nenhuma atividade de usuário).
- Técnicas: Informações técnicas sobre os dispositivos dos usuários do seu aplicativo que você pode rastrear, como a localidade, operadora, rede, dispositivo, Firmware e tamanho da tela dos dispositivos dos usuários e a versão do seu aplicativo e a versão do SDK usado em seu aplicativo.
- Erros: Informações sobre erros técnicos dentro do aplicativo que fazem com que o aplicativo falhe. Um erro representa um problema instantâneo, por exemplo, uma falha de rede ou uma manipulação incorreta. (O significado de eventos depende de como o SDK foi integrado no aplicativo.) Um erro pode ocorrer durante uma sessão ou um trabalho ou pode ser autônomo.
- Falhas: Informações sobre erros que causam a falha do aplicativo. Uma falha é uma condição inesperada onde o aplicativo para de executar suas funções esperadas e deve ser interrompido. Uma falha é geralmente devida a um erro no aplicativo.
 
![Analytics2][11] 

## Acessando a visão geral de retenção
 
![Analytics3][12] 

A visão geral de retenção é dividida no meio em vários cartões, cada uma mostrando a visão geral de um determinado período de retenção. O período de retenção de 2 dias é visto no exemplo. Os outros cartões mostram os períodos de retenção de 4 e 7 dias. 

## Noções básicas sobre os cartões de visão geral de retenção
 
![Analytics4][13] 

Cada cartão é composto de 3 partes principais:

1. 1: O cohort e o período considerado
2. 2 a 4: A retenção para o período atual
3. 5: Um minigráfico do histórico

**Aqui há informações detalhadas sobre cada elemento:**

1.    Cohort e período: Este tópico fornece o tipo de cohort. Aqui o "período de 2 dias" significa que vamos examinar o comportamento dos usuários durante 2 dias, os Usuários que tenham chegado por um período de 2 dias e como eles se conectaram nos seguintes blocos de 2 dias. O exemplo acima considera que a atividade dos usuários entre os dias 21 e 22 de novembro.
2.    A taxa de retenção determinada durante 21 e 22 de novembro para os usuários que chegaram em 19 e 20 de novembro. Aqui, tínhamos 1 usuário ativo entre os dias 21 e 22, sobre o 3 que eram novos usuários entre os dias 19 e 20.
3.    Esse indicador visual fornece as mesmas informações como as representadas graficamente acima. (A terceira do círculo é de número de 33%.) A cor fornece informações adicionais: O verde indica que esse número está crescendo a partir do cálculo anterior. Amarelo significa estável e vermelho significa redução.
4.    Isso indica os valores usados para o cálculo.
5.    Este é um minigráfico de histórico dos valores de retenção. Ele permite que você veja os valores no passado para ter uma visão geral de como ele evoluiu.

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: ../mobile-engagement-user-interface/
[Link 2]: ../mobile-engagement-troubleshooting-guide/
[Link 3]: ../mobile-engagement-how-tos/
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: ../mobile-engagement-user-interface-navigation/
[Link 13]: ../mobile-engagement-user-interface-home/
[Link 14]: ../mobile-engagement-user-interface-my-account/
[Link 15]: ../mobile-engagement-user-interface-analytics/
[Link 16]: ../mobile-engagement-user-interface-monitor/
[Link 17]: ../mobile-engagement-user-interface-reach/
[Link 18]: ../mobile-engagement-user-interface-segments/
[Link 19]: ../mobile-engagement-user-interface-dashboard/
[Link 20]: ../mobile-engagement-user-interface-settings/
[Link 21]: ../mobile-engagement-troubleshooting-guide-analytics/
[Link 22]: ../mobile-engagement-troubleshooting-guide-apis/
[Link 23]: ../mobile-engagement-troubleshooting-guide-push-reach/
[Link 24]: ../mobile-engagement-troubleshooting-guide-service/
[Link 25]: ../mobile-engagement-troubleshooting-guide-sdk/
[Link 26]: ../mobile-engagement-troubleshooting-guide-sr-info/
[Link 27]: ../mobile-engagement-how-tos-first-push/
[Link 28]: ../mobile-engagement-how-tos-test-campaign/
[Link 29]: ../mobile-engagement-how-tos-personalize-push/
[Link 30]: ../mobile-engagement-how-tos-differentiate-push/
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign/
[Link 32]: ../mobile-engagement-how-tos-text-view/
[Link 33]: ../mobile-engagement-how-tos-web-view/

<!--HONumber=47-->