<properties 
   pageTitle="Exibir alertas do Operations Manager"
   description="Saiba mais sobre como gerenciar alertas do Operations Manager para servidores monitorados na sua infraestrutura"
   services="operational-insights"
   documentationCenter=""
   authors="bandersmsft"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operational-insights"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="03/20/2015"
   ms.author="banders" />



# Exibir alertas do Operations Manager

Antes de usar o gerenciamento de alertas no Insights Operacionais do Microsoft Azure, você deve ter o pacote de inteligência instalado. Para saber mais sobre como instalar pacotes de inteligência, consulte [Usar a Galeria para adicionar ou remover pacotes de inteligência](../operational-insights-add-intelligence-packs.md). O pacote de inteligência só funciona quando seus servidores são monitorados por agentes do Operations Manager. Consulte [Coletar dados do computador](../operational-insights-collect-data.md) para obter informações sobre como usar o Operations Manager com Insights Operacionais.

Depois que o pacote de inteligência estiver instalado, você pode exibir alertas para os servidores monitorados usando o bloco **Gerenciamento de Alertas** no painel **Visão Geral** no Insights Operacionais. 

![imagem do bloco Gerenciamento de Alertas](./media/operational-insights-alerts/overview-alert.png)


Você pode exibir e gerenciar alertas do Insights Operacionais do Microsoft Azure do painel **Alertas**. No painel, as informações de alerta são exibidas nas seguintes categorias:

- Alertas ativos
	- Alertas críticos
	- Alertas de aviso
	- Fontes de alerta
- Todos os alertas ativos
	- Mostra o percentual de alertas críticos, avisos e informativos.
- Consultas comuns de alertas
	- Esta área contém consultas predefinidas que a equipe de desenvolvimento de software do Insights Operacionais criou para ajudar você a começar a usar alertas.


Alertas informam que foi detectado um problema, qual servidor o alerta afeta, a prioridade e o nome do alerta.

![imagem do painel Alerta](./media/operational-insights-alerts/alert-drilldown1.png)

![imagem do painel Alerta](./media/operational-insights-alerts/alert-drilldown2.png)


No painel **Gerenciamento de Alertas**, você pode exibir todos os alertas que o Alert Management do Microsoft Azure encontrou.

## Para exibir alertas do Alert Management

1. Na página **Visão Geral**, clique no bloco **Gerenciamento de Alertas**.

2. Sobre no painel **Gerenciamento de Alertas**, exiba as categorias de alerta e escolha um com a qual trabalhar.

3. Clique em um bloco ou qualquer item para exibir informações detalhadas sobre ele na página de **Pesquisa**.

4. Usando as informações encontradas, você pode investigar o alerta e determinar as ações adicionais que talvez você precise executar para resolvê-lo.


<!--HONumber=52--> 