---
title: Предоставление согласия администратора на уровне клиента приложению в Azure AD
description: Узнайте, как предоставить приложению согласие на выполнение приложения, чтобы конечные пользователи не запрашивают согласие при входе в приложение.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 11/04/2019
ms.author: kenwith
ms.reviewer: phsignor
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9680c9bee6d0cf5c9605ce7b6009a500abd81ffb
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2020
ms.locfileid: "92369103"
---
# <a name="grant-tenant-wide-admin-consent-to-an-application"></a>Предоставление приложению согласия администратора на уровне арендатора

Узнайте, как упростить взаимодействие с пользователем, предоставив приложению согласие администратора на уровне клиента. В этой статье представлены различные способы достижения этой ситуации. Эти методы применяются для всех пользователей в клиенте Azure Active Directory (AAD).

Дополнительные сведения о предоставлении согласия для приложений см. в статье [Платформа предоставления согласия Azure Active Directory](../develop/consent-framework.md).

## <a name="prerequisites"></a>Обязательные условия

Для предоставления согласия администратора на уровне клиента необходимо войти как [глобальный администратор](../roles/permissions-reference.md#global-administrator--company-administrator), [Администратор приложения](../roles/permissions-reference.md#application-administrator)или [Администратор облачных приложений](../roles/permissions-reference.md#cloud-application-administrator).

> [!IMPORTANT]
> Когда приложению было предоставлено согласие администратора на уровне клиента, все пользователи смогут войти в приложение, если оно не настроено для обязательного назначения пользователей. Чтобы ограничить круг пользователей, которые могут входить в приложение, требуется назначение пользователей и назначьте приложению пользователей или группы. Подробнее см. статью [Методы назначения пользователей и групп](methods-for-assigning-users-and-groups.md).
>
> Роль глобального администратора необходима для предоставления согласия администратора на разрешения приложений для Microsoft Graph API.

> [!WARNING]
> Предоставление согласия администратора на уровне клиента приложению предоставит приложению и издателю приложения доступ к данным вашей организации. Внимательно просматривайте разрешения, запрашиваемые приложением, перед предоставлением согласия.
>
> Роль глобального администратора необходима для предоставления согласия администратора на разрешения приложений для Microsoft Graph API.

## <a name="grant-admin-consent-from-the-azure-portal"></a>Предоставление согласия администратора от портал Azure

### <a name="grant-admin-consent-in-enterprise-apps"></a>Предоставление согласия администратора в корпоративных приложениях

Вы можете предоставить согласие администратора на уровне клиента через *корпоративные приложения* , если приложение уже подготовлено в клиенте. Например, приложение может быть подготовлено в клиенте, если хотя бы один пользователь уже выполнил отправку в приложение. Дополнительные сведения см. [в разделе как и почему приложения добавляются в Azure Active Directory](../develop/active-directory-how-applications-are-added.md).

Чтобы предоставить согласие администратора на уровне клиента для приложения, указанного в **корпоративном приложении**:

1. Войдите в [портал Azure](https://portal.azure.com) в качестве [глобального администратора](../roles/permissions-reference.md#global-administrator--company-administrator), [администратора приложения](../roles/permissions-reference.md#application-administrator)или [администратора облачных приложений](../roles/permissions-reference.md#cloud-application-administrator).
2. Выберите **Azure Active Directory** **корпоративные приложения**.
3. Выберите приложение, которому необходимо предоставить согласие администратора на уровне клиента.
4. Выберите **разрешения** и щелкните **предоставить согласие администратора**.
5. Внимательно ознакомьтесь с разрешениями, которые требуются приложению.
6. Если вы согласны с разрешениями, которые требуются приложению, предоставьте согласие. В противном случае нажмите кнопку **Отмена** или закройте окно.

> [!WARNING]
> Предоставление согласия администратора на уровне клиента с помощью **корпоративных приложений** приведет к отмене всех разрешений, которые ранее были предоставлены на уровне клиента. Разрешения, которые ранее были предоставлены пользователям от своего имени, не затрагиваются. 

### <a name="grant-admin-consent-in-app-registrations"></a>Предоставление согласия администратора в Регистрация приложений

Для приложений, разработанных вашей организацией или зарегистрированных непосредственно в клиенте Azure AD, можно также предоставить согласие администратора на уровне клиента от **Регистрация приложений** в портал Azure.

Чтобы предоставить согласие администратора на уровне клиента от **Регистрация приложений**:

1. Войдите в [портал Azure](https://portal.azure.com) в качестве [глобального администратора](../roles/permissions-reference.md#global-administrator--company-administrator), [администратора приложения](../roles/permissions-reference.md#application-administrator)или [администратора облачных приложений](../roles/permissions-reference.md#cloud-application-administrator).
2. Выберите **Azure Active Directory** затем **Регистрация приложений**.
3. Выберите приложение, которому необходимо предоставить согласие администратора на уровне клиента.
4. Выберите **разрешения API** и щелкните **предоставить согласие администратора**.
5. Внимательно ознакомьтесь с разрешениями, которые требуются приложению.
6. Если вы согласны с разрешениями, которые требуются приложению, предоставьте согласие. В противном случае нажмите кнопку **Отмена** или закройте окно.

> [!WARNING]
> Предоставление согласия администратора на уровне клиента с помощью **Регистрация приложений** приведет к отмене всех разрешений, которые ранее были предоставлены на уровне клиента. Разрешения, которые ранее были предоставлены пользователям от своего имени, не затрагиваются. 

## <a name="construct-the-url-for-granting-tenant-wide-admin-consent"></a>Создание URL-адреса для предоставления согласия администратора на уровне клиента

При предоставлении согласия администратора на уровне клиента с помощью любого из описанных выше методов, откроется окно из портал Azure, чтобы запросить согласие администратора на уровне клиента. Если известен идентификатор клиента (также называемый ИДЕНТИФИКАТОРом приложения) приложения, можно создать тот же URL-адрес, чтобы предоставить согласие администратора на уровне клиента.

URL-адрес согласия администратора на уровне клиента следует в следующем формате:

```http
https://login.microsoftonline.com/{tenant-id}/adminconsent?client_id={client-id}
```

Где:

* `{client-id}` Идентификатор клиента приложения (также известный как идентификатор приложения).
* `{tenant-id}` — Идентификатор клиента вашей организации или любое проверенное доменное имя.

Как всегда, внимательно просматривайте разрешения, запрашиваемые приложением, перед предоставлением согласия.

> [!WARNING]
> Предоставление согласия администратора на уровне клиента с помощью этого URL-адреса приведет к отмене всех разрешений, которые ранее были предоставлены для всего клиента. Разрешения, которые ранее были предоставлены пользователям от своего имени, не затрагиваются. 

## <a name="next-steps"></a>Дальнейшие шаги

[Настройка согласия конечных пользователей для приложений](configure-user-consent.md)

[Настройка рабочего процесса согласия администратора](configure-admin-consent-workflow.md)

[Разрешения и согласие для платформы удостоверений Майкрософт](../develop/active-directory-v2-scopes.md)

[Azure AD в StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
