<properties 
	pageTitle="Limites da versão de teste do Banco de Dados de Documentos | Azure" 
	description="Saiba mais sobre os limites e imposições de cota do Banco de Dados de Documentos para a versão de visualização." 
	services="documentdb" 
	authors="mimig1" 
	manager="jhubbard" 
	editor="cgronlun" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="01/29/2015" 
	ms.author="mimig"/>


#Limites da versão de teste do Banco de Dados de Documentos
A tabela a seguir descreve os limites e imposições de cota do Banco de Dados de Documentos durante a versão de teste. Na maioria dos casos, os limites são impostos com a intenção de obter seus comentários ou com base nas restrições de capacidade atual. Se você tem a necessidade comercial de reduzir os limites, entre em contato conosco e faremos o possível para acomodá-la dentro das restrições da oferta pública.    

|Entidade |Quota (oferta Standard na versão de teste)|
|-------|--------|
|Contas do banco de dados     |5
|Número de bancos de dados por conta de banco de dados     |100
|Número de usuários por conta de banco de dados - em todos os bancos de dados |500.000
|Número de permissões por conta de banco de dados - em todos os bancos de dados   |2.000.000
|Armazenamento de anexos por conta de banco de dados      |2 GB
|Número máximo de unidades de capacidade por conta de banco de dados       |50
|Número de coleções por unidade de capacidade      |3
|Armazenamento mínimo alocado por coleção com, no mínimo, 1 documento    |3,3 GB
|Rendimento mínimo alocado por coleção com, no mínimo, 1 documento |667 RUs (unidades de solicitação)
|Elasticidade da coleção    |0 GB a 10 GB
|Máximo de unidades de solicitação/s por coleção   |2000
|Número de procedimentos armazenados, gatilhos e UDFs por coleção       |25 cada
|Tempo de execução máximo para procedimento armazenado e gatilho     |5 s
|Armazenamento/unidade de capacidade de documento provisionado |10 GB
|Unidades de solicitação/s provisionadas por unidade de capacidade     |2000
|Armazenamento máximo de documentos por banco de dados (5 unidades de capacidade)    |500 GB
|Comprimento máximo da propriedade Id    |255 caracteres
|Número padrão de itens por página     |100
|Máximo de itens por página        |1000
|Tamanho máximo da solicitação de documento e anexo       |512KB
|Tamanho máximo da solicitação de procedimento armazenado, gatilho e UDF        |256KB
|Tamanho de resposta máximo |1MB
|Número máximo de caminhos exclusivos por coleção       |100
|Cadeia de caracteres |Todas as cadeias de caracteres devem ser codificadas em UTF-8. Como UTF-8 é uma codificação com largura variável, os tamanhos das cadeias de caracteres são determinados usando os bytes UTF-8.
|Comprimento máximo da propriedade ou valor  |Sem limite prático
|Número máximo de UDFs por consulta     |1
|Número máximo de JOINs por consulta    |2
|Número máximo de cláusulas AND por consulta      |5
|Número máximo de cláusulas OR por consulta       |5

<!--HONumber=49--> 