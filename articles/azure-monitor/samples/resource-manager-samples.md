---
title: Примеры шаблонов Azure Resource Manager для службы Azure Monitor
description: Развертывание и настройка функций Azure Monitor с помощью шаблонов Resource Manager
author: bwren
ms.author: bwren
services: azure-monitor
ms.topic: sample
ms.date: 05/18/2020
ms.subservice: ''
ms.openlocfilehash: ab869fc8577d4d1934be96404ded5a2051237bbf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87922734"
---
# <a name="resource-manager-template-samples-for-azure-monitor"></a>Примеры шаблонов Resource Manager для службы Azure Monitor

Azure Monitor можно развертывать и настраивать в большом масштабе с помощью [шаблонов Azure Resource Manager](../../azure-resource-manager/templates/template-syntax.md). В следующих статьях представлены примеры шаблонов для разных функций Azure Monitor. Эти примеры можно изменить под конкретные требования и развернуть с помощью любого стандартного метода, позволяющего развертывать шаблоны Resource Manager. 

## <a name="deploying-the-sample-templates"></a>Развертывание примеров шаблонов
Ниже перечислены основные шаги по применению этих шаблонов.

1. Скопируйте шаблон и сохраните его в виде JSON-файла.
2. Измените параметры среды в JSON-файле и сохраните его.
4. Разверните шаблон с помощью [любого метода развертывания для шаблонов Resource Manager](../../azure-resource-manager/templates/deploy-powershell.md). 

Например, следующие команды позволяют развертывать шаблоны и файлы параметров в группе ресурсов с помощью PowerShell или Azure CLI.


```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName my-subscription
New-AzResourceGroupDeployment -Name AzureMonitorDeployment -ResourceGroupName my-resource-group -TemplateFile azure-monitor-deploy.json -TemplateParameterFile azure-monitor-deploy.parameters.json
```

```azurecli
az login
az deployment group create \
    --name AzureMonitorDeployment \
    --resource-group ResourceGroupofTargetResource \
    --template-file azure-monitor-deploy.json \
    --parameters azure-monitor-deploy.parameters.json
```

## <a name="list-of-sample-templates"></a>Список примеров шаблонов

- [Агенты](resource-manager-agent.md) — развертывание и настройка агента Log Analytics и диагностического расширения.
- видны узлы
  - [Правила генерации оповещений журнала](resource-manager-alerts-log.md) — оповещения от запросов журналов и журнала действий Azure.
  - [Правила генерации оповещений метрик](resource-manager-alerts-metric.md) — оповещения от метрик с использованием разных типов логики.
- [Application Insights](resource-manager-app-resource.md)
- [Параметры диагностики](resource-manager-diagnostic-settings.md) — создание параметров диагностики для пересылки журналов и метрик от разных типов ресурсов.
- [Запросы журнала](resource-manager-log-queries.md) — создание сохраненных запросов к журналу в рабочей области Log Analytics.
- [Рабочая область Log Analytics](resource-manager-workspace.md) — создание рабочей области Log Analytics и настройка коллекции разных источников данных из агента Log Analytics.
- [Книги](resource-manager-workbooks.md) — создание книг.
- [Azure Monitor для контейнеров](resource-manager-container-insights.md) — подключение кластеров к Azure Monitor для создания контейнеров.
- [Azure Monitor для виртуальных машин](resource-manager-vminsights.md) — подключение виртуальных машин к Azure Monitor.



## <a name="next-steps"></a>Дальнейшие действия

- См. дополнительные сведения о [шаблонах Resource Manager](../../azure-resource-manager/templates/overview.md)