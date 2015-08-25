<properties 
	pageTitle="Mover dados para um banco de dados do SQL Azure para Aprendizado de Máquina do Azure | Azure" 
	description="Criar tabela SQL e carregar dados para a tabela SQL" 
	services="machine-learning" 
	documentationCenter="" 
	authors="fashah" 
	manager="jacob.spoelstra" 
	editor="" />

<tags 
	ms.service="machine-learning" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/10/2015" 
	ms.author="fashah;bradsev" />

# Mover dados para um banco de dados do SQL Azure para aprendizado de máquina do Azure


Neste tópico, resumimos as opções para movimentação de dados de arquivos simples (formatos CSV ou TSV) ou de dados armazenados em um SQL Server local para um banco de dados do SQL Azure. Essas tarefas para mover dados para a nuvem fazem parte do Processo de Análise Avançada e Tecnologia (ADAPT) fornecida pelo Aprendizado de Máquina do Azure.

Para um tópico que descreve as opções para mover dados para um SQL Server local para o aprendizado de máquina, consulte [Mover dados para o SQL Server em uma máquina virtual do Azure](machine-learning-data-science-move-sql-server-virtual-machine.md).

A tabela a seguir resume as opções para mover dados para um banco de dados do SQL Azure. <table>

<tr>
<td><b>ORIGEM</b></td>
<td colspan="2"><b>DESTINO: Banco de dados SQL Azure</b></td>
</tr>

<tr>
  <td><b>Arquivo simples (CSV ou TSV formatado)</b></td>  

  <td>
    1. <a href="#bulk-insert-sql-query">Consulta SQL de inserção em massa
  </td>
</tr>

<tr>
  <td><b>SQL Server local</b></td>

  <td>
    1. <a href="#export-flat-file">Exportar para Arquivo simples<br>
    2. <a href="#insert-tables-bcp">Assistente de migração de Banco de Dados SQL<br>
    3. <a href="#db-migration">Banco de dados de backup e restauração<br>
    4. <a href="#adf">Azure Data Factory (ADF)
  </td>
</tr>

</table>


## <a name="prereqs"></a>Pré-requisitos
Esse procedimento descrito aqui requer que você tenha:

* Uma **assinatura do Azure**. Se você não tiver uma assinatura, você pode se inscrever em uma [avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/).
* Uma **conta de armazenamento do Azure**. Você usará uma conta de armazenamento do Azure para armazenar os dados neste tutorial. Se você não tiver uma conta de armazenamento do Azure, consulte o artigo [Criar uma conta de armazenamento](storage-create-storage-account.md#create-a-storage-account). Depois de criar a conta de armazenamento, você precisará obter a chave de conta usada para acessar o armazenamento. Consulte [Exibir, copiar e regenerar chaves de acesso de armazenamento](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Acesso a um **Banco de dados do SQL Azure**. Se você deve configurar um banco de dados do SQL Azure, a [Guia de Introdução ao banco de dados SQL do Microsoft Azure](sql-database-get-started.md) fornece informações sobre como provisionar uma nova instância de um banco de dados do SQL Azure.
* **Azure PowerShell** instalado e configurado localmente. Para saber mais, confira [Como instalar e configurar o PowerShell do Azure](powershell-install-configure.md).

**Dados**: os processos de migração são demonstrados usando o [Conjunto de Dados de Táxi de NYC](http://chriswhong.com/open-data/foil_nyc_taxi/). O conjunto de Dados de Táxi de NYC contém informações sobre dados de viagem e feiras e está disponível, conforme observado nessa postagem, no armazenamento de blob do Azure [aqui](http://www.andresmh.com/nyctaxitrips/). Um exemplo e uma descrição desses arquivos são fornecidos na [Descrição do Conjunto de Dados de Viagens de Táxi de NYC](machine-learning-data-science-process-sql-walkthrough.md#dataset).
 
Você pode adaptar os procedimentos descritos aqui para um conjunto de seus próprios dados ou seguir as etapas conforme descrito usando o conjunto de dados de Táxi de NYC. Para carregar o conjunto de dados de Táxi de NYC em seu banco de dados do SQL Server local, execute o procedimento descrito em [Importação de dados em massa para o Banco de Dados do SQL Server](machine-learning-data-science-process-sql-walkthrough.md#dbload). Essas instruções são para um SQL Server em uma máquina Virtual do Azure, mas o procedimento para carregar o SQL Server no local é o mesmo.

## <a name="file-to-azure-sql-database"></a>Movendo dados de uma fonte de arquivo simples para um banco de dados do SQL Azure

Dados em arquivos simples (CSV ou TSV formatado) podem ser movidos para um banco de dados do SQL Azure usando uma consulta SQL de inserção em massa.

### <a name="bulk-insert-sql-query"></a>Consulta SQL de inserção em massa

As etapas para o procedimento usando a consulta SQL de inserção em massa são semelhantes àquelas abordados nas seções para mover dados de uma fonte de arquivo simples para o SQL Server em uma VM do Azure. Para obter detalhes, consulte [Inserção em massa de consulta SQL](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery).

##<a name="sql-on-prem-to-sazure-sql-database"></a>Movimentação de dados do SQL Server local para um banco de dados do SQL Azure

Se a fonte de dados é armazenada em um SQL Server local, existem várias possibilidades para mover os dados para um banco de dados do SQL Azure:

1. [Exportar para arquivo simples](#export-flat-file) 
2. [Assistente de Migração de Banco de Dados SQL](#insert-tables-bcp)
3. [Backup e restauração de banco de dados](#db-migration)
4. [Azure Data Factory](#adf)

As etapas para as três primeiras são muito semelhantes às seções em [Mover dados para o SQL Server em uma máquina virtual do Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) que abordam esses mesmos procedimentos. Os links para as seções apropriadas sobre este tópico são fornecidos abaixo.

###<a name="export-flat-file"></a>Exportar para arquivo simples

As etapas para essa exportação para um arquivo simples são semelhantes a aquelas tratadas em [Exportar para arquivo simples](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file).

###<a name="insert-tables-bcp"></a>Assistente de Migração de Banco de Dados SQL

As etapas para usar o Assistente de Migração de Banco de Dados SQL são semelhantes àquelas tratadas no [Assistente de Migração de Banco de Dados SQL](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration).

###<a name="db-migration"></a>Backup e restauração de banco de dados

As etapas para usar o backup e restauração de banco de dados são semelhantes àquelas tratadas em [Backup e restauração de banco de dados](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup).

###<a name="adf"></a>Azure Data Factory (ADF)

O procedimento para mover dados para um banco de dados do SQL Azure com o Azure Data Factory (ADF) é fornecido no tópico [Mover dados de um SQL Server local para o SQL Azure com o Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md). Este tópico mostra como mover dados de um banco de dados do SQL Server local para um banco de dados do SQL Azure por meio do armazenamento de blob do Azure usando o ADF.

Considere usar o ADF quando dados precisam ser migrados continuamente em um cenário híbrido que acessa os recursos locais e de nuvem e quando os dados são transacionados ou precisam ser modificados ou tiver lógica de negócios adicionada a eles durante a migração. O ADF permite o planejamento e monitoramento de trabalhos usando scripts simples de JSON que gerenciam a movimentação de dados em intervalos periódicos. O ADF também possui outros recursos, como suporte para operações complexas.

<!---HONumber=August15_HO7-->