---
title: О соединителях API в потоках самостоятельной регистрации в Azure AD
description: Используйте соединители API Azure Active Directory (Azure AD) для настройки и расширения возможностей самостоятельной регистрации пользователей с помощью веб-API.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: cdb6e85b6d81de3d4b88ba315ddd35bd5b37ae7a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88165215"
---
# <a name="use-api-connectors-to-customize-and-extend-self-service-sign-up"></a>Использование соединителей API для настройки и расширения самостоятельной регистрации 

## <a name="overview"></a>Обзор 
Как разработчик или ИТ-администратор, вы можете использовать соединители API для интеграции [пользователя регистрации самообслуживания](self-service-sign-up-overview.md) с внешними системами, используя веб API. Например, соединители API можно использовать для:

- [**Интеграция с настраиваемым рабочим процессом утверждения**](self-service-sign-up-add-approvals.md). Подключитесь к пользовательской системе утверждения для управления созданием учетной записи.
- [**Выполните проверку личности**](code-samples-self-service-sign-up.md#identity-verification). Используйте службу проверки личности, чтобы добавить дополнительный уровень безопасности для принятия решений о создании учетных записей.
- **Проверять входные данные пользователя.** Проверка на соответствие некорректным или недопустимым данным пользователя. Например, можно проверить данные, предоставленные пользователем, по существующим данным во внешнем хранилище данных или в списке разрешенных значений. Если значение недопустимо, можно попросить пользователя предоставить допустимые данные или запретить пользователю продолжать процесс регистрации.
- **Перезаписать атрибуты пользователя**. Переформатируйте или присвойте значение атрибуту, полученному от пользователя. Например, если пользователь вводит имя только строчными или заглавными буквами, вы можете изменить его формат, оставив прописной только первую букву. 
<!-- - **Enrich user data**. Integrate with your external cloud systems that store user information to integrate them with the sign-up flow. For example, your API can receive the user's email address, query a CRM system, and return the user's loyalty number. Returned claims can be used to pre-fill form fields or return additional data in the application token.  -->
- **Запускать пользовательскую бизнес-логику**. Вы можете активировать нисходящие события в облачных системах для отправки push-уведомлений, обновления корпоративных баз данных, управления разрешениями, аудита баз данных и выполнения других настраиваемых действий.

Соединитель API предоставляет Azure Active Directory с информацией, необходимой для вызова конечной точки API, определяя URL-адрес конечной точки HTTP и проверку подлинности. После настройки соединителя API его можно включить для определенного шага в потоке пользователя. Когда пользователь достигает этого этапа в потоке регистрации, соединитель API вызывается и представляется как запрос HTTP POST к API, отправляя сведения о пользователе ("утверждения") в качестве пар "ключ-значение" в теле JSON. Ответ API может повлиять на выполнение потока пользователя. Например, ответ API может блокировать регистрацию пользователя, попросите пользователя повторно ввести сведения или перезаписать и добавить атрибуты пользователя.

## <a name="where-you-can-enable-an-api-connector-in-a-user-flow"></a>Где можно включить соединитель API в потоке пользователя

Существует два места в потоке пользователя, где можно включить соединитель API:

- После входа с помощью поставщика удостоверений
- Перед созданием пользователя

> [!IMPORTANT]
> В обоих случаях соединители API вызываются во время **регистрации**пользователя, а не для входа.

### <a name="after-signing-in-with-an-identity-provider"></a>После входа с помощью поставщика удостоверений

Соединитель API на этом этапе процесса регистрации вызывается сразу после проверки подлинности пользователя с помощью поставщика удостоверений (Google, Facebook, Azure AD). Этот шаг предшествует ***странице Коллекция атрибутов***, которая является формой, представленной пользователю для сбора атрибутов пользователя. Ниже приведены примеры сценариев соединителя API, которые можно включить на этом шаге.

- Используйте адрес электронной почты или федеративный идентификатор, предоставленный пользователем для поиска утверждений в существующей системе. Верните эти утверждения из существующей системы, предварительно заполнив страницу коллекции атрибутов и сделав их доступными для возврата в маркер.
- Проверьте, включен ли пользователь в список разрешений или запретов, и контролируйте, можно ли продолжить процесс регистрации.

### <a name="before-creating-the-user"></a>Перед созданием пользователя

Соединитель API на этом этапе в процессе регистрации вызывается после страницы коллекции атрибутов, если она включена. Этот шаг всегда вызывается перед созданием учетной записи пользователя в Azure AD. Ниже приведены примеры сценариев, которые можно включить на этом этапе во время регистрации.

- Проверьте входные данные пользователя и попросите пользователя повторно отправить данные.
- Блокировка регистрации пользователя на основе данных, вводимых пользователем.
- Выполните проверку личности.
- Запросите внешние системы для существующих данных о пользователе, чтобы вернуть их в маркер приложения или сохранить в Azure AD.

<!-- > [!IMPORTANT]
> If an invalid response is returned or another error occurs (for example, a network error), the user will be redirected to the app with the error re -->

## <a name="next-steps"></a>Дальнейшие шаги
- Узнайте, как [Добавить соединитель API в поток пользователя](self-service-sign-up-add-api-connector.md)
- Узнайте, как [добавить пользовательскую систему утверждения для самостоятельной регистрации](self-service-sign-up-add-approvals.md)