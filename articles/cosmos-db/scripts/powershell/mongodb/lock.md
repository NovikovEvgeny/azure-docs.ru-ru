---
title: Скрипт PowerShell для создания блокировки ресурсов для базы данных и контейнера API MongoDB в Azure Cosmos
description: Создание блокировки ресурса для базы данных и коллекции API MongoDB в Azure Cosmos
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 06/12/2020
ms.openlocfilehash: b9b743e9f293455c09557348c53cc76b1f904b0a
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2020
ms.locfileid: "92489138"
---
# <a name="create-a-resource-lock-for-azure-cosmos-mongodb-api-database-and-collection-using-azure-powershell"></a>Создание блокировки ресурсов для базы данных и контейнера API MongoDB в Azure Cosmos с помощью Azure PowerShell

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

> [!IMPORTANT]
> Блокировки ресурсов не работают для изменений, внесенных пользователями, которые подключаются с помощью любого пакета SDK MongoDB, Mongoshell или портала Azure, если только учетная запись Cosmos DB сначала не будет заблокирована с помощью включения свойства `disableKeyBasedMetadataWriteAccess`. Дополнительные сведения о том, как включить это свойство, см. в разделе [Предотвращение изменений в пакетах SDK](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Пример скрипта

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-lock.ps1 "Create, list, and remove resource locks")]

## <a name="clean-up-deployment"></a>Очистка развертывания

После выполнения примера сценария можно удалить группу ресурсов и все связанные с ней ресурсы, выполнив следующую команду.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
|**Ресурс Azure**| |
| [New-AzResourceLock](/powershell/module/az.resources/new-azresourcelock) | Создает блокировку ресурса. |
| [Get-AzResourceLock](/powershell/module/az.resources/get-azresourcelock) | Возвращает блокировку ресурса или список блокировок ресурсов. |
| [Remove-AzResourceLock](/powershell/module/az.resources/remove-azresourcelock) | Удаляет блокировку ресурса. |
|||

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure PowerShell см. в [документации по Azure PowerShell](/powershell/).