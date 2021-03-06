---
title: Метрики и журналы диагностики служб мультимедиа с Azure Monitor
titleSuffix: Azure Media Services
description: Узнайте, как отслеживать метрики и журналы диагностики служб мультимедиа Azure с помощью Azure Monitor.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/02/2020
ms.author: inhenkel
ms.openlocfilehash: 33aed32c30f298fd3432f4cebcc28b9c20974545
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2020
ms.locfileid: "93309060"
---
# <a name="monitor-media-services-metrics-and-diagnostic-logs-with-azure-monitor"></a>Мониторинг метрик и журналов диагностики служб мультимедиа с помощью Azure Monitor

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

[Azure Monitor](../../azure-monitor/overview.md) позволяет отслеживать метрики и журналы диагностики, которые помогут понять, как работают приложения. Все данные, собираемые службой Azure Monitor, соответствуют одному из двух основных типов, то есть представляют собой метрики или журналы. Вы можете отслеживать журналы диагностики служб мультимедиа и создавать оповещения и уведомления для собранных метрик и журналов. Данные метрик можно визуализировать и анализировать с помощью [обозревателя метрик](../../azure-monitor/platform/metrics-getting-started.md). Вы можете отправить журналы в службу [хранилища Azure](https://azure.microsoft.com/services/storage/), выполнить их потоковую передачу в [концентраторы событий Azure](https://azure.microsoft.com/services/event-hubs/), экспортировать их в [log Analytics](https://azure.microsoft.com/services/log-analytics/)или использовать сторонние службы.

Подробные сведения см. в разделе [метрики Azure Monitor](../../azure-monitor/platform/data-platform.md) и [Azure Monitor журналов диагностики](../../azure-monitor/platform/platform-logs-overview.md).

В этом разделе обсуждаются поддерживаемые [метрики служб мультимедиа](#media-services-metrics) и [журналы диагностики служб мультимедиа](#media-services-diagnostic-logs).

## <a name="media-services-metrics"></a>Метрики служб мультимедиа

Метрики собираются через регулярные промежутки времени независимо от того, изменяется ли значение. Метрики удобно использовать для создания оповещений, так можно часто делать их выборку и быстро создавать оповещения с использованием относительно простой логики. Сведения о создании оповещений метрик см. в статье [Создание, просмотр и Управление оповещениями метрик с помощью Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

Службы мультимедиа поддерживают метрики мониторинга для следующих ресурсов:

* Учетная запись
* Конечная точка потоковой передачи

### <a name="account"></a>Учетная запись

Вы можете отслеживать следующие метрики учетной записи.

|Имя метрики|Отображаемое имя|Описание|
|---|---|---|
|AssetCount|Число ресурсов|Активы в вашей учетной записи.|
|AssetQuota|Квота ресурсов|Квота ресурса в вашей учетной записи.|
|AssetQuotaUsedPercentage|Процент используемой квоты ресурсов|Процент использования квоты актива.|
|ContentKeyPolicyCount|Число политик ключей содержимого|Политики ключей содержимого в вашей учетной записи.|
|ContentKeyPolicyQuota|Квота политик ключей содержимого|Политики ключей содержимого. квота для учетной записи.|
|ContentKeyPolicyQuotaUsedPercentage|Процент использования квоты политик ключей содержимого|Процентная доля квоты политики ключей содержимого, которая уже использовалась.|
|StreamingPolicyCount|Число политик потоковой передачи|Политики потоковой передачи в учетной записи.|
|StreamingPolicyQuota|Квота политик потоковой передачи|Квота потоковой передачи политик в вашей учетной записи.|
|StreamingPolicyQuotaUsedPercentage|Процент использования квоты политик потоковой передачи|Процент уже использованной квоты политики потоковой передачи.|

Также следует проверить [квоты и ограничения учетной записи](limits-quotas-constraints.md).

### <a name="streaming-endpoint"></a>Конечная точка потоковой передачи

Поддерживаются следующие метрики [конечных точек потоковой передачи](/rest/api/media/streamingendpoints) служб мультимедиа:

|Имя метрики|Отображаемое имя|Описание|
|---|---|---|
|Requests|Requests|Предоставляет общее число HTTP-запросов, обслуживаемых конечной точкой потоковой передачи.|
|Исходящие|Исходящие|Всего байтов исходящего трафика в минуту на конечную точку потоковой передачи.|
|SuccessE2ELatency|Сквозная задержка успешного запроса|Период времени, в течение которого конечная точка потоковой передачи получает запрос на момент отправки последнего байта ответа.|
|Использование ЦП| Загрузка ЦП для конечных точек потоковой передачи уровня "Премиум". Эти данные недоступны для стандартных конечных точек потоковой передачи. |
|Пропускная способность для исходящих данных | Пропускная способность исходящего трафика в битах в секунду.|

### <a name="metrics-are-useful"></a>Метрики полезны

Ниже приведены примеры того, как мониторинг метрик служб мультимедиа поможет понять, как работают ваши приложения. Ниже приведены некоторые вопросы, которые можно решить с помощью метрик служб мультимедиа.

* Разделы справки отслеживать мои стандартные конечные точки потоковой передачи, чтобы узнать, когда превышены ограничения?
* Разделы справки узнать, достаточно ли единиц масштабирования для конечных точек потоковой передачи уровня "Премиум"?
* Как настроить оповещение о том, когда следует масштабировать конечные точки потоковой передачи?
* Разделы справки настроить оповещение о достижении максимального исходящего исходящего трафика для учетной записи?
* Как увидеть разбивку запросов на ошибки и причину сбоя?
* Как узнать, сколько запросов HLS или ТИРЕ извлекается из упаковщика?
* Разделы справки задать оповещение о том, что было достигнуто пороговое значение числа неудачных запросов?

Параллелизм приобретает значение для количества конечных точек потоковой передачи, используемых в одной учетной записи с течением времени. Необходимо помнить о взаимосвязи между числом параллельных потоков со сложными параметрами публикации, такими как динамическая упаковка к нескольким протоколам, множественное шифрование DRM и т. д. Каждый дополнительный опубликованный динамический поток добавляет к ЦП и пропускной способности выходных данных в конечной точке потоковой передачи. С учетом этого следует использовать Azure Monitor, чтобы точно отслеживать использование конечной точки потоковой передачи (мощности ЦП и исходящего трафика), чтобы убедиться в том, что масштабирование выполняется правильно (или разделите трафик между несколькими конечными точками потоковой передачи, если вы получаете очень высокую степень параллелизма).

### <a name="example"></a>Пример

См. раздел [мониторинг метрик служб мультимедиа](media-services-metrics-howto.md).

## <a name="media-services-diagnostic-logs"></a>Журналы диагностики служб мультимедиа

Журналы диагностики предоставляют обширные и часто встречающиеся данные о работе ресурсов Azure. Дополнительные сведения см. в статье получение [и использование данных журнала из ресурсов Azure](../../azure-monitor/platform/platform-logs-overview.md).

Службы мультимедиа поддерживают следующие журналы диагностики:

* Доставка ключей

### <a name="key-delivery"></a>Доставка ключей

|Имя|Описание|
|---|---|
|Запрос службы доставки ключей|Журналы, отображающие сведения о запросе службы доставки ключей. Дополнительные сведения см. в разделе [схемы](media-services-diagnostic-logs-schema.md).|

### <a name="why-would-i-want-to-use-diagnostics-logs"></a>Зачем нужно использовать журналы диагностики?

Ниже перечислены некоторые вещи, которые можно проверить с помощью журналов диагностики доставки ключей.

* См. число лицензий, доставленных по типу DRM.
* См. число лицензий, доставленных политикой.
* См. ошибки по DRM или типу политики.
* Просмотр числа неавторизованных запросов лицензий от клиентов.

### <a name="example"></a>Пример

См. раздел [мониторинг журналов диагностики службы мультимедиа](media-services-diagnostic-logs-howto.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Как получить и использовать данные журнала из ресурсов Azure](../../azure-monitor/platform/platform-logs-overview.md)
* [Создание и просмотр оповещений метрик, а также управление ими с помощью Azure Monitor](../../azure-monitor/platform/alerts-metric.md)
* [Мониторинг метрик служб мультимедиа](media-services-metrics-howto.md)
* [Мониторинг журналов диагностики службы мультимедиа](media-services-diagnostic-logs-howto.md)
