---
title: 'Сценарий: настраиваемая маршрутизация для виртуальной глобальной сети в брандмауэре Azure'
titleSuffix: Azure Virtual WAN
description: Сценарии маршрутизации трафика между виртуальных сетей напрямую, но с помощью брандмауэра Azure для виртуальной сети >Интернет/ветвь и ветвь к потокам трафика виртуальной сети.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: 301bc64bee291fa25506e7f435e923be7e244cd4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91267522"
---
# <a name="scenario-azure-firewall---custom"></a>Сценарий: Пользовательский брандмауэр Azure — настраиваемый

При работе с маршрутизацией виртуальных сетей виртуальной глобальной сети существует несколько доступных сценариев. В этом сценарии цель заключается в том, чтобы направить трафик между виртуальных сетей напрямую, но используйте брандмауэр Azure для потоков трафика между виртуальными сетями и ветвями и виртуальными сетями.

## <a name="design"></a><a name="design"></a>Конструирование

Чтобы определить, сколько таблиц маршрутов потребуется, можно создать матрицу подключения, где каждая ячейка показывает, может ли источник (строка) обмениваться данными с целевым объектом (столбцом). Матрица подключения в этом сценарии является тривиальной, но ее можно использовать в других сценариях.

**Матрица подключения**

| Как получить           | на:      | *Виртуальных сетей*      | *Ветви*    | *Интернет*;   |
|---             |---       |---           |---            |---           |
| **Виртуальных сетей**      |   &#8594;|     X        |     азфв      |     азфв     |
| **Ветви**   |   &#8594;|    азфв      |       X       |       X      |

В предыдущей таблице "X" представляет прямое подключение между двумя подключениями без прохождения трафика в брандмауэре Azure в виртуальной глобальной сети, а "Азфв" указывает, что поток будет проходить через брандмауэр Azure. Так как в матрице есть два различных шаблона подключения, для них потребуется две таблицы маршрутов, которые будут настроены следующим образом:

* Виртуальные сети:
  * Связанная таблица маршрутов: **RT_VNet**
  * Распространение на таблицы маршрутов: **RT_VNet**
* Ветк
  * Связанная таблица маршрутов: **по умолчанию**
  * Распространение на таблицы маршрутов: **по умолчанию**

> [!NOTE]
> Вы можете создать отдельный виртуальный экземпляр глобальной сети с одним безопасным виртуальным концентратором в каждом регионе, а затем подключить каждую виртуальную сеть к друг другу через VPN типа "сеть — сеть".

Дополнительные сведения о маршрутизации виртуальных концентраторов см. в статье [о маршрутизации виртуальных концентраторов](about-virtual-hub-routing.md).

## <a name="workflow"></a><a name="workflow"></a>Рабочий процесс

В этом сценарии вы хотите направить трафик через брандмауэр Azure для трафика между виртуальными сетями, виртуальными сетями или ветвями и виртуальными сетями, но хотите перейти прямо к трафику между виртуальными сетями. Если вы использовали диспетчер брандмауэра Azure, параметры маршрута автоматически заполняются в **таблице маршрутов по умолчанию**. Частный трафик применяется к виртуальным сетям и ветвям, а Интернет-трафик применяется к 0.0.0.0/0.

VPN, ExpressRoute и VPN-подключения пользователей совместно называются ветвями и связываются с одной и той же таблицей маршрутов (по умолчанию). Все VPN-подключения ExpressRoute и User VPN распространяют маршруты на один и тот же набор таблиц маршрутов. Чтобы настроить этот сценарий, примите во внимание следующие шаги:

1. Создайте настраиваемую таблицу маршрутов **RT_VNet**.
1. Создайте маршрут, чтобы активировать VNet-to-Internet и VNet-to-Branch: 0.0.0.0/0 со следующим прыжком, указывающим на брандмауэр Azure. В разделе распространение вы убедитесь, что выбраны виртуальных сетей, что обеспечит более конкретные маршруты, тем самым обеспечивая прямой поток трафика между виртуальными сетями.

   * В поле **Ассоциация:** выберите виртуальных сетей, чтобы указать, что виртуальных сетей будет обращаться к целевому объекту в соответствии с маршрутами этой таблицы маршрутов.
   * В поле **распространение** выберите виртуальных сетей, которое предполагает, что виртуальных сетей распространится на эту таблицу маршрутов. Иными словами, более конкретные маршруты будут распространяться на эту таблицу маршрутов, таким образом обеспечивая прямой поток трафика между виртуальной сетью и виртуальной сетью.

1. Добавьте агрегированный статический маршрут для виртуальных сетей в **таблицу маршрутов по умолчанию** , чтобы активировать поток ветвей между виртуальными сетями через брандмауэр Azure.

   * Помните, что ветви связаны и распространяются в таблицу маршрутов по умолчанию.
   * Ветви не распространяются на RT_VNet таблицу маршрутов. Это обеспечит передачу трафика из виртуальной сети в ветвь через брандмауэр Azure.

Это приведет к изменению конфигурации маршрутизации, как показано на **рис. 1**.

**Рис. 1**

:::image type="content" source="./media/routing-scenarios/between-vnets-firewall/routing.png" alt-text="Рис. 1":::

> [!NOTE]
> Виртуальные глобальные концентраторы глобальной сети и подключенные виртуальные сети должны находиться в одном регионе Azure.

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о виртуальной глобальной сети см. в статье, содержащей [Часто задаваемые вопросы](virtual-wan-faq.md) о ней.
* Дополнительные сведения о маршрутизации виртуальных концентраторов см. в статье [о маршрутизации виртуальных концентраторов](about-virtual-hub-routing.md).
* Дополнительные сведения о настройке маршрутизации виртуальных концентраторов см. в статье [Настройка маршрутизации виртуальных концентраторов](how-to-virtual-hub-routing.md).
