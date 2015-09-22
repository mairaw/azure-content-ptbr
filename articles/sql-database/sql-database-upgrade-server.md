<properties 
	pageTitle="Atualizar um servidor do SQL Azure para a V12 com o PowerShell" 
	description="Atualize um servidor do SQL Azure para a V12 com o PowerShell." 
	services="sql-database" 
	documentationCenter="" 
	authors="stevestein" 
	manager="jeffreyg" 
	editor=""/>

<tags 
	ms.service="sql-database" 
	ms.workload="data-management" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/05/2015" 
	ms.author="sstein"/>

# Atualizar o servidor do SQL Azure para a V12 com o PowerShell
 

Este artigo mostra como atualizar um servidor do Banco de Dados SQL para a V12 usando as recomendações de camada de preços e pool elástico.



## Pré-requisitos 

Para atualizar um servidor para a V12 com o PowerShell, você precisa ter o Azure PowerShell instalado e em execução e, dependendo da versão, talvez seja preciso alterná-lo para o modo de gerenciador de recursos, a fim de acessar os Cmdlets do PowerShell do Gerenciador de Recursos do Azure.

Você pode baixar e instalar o módulo PowerShell no Azure executando o [Microsoft Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409). Para obter informações detalhadas, confira [Como instalar e configurar o PowerShell do Azure](../powershell-install-configure.md).

Os cmdlets para criação e gerenciamento de Bancos de Dados SQL do Azure estão localizados no módulo do Gerenciador de Recursos do Azure. Quando você inicia o PowerShell do Azure, os cmdlets no módulo do Azure são importados por padrão. Para alternar para o módulo do Gerenciador de Recursos do Azure, use o cmdlet **Switch-AzureMode**.

	Switch-AzureMode -Name AzureResourceManager

Se for exibido um aviso informando “O cmdlet Switch-AzureMode foi substituído e será removido em uma versão futura”, você poderá ignorá-lo e passar para a próxima seção.

Para obter informações detalhadas, confira [Usando o Windows PowerShell com o Gerenciador de Recursos](../powershell-azure-resource-manager.md).



## Configurar credenciais

Para executar os cmdlets do PowerShell em sua assinatura do Azure, primeiramente você deve estabelecer acesso à sua conta do Azure. Execute o seguinte e você receberá um sinal na tela para inserir suas credenciais. Use o mesmo email e senha usados para entrar no Portal do Azure.

	Add-AzureAccount

Após entrar, você verá algumas informações na tela, incluindo a ID usada para entrar e as assinaturas do Azure as quais você tem acesso.


## Selecionar sua assinatura do Azure

Para selecionar a assinatura com a qual deseja trabalhar, você precisa da ID (**-SubscriptionId**) ou do nome da assinatura (**-SubscriptionName**). Você pode copiar essas informações da etapa anterior, ou, se tiver várias assinaturas, poderá executar o cmdlet **Get-AzureSubscription** e copiar as informações da assinatura desejada do resultado.

Execute o cmdlet a seguir com as informações da sua assinatura para definir a assinatura atual:

	Select-AzureSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Os comandos a seguir serão executados na assinatura que você acabou de selecionar acima.

## Obter recomendações

Para obter a recomendação para a atualização do servidor, execute o seguinte cmdlet:

    $hint = Get-AzureSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1” 

Para obter mais informações, confira [as recomendações de pool de banco de dados elástico do Banco de Dados SQL do Azure](sql-database-elastic-pool-portal.md#elastic-database-pool-pricing-tier-recommendations) e [as recomendações de camada de preços do Banco de Dados SQL](sql-database-service-tier-advisor.md).



## Inicie a atualização

Para iniciar a atualização do servidor, execute o seguinte cmdlet:

    Start-AzureSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


Quando você executar este comando, o processo de atualização será iniciado. Você pode personalizar a saída da recomendação e fornecer a recomendação editada para esse cmdlet.


## Atualizar um servidor do SQL Azure


    # Adding the account
    #
    Add-AzureAccount
    Switch-AzureMode -Name AzureResourceManager

    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION' 
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP' $ServerName = 'YOUR_SERVER' 
    
    # Selecting the right subscription 
    # 
    Select-AzureSubscription $SubscriptionName 
    
    # Getting the upgrade recommendations 
    #
    $hint = Get-AzureSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    
    # Starting the upgrade process 
    #
    Start-AzureSqlServerUpgrade -ResourceGroupName 
    $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## Mapeamento de atualização personalizada

Se as recomendações não forem apropriadas para seu servidor e caso comercial, você poderá escolher o modo como seus bancos de dados são atualizados e mapeá-los a bancos de dados simples ou elásticos.

Atualizar bancos de dados em um pool de banco de dados elástico:

    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties
    $elasticPool.DatabaseDtuMax = 100  
    $elasticPool.DatabaseDtuMin = 0  
    $elasticPool.Dtu = 800
    $elasticPool.Edition = "Standard"  
    $elasticPool.DatabaseCollection = ("DB1")  
    $elasticPool.Name = "elasticpool_1"  


Atualize bancos de dados em um único banco de dados:

    $databaseMap = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties  $databaseMap.Name = "DB2"
    $databaseMap.TargetEdition = "Standard"
    $databaseMap.TargetServiceLevelObjective = "S0"
    Start-AzureSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -Version 12.0 -DatabaseCollection($databaseMap) -ElasticPoolCollection ($elasticPool)
    




## Informações relacionadas

- [Cmdlets do Gerenciador de Recursos do Banco de Dados SQL do Azure](https://msdn.microsoft.com/library/mt163521.aspx)
- [Cmdlets do Gerenciamento de Serviço do Banco de Dados SQL do Azure](https://msdn.microsoft.com/library/dn546726.aspx)
 

<!---HONumber=Sept15_HO2-->