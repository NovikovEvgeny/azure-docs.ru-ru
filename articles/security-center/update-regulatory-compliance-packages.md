---
title: Использование панели мониторинга соответствия нормативным требованиям в центре безопасности Azure
description: Узнайте, как добавлять и удалять нормативные стандарты на панели мониторинга соответствия нормативным требованиям в центре безопасности.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/15/2020
ms.author: memildin
ms.openlocfilehash: e7e1567a487dc6cadc94a42f02c597ff0e02665b
ms.sourcegitcommit: 65d518d1ccdbb7b7e1b1de1c387c382edf037850
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/09/2020
ms.locfileid: "94372767"
---
# <a name="customizing-the-set-of-standards-in-your-regulatory-compliance-dashboard"></a>Настройка набора стандартов на панели мониторинга соответствия нормативным требованиям

Центр безопасности Azure постоянно сравнивает конфигурацию всех ресурсов на предмет соответствия требованиям отраслевых стандартов, тестам производительности и нормативным требованиям. **Панель мониторинга соответствия нормативным требованиям** предоставляет аналитические сведения о состоянии соответствия на основе того, как вы используете определенные средства и соблюдаете определенные требования.


## <a name="overview-of-compliance-packages"></a>Обзор пакетов соответствия

Отраслевые стандарты, нормативные стандарты и тесты производительности представлены в Центре безопасности как *пакеты соответствия*.  Каждый такой пакет определен как инициатива в Политике Azure. Чтобы просмотреть данные о соответствии, сопоставленные с оценками на панели мониторинга, добавьте пакет соответствия в группу управления или подписку на странице **Политика безопасности**. (Дополнительные сведения о Политике Azure и инициативах см. в статье [Использование политик безопасности](tutorial-security-policy.md).)

Когда вы подключаете стандарт или тест производительности к выбранной области, этой области назначается новая инициатива. После этого стандарт появляется на панели мониторинга соответствия нормативным требованиям, и все связанные с ним данные о соответствии сопоставляются как оценки. По любому из подключенных стандартов доступен также сводный отчет для скачивания.

Корпорация Майкрософт также отслеживает сами нормативные стандарты и время от времени автоматически улучшает покрытие стандартов в некоторых пакетах. Когда корпорация Майкрософт выпускает новое содержимое для инициативы (новые политики, которые сопоставляются с дополнительными элементами управления в стандарте), такое дополнительное содержимое автоматически отображается на панели мониторинга.

