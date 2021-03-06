---
title: Руководство по интеграции единого входа Azure Active Directory с Blue Access for Members (BAM) | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Blue Access for Members (BAM).
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/06/2019
ms.author: jeedes
ms.openlocfilehash: 38765fd6d740c7494cbf7e5a0a38f1d98aecf4a6
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2020
ms.locfileid: "92456982"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-blue-access-for-members-bam"></a>Руководство по интеграции единого входа Azure Active Directory с Blue Access for Members (BAM)

В этом учебнике описано, как интегрировать Blue Access for Members (BAM) с Azure Active Directory (Azure AD). Интеграция Blue Access for Members (BAM) с Azure AD обеспечивает следующие возможности:

* Управление доступом к Blue Access for Members (BAM) в Azure AD.
* Включение автоматического входа пользователей в Blue Access for Members (BAM) с помощью их учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Blue Access for Members (BAM) с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.


* Blue Access for Members (BAM) поддерживает единый вход, инициированный **поставщиком удостоверений**.




## <a name="adding-blue-access-for-members-bam-from-the-gallery"></a>Добавление Blue Access for Members (BAM) из коллекции

Чтобы настроить интеграцию Blue Access for Members (BAM) с Azure AD, необходимо добавить Blue Access for Members (BAM) из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Blue Access for Members (BAM)** .
1. Выберите **Blue Access for Members (BAM)** на панели результатов, а затем добавьте приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-single-sign-on-for-blue-access-for-members-bam"></a>Настройка и проверка единого входа Azure AD для Blue Access for Members (BAM)

Настройте и проверьте единый вход Azure AD в Blue Access for Members (BAM) с помощью тестового пользователя **B. Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Blue Access for Members (BAM).

Чтобы настроить и проверить единый вход Azure AD в Blue Access for Members (BAM), выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    * **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    * **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Blue Access for Members (BAM)](#configure-blue-access-for-members-bam-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    * **[Создание тестового пользователя Blue Access for Members (BAM)](#create-blue-access-for-members-bam-test-user)** требуется для того, чтобы в Blue Access for Members (BAM) существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Blue Access for Members (BAM)** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `<Custom Domain Value>`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<CUSTOMURL>/affwebservices/public/saml2assertionconsumer`.

    c. Щелкните **Задать дополнительные URL-адреса**.

    d. В текстовом поле **Состояние ретранслятора** введите URL-адрес в формате `https://<CUSTOMURL>/BAMSSOServlet/sso/BamInboundSsoServlet`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените эти значения фактическим идентификатором, URL-адресом ответа и состоянием ретранслятора. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов Blue Access for Members (BAM)](https://www.bcbstx.com/contact-us). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. Приложение Blue Access for Members (BAM) ожидает проверочные утверждения SAML в определенном формате, который требует добавить сопоставления настраиваемых атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    ![Изображение](common/default-attributes.png)

1. В дополнение к описанному выше приложение Blue Access for Members (BAM) ожидает несколько дополнительных атрибутов в ответе SAML, как показано ниже. Эти атрибуты также заранее заполнены, но вы можете изменить их в соответствии со своими требованиями.

    | Имя |  Исходный атрибут|
    | ------------ | --------- |
    | ClientID | `<ClientID>` |
    | ИД пользователя | `<UID>` |

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите элемент **XML метаданных федерации** и выберите **Скачать** , чтобы скачать сертификат и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

1. Требуемый URL-адрес можно скопировать из раздела **Настройка Blue Access for Members (BAM)** .

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory** , **Пользователи** , а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя B. Simon, предоставив этому пользователю доступ к Blue Access for Members (BAM).

1. На портале Azure выберите **Корпоративные приложения** , а затем — **Все приложения**.
1. Из списка приложений выберите **Blue Access for Members (BAM)** .
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя** , а в диалоговом окне **Добавление назначения**  выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать** , расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-blue-access-for-members-bam-sso"></a>Настройка единого входа в Blue Access for Members (BAM)

Чтобы настроить единый вход на стороне **Blue Access for Members (BAM)** , нужно отправить скачанный **XML-файл метаданных федерации** и соответствующие URL-адреса, скопированные на портале Azure, в [группу поддержки Blue Access for Members (BAM)](https://www.bcbstx.com/contact-us). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-blue-access-for-members-bam-test-user"></a>Создание тестового пользователя в Blue Access for Members (BAM)

В этом разделе описано, как создать пользователя B. Simon в приложении Blue Access for Members (BAM). Чтобы добавить пользователей на платформу Blue Access for Members (BAM), обратитесь в [службу поддержки этой платформы](https://www.bcbstx.com/contact-us). Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Blue Access for Members (BAM) на Панели доступа, вы автоматически войдете в приложение Blue Access for Members (BAM), для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)

- [Попробуйте использовать Blue Access for Members (BAM) с Azure AD](https://aad.portal.azure.com/)