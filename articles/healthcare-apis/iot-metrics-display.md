---
title: Отображение и Настройка соединителя Azure IoT для метрик FHIR (Предварительная версия)
description: В этой статье объясняется, как отобразить и настроить соединитель Azure IoT для метрик FHIR (Предварительная версия).
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 10/29/2020
ms.author: jasteppe
ms.openlocfilehash: 9a4e2c4dfe8a9de28688afe0dd036cecb7ce2b39
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/09/2020
ms.locfileid: "94381224"
---
# <a name="display-and-configure-azure-iot-connector-for-fhir-preview-metrics"></a>Отображение и Настройка соединителя Azure IoT для метрик FHIR (Предварительная версия) 

В этой статье вы узнаете, как отобразить и настроить соединитель Azure IoT для быстрого получения ресурсов о взаимодействии в сфере здравоохранения (FHIR&#174;) *.

> [!TIP]
> Чтобы узнать, как настроить экспорт данных метрик, следуйте указаниям в статье [Экспорт соединителя Azure IOT для FHIR (Предварительная версия) для метрик с помощью параметров диагностики](./iot-metrics-diagnostics-export.md).

## <a name="display-metrics-for-azure-iot-connector-for-fhir-preview"></a>Отображение метрик для соединителя Azure IoT для FHIR (Предварительная версия)

1. Войдите в портал Azure, а затем выберите API Azure для службы FHIR. 

2. В области слева выберите **метрики**. 

3. Перейдите на вкладку **соединитель IOT** .

   :::image type="content" source="media/iot-metrics-display/iot-metrics-main.png" alt-text="Снимок экрана: панель &quot;соединитель IoT&quot; с графикой, отображающей количество входящих и нормализованных сообщений." lightbox="media/iot-metrics-display/iot-metrics-main.png"::: 

4. Выберите соединитель IoT для просмотра его метрик. Например, существует четыре соединителя Интернета вещей ( *соединитель 1* , *соединитель 2* и т. д.), связанные с этим API Azure для службы FHIR.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-select-connector.png" alt-text="Снимок экрана: панель &quot;соединитель IoT&quot;, на которой показаны вкладки соединителя IoT 1, 2, 3 и 4." lightbox="media/iot-metrics-display/iot-metrics-select-connector.png"::: 

5. Выберите период времени (например, **1 час** , **24 часа** , **7 дней** или **Настраиваемый** ) метрик соединителя Интернета вещей, которые требуется отобразить. Выбрав **настраиваемую** вкладку, можно создать определенные сочетания времени и дат для отображения метрик соединителя IOT.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-select-time.png" alt-text="Снимок экрана: панель &quot;соединитель IoT&quot;, отображающая график периода времени &quot;1 час&quot; для &quot;соединителя 1&quot;." lightbox="media/iot-metrics-display/iot-metrics-select-time.png"::: 
 
## <a name="metric-types-for-azure-iot-connector-for-fhir-preview"></a>Типы метрик для соединителя Azure IoT для FHIR (Предварительная версия) 

Метрики соединителя IoT, которые можно отобразить, перечислены в следующей таблице.

|Тип метрики|Назначение метрики| 
|-----------|--------------|
|Число входящих сообщений|Отображает число полученных необработанных входящих сообщений (например, событий устройства).|
|Число нормализованных сообщений|Отображает число нормализованных сообщений.|
|Число групп сообщений|Отображает число групп, для которых в заданном временном окне собраны сообщения.|
|Средняя задержка нормализованного этапа|Отображает среднюю задержку нормализованного этапа. Нормализованная стадия выполняет нормализацию необработанных входящих сообщений.|
|Средняя задержка этапа группы|Отображает среднюю задержку этапа группы. На этапе группы выполняется буферизация, агрегирование и группирование нормализованных сообщений.| 
|общее число ошибок|Отображает общее число ошибок.| 

## <a name="focus-on-and-configure-azure-iot-connector-for-fhir-preview-metrics"></a>Сосредоточьтесь на и настройте соединитель Azure IoT для метрик FHIR (Предварительная версия)

В этом примере основное внимание уделяется **числу метрик входящих сообщений** .

1. Выберите момент времени, на который вы хотите сосредоточиться.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-focus.png" alt-text="Снимок экрана области метрики &quot;число входящих сообщений&quot;, выделяющий один момент времени на линейном графике." lightbox="media/iot-metrics-display/iot-metrics-focus.png"::: 

2. В области **число входящих сообщений** можно дополнительно настроить метрику, выбрав **Добавить метрику** , **Добавить фильтр** или **Применить разделение**. 

   :::image type="content" source="media/iot-metrics-display/iot-metrics-add-options.png" alt-text="Снимок экрана: область метрики &quot;число входящих сообщений&quot;, в которой выделяются кнопки &quot;добавить метрику&quot;, &quot;добавить фильтр&quot; и &quot;применить разделение&quot;." lightbox="media/iot-metrics-display/iot-metrics-add-options.png"::: 

## <a name="conclusion"></a>Заключение 
Наличие доступа к метрикам плоскости данных очень важно для мониторинга и устранения неполадок. Соединитель Azure IoT для FHIR помогает выполнять эти действия с помощью метрик. 

## <a name="next-steps"></a>Дальнейшие действия

Получите ответы на часто задаваемые вопросы о соединителе Azure IoT для FHIR.

>[!div class="nextstepaction"]
>[Вопросы и ответы о соединителе Azure IoT для FHIR](fhir-faq.md)

* В портал Azure соединитель Azure IoT для FHIR называется соединителем IoT (Предварительная версия). FHIR является охраняемым товарным знаком HL7 и используется с разрешением HL7. 
