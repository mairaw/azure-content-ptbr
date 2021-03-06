<properties 
	pageTitle="Código do Buffer de Anel de XEvent para o Banco de Dados SQL | Microsoft Azure" 
	description="Fornece um exemplo de código Transact-SQL que se torna fácil e rápido por meio do uso do destino do Buffer de Anéis, no Banco de Dados SQL do Azure." 
	services="sql-database" 
	documentationCenter="" 
	authors="MightyPen" 
	manager="jeffreyg" 
	editor="" 
	tags=""/>


<tags 
	ms.service="sql-database" 
	ms.workload="data-management" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="10/22/2015" 
	ms.author="genemi"/>


# Código de destino do Buffer de Anéis para eventos estendidos no Banco de Dados SQL


Você deseja um exemplo de código completo da maneira mais fácil e rápida de capturar e relatar informações para um evento estendido durante um teste. O destino mais fácil para os dados do evento estendido é o [destino do Buffer de Anéis](http://msdn.microsoft.com/library/ff878182.aspx).


Este tópico apresenta um exemplo de código Transact-SQL que:


1. Cria uma tabela com dados para demonstração.

2. Cria uma sessão para um evento estendido existente, ou seja, **sqlserver.sql\_statement\_starting**.
 - O evento é limitado a instruções SQL que contêm uma determinada cadeia de caracteres de Atualização: **statement LIKE '%UPDATE tabEmployee%'**.
 - Escolhe enviar a saída do evento para um destino do tipo Buffer de Anéis, ou seja, **package0.ring\_buffer**.

3. Inicia a sessão de evento.

4. Emite algumas instruções SQL UPDATE simples.

5. Emite uma instrução SQL SELECT para recuperar a saída de evento do Buffer de Anéis.
 - **sys.dm\_xe\_database\_session\_targets** e outras exibições de gerenciamento dinâmico (DMVs) são ingressadas.

6. Interrompe a sessão de evento.

7. Descarta o destino do Buffer de Anéis, para liberar seus recursos.

8. Descarta a sessão de evento e a tabela de demonstração.


## Pré-requisitos


- Uma conta e uma assinatura do Azure. Você pode se inscrever para obter uma [avaliação gratuita](http://azure.microsoft.com/pricing/free-trial/).


- Qualquer banco de dados no qual você possa criar uma tabela.
 - Opcionalmente, você pode [criar um banco de dados de demonstração](sql-database-get-started.md) do **AdventureWorksLT** em minutos.


- SQL Server Management Studio (ssms.exe), sua Visualização de agosto de 2015 ou uma versão posterior. Você pode baixar o ssms.exe mais recente de:
 - [Um link no tópico.](http://msdn.microsoft.com/library/mt238290.aspx)
 - [Um link direto para o download.](http://go.microsoft.com/fwlink/?linkid=616025)
 - A Microsoft recomenda que você atualize seu ssms.exe periodicamente.


## Exemplo de código


Com modificações mínimas, o exemplo de código do Buffer de Anéis a seguir pode ser executado no Banco de Dados SQL do Azure ou no Microsoft SQL Server. A diferença é a presença do nó “\_database” no nome de algumas exibições de gerenciamento dinâmico (DMVs) na cláusula FROM na Etapa 5. Por exemplo:

- sys.dm\_xe**\_database**\_session\_targets
- sys.dm\_xe\_session\_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
	(SELECT * FROM sys.objects
		WHERE type = 'U' and name = 'tabEmployee')
BEGIN
	DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
	EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
	EmployeeId           int                not null  identity(1,1),
	EmployeeKudosCount   int                not null  default 0,
	EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
	VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
	(SELECT * from sys.database_event_sessions
		WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
	DROP EVENT SESSION eventsession_gm_azuresqldb51
		ON DATABASE;
END
GO


CREATE
	EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	ADD EVENT
		sqlserver.sql_statement_starting
			(
			ACTION (sqlserver.sql_text)
			WHERE statement LIKE '%UPDATE tabEmployee%'
			)
	ADD TARGET
		package0.ring_buffer
			(SET
				max_memory = 500   -- Units of KB.
			);
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
	SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
	SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
		se.name  AS [session-name],
		ev.event_name,
		ac.action_name,
		st.target_name,
		se.session_source,
		st.target_data,
		CAST(st.target_data AS XML)  AS [target_data_XML]
	FROM
				   sys.dm_xe_database_session_event_actions  AS ac
		INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name            = ac.event_name
		                                                           AND ev.event_session_address = ac.event_session_address
		INNER JOIN sys.dm_xe_database_session_object_columns AS oc  ON oc.event_session_address = ac.event_session_address
		INNER JOIN sys.dm_xe_database_session_targets        AS st  ON st.event_session_address = ac.event_session_address
		INNER JOIN sys.dm_xe_database_sessions               AS se  ON ac.event_session_address = se.address
	WHERE
		oc.column_name = 'occurrence_number'
		AND
		se.name        = 'eventsession_gm_azuresqldb51'
		AND
		ac.action_name = 'sql_text'
	ORDER BY
		se.name,
		ev.event_name,
		ac.action_name,
		st.target_name,
		se.session_source;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## Conteúdo do Buffer de Anéis


Nós usamos o ssms.exe para executar o exemplo de código.


Para exibir os resultados, clicamos na célula no título de coluna **target\_data\_XML**.

Em seguida, no painel de resultados, clicamos na célula no título de coluna **target\_data\_XML**. Isso criou outra guia de arquivo no ssms.exe em que o conteúdo da célula de resultado foi exibido, como XML.


A saída é mostrada no bloco a seguir. Parece longo, mas são apenas dois elementos **<event>**.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### Liberar os recursos mantidos pelo seu Buffer de Anéis


Quando concluir o Buffer de Anéis, é possível removê-lo e liberar seus próprios recursos emitindo um comando **ALTER** semelhante ao seguinte:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	DROP TARGET package0.ring_buffer;
GO
```


A definição de sua sessão de evento é atualizada, mas não descartada. Mais tarde, é possível adicionar outra instância do Buffer de Anéis à sessão de evento:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	ADD TARGET
		package0.ring_buffer
			(SET
				max_memory = 500   -- Units of KB.
			);
```


## Mais informações


O tópico principal para eventos estendidos no Banco de Dados SQL do Azure é:


- [Considerações sobre eventos estendidos no Banco de Dados SQL](sql-database-xevent-db-diff-from-svr.md), que é diferente em alguns aspectos dos eventos estendidos que diferem entre o Banco de Dados SQL do Azure e o Microsoft SQL Server.


Outros tópicos com exemplos de código para eventos estendidos estão disponíveis nos links a seguir. No entanto, você deve verificar regularmente os exemplos para ver se eles se destinam ao Microsoft SQL Server ou ao Banco de Dados SQL do Azure. Em seguida, você pode decidir se alterações mínimas são necessárias para realizar o exemplo.


- Exemplo de código do Banco de Dados SQL do Azure: [código de destino do Arquivo de Evento para eventos estendidos no Banco de Dados SQL](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->

<!---HONumber=Nov15_HO2-->