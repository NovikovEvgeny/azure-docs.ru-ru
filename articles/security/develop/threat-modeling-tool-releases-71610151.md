---
title: Microsoft Threat Modeling Tool выпуск 10/16/2019 — Azure
description: Документирование заметок о выпуске для 7.1.61015.1 выпуска средства моделирования угроз.
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 10/16/2019
ms.openlocfilehash: e45d1347993611130d1bb5036ad4638210c15517
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91317945"
---
# <a name="threat-modeling-tool-update-release-71610151---10162019"></a>Threat Modeling Tool, выпуск обновления 7.1.61015.1 от 16.10.2019

Версия 7.1.61015.1 Microsoft Threat Modeling Tool (ТМТ) была выпущена 16 2019 октября и содержит следующие изменения:

- Улучшения специальных возможностей
- Исправления ошибок
- Новые наборы элементов для Azure Logic Apps и Azure обозреватель данных

## <a name="notable-bug-fixes"></a>Исправления важных ошибок

### <a name="improved-backward-compatibility-with-files-created-in-threat-modeling-tool-2016"></a>Улучшена обратная совместимость с файлами, созданными в "Threat Modeling Tool 2016"

Были исправлены некоторые ошибки, связанные с открытием или отображением файлов модели угроз, созданных в "Threat Modeling Tool 2016".

## <a name="feature-enhancements"></a>Усовершенствования функций

### <a name="new-stencils-for-azure-logic-apps-and-azure-data-explorer"></a>Новые наборы элементов для Azure Logic Apps и Azure обозреватель данных

Новые наборы элементов для Azure Logic Apps и Azure обозреватель данных были добавлены в набор элементов Azure вместе со связанными с ними угрозами и их устранением.

![Наборы элементов Azure Logic Apps и Azure обозреватель данных](./media/threat-modeling-tool-releases-71610151/tmt-logic-apps.png)

## <a name="known-issues"></a>Известные проблемы

### <a name="errors-related-to-priority-values-outside-of-the-expected-ranges"></a>Ошибки, связанные со значениями приоритета за пределами ожидаемых диапазонов

Некоторые клиенты сообщали о получении следующего сообщения об ошибке при открытии файлов, созданных в "Threat Modeling Tool 2016" или пользовательских шаблонах:

```output
System.InvalidOperationException: Invalid Priority value. Accepted values are [0..4] and 'High', 'Medium', 'Low' at ThreatModeling.Model.Threat.get_Priority()

System.ArgumentOutOfRangeException: Accepted values are 'High', 'Medium', and 'Low' Parameter name: value Actual value was 5.6. at ThreatModeling.Model.Threat.set_Priority(String value)
```

Эта проблема находится в расследовании

## <a name="system-requirements"></a>Требования к системе

- Поддерживаемые операционные системы
  - [Юбилейное обновление Microsoft Windows 10](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) или более поздняя версия
- Требуемая версия .NET
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) или более поздней версии
- Дополнительные требования
  - Для получения обновлений средства и шаблонов требуется подключение к Интернету.

## <a name="documentation-and-feedback"></a>Документация и отзывы

- Документация по Threat Modeling Tool находится на сайте [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) и содержит информацию [об использовании средства](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Дальнейшие шаги

Скачайте последнюю версию [Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool).
