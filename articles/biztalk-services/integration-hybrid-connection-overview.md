<properties
	pageTitle="Visão geral de Conexões Híbridas | Microsoft Azure"
	description="Saiba mais sobre conexões híbridas, incluindo segurança, portas TCP e configurações com suporte. MABS, WABS."
	services="biztalk-services"
	documentationCenter=""
	authors="MandiOhlinger"
	manager="dwrede"
	editor="cgronlun"/>

<tags
	ms.service="biztalk-services"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="12/01/2015"
	ms.author="mandia"/>


# Visão geral de Conexões Híbridas
Este artigo apresenta Conexões Híbridas, lista as configurações com suporte e lista também as portas TCP necessárias.


## O que é uma conexão híbrida

As Conexões Híbridas são um recurso dos Serviços BizTalk do Azure. Conexões híbridas fornecem uma maneira fácil e conveniente de conectar o recurso de Aplicativos Web no Serviço de Aplicativo do Azure (anteriormente, Sites) e o recurso Aplicativos Móveis no Serviço de Aplicativo do Azure (anteriormente, Serviços Móveis) para recursos locais por trás do firewall.

![Conexões Híbridas][HCImage]

Os benefícios das Conexões Híbridas incluem:

- Aplicativos Web e Aplicativos Móveis podem acessar, com segurança, serviços e dados locais existentes.
- Múltiplos Aplicativos Web ou Aplicativos Móveis podem compartilhar uma Conexão Híbrida para acessar um recurso local.
- O mínimo de portas TCP é necessário para acessar sua rede.
- Os aplicativos utilizando Conexões Híbridas acessam somente o recurso específico local que é publicado por meio da Conexão Híbrida.
- Eles podem se conectar a qualquer recurso local que utilize uma porta TCP estática, como o SQL Server, MySQL, APIs Web HTTP e a maioria dos serviços web personalizados.

	> [AZURE.NOTE]Serviços baseados em TCP que utilizam portas dinâmicas (como um Modo passivo de FTP ou Modo passivo estendido) não têm suporte atualmente.

- Podem ser utilizados com todas as estruturas com suporte nos Aplicativos Web (.NET, PHP, Java, Python, Node.js) e Aplicativos Móveis (Node.js, .NET).
- Os Aplicativos Web e Aplicativos Móveis podem acessar recursos locais exatamente do mesmo modo que fariam se o Aplicativo Web ou Móvel estivesse em sua rede local. Por exemplo, a mesma cadeia de conexão utilizada localmente também pode ser utilizada no Azure.


As Conexões Híbridas também fornecem aos administradores corporativos controle e visibilidade dos recursos corporativos acessados pelos aplicativos híbridos, incluindo:

- Usando configurações de Política de Grupo, os administradores podem permitir Conexões Híbridas na rede e também atribuir recursos que podem ser acessados por aplicativos híbridos.
- Logs de evento e auditoria na rede corporativa oferecem visibilidade dos recursos acessados por Conexões Híbridas.


## Configurações com suporte

As Conexões Híbridas suportam as combinações a seguir de estrutura e aplicativo:

- Acesso .NET framework ao SQL Server
- Acesso .NET framework a serviços HTTP/HTTPS com WebClient
- Acesso PHP a SQL Server, MySQL
- Acesso Java a SQL Server, MySQL e Oracle
- Acesso Java a serviços HTTP/HTTPS

Ao utilizar Conexões Híbridas para acessar o SQL Server local, considere o seguinte:

- Instâncias nomeadas como SQL Express devem ser configuradas para utilizar portas estáticas. Por padrão, as instâncias nomeadas como SQL Express utilizam portas dinâmicas.
- As instâncias padrão do SQL Express utilizam uma porta estática, mas o TCP precisa estar habilitado. Por padrão, o uso de TCP não está habilitado.
- Ao utilizar Armazenamento em Cluster ou Grupos de Disponibilidade, o modo `MultiSubnetFailover=true` não é suportado no momento.
- O `ApplicationIntent=ReadOnly` não é suportado no momento.
- A autenticação SQL pode ser exigida como o método de autorização de ponta a ponta suportado pelo aplicativo do Azure e o SQL Server local.


## Segurança e portas

As Conexões Híbridas utilizam a autorização de Assinatura de Acesso Compartilhado (SAS) para proteger as conexões dos aplicativos do Azure e o Gerenciador de Conexões Híbridas local para a Conexão Híbrida. Chaves de conexão separadas são criadas para o aplicativo e para o Gerenciador de Conexões Híbridas local. É possível fazer a rolagem por essas chaves de conexão e revogá-las de modo independente.

As Conexões Híbridas fornecem uma distribuição perfeita e segura das chaves aos aplicativos e ao Gerenciador de Conexões Híbridas local.

Consulte [Criar e gerenciar Conexões Híbridas](integration-hybrid-connection-create-manage.md).

*A autorização do aplicativo é separada da Conexão Híbrida*. Qualquer método adequado de autorização pode ser usado. O método de autorização depende dos métodos de autorização de ponta a ponta suportados pela nuvem do Azure e nos componentes locais. Por exemplo, o seu aplicativo do Azure acessa um SQL Server local. Neste cenário, a Autorização SQL pode ser o método de autorização suportado de ponta a ponta.

#### Portas TCP
Conexões Híbridas exigem somente conectividade TCP ou HTTP de saída da sua rede privada. Você não precisa abrir portas de firewall ou alterar sua configuração de perímetro de rede para permitir qualquer conectividade de entrada em sua rede.

As portas TCP a seguir são usadas por Conexões Híbridas:

Porta | Por que você precisa disto
--- | ---
9350 - 9354 | Essas portas são usadas para a transmissão de dados. O gerenciador de retransmissão do Barramento de Serviço investiga a porta 9350 para determinar se a conectividade TCP está disponível. Quando está disponível, ele pressupõe que a porta 9352 também está. O tráfego de dados passa pela porta 9352. <br/><br/>Permita conexões de saída para essas portas.
5671 | Quando a porta 9352 é usada para tráfego de dados, a porta 5671 é usada como canal de controle. <br/><br/>Permita conexões de saída para essa porta.
80, 443 | Essas portas são usadas para algumas solicitações de dados no Azure. Além disso, se as portas 9352 e 5671 não forem utilizáveis, *então* as portas 80 e 443 serão as portas de fallback usadas para transmissão de dados e o canal de controle.<br/><br/>Permita conexões de saída para essas portas.<br/><br/>**Observação** Não é recomendado usá-las como as portas de fallback no lugar de outras portas TCP. O HTTP/WebSocket é usado como o protocolo, em vez do TCP nativo para canais de dados. Isso poderia degradar o desempenho.



## Próximas etapas

[Criar e gerenciar Conexões Híbridas](integration-hybrid-connection-create-manage.md)<br/> [Conectar um Site do Azure a um recurso local](../web-sites-hybrid-connection-get-started.md)<br/> [Conectar-se a um Servidor SQL local a partir de um aplicativo web do Azure](../web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/> [Serviços Móveis do Azure e Conexões Híbridas](../mobile-services-dotnet-backend-hybrid-connections-get-started.md)


## Consulte também

[API REST para gerenciar Serviços BizTalk no Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx) [Serviços BizTalk: gráfico de edições](biztalk-editions-feature-chart.md)<br/> [Criar um Serviço BizTalk usando o Portal do Azure](biztalk-provision-services.md)<br/> [Serviços BizTalk: guias Painel, Monitor e Escala](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png

<!---HONumber=AcomDC_1203_2015-->