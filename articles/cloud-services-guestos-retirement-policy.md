﻿<properties 
   pageTitle="Guia de capacidade de suporte e política de desativação para sistema operacional convidado do Azure | Azure" 
   description="Fornece informações sobre a que a Microsoft dará suporte como relação ao sistema operacional convidado do Azure usado pelos Serviços de Nuvem." 
   services="cloud-services" 
   documentationCenter="na" 
   authors="Thraka" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="02/23/2015"
   ms.author="adegeo"/>

# Suporte do sistema operacional convidado do Azure e política de desativação
As informações nesta página se relacionam ao sistema operacional convidado do Azure ([SO Convidado](https://msdn.microsoft.com/library/azure/ff729422.aspx)) para serviços de nuvem funções web e de trabalho (PaaS). Não se aplicam às Máquinas Virtuais (IaaS). 

A Microsoft tem uma [política de suporte para o sistema operacional convidado](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq) publicada. A página que você está lendo agora descreve como a política é implementada.

A política é 

1. A Microsoft dará suporte a **pelo menos as duas famílias mais recentes do sistema operacional convidado**. Quando uma família for desativada, os clientes terão 12 meses a partir da data de desativação oficial para atualizar para uma família dps sistemas operacionais convidados com suporte mais recente.
2. A Microsoft dará suporte a **pelo menos as duas versões mais recentes das famílias de sistemas operacionais convidados com suporte**. 
3. A Microsoft dará suporte a **pelo menos as duas versões mais recentes do SDK do Azure**. Quando uma versão do SDK for desativada, os clientes terão 12 meses a partir da data de desativação oficial para atualizar para uma versão mais recente. 

Às vezes, pode haver suporte para mais de duas famílias ou versões. Informações de suporte oficiais do sistema operacional convidado serão exibidas na [Matriz de compatibilidade do SDK e lançamentos do SO convidado do Azure](cloud-services-guestos-update-matrix.md).


## Quando uma família ou versão de sistemas operacionais convidados for desativada 


Uma nova **família** de sistemas operacionais convidados é introduzida algum tempo após o lançamento de uma nova versão oficial do sistema operacional Windows Server. Sempre que uma nova família de sistemas operacionais convidados é introduzida, a Microsoft desativa a família mais antiga do sistema operacional convidado. 

As novas **versões** do sistema operacional convidado são lançadas a cada mês para incorporar as atualizações mais recentes do MSRC. Devido às atualizações regulares mensais, uma versão do sistema operacional convidado é desabilitada normalmente 60 dias após seu lançamento. Isso mantém pelo menos duas versões de sistema operacional convidado para cada família disponíveis para uso. 

### Processo durante a desativação de uma família de sistemas operacionais convidados 


Depois que a desativação é anunciada, os clientes têm um período de "transição" de 12 meses antes de a família mais antiga ser oficialmente removida do serviço. Esse período de transição pode ser estendido a critério da Microsoft. As atualizações serão postadas na [Matriz de compatibilidade do SDK e lançamentos do SO convidado do Azure](cloud-services-guestos-update-matrix.md).

Um processo gradual de desativação se iniciará em 6 meses no período de transição. Durante esse tempo:

1. A Microsoft notificará os clientes da desativação. 
2. A versão mais recente do SDK do Azure não dará suporte à família do sistema operacional convidado desativada.
3. Novas implantações e reimplantações dos Serviços de Nuvem não serão permitidas na família desativada

A Microsoft continuará a apresentar a nova versão do sistema operacional convidado incorporando as atualizações mais recentes do MSRC até o último dia do período de transição, conhecido como "data de expiração". Nesse momento, quaisquer Serviços de Nuvem ainda em execução não terão mais suporte no SLA do Azure. A Microsoft pode impor a atualização, excluir ou interromper os serviços após essa data.



### Processo durante a desativação de uma versão do SO convidado 
Se os clientes definirem o SO convidado para atualizar automaticamente, nunca precisarão se preocupar em lidar com as versões de sistema operacional convidado. Eles sempre usarão a versão mais recente do sistema operacional convidado.

Versões do SO convidado são lançadas a cada mês. Devido aos lançamentos regulares, cada versão tem uma vida útil fixa.

Aos 60 dias da vida útil, uma versão é "*desabilitada*". "Desabilitada" significa que a versão é removida do Portal de Gerenciamento do Azure. Ela também pode não ser definida no arquivo de configuração CSCFG. As implantações existentes são mantidas em execução, mas novas implantações e atualizações de código e a configuração para as implantações existentes não serão permitidas. 

Em um momento posterior, a versão do sistema operacional convidado "*expira*", e todas as instalações que ainda estiverem executando essa versão serão forçosamente atualizadas e definidas para atualizar automaticamente o sistema operacional convidado no futuro. A expiração é feita em lotes, de modo que o período de tempo de desativação da validade pode variar. 

Esses períodos podem ser mais longos, a critério da Microsoft, para facilitar as transições do cliente. Todas as alterações serão comunicadas na [Matriz de compatibilidade do SDK e lançamentos do SO convidado do Azure](cloud-services-guestos-update-matrix.md).



### Notificações durante a desativação 

* **Desativação de família** <br>A Microsoft usará postagens de blog e notificação no portal de gerenciamento. Os clientes que ainda estiverem usando uma família desativada do sistema operacional convidado serão notificados por meio de comunicação direta (email, mensagens do portal, telefonema) para administradores de serviço atribuídos. Todas as alterações serão postadas nessa página e o RSS feed será listado no início desta página. 


* **Desativação de versão** <br>Todas as alterações serão postadas nessa página e o RSS feed será listado no início desta página, incluindo as datas de lançamento, desabilitação e expiração. Os administradores de serviços receberão emails se tiverem implantações em execução em uma família ou versão do SO convidado desabilitada. O intervalo desses emails pode variar. Geralmente, eles são enviados pelo menos um mês antes da desabilitação, embora esse intervalo não seja um SLA oficial. 


## Perguntas frequentes

**Como posso reduzir os impactos da migração?**

Você deve usar a família mais recente do sistema operacional convidado para criar seus Serviços de Nuvem. 

1. Comece cedo a planejar a migração para uma família mais recente. 
2. Configure implantações de teste temporárias para testar seu serviço de nuvem em execução na nova família. 
3. [Defina a versão do sistema operacional convidado](https://msdn.microsoft.com/library/azure/gg433101.aspx) para "Automática" (osVersion=* no arquivo [.CSCFG](https://msdn.microsoft.com/library/azure/gg456324.aspx)) para que a migração para novas versões de sistema operacional convidado ocorra automaticamente.

**E se meu aplicativo Web exigir uma integração mais profunda com o sistema operacional?**

Se a arquitetura do aplicativo Web exigir uma dependência mais profunda do sistema operacional subjacente, use os recursos de suporte de plataforma como "[Tarefas de Inicialização](https://msdn.microsoft.com/library/windowsazure/gg456327.aspx)" ou outros mecanismos de extensibilidade que possam existir no futuro. Como alternativa, você também pode usar [Máquinas Virtuais do Azure](http://www.windowsazure.com/home/scenarios/virtual-machines/) (IaaS - infraestrutura como serviço), em que você é responsável por manter o sistema operacional subjacente.

<!--HONumber=52-->