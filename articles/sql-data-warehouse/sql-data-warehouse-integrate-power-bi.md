<properties
   pageTitle="Usar o Power BI com o SQL Data Warehouse | Microsoft Azure"
   description="Dicas para usar o Power BI com o SQL Data Warehouse do Azure para desenvolvimento de soluções."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/22/2015"
   ms.author="lodipalm"/>

# Usar o Power BI com o SQL Data Warehouse
Assim como o Banco de Dados SQL do Azure, a Conexão Direta do SQL Data Warehouse permite ao usuário aproveitar a aplicação lógica ao longo dos recursos analíticos do Power BI. Com a Conexão Direta, as consultas são enviadas de volta ao SQL Data Warehouse do Azure em tempo real enquanto você explora os dados. Esse recurso, combinado com a escala do SQL Data Warehouse, permite aos usuários criar relatórios dinâmicos dos terabytes de dados em questão de minutos. Além disso, a introdução do botão Abrir no Power BI permite que os usuários conectem o Power BI diretamente ao respectivo SQL Data Warehouse sem coletar informações de outras partes do Azure.

Ao usar a Conexão Direta:

+ Especifique o nome de servidor totalmente qualificado ao conectar (veja mais detalhes abaixo)
+ Certifique-se de que as regras de firewall estejam configuradas como “Permitir acesso aos serviços do Azure”.
+ Cada ação, como selecionar uma coluna ou adicionar um filtro, consultará diretamente o data warehouse 
+ Os blocos são atualizados aproximadamente a cada 15 minutos (a atualização não precisa ser agendada)
+ Perguntas e respostas não estão disponíveis para conjuntos de dados de Conexão Direta
+ As alterações no esquema não são selecionadas automaticamente

Essas restrições e observações podem mudar à medida que aprimoramos as experiências. As etapas para conexão estão detalhadas abaixo.

## Usando o botão ‘Abrir no Power BI’
A maneira mais fácil de mover entre o SQL Data Warehouse e o Power BI é usando o botão Abrir no Power BI. Esse botão permite que você comece a criar novos painéis diretamente no Power BI.

1.	Para começar, navegue até sua instância do SQL Data Warehouse no Portal do Azure.
2.	Clique no botão Abrir no Power BI.
3.	Se não pudermos conectar você diretamente ou se você não tiver uma conta do Power BI, será preciso se conectar.  
4.	Você será direcionado para a página de conexão do SQL Data Warehouse, com as informações do seu SQL Data Warehouse previamente populadas.
5.  Depois de inserir suas credenciais, você será totalmente conectado ao SQL Data Warehouse. 

## Conectando-se pelo portal do Power BI
Além de usar o botão Abrir no Power BI, os usuários também podem se conectar ao respectivo SQL Data Warehouse pelo Portal do Power BI.

1.   Clique em Obter Dados, na parte inferior do painel de navegação.
2.  Selecione Big Data e Mais.
3.  Assim que estiver na página Big Data e Mais, selecione SQL Data Warehouse.
4.  Insira as informações de conexão necessárias. A seção abaixo, Localizando parâmetros, mostra onde esses dados podem ser encontrados.  
5.	Analisando o conjunto de dados, você poderá explorar todas as tabelas e colunas no banco de dados. Selecionar uma coluna enviará uma consulta de volta à origem, criando dinamicamente seu visual. Esses visuais podem ser salvos em um novo relatório e fixados no seu painel.

## Localizando valores de parâmetro
O nome de servidor totalmente qualificado e o nome do banco de dados podem ser encontrados no Portal do Azure. Observe que o SQL Data Warehouse tem apenas uma presença no Portal de Visualização do Azure neste momento.

## Próximas etapas
Para obter uma visão geral da integração, consulte [Visão geral de integração do SQL Data Warehouse][]. Para obter mais dicas de desenvolvimento, consulte [Visão geral de desenvolvimento do SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Visão geral de desenvolvimento do SQL Data Warehouse]: ./sql-data-warehouse-overview-develop/
[Visão geral de integração do SQL Data Warehouse]: ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->

<!---HONumber=July15_HO4-->