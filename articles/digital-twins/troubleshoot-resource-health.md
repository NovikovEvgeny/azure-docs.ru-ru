---
title: Общие сведения о работоспособности ресурсов
titleSuffix: Azure Digital Twins
description: Узнайте, как использовать Работоспособность ресурсов Azure для проверки работоспособности своего экземпляра Azure Digital двойников.
author: baanders
ms.author: baanders
ms.date: 10/6/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: 9c31345a4ddaf9ac2b75204172dbc47606cb07db
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "92681728"
---
# <a name="troubleshooting-azure-digital-twins-resource-health"></a>Устранение неполадок в Azure Digital двойников: работоспособность ресурсов

[Работоспособность ресурсов Azure](../service-health/resource-health-overview.md) помогает диагностировать и получать поддержку проблем служб, влияющих на ресурсы Azure. Он сообщает о текущем и прошлом состоянии ресурсов.

В этой статье показано, как получить сведения о **работоспособности ресурсов** для экземпляров Azure Digital двойников.

## <a name="use-azure-resource-health"></a>Использование службы "Работоспособность ресурса Azure"

Работоспособность ресурсов Azure помогает отслеживать, работает ли экземпляр Azure Digital двойников. Его также можно использовать, чтобы узнать, влияет ли региональный сбой на работоспособность вашего экземпляра.

Чтобы проверить работоспособность экземпляра, выполните следующие действия.

1. Войдите в [портал Azure](https://portal.azure.com) и перейдите к своему экземпляру Azure Digital двойников. Его можно найти, введя его имя на панели поиска портала. 

2. В меню своего экземпляра выберите _**работоспособность ресурсов**_ в разделе *Поддержка и устранение неполадок*. Откроется страница просмотра журнала работоспособности ресурсов. 

    :::image type="content" source="media/troubleshoot-resource-health/resource-health.png" alt-text="Снимок экрана, показывающий страницу &quot;работоспособность ресурсов&quot;. В разделе &quot;журнал работоспособности&quot; показан ежедневный отчет за последние девять дней. Каждый день отображает состояние &quot;доступно&quot;.":::

На приведенном выше рисунке этот экземпляр показан как *доступный* и был за последние девять дней. Дополнительные сведения о состоянии *доступности* и других отображаемых типах состояния см. в статье [*Общие сведения о работоспособности ресурсов Azure*](../service-health/resource-health-overview.md).

Кроме того, вы можете узнать больше о различных проверках, которые переходят в работоспособность ресурсов для различных типов ресурсов Azure, в [*типах ресурсов и проверках работоспособности в службе работоспособности ресурсов Azure*](../service-health/resource-health-checks-resource-types.md).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о других способах мониторинга экземпляра Azure Digital двойников см. в следующих статьях:
* [*Устранение неполадок: Просмотр метрик с помощью Azure Monitor*](troubleshoot-metrics.md)
* [*Устранение неполадок: Настройка диагностики*](troubleshoot-diagnostics.md).
* [*Устранение неполадок: Настройка оповещений*](troubleshoot-alerts.md)
