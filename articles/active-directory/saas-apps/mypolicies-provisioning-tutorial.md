---
title: Руководство. Настройка myPolicies для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить Azure Active Directory для автоматической инициализации и отзыва учетных записей пользователей в myPolicies.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 55f7b64c9ade91bb2923161d60568e3ea14ee034
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2020
ms.locfileid: "94353525"
---
# <a name="tutorial-configure-mypolicies-for-automatic-user-provisioning"></a>Учебник. Настройка myPolicies для автоматической подготовки пользователей

Цель этого руководства — продемонстрировать шаги, которые необходимо выполнить в myPolicies и Azure Active Directory (Azure AD), чтобы настроить Azure AD для автоматической инициализации и отзыва пользователей и групп в myPolicies.

> [!NOTE]
> В этом руководстве рассматривается соединитель, созданный на базе службы подготовки пользователей Azure AD. Подробные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Сейчас этот соединитель предоставляется в общедоступной предварительной версии. Дополнительные сведения об общих условиях использования продуктов в предварительной версии см. в документе [Дополнительные условия использования Предварительных версий Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Предварительные требования

В сценарии, описанном в этом руководстве, предполагается, что у вас уже имеется:

* Клиент Azure AD.
* [Клиент myPolicies](https://mypolicies.com/index.html#section10).
* Учетная запись пользователя в myPolicies с разрешениями администратора.

## <a name="assigning-users-to-mypolicies"></a>Назначение пользователей в myPolicies

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая *назначением*. В контексте автоматической подготовки учетных записей пользователей синхронизируются только пользователи и группы, назначенные приложению в Azure AD.

Перед настройкой и включением автоматической подготовки пользователей следует решить, какие пользователи и (или) группы в Azure AD должны иметь доступ к myPolicies. После принятия решения вы можете назначить этих пользователей и (или) группы для myPolicies, следуя приведенным ниже инструкциям.
* [Назначение корпоративному приложению пользователя или группы](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-mypolicies"></a>Важные советы по назначению пользователей в myPolicies

* Рекомендуется назначить одного пользователя Azure AD в myPolicies для тестирования конфигурации автоматической подготовки пользователей. Дополнительные пользователи и/или группы можно назначить позднее.

* При назначении пользователя в myPolicies необходимо выбрать в диалоговом окне назначения любую допустимую роль конкретного приложения (если она доступна). Пользователи с ролью **Доступ по умолчанию** исключаются из подготовки.

## <a name="setup-mypolicies-for-provisioning"></a>Настройка myPolicies для подготовки

Перед настройкой myPolicies для автоматической подготовки пользователей с помощью Azure AD необходимо включить подготовку SCIM для myPolicies.

1. Обратитесь к представителям myPolicies по адресу, **support@mypolicies.com** чтобы получить маркер секрета, необходимый для настройки подготовки scim.

2.  Сохраните значение маркера, предоставленное представителями myPolicies. Это значение будет указано в поле **секретный токен** на вкладке Подготовка приложения myPolicies в портал Azure.

## <a name="add-mypolicies-from-the-gallery"></a>Добавление myPolicies из коллекции

Чтобы настроить myPolicies для автоматической подготовки пользователей с помощью Azure AD, необходимо добавить myPolicies из коллекции приложений Azure AD в список управляемых приложений SaaS.

**Чтобы добавить myPolicies из коллекции приложений Azure AD, выполните следующие действия.**

1. В **[портал Azure](https://portal.azure.com)** на панели навигации слева выберите **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, нажмите кнопку **новое приложение** в верхней части области.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **myPolicies** , выберите **myPolicies** на панели результатов и нажмите кнопку **добавить** , чтобы добавить это приложение.

    ![myPolicies в списке результатов](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-mypolicies"></a>Настройка автоматической подготовки пользователей в myPolicies 

В этом разделе описано, как настроить службу подготовки Azure AD для создания, обновления и отключения пользователей и (или) групп в myPolicies на основе назначений пользователей и групп в Azure AD.

> [!TIP]
> Вы также можете включить единый вход на основе SAML для myPolicies, следуя инструкциям, приведенным в [руководстве по единому входу myPolicies](mypolicies-tutorial.md). Единый вход можно настроить независимо от автоматической подготовки пользователей, хотя эти две возможности дополняют друг друга.

### <a name="to-configure-automatic-user-provisioning-for-mypolicies-in-azure-ad"></a>Чтобы настроить автоматическую подготовку учетных записей пользователей для myPolicies в Azure AD, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com). Выберите **Корпоративные приложения** , а затем **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **myPolicies**.

    ![Ссылка на myPolicies в списке приложений](common/all-applications.png)

3. Выберите вкладку **Подготовка**.

    ![Снимок экрана параметров управления с вызываемым параметром подготовки.](common/provisioning.png)

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

    ![Снимок экрана: раскрывающийся список режима подготовки с вызываемым автоматическим параметром.](common/provisioning-automatic.png)

5. В разделе **учетные данные администратора** введите `https://<myPoliciesCustomDomain>.mypolicies.com/scim` **URL-адрес клиента** , где `<myPoliciesCustomDomain>` — это пользовательский домен myPolicies. Вы можете получить домен клиента myPolicies из URL-адреса.
Пример: `<demo0-qa>` . mypolicies.com.

6. В поле **секретный токен** введите значение токена, которое было получено ранее. Щелкните **проверить подключение** , чтобы убедиться, что Azure AD может подключиться к myPolicies. Если подключение не выполняется, убедитесь, что у учетной записи myPolicies есть разрешения администратора, и повторите попытку.

    ![URL-адрес клиента + токен](common/provisioning-testconnection-tenanturltoken.png)

7. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок **Send an email notification when a failure occurs** (Отправить уведомление по электронной почте при сбое).

    ![Почтовое уведомление](common/provisioning-notification-email.png)

8. Выберите команду **Сохранить**.

9. В разделе **сопоставления** выберите **синхронизировать Azure Active Directory пользователей с myPolicies**.

    :::image type="content" source="media/mypolicies-provisioning-tutorial/usermapping.png" alt-text="Снимок экрана раздела &quot;сопоставления&quot;. В поле Имя выполните синхронизацию Azure Active Directory пользователей с кустомаппссо." border="false":::

10. Проверьте атрибуты пользователя, которые синхронизированы из Azure AD в myPolicies в разделе **сопоставление атрибутов** . Атрибуты, выбранные как свойства **Matching** , используются для сопоставления учетных записей пользователей в myPolicies для операций обновления. Нажмите кнопку **Сохранить** , чтобы зафиксировать все изменения.

   |attribute|Тип|
   |---|---|
   |userName|Строка|
   |active|Логическое|
   |emails[type eq "work"].value|Строка|
   |name.givenName|Строка|
   |name.familyName|Строка|
   |name.formatted|Строка|
   |externalId|Строка|
   |addresses[type eq "work"].country|Строка|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|Справочник|


11. Чтобы настроить фильтры области, ознакомьтесь со следующими инструкциями, предоставленными в [руководстве по фильтрам области](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

12. Чтобы включить службу подготовки Azure AD для myPolicies, измените значение параметра **состояние подготовки** на **включено** в разделе **Параметры** .

    ![Состояние подготовки "Включено"](common/provisioning-toggle-on.png)

13. Определите пользователей и (или) группы, которые вы хотите подготавливать к myPolicies, выбрав нужные значения в **области** в разделе **Параметры** .

    ![Область действия подготовки](common/provisioning-scope.png)

14. Когда будете готовы выполнить подготовку, нажмите кнопку **Сохранить**.

    ![Сохранение конфигурации подготовки](common/provisioning-configuration-save.png)

После этого начнется начальная синхронизация пользователей и (или) групп, определенных в поле **Область** раздела **Параметры**. Начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Если служба запущена, они выполняются примерно каждые 40 минут. В разделе **сведения о синхронизации** можно отслеживать ход выполнения и переходить по ссылкам для просмотра отчетов по подготовке, в которых описаны все действия, выполняемые службой подготовки Azure AD в myPolicies.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Ограничения соединителя

* myPolicies всегда требует **имя пользователя** , **адрес электронной почты** и **externalId**.
* myPolicies не поддерживает жесткие удаления для атрибутов пользователей.

## <a name="change-log"></a>Журнал изменений

* 09/15/2020 — добавлена поддержка атрибута Country для пользователей.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](../app-provisioning/check-status-user-account-provisioning.md)
