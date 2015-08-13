<properties
    pageTitle="Banco de Dados SQL do Azure – biblioteca de cliente"
    description="Crie aplicativos de banco de dados .NET escalonáveis"
    services="sql-database"
    documentationCenter=""
    manager="jeffreyg"
    authors="sidneyh"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2015"
    ms.author="sidneyh"/>

# Visão geral da biblioteca de cliente do Banco de Dados Elástico

A **biblioteca de cliente do Banco de Dados Elástico** permite criar aplicativos cliente que gerenciam coleções de centenas, ou até mesmo milhares, de bancos de dados SQL do Azure hospedados no Microsoft Azure. Essas coleções costumam ser usadas para aplicativos de software como serviço (SaaS), que tipicamente são arquiteturas de locatário único, em que cada locatário é provisionado com um banco de dados. Criar e gerenciar um aplicativo assim são os objetivos da biblioteca.

A biblioteca de cliente é uma biblioteca do .NET Framework que pode ser instalada em qualquer projeto de aplicativo usando Visual Studio e Nuget. A biblioteca de cliente faz parte de um conjunto de [Ferramentas de Banco de Dados Elástico](sql-azure-database-elastic-scale-introduction.md).


## Recursos do cliente

Desenvolver, dimensionar e gerenciar aplicativos de expansão usando *fragmentação* (discutida a seguir) apresentam desafios para o desenvolvedor e para o administrador. A biblioteca de cliente torna a vida mais fácil para ambas essas funções. Os números na figura a seguir descrevem os principais recursos fornecidos pela biblioteca de cliente do banco de dados elástico e ferramenta de divisão/mesclagem. A figura ilustra um ambiente com muitos bancos de dados e cada banco de dados corresponde a um fragmento. As ferramentas facilitam o desenvolvimento de aplicativos do Banco de Dados SQL do Azure fragmentados por meio dos seguintes recursos específicos:

Para obter definições dos termos usados aqui, consulte o [Glossário de ferramentas do banco de dados elástico](sql-database-elastic-scale-glossary.md).

![Capacidades de escala elástica][1]

1.  **Gerenciamento de mapa de fragmentos**: para gerenciar uma coleção de fragmentos, é criado um banco de dados especial chamado de “gerenciador de mapa de fragmentos". Gerenciamento de mapa do fragmento é a capacidade de um aplicativo para gerenciar vários metadados sobre seus fragmentos. Os desenvolvedores podem usar essa funcionalidade para registrar bancos de dados como fragmentos, descrever os mapeamentos de chaves de fragmentação individuais ou intervalos de chaves para os bancos de dados e manter esses metadados como o número e a composição de bancos de dados que evolui para refletir as alterações de capacidade. Sem a biblioteca de cliente do banco de dados elástico, você precisaria gastar muito tempo escrevendo o código de gerenciamento ao implementar a fragmentação. Para obter mais detalhes, consulte [Gerenciamento de mapa de fragmentos](sql-database-elastic-scale-shard-map-management.md).

* **Roteamento dependente de dados**: imagine uma solicitação chegando no aplicativo. Com base no valor da chave de fragmentação da solicitação, o aplicativo precisa determinar o banco de dados correto que contém os dados para esse valor de chave e, em seguida, abrir uma conexão para processar a solicitação. Roteamento dependente de dados fornece a capacidade de abrir conexões com uma única chamada simples para o mapa do fragmento do aplicativo. O roteamento dependente de dados era outra área do código de infraestrutura que agora é coberta por uma funcionalidade na biblioteca de cliente de banco de dados elástico. Para obter detalhes, consulte [Roteamento dependente de dados](sql-database-elastic-scale-data-dependent-routing.md).

* **MSQ (Consultas em Vários Fragmentos)**: consulta em vários fragmentos funciona quando uma solicitação envolve vários fragmentos, ou todos eles. Uma consulta de vários fragmento executa o mesmo código T-SQL em todos os fragmentos ou um conjunto de fragmentos. Os resultados de fragmentos de participantes são mesclados em um resultado geral definido usando a semântica UNION ALL. A funcionalidade é exposta por meio da biblioteca cliente que trata muitas tarefas, incluindo: gerenciamento de conexões, gerenciamento de threads, tratamento de falhas e os processamento de resultados intermediários. MSQ pode consultar até centenas de fragmentos. Para obter mais detalhes, consulte [Consulta de vários fragmentos](sql-database-elastic-scale-multishard-querying.md).

Em geral, os clientes que usam as ferramentas de banco de dados elástico podem esperar obter a funcionalidade completa do T-SQL durante o envio de operações de fragmento local em vez de operações entre fragmentos que têm suas próprias semânticas.

## Próximas etapas

Experimente o [aplicativo de exemplo](sql-database-elastic-scale-get-started.md), que demonstra as funções do cliente.

Para instalar a biblioteca, vá para [Biblioteca de cliente do Banco de Dados Elástico](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Para obter instruções sobre o uso da ferramenta de mesclagem e divisão, consulte [Visão geral da ferramenta de mesclagem e divisão](sql-database-elastic-scale-overview-split-and-merge.md).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]: ./media/sql-database-elastic-database-client-library/glossary.png

<!---HONumber=July15_HO5-->