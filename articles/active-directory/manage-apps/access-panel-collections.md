---
title: Создание коллекций для порталов "Мои приложения" в Azure Active Directory | Документация Майкрософт
description: Используйте мои коллекции "Мои приложения", чтобы настроить страницы "Мои приложения" для упрощения работы своих приложений для конечных пользователей. Упорядочите приложения в группы с помощью отдельных вкладок.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 02/10/2020
ms.author: kenwith
ms.reviewer: kasimpso
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f520141d36726e94dc8d49d7e5aa95bb35d5484
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "85956242"
---
# <a name="create-collections-on-the-my-apps-portal"></a>Создание коллекций на портале "Мои приложения"

Пользователи могут использовать портал "Мои приложения" для просмотра и запуска облачных приложений, к которым у них есть доступ. По умолчанию все приложения, к которым у пользователя есть доступ, перечисляются на одной странице. Чтобы лучше организовать эту страницу для пользователей, при наличии лицензии Azure AD Premium P1 или P2 можно настроить коллекции. С помощью коллекции можно сгруппировать взаимосвязанные приложения (например, роль задания, задачу или проект) и отобразить их на отдельной вкладке. По сути, коллекция применяет фильтр к приложениям, к которым у пользователя уже есть доступ, поэтому пользователь видит только те приложения из коллекции, которые были назначены им.

> [!NOTE]
> В этой статье описывается, как администратор может включать и создавать коллекции. Сведения о том, как использовать портал и коллекции "Мои приложения", см. в разделе [доступ и использование коллекций](https://docs.microsoft.com/azure/active-directory/user-help/my-applications-portal-workspaces).

## <a name="enable-the-latest-my-apps-features"></a>Включить новейшие функции "Мои приложения"

1. Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как администратор пользователя или глобальный администратор.

2. Выберите **Azure Active Directory**  >  **Параметры пользователя**.

3. В разделе Предварительный **Просмотр пользовательских функций**выберите пункт **Управление параметрами предварительной версии функции пользователя**.

4. В разделе **пользователи могут использовать функции предварительной версии для моих приложений**выберите один из следующих вариантов.
   * **Выбрано** — включает функции для определенной группы. Используйте параметр **выбрать группу** , чтобы выбрать группу, для которой необходимо включить функции.  
   * **Все** — включает функции для всех пользователей.

> [!NOTE]
> Чтобы открыть портал "Мои приложения", пользователи могут использовать ссылку `https://myapps.microsoft.com` или настроенную ссылку для вашей организации, например `https://myapps.microsoft.com/contoso.com` . После включения интерфейса "новые приложения" в верхней части страницы "Мои приложения" отобразится баннер с **обновленным интерфейсом** "Мои приложения". пользователи смогут выбрать вариант **попробовать** , чтобы просмотреть новый интерфейс. Чтобы приостанавливать использование нового интерфейса, пользователи могут выбрать **Да** в заголовке **покинуть новый интерфейс** в верхней части страницы.

## <a name="create-a-collection"></a>Создание коллекции

Чтобы создать коллекцию, необходимо иметь лицензию Azure AD Premium P1 или P2.

1. Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как администратор с лицензией Azure AD Premium P1 или P2.

2. Перейдите в раздел **Azure Active Directory**  >  **корпоративные приложения**.

3. В разделе **Управление**выберите **коллекции**.

4. Выберите **создать коллекцию**. На странице **Новая коллекция** введите **имя** коллекции (в имени не рекомендуется использовать "Collection". Затем введите **Описание**.

   ![Страница «Создание коллекции»](media/acces-panel-collections/new-collection.png)

5. Перейдите на вкладку **приложения** . Выберите **+ Добавить приложение**, а затем на странице **Добавление приложений** выберите все приложения, которые необходимо добавить в коллекцию, или используйте поле **поиска** для поиска приложений.

   ![Добавление приложения в коллекцию](media/acces-panel-collections/add-applications.png)

6. Завершив добавление приложений, нажмите кнопку **Добавить**. Откроется список выбранных приложений. Для изменения порядка приложений в списке можно использовать кнопки со стрелками вверх. Чтобы переместить приложение или удалить его из коллекции, выберите меню **More** (**...**).

7. Перейдите на вкладку **владельцы** . Выберите **+ Добавить пользователей и группы**, а затем на странице **Добавление пользователей и групп** выберите пользователей или группы, которым нужно назначить владение. Завершив выбор пользователей и групп, нажмите кнопку **выбрать**.

9. Перейдите на вкладку **Пользователи и группы** . Выберите **+ Добавить пользователей и группы**, а затем на странице **Добавление пользователей и групп** выберите пользователей или группы, которым нужно назначить коллекцию. Или используйте поле **поиска** для поиска пользователей или групп. Завершив выбор пользователей и групп, нажмите кнопку **выбрать**.

   ![Добавление пользователей и групп](media/acces-panel-collections/add-users-and-groups.png)

11. Выберите **Review + Create** (Просмотреть и создать). Отобразятся свойства новой коллекции.


## <a name="view-audit-logs"></a>Просмотр журналов аудита

В журналах аудита регистрируются операции коллекций приложений, включая действия, выполняемые конечным пользователем для создания коллекции. Из папки "Мои приложения" создаются следующие события:

* Создать коллекцию
* Редактировать коллекцию
* Удаление коллекции
* Запуск приложения (конечный пользователь)
* Самостоятельное добавление приложений (конечный пользователь)
* Самостоятельное удаление приложения (конечный пользователь)

Вы можете получить доступ к журналам аудита в [портал Azure](https://portal.azure.com) , выбрав **Azure Active Directory**  >  **корпоративные приложения**  >  **журналы аудита** в разделе действие. Для **службы**выберите **Мои приложения**.

## <a name="get-support-for-my-account-pages"></a>Получение поддержки для страниц моей учетной записи

На странице Мои приложения пользователь может выбрать пункт **Моя учетная запись**  >  **Просмотреть мою учетную** запись, чтобы открыть параметры учетной записи. На странице **учетная запись** Azure AD пользователи могут управлять сведениями о безопасности, устройствами, паролями и т. д. Они также могут получить доступ к параметрам учетной записи Office.

Если вам нужно отправить запрос в службу поддержки на странице учетной записи Azure AD или учетной записи Office, выполните следующие действия, чтобы запрос направлялся правильно: 

* Чтобы получить сведения о проблемах со страницей **"Моя учетная запись" Azure AD** , откройте запрос в службу поддержки в портал Azure. Перейдите в **портал Azure**  >  **Azure Active Directory**  >  **новый запрос на поддержку**.

* Чтобы получить сведения о проблемах с страницей **"Моя учетная запись" Office** , откройте запрос в службу поддержки в Microsoft 365 центре администрирования. Перейдите в раздел **Microsoft 365 Поддержка центра администрирования**  >  **Support**. 

## <a name="next-steps"></a>Дальнейшие действия
[Возможности для взаимодействия пользователя с приложениями в Azure Active Directory](end-user-experiences.md)