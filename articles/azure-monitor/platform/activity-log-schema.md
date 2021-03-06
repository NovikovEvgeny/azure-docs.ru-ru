---
title: Схема событий журнала действий Azure
description: Описание схемы событий для каждой категории в журнале действий Azure.
author: bwren
services: azure-monitor
ms.topic: reference
ms.date: 09/30/2020
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 52f0db4086bac7c8131015114ea6ecfdc391a4af
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91612767"
---
# <a name="azure-activity-log-event-schema"></a>Схема событий журнала действий Azure
[Журнал действий Azure](platform-logs-overview.md) позволяет получить представление о любых событиях уровня подписки, произошедших в Azure. В этой статье описываются категории журналов действий и схема для каждой из них. 

Схема зависит от способа доступа к журналу.
 
- Схемы, описанные в этой статье, доступны при доступе к журналу действий из [REST API](/rest/api/monitor/activitylogs). Это также схема, используемая при выборе параметра **JSON** при просмотре события в портал Azure.
- См. последнюю [схему раздела из учетной записи хранения и концентраторов событий](#schema-from-storage-account-and-event-hubs) для схемы при использовании [параметра диагностики](diagnostic-settings.md) для отправки журнала действий в службу хранилища Azure или концентраторы событий Azure.
- Сведения о том, как использовать [параметр диагностики](diagnostic-settings.md) для отправки журнала действий в рабочую область log Analytics, см. в разделе [Azure Monitor Справочник по данным](/azure/azure-monitor/reference/) схемы.

## <a name="severity-level"></a>Степень серьезности
Каждая запись в журнале действий имеет уровень серьезности. Степень серьезности может иметь одно из следующих значений:  

| Уровень серьезности | Описание |
|:---|:---|
| Критический | События, требующие немедленного вмешательства системного администратора. Может означать, что приложение или система завершили работу или перестала отвечать на запросы.
| Ошибка | События, указывающие на проблему, но не требующие немедленного вмешательства.
| Предупреждение | События, которые предоставляют фореварнинг потенциальных проблем, хотя и не являются фактической ошибкой. Укажите, что ресурс не находится в идеальном состоянии и может привести к последующему отображению ошибок или критических событий.  
| Informational | События, передающие некритические сведения администратору. Аналогично примечанию: "сведения о вашей информации". 

Девлоперс каждого поставщика ресурсов выберите степени серьезности записей ресурсов. В результате фактическая степень серьезности можно изменить в зависимости от того, как строится приложение. Например, элементы, являющиеся критически важными для определенного ресурса в ислоатион, могут быть не так важны, как "ошибки", в типе ресурсов, который является центральным для вашего приложения Azure. Этот факт следует учитывать при выборе событий для оповещения.  

## <a name="categories"></a>Категории
Каждое событие в журнале действий имеет определенную категорию, как описано в следующей таблице. Дополнительные сведения о каждой категории и ее схеме при обращении к журналу действий с портала, PowerShell, CLI и REST API см. в следующих разделах. При [потоковой передаче журнала действий в хранилище или концентраторы событий](./resource-logs.md#send-to-azure-event-hubs)схема отличается. Сопоставление свойств [схемам журналов ресурсов](./resource-logs-schema.md) приводится в последнем разделе статьи.

| Категория | Описание |
|:---|:---|
| [Administrative](#administrative-category) | Содержит записи всех операций создания, обновления, удаления и других действий, которые выполняются через Resource Manager. Административными событиями считаются, например, _создание виртуальной машины_ и _удаление группы безопасности сети_.<br><br>Каждое действие, выполняемое пользователем или приложением с помощью Resource Manager, моделируется как операция с определенным типом ресурса. Если операция относится к типу _Запись_, _Удаление_ или _Действие_, то сведения о запуске, успешном выполнении или сбое такой операции относятся к категории "Административная". К административным событиям также относятся любые изменения в управлении доступом на основе ролей, которые происходят в подписке. |
| [Работоспособность служб](#service-health-category) | Содержит записи всех инцидентов, связанных с работоспособностью службы, которые произошли в Azure. Пример события "Работоспособность служб": _SQL Azure in East US is experiencing downtime_ (SQL Azure в регионе "Восточная часть США" не работает). <br><br>Существует шесть разновидностей событий работоспособности служб: _Action Required_ (Требуется действие), _Assisted Recovery_ (Помощь при восстановлении), _Incident_ (Инцидент), _Maintenance_ (Техническое обслуживание), _Information_ (Информация) и _Security_ (Безопасность). Эти события создаются только при наличии в подписке ресурса, на который может повлиять это событие.
| [Работоспособность ресурса](#resource-health-category) | Содержит записи всех событий, связанных с работоспособностью службы, которые произошли с ресурсами Azure. Пример события "Работоспособность ресурсов": _Virtual Machine health status changed to unavailable_ (Состояние работоспособности виртуальной машины изменилось на "недоступно").<br><br>Существует четыре вида событий работоспособности ресурсов: _Available_ (Доступно), _Unavailable_ (Недоступно), _Degraded_ (Понижено) и _Unknown_ (Неизвестно). Кроме того, события работоспособности ресурсов могут классифицироваться по аспекту _PlatformInitiated_ (Инициировано платформой) или _UserInitiated_ (Инициировано пользователем). |
| [Оповещение](#alert-category) | Содержит записи активаций для оповещений Azure. Пример события "Предупреждение": _CPU % on myVM has been over 80 for the past 5 minutes_ (Загрузка ЦП на виртуальной машине myVM в течение последних 5 минут превышает 80 %).|
| [Автомасштабирование](#autoscale-category) | Содержит записи всех событий, связанных с операциями системы автоматического масштабирования, которые основаны на определенных в подписке параметрах автомасштабирования. Пример события "Автомасштабирование": _Autoscale scale up action failed_ (Не удалось выполнить действие автоматического увеличения масштаба). |
| [Рекомендация](#recommendation-category) | Содержит события рекомендаций Помощника по Azure. |
| [Безопасность](#security-category) | Содержит записи любых предупреждений, созданных Центром безопасности Azure. Пример события "Безопасность": _Suspicious double extension file executed_ (Выполнен файл с подозрительным двойным расширением). |
| [Политика](#policy-category) | Содержит записи всех операций действия эффекта, выполняемых Политикой Azure. Примеры событий "Политика": _Audit_ (Аудит) и _Deny_ (Запрет). Каждое действие, выполняемое Политикой, моделируется как операция для ресурса. |

## <a name="administrative-category"></a>Административная категория
Эта категория содержит записи всех операций создания, обновления, удаления и других действий, которые выполняются через Resource Manager. В качестве примеров типов событий, которые относятся к этой категории, можно назвать "создание виртуальной машины" или "удаление группы безопасности сети". Каждое действие, выполняемое пользователем или приложением с помощью Resource Manager, моделируется как операция с определенным типом ресурсов. Если тип операции — "запись", "удаление" или "действие", то любые сведения об этой операции (о ее запуске, успешном выполнении или сбое) записываются в категорию "Административная". Категория "Административная" также включает в себя любые изменения в управлении доступом на основе ролей, которые происходят в подписке.

### <a name="sample-event"></a>Событие выборки
```json
{
    "authorization": {
        "action": "Microsoft.Network/networkSecurityGroups/write",
        "scope": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG"
    },
    "caller": "rob@contoso.com",
    "channels": "Operation",
    "claims": {
        "aud": "https://management.core.windows.net/",
        "iss": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "iat": "1234567890",
        "nbf": "1234567890",
        "exp": "1234567890",
        "_claim_names": "{\"groups\":\"src1\"}",
        "_claim_sources": "{\"src1\":{\"endpoint\":\"https://graph.microsoft.com/1114444b-7467-4144-a616-e3a5d63e147b/users/f409edeb-4d29-44b5-9763-ee9348ad91bb/getMemberObjects\"}}",
        "http://schemas.microsoft.com/claims/authnclassreference": "1",
        "aio": "A3GgTJdwK4vy7Fa7l6DgJC2mI0GX44tML385OpU1Q+z+jaPnFMwB",
        "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
        "appid": "355249ed-15d9-460d-8481-84026b065942",
        "appidacr": "2",
        "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "10845a4d-ffa4-4b61-a3b4-e57b9b31cdb5",
        "e_exp": "262800",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Robertson",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Rob",
        "ipaddr": "111.111.1.111",
        "name": "Rob Robertson",
        "http://schemas.microsoft.com/identity/claims/objectidentifier": "f409edeb-4d29-44b5-9763-ee9348ad91bb",
        "onprem_sid": "S-1-5-21-4837261184-168309720-1886587427-18514304",
        "puid": "18247BBD84827C6D",
        "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "b-24Jf94A3FH2sHWVIFqO3-RSJEiv24Jnif3gj7s",
        "http://schemas.microsoft.com/identity/claims/tenantid": "1114444b-7467-4144-a616-e3a5d63e147b",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "rob@contoso.com",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "rob@contoso.com",
        "uti": "IdP3SUJGtkGlt7dDQVRPAA",
        "ver": "1.0"
    },
    "correlationId": "b5768deb-836b-41cc-803e-3f4de2f9e40b",
    "eventDataId": "d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d",
    "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
    },
    "category": {
        "value": "Administrative",
        "localizedValue": "Administrative"
    },
    "eventTimestamp": "2018-01-29T20:42:31.3810679Z",
    "id": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG/events/d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d/ticks/636528553513810679",
    "level": "Informational",
    "operationId": "04e575f8-48d0-4c43-a8b3-78c4eb01d287",
    "operationName": {
        "value": "Microsoft.Network/networkSecurityGroups/write",
        "localizedValue": "Microsoft.Network/networkSecurityGroups/write"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Network",
        "localizedValue": "Microsoft.Network"
    },
    "resourceType": {
        "value": "Microsoft.Network/networkSecurityGroups",
        "localizedValue": "Microsoft.Network/networkSecurityGroups"
    },
    "resourceId": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-01-29T20:42:50.0724829Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "statusCode": "Created",
        "serviceRequestId": "a4c11dbd-697e-47c5-9663-12362307157d",
        "responseBody": "",
        "requestbody": ""
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Описания свойств
| Имя элемента | Описание |
| --- | --- |
| авторизация |BLOB-объект со свойствами RBAC события. Обычно включает следующие свойства: action, role и scope. |
| caller |Адрес электронной почты пользователя, который выполнил операцию, утверждение имени субъекта-службы или имени участника-пользователя в зависимости от доступности. |
| каналов |Одно из следующих значений: Admin или Operation. |
| claims |Токен JWT, который используется Active Directory для аутентификации пользователя или приложения при выполнении этой операции в Resource Manager. |
| correlationId |Обычно GUID в строковом формате. События, которые совместно используют идентификатор correlationId, принадлежат к одному общему действию. |
| description |Статическое описание события в текстовом виде. |
| eventDataId |Уникальный идентификатор события. |
| eventName | Понятное имя административного события. |
| категория | Всегда "административный" |
| httpRequest |Большой двоичный объект, описывающий HTTP-запрос. Обычно включает clientRequestId, clientIpAddress и method (метод HTTP, например PUT). |
| уровень |Уровень события. Одно из таких значений: Critical, Error, Warning, Informational. |
| имя_группы_ресурсов |Имя группы ресурсов для затронутого ресурса. |
| resourceProviderName |Имя поставщика ресурса для затронутого ресурса. |
| тип_ресурса | Тип ресурса, затронутого административным событием. |
| resourceId |Идентификатор ресурса для затронутого ресурса. |
| operationId |События, относящиеся к одной операции, совместно используют один GUID. |
| operationName |Имя операции. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием события. |
| status |Строка, описывающая состояние операции. Обычные значения: Started, In Progress, Succeeded, Failed, Active, Resolved. |
| subStatus |Обычно содержит код состояния HTTP для соответствующего вызова REST, но может содержать и другие строковые описания подсостояния, например: OK (код состояния HTTP: 200); Created (код состояния HTTP: 201); Accepted (код состояния HTTP: 202); No Content (код состояния HTTP: 204); Bad Request (код состояния HTTP: 400); Not Found (код состояния HTTP: 404); Conflict (код состояния HTTP: 409); Internal Server Error (код состояния HTTP: 500); Service Unavailable (код состояния HTTP: 503); Gateway Timeout (код состояния HTTP: 504). |
| eventTimestamp |Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| submissionTimestamp |Метка времени, когда событие стало доступно для запросов. |
| subscriptionId |Идентификатор подписки Azure. |

## <a name="service-health-category"></a>Категория «работоспособность служб»
Эта категория содержит записи всех инцидентов, связанных с работоспособностью службы, которые произошли в Azure. В качестве примера типа события из этой категории можно назвать следующее: "В работе SQL Azure в восточной части США наблюдаются простои". Существует шесть разновидностей событий работоспособности службы: "Требуется действие", "Помощь при восстановлении", "Инцидент", "Обслуживание", "Сведения" и "Безопасность". Они отображаются, только если в подписке есть ресурс, на который повлияет данное событие.

### <a name="sample-event"></a>Событие выборки
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/<subscription ID>/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/<subscription ID>",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "<subscription ID>",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Cognitive Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Cognitive Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```
Дополнительные сведения о значениях в свойствах см. в статье об [уведомлениях о работоспособности службы](../../service-health/service-notifications.md).

## <a name="resource-health-category"></a>Категория работоспособности ресурсов
Эта категория содержит записи всех событий, связанных с работоспособностью службы, которые произошли с ресурсами Azure. Пример события такого типа из этой категории: Virtual Machine health status changed to unavailable (Состояние работоспособности виртуальной машины изменилось на "Недоступно"). События работоспособности ресурсов соответствуют одному из четырех состояний: Available (Доступно), Unavailable (Недоступно), Degraded (Понижено) и Unknown (Неизвестно). Кроме того, события работоспособности ресурсов можно классифицировать по свойствам PlatformInitiated (Инициировано платформой) и UserInitiated (Инициировано пользователем).

### <a name="sample-event"></a>Событие выборки

```json
{
    "channels": "Admin, Operation",
    "correlationId": "28f1bfae-56d3-7urb-bff4-194d261248e9",
    "description": "",
    "eventDataId": "a80024e1-883d-37ur-8b01-7591a1befccb",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "ResourceHealth",
        "localizedValue": "Resource Health"
    },
    "eventTimestamp": "2018-09-04T15:33:43.65Z",
    "id": "/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<resource name>/events/a80024e1-883d-42a5-8b01-7591a1befccb/ticks/636716720236500000",
    "level": "Critical",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Resourcehealth/healthevent/Activated/action",
        "localizedValue": "Health Event Activated"
    },
    "resourceGroupName": "<resource group>",
    "resourceProviderName": {
        "value": "Microsoft.Resourcehealth/healthevent/action",
        "localizedValue": "Microsoft.Resourcehealth/healthevent/action"
    },
    "resourceType": {
        "value": "Microsoft.Compute/virtualMachines",
        "localizedValue": "Microsoft.Compute/virtualMachines"
    },
    "resourceId": "/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<resource name>",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-09-04T15:36:24.2240867Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "stage": "Active",
        "title": "Virtual Machine health status changed to unavailable",
        "details": "Virtual machine has experienced an unexpected event",
        "healthStatus": "Unavailable",
        "healthEventType": "Downtime",
        "healthEventCause": "PlatformInitiated",
        "healthEventCategory": "Unplanned"
    },
    "relatedEvents": []
}
```

### <a name="property-descriptions"></a>Описания свойств
| Имя элемента | Описание |
| --- | --- |
| каналов | Всегда имеет значение "Admin, Operation". |
| correlationId | Глобальный уникальный идентификатор (GUID) в строковом формате. |
| description |Статическое текстовое описание события оповещения. |
| eventDataId |Уникальный идентификатор события оповещения. |
| категория | Всегда ResourceHealth. |
| eventTimestamp |Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| уровень |Уровень события. Принимает одно из следующих значений: Critical (Критическое), Error (Ошибка), Warning (Предупреждение), Informational (Информационное) или Verbose (Подробные сведения). |
| operationId |События, относящиеся к одной операции, совместно используют один GUID. |
| operationName |Имя операции. |
| имя_группы_ресурсов |Имя группы ресурсов, к которой относится ресурс. |
| resourceProviderName |Всегда Microsoft.Resourcehealth/healthevent/action. |
| тип_ресурса | Тип ресурса, на который повлияло событие работоспособности ресурса. |
| resourceId | Имя идентификатора затронутого ресурса. |
| status |Строка, описывающая состояние события работоспособности. Возможные значения: Active (Активно), Resolved (Разрешено), InProgress (Выполняется), Updated (Обновлено). |
| subStatus | Обычно для оповещений имеет значение null. |
| submissionTimestamp |Метка времени, когда событие стало доступно для запросов. |
| subscriptionId |Идентификатор подписки Azure. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием события.|
| properties.title | Понятная для пользователя строка с описанием состояния работоспособности ресурса. |
| properties.details | Понятная для пользователя строка с описанием подробностей события. |
| properties.currentHealthStatus | Текущее состояние работоспособности ресурса. Возможные значения: Available (Доступно), Unavailable (Недоступно), Degraded (Понижено) и Unknown (Неизвестно). |
| properties.previousHealthStatus | Предыдущее состояние работоспособности ресурса. Возможные значения: Available (Доступно), Unavailable (Недоступно), Degraded (Понижено) и Unknown (Неизвестно). |
| properties.type | Описание типа события работоспособности ресурса. |
| properties.cause | Описание причины события работоспособности ресурса: UserInitiated либо PlatformInitiated. |


## <a name="alert-category"></a>Категория предупреждения
Эта категория содержит записи обо всех активациях классических оповещений Azure. В качестве примера типа события из этой категории можно назвать следующее: "Процент использования ЦП на виртуальной машине myVM за последние 5 минут превышал 80 %". Различные системы Azure работают по принципу оповещений, то есть вы можете определить какое-то правило, и система будет направлять вам уведомление, когда условия этого правила выполняются. Каждый раз, когда "активируется" поддерживаемый тип оповещения Azure или выполняются условия для создания уведомления, в эту категорию журнала активности также передается запись об активации.

### <a name="sample-event"></a>Событие выборки

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in the last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "<subscription ID>"
}
```

### <a name="property-descriptions"></a>Описания свойств
| Имя элемента | Описание |
| --- | --- |
| caller | Всегда имеет значение Microsoft.Insights/alertRules. |
| каналов | Всегда имеет значение "Admin, Operation". |
| claims | Большой двоичный объект JSON с именем субъекта-службы (SPN) или типом ресурса обработчика предупреждений. |
| correlationId | Глобальный уникальный идентификатор (GUID) в строковом формате. |
| description |Статическое текстовое описание события оповещения. |
| eventDataId |Уникальный идентификатор события оповещения. |
| категория | Всегда "предупреждение" |
| уровень |Уровень события. Одно из таких значений: Critical, Error, Warning, Informational. |
| имя_группы_ресурсов |Имя группы ресурсов для затронутого ресурса, если это оповещение метрики. Для других типов оповещений это имя группы ресурсов, содержащей само оповещение. |
| resourceProviderName |Имя поставщика ресурсов для затронутого ресурса, если это оповещение метрики. Для других типов оповещений это имя поставщика ресурсов для самого оповещения. |
| resourceId | Имя идентификатора ресурса для затронутого ресурса, если это оповещение метрики. Для других типов оповещений это идентификатор ресурса самого ресурса оповещения. |
| operationId |События, относящиеся к одной операции, совместно используют один GUID. |
| operationName |Имя операции. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием события. |
| status |Строка, описывающая состояние операции. Обычные значения: Started, In Progress, Succeeded, Failed, Active, Resolved. |
| subStatus | Обычно для оповещений имеет значение null. |
| eventTimestamp |Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| submissionTimestamp |Метка времени, когда событие стало доступно для запросов. |
| subscriptionId |Идентификатор подписки Azure. |

### <a name="properties-field-per-alert-type"></a>Поле свойств по типу оповещения
Поле свойств будет содержать разные значения в зависимости от источника события оповещения. Два распространенных поставщика событий оповещений — оповещения журнала действий и оповещения метрик.

#### <a name="properties-for-activity-log-alerts"></a>Свойства оповещений журнала действий
| Имя элемента | Описание |
| --- | --- |
| properties.subscriptionId | Идентификатор подписки из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий. |
| properties.eventDataId | Идентификатор данных события из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий. |
| properties.resourceGroup | Идентификатор группы ресурсов из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий. |
| properties.resourceId | Идентификатор ресурса из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий. |
| properties.eventTimestamp | Метка времени события из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий. |
| properties.operationName | Имя операции из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий. |
| properties.status | Состояние из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий.|

#### <a name="properties-for-metric-alerts"></a>Свойства оповещений метрик
| Имя элемента | Описание |
| --- | --- |
| properties.RuleUri | Идентификатор ресурса самого правила оповещения метрики. |
| properties.RuleName | Имя правила оповещения метрики. |
| properties.RuleDescription | Описание правила оповещения метрики (как указано в правиле оповещения). |
| properties.Threshold | Пороговое значение, используемое для оценки правила оповещения метрики. |
| properties.WindowSizeInMinutes | Размер окна, используемый для оценки правила оповещения метрики. |
| properties.Aggregation | Тип агрегата, определенный в правиле оповещения метрики. |
| properties.Operator | Условный оператор, используемый для оценки правила оповещения метрики. |
| properties.MetricName | Имя метрики, используемой для оценки правила оповещения метрики. |
| properties.MetricUnit | Единица измерения метрики, используемая для оценки правила оповещения метрики. |

## <a name="autoscale-category"></a>Категория автомасштабирования
Эта категория содержит записи всех событий, связанных с операциями системы автоматического масштабирования, в основе которой лежат параметры автомасштабирования, определенные в подписке. В качестве примера типа события из этой категории можно назвать следующее: "Не удалось выполнить автоматическое увеличение масштаба". С помощью функции автомасштабирования можно автоматически увеличивать или уменьшать число экземпляров в поддерживаемом типе ресурсов на основе времени суток и (или) загружать данные (метрики), используя параметр автомасштабирования. Когда выполняются условия для увеличения или уменьшения масштаба, в эту категорию записываются соответствующие события (запуск, успешное выполнение или сбой).

### <a name="sample-event"></a>Событие выборки
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "The autoscale engine attempting to scale resource '/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "The autoscale engine attempting to scale resource '/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
    "ResourceName": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "<subscription ID>"
}

```

### <a name="property-descriptions"></a>Описания свойств
| Имя элемента | Описание |
| --- | --- |
| caller | Всегда имеет значение Microsoft.Insights/autoscaleSettings. |
| каналов | Всегда имеет значение "Admin, Operation". |
| claims | Большой двоичный объект JSON с именем субъекта-службы (SPN) или типом ресурса системы автомасштабирования. |
| correlationId | Глобальный уникальный идентификатор (GUID) в строковом формате. |
| description |Статическое текстовое описание события автомасштабирования. |
| eventDataId |Уникальный идентификатор события автомасштабирования. |
| уровень |Уровень события. Одно из таких значений: Critical, Error, Warning, Informational. |
| имя_группы_ресурсов |Имя группы ресурсов для параметра автомасштабирования. |
| resourceProviderName |Имя поставщика ресурсов для параметра автомасштабирования. |
| resourceId |Идентификатор ресурса параметра автомасштабирования. |
| operationId |События, относящиеся к одной операции, совместно используют один GUID. |
| operationName |Имя операции. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием события. |
| properties.Description | Подробное описание действий, выполненных системой автоматического масштабирования. |
| properties.ResourceName | Идентификатор ресурса для затронутого ресурса (ресурса, в отношении которого выполнялось действие масштабирования). |
| properties.OldInstancesCount | Число экземпляров до выполнения действия автомасштабирования. |
| properties.NewInstancesCount | Число экземпляров после выполнения действия автомасштабирования. |
| properties.LastScaleActionTime | Метка времени, когда произошло действие автоматического масштабирования. |
| status |Строка, описывающая состояние операции. Обычные значения: Started, In Progress, Succeeded, Failed, Active, Resolved. |
| subStatus | Обычно для автоматического масштабирования имеет значение null. |
| eventTimestamp |Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| submissionTimestamp |Метка времени, когда событие стало доступно для запросов. |
| subscriptionId |Идентификатор подписки Azure. |

## <a name="security-category"></a>Категория безопасности
Эта категория содержит запись любых предупреждений, созданных центром безопасности Azure. Примером типа события в этой категории является "Выполнен подозрительный файл с двойным расширением".

### <a name="sample-event"></a>Событие выборки
```json
{
    "channels": "Operation",
    "correlationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "description": "Suspicious double extension file executed. Machine logs indicate an execution of a process with a suspicious double extension.\r\nThis extension may trick users into thinking files are safe to be opened and might indicate the presence of malware on the system.",
    "eventDataId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "eventName": {
        "value": "Suspicious double extension file executed",
        "localizedValue": "Suspicious double extension file executed"
    },
    "category": {
        "value": "Security",
        "localizedValue": "Security"
    },
    "eventTimestamp": "2017-10-18T06:02:18.6179339Z",
    "id": "/subscriptions/<subscription ID>/providers/Microsoft.Security/locations/centralus/alerts/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/events/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/ticks/636439033386179339",
    "level": "Informational",
    "operationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "operationName": {
        "value": "Microsoft.Security/locations/alerts/activate/action",
        "localizedValue": "Microsoft.Security/locations/alerts/activate/action"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Security",
        "localizedValue": "Microsoft.Security"
    },
    "resourceType": {
        "value": "Microsoft.Security/locations/alerts",
        "localizedValue": "Microsoft.Security/locations/alerts"
    },
    "resourceId": "/subscriptions/<subscription ID>/providers/Microsoft.Security/locations/centralus/alerts/2518939942613820660_a48f8653-3fc6-4166-9f19-914f030a13d3",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": null
    },
    "submissionTimestamp": "2017-10-18T06:02:52.2176969Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "accountLogonId": "0x2r4",
        "commandLine": "c:\\mydirectory\\doubleetension.pdf.exe",
        "domainName": "hpc",
        "parentProcess": "unknown",
        "parentProcess id": "0",
        "processId": "6988",
        "processName": "c:\\mydirectory\\doubleetension.pdf.exe",
        "userName": "myUser",
        "UserSID": "S-3-2-12",
        "ActionTaken": "Detected",
        "Severity": "High"
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Описания свойств
| Имя элемента | Описание |
| --- | --- |
| каналов | Всегда значение Operation. |
| correlationId | Глобальный уникальный идентификатор (GUID) в строковом формате. |
| description |Статическое текстовое описание события безопасности. |
| eventDataId |Уникальный идентификатор события безопасности. |
| eventName |Понятное имя события безопасности. |
| категория | Всегда «безопасность» |
| ID |Уникальный идентификатор ресурса события безопасности. |
| уровень |Уровень события. Одно из таких значений: Critical, Error, Warning или Informational. |
| имя_группы_ресурсов |Имя группы ресурсов для ресурса. |
| resourceProviderName |Имя поставщика ресурсов для центра безопасности Azure. Всегда значение Microsoft.Security. |
| тип_ресурса |Тип ресурса, создавшего событие безопасности, например Microsoft.Security/locations/alerts. |
| resourceId |Идентификатор ресурса оповещения системы безопасности. |
| operationId |События, относящиеся к одной операции, совместно используют один GUID. |
| operationName |Имя операции. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием события. Эти свойства будут различаться в зависимости от типа оповещения системы безопасности. [Здесь](../../security-center/security-center-alerts-overview.md) описаны типы предупреждений, поступающих из центра безопасности. |
| properties.Severity |Уровень серьезности. Возможные значения: High, Medium или Low. |
| status |Строка, описывающая состояние операции. Обычные значения: Started, In Progress, Succeeded, Failed, Active, Resolved. |
| subStatus | Обычно имеет значение null для событий безопасности. |
| eventTimestamp |Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| submissionTimestamp |Метка времени, когда событие стало доступно для запросов. |
| subscriptionId |Идентификатор подписки Azure. |

## <a name="recommendation-category"></a>Категория рекомендации
Эта категория содержит запись о любых новых рекомендациях, которые создаются для служб. Примером рекомендации будет "использование группы доступности для повышения отказоустойчивости". Существует четыре типа событий рекомендаций, которые могут быть созданы: высокая доступность, производительность, безопасность и оптимизация затрат. 

### <a name="sample-event"></a>Событие выборки
```json
{
    "channels": "Operation",
    "correlationId": "92481dfd-c5bf-4752-b0d6-0ecddaa64776",
    "description": "The action was successful.",
    "eventDataId": "06cb0e44-111b-47c7-a4f2-aa3ee320c9c5",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "Recommendation",
        "localizedValue": "Recommendation"
    },
    "eventTimestamp": "2018-06-07T21:30:42.976919Z",
    "id": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/MYVM/events/06cb0e44-111b-47c7-a4f2-aa3ee320c9c5/ticks/636640038429769190",
    "level": "Informational",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Advisor/generateRecommendations/action",
        "localizedValue": "Microsoft.Advisor/generateRecommendations/action"
    },
    "resourceGroupName": "MYRESOURCEGROUP",
    "resourceProviderName": {
        "value": "MICROSOFT.COMPUTE",
        "localizedValue": "MICROSOFT.COMPUTE"
    },
    "resourceType": {
        "value": "MICROSOFT.COMPUTE/virtualmachines",
        "localizedValue": "MICROSOFT.COMPUTE/virtualmachines"
    },
    "resourceId": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/MYVM",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-06-07T21:30:42.976919Z",
    "subscriptionId": "<Subscription ID>",
    "properties": {
        "recommendationSchemaVersion": "1.0",
        "recommendationCategory": "Security",
        "recommendationImpact": "High",
        "recommendationRisk": "None"
    },
    "relatedEvents": []
}

```
### <a name="property-descriptions"></a>Описания свойств
| Имя элемента | Описание |
| --- | --- |
| каналов | Всегда значение Operation. |
| correlationId | Глобальный уникальный идентификатор (GUID) в строковом формате. |
| description |Статическое текстовое описание события рекомендации |
| eventDataId | Уникальный идентификатор события рекомендации. |
| категория | Всегда "Рекомендация" |
| ID |Уникальный идентификатор ресурса события рекомендации. |
| уровень |Уровень события. Одно из таких значений: Critical, Error, Warning или Informational. |
| operationName |Имя операции.  Всегда "Microsoft.Advisor/generateRecommendations/action"|
| имя_группы_ресурсов |Имя группы ресурсов для ресурса. |
| resourceProviderName |Имя поставщика ресурсов для ресурса, к которому применяется данная рекомендация, например "MICROSOFT.COMPUTE" |
| тип_ресурса |Имя типа ресурсов для ресурса, к которому применяется данная рекомендация, например "MICROSOFT.COMPUTE/virtualmachines" |
| resourceId |Идентификатор ресурса для ресурса, к которому применяется рекомендация |
| status | Всегда "Active" |
| submissionTimestamp |Метка времени, когда событие стало доступно для запросов. |
| subscriptionId |Идентификатор подписки Azure. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием рекомендации.|
| properties.recommendationSchemaVersion| Версия схемы свойств рекомендации, опубликована в записи журнала действий |
| properties.recommendationCategory | Категория рекомендации. Возможные значения: "Высокая доступность", "Производительность", "Безопасность" и "Стоимость" |
| properties.recommendationImpact| Влияние рекомендации Возможные значения: High, Medium или Low |
| properties.recommendationRisk| Риск рекомендации. Возможные значения: Error, Warning, None |

## <a name="policy-category"></a>Категория политики

Эта категория содержит записи всех операций действия эффекта, выполняемых [Политикой Azure](../../governance/policy/overview.md). Примеры типов событий, которые можно увидеть в этой категории: _Аудит_ и _отказ_. Каждое действие, выполняемое Политикой, моделируется как операция для ресурса.

### <a name="sample-policy-event"></a>Пример события Политики

```json
{
    "authorization": {
        "action": "Microsoft.Resources/checkPolicyCompliance/read",
        "scope": "/subscriptions/<subscriptionID>"
    },
    "caller": "33a68b9d-63ce-484c-a97e-94aef4c89648",
    "channels": "Operation",
    "claims": {
        "aud": "https://management.azure.com/",
        "iss": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "iat": "1234567890",
        "nbf": "1234567890",
        "exp": "1234567890",
        "aio": "A3GgTJdwK4vy7Fa7l6DgJC2mI0GX44tML385OpU1Q+z+jaPnFMwB",
        "appid": "1d78a85d-813d-46f0-b496-dd72f50a3ec0",
        "appidacr": "2",
        "http://schemas.microsoft.com/identity/claims/identityprovider": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "http://schemas.microsoft.com/identity/claims/objectidentifier": "f409edeb-4d29-44b5-9763-ee9348ad91bb",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "b-24Jf94A3FH2sHWVIFqO3-RSJEiv24Jnif3gj7s",
        "http://schemas.microsoft.com/identity/claims/tenantid": "1114444b-7467-4144-a616-e3a5d63e147b",
        "uti": "IdP3SUJGtkGlt7dDQVRPAA",
        "ver": "1.0"
    },
    "correlationId": "b5768deb-836b-41cc-803e-3f4de2f9e40b",
    "description": "",
    "eventDataId": "d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d",
    "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
    },
    "category": {
        "value": "Policy",
        "localizedValue": "Policy"
    },
    "eventTimestamp": "2019-01-15T13:19:56.1227642Z",
    "id": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/contososqlpolicy/events/13bbf75f-36d5-4e66-b693-725267ff21ce/ticks/636831551961227642",
    "level": "Warning",
    "operationId": "04e575f8-48d0-4c43-a8b3-78c4eb01d287",
    "operationName": {
        "value": "Microsoft.Authorization/policies/audit/action",
        "localizedValue": "Microsoft.Authorization/policies/audit/action"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Sql",
        "localizedValue": "Microsoft SQL"
    },
    "resourceType": {
        "value": "Microsoft.Resources/checkPolicyCompliance",
        "localizedValue": "Microsoft.Resources/checkPolicyCompliance"
    },
    "resourceId": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/contososqlpolicy",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-01-15T13:20:17.1077672Z",
    "subscriptionId": "<subscriptionID>",
    "properties": {
        "isComplianceCheck": "True",
        "resourceLocation": "westus2",
        "ancestors": "72f988bf-86f1-41af-91ab-2d7cd011db47",
        "policies": "[{\"policyDefinitionId\":\"/subscriptions/<subscriptionID>/providers/Microsoft.
            Authorization/policyDefinitions/5775cdd5-d3d3-47bf-bc55-bb8b61746506/\",\"policyDefiniti
            onName\":\"5775cdd5-d3d3-47bf-bc55-bb8b61746506\",\"policyDefinitionEffect\":\"Deny\",\"
            policyAssignmentId\":\"/subscriptions/<subscriptionID>/providers/Microsoft.Authorization
            /policyAssignments/991a69402a6c484cb0f9b673/\",\"policyAssignmentName\":\"991a69402a6c48
            4cb0f9b673\",\"policyAssignmentScope\":\"/subscriptions/<subscriptionID>\",\"policyAssig
            nmentParameters\":{}}]"
    },
    "relatedEvents": []
}
```

### <a name="policy-event-property-descriptions"></a>Описания свойств события Политики

| Имя элемента | Описание |
| --- | --- |
| авторизация | Массив со свойствами RBAC события. Для новых ресурсов — это действие и область запроса, вызвавшего вычисление. Для имеющихся ресурсов действием является "Microsoft.Resources/checkPolicyCompliance/read". |
| caller | Для новых ресурсов — это удостоверение, которое запустило развертывание. Для имеющихся ресурсов — глобальный уникальный идентификатор поставщика ресурсов Microsoft Azure Policy Insights. |
| каналов | События политики используют только канал "Operation". |
| claims | Токен JWT, который используется Active Directory для аутентификации пользователя или приложения при выполнении этой операции в Resource Manager. |
| correlationId | Обычно GUID в строковом формате. События, которые совместно используют идентификатор correlationId, принадлежат к одному общему действию. |
| description | Это поле будет пустым для событий Политики. |
| eventDataId | Уникальный идентификатор события. |
| eventName | "BeginRequest" или "EndRequest". "BeginRequest" используется для отложенной оценки auditIfNotExists и deployIfNotExists, а также при запуске эффектом deployIfNotExists развертывания шаблона. Все остальные операции возвращают "EndRequest". |
| категория | Объявляет события журнала действий как относящиеся к "Policy". |
| eventTimestamp | Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| ID | Уникальный идентификатор события для конкретного ресурса. |
| уровень | Уровень события. Audit использует значение "Warning", а Deny — "Error". Ошибка auditIfNotExists или deployIfNotExists может создать значения "Warning" и "Error" в зависимости от серьезности. Все события Политики используют значение "Informational". |
| operationId | События, относящиеся к одной операции, совместно используют один GUID. |
| operationName | Имя операции непосредственно коррелируется c эффектом Политики. |
| имя_группы_ресурсов | Имя группы ресурсов для оцениваемого ресурса. |
| resourceProviderName | Имя поставщика ресурсов для оцениваемого ресурса. |
| тип_ресурса | Для новых ресурсов — это оцениваемый тип. Для имеющихся ресурсов — возвращает "Microsoft.Resources/checkPolicyCompliance/read". |
| resourceId | Идентификатор оцениваемого ресурса. |
| status | Строка, описывающая состояние результата оценки Политики. Большинство оценок Политики возвращают значение "Succeeded", но эффект Deny возвращает значение "Failed". Ошибки в auditIfNotExists или deployIfNotExists также возвращают значение "Failed". |
| subStatus | Поле будет пустым для событий Политики. |
| submissionTimestamp | Метка времени, когда событие стало доступно для запросов. |
| subscriptionId | Идентификатор подписки Azure. |
| properties.isComplianceCheck | Возвращает значение "False" при развертывании нового ресурса или обновлении свойств Azure Resource Manager имеющегося ресурса. Все остальные [триггеры оценки](../../governance/policy/how-to/get-compliance-data.md#evaluation-triggers) приводят к значению "True". |
| properties.resourceLocation | Регион Azure оцениваемого ресурса. |
| properties.ancestors | Разделенный запятыми список родительских групп управления, упорядоченный от непосредственно родительской к самой дальней родительской. |
| properties.policies | Содержит сведения об определении политики, назначении, влиянии и параметрах, результатом которых является эта оценка Политики. |
| relatedEvents | Это поле будет пустым для событий Политики. |


## <a name="schema-from-storage-account-and-event-hubs"></a>Схема из учетной записи хранения и концентраторов событий
При потоковой передаче журнала действий Azure в учетную запись хранения или концентратор событий данные следуют за [схемой журнала ресурсов](./resource-logs-schema.md). В таблице ниже приведено сопоставление свойств из указанных выше схем с схемой журналов ресурсов.

> [!IMPORTANT]
> Формат данных журнала действий, записываемых в учетную запись хранения, изменился на строки JSON в 2018 ноября. 1. Дополнительные сведения об этом изменении формата см. в разделе [Подготовка к изменению формата для Azure Monitor журналов ресурсов, архивных в учетной записи хранения](./resource-logs-blob-format.md) .


| Свойство схемы журналов ресурсов | Свойство схемы журнала действий REST API | Примечания |
| --- | --- | --- |
| time | eventTimestamp |  |
| resourceId | resourceId | Параметры subscriptionId, resourceType и resourceGroupName получаются из параметра resourceId. |
| operationName | operationName.value |  |
| категория | Часть имени операции | Разделитель типа операции — "Запись", "Удаление" или "Действие" |
| resultType | status.value | |
| resultSignature | substatus.value | |
| resultDescription | description |  |
| durationMs | Недоступно | Всегда 0 |
| callerIpAddress | httpRequest.clientIpAddress |  |
| correlationId | correlationId |  |
| удостоверение | свойства удостоверений и авторизации |  |
| Level | Level |  |
| location | Недоступно | Расположение, где было обработано событие. *Это не расположение ресурса, а, если событие было обработано. Это свойство будет удалено в будущем обновлении.* |
| Свойства | properties.eventProperties |  |
| properties.eventCategory | категория | Если свойство properties.eventCategory не задано, параметр category равен "Административная" |
| properties.eventName | eventName |  |
| properties.operationId | operationId |  |
| properties.eventProperties | properties |  |

Ниже приведен пример события, использующего эту схему.

``` JSON
{
    "records": [
        {
            "time": "2019-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "00000000-0000-0000-0000-000000000000",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```





## <a name="next-steps"></a>Дальнейшие шаги
* [Дополнительные сведения о журнале действий](platform-logs-overview.md)
* [Создание параметра диагностики для отправки журнала действий в Log Analytics рабочую область, службу хранилища Azure или концентраторы событий](diagnostic-settings.md)
