---
title: Разметка ресурсов, групп ресурсов и подписок для логической организации
description: Здесь описано, как применить теги, чтобы организовать ресурсы Azure для выставления счетов и управления.
ms.topic: conceptual
ms.date: 07/27/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 3ffcb4a0f2f5dc64b165fcdec03f7c3ced258cc1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90086765"
---
# <a name="use-tags-to-organize-your-azure-resources-and-management-hierarchy"></a>Использование тегов для Организации ресурсов Azure и иерархии управления

Вы примените теги к ресурсам, группам ресурсов и подпискам Azure, чтобы логически упорядочить их в таксономии. Каждый тег состоит из пары "имя — значение". Например, имя Environment и значение Production можно применить ко всем ресурсам в рабочей среде.

Рекомендации по реализации стратегии тегов см. в разделе [руководство по именованию ресурсов и созданию тегов](/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json).

> [!IMPORTANT]
> В именах тегов не учитывается регистр для операций. Обновляется или извлекается тег с именем тега, независимо от регистра. Однако поставщик ресурсов может оставить регистр, который вы задаюте для имени тега. Вы увидите этот регистр в отчетах о затратах.
> 
> В значениях тегов учитывается регистр.

[!INCLUDE [Handle personal data](../../../includes/gdpr-intro-sentence.md)]

## <a name="required-access"></a>Требуемый доступ

