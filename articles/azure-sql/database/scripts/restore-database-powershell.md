---
title: PowerShell. Восстановление автоматически созданной резервной копии базы данных в Базе данных SQL
description: Используйте пример сценария Azure PowerShell для восстановления базы данных в Базе данных SQL на момент автоматического создания резервной копии.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: mashamsft
ms.author: mathoma
ms.reviewer: sstein
ms.date: 03/27/2019
ms.openlocfilehash: 231a97a375a46940f461d5783f9d70c701389e78
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2020
ms.locfileid: "92792792"
---
# <a name="use-powershell-to-restore-a-database-to-an-earlier-point-in-time"></a>Использование PowerShell для восстановления базы данных на более ранний момент времени

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Этот пример сценария PowerShell восстанавливает базу данных в Базе данных SQL на определенный момент времени.  

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Если требуется установить и использовать PowerShell локально, для работы с этим руководством вам понадобится AZ PowerShell 1.4.0 или последующей версии. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-az-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Connect-AzAccount`, чтобы создать подключение к Azure.

## <a name="sample-script"></a>Пример скрипта

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Очистка развертывания

Используйте следующую команду, чтобы удалить группу ресурсов и все связанные с ней ресурсы.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Создает сервер, на котором размещены базы данных и эластичные пулы. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Создает базу данных на сервере. |
| [Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) | Получает геоизбыточную резервную копию отдельной базы данных или базы данных в пуле. |
| [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) | Восстанавливает базу данных. |
| [Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase) | Удаляет базу данных. |
| [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Получает удаленную базу данных, которую можно восстановить. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Удаляет группу ресурсов со всеми вложенными ресурсами. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure PowerShell см. в [документации по Azure PowerShell](/powershell/azure/).

Дополнительные примеры сценариев PowerShell для базы данных SQL Azure можно найти [здесь](../powershell-script-content-guide.md).