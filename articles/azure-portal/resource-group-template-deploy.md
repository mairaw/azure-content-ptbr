<properties
   pageTitle="Implantar um aplicativo com o modelo do Gerenciador de Recursos do Azure"
   services="azure-portal"
   description="Use o Gerenciador de recursos do Azure para implantar um aplicativo no Azure. Um modelo é um arquivo JSON e pode ser usado do Portal, PowerShell, a Interface de linha de comando do Azure para Mac, Linux e Windows ou REST."
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-portal"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/22/2015"
   ms.author="tomfitz"/>

# Implantar um aplicativo com o modelo do Gerenciador de Recursos do Azure

Este tópico explica como usar modelos do Gerenciador de recursos do Azure para implantar seu aplicativo no Azure. Ele mostra como implantar seu aplicativo usando o PowerShell do Azure, a CLI do Azure, a API REST ou o portal de visualização do Microsoft Azure.

Os modelos de Gerenciador de recursos do Azure permitem o provisionamento de forma rápida e fácil de seus aplicativos no Azure via JSON declarativa. Em um único modelo JSON, você pode implantar vários serviços, como máquinas virtuais, redes virtuais, armazenamento, serviços de aplicativos e bancos de dados. Use o mesmo modelo para implantar repetida e consistentemente seu aplicativo durante todos os estágios do ciclo de vida do aplicativo.

Para simplificar o gerenciamento de seu aplicativo, você pode organizar todos os recursos que compartilham um ciclo de vida comum em um grupo único recurso. Os grupos de recursos tornam mais fácil de implantar, atualizar e excluir todos os recursos relacionados juntos. Na maioria dos casos, um grupo de recursos é mapeado para um único aplicativo ou uma camada de aplicativo (para aplicativos grandes). O recurso implantado por meio de um modelo ficará dentro de um único grupo de recurso, mas pode incluir dependências em outros grupos de recursos.

Dentro de um grupo de recursos, você pode acompanhar a execução de uma implantação e ver o status da implantação e qualquer saída de execução do modelo.

Ao implantar um aplicativo com um modelo, você pode fornecer valores de parâmetro para personalizar como os recursos são criados. Você especifica valores para esses parâmetros embutidos ou em um arquivo de parâmetros.

## Conceitos

- Grupo de recursos - conjunto de entidades que compartilham um ciclo de vida comum
- Modelo do Gerenciador de recursos - arquivo JSON declarativo que define o estado da meta de uma implantação
- Implantação - operação que acompanha a execução de um modelo do Gerenciador de recursos
- Parâmetros - valores fornecido pelo usuário que está executando a implantação para personalizar recursos implantados
- Arquivo de parâmetro - arquivo JSON que armazena nomes e valores de parâmetros 

## Cenários

Com os modelos Gerenciador de recursos, você pode:

- Implantar aplicativos complexos de várias camadas, como o Microsoft SharePoint.
- Implantar, de forma consistente e repetida, seus aplicativos.
- Suporte a ambientes de desenvolvimento, teste e produção.
- Exibir o status das implantações.
- Solucionar problemas de falhas de implantação usando os logs de auditoria de implantação.

## Implantar com o portal de visualização

E adivinhe? Todos os aplicativos na Galeria são respaldados por um modelo do Gerenciador de recursos do Azure! Ao simplesmente criar uma Máquina Virtual, Rede Virtual, Conta de Armazenamento, Serviço de Aplicativo ou banco de dados por meio do portal, você já está colhendo os benefícios do Gerenciador de recursos do Azure sem muito esforço.

Para solucionar problemas de implantações por meio do portal de visualização, clique em **Procurar** -> **grupos de recursos** -> *YourResourceGroupName*. Aqui, clique no bloco **Eventos**, sob a lente **Monitoramento**. Finalmente, você pode selecionar uma **operação** e **evento** individual para exibir os detalhes.

## Implantação com o PowerShell

Se você ainda não utilizou o PowerShell do Azure com o Gerenciador de recursos, consulte [Uso do PowerShell do Azure com o Gerenciador de recursos do Azure](../powershell-azure-resource-manager.md).

1. Faça logon em sua conta do Azure. Depois de fornecer suas credenciais, o comando retornará informações sobre sua conta.

        PS C:\> Add-AzureAccount

        Id                             Type       ...
        --                             ----    
        someone@example.com            User       ...   

2. Se você tiver várias assinaturas, forneça a ID da assinatura que deseja usar para implantação.

        PS C:\> Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. Alterne para o módulo Gerenciador de Recursos do Azure.

        PS C:\> Switch-AzureMode AzureResourceManager

