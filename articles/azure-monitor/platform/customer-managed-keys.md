---
title: Ключ Azure Monitor, управляемый клиентом
description: Сведения и действия по настройке ключа Customer-Managed для шифрования данных в рабочих областях Log Analytics с помощью ключа Azure Key Vault.
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 11/09/2020
ms.openlocfilehash: 62621a36955808ec3f2c796681fe660e6e8524bc
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94443387"
---
# <a name="azure-monitor-customer-managed-key"></a>Ключ Azure Monitor, управляемый клиентом 

В этой статье содержатся общие сведения и инструкции по настройке ключей, управляемых клиентом, для рабочих областей Log Analytics. После их настройки все данные, отправленные в рабочие области, шифруются с помощью ключа Azure Key Vault.

Перед настройкой рекомендуется ознакомиться с разделом [Пределы и ограничения](#limitationsandconstraints) ниже.

## <a name="customer-managed-key-overview"></a>Обзор ключа, управляемого клиентом

[Шифрование неактивных](../../security/fundamentals/encryption-atrest.md) данных — это общие требования к конфиденциальности и безопасности в организациях. Вы можете полностью управлять шифрованием в Azure, в то время как у вас есть различные варианты для тесного управления шифрованием или ключами шифрования.

Azure Monitor гарантирует, что все данные и сохраненные запросы шифруются при хранении с помощью ключей, управляемых корпорацией Майкрософт (ММК). Azure Monitor также предоставляет возможность шифрования с помощью собственного ключа, который хранится в [Azure Key Vault](../../key-vault/general/overview.md) и используется хранилищем для шифрования данных. Ключом может быть [программное или аппаратное обеспечение с защитой HSM](../../key-vault/general/overview.md). Azure Monitor использование шифрования идентично тому, как работает [Шифрование службы хранилища Azure](../../storage/common/storage-service-encryption.md#about-azure-storage-encryption) .

Возможность использования ключа, управляемого клиентом, доставляется в выделенных кластерах Log Analytics. Она позволяет защищать данные с помощью элемента управления [защищенным хранилищем](#customer-lockbox-preview) и предоставляет элементу управления возможность отзывать доступ к данным в любое время. Данные, полученные за последние 14 дней, также хранятся в кэше горячего уровня доступа (с поддержкой SSD) для эффективной работы обработчика запросов. Эти данные остаются зашифрованными с помощью ключей Майкрософт независимо от управляемой клиентом конфигурации ключа, но контроль над данными SSD соответствует [отзыву ключа](#key-revocation). Мы работаем над тем, чтобы данные SSD были зашифрованы с Customer-Managed ключом в первой половине 2021.

[Модель ценообразования log Analytics кластеров](./manage-cost-storage.md#log-analytics-dedicated-clusters) использует резервирование емкости, начиная с 1000 ГБ/день.

> [!IMPORTANT]
> В связи с временными ограничениями емкости перед созданием кластера необходимо предварительно зарегистрироваться в. Используйте свои контакты в Майкрософт или откройте запрос в службу поддержки, чтобы зарегистрировать идентификаторы подписок.

## <a name="how-customer-managed-key-works-in-azure-monitor"></a>Как работает ключ Customer-Managed в Azure Monitor

Azure Monitor использует назначаемое системой управляемое удостоверение для предоставления доступа к Azure Key Vault. Управляемое системой удостоверение может быть связано только с одним ресурсом Azure, в то время как удостоверение кластера Log Analytics поддерживается на уровне кластера. это указывает на то, что эта возможность доставляется в выделенный кластер Log Analytics. Для поддержки ключа Customer-Managed в нескольких рабочих областях новый ресурс *кластера* log Analytics работает как промежуточное подключение между Key Vault и рабочими областями log Analytics. Хранилище кластеров Log Analytics использует управляемое удостоверение, связанное \'с ресурсом *Кластер* , для проверки подлинности Azure Key Vault через Azure Active Directory. 

После настройки все данные, принимаемые в рабочие области, связанные с выделенным кластером, шифруются с помощью ключа в Key Vault. Вы можете удалить связь рабочих областей из кластера в любое время. Новые данные затем принимаются в хранилище Log Analytics и шифруются с помощью ключа Microsoft, тогда как можно легко запросить новые и старые данные.


![Обзор ключа Customer-Managed](media/customer-managed-keys/cmk-overview.png)

1. Key Vault
2. Ресурс *Кластер* Log Analytics, имеющий управляемое удостоверение с разрешениями на Key Vault. Удостоверение распространяется на подчиненное выделенное хранилище кластера Log Analytics.
3. Выделенный кластер Log Analytics.
4. Рабочие области, связанные с ресурсом *кластера* 

## <a name="encryption-keys-operation"></a>Операция с ключами шифрования

Существует 3 типа ключей, участвующих в шифровании данных службы хранилища:

- Ключ шифрования **KEK** -Key (ваш ключ Customer-Managed)
- **AEK**  — ключ шифрования учетной записи
- **DEK**  — ключ шифрования данных

Применяются следующие правила.

- Log Analytics учетные записи хранения кластера создают уникальный ключ шифрования для каждой учетной записи хранения, который называется АЕК.
- АЕК используется для получения DEK, которые являются ключами, используемыми для шифрования каждого блока данных, записываемых на диск.
- Когда вы настраиваете ключ в Key Vault и ссылаетсяе на него в кластере, служба хранилища Azure отправляет запросы в Azure Key Vault для переноса и распаковки АЕК для выполнения операций шифрования и расшифровки данных.
- KEK никогда не покидает ваш Key Vault и в случае ключа HSM он никогда не покидает оборудование.
- Служба хранилища Azure использует управляемое удостоверение, связанное с ресурсом *кластера* , для проверки подлинности и доступа к Azure Key Vault через Azure Active Directory.

## <a name="customer-managed-key-provisioning-procedure"></a>Процедура подготовки ключа Customer-Managed

1. Регистрация подписки для разрешения создания кластера
1. Создание Azure Key Vault и ключа хранилища.
1. Создание кластера
1. Предоставление разрешений для вашего Key Vault.
1. Связывание рабочих областей Log Analytics

Customer-Managed конфигурация ключа не поддерживается в портал Azure и подготовка выполняется с помощью [PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights/), [CLI](https://docs.microsoft.com/cli/azure/monitor/log-analytics) или запросов [RESTful](https://docs.microsoft.com/rest/api/loganalytics/) .

### <a name="asynchronous-operations-and-status-check"></a>Асинхронные операции и проверка состояния

Некоторые этапы настройки выполняются асинхронно, так как они не могут быть завершены быстро. При использовании запросов RESTFUL в конфигурации ответ сначала возвращает код состояния HTTP 200 (ОК) и заголовок с помощью свойства *Azure-AsyncOperation* , если оно принято:
```json
"Azure-AsyncOperation": "https://management.azure.com/subscriptions/subscription-id/providers/Microsoft.OperationalInsights/locations/region-name/operationStatuses/operation-id?api-version=2020-08-01"
```

Затем можно проверить состояние асинхронной операции, отправив запрос GET в значение заголовка *Azure-AsyncOperation* :
```rst
GET https://management.azure.com/subscriptions/subscription-id/providers/microsoft.operationalInsights/locations/region-name/operationstatuses/operation-id?api-version=2020-08-01
Authorization: Bearer <token>
```

Ответ содержит сведения об операции, а также ее *состояние*. Он может принимать одно из следующих значений:

Операция выполняется
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "InProgress", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
}
```

Выполняется операция обновления идентификатора ключа.
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Updating", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
}
```

Выполняется удаление кластера. при удалении кластера со связанными рабочими областями операция удаления связи выполняется для каждой из рабочих областей в асинхронном режиме, и операция может занять некоторое время.
Это не относится к удалению кластера без связанной рабочей области. в этом случае кластер удаляется немедленно.
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Deleting", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
}
```

Операция завершена
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Succeeded", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
}
```

Ошибка при выполнении операции
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Failed", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
    "error" : { 
        "code": "error-code",  
        "message": "error-message" 
    }
}
```

### <a name="allowing-subscription"></a>Разрешение подписки

> [!IMPORTANT]
> Customer-Managed ключом является региональный. Azure Key Vault, кластер и связанные рабочие области Log Analytics должны находиться в одном регионе, но они могут находиться в разных подписках.

### <a name="storing-encryption-key-kek"></a>Хранение ключа шифрования (KEK)

Создайте или используйте решение Azure Key Vault, которое вы должны были уже создать, или импортируйте ключ, который будет использоваться для шифрования данных. Для Azure Key Vault нужно настроить возможность восстановления, чтобы обезопасить ключи и доступ к данным в Azure Monitor. Вы можете проверить эту конфигурацию в свойствах вашего Key Vault. Следует включить пункты *Обратимое удаление* и *Защита от очистки*.

![Параметры обратимого удаления и защиты от очистки](media/customer-managed-keys/soft-purge-protection.png)

Эти параметры можно обновить в Key Vault с помощью интерфейса командной строки и PowerShell:

- [обратимое удаление](../../key-vault/general/soft-delete-overview.md)
- [Защита от очистки](../../key-vault/general/soft-delete-overview.md#purge-protection) защищает секрет и данные хранилища от принудительного удаления даже после обратимого удаления.

### <a name="create-cluster"></a>Создание кластера

Выполните процедуру, показанную в [статье выделенные кластеры](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters#creating-a-cluster). 

> [!IMPORTANT]
> Скопируйте и сохраните ответ, так как эти сведения понадобятся вам в дальнейших действиях.

### <a name="grant-key-vault-permissions"></a>Предоставление разрешений Key Vault

Создайте политику доступа в Key Vault, чтобы предоставить разрешения для кластера. Эти разрешения используются в базовой службе хранилища Azure Monitor для шифрования данных. Откройте Key Vault на портале Azure и щелкните "Политики доступа", а затем "+ Добавить политику доступа", чтобы создать политику с такими параметрами:

- Разрешения ключа: выберите "Получение", "Упаковка ключа" и "Распаковка ключа".
- Выбор субъекта: введите имя кластера или идентификатор субъекта, который был возвращен в ответе на предыдущем шаге.

![предоставление разрешений Key Vault](media/customer-managed-keys/grant-key-vault-permissions-8bit.png)

Разрешение *Получение* необходимо, чтобы убедиться, что Key Vault настроено как восстанавливаемое для защиты ключа и доступа к данным Azure Monitor.

### <a name="update-cluster-with-key-identifier-details"></a>Обновление кластера с подробными сведениями об идентификаторе ключа

Для всех операций в кластере требуется разрешение Microsoft. OperationalInsights/Clusters/Write Action. Это разрешение можно предоставить с помощью владельца или участника, который содержит *действие/Врите, или с помощью роли участника log Analytics, которая содержит действие Microsoft. OperationalInsights/* Action.

На этом шаге Azure Monitor хранилище обновляется ключом и версией, которые будут использоваться для шифрования данных. При обновлении новый ключ используется для упаковки и распаковки ключа хранилища (АЕК).

Выберите текущую версию ключа в Azure Key Vault, чтобы получить сведения об идентификаторе ключа.

![Предоставление разрешений Key Vault](media/customer-managed-keys/key-identifier-8bit.png)

Обновите Кэйваултпропертиес в кластере, используя сведения об идентификаторе ключа.

Операция является асинхронной и может занять некоторое время.

```azurecli
az monitor log-analytics cluster update --name "cluster-name" --resource-group "resource-group-name" --key-name "key-name" --key-vault-uri "key-uri" --key-version "key-version"
```

```powershell
Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -KeyVaultUri "key-uri" -KeyName "key-name" -KeyVersion "key-version"
```

```rst
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/cluster-name"?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "keyVaultProperties": {
      "keyVaultUri": "https://key-vault-name.vault.azure.net",
      "kyName": "key-name",
      "keyVersion": "current-version"
  },
  "sku": {
    "name": "CapacityReservation",
    "capacity": 1000
  }
}
```

**Ответ**

Завершение распространения идентификатора ключа занимает несколько минут. Проверить состояние обновления можно двумя способами.
1. Скопируйте значение URL-адреса Azure-AsyncOperation из ответа и выполните [проверку состояния асинхронных операций](#asynchronous-operations-and-status-check).
2. Отправьте запрос GET в кластер и просмотрите свойства *кэйваултпропертиес* . В ответе должны возвращаться недавно обновленные сведения об идентификаторе ключа.

Ответ на запрос GET должен выглядеть следующим образом при завершении обновления идентификатора ключа: 200 ОК и заголовок
```json
{
  "identity": {
    "type": "SystemAssigned",
    "tenantId": "tenant-id",
    "principalId": "principle-id"
    },
  "sku": {
    "name": "capacityReservation",
    "capacity": 1000,
    "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
    },
  "properties": {
    "keyVaultProperties": {
      "keyVaultUri": "https://key-vault-name.vault.azure.net",
      "kyName": "key-name",
      "keyVersion": "current-version"
      },
    "provisioningState": "Succeeded",
    "billingType": "cluster",
    "clusterId": "cluster-id"
  },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```

### <a name="link-workspace-to-cluster"></a>Связывание рабочей области с кластером

Для выполнения этой операции необходимы разрешения "запись" для рабочей области и кластера, в том числе следующие действия:

- В рабочей области: Microsoft.OperationalInsights/workspaces/write
- В кластере: Microsoft. OperationalInsights/Clusters/Write

> [!IMPORTANT]
> Этот шаг следует выполнить только после завершения подготовки кластера Log Analytics. Если вы связываете рабочие области и принимаете данные до подготовки, полученные данные будут удалены и не будут восстановлены.

Эта операция является асинхронной, и ее выполнение может быть завершено.

Выполните процедуру, показанную в [статье выделенные кластеры](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters#link-a-workspace-to-the-cluster).

## <a name="key-revocation"></a>Отзыв ключа

Вы можете отозвать доступ к данным, отключив ключ или удалив политику доступа кластера в Key Vault. Хранилище кластера Log Analytics всегда реагирует на изменения в разрешениях ключей в течение часа или раннее, после чего служба хранилища станет недоступной. Все новые данные, принимаемые в рабочие области, связанные с вашим кластером, будут удалены и не будут восстановлены, данные будут недоступны, а запросы к этим рабочим областям завершатся неудачей. Ранее полученные данные остаются в хранилище, пока кластер и ваши рабочие области не удаляются. Недоступные данные управляются политикой хранения данных и очищаются при достижении окончания срока хранения. 

Полученные данные за последние 14 дней также хранятся в кэше горячего уровня доступа (с поддержкой SSD) для эффективной работы обработчика запросов. Эта операция удаляется из операции отзыва ключа и становится недоступной.

Служба хранилища периодически проверяет Key Vault, чтобы попытаться развернуть ключ шифрования и после доступа к нему в течение 30 минут возобновить прием данных и запрос.

## <a name="key-rotation"></a>Смена ключей

Для смены ключа Customer-Managed требуется явное обновление кластера с новой версией ключа в Azure Key Vault. Следуйте инструкциям в разделе "обновление кластера с подробными сведениями об идентификаторе ключа". Если вы не обновляете новые сведения об идентификаторе ключа в кластере, Log Analytics хранилище кластера будет использовать предыдущий ключ для шифрования. Если вы отключаете или удаляете старый ключ перед обновлением нового ключа в кластере, вы получите состояние [отзыва ключа](#key-revocation) .

После операции смены ключа все данные остаются доступными, так как данные всегда шифруются с помощью ключа шифрования учетной записи (АЕК), а АЕК теперь зашифрован с помощью новой версии ключа шифрования ключей (KEK) в Key Vault.

## <a name="customer-managed-key-for-queries"></a>Customer-Managedный ключ для запросов

Язык запросов, используемый в Log Analytics, является выразительным и может содержать конфиденциальные сведения в комментариях, добавляемых к запросам или в синтаксисе запросов. Некоторые организации требуют, чтобы такие сведения были защищены в соответствии с политикой Customer-Managed Key, и вам нужно сохранить запросы, зашифрованные с помощью ключа. Azure Monitor позволяет хранить запросы, *сохраненные для поиска* и *оповещения журналов* , зашифрованные с помощью ключа в вашей учетной записи хранения при подключении к рабочей области. 

> [!NOTE]
> Log Analytics запросы можно сохранять в различных хранилищах в зависимости от используемого сценария. Запросы остаются зашифрованными с помощью ключа Microsoft Key (ММК) в следующих сценариях независимо от того, Customer-Managed конфигурация ключей: книги в Azure Monitor, панели мониторинга Azure, Azure Logic App, записные книжки Azure и модули Runbook службы автоматизации.

Когда вы перейдете в собственное хранилище (BYOS) и свяжете его с рабочей областью, служба загрузит *Сохраненные операции поиска* и *оповещения журналов* в вашу учетную запись хранения. Это означает, что вы управляете учетной записью хранения и [политикой шифрования неактивных](../../storage/common/customer-managed-keys-overview.md) данных с помощью того же ключа, который используется для шифрования log Analytics кластера или другого ключа. Тем не менее, вы несете ответственность за затраты, связанные с этой учетной записью хранения. 

**Рекомендации по настройке ключа Customer-Managed для запросов**
* У вас должны быть разрешения на запись в рабочую область и в учетную запись хранения.
* Не забудьте создать учетную запись хранения в том же регионе, в котором находится рабочая область Log Analytics
* *Сохранение поиска* в хранилище считается артефактами службы, и их формат может измениться.
* Существующие операции *поиска с сохранением* удаляются из рабочей области. При этом копируются и *сохраняются результаты поиска* , которые необходимы перед настройкой. Вы можете просмотреть *сохраненные и поисковые запросы* с помощью [PowerShell](/powershell/module/az.operationalinsights/get-azoperationalinsightssavedsearch) .
* Журнал запросов не поддерживается, и вы не сможете просматривать запущенные запросы.
* Вы можете связать одну учетную запись хранения с рабочей областью для сохранения запросов, но можно использовать из как *сохраненные-поисковые* запросы, так и *оповещения журнала* .
* Закрепление на панели мониторинга не поддерживается

**Настройка BYOS для сохраненных запросов поиска**

Связывание учетной записи хранения для *запроса* к рабочей области — запросы, *сохраненные для поиска* , сохраняются в учетной записи хранения. 

```azurecli
$storageAccountId = '/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage name>'
az monitor log-analytics workspace linked-storage create --type Query --resource-group "resource-group-name" --workspace-name "workspace-name" --storage-accounts $storageAccountId
```

```powershell
$storageAccount.Id = Get-AzStorageAccount -ResourceGroupName "resource-group-name" -Name "storage-account-name"
New-AzOperationalInsightsLinkedStorageAccount -ResourceGroupName "resource-group-name" -WorkspaceName "workspace-name" -DataSourceType Query -StorageAccountIds $storageAccount.Id
```

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>/linkedStorageAccounts/Query?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "dataSourceType": "Query", 
    "storageAccountIds": 
    [
      "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-account-name>"
    ]
  }
}
```

После настройки любой новый *сохраненный поисковый* запрос будет сохранен в хранилище.

**Настройка BYOS для запросов оповещений журнала**

Свяжите учетную запись хранения с *оповещениями* в рабочей области — запросы на *оповещения журнала* сохраняются в учетной записи хранения. 

```azurecli
$storageAccountId = '/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage name>'
az monitor log-analytics workspace linked-storage create --type ALerts --resource-group "resource-group-name" --workspace-name "workspace-name" --storage-accounts $storageAccountId
```

```powershell
$storageAccount.Id = Get-AzStorageAccount -ResourceGroupName "resource-group-name" -Name "storage-account-name"
New-AzOperationalInsightsLinkedStorageAccount -ResourceGroupName "resource-group-name" -WorkspaceName "workspace-name" -DataSourceType Alerts -StorageAccountIds $storageAccount.Id
```

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>/linkedStorageAccounts/Alerts?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "dataSourceType": "Alerts", 
    "storageAccountIds": 
    [
      "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-account-name>"
    ]
  }
}
```

После настройки любой новый запрос на оповещение будет сохранен в хранилище.

## <a name="customer-lockbox-preview"></a>Защищенное хранилище (Предварительная версия)
Защищенное хранилище предоставляет элементу управления возможность утверждать или отклонять запросы инженера Майкрософт на доступ к данным во время запроса на поддержку.

В Azure Monitor Вы можете управлять данными в рабочих областях, связанных с выделенным кластером Log Analytics. Элемент управления защищенного хранилища применяется к данным, хранящимся в Log Analytics выделенном кластере, где он хранится в учетных записях хранения кластера в защищенной подписке вашего абонента.  

Дополнительные сведения о [защищенное хранилище для Microsoft Azure](../../security/fundamentals/customer-lockbox-overview.md)

## <a name="customer-managed-key-operations"></a>Операции с ключами Customer-Managed

- **Получение всех кластеров в группе ресурсов**
  
  ```azurecli
  az monitor log-analytics cluster list --resource-group "resource-group-name"
  ```

  ```powershell
  Get-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name"
  ```

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  **Ответ**
  
  ```json
  {
    "value": [
      {
        "identity": {
          "type": "SystemAssigned",
          "tenantId": "tenant-id",
          "principalId": "principal-Id"
        },
        "sku": {
          "name": "capacityReservation",
          "capacity": 1000,
          "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
          },
        "properties": {
           "keyVaultProperties": {
              "keyVaultUri": "https://key-vault-name.vault.azure.net",
              "keyName": "key-name",
              "keyVersion": "current-version"
              },
          "provisioningState": "Succeeded",
          "billingType": "cluster",
          "clusterId": "cluster-id"
        },
        "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name",
        "name": "cluster-name",
        "type": "Microsoft.OperationalInsights/clusters",
        "location": "region-name"
      }
    ]
  }
  ```

- **Получение всех кластеров в подписке**

  ```azurecli
  az monitor log-analytics cluster list
  ```

  ```powershell
  Get-AzOperationalInsightsCluster
  ```

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-08-01
  Authorization: Bearer <token>
  ```
    
  **Ответ**
    
  Тот же ответ, что и для "кластера в группе ресурсов", но в области подписки.

