<properties
   pageTitle="Introdução: conectar-se ao SQL Data Warehouse do Azure| Microsoft Azure"
   description="Introdução ao SQL Data Warehouse e execução de algumas consultas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager=""
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/20/2015"
   ms.author="twounder"/>

# Conectar e consultar com o Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-data-warehouse-get-started-connect.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md)

Este passo a passo mostra como se conectar e consultar um banco de dados do SQL Data Warehouse do Azure em apenas alguns minutos usando o Visual Studio. Neste passo a passo, você vai:

+ Instalar software de pré-requisito
+ Conectar-se a um banco de dados que contém o banco de dados AdventureWorksDW
+ Executar uma consulta em relação o banco de dados de exemplo  

## Pré-requisitos

+ Visual Studio 2013/2015 - para baixar e instalar o Visual Studio 2015 e/ou SSDT, consulte [Instalar o Visual Studio e o SSDT](sql-data-warehouse-install-visual-studio.md)

## Obtenha o nome de servidor totalmente qualificado do SQL Azure

Para conectar-se ao banco de dados, você precisará do nome completo do servidor (****nomedoservidor**.database.windows.net*) que contenha o banco de dados ao qual você deseja se conectar.

1. Vá para o [Portal de Visualização do Azure](https://portal.azure.com).
2. Navegue até o banco de dados ao qual você deseja se conectar.
3. Localize o nome do servidor completo (vamos usá-lo nas etapas abaixo):

![][1]

## Conectar-se ao seu Banco de Dados SQL

1. Abra o Visual Studio.
2. Abra o **Explorador de Objetos do SQL Server** no menu Exibir
 
![][2]

3. Clique no botão **Adicionar SQL Server**

![][3]

4. Insira o *nome do servidor* que capturamos acima
5. Na lista **Autenticação**, selecione **Autenticação do SQL Server**.
6. Insira o **Login** e a **Senha** especificados durante a criação de seu servidor do Banco de Dados SQL e clique em **Conectar**.

## Executar consultas de exemplo

Agora que registramos o nosso servidor, vamos prosseguir e criar uma consulta.

1. Clique no banco de dados de usuário no SSDT.

2. Clique no botão **Nova Consulta**. Uma nova janela é aberta.

![][4]

3. Digite o seguinte código na janela de consulta:

	```
	SELECT COUNT(*) FROM dbo.FactInternetSales;
	```

4. Execute a consulta.

	Para executar a consulta, clique na seta verde ou use este atalho: `CTRL`+`SHIFT`+`E`:

## Próximas etapas

Agora que você pode se conectar e consultar, tente [conectar-se ao PowerBI][].

[conectar-se ao PowerBI]: ./sql-data-warehouse-integrate-power-bi.md


<!--Image references-->

[1]: ./media/sql-data-warehouse-get-started-connect/get-server-name.png
[2]: ./media/sql-data-warehouse-get-started-connect/open-ssdt.png
[3]: ./media/sql-data-warehouse-get-started-connect/connection-dialog.png
[4]: ./media/sql-data-warehouse-get-started-connect/new-query.png

<!---HONumber=Oct15_HO4-->