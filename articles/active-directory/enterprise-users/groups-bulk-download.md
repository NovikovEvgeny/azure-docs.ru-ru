---
title: Скачайте список групп на портале Azure Active Directory | Документация Майкрософт
description: Пакетная служба скачивает свойства группы в центре администрирования Azure в Azure Active Directory.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 04/16/2020
ms.topic: how-to
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: db800f3827d1b818115c886412ff7d4ffea3e768
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2020
ms.locfileid: "92376862"
---
# <a name="bulk-download-a-list-of-groups-in-azure-active-directory"></a>Выполнить пакетную загрузку списка групп в Azure Active Directory

С помощью портала Azure Active Directory (Azure AD) можно выполнить пакетную загрузку списка всех групп в Организации в файл значений с разделителями-запятыми (CSV).

## <a name="to-download-a-list-of-groups"></a>Загрузка списка групп

1. Войдите в [портал Azure](https://portal.azure.com) с помощью учетной записи администратора в Организации.
1. В Azure AD выберите **группы**  >  **скачать группы**.
1. На странице **загрузки групп** выберите **начать** , чтобы получить CSV-файл со списком ваших групп.

   ![Команда скачать группы находится на странице все группы.](./media/groups-bulk-download/bulk-download.png)

## <a name="check-download-status"></a>Проверить состояние скачивания

Состояние всех ожидающих выполнения массовых запросов можно просмотреть на странице **Результаты массовой операции**.

[![Проверьте состояние на странице результатов с массовыми операциями.](./media/groups-bulk-download/bulk-center.png)](./media/groups-bulk-download/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>Ограничения службы пакетного скачивания

Каждое групповое действие по скачиванию списка групп может выполняться в течение одного часа. Это позволяет скачать список по крайней мере 300 000 групп.

## <a name="next-steps"></a>Дальнейшие действия

- [Массовое удаление участников группы](groups-bulk-remove-members.md)
- [Загрузка участников группы](groups-bulk-download-members.md)