- **Обновление *резервирования емкости* в кластере**

  Когда объем данных в связанных рабочих областях изменяется с течением времени и необходимо соответствующим образом обновить уровень резервирования емкости. Выполните [Обновление кластера](#update-cluster-with-key-identifier-details) и укажите новое значение емкости. Это значение может находиться в диапазоне от 1000 до 3000 Гб в день и в шагах 100. Чтобы включить уровень выше 3000 Гб в день, обратитесь к своему контакту Майкрософт. Обратите внимание, что вам не нужно указывать полный текст запроса на оставшуюся часть, но должен быть указан номер SKU:

  ```azurecli
  az monitor log-analytics cluster update --name "cluster-name" --resource-group "resource-group-name" --sku-capacity daily-ingestion-gigabyte
  ```

  ```powershell
  Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -SkuCapacity daily-ingestion-gigabyte
  ```

  ```rst
  PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  Content-type: application/json

  {
    "sku": {
      "name": "capacityReservation",
      "Capacity": daily-ingestion-gigabyte
    }
  }
  ```

- **Обновление *биллингтипе* в кластере**

  Свойство *биллингтипе* определяет атрибуты выставления счетов для кластера и его данных:
  - *кластер* (по умолчанию) — выставление счетов относится к подписке, в которой размещен ваш ресурс "Кластер";
  - *рабочие области*  — выставление счетов относится к подпискам, в которых пропорционально размещаются ваши рабочие области.
  
  Выполните [Обновление кластера](#update-cluster-with-key-identifier-details) и укажите новое значение биллингтипе. Обратите внимание, что вы не обязаны предоставлять полный текст REST-запроса и должны включить в него *billingType* :

  ```rst
  PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  Content-type: application/json

  {
    "properties": {
      "billingType": "cluster",
      }  
  }
  ``` 

- **Удаление связи с рабочей областью**

  Для выполнения этой операции требуются разрешения на запись в рабочую область и кластер. Удалить связь рабочей области из кластера можно в любое время. Новые полученные данные после операции unlink сохраняются в хранилище Log Analytics и шифруются с помощью ключа Microsoft Key. Вы можете запросить данные, которые были переданы в рабочую область, до и после нелегкого разрыва связи при условии, что кластер подготовлен и настроен с допустимым ключом Key Vault.

  Эта операция является асинхронной, и ее выполнение может быть завершено.

  ```azurecli
  az monitor log-analytics workspace linked-service delete --resource-group "resource-group-name" --name "cluster-name" --workspace-name "workspace-name"
  ```

  ```powershell
  Remove-AzOperationalInsightsLinkedService -ResourceGroupName "resource-group-name" -Name "workspace-name" -LinkedServiceName cluster
  ```

  ```rest
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  - **Проверить состояние ссылки на рабочую область**
  
  Выполните операцию Get в рабочей области и проверьте, есть ли в ответе свойство *клустерресаурцеид* в разделе *Features*. Связанная Рабочая область будет иметь свойство *клустерресаурцеид* .

  ```azurecli
  az monitor log-analytics cluster show --resource-group "resource-group-name" --name "cluster-name"
  ```

  ```powershell
  Get-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
  ```

- **Удаление кластера**

  Для выполнения этой операции требуются разрешения на запись в кластер. Операция обратимого удаления выполняется, чтобы разрешить восстановление кластера, включая его данные в течение 14 дней, независимо от того, было ли удаление случайным или умышленно. Имя кластера остается зарезервированным в течение периода обратимого удаления, и вы не можете создать новый кластер с таким именем. После периода обратимого удаления имя кластера освобождается, а кластер и его данные удаляются без возможности восстановления. Связь между связанными рабочими областями удаляется из кластера при операции удаления. Новые принимаемые данные сохраняются в хранилище Log Analytics и шифруются с помощью ключа Майкрософт. 
  
  Операция unlink является асинхронной и может занять до 90 минут.

  ```azurecli
  az monitor log-analytics cluster delete --resource-group "resource-group-name" --name "cluster-name"
  ```
 
  ```powershell
  Remove-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name"
  ```

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  ```
  
- **Восстановление кластера и данных** 
  
  Кластер, который был удален за последние 14 дней, находится в состоянии обратимого удаления и может быть восстановлен с помощью его данных. Так как все рабочие области были удалены из связи с удалением кластера, необходимо повторно связать рабочие области после восстановления кластера. В настоящее время операция восстановления выполняется вручную группой продуктов. Используйте канал Майкрософт или откройте запрос в службу поддержки для восстановления удаленного кластера.

## <a name="limitations-and-constraints"></a>Пределы и ограничения

- Ключ Customer-Managed поддерживается в выделенном кластере Log Analytics и подходит для клиентов, отправляющих 1 ТБ в день или более.

- Максимальное число кластеров на регион и подписку равно 2

- Максимальное число связанных рабочих областей в кластере — 1000

- Вы можете связать рабочую область с кластером, а затем удалить ее связь. Число операций ссылок на рабочую область в конкретной рабочей области ограничено 2 в течение 30 дней.

- Ссылка на рабочую область кластера должна быть выполнена только после того, как была выполнена проверка завершения подготовки кластера Log Analytics.  Данные, отправляемые в рабочую область до завершения, будут удалены и не подлежат восстановлению.

- Шифрование ключей Customer-Managed применяется к вновь принятым данным после времени настройки. Данные, которые были приняты до настройки, остаются зашифрованными с помощью ключа Microsoft Key. Вы можете легко запрашивать данные, полученные до и после Customer-Managed настройки ключа.

- Azure Key Vault должны быть настроены как восстанавливаемые. Эти свойства не включены по умолчанию и должны быть настроены с помощью интерфейса командной строки или PowerShell:<br>
  - [обратимое удаление](../../key-vault/general/soft-delete-overview.md)
  - Чтобы защититься от принудительного удаления секрета или хранилища даже после обратимого удаления, следует включить [защиту](../../key-vault/general/soft-delete-overview.md#purge-protection) от удаления.

- Перемещение кластера в другую группу ресурсов или подписку сейчас не поддерживается.

- Azure Key Vault, кластер и связанные рабочие области должны находиться в одном и том же регионе и в одном клиенте Azure Active Directory (Azure AD), но они могут находиться в разных подписках.

- Ссылка на рабочую область кластера завершится ошибкой, если она связана с другим кластером.

## <a name="troubleshooting"></a>Устранение неполадок

- Поведение при доступности Key Vault:
  - При нормальных операциях. Служба хранилища кэширует АЕК в течение короткого периода времени и возвращается к Key Vault для периодического разворачивания.
    
  - Нерегулярные ошибки подключений. Служба хранилища обрабатывает нерегулярные ошибки (время ожидания, сбои подключений, проблемы DNS), позволяя ключам оставаться в кэше в течение короткого промежутка времени, что приводит к небольшому перебою в доступности. Возможность создавать запросы и принимать данные не прерывается.
    
  - Действующий сайт. Недоступность в течение 30 минут приведет к тому, что учетная запись хранения станет недоступной. Создание запросов недоступно, а полученные данные кэшируются в течение нескольких часов с помощью ключа Майкрософт, чтобы избежать потери данных. При восстановлении доступа к Key Vault запрос становится доступным, а временные кэшированные данные принимаются в хранилище данных и шифруются с помощью Customer-Managed ключа.

  - Частота доступа Key Vault. Частота, с которой Azure Monitor получает доступ к хранилищу Key Vault для операций упаковки и распаковки, составляет от 6 до 60 секунд.

- Если вы создадите кластер и сразу укажете Кэйваултпропертиес, операция может завершиться ошибкой, так как политика доступа не может быть определена, пока кластер не будет назначен системному удостоверению.

- Если вы обновляете существующий кластер с помощью Кэйваултпропертиес и в Key Vault отсутствует политика доступа к ключам "Get", операция завершится ошибкой.

- Если при создании кластера возникает ошибка конфликта, возможно, вы удалили кластер за последние 14 дней и в течение периода обратимого удаления. Имя кластера остается зарезервированным в течение периода обратимого удаления, и вы не можете создать новый кластер с таким именем. Имя освобождается после периода обратимого удаления, когда кластер окончательно удаляется.

- Если вы обновляете кластер во время выполнения операции, операция завершится ошибкой.

- Если вы не развернете кластер, убедитесь, что Azure Key Vault, кластер и связанные рабочие области Log Analytics находятся в одном регионе. Они могут находиться в разных подписках.

- Если вы обновляете версию ключа в Key Vault и не обновляете новые сведения об идентификаторе ключа в кластере, Log Analytics кластер будет использовать предыдущий ключ, и ваши данные станут недоступными. Обновите сведения о новом идентификаторе ключа в кластере, чтобы возобновить прием данных и возможность запрашивать данные.

- Некоторые операции являются длительными и могут занять некоторое время — это создание кластера, обновление ключа кластера и удаление кластера. Проверить состояние операции можно двумя способами.
  1. При использовании функции "ОСТАВШАЯся" скопируйте значение Azure-AsyncOperation URL-адрес из ответа и выполните [проверку состояния асинхронных операций](#asynchronous-operations-and-status-check).
  2. Отправьте запрос GET в кластер или рабочую область и просмотрите ответ. Например, несвязанная Рабочая область не будет иметь *клустерресаурцеид* в разделе " *компоненты* ".

- Чтобы получить поддержку и справку, связанные с ключом, управляемым клиентом, используйте свои контактные данные в Майкрософт.

- Сообщения об ошибках
  
  Создание кластера:
  -  400--недопустимое имя кластера. Имя кластера может содержать символы a – z, A – Z, 0-9 и длину 3-63.
  -  400--текст запроса имеет значение null или имеет неправильный формат.
  -  400--недопустимое имя SKU. Задайте для номера SKU имя КапаЦитиресерватион.
  -  400--Емкость предоставлена, но SKU не КапаЦитиресерватион. Задайте для номера SKU имя КапаЦитиресерватион.
  -  400--отсутствует емкость в номере SKU. Задайте для параметра емкость значение 1000 или выше на шаге 100 (ГБ).
  -  400--Емкость SKU не входит в диапазон. Должно быть не менее 1000 и не больше максимальной допустимой емкости, доступной в разделе "использование и оценочная стоимость" в рабочей области.
  -  400--Емкость заблокирована в течение 30 дней. Уменьшение емкости разрешено через 30 дней после обновления.
  -  400--номер SKU не задан. Задайте для номера SKU значение КапаЦитиресерватион и Capacity, равное 1000 или выше, на шаге 100 (ГБ).
  -  400--Identity имеет значение null или пусто. Задайте удостоверение с типом systemAssigned.
  -  400--Кэйваултпропертиес устанавливаются при создании. Обновление Кэйваултпропертиес после создания кластера.
  -  400--невозможно выполнить операцию сейчас. Асинхронная операция находится в состоянии, отличном от "завершено". Перед выполнением операции обновления кластер должен завершить свою операцию.

  Обновление кластера
  -  400--кластер находится в состоянии удаления. Асинхронная операция выполняется. Перед выполнением операции обновления кластер должен завершить свою операцию.
  -  400--Кэйваултпропертиес не является пустым, но имеет неправильный формат. См. [раздел об обновлении идентификатора ключа](#update-cluster-with-key-identifier-details).
  -  400--не удалось проверить ключ в Key Vault. Возможно, из-за отсутствия разрешений или если ключ не существует. Убедитесь, что [Политика ключа и доступа задана](#grant-key-vault-permissions) в Key Vault.
  -  400-ключ не может быть восстановлен. Для Key Vault должно быть задано обратимое удаление и защита от удаления. См. [документацию по Key Vault](../../key-vault/general/soft-delete-overview.md)
  -  400--невозможно выполнить операцию сейчас. Дождитесь завершения асинхронной операции и повторите попытку.
  -  400--кластер находится в состоянии удаления. Дождитесь завершения асинхронной операции и повторите попытку.

  Получение кластера:
    -  404--кластер не найден, возможно, кластер удален. Если вы попытаетесь создать кластер с этим именем и получить конфликт, кластер будет обратимо удален в течение 14 дней. Вы можете обратиться в службу поддержки, чтобы восстановить ее, или использовать другое имя для создания нового кластера. 

  Удаление кластера
    -  409--не удается удалить кластер в состоянии подготовки. Дождитесь завершения асинхронной операции и повторите попытку.

  Ссылка на рабочую область:
  -  404--Рабочая область не найдена. Указанная Рабочая область не существует или была удалена.
  -  409--ссылка на рабочую область или отмена связи в процессе.
  -  400--кластер не найден, указанный кластер не существует или был удален. Если вы попытаетесь создать кластер с этим именем и получить конфликт, кластер будет обратимо удален в течение 14 дней. Вы можете обратиться в службу поддержки, чтобы восстановить ее.

  Отмена связи рабочей области:
  -  404--Рабочая область не найдена. Указанная Рабочая область не существует или была удалена.
  -  409--ссылка на рабочую область или отмена связи в процессе.
