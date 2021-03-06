<properties
   pageTitle="Criando modelos do Gerenciador de Recursos do Azure"
   description="Crie modelos do Gerenciador de Recursos do Azure usando sintaxe JSON declarativa para implantar aplicativos no Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/07/2015"
   ms.author="tomfitz"/>

# Criando modelos do Gerenciador de Recursos do Azure

Normalmente, os aplicativos do Azure exigem uma combinação de recursos (como servidor de banco de dados, banco de dados ou site) para cumprir as metas desejadas. Em vez de implantar e gerenciar cada recurso separadamente, você pode criar um modelo do Gerenciador de Recursos do Azure que implanta e provisiona todos os recursos para seu aplicativo em uma única operação coordenada. No modelo, você define os recursos que são necessários para o aplicativo e especifica os parâmetros de implantação para inserir valores para ambientes diferentes. O modelo consiste em JSON e expressões que podem ser usados na construção de valores para sua implantação.

Este tópico descreve as seções do modelo. Para os esquemas reais, consulte [Esquemas do Gerenciador de Recursos do Azure](https://github.com/Azure/azure-resource-manager-schemas). O Visual Studio fornece ferramentas para ajudá-lo com a criação de modelos. Para obter mais informações sobre como usar o Visual Studio com seus modelos, consulte [Criar e implantar grupos de recursos do Azure com o Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) e modelos [do Gerenciador de Recursos de Edição com o Visual Studio](vs-azure-tools-resource-group-adding-resources.md).

Você deve limitar o tamanho de seu modelo para 1 MB, e cada arquivo de parâmetro para 64 KB. O limite de 1 MB se aplica para o estado final do modelo depois que ele foi expandido com definições de recurso iterativo e valores para variáveis e parâmetros.

## Formato de modelo

O exemplo a seguir mostra as seções que compõem a estrutura básica de um modelo.

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| Nome do elemento | Obrigatório | Descrição
| :------------: | :------: | :----------
| $schema | Sim | Local do arquivo de esquema JSON que descreve a versão da linguagem do modelo.
| contentVersion | Sim | Versão do modelo (como 1.0.0.0). Ao implantar recursos com o modelo, esse valor pode ser usado para garantir que o modelo certo esteja sendo usado.
| parameters | Não | Valores que são fornecidos quando a implantação é executada para personalizar a implantação dos recursos.
| variáveis | Não | Valores que são usados como fragmentos JSON no modelo para simplificar expressões de linguagem do modelo.
| recursos | Sim | Tipos de serviço que são implantados ou atualizados em um grupo de recursos.
| outputs | Não | Valores que são retornados após a implantação.

Examinaremos as seções do modelo em detalhes mais adiante neste tópico. Por enquanto, analisaremos parte da sintaxe que compõe o modelo.

## Expressões e funções

A sintaxe básica do modelo é JSON. No entanto, expressões e funções estendem o JSON que está disponível no modelo e permitem criar valores que não são valores literais rígidos. As expressões são colocadas entre colchetes ([ e ]) e avaliadas quando o modelo é implantado. As expressões podem aparecer em qualquer lugar em um valor de cadeia de caracteres JSON e sempre retornam outro valor JSON. Se precisar usar uma cadeia de caracteres literal que comece com um colchete [, você deverá usar dois colchetes [[.

Normalmente, você usa expressões com funções para executar operações e configurar a implantação. Assim como no JavaScript, as chamadas de função são formatadas como **functionName(arg1,arg2,arg3)**. Você faz referência às propriedades usando os operadores dot e [index].

O seguinte exemplo mostra como usar várias das funções ao construir valores:
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

Para obter a lista completa das funções de modelo, veja [Funções de modelo do Gerenciador de Recursos do Azure](./resource-group-template-functions.md).


## Parâmetros

Na seção de parâmetros do modelo, você deve especificar os valores que um usuário pode inserir ao implantar os recursos. Você pode usar esses valores de parâmetro em todo o modelo para definir valores para os recursos implantados. Somente os parâmetros que são declarados na seção de parâmetros podem ser usados em outras seções do modelo.

Na seção de parâmetros, é possível usar um valor de parâmetro para construir outro valor de parâmetro. Esse tipo de operação geralmente ocorre na seção de variáveis.

Você define parâmetros com a seguinte estrutura:

    "parameters": {
       "<parameterName>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<optional-default-value-of-parameter>",
         "allowedValues": [ "<optional-array-of-allowed-values>" ],
         "minValue": <optional-minimum-value-for-int-parameters>,
         "maxValue": <optional-maximum-value-for-int-parameters>,
         "minLength": <optional-minimum-length-for-string-secureString-array-parameters>,
         "maxLength": <optional-maximum-length-for-string-secureString-array-parameters>,
         "metadata": {
             "description": "<optional-description-of-the parameter>" 
         }
       }
    }

| Nome do elemento | Obrigatório | Descrição
| :------------: | :------: | :----------
| parameterName | Sim | Nome do parâmetro. Deve ser um identificador JavaScript válido.
| type | Sim | Tipo do valor do parâmetro. Consulte a lista de tipos permitidos abaixo.
| defaultValue | Não | Valor padrão do parâmetro, se nenhum valor for fornecido para o parâmetro.
| allowedValues | Não | Matriz de valores permitidos para o parâmetro para garantir que o valor correto seja fornecido.
| minValue | Não | O valor mínimo para parâmetros de tipo int, esse valor é inclusivo.
| maxValue | Não | O valor máximo para parâmetros de tipo int, esse valor é inclusivo.
| minLength | Não | O comprimento mínimo para cadeia, secureString e parâmetros do tipo de matriz, esse valor é inclusivo.
| maxLength | Não | O comprimento máximo para cadeia, secureString e parâmetros do tipo de matriz, esse valor é inclusivo.
| description | Não | Descrição do parâmetro que será exibida aos usuários do modelo por meio da interface do modelo personalizado do portal.

Os valores e tipos permitidos são:

- string ou secureString - qualquer cadeia de caracteres JSON válida
- int - qualquer inteiro JSON válido
- bool - qualquer booliano JSON válido
- object ou secureObject - qualquer objeto JSON válido
- array - qualquer matriz JSON válida

Para especificar um parâmetro como opcional, defina seu defaultValue como uma cadeia de caracteres vazia.

>[AZURE.NOTE]Todas as senhas, chaves e outros segredos devem usar o tipo **secureString**. Os parâmetros do modelo com o tipo secureString não podem ser lidos após a implantação de recursos.

O seguinte exemplo mostra como definir parâmetros:

    "parameters": {
       "siteName": {
          "type": "string",
          "minLength": 2,
          "maxLength": 60
       },
       "siteLocation": {
          "type": "string",
          "minLength": 2
       },
       "hostingPlanName": {
          "type": "string"
       },  
       "hostingPlanSku": {
          "type": "string",
          "allowedValues": [
            "Free",
            "Shared",
            "Basic",
            "Standard",
            "Premium"
          ],
          "defaultValue": "Free"
       },
       "instancesCount": {
          "type": "int",
          "maxValue": 10
       },
       "numberOfWorkers": {
          "type": "int",
          "minValue": 1
       }
    }

## Variáveis

Na seção de variáveis, você constroi valores que podem ser usados para simplificar expressões de idioma do modelo. Normalmente, essas variáveis serão baseadas em valores fornecidos pelos parâmetros.

Você define variáveis com a seguinte estrutura:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

O seguinte exemplo mostra como definir uma variável que é construída com base em dois valores de parâmetro:

    "parameters": {
       "username": {
         "type": "string"
       },
       "password": {
         "type": "secureString"
       }
     },
     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

O próximo exemplo mostra uma variável que é um tipo JSON complexo e variáveis que são construídas com base em outras variáveis:

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount"
    }

## Recursos

Na seção de recursos, você define os recursos que são implantados ou atualizados.

Você define recursos com a seguinte estrutura:

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "resources": [
           "<array-of-dependent-resources>"
         ]
       }
    ]

