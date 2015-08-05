<properties
   pageTitle="Create table as select (CTAS) no SQL Data Warehouse | Microsoft Azure"
   description="Dicas para codificação com a instrução create table as select (CTAS) no SQL Data Warehouse do Azure para desenvolvimento de soluções."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/26/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>

# Instrução Create Table As Select (CTAS) no SQL Data Warehouse
A instrução Create Table As Select ou CTAS é um dos mais importantes recursos do T-SQL disponível. É uma operação totalmente em paralelo que cria uma nova tabela com base na saída de uma instrução Select. Você pode considerá-la uma versão sobrecarregada de SELECT... INTO se desejar.

A CTAS também pode ser usada para solucionar diversos recursos sem suporte listados acima. Isso pode ser geralmente uma situação de ganho mútuo pois não apenas o seu código estará compatível, mas ele será executado muitas vezes mais rapidamente no SQL Data Warehouse. Isso é devido ao seu design totalmente em paralelo.

> [AZURE.NOTE]Tente imaginar “CTAS primeiro”. Se você achar que você pode resolver um problema usando CTAS, que geralmente é a melhor maneira de abordá-lo, mesmo se você estiver escrevendo mais dados como um resultado.

Os cenários que podem ser solucionados com CTAS incluem:

- SELECT..INTO
- ANSI JOINS em instruções UPDATE 
- ANSI JOINs em instruções DELETE
- Instrução MERGE

Este documento também inclui algumas práticas recomendadas para durante a codificação com CTAS.

## SELECT..INTO
Você pode achar SELECT... INTO aparecendo em vários lugares na sua solução.

Um exemplo SELECT..INTO está ilustrado abaixo:

```
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

A maneira para converter isso em CTAS é bastante direta:

```
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

O uso das CTAS significa que você também pode especificar uma preferência de distribuição de dados e o índice opcional na tabela também.

## Substituição da junção ANSI para atualizar instruções

Você pode descobrir uma atualização complexa que une mais de duas tabelas usando a sintaxe da junção ANSI para executar a UPDATE ou DELETE.

Imagine que você precisava atualizar esta tabela:

```
CREATE TABLE [dbo].[AnnualCategorySales]
(	[EnglishProductCategoryName]	NVARCHAR(50)	NOT NULL
,	[CalendarYear]					SMALLINT		NOT NULL
,	[TotalSalesAmount]				MONEY			NOT NULL
)
WITH
(
	DISTRIBUTION = ROUND_ROBIN
)
;
```

A consulta original pode se parecer com isso:

```
UPDATE	acs
SET		[TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM	[dbo].[AnnualCategorySales] 	AS acs
JOIN	(
		SELECT	[EnglishProductCategoryName]
		,		[CalendarYear]
		,		SUM([SalesAmount])				AS [TotalSalesAmount]
		FROM	[dbo].[FactInternetSales]		AS s
		JOIN	[dbo].[DimDate]					AS d	ON s.[OrderDateKey]				= d.[DateKey]
		JOIN	[dbo].[DimProduct]				AS p	ON s.[ProductKey]				= p.[ProductKey]
		JOIN	[dbo].[DimProductSubCategory]	AS u	ON p.[ProductSubcategoryKey]	= u.[ProductSubcategoryKey]
		JOIN	[dbo].[DimProductCategory]		AS c	ON u.[ProductCategoryKey]		= c.[ProductCategoryKey]
		WHERE 	[CalendarYear] = 2004
		GROUP BY
				[EnglishProductCategoryName]
		,		[CalendarYear]
		) AS fis
ON	[acs].[EnglishProductCategoryName]	= [fis].[EnglishProductCategoryName]
AND	[acs].[CalendarYear]				= [fis].[CalendarYear]
;
```

Como o SQL Data Warehouse não oferece suporte a junções ANSI, você não pode copiar esse código sem alterá-lo um pouco.

Você pode usar uma combinação de um CTAS e uma junção implícita para substituir este código:

```
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT	ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)	AS [EnglishProductCategoryName]
,		ISNULL(CAST([CalendarYear] AS SMALLINT),0) 						AS [CalendarYear]
,		ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)						AS [TotalSalesAmount]
FROM	[dbo].[FactInternetSales]		AS s
JOIN	[dbo].[DimDate]					AS d	ON s.[OrderDateKey]				= d.[DateKey]
JOIN	[dbo].[DimProduct]				AS p	ON s.[ProductKey]				= p.[ProductKey]
JOIN	[dbo].[DimProductSubCategory]	AS u	ON p.[ProductSubcategoryKey]	= u.[ProductSubcategoryKey]
JOIN	[dbo].[DimProductCategory]		AS c	ON u.[ProductCategoryKey]		= c.[ProductCategoryKey]
WHERE 	[CalendarYear] = 2004
GROUP BY
		[EnglishProductCategoryName]
,		[CalendarYear]
;

-- Use an implicit join to perform the update 
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## Substituição de junção ANSI para instruções delete
Às vezes, a melhor abordagem para a exclusão de dados é usar CTAS. Em vez de excluir os dados, simplesmente selecione os dados que você deseja manter. Isso é especialmente verdadeiro para as instruções DELETE que usam a sintaxe da junção ansi pois isso não tem suporte no SQL Data Warehouse.

Um exemplo de uma instrução DELETE convertida está disponível abaixo:

```
CREATE TABLE dbo.DimProduct_upsert
WITH 
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p 
RIGHT JOIN dbo.stg_DimProduct s 
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## Substituir instruções merge
Instruções de mesclagem podem ser substituídas, pelo menos em parte, usando CTAS. Você pode consolidar o `INSERT` e a `UPDATE` em uma única instrução. Quaisquer registros excluídos precisariam ser fechados em uma segunda instrução.

