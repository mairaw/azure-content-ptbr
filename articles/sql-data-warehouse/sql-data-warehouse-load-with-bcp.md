<properties
   pageTitle="Usar o bcp para carregar dados no SQL Data Warehouse | Microsoft Azure"
   description="Saiba o que é o bcp e como usá-lo em cenários de data warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="TwoUnder"
   manager="barbkess"
   editor="JRJ@BigBangData.co.uk"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/23/2015"
   ms.author="mausher;barbkess"/>


# Carregar dados com o bcp
O **[bcp][]** é um utilitário de carregamento em massa de linha de comando que permite copiar dados entre o SQL Server, arquivos de dados e o SQL Data Warehouse. Use o bcp para importar grandes quantidades de linhas nas tabelas do SQL Data Warehouse ou para exportar dados das tabelas do SQL Server em arquivos de dados. Exceto quando usado com a opção queryout, o bcp não exige conhecimento em Transact-SQL.

O bcp é uma maneira rápida e fácil de mover conjuntos de dados menores para dentro e fora de um banco de dados do SQL Data Warehouse. O volume exato de dados que é recomendado para carregamento/extração por meio do bcp dependerá da sua conexão de rede com o datacenter do Azure. Geralmente, as tabelas de dimensão podem ser carregadas e extraídas, mas as tabelas de fatos razoavelmente grandes podem demorar bastante para serem carregadas ou extraídas.

Com o bcp, você pode: - usar um utilitário de linha de comando simples para carregar dados no SQL Data Warehouse. - Usar um utilitário de linha de comando simples para extrair dados do SQL Data Warehouse.

Este tutorial mostrará a você como: - importar dados em uma tabela usando o bcp no comando - Exportar dados de uma tabela usando o bcp fora do comando

## Pré-requisitos
Para acompanhar este tutorial, você precisa: - de um banco de dados do SQL Data Warehouse - Do utilitário de linha de comando bcp instalado - Do utilitário de linha de comando SQLCMD instalado

>[AZURE.NOTE]Você pode baixar os utilitários bcp e sqlcmd do [Centro de Download da Microsoft][].

##Importar dados no SQL Data Warehouse
Neste tutorial, você criará uma tabela no SQL Data Warehouse e importará dados na tabela.

### Etapa 1: Criar uma tabela no SQL Data Warehouse do Azure
Em um prompt de comando, conecte-se à sua instância usando o comando a seguir, substituindo os valores conforme apropriado:

```
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I
```
Depois de conectado, copie o script de tabela a seguir no prompt do sqlcmd e pressione a tecla Enter:

```
CREATE TABLE DimDate2 (DateId INT NOT NULL, CalendarQuarter TINYINT NOT NULL, FiscalQuarter TINYINT NOT NULL);
```

Na próxima linha, insira o terminador de lote GO e, em seguida, pressione a tecla Enter para executar a instrução:

```
GO
```

### Etapa 2: Criar um arquivo de dados de origem

Abra o Bloco de Notas e copie as linhas de dados a seguir em um novo arquivo.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

Salve-o no diretório temporário local, C:\\Temp\\DimDate2.txt.

> [AZURE.NOTE]É importante lembrar que o bcp.exe não oferece suporte a codificação do arquivo UTF-8. Use arquivos codificados ASCII ou a codificação UTF-16 para os seus arquivos ao usar o bcp.exe.

### Etapa 3: Conectar e importar os dados
Usando o bcp, você pode conectar e importar os dados usando o comando a seguir, substituindo os valores conforme apropriado:

```
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Você pode verificar se os dados foram carregados conectando-se ao sqlcmd como antes e executando o seguinte comando TSQL:

```
SELECT * FROM DimDate2 ORDER BY 1;
GO
```

Isso deve retornar os seguintes resultados:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2

## Exportar dados do SQL Data Warehouse
Neste tutorial, você criará um arquivo de dados de uma tabela no SQL Data Warehouse. Exportaremos os dados que criamos acima para um novo arquivo de dados chamado DimDate2_export.txt.

### Etapa 1: Exportar os dados

Usando o utilitário bcp, você pode conectar e exportar dados usando o comando a seguir, substituindo os valores conforme apropriado:

```
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Você pode verificar se os dados foram exportados corretamente abrindo o novo arquivo. Os dados no arquivo devem corresponder ao texto abaixo:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

>[AZURE.NOTE]Devido à natureza dos sistemas distribuídos, a ordem dos dados pode não ser a mesma entre os bancos de dados do SQL Data Warehouse. Como opção, você pode usar o parâmetro queryout para especificar qual consulta Transact-SQL executar.

## Próximas etapas
Para obter uma visão geral do carregamento, consulte [Carregar dados no SQL Data Warehouse][]. Para obter mais dicas de desenvolvimento, consulte [Visão geral de desenvolvimento do SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->

[Carregar dados no SQL Data Warehouse]: ./sql-data-warehouse-overview-load/
[Visão geral de desenvolvimento do SQL Data Warehouse]: ./sql-data-warehouse-overview-develop/

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx


<!--Other Web references-->
[Centro de Download da Microsoft]: http://www.microsoft.com/download/details.aspx?id=36433

<!---HONumber=July15_HO4-->