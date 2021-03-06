---
title: Аутентификация OAUTH 2,0 с помощью Azure Active Directory
description: Руководство по архитектуре при достижении этого шаблона проверки подлинности
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea22c4e5b363eaa3ecc2a736dfef714666310062
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114373"
---
# <a name="oauth-20-authentication-with-azure-active-directory"></a>Аутентификация OAuth 2,0 с помощью Azure Active Directory

OAuth 2,0 — это промышленный протокол для авторизации. Он позволяет пользователю предоставить ограниченный доступ к защищенным ресурсам. В частности, для работы с протоколом HTTP, OAuth отделяет роль клиента от владельца ресурса. Клиент запрашивает доступ к ресурсам, управляемым владельцем ресурса и размещенным на сервере ресурсов. Сервер ресурсов выдает маркеры доступа с утверждением владельца ресурса. Клиент использует маркеры доступа для доступа к защищенным ресурсам, размещенным на сервере ресурсов. 

OAuth 2,0 напрямую связан с OpenID Connect Connect (OIDC). Так как OIDC — это уровень проверки подлинности и авторизации, основанный на OAuth 2,0, он не обратно совместим с OAuth 1,0. Azure Active Directory (Azure AD) поддерживает все потоки OAuth 2,0. 

## <a name="use-when"></a>Используется, когда:

Для полнофункциональных клиентских & сценариев современных приложений и доступа к веб-API RESTFUL.

![Схема архитектуры](./media/authentication-patterns/oauth.png)

## <a name="components-of-system"></a>Компоненты системы

* **Пользователь**: запрашивает службу из веб-приложения (приложение). Пользователь обычно является владельцем ресурса, которому принадлежат данные, и обладает возможностями, позволяющими клиентам получать доступ к данным или ресурсам. 

* **Веб-браузер**. веб-браузер, с которым взаимодействует пользователь, является клиентом OAuth. 

* **Веб-приложение**. это веб-приложение или сервер ресурсов, где находится ресурс или данные. Он доверяет серверу авторизации безопасную проверку подлинности и авторизацию клиента OAuth. 

* **Azure AD**: Azure AD — это сервер авторизации, также известный как поставщик удостоверений (IDP). Он обеспечивает безопасную обработку любых данных, связанных с данными пользователя, их доступом и отношениями доверия. Он отвечает за выдачу маркеров, которые предоставляют и отменяют доступ к ресурсам.

## <a name="implement-oauth-20-with-azure-ad"></a>Реализация OAuth 2,0 с помощью Azure AD

* [Интеграция приложений с Azure AD](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) 

* [Протоколы OAuth 2,0 и OpenID Connect Connect на платформе Microsoft Identity](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols) 

* [Типы приложений и OAuth2](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) 

 