| Nome do elemento | Obrigatório | Descrição
| :----------------------: | :------: | :----------
| apiVersion | Sim | Versão da API REST a ser usada para criar o recurso. Para determinar os números de versão disponíveis para um tipo de recurso específico, confira [Versões da API com suporte](../resource-manager-supported-services/#supported-api-versions).
| type | Sim | Tipo do recurso. Esse valor é uma combinação do namespace do provedor de recursos e do tipo de recurso que tem suporte do provedor de recursos.
| name | Sim | Nome do recurso. O nome deve seguir as restrições de componente URI definidas em RFC3986.
| location | Não | Locais geográficos com suporte do recurso fornecido.
| marcas | Não | Marcas que são associadas ao recurso.
| comentários | Não | Suas anotações para documentar os recursos em seu modelo
| dependsOn | Não | Recursos dos quais o recurso que está sendo definido depende. As dependências entre recursos são avaliadas e os recursos são implantados na ordem de dependência. Quando os recursos não dependem uns dos outros, é possível que tentem ser implantados paralelamente. O valor pode ser uma lista separada por vírgulas de nomes de recursos ou identificadores exclusivos de recursos.
| propriedades | Não | Definições de configuração específicas do recurso.
| recursos | Não | Recursos filho que dependem do recurso que está sendo definido. Você pode fornecer apenas os tipos de recurso permitidos pelo esquema do recurso pai. O nome totalmente qualificado do tipo de recurso filho inclui o tipo de recurso pai, como **Microsoft.Web/sites/extensions**.

Se o nome do recurso não for exclusivo, você poderá usar a função auxiliar **resourceId** (descrita abaixo) para obter o identificador exclusivo de qualquer recurso.

Os valores para o elemento **properties** são exatamente iguais aos valores que você fornece no corpo da solicitação para a operação da API REST (método PUT) para criar o recurso. Consulte [Referência do Azure](https://msdn.microsoft.com/library/azure/mt420159.aspx) para obter as operações da API REST para o recurso que você deseja implantar.

O exemplo a seguir mostra um recurso **Microsoft.Web/serverfarms** e um recurso **Microsoft.Web/sites** com um recurso **Extensions** aninhado:

    "resources": [
        {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/serverfarms",
          "name": "[parameters('hostingPlanName')]",
          "location": "[resourceGroup().location]",
          "properties": {
              "name": "[parameters('hostingPlanName')]",
              "sku": "[parameters('hostingPlanSku')]",
              "workerSize": "0",
              "numberOfWorkers": 1
          }
        },
        {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/sites",
          "name": "[parameters('siteName')]",
          "location": "[resourceGroup().location]",
          "tags": {
              "environment": "test",
              "team": "ARM"
          },
          "dependsOn": [
              "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
          ],
          "properties": {
              "name": "[parameters('siteName')]",
              "serverFarm": "[parameters('hostingPlanName')]"
          },
          "resources": [
              {
                  "apiVersion": "2014-06-01",
                  "type": "Extensions",
                  "name": "MSDeploy",
                  "dependsOn": [
                      "[resourceId('Microsoft.Web/sites', parameters('siteName'))]"
                  ],
                  "properties": {
                    "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                      "Application Path": "[parameters('siteName')]"
                    }
                  }
              }
          ]
        }
    ]


## Saídas

Na seção de saídas, você especifica valores que são retornados da implantação. Por exemplo, é possível retornar o URI para acessar um recurso implantado.

O exemplo a seguir mostra a estrutura de uma definição de saída:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>",
       }
    }

