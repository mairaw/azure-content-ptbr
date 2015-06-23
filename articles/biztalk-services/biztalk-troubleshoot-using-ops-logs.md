<properties 
	pageTitle="Serviços BizTalk: Solucionar problemas usando logs de operação | Azure" 
	description="Serviços BizTalk de solução de problemas usando logs de operação. MABS, WABS" 
	services="biztalk-services" 
	documentationCenter="" 
	authors="nitinme" 
	manager="dwrede" 
	editor="cgronlun"/>

<tags 
	ms.service="biztalk-services" 
	ms.workload="integration" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/16/2015" 
	ms.author="mandia"/>


# Serviços do BizTalk: Solução de problemas usando os logs de operação

Os Logs de Operação são um recurso dos Serviços de Gerenciamento disponível no Portal de Gerenciamento do Azure que permite exibir os logs históricos de operações executadas nos serviços do Azure, incluindo o Serviço do BizTalk. Isso permite exibir dados históricos relacionados às operações de gerenciamento em sua assinatura do Serviço do BizTalk por um período de até 180 dias.

<div class="dev-callout"><b>Observação</b>
<p>Esse recurso captura apenas logs de operações de gerenciamento nos Serviços BizTalk, como quando o serviço foi iniciado, o backup foi feito etc. Essas operações são acompanhadas independentemente de serem executadas no Portal de Gerenciamento do Azure ou usando as <a href="http://msdn.microsoft.com/library/windowsazure/dn232347.aspx">APIs REST do Serviço BizTalk</a>. Para obter uma lista completa das operações que são acompanhadas usando os Serviços de Gerenciamento, consulte <a href="#bizops">Operações acompanhadas usando os Serviços de Gerenciamento do Azure</a>.</p>
<p>Isso não captura os logs de atividades relacionadas ao tempo de execução do Serviço BizTalk (como mensagens processadas pelas pontes etc.) Para exibir esses logs, você deve usar a exibição Acompanhamento do portal dos Serviços BizTalk. Para obter mais informações, consulte <a HREF="http://msdn.microsoft.com/library/windowsazure/hh949805.aspx">Acompanhando mensagens</a>.</p>
</div>

##<a name="viewlogs"></a>Exibir os logs de operações dos Serviços BizTalk
1. No Portal de Gerenciamento do Azure, clique em Serviços de Gerenciamento e, em seguida, clique na guia Logs de Operação.
2. Você pode filtrar os logs com base em vários parâmetros, como assinatura, intervalo de dados, tipo de serviço (por exemplo, Serviços do BizTalk), nome do serviço ou status (da operação, por exemplo, Obteve êxito, Com falha)
3. Clique na marca de seleção para exibir a lista filtrada. A ilustração a seguir mostra as atividades relacionadas ao testbiztalkservice.
	![View operation logs][ViewLogs] 
4. Para v mais sobre uma operação específica, selecione a linha e clique em <b>Detalhes</b> na parte inferior da página.


##<a name="bizops"></a>Operações acompanhadas usando os Serviços de Gerenciamento do Azure
A tabela a seguir lista as operações que são acompanhadas usando os Serviços de Gerenciamento do Azure.

<table border="1" cellpadding="5">
<tr>
<td>CreateBizTalkService</td> 
<td align="left">Operação para criar um novo Serviço BizTalk</td> 
</tr> 
<tr>
<td>DeleteBizTalkService</td> 
<td align="left">Operação para excluir um novo Serviço BizTalk</td>  
</tr> 
<tr>
<td>RestartBizTalkService</td> 
<td align="left">Operação para reiniciar um Serviço BizTalk</td> 
</tr>
<tr>
<td>StartBizTalkService</td> 
<td align="left">Operação para iniciar um Serviço BizTalk</td> 
</tr>
<tr>
<td>StopBizTalkService</td> 
<td align="left">Operação para parar um Serviço BizTalk</td> 
</tr>
<tr>
<td>DisableBizTalkService</td> 
<td align="left">Operação para desabilitar um Serviço BizTalk</td> 
</tr>
<tr>
<td>EnableBizTalkService</td> 
<td align="left">Operação para habilitar um Serviço BizTalk</td> 
</tr>
<tr>
<td>BackupBizTalkService</td> 
<td align="left">Operação para fazer backup de um Serviço BizTalk</td> 
</tr>
<tr>
<td>RestoreBizTalkService</td> 
<td align="left">Operação para restaurar um Serviço BizTalk de um backup especificado</td> 
</tr>
<tr>
<td>SuspendBizTalkService</td> 
<td align="left">Operação para suspender um Serviço BizTalk</td> 
</tr>
<tr>
<td>ResumeBizTalkService</td> 
<td align="left">Operação para continuar um Serviço BizTalk</td> 
</tr>
<tr>
<td>ScaleBizTalkService</td> 
<td align="left">Operação para ampliar ou reduzir um Serviço BizTalk</td> 
</tr>
<tr>
<td>ConfigUpdateBizTalkService</td> 
<td align="left">Operação para atualizar a configuração de um Serviço BizTalk</td> 
</tr>
<tr>
<td>ServiceUpdateBizTalkService</td> 
<td align="left">Operação para fazer upgrade ou downgrade um Serviço BizTalk Service para outra versão</td> 
</tr>
<tr>
<td>PurgeBackupBizTalkService</td> 
<td align="left">Operação para limpar backups do Serviço BizTalk fora do período de retenção</td> 
</tr>
</table>


## Consulte também
- [Fazer o backup do Serviço BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [Restaurar o Serviço BizTalk do backup](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [Serviços do BizTalk: tabela de edições Developer, Basic, Standard e Premium](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [Serviços do BizTalk: provisionamento usando o Portal de Gerenciamento do Azure](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [Serviços do BizTalk: Gráfico de status do provisionamento](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [Serviços do BizTalk: guias Painel, Monitoramento e Escala](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [Serviços do BizTalk: limitação](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [Serviços do BizTalk: nome e chave do emissor](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Como começar a usar o SDK dos Serviços BizTalk do Azure (a página pode estar em inglês)](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

<!--HONumber=46--> 
 