<properties
   pageTitle="Criando modelos do Gerenciador de Recursos do Azure"
   description="Crie modelos do Gerenciador de Recursos do Azure usando sintaxe JSON declarativa para implantar aplicativos no Azure."
   services="multiple"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2015"
   ms.author="tomfitz;ilygre"/>

# Criando modelos do Gerenciador de Recursos do Azure

Normalmente, os aplicativos do Azure exigem uma combinação de recursos (como servidor de banco de dados, banco de dados ou site) para cumprir as metas desejadas. Em vez de implantar e gerenciar cada recurso separadamente, você pode criar um modelo do Gerenciador de Recursos do Azure que implanta e provisiona todos os recursos para seu aplicativo em uma única operação coordenada. No modelo, você define os recursos que são necessários para o aplicativo e especifica os parâmetros de implantação para inserir valores para ambientes diferentes. O modelo consiste em JSON e expressões que podem ser usados na construção de valores para sua implantação.

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

A sintaxe básica do modelo é JSON. No entanto, expressões e funções estendem o JSON que está disponível no modelo e permitem criar valores que não são valores literais rígidos. As expressões são colocadas entre colchetes ([ e ]) e avaliadas quando o modelo é implantado. As expressões podem aparecer em qualquer lugar em um valor de cadeia de caracteres JSON e sempre retornam outro valor JSON. Se precisar usar uma cadeia de caracteres literal que comece com um colchete [, você deverá usar dois colchetes [[. Normalmente, você usa expressões com funções para executar operações e configurar a implantação. Assim como no JavaScript, as chamadas de função são formatadas como **functionName(arg1,arg2,arg3)**. Você faz referência às propriedades usando os operadores dot e [index]. A lista a seguir mostra funções comuns. - **parameters(parameterName)** Retorna um valor de parâmetro que é fornecido quando a implantação é executada. - **variables(variableName)** Retorna uma variável que é definida no modelo. - **concat(arg1,arg2,arg3,...)** Combina vários valores de cadeia de caracteres. Essa função pode usar qualquer número de argumentos. - **base64(inputString)** Retorna a representação base64 da cadeia de caracteres de entrada. - **resourceGroup()** Retorna um objeto estruturado (com ID, nome e propriedades do local) que representa o grupo de recursos atual. - **resourceId([resourceGroupName], resourceType, resourceName1, [resourceName2]...)** Retorna o identificador exclusivo de um recurso. Pode ser usada para recuperar o recurso de outro grupo de recursos. O exemplo a seguir mostra como usar várias funções ao construir valores: "variables": { "location": "[resourceGroup().location]", "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]", "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]" } Por enquanto, você sabe o suficiente sobre expressões e funções para entender as seções do modelo. Para obter informações mais detalhadas sobre todas as funções do modelo, incluindo parâmetros e o formato dos valores retornados, consulte [Azure Resource Manager template functions](./resource-group-template-functions.md). ## Parâmetros Na seção de parâmetros do modelo, você especifica quais valores um usuário pode inserir ao implantar os recursos. Você pode usar esses valores de parâmetro em todo o modelo para definir valores para os recursos implantados. Somente os parâmetros que são declarados na seção de parâmetros podem ser usados em outras seções do modelo. Na seção de parâmetros, é possível usar um valor de parâmetro para construir outro valor de parâmetro. Esse tipo de operação geralmente ocorre na seção de variáveis. Você define parâmetros com a seguinte estrutura: "parameters": { "<parameterName>" : { "type" : "<type-of-parameter-value>", "defaultValue": "<optional-default-value-of-parameter>", "allowedValues": [ "<optional-array-of-allowed-values>" ] } } | Nome do elemento | Obrigatório | Descrição | :------------: | :------: | :---------- | parameterName | Sim | Nome do parâmetro. Deve ser um identificador JavaScript válido. | type | Sim | Tipo de valor do parâmetro. Consulte a lista abaixo de tipos permitidos. | defaultValue | Não | Valor padrão para o parâmetro, se nenhum valor for fornecido para o parâmetro. | allowedValues | Não | Matriz de valores permitidos para o parâmetro a fim de garantir que o valor certo seja fornecido. Os tipos e valores permitidos são: - string or secureString - qualquer cadeia de caracteres JSON válida - int - qualquer inteiro JSON válido - bool - qualquer booliano JSON válido - object - qualquer objeto JSON válido - array - qualquer matriz JSON válida >[AZURE.NOTE] Todas as senhas, chaves e outros segredos devem usar o tipo **secureString**. Os parâmetros do modelo com o tipo secureString não podem ser lidos após a implantação de recursos. O exemplo a seguir mostra como definir parâmetros: "parameters": { "siteName": { "type": "string" }, "siteLocation": { "type": "string" }, "hostingPlanName": { "type": "string" }, "hostingPlanSku": { "type": "string", "allowedValues": [ "Free", "Shared", "Basic", "Standard", "Premium" ], "defaultValue": "Free" } } ## Variáveis Na seção de variáveis, você constrói valores que podem ser usados para simplificar expressões de linguagem do modelo. Normalmente, essas variáveis serão baseadas em valores fornecidos pelos parâmetros. O exemplo a seguir mostra como definir uma variável que é construída de dois valores de parâmetro: "parameters": { "username": { "type": "string" }, "password": { "type": "secureString" } }, "variables": { "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]" } O próximo exemplo mostra uma variável que é um tipo JSON complexo e as variáveis que são construídas de outras variáveis: "parameters": { "environmentName": { "type": "string", "allowedValues": [ "test", "prod" ] } }, "variables": { "environmentSettings": { "test": { "instancesSize": "Small", "instancesCount": 1 }, "prod": { "instancesSize": "Large", "instancesCount": 4 } }, "currentEnvironmentSettings": "[[variables('environmentSettings')[parameters('environmentName')]]", "instancesSize": "[variables('currentEnvironmentSettings').instancesSize", "instancesCount": "[variables('currentEnvironmentSettings').instancesCount" }

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
| apiVersion | Sim | Versão da API que oferece suporte ao recurso.
| type | Sim | Tipo do recurso. Esse valor é uma combinação do namespace do provedor de recursos e do tipo de recurso que tem suporte do provedor de recursos.
| name | Sim | Nome do recurso. O nome deve seguir as restrições de componente URI definidas em RFC3986.
| location | Não | Locais geográficos com suporte do recurso fornecido.
| marcas | Não | Marcas que são associadas ao recurso.
| dependsOn | Não | Recursos dos quais o recurso que está sendo definido depende. As dependências entre recursos são avaliadas e os recursos são implantados na ordem de dependência. Quando os recursos não dependem uns dos outros, é possível que tentem ser implantados paralelamente. O valor pode ser uma lista separada por vírgulas de nomes de recursos ou identificadores exclusivos de recursos.
| propriedades | Não | Definições de configuração específicas do recurso.
| recursos | Não | Recursos filho que dependem do recurso que está sendo definido.

Se o nome do recurso não for exclusivo, você poderá usar a função auxiliar **resourceId** (descrita abaixo) para obter o identificador exclusivo de qualquer recurso.

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

Talvez seja necessário mesclar dois modelos ou usar um modelo filho dentro de um modelo pai. Para obter mais informações, consulte [Modelos aninhados](../resource-group-advanced-template#nested-template).

Talvez seja necessário usar recursos que existam em um grupo de recursos diferente. Isso é comum ao trabalhar com contas de armazenamento ou redes virtuais que são compartilhadas entre vários grupos de recursos. Para obter mais informações, consulte a [função resourceId](../resource-group-template-functions#resourceid).

## Modelo completo
O modelo a seguir implanta um aplicativo Web e o provisiona com o código de um arquivo .zip.

    {
       "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
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
- [Funções de modelo do Gerenciador de Recursos do Azure](./resource-group-template-functions.md)
- [Implantar um aplicativo com o modelo do Gerenciador de Recursos do Azure](./resource-group-template-deploy.md)
- [Operações avançadas de modelo](./resource-group-advanced-template.md)
- [Visão Geral do Gerenciador de Recursos do Azure](./resource-group-overview.md)

<!---HONumber=58-->