Um exemplo de uma instrução `UPSERT` está disponível abaixo:

```
CREATE TABLE dbo.[DimProduct_upsert]
WITH 
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS 
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## Recomendação de CTAS: declarar explicitamente o tipo de dados e a nulidade da saída

Ao migrar o código, você pode achar executar por esse tipo de padrão de codificação:

```
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f 
;
```

Instintivamente, é possível pensar que é necessário migrar esse código para um CTAS e você estaria certo. No entanto, há um problema oculto aqui.

O código a seguir NÃO produz o mesmo resultado:

```
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Observe que a coluna “resultado” transporta os valores de tipo e a nulidade de dados da expressão. Se você não tiver cuidado, isso pode levar a variações sutis em valores.

Tente o seguinte exemplo:

```
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

O valor armazenado para o resultado é diferente. Como o valor persistente na coluna de resultado é usado em outras expressões, o erro se torna ainda mais significativo.

![][1]

Isso é particularmente importante para migrações de dados. Mesmo que a segunda consulta seja indiscutivelmente mais precisa, há um problema. Os dados seriam diferentes em comparação com o sistema de origem e que leva a questões de integridade da migração. Esse é um dos casos raros em que a resposta “incorreta” é realmente a melhor!

O motivo que vemos esta disparidade entre os dois resultados é a conversão de tipo implícito. No primeiro exemplo, a tabela define a definição de coluna. Quando a linha é inserida uma conversão implícita de tipo ocorre. No segundo exemplo não há nenhuma conversão implícita de tipo como a expressão define o tipo de dados da coluna. Observe também que a coluna no segundo exemplo tenha sido definida como uma coluna anulável, enquanto no primeiro exemplo não tem. Quando a tabela foi criada no primeiro exemplo, a nulidade da coluna foi explicitamente definida. No segundo exemplo, ela estava à esquerda da expressão e, por padrão isso resultaria em uma definição NULL.

Para resolver esses problemas você deve definir explicitamente a nulidade e conversão de tipo na parte SELECT da instrução CTAS. Você não pode definir essas propriedades na parte criar tabela.

O exemplo a seguir demonstra como corrigir o código:

```
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Observe o seguinte: - CAST ou CONVERT poderia ter sido usado - ISNULL é usado para forçar a nulidade não COALESCE - ISNULL é a função mais externa - a segunda parte do ISNULL é uma constante ou seja, 0

> [AZURE.NOTE]Para a nulidade ser corretamente definida é vital usar ISNULL e não COALESCE. COALESCE não é uma função determinística e então o resultado da expressão sempre será anulável. ISNULL é diferente. É determinístico. Portanto, quando a segunda parte da função ISNULL é uma constante ou um literal, o valor resultante será NOT NULL.

Esta dica não é apenas útil para assegurar a integridade de seus cálculos. Também é importante para a alternância de partição de tabela. Imagine que você tenha essa tabela definida como o caso:

```
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH 
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
) 
;
```

No entanto, o campo de valor é uma expressão calculada que não é parte dos dados de origem.

Para criar o conjunto de dados particionado, você poderia fazer isso:

```
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product] 
,   [store] 
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

A consulta seria executada perfeitamente bem. O problema surge quando você tenta executar a opção de partição. As definições de tabela não correspondem. Para fazer com que as definições de tabela correspondam, a CTAS precisa ser modificada.

```
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product] 
,   [store] 
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Você pode ver, portanto, que a consistência de tipo e manutenção das propriedades de nulidade em um CTAS é uma boa prática recomendada de desenvolvimento. Ela ajuda a manter a integridade de seus cálculos e também garante que a alternância de partição é possível.

Consulte o MSDN para obter mais informações sobre como usar o [CTAS][]. Ela é uma das instruções mais importantes no SQL Data Warehouse do Azure. Certifique-se compreendê-la totalmente.

## Próximas etapas
Para obter mais dicas de desenvolvimento, consulte [Visão geral do desenvolvimento][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[Visão geral do desenvolvimento]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CTAS]: https://msdnstage.redmond.corp.microsoft.com/en-us/library/mt204041.aspx

<!--Other Web references-->

<!---HONumber=July15_HO4-->