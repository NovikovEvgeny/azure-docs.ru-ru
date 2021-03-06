---
title: Задание версии Redis для кэша Azure для Redis (Предварительная версия)
description: Узнайте, как настроить версию Redis
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 09/30/2020
ms.openlocfilehash: d9f48de7ef5d9525a995af4ebbd12c5f14f40189
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2020
ms.locfileid: "93349143"
---
# <a name="set-redis-version-for-azure-cache-for-redis-preview"></a>Задание версии Redis для кэша Azure для Redis (Предварительная версия)
В этой статье вы узнаете, как настроить версию программного обеспечения Redis для использования с экземпляром кэша. Кэш Azure для Redis предлагает последнюю основную версию Redis и по крайней мере одну предыдущую версию. Эти версии будут регулярно обновляться по мере выпуска нового программного обеспечения Redis. Можно выбрать одну из двух доступных версий. Помните, что кэш будет обновлен до следующей версии автоматически, если используемая сейчас версия больше не поддерживается.

## <a name="prerequisites"></a>Предварительные требования
* Подписка Azure — [создайте бесплатную учетную запись](https://azure.microsoft.com/free/).

## <a name="create-a-cache"></a>Создание кэша
Чтобы создать кэш, выполните следующие действия.

1. Войдите на [портал Azure](https://portal.azure.com) и выберите **Создать ресурс**.
  
1. На странице **Создание** выберите **Базы данных** , а затем **Кэш Azure для Redis**.

    :::image type="content" source="media/cache-create/new-cache-menu.png" alt-text="Выбор элемента &quot;Кэш Azure для Redis&quot;.":::
   
1. На странице **Основные сведения** настройте параметры для нового кэша.
   
    | Параметр      | Рекомендуемое значение  | Описание |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Подписка** | Выберите свою подписку. | В этой подписке будет создан новый экземпляр кэша Redis для Azure. | 
    | **Группа ресурсов** | Выберите группу ресурсов или щелкните **создать** и введите новое имя группы ресурсов. | Имя группы ресурсов, в которой будут созданы кэш и другие ресурсы. Поместив все ресурсы приложения в одну группу ресурсов, вы сможете легко управлять ими и/или удалить их вместе. | 
    | **DNS-имя** | Введите глобально уникальное имя | Имя кэша должно быть строкой длиной от 1 до 63 символов и содержать только цифры, буквы и дефисы. Имя должно начинаться и заканчиваться цифрой или буквой и не может содержать более одного дефиса подряд. *Имя узла* для экземпляра кэша получит значение *\<DNS name>.redis.cache.windows.net*. | 
    | **Расположение** | Выберите расположение. | Выберите оптимальный [регион](https://azure.microsoft.com/regions/) для других служб, которые будут использовать кэш. |
    | **Тип кэша** | Выберите [уровень и размер кэша](https://azure.microsoft.com/pricing/details/cache/). |  Ценовая категория определяет размер, производительность и функции, доступные для кэша. Дополнительные сведения см. в [обзоре предложения "Кэш Redis для Azure"](cache-overview.md). |
   
1. На странице **Дополнительно** выберите используемую версию Redis.
   
    :::image type="content" source="media/cache-how-to-version/select-redis-version.png" alt-text="Версия Redis.":::

1. Нажмите кнопку **Создать**. 
   
    На создание кэша требуется некоторое время. Вы можете отслеживать ход выполнения на странице **обзорных сведений** кэша Azure для Redis. Когда **Состояние** примет значение **Running** (Выполняется), кэш будет готов к использованию.

    > [!NOTE]
    > В настоящее время версию Redis нельзя изменить после создания кэша.
    >

## <a name="next-steps"></a>Next Steps
Дополнительные сведения о кэше Azure для функций Redis.

> [!div class="nextstepaction"]
> [Кэш Azure для уровней службы Redis Premium](cache-overview.md#service-tiers)
