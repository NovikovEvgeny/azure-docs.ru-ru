---
title: 'Основные понятия: поток данных в соединителе Azure IoT для FHIR (Предварительная версия) функции Azure API для FHIR'
description: Сведения о соединителе Azure IoT для потока данных FHIR (Предварительная версия). Соединитель Azure IoT для FHIR (Предварительная версия) принимает, нормализует, группирует, преобразует и сохраняет данные Иомт в API Azure для FHIR.
services: healthcare-apis
author: ms-puneet-nagpal
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: conceptual
ms.date: 07/31/2020
ms.author: punagpal
ms.openlocfilehash: 3cae648e3c2bddbafec555621d97575a007cfeb4
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93394872"
---
# <a name="azure-iot-connector-for-fhir-preview-data-flow"></a>Поток данных в соединителе "Azure IoT для FHIR" (предварительная версия)

В этой статье представлен обзор потока данных в соединителе Azure IoT для FHIR *. Вы узнаете о различных стадиях обработки данных в соединителе Azure IoT для FHIR, которые преобразуют данные устройства в ресурсы [наблюдения](https://www.hl7.org/fhir/observation.html) на основе FHIR.

![Поток данных для соединителя "Azure IoT для FHIR"](media/concepts-iot-data-flow/iot-connector-data-flow.png)

На схеме выше показаны общие потоки данных с помощью соединителя Azure IoT для FHIR. 

Ниже приведены различные этапы, через которые данные проходят через соединитель Azure IoT для FHIR.

## <a name="ingest"></a>Прием
Прием — это первый этап, в который данные устройства поступают в соединитель Azure IoT для FHIR. Конечная точка приема данных устройства размещается в [концентраторе событий Azure](../event-hubs/index.yml). Платформа концентратора событий Azure поддерживает высокую масштабируемость и пропускную способность с возможностью получения и обработки миллионов сообщений в секунду. Он также позволяет соединителю Azure IoT для FHIR потреблять сообщения асинхронно, устраняя необходимость в ожидании устройств во время обработки данных устройства.

> [!NOTE]
> В настоящее время формат JSON поддерживается только для данных устройства.

## <a name="normalize"></a>Нормализовать
Нормализация — это следующий этап, в котором данные устройства извлекаются из указанного выше концентратора событий Azure и обрабатываются с помощью шаблонов сопоставления устройств. Этот процесс сопоставления приводит к преобразованию данных устройства в нормализованную схему. 

Процесс нормализации упрощает обработку данных на более поздних стадиях, но также предоставляет возможность проецирования одного входного сообщения в несколько нормализованных сообщений. Например, устройство может отправить несколько важнейших знаков для основной температуры, скорости импульса, кровяного давления и нормы дыхания в одном сообщении. Это входное сообщение создаст четыре отдельных ресурса FHIR. Каждый ресурс будет представлять собой свой важный знак, а входное сообщение, запланированное на четыре разных нормализованных сообщения.

## <a name="group"></a>Группа
Группа — это следующий этап, в котором нормализованные сообщения, доступные на предыдущем этапе, группируются с использованием трех различных параметров: удостоверение устройства, тип измерения и период времени.

При группировании удостоверений устройств и типов измерений включить использование типа измерения [сампледдата](https://www.hl7.org/fhir/datatypes.html#SampledData) . Этот тип обеспечивает краткий способ представления ряда измерений на основе времени с устройства в FHIR. И период времени определяет задержку, при которой ресурсы наблюдения, создаваемые соединителем Azure IoT для FHIR, записываются в API Azure для FHIR.

> [!NOTE]
> Значение периода времени по умолчанию равно 15 минутам и не может быть настроено для просмотра.

## <a name="transform"></a>Преобразование
На этапе преобразования сгруппированные нормализованные сообщения обрабатываются с помощью шаблонов сопоставления FHIR. Сообщения, соответствующие типу шаблона, преобразуются в ресурсы наблюдения на основе FHIR, как указано в сопоставлении.

На этом этапе ресурс [устройства](https://www.hl7.org/fhir/device.html) и связанный с ним ресурс [пациента](https://www.hl7.org/fhir/patient.html) также получаются с сервера FHIR с использованием идентификатора устройства, присутствующего в сообщении. Эти ресурсы добавляются в качестве ссылки на создаваемый ресурс наблюдения.

> [!NOTE]
> Все поисковые идентификаторы кэшируются после разрешения, чтобы снизить нагрузку на сервер FHIR. Если вы планируете повторно использовать устройства с несколькими пациентов, рекомендуется создать ресурс виртуального устройства, относящийся к пациентам, и отправить идентификатор виртуального устройства в полезные данные сообщения. Виртуальное устройство может быть связано с фактическим ресурсом устройства в качестве родительского.

Если на сервере FHIR не существует ресурса устройства для данного идентификатора устройства, результат зависит от значения `Resolution Type` Set во время создания. Если задано значение `Lookup` , то конкретное сообщение игнорируется и конвейер продолжает обрабатывать другие входящие сообщения. Если задано значение `Create` , соединитель Azure IOT для FHIR создаст устройство с исходными костями и ресурсы пациента на сервере FHIR.  

## <a name="persist"></a>Сохранить
После создания ресурса наблюдения FHIR на этапе преобразования ресурс сохраняется в API Azure для FHIR. Если ресурс FHIR является новым, он будет создан на сервере. Если ресурс FHIR уже существовал, он будет обновлен.

## <a name="next-steps"></a>Дальнейшие шаги

Щелкните ниже следующий шаг, чтобы узнать, как создать шаблоны сопоставления устройств и FHIR.

>[!div class="nextstepaction"]
>[Шаблоны сопоставления для соединителя "Azure IoT для FHIR"](iot-mapping-templates.md)

*На портале Azure соединитель "Azure IoT для FHIR" называется соединителем IoT (предварительная версия).

FHIR — это зарегистрированная торговая марка организации HL7, которая используется с разрешения HL7.