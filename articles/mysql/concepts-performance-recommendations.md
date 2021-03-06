---
title: 'База данных Azure для MySQL: рекомендации по повышению производительности'
description: В этой статье описывает функция предоставления рекомендаций по повышению производительности в Базе данных Azure для MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 6/3/2020
ms.openlocfilehash: 6f41863f45bdc90cb9fe589ba0a5011dea84a67c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "84485242"
---
# <a name="performance-recommendations-in-azure-database-for-mysql"></a>Рекомендации по повышению производительности в Базе данных Azure для MySQL

**Применимо к:** База данных Azure для MySQL 5.7, 8.0.

Функция рекомендаций по повышению производительности анализирует базы данных, чтобы создавать настраиваемые предложения по повышению производительности. Для получения рекомендаций функция анализа рассматривает различные характеристики базы данных, включая схему. Включите [хранилище запросов](concepts-query-store.md) на сервере, чтобы полностью использовать возможности рекомендаций по повышению производительности. Если схема производительности отключена, то включение хранилища запросов включит схему performance_schema и набор инструментов схемы производительности, необходимых для работы этой функции. После реализации любой из рекомендаций по повышению производительности следует протестировать производительность, чтобы оценить результаты внесенных изменений.

## <a name="permissions"></a>Разрешения

Для выполнения анализа с помощью функции рекомендаций по повышению производительности требуются разрешения **владельца** или **участника**.

## <a name="performance-recommendations"></a>Рекомендации по производительности

Функция [рекомендаций по повышению производительности](concepts-performance-recommendations.md) анализирует рабочие нагрузки на сервере и определяет, какие индексы могут помочь в повышении производительности.

Откройте **рекомендации по повышению производительности** из раздела **Интеллектуальная производительность** в строке меню на странице портала Azure для сервера MySQL.

:::image type="content" source="./media/concepts-performance-recommendations/performance-recommendations-page.png" alt-text="Целевая страница рекомендаций по повышению производительности":::

Щелкните **Анализировать** и выберите базу данных, с которой начнется анализ. В зависимости от рабочей нагрузки анализ может занять несколько минут. По окончании анализа на портале появится уведомление. При анализе выполняется глубокое изучение базы данных. Рекомендуется выполнять анализ в периоды наименьшей нагрузки.

В окне **Рекомендации** отобразится список рекомендаций, если они были найдены, и связанный идентификатор запроса, создавшего эту рекомендацию. С помощью идентификатора запроса можно получить дополнительные сведения о нем в представлении [mysql.query_store](concepts-query-store.md#mysqlquery_store).

:::image type="content" source="./media/concepts-performance-recommendations/performance-recommendations-result.png" alt-text="Целевая страница рекомендаций по повышению производительности":::

Найденные рекомендации не применяются автоматически. Чтобы применить рекомендацию, скопируйте текст запроса и выполните его в клиенте по своему выбору. Не забудьте протестировать и отследить результаты применения рекомендации, чтобы оценить ее.

## <a name="recommendation-types"></a>Типы рекомендаций

### <a name="index-recommendations"></a>Рекомендации по индексам

*Рекомендации по созданию индексов* предлагают новые индексы для ускорения наиболее часто выполняемых и длительных запросов в рабочей нагрузке. Для этого типа рекомендации требуется включить [хранилище запросов](concepts-query-store.md). Хранилище запросов собирает сведения о запросах и предоставляет подробную статистику по времени и частоте выполнения запросов, которую функция анализа использует для создания рекомендаций.

### <a name="query-recommendations"></a>Рекомендации по запросам

Рекомендации по запросам предлагают оптимизации и перезапись для запросов в рабочей нагрузке. Определяя антишаблоны запросов MySQL и устраняя их синтаксические действия, можно улучшить производительность запросов, требующих много времени. Для этого типа рекомендации требуется включить хранилище запросов. Хранилище запросов собирает сведения о запросах и предоставляет подробную статистику по времени и частоте выполнения запросов, которую функция анализа использует для создания рекомендаций.

## <a name="next-steps"></a>Дальнейшие действия
- Узнайте больше о [мониторинге и настройке](concepts-monitoring.md) в Базе данных Azure для MySQL.