Чтобы применить теги к ресурсу, необходимо иметь доступ на запись к типу ресурса **Microsoft. Resources/Tags** . Роль « [участник тегов](../../role-based-access-control/built-in-roles.md#tag-contributor) » позволяет применять теги к сущности без доступа к самой сущности. В настоящее время роль участника тега не может применять теги к ресурсам или группам ресурсов на портале. Он может применять теги к подпискам на портале. Она поддерживает все операции с тегами с помощью PowerShell и REST API.  

Роль [участника](../../role-based-access-control/built-in-roles.md#contributor) также предоставляет необходимый доступ для применения тегов к любой сущности. Чтобы применить теги только к одному типу ресурсов, используйте роль участника для этого ресурса. Например, чтобы применить теги к виртуальным машинам, используйте [Участник виртуальных машин](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor).

## <a name="powershell"></a>PowerShell

### <a name="apply-tags"></a>Применить Теги

Azure PowerShell предлагает две команды для применения тегов- [New-азтаг](/powershell/module/az.resources/new-aztag) и [Update-азтаг](/powershell/module/az.resources/update-aztag). Необходимо иметь модуль AZ. Resources 1.12.0 или более поздней версии. Версию можно проверить с помощью `Get-Module Az.Resources` . Вы можете установить этот модуль или [установить Azure PowerShell](/powershell/azure/install-az-ps) 3.6.1 или более поздней версии.

**New-азтаг** заменяет все теги в ресурсе, группе ресурсов или подписке. При вызове команды передайте идентификатор ресурса сущности, которую вы хотите пометить.

В следующем примере набор тегов применяется к учетной записи хранения.

```azurepowershell-interactive
$tags = @{"Dept"="Finance"; "Status"="Normal"}
$resource = Get-AzResource -Name demoStorage -ResourceGroup demoGroup
New-AzTag -ResourceId $resource.id -Tag $tags
```

После выполнения команды Обратите внимание, что ресурс имеет два тега.

```output
Properties :
        Name    Value
        ======  =======
        Dept    Finance
        Status  Normal
```

Если выполнить команду еще раз, но на этот раз с другими тегами, обратите внимание, что предыдущие Теги удалены.

```azurepowershell-interactive
$tags = @{"Team"="Compliance"; "Environment"="Production"}
New-AzTag -ResourceId $resource.id -Tag $tags
```

```output
Properties :
        Name         Value
        ===========  ==========
        Environment  Production
        Team         Compliance
```

Чтобы добавить теги к ресурсу, который уже содержит теги, используйте **Update-азтаг**. Задайте для параметра **-Operation** значение **Merge**.

```azurepowershell-interactive
$tags = @{"Dept"="Finance"; "Status"="Normal"}
Update-AzTag -ResourceId $resource.id -Tag $tags -Operation Merge
```

Обратите внимание, что два новых тега были добавлены к двум существующим тегам.

```output
Properties :
        Name         Value
        ===========  ==========
        Status       Normal
        Dept         Finance
        Team         Compliance
        Environment  Production
```

Каждое имя тега может иметь только одно значение. Если указать новое значение для тега, старое значение будет заменено даже при использовании операции MERGE. В следующем примере тег Status изменяется с нормального на зеленый.

```azurepowershell-interactive
$tags = @{"Status"="Green"}
Update-AzTag -ResourceId $resource.id -Tag $tags -Operation Merge
```

```output
Properties :
        Name         Value
        ===========  ==========
        Status       Green
        Dept         Finance
        Team         Compliance
        Environment  Production
```

Если для параметра **-Operation** задано значение **Replace**, существующие теги заменяются новым набором тегов.

```azurepowershell-interactive
$tags = @{"Project"="ECommerce"; "CostCenter"="00123"; "Team"="Web"}
Update-AzTag -ResourceId $resource.id -Tag $tags -Operation Replace
```

В ресурсе остаются только новые теги.

```output
Properties :
        Name        Value
        ==========  =========
        CostCenter  00123
        Team        Web
        Project     ECommerce
```

Одни и те же команды также работают с группами ресурсов или подписками. Вы передаете идентификатор для группы ресурсов или подписки, которые необходимо пометить.

Чтобы добавить новый набор тегов в группу ресурсов, используйте:

```azurepowershell-interactive
$tags = @{"Dept"="Finance"; "Status"="Normal"}
$resourceGroup = Get-AzResourceGroup -Name demoGroup
New-AzTag -ResourceId $resourceGroup.ResourceId -tag $tags
```

Чтобы обновить теги для группы ресурсов, используйте:

```azurepowershell-interactive
$tags = @{"CostCenter"="00123"; "Environment"="Production"}
$resourceGroup = Get-AzResourceGroup -Name demoGroup
Update-AzTag -ResourceId $resourceGroup.ResourceId -Tag $tags -Operation Merge
```

Чтобы добавить новый набор тегов к подписке, используйте:

```azurepowershell-interactive
$tags = @{"CostCenter"="00123"; "Environment"="Dev"}
$subscription = (Get-AzSubscription -SubscriptionName "Example Subscription").Id
New-AzTag -ResourceId "/subscriptions/$subscription" -Tag $tags
```

Чтобы обновить теги для подписки, используйте:

```azurepowershell-interactive
$tags = @{"Team"="Web Apps"}
$subscription = (Get-AzSubscription -SubscriptionName "Example Subscription").Id
Update-AzTag -ResourceId "/subscriptions/$subscription" -Tag $tags -Operation Merge
```

В группе ресурсов может быть несколько ресурсов с одним и тем же именем. В этом случае можно задать каждый ресурс с помощью следующих команд:

```azurepowershell-interactive
$resource = Get-AzResource -ResourceName sqlDatabase1 -ResourceGroupName examplegroup
$resource | ForEach-Object { Update-AzTag -Tag @{ "Dept"="IT"; "Environment"="Test" } -ResourceId $_.ResourceId -Operation Merge }
```

### <a name="list-tags"></a>Вывод списка тегов

Чтобы получить теги для ресурса, группы ресурсов или подписки, используйте команду [Get-азтаг](/powershell/module/az.resources/get-aztag) и передайте идентификатор ресурса для сущности.

Чтобы просмотреть теги для ресурса, используйте:

```azurepowershell-interactive
$resource = Get-AzResource -Name demoStorage -ResourceGroup demoGroup
Get-AzTag -ResourceId $resource.id
```

Чтобы просмотреть теги для группы ресурсов, используйте:

```azurepowershell-interactive
$resourceGroup = Get-AzResourceGroup -Name demoGroup
Get-AzTag -ResourceId $resourceGroup.ResourceId
```

Чтобы просмотреть теги для подписки, используйте:

```azurepowershell-interactive
$subscription = (Get-AzSubscription -SubscriptionName "Example Subscription").Id
Get-AzTag -ResourceId "/subscriptions/$subscription"
```

### <a name="list-by-tag"></a>Список по тегу

Чтобы получить ресурсы с указанными именем и значением тега, используйте:

```azurepowershell-interactive
(Get-AzResource -Tag @{ "CostCenter"="00123"}).Name
```

Чтобы получить ресурсы с заданным именем тега и любым значением тега, используйте:

```azurepowershell-interactive
(Get-AzResource -TagName "Dept").Name
```

Чтобы получить группы ресурсов с указанными именем и значением тега, используйте:

```azurepowershell-interactive
(Get-AzResourceGroup -Tag @{ "CostCenter"="00123" }).ResourceGroupName
```

### <a name="remove-tags"></a>Удалить теги

Чтобы удалить определенные теги, используйте **Update-азтаг** и Set **-Operation** для **удаления**. Передайте теги, которые нужно удалить.

```azurepowershell-interactive
$removeTags = @{"Project"="ECommerce"; "Team"="Web"}
Update-AzTag -ResourceId $resource.id -Tag $removeTags -Operation Delete
```

Указанные теги удаляются.

```output
Properties :
        Name        Value
        ==========  =====
        CostCenter  00123
```

Чтобы удалить все теги, используйте команду [Remove-азтаг](/powershell/module/az.resources/remove-aztag) .

```azurepowershell-interactive
$subscription = (Get-AzSubscription -SubscriptionName "Example Subscription").Id
Remove-AzTag -ResourceId "/subscriptions/$subscription"
```

## <a name="azure-cli"></a>Azure CLI

### <a name="apply-tags"></a>Применить Теги

При добавлении тегов в группу ресурсов или ресурс можно либо перезаписать существующие теги, либо добавить новые теги в существующие теги.

Чтобы перезаписать теги для ресурса, используйте:

```azurecli-interactive
az resource tag --tags 'Dept=IT' 'Environment=Test' -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Чтобы добавить тег к существующим тегам для ресурса, используйте:

```azurecli-interactive
az resource update --set tags.'Status'='Approved' -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Чтобы перезаписать существующие теги в группе ресурсов, используйте:

```azurecli-interactive
az group update -n examplegroup --tags 'Environment=Test' 'Dept=IT'
```

Чтобы добавить тег к существующим тегам в группе ресурсов, используйте:

```azurecli-interactive
az group update -n examplegroup --set tags.'Status'='Approved'
```

В настоящее время Azure CLI не имеет команды для применения тегов к подпискам. Однако можно использовать интерфейс командной строки для развертывания шаблона ARM, который применяет теги к подписке. См. раздел [применение тегов к группам ресурсов или подпискам](#apply-tags-to-resource-groups-or-subscriptions).

### <a name="list-tags"></a>Вывод списка тегов

Чтобы просмотреть существующие теги для ресурса, используйте:

```azurecli-interactive
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Чтобы просмотреть существующие теги для группы ресурсов, используйте этот командлет:

```azurecli-interactive
az group show -n examplegroup --query tags
```

Этот скрипт вернет ответ в следующем формате:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

### <a name="list-by-tag"></a>Список по тегу

Чтобы получить все ресурсы с определенным тегом и значением, используйте команду `az resource list`:

```azurecli-interactive
az resource list --tag Dept=Finance
```

Чтобы получить группы ресурсов с определенным тегом, используйте команду `az group list`:

```azurecli-interactive
az group list --tag Dept=IT
```

### <a name="handling-spaces"></a>Обработка пробелов

Если имена или значения тегов содержат пробелы, необходимо выполнить несколько дополнительных действий. 

`--tags`Параметры в Azure CLI могут принимать строку, состоящую из массива строк. В следующем примере выполняется перезапись тегов в группе ресурсов, в которой теги содержат пробелы и дефисы: 

```azurecli-interactive
TAGS=("Cost Center=Finance-1222" "Location=West US")
az group update --name examplegroup --tags "${TAGS[@]}"
```

Такой же синтаксис можно использовать при создании или обновлении группы ресурсов или ресурсов с помощью `--tags` параметра.

Чтобы обновить теги с помощью `--set` параметра, необходимо передать ключ и значение в виде строки. В следующем примере один тег добавляется в группу ресурсов:

```azurecli-interactive
TAG="Cost Center='Account-56'"
az group update --name examplegroup --set tags."$TAG"
```

В этом случае значение тега помечается одинарными кавычками, поскольку оно имеет дефис.

Также может потребоваться применить теги ко многим ресурсам. В следующем примере все теги из группы ресурсов применяются к своим ресурсам, если теги могут содержать пробелы:

```azurecli-interactive
jsontags=$(az group show --name examplegroup --query tags -o json)
tags=$(echo $jsontags | tr -d '{}"' | sed 's/: /=/g' | sed "s/\"/'/g" | sed 's/, /,/g' | sed 's/ *$//g' | sed 's/^ *//g')
origIFS=$IFS
IFS=','
read -a tagarr <<< "$tags"
resourceids=$(az resource list -g examplegroup --query [].id --output tsv)
for id in $resourceids
do
  az resource tag --tags "${tagarr[@]}" --id $id
done
IFS=$origIFS
```

## <a name="templates"></a>Шаблоны

Вы можете помечать ресурсы, группы ресурсов и подписки во время развертывания с помощью шаблона диспетчер ресурсов.

### <a name="apply-values"></a>Применить значения

В следующем примере выполняется развертывание учетной записи хранения с тремя тегами. Два тега ( `Dept` и `Environment` ) задаются как литеральные значения. Для одного тега ( `LastDeployed` ) задан параметр, который по умолчанию имеет текущую дату.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcShort": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": {
                "Dept": "Finance",
                "Environment": "Production",
                "LastDeployed": "[parameters('utcShort')]"
            },
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

### <a name="apply-an-object"></a>Применение объекта

Можно определить параметр объекта, который хранит несколько тегов, и применить этот объект к элементу тега. Такой подход обеспечивает большую гибкость, чем предыдущий пример, поскольку объект может иметь различные свойства. Каждое свойство в объекте становится отдельным тегом ресурса. В следующем примере содержится параметр с именем `tagValues`, который применяется к элементу тега.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        },
        "tagValues": {
            "type": "object",
            "defaultValue": {
                "Dept": "Finance",
                "Environment": "Production"
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": "[parameters('tagValues')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

### <a name="apply-a-json-string"></a>Применение строки JSON

Для хранения большого количества значений в одном теге примените строку JSON, представляющую значения. Вся строка JSON хранится в виде одного тега, длина которого не должна превышать 256 символов. В следующем примере приведен один тег с именем `CostCenter`, содержащий несколько значений из строки JSON.  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": {
                "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
            },
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

### <a name="apply-tags-from-resource-group"></a>Применение тегов из группы ресурсов

Чтобы применить теги из группы ресурсов к ресурсу, используйте функцию [resourceGroup ()](../templates/template-functions-resource.md#resourcegroup) . При получении значения тега используйте `tags[tag-name]` синтаксис вместо `tags.tag-name` синтаксиса, так как некоторые символы не анализируются правильно в точечной нотации.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": {
                "Dept": "[resourceGroup().tags['Dept']]",
                "Environment": "[resourceGroup().tags['Environment']]"
            },
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

### <a name="apply-tags-to-resource-groups-or-subscriptions"></a>Применение тегов к группам ресурсов или подпискам

Вы можете добавить теги в группу ресурсов или подписку, развернув тип ресурса **Microsoft. Resources/Tags** . Теги применяются к целевой группе ресурсов или подписке для развертывания. Каждый раз при развертывании шаблона заменяются все теги, которые были ранее применены.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "tagName": {
            "type": "string",
            "defaultValue": "TeamName"
        },
        "tagValue": {
            "type": "string",
            "defaultValue": "AppTeam1"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/tags",
            "name": "default",
            "apiVersion": "2019-10-01",
            "dependsOn": [],
            "properties": {
                "tags": {
                    "[parameters('tagName')]": "[parameters('tagValue')]"
                }
            }
        }
    ]
}
```

Чтобы применить теги к группе ресурсов, используйте либо PowerShell, либо Azure CLI. Разверните в группе ресурсов, которую необходимо пометить.

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName exampleGroup -TemplateFile https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/tags.json
```

```azurecli-interactive
az deployment group create --resource-group exampleGroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/tags.json
```

Чтобы применить теги к подписке, используйте либо PowerShell, либо Azure CLI. Выполните развертывание в подписке, которую нужно пометить.

```azurepowershell-interactive
New-AzSubscriptionDeployment -name tagresourcegroup -Location westus2 -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/tags.json
```

```azurecli-interactive
az deployment sub create --name tagresourcegroup --location westus2 --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/tags.json
```

Дополнительные сведения о развертывании подписок см. в статье [Создание групп ресурсов и ресурсов на уровне подписки](../templates/deploy-to-subscription.md).

Следующий шаблон добавляет теги из объекта в группу ресурсов или подписку.

```json
"$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "tags": {
            "type": "object",
            "defaultValue": {
                "TeamName": "AppTeam1",
                "Dept": "Finance",
                "Environment": "Production"
            }
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/tags",
            "name": "default",
            "apiVersion": "2019-10-01",
            "dependsOn": [],
            "properties": {
                "tags": "[parameters('tags')]"
            }
        }
    ]
}
```

## <a name="portal"></a>Портал

[!INCLUDE [resource-manager-tag-resource](../../../includes/resource-manager-tag-resources.md)]

## <a name="rest-api"></a>REST API

Для работы с тегами с помощью REST API Azure используйте:

* [Теги — создание или обновление в области действия](/rest/api/resources/tags/createorupdateatscope) (операция размещения)
* [Теги — обновление в области](/rest/api/resources/tags/updateatscope) (операция исправления)
* [Теги — получение в области действия](/rest/api/resources/tags/getatscope) (операция Get)
* [Теги — удаление в области действия](/rest/api/resources/tags/deleteatscope) (операция удаления)

## <a name="inherit-tags"></a>Наследование тегов

Теги, применяемые к группе ресурсов или подписке, не наследуются ресурсами. Чтобы применить теги из подписки или группы ресурсов к ресурсам, см. раздел [политики Azure — Теги](tag-policies.md).

## <a name="tags-and-billing"></a>Теги и выставление счетов

С помощью тегов можно группировать данные о выставлении счетов. Например, если у вас работает несколько виртуальных машин для разных организаций, то с помощью тегов можно группировать сведения об использовании по месту возникновения затрат. Кроме того, теги можно использовать для группирования затрат по среде выполнения (например, сведения о выставленных счетах за виртуальные машины, запущенные в рабочей среде).

Сведения о тегах можно получить с помощью [API-интерфейсов использования ресурсов Azure и платной карты](../../cost-management-billing/manage/usage-rate-card-overview.md) , а также файла данных, разделенных запятыми (CSV). Файл использования загружается из портал Azure. Дополнительные сведения см. в статье [Скачивание или просмотр счета на оплату и данных о ежедневном использовании в Azure](../../cost-management-billing/manage/download-azure-invoice-daily-usage-date.md). При скачивании файла сведений об использовании из Центра управления учетной записью Azure выберите **Версия 2**. Для служб, поддерживающих теги выставления счетов, эти теги отображаются в столбце **Теги**.

Подробнее об операциях REST API см. в [справочнике по REST API для выставления счетов Azure](/rest/api/billing/).

## <a name="limitations"></a>Ограничения

Действительны следующие ограничения для тегов.

* Не все типы ресурсов поддерживают теги. Сведения о возможности применения тегов к типу ресурса см. в статье о [поддержке тегов ресурсами Azure](tag-support.md).
* Каждый ресурс, Группа ресурсов и подписка могут иметь не более 50 пар "имя-значение" для тегов. Если необходимо применить больше тегов, чем максимально допустимое число, используйте строку JSON для значения тега. Строка JSON может содержать много значений, применяемых к одному имени тега. Группа ресурсов или подписка может содержать множество ресурсов, каждая из которых содержит 50 пар "имя-значение" для тегов.
* Имя тега ограничено 512 символами, а значение тега — 256 символами. Для учетных записей хранения имя тега ограничено 128 символами, а значение тега — 256 символами.
* Теги нельзя применять к классическим ресурсам, например к облачным службам Microsoft Azure.
* Имена тегов не могут содержать следующие символы: `<`, `>`, `%`, `&`, `\`, `?`, `/`

   > [!NOTE]
   > В настоящее время зоны Azure DNS и службы диспетчера трафика также не разрешают использовать пробелы в теге.
   >
   > Передняя дверца Azure не поддерживает использование `#` в имени тега.
   >
   > Служба автоматизации Azure и Azure CDN поддерживают только 15 тегов для ресурсов.

## <a name="next-steps"></a>Дальнейшие шаги

* Не все типы ресурсов поддерживают теги. Сведения о возможности применения тегов к типу ресурса см. в статье о [поддержке тегов ресурсами Azure](tag-support.md).
* Рекомендации по реализации стратегии тегов см. в разделе [руководство по именованию ресурсов и созданию тегов](/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json).