| Nome do elemento | Obrigatório | Descrição
| :------------: | :------: | :----------
| outputName | Sim | Nome do valor de saída. Deve ser um identificador JavaScript válido.
| type | Sim | Tipo do valor de saída. Valores de saída oferecem suporte aos mesmos tipos que os parâmetros de entrada do modelo.
| value | Sim | Expressão de linguagem do modelo que será avaliada e retornada como valor de saída.


O exemplo a seguir mostra um valor que é retornado na seção de saídas.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

## Cenários mais avançados.
Este tópico fornece uma introdução do modelo. No entanto, o cenário pode exigir tarefas mais avançadas.

Talvez seja necessário mesclar dois modelos ou usar um modelo filho dentro de um modelo pai. Para saber mais, confira [Usando modelos vinculados com o Gerenciador de Recursos do Azure](resource-group-linked-templates.md).

Para iterar um número de vezes especificado ao criar um tipo de recurso, consulte [Criar várias instâncias de recursos no Gerenciador de Recursos do Azure](resource-group-create-multiple.md).

Talvez seja necessário usar recursos que existam em um grupo de recursos diferente. Isso é comum ao trabalhar com contas de armazenamento ou redes virtuais que são compartilhadas entre vários grupos de recursos. Para obter mais informações, consulte a [função resourceId](../resource-group-template-functions#resourceid).

## Modelo completo
O modelo a seguir implanta um aplicativo Web e o provisiona com o código de um arquivo .zip.

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "siteName": {
           "type": "string"
         },
         "hostingPlanName": {
           "type": "string"
         },
         "hostingPlanSku": {
           "type": "string",
           "allowedValues": [
             "Free",
             "Shared",
             "Basic",
             "Standard",
             "Premium"
           ],
           "defaultValue": "Free"
         }
       },
       "resources": [
         {
           "apiVersion": "2014-06-01",
           "type": "Microsoft.Web/serverfarms",
           "name": "[parameters('hostingPlanName')]",
           "location": "[resourceGroup().location]",
           "properties": {
             "name": "[parameters('hostingPlanName')]",
             "sku": "[parameters('hostingPlanSku')]",
             "workerSize": "0",
             "numberOfWorkers": 1
           }
         },
         {
           "apiVersion": "2014-06-01",
           "type": "Microsoft.Web/sites",
           "name": "[parameters('siteName')]",
           "location": "[resourceGroup().location]",
           "tags": {
             "environment": "test",
             "team": "ARM"
           },
           "dependsOn": [
             "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
           ],
           "properties": {
             "name": "[parameters('siteName')]",
             "serverFarm": "[parameters('hostingPlanName')]"
           },
           "resources": [
             {
               "apiVersion": "2014-06-01",
               "type": "Extensions",
               "name": "MSDeploy",
               "dependsOn": [
                 "[resourceId('Microsoft.Web/sites', parameters('siteName'))]"
               ],
               "properties": {
                 "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
                 "dbType": "None",
                 "connectionString": "",
                 "setParameters": {
                   "Application Path": "[parameters('siteName')]"
                 }
               }
             }
           ]
         }
       ],
       "outputs": {
         "siteUri": {
           "type": "string",
           "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
         }
       }
    }

## Próximas etapas
- Para obter detalhes sobre as funções que você pode usar de dentro de um modelo, consulte [Funções de modelo do Gerenciador de Recursos do Azure](resource-group-template-functions.md)
- Para ver como implantar o modelo que você criou, consulte [Implantar um aplicativo com o Modelo do Gerenciador de Recursos do Azure](resource-group-template-deploy.md)
- Para obter um exemplo detalhado de implantação de um aplicativo, confira [Provisionar e implantar microsserviços de forma previsível no Azure](app-service-web/app-service-deploy-complex-application-predictably.md)
- Para ver os esquemas disponíveis, consulte [Esquemas do Gerenciador de Recursos do Azure](https://github.com/Azure/azure-resource-manager-schemas)

<!---HONumber=AcomDC_1210_2015-->