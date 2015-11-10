<properties 
   pageTitle="Introdução ao Repositório Data Lake | Azure" 
   description="Usar o Azure PowerShell para criar uma conta do Repositório Data Lake e executar operações básicas" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/28/2015"
   ms.author="nitinme"/>

# Introdução ao Repositório Azure Data Lake usando o Azure PowerShell

> [AZURE.SELECTOR]
- [Using Portal](data-lake-store-get-started-portal.md)
- [Using PowerShell](data-lake-store-get-started-powershell.md)
- [Using .NET SDK](data-lake-store-get-started-net-sdk.md)

Saiba como usar o Azure PowerShell para criar uma conta do Repositório Azure Data Lake e executar operações básicas, como criar pastas, carregar e baixar arquivos de dados, excluir sua conta, etc. Para obter mais informações sobre o Repositório Data Lake, veja [Visão geral do Repositório Data Lake](data-lake-store-overview.md).

## Pré-requisitos

Antes de começar este tutorial, você deve ter o seguinte:

- **Uma assinatura do Azure**. Consulte [Obter avaliação gratuita do Azure](https://azure.microsoft.com/pt-BR/pricing/free-trial/).
- **Habilite sua assinatura do Azure** para a visualização pública do Repositório Data Lake. Veja [instruções](data-lake-store-get-started-portal.md#signup).
- **Azure PowerShell 1.0 ou posterior**. Veja [Instalar e configurar o Azure PowerShell](../install-configure-powershell.md) para obter instruções. Depois de instalar o Azure PowerShell 1.0 ou superior, você deve executar o cmdlet a seguir para instalar o módulo do Repositório Azure Data Lake.

		Install-Module AzureRM.DataLakeStore

	Para obter mais informações sobre o módulo **AzureRM.DataLakeStore**, veja [Galeria do PowerShell](http://www.powershellgallery.com/packages/AzureRM.DataLakeStore).

## Criar uma conta do Repositório Azure Data Lake

1. Na área de trabalho, abra uma nova janela do Azure PowerShell e insira o trecho de código a seguir para fazer logon em sua conta do Azure, definir a assinatura e registrar o provedor do Repositório Data Lake. Quando solicitado a fazer logon, lembre-se de fazer logon como o proprietário ou um dos administradores da assinatura:

        # Log in to your Azure account
		Login-AzureRmAccount
        
		# List all the subscriptions associated to your account
		Get-AzureRmSubscription
		
		# Select a subscription 
		Set-AzureRmContext -SubscriptionId <subscription ID>
        
		# Register for Azure Data Lake Store
		Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore" 


2. Uma conta do Repositório Azure Data Lake está associada a um Grupo de Recursos do Azure. Comece criando um Grupo de Recursos do Azure.

		$resourceGroupName = "<your new resource group name>"
    	New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

	![Criar um grupo de recursos do Azure](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Criar um grupo de recursos do Azure")

2. Crie uma conta do Repositório Azure Data Lake. O nome especificado deve conter apenas letras minúsculas e números.

		$dataLakeStoreName = "<your new Data Lake Store name>"
    	New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

	![Criar uma conta do Repositório Azure Data Lake](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Criar uma conta do Repositório Azure Data Lake")

3. Verifique se a conta foi criada com êxito.

		Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

	A saída para isso deve ser **True**.

## Criar estruturas de diretório no Repositório Azure Data Lake

Você pode criar diretórios em sua conta do Repositório Azure Data Lake para gerenciar e armazenar dados.

1. Especifique um diretório raiz.

		$myrootdir = "/"

2. Criar um novo diretório chamado **mynewdirectory** na raiz especificada.

		New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Verifique se o novo diretório foi criado com êxito.

		Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

	Ele deve mostrar uma saída semelhante à seguinte:

	![Verificar diretório](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verificar diretório")


## Carregar dados no Repositório Azure Data Lake

É possível carregar seus dados no Repositório Data Lake diretamente no nível da raiz ou em um diretório que você criou na conta. Os trechos de código a seguir demonstram como carregar alguns dados de exemplo no diretório (**mynewdirectory**) criado na seção anterior.

Se estiver procurando alguns dados de exemplo para carregar, é possível obter a pasta **Dados da Ambulância** no [Repositório Git do Azure Data Lake](https://github.com/MicrosoftBigData/ProjectKona/tree/master/SQLIPSamples/SampleData/AmbulanceData). Baixe o arquivo e armazene-o em um diretório local no computador, como C:\\sampledata.

	Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## Renomear, baixar e excluir dados do Repositório Data Lake

Para renomear um arquivo, use o seguinte comando:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Para baixar um arquivo, use o seguinte comando:

	Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Para excluir um arquivo, use o seguinte comando:

	Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv 
	
Quando solicitado, insira **Y** para excluir o item. Se você tiver mais de um arquivo para excluir, você pode fornecer todos os caminhos separados por vírgula.

	Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## Excluir sua conta do Repositório Azure Data Lake

Use o comando a seguir para excluir sua conta do Repositório Data Lake.

	Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Quando solicitado, insira **Y** para excluir a conta.


## Outras maneiras de criar uma conta do Repositório Data Lake

- [Introdução ao Repositório Data Lake usando o Portal](data-lake-store-get-started-portal.md)
- [Introdução ao Repositório Data Lake usando o SDK do .NET](data-lake-store-get-started-net-sdk.md)


## Próximas etapas

- [Proteger dados no Repositório Data Lake](data-lake-store-secure-data.md)
- [Usar a Análise Azure Data Lake com o Repositório Data Lake](data-lake-analytics-get-started-portal.md)
- [Usar o Azure HDInsight com o Repositório Data Lake](data-lake-store-hdinsight-hadoop-use-portal.md)

<!---HONumber=Nov15_HO1-->