---
title: Скрипт PowerShell для обновления уровня согласованности по умолчанию в учетной записи Azure Cosmos
description: Пример скрипта Azure PowerShell. Обновление уровня согласованности по умолчанию для учетной записи Azure Cosmos DB с помощью PowerShell
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/21/2020
ms.author: mjbrown
ms.openlocfilehash: ea1000e6afa5db74cb21c2a061ad3cdf4b43fd6c
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2020
ms.locfileid: "92488288"
---
# <a name="update-the-regions-on-an-azure-cosmos-db-account-using-powershell"></a>Изменение региона в учетной записи Azure Cosmos DB с помощью PowerShell

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Пример скрипта

> [!NOTE]
> Нельзя изменять регионы вместе с другими свойствами учетной записи Cosmos в рамках одной операции. Такие изменения должны выполняться поочередно.
> [!NOTE]
> В этом примере демонстрируется использование учетной записи API SQL. Чтобы использовать этот пример с другими API, соответствующие свойства необходимо скопировать в скрипт для такого API.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-update.ps1 "Update an Azure Cosmos DB account")]

## <a name="clean-up-deployment"></a>Очистка развертывания

После выполнения примера сценария можно удалить группу ресурсов и все связанные с ней ресурсы, выполнив следующую команду.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
|**Azure Cosmos DB**| |
| [Get-AzCosmosDBAccount](/powershell/module/az.cosmosdb/get-azcosmosdbaccount) | Выводит список учетных записей Cosmos DB или указанную учетную запись Cosmos DB. |
| [Update-AzCosmosDBAccount](/powershell/module/az.cosmosdb/update-azcosmosdbaccountfailoverpriority) | Обновление учетной записи Cosmos DB. |
|**Группы ресурсов Azure**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Удаляет группу ресурсов со всеми вложенными ресурсами. |
|||

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о Azure PowerShell см. в [документации по Azure PowerShell](/powershell/).