> [!TIP]
> По мере выпуска корпорацией Майкрософт нового содержимого постоянно обновляется один стандарт, **Azure CIS 1.1.0 (новый)** (официальное название — [Тест производительности CIS для платформ Microsoft Azure версии 1.1.0](https://www.cisecurity.org/benchmark/azure/)). Его нужно добавить на панель мониторинга вместе с Azure CIS 1.1.0 — представлением Azure CIS, которое по умолчанию настраивается в каждой среде Центра безопасности. Этот пакет использует статический набор правил. Новый пакет содержит больше политик и будет со временем автоматически обновляться. Чтобы выполнить обновление до нового динамического пакета, выполните описанные ниже действия.


## <a name="available-packages"></a>Доступные пакеты

Вы можете добавить такие стандарты, как NIST SP 800-53 R4, SWIFT CSP CSCF-v2020, UK Official и UK NHS, Canada Federal PBMM и Azure CIS 1.1.0 (новый), который содержит более полное представление стандарта Azure CIS 1.1.0. 

Кроме того, вы можете добавить **Тест производительности системы безопасности Azure**. Это руководства с рекомендациями по обеспечению безопасности и соответствия требованиям для Azure, подготовленные корпорацией Майкрософт на основе распространенных платформ соответствия. (См. сведения в статье [Общие сведения о Тесте производительности системы безопасности Azure](../security/benchmarks/introduction.md).)

В дальнейшем на панели мониторинга будут поддерживаться и другие доступные стандарты. 


## <a name="add-a-regulatory-standard-to-your-dashboard"></a>Добавление нормативного стандарта на панель мониторинга

Ниже показано, как добавить пакет для отслеживания соответствия одному из поддерживаемых нормативных стандартов.

> [!NOTE]
> Только пользователи с ролями владельца или участника политики имеют достаточные разрешения для добавления стандартов соответствия. 

1. На боковой панели Центра безопасности выберите **Соответствие нормативным требованиям** , чтобы открыть панель мониторинга соответствия нормативным требованиям. Здесь вы увидите стандарты соответствия, назначенные выбранным подпискам.   

1. Вверху страницы щелкните **Управление политиками соответствия**. Откроется страница управления политиками.

1. Выберите здесь подписку или группу управления, для которой вы хотите изменить условия соответствия нормативным требованиям. 

    > [!TIP]
    > Мы рекомендуем выбрать максимальную область применимости стандарта, чтобы данные о соответствии собирались и отслеживались для всех вложенных ресурсов. 

1. Чтобы добавить стандарты, относящиеся к вашей организации, щелкните **Добавить другие стандарты**. 

1. На странице **Добавление стандартов соответствия нормативным требованиям** вы можете искать пакеты для любого из доступных стандартов. Ниже приведены некоторые из доступных стандартов.

    - **Тесты производительности системы безопасности Azure**
    - **NIST SP 800-53, ред. 4**
    - **ДИРЕКТИВА NIST SP 800 171 R2**
    - **SWIFT CSP-CSCF, версия 2020**
    - **UKO и UK NHS**
    - **Canada PBMM**
    
    ![Добавление пакетов соответствия нормативным требованиям на панель мониторинга соответствия нормативным требованиям в Центре безопасности](./media/update-regulatory-compliance-packages/dynamic-regulatory-compliance-additional-standards.png)

1. Выберите **Добавить** и введите все необходимые сведения для конкретной инициативы, например область, параметры и исправление.

1. На боковой панели Центра безопасности снова выберите **Соответствие нормативным требованиям** , чтобы вернуться к панели мониторинга соответствия нормативным требованиям.
    * Новый стандарт появится в списке отраслевых и нормативных требований. 
    * Если вы добавили **Azure CIS 1.1.0 (новый)** , исходное *статическое* представление соответствия требованиям Azure CIS 1.1.0 сохранится рядом с ним. Возможно, в будущем оно будет автоматически удалено.

    > [!NOTE]
    > Стандарт, добавленный на панель мониторинга соответствия, может отобразиться через несколько часов.

    [![Панель мониторинга соответствия нормативным требованиям, где отображаются старые и новые представления Azure CIS](media/update-regulatory-compliance-packages/regulatory-compliance-dashboard-with-benchmark-small.png)](media/update-regulatory-compliance-packages/regulatory-compliance-dashboard-with-benchmark.png#lightbox)


## <a name="removing-a-standard-from-your-dashboard"></a>Удаление стандарта из панели мониторинга

Если какой-либо из представленных нормативных требований не относится к вашей организации, это простой процесс просто удалить их из пользовательского интерфейса. Это позволяет дополнительно настроить панель мониторинга соответствия нормативным требованиям и сосредоточиться только на тех применимых к вам стандартам.

Чтобы удалить стандартный:

1. В меню центра безопасности выберите пункт **Политика безопасности**.

1. Выберите подходящую подписку, из которой необходимо удалить стандарт.

    > [!NOTE]
    > Вы можете удалить стандарт из подписки, но не из группы управления. 

    Откроется страница Политика безопасности. Для выбранной подписки отображается политика по умолчанию, отраслевые и нормативные стандарты, а также все созданные пользовательские инициативы.

    :::image type="content" source="./media/update-regulatory-compliance-packages/remove-standard.png" alt-text="Удаление нормативного стандарта с панели мониторинга соответствия нормативным требованиям в центре безопасности Azure":::

1. Для стандартного, который требуется удалить, выберите **Отключить**. Появится окно подтверждения.

    :::image type="content" source="./media/update-regulatory-compliance-packages/remove-standard-confirm.png" alt-text="Подтвердите, что вы действительно хотите удалить выбранный нормативный стандарт.":::

1. Выберите **Да**. Стандартный будет удален. 


## <a name="next-steps"></a>Дальнейшие действия

В этой статье показано, как **добавить новые пакеты соответствия** для мониторинга соответствия требованиям дополнительных стандартов. 

Другие материалы на эту тему см. в следующих статьях:. 

- [Общие сведения о Тесте производительности системы безопасности Azure](../security/benchmarks/introduction.md)
- [Руководство по обеспечению соответствия нормативным требованиям на высоком уровне](security-center-compliance-dashboard.md)
- [Использование политик безопасности](tutorial-security-policy.md)