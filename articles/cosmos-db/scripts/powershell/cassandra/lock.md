---
title: Скрипт PowerShell для создания блокировки ресурсов для пространства ключей и таблицы API Cassandra Azure Cosmos
description: Создание блокировки ресурса для пространства ключей и таблицы API Cassandra Azure Cosmos
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 06/12/2020
ms.openlocfilehash: ab8ffb41d45f60394c27eb492b19eaf280c2e64e
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2020
ms.locfileid: "92488305"
---
# <a name="create-a-resource-lock-for-azure-cosmos-cassandra-api-keyspace-and-table-using-azure-powershell"></a>Создание блокировки ресурса для пространства ключей и таблицы API Cassandra Azure Cosmos с помощью Azure PowerShell

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

> [!IMPORTANT]
> Блокировки ресурсов не работают для изменений, внесенных пользователями, которые подключаются с помощью любого пакета Cassandra SDK, CQL Shell или портала Azure, если только учетная запись Cosmos DB сначала не будет заблокирована с включенным свойством `disableKeyBasedMetadataWriteAccess`. Дополнительные сведения о том, как включить это свойство, см. в разделе [Предотвращение изменений в пакетах SDK](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Пример скрипта

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/cassandra/ps-cassandra-lock.ps1 "Create, list, and remove resource locks")]

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