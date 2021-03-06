---
title: Управление несколькими арендаторами в Индексаторе видео (Azure)
description: В этой статье описаны разные варианты интеграции, позволяющие управлять в Индексаторе видео несколькими арендаторами.
services: media-services
documentationcenter: ''
author: ika-microsoft
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 05/15/2019
ms.author: ikbarmen
ms.openlocfilehash: 18f2cf3daa281400151ba223e1735e7138d97e8e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "76990510"
---
# <a name="manage-multiple-tenants"></a>Управление несколькими арендаторами

В этой статье описаны разные варианты интеграции, позволяющие управлять в Индексаторе видео несколькими арендаторами. Выберите метод, который лучше всего подходит для вашего сценария:

* Отдельная учетная запись Индексатора видео для каждого арендатора
* одна учетная запись Индексатора видео для всех арендаторов;
* Подписка Azure для каждого арендатора

## <a name="video-indexer-account-per-tenant"></a>Отдельная учетная запись Индексатора видео для каждого арендатора

При использовании этой архитектуры для каждого арендатора создается учетная запись Индексатора видео. Арендаторы получают полную изоляцию на уровнях постоянного хранилища и вычислений.  

![Отдельная учетная запись Индексатора видео для каждого арендатора](./media/manage-multiple-tenants/video-indexer-account-per-tenant.png)

### <a name="considerations"></a>Рекомендации

* Клиенты используют разные учетные записи хранения (если клиент не настроит другой вариант вручную).
* Клиенты используют разные вычислительные ресурсы (зарезервированные единицы), и не влияют на время обработки заданий друг друга.
* Вы можете легко удалить из системы любого арендатора, просто удалив его учетную запись Индексатора видео.
* Нет возможности использовать пользовательские модели для нескольких арендаторов.

    Убедитесь, что для бизнеса нет необходимости совместно использовать пользовательские модели.
* Управление усложняется из-за наличия нескольких учетных записей Индексатора видео и связанных с ними Служб мультимедиа для каждого арендатора.

> [!TIP]
> Создайте для вашей системы пользователя с правами администратора на [Портале разработчика Индексатора видео](https://api-portal.videoindexer.ai/) и предоставьте арендаторам соответствующие [маркеры доступа учетных записей](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Account-Access-Token) с помощью API авторизации.

## <a name="single-video-indexer-account-for-all-users"></a>Одна учетная запись Индексатора видео для всех пользователей

При использовании этой архитектуры ответственность за изоляцию арендаторов возлагается на клиента. Все клиенты должны использовать одну учетную запись Индексатора видео и одну учетную запись Службы мультимедиа Azure. При передаче, поиске и удалении содержимого клиенту придется фильтровать результаты для каждого арендатора.

![Одна учетная запись Индексатора видео для всех пользователей](./media/manage-multiple-tenants/single-video-indexer-account-for-all-users.png)

В этом варианте модели настройки (контактное лицо, язык и торговые марки) можно использовать между арендаторами совместно или изолированно, выполняя фильтрацию моделей по арендатору.

При [отправке видео](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) вы можете использовать для каждого арендатора свой атрибут секции. Это позволит реализовать изоляцию в [API поиска](https://api-portal.videoindexer.ai/docs/services/operations/operations/Search-videos?). Указав в API поиска нужный атрибут секции, вы получите результаты только для указанной секции. 

### <a name="considerations"></a>Рекомендации

* Возможность совместно использовать содержимое и модели настройки для нескольких арендаторов.
* Каждый арендатор влияет на производительность системы для других арендаторов.
* Пользователю потребуется сложный слой управления поверх Индексатора видео.

> [!TIP]
> Вы можете использовать атрибут [priority](upload-index-videos.md), чтобы распределять приоритеты для заданий арендаторов.

## <a name="azure-subscription-per-tenant"></a>Подписка Azure для каждого арендатора 

При использовании этой архитектуры каждый клиент будет иметь собственную подписку Azure. Для каждого пользователя вы создаете в подписке соответствующего арендатора учетную запись Индексатора видео.

![Подписка Azure для каждого арендатора](./media/manage-multiple-tenants/azure-subscription-per-tenant.png)

### <a name="considerations"></a>Рекомендации

* Это единственный вариант, который позволяет раздельно выставлять счета.
* Такая интеграция влечет больше накладных расходов на управление, чем отдельная учетная запись Индексатора видео для каждого арендатора. Если раздельное выставление счетов не является обязательным, мы рекомендуем выбрать другой вариант из числа описанных в этой статье.

## <a name="next-steps"></a>Дальнейшие действия

[Обзор](video-indexer-overview.md)
