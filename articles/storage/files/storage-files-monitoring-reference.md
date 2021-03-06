---
title: Справочник по данным мониторинга файлов Azure | Документация Майкрософт
description: Справочник по журналам и метрикам для мониторинга данных из файлов Azure.
author: normesta
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 10/02/2020
ms.author: normesta
ms.subservice: logs
ms.custom: monitoring
ms.openlocfilehash: c6f4b932bb8f16af3c0368ab219bd8a12bf2face
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2020
ms.locfileid: "94357986"
---
# <a name="azure-files-monitoring-data-reference"></a>Справочник по данным мониторинга файлов Azure

Подробные сведения о сборе и анализе данных мониторинга для службы файлов Azure см. в статье [мониторинг файлов Azure](storage-files-monitoring.md) .

## <a name="metrics"></a>Метрики

В следующих таблицах перечислены метрики платформы, собранные для службы файлов Azure. 

### <a name="capacity-metrics"></a>Метрики емкости

Значения метрик емкости обновляются ежедневно (до 24 часов). Интервал времени определяет интервал, за который представлены значения метрик. Поддерживаемый интервал времени для всех метрик емкости — один час (PT1H).

Службы файлов Azure предоставляют следующие метрики емкости в Azure Monitor.

#### <a name="account-level"></a>На уровне учетной записи

[!INCLUDE [Account level capacity metrics](../../../includes/azure-storage-account-capacity-metrics.md)]

#### <a name="azure-files"></a>Файлы Azure

В этой таблице показаны [метрики службы файлов Azure](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccountsfileservices).

| Метрика | Описание |
| ------------------- | ----------------- |
| FileCapacity | Объем хранилища файлов, используемый учетной записью хранения. <br/><br/> Единица измерения: Байты <br/> Тип агрегирования: Среднее <br/> Пример значения. 1024 |
| FileCount   | Количество файлов в учетной записи хранения. <br/><br/> Единица измерения: Count <br/> Тип агрегирования: Среднее <br/> Пример значения. 1024 |
| филешарекапаЦитикуота | Верхний предел объема хранилища, который может использоваться службой файлов Azure в байтах. <br/><br/> Единица измерения: Байты <br/> Тип агрегирования: Среднее <br/> Пример значения. 1024|
| FileShareCount | Количество общих папок в учетной записи хранения. <br/><br/> Единица измерения: Count <br/> Тип агрегирования: Среднее <br/> Пример значения. 1024 |
| филешарепровисионедиопс | Число подготовленных операций ввода-вывода в общей папке. Эта метрика применима только к хранилищу файлов уровня "Премиум". <br/><br/> Единица: байт <br/> Тип агрегирования: Среднее |
| FileShareSnapshotCount | Количество моментальных снимков, имеющихся в общей папке в службе файлов Azure в учетной записи хранения. <br/><br/> Единица измерения: количество <br/> Тип агрегирования: Среднее | 
|FileShareSnapshotSize|Объем хранилища, используемый моментальными снимками в службе файлов Azure в учетной записи хранения. <br/><br/> Единица измерения: Байты <br/> Тип агрегирования: Среднее|

### <a name="transaction-metrics"></a>Метрики транзакций

Метрики транзакций создаются при каждом запросе к учетной записи хранения, отправляемом из службы хранилища Azure в Azure Monitor. Если учетная запись хранения неактивна, данные на основе метрик транзакций за этот период будут отсутствовать. Все метрики транзакций доступны на уровне учетной записи и службы файлов Azure. Интервал времени определяет интервал, за который представлены значения метрик. Поддерживаемые интервалы времени для всех метрик транзакций — PT1H и PT1M.

[!INCLUDE [Transaction metrics](../../../includes/azure-storage-account-transaction-metrics.md)]

<a id="metrics-dimensions"></a>

## <a name="metrics-dimensions"></a>Измерения метрик

Службы файлов Azure поддерживают следующие измерения для метрик в Azure Monitor.

> [!NOTE] 
> Измерение файлового ресурса недоступно для стандартных файловых ресурсов (только общие файловые ресурсы уровня "Премиум"). При использовании стандартных файловых ресурсов метрики предоставляются для всех общих файлов в учетной записи хранения. Чтобы получить метрики общего ресурса для стандартных файловых ресурсов, создайте одну общую папку для каждой учетной записи хранения.

[!INCLUDE [Metrics dimensions](../../../includes/azure-storage-account-metrics-dimensions.md)]

## <a name="resource-logs-preview"></a>Журналы ресурсов (предварительная версия)

> [!NOTE]
> Журналы службы хранилища Azure в Azure Monitor предоставляются в общедоступной предварительной версии. Они также доступны для предварительного тестирования во всех регионах общедоступного облака. Чтобы зарегистрироваться для использования предварительной версии, см. [эту страницу](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u).  Эта предварительная версия включает журналы для больших двоичных объектов (в том числе Azure Data Lake Storage 2-го поколения), файлов, очередей, таблиц, учетных записей хранения категории "Премиум" общего назначения версии 1 и учетных записей хранения общего назначения версии 2. Классические учетные записи хранения не поддерживаются.

В следующей таблице перечислены свойства журналов ресурсов службы хранилища Azure при их сборе в журналах Azure Monitor или службе хранилища Azure. Свойства содержат сведения об операции, службе и типе авторизации, которые использовались для выполнения операции.

### <a name="fields-that-describe-the-operation"></a>Поля со сведениями об операции


[!INCLUDE [Account level capacity metrics](../../../includes/azure-storage-logs-properties-operation.md)]

### <a name="fields-that-describe-how-the-operation-was-authenticated"></a>Поля, описывающие аутентификацию операции

[!INCLUDE [Account level capacity metrics](../../../includes/azure-storage-logs-properties-authentication.md)]

### <a name="fields-that-describe-the-service"></a>Поля со сведениями о службе

[!INCLUDE [Account level capacity metrics](../../../includes/azure-storage-logs-properties-service.md)]

## <a name="see-also"></a>См. также

- Описание мониторинга службы хранилища Azure см. в статье [мониторинг файлов Azure](storage-files-monitoring-reference.md) .
- Подробные сведения о мониторинге ресурсов Azure см. в статье [Мониторинг ресурсов Azure с помощью Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md).
