---
author: cherylmc
ms.date: 12/06/2019
ms.service: expressroute
ms.topic: include
ms.author: cherylmc
ms.openlocfilehash: f04861d55c9cea3c79f4983f7be2e1f5a3c6d864
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "74926213"
---
Установите последние версии модулей PowerShell управления службами Azure (SM) и модуля ExpressRoute. В среде Azure CloudShell нельзя запускать модули SM.

1. Воспользуйтесь инструкциями по [установке модуля управления службами Azure](/powershell/azure/servicemanagement/install-azure-ps). Если модуль Az или RM уже установлен, обязательно используйте "-AllowClobber".
2. Импортируйте установленные модули. При использовании приведенного ниже примера измените путь в соответствии с расположением и версией установленных модулей PowerShell.

   ```powershell
   Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.3.0\Azure.psd1'
   Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.3.0\ExpressRoute\ExpressRoute.psd1'
   ```
3. Чтобы войти в свою учетную запись Azure, откройте консоль PowerShell с повышенными правами и подключитесь к учетной записи. Следующий пример позволяет установить подключение с помощью модуля управления службами:

   ```powershell
   Add-AzureAccount
   ```