4. Se você não tiver um grupo de recursos existente, crie um novo grupo de recursos. Forneça o nome do grupo de recursos e o local necessários para sua solução. Um resumo do novo grupo de recursos é retornado.

        PS C:\> New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. Para criar uma nova implantação para seu grupo de recursos, execute o comando **New-AzureResourceGroupDeployment** e forneça os parâmetros necessários. Os parâmetros incluirão um nome para sua implantação, o nome do seu grupo de recursos, o caminho ou a URL para o modelo criado e qualquer outro parâmetro necessário para seu cenário.
   
     Você tem as seguintes opções para fornecer valores de parâmetro:
   
     - Use parâmetros embutidos.

            PS C:\> New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

     - Use um objeto de parâmetro.

            PS C:\> $parameters = @{"<ParameterName>"="<Parameter Value>"}
            PS C:\> New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

     - Usando um arquivo de parâmetro. Para obter informações sobre o arquivo de modelo, consulte [Arquivo de parâmetro](./#parameter-file).

            PS C:\> New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

     Quando o grupo de recursos tiver sido implantado, você verá um resumo da implantação.

          DeploymentName    : ExampleDeployment
          ResourceGroupName : ExampleResourceGroup
          ProvisioningState : Succeeded
          Timestamp         : 4/14/2015 7:00:27 PM
          Mode              : Incremental
          ...

6. Para obter informações sobre falhas de implantação.

        PS C:\> Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. Para obter informações detalhadas sobre falhas de implantação.

        PS C:\> Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput

## Implantar a CLI do Azure para Mac, Linux e Windows

Se você não utilizou anteriormente a CLI do Azures com o Gerenciamento de Recursos, consulte [Usando a CLI do Azure para Mac, Linux e Windows e Windows com o Gerenciamento de Recursos do Azure](../xplat-cli-azure-resource-manager.md).

1. Faça logon em sua conta do Azure. Depois de fornecer suas credenciais, o comando retornará o resultado do seu logon.

        azure login
  
        ...
        info:    login command OK

2. Se você tiver várias assinaturas, forneça a ID da assinatura que deseja usar para implantação.

        azure account set <YourSubscriptionNameOrId>

3. Alternar para o módulo do Gerenciador de Recursos do Azure. Você receberá a confirmação do novo modo.

        azure config mode arm
   
        info:     New mode is arm

4. Se você não tiver um grupo de recursos existente, crie um novo grupo de recursos. Forneça o nome do grupo de recursos e o local necessários para sua solução. Um resumo do novo grupo de recursos é retornado.

        azure group create -n ExampleResourceGroup -l "West US"
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Para criar uma nova implantação para seu grupo de recursos, execute o comando a seguir e ofereça os parâmetros necessários. Os parâmetros incluirão um nome para sua implantação, o nome do seu grupo de recursos, o caminho ou a URL para o modelo criado e qualquer outro parâmetro necessário para seu cenário.
   
     Você tem as seguintes opções para fornecer valores de parâmetro:

     - Use parâmetros embutidos e um modelo local.

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

     - Use parâmetros embutidos e um link para um modelo.

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

     - Use um arquivo de parâmetro. Para obter informações sobre o arquivo de modelo, consulte [Arquivo de parâmetro](./#parameter-file).
    
             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Quando o grupo de recursos tiver sido implantado, você verá um resumo da implantação.
  
           info:    Executing command group deployment create
           + Initializing template configurations and parameters
           + Creating a deployment
           ...
           info:    group deployment create command OK


6. Para obter informações sobre sua implantação mais recente.

         azure group log show -l ExampleResourceGroup

7. Para obter informações detalhadas sobre falhas de implantação.
      
         azure group log show -l -v ExampleResourceGroup

## Implantar com a API REST
1. Definir [Parâmetros e cabeçalhos comuns](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common), incluindo tokens de autenticação.
2. Se você não tiver um grupo de recursos existente, crie um novo grupo de recursos. Forneça sua id de assinatura, o nome do novo grupo de recursos e local que você precisa para sua solução. Para obter mais informações, consulte [Criar um grupo de recursos](https://msdn.microsoft.com/library/azure/dn790525.aspx).

         PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
           <common headers>
           {
             "location": "West US",
             "tags": {
               "tagname1": "tagvalue1"
             }
           }
   
3. Criar um novo grupo de recursos. Forneça seu id de assinatura, o nome do grupo dos recursos para a serem implantados, o nome da implantação e o local do seu modelo. Para obter informações sobre o arquivo de modelo, consulte [Arquivo de parâmetro](./#parameter-file). Para obter mais informações sobre a API REST para criar um grupo de recursos, consulte [Criar uma implantação de modelo](https://msdn.microsoft.com/library/azure/dn790564.aspx).
    
         PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
            <common headers>
            {
              "properties": {
                "templateLink": {
                  "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                  "contentVersion": "1.0.0.0",
                },
                "mode": "Incremental",
                "parametersLink": {
                  "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                  "contentVersion": "1.0.0.0",
                }
              }
            }
   
4. Obtém o status da implantação do modelo. Para obter mais informações, consulte [obter informações sobre uma implantação de modelo](https://msdn.microsoft.com/library/azure/dn790565.aspx).

         GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

## Arquivo de parâmetro.

Se você usar um arquivo de parâmetro para passar os valores de parâmetro para seu modelo durante a implantação, será necessário criar um arquivo JSON com um formato semelhante ao exemplo a seguir.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "webSiteName": {
                "value": "ExampleSite"
            },
            "webSiteHostingPlanName": {
                "value": "DefaultPlan"
            },
            "webSiteLocation": {
                "value": "West US"
            }
       }
    }

## Próximas etapas
- [Visão Geral do Gerenciador de Recursos do Azure](../resource-group-overview.md)
- [Implantar dos recursos do Azure usando bibliotecas .NET e um modelo](../arm-template-deployment.md)
- [Implantar um aplicativo complexo de modo previsível no Azure](../app-service-web/app-service-deploy-complex-application-predictably.md)
- [Criação de modelos](../resource-group-authoring-templates.md)
- [Funções de modelo](../resource-group-template-functions.md)
- [Operações avançadas de modelo](../resource-group-advanced-template.md)  

 

<!---HONumber=62-->