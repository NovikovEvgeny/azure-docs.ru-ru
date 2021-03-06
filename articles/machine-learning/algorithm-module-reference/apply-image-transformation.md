---
title: Применение преобразования изображений
titleSuffix: Azure Machine Learning
description: Узнайте, как использовать модуль преобразования «применение изображений» для применения преобразования изображений к каталогу изображений.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: a64d5cebfd8e70e2f54a66193a7041c47887c54a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90898913"
---
# <a name="apply-image-transformation"></a>Применение преобразования изображений 

В этой статье описывается, как использовать модуль применения преобразования изображений в Машинное обучение Azure Designer для изменения каталога входных изображений, основанного на ранее заданном преобразовании изображений.  

Необходимо подключить модуль [преобразования «инициализация изображений](init-image-transformation.md) », чтобы указать преобразование, а затем применить такое преобразование к входному каталогу изображений модуля Apply Image преобразование.

## <a name="how-to-use-apply-image-transformation"></a>Использование преобразования «применение изображения»  

1. Добавьте модуль **Apply Image преобразование** в конвейер. Этот модуль можно найти в категории *преобразования данных компьютерное зрение/Image* . 

2. Соедините выход **преобразования «Инициализация изображения** » с левым входом **преобразования «применить изображение**».

     > [!NOTE]
     > Этот модуль принимает только преобразование "изображение", созданное модулем [преобразования "Инициализация изображений](init-image-transformation.md) ". Для преобразования другого типа необходимо подключить его для [применения преобразования](apply-transformation.md), в противном случае будет выдано исключение "инвалидтрансформатиондиректореррор".


3. Подключите Каталог образов, который требуется преобразовать.

4. В качестве **режима**укажите, для какого назначения используется входное преобразование: "для обучения" или "для вывода". 

   Если выбрать **для обучения**, будут применены все преобразования, указанные в преобразовании «инициализация изображений».

   Если выбрать **для вывода**, то преобразование, например создание новых примеров, случайным образом будет исключено перед применением. Это обусловлено тем, что операции преобразования для создания новых образцов случайным образом, например "случайная горизонтальная перелистывание", используются для расширения данных в процессе обучения, которые должны быть удалены при выводе, поскольку для точного прогнозирования и оценки необходимо исправить примеры.

   > [!NOTE]
   > Преобразования, которые будут исключены в режиме **для вывода** ,: Обрезка случайного изменения размера, случайная обрезка, случайное горизонтальное перелистывание, случайное вертикальное перелистывание, случайное вращение, случайное аффинное, случайное градации серого, случайная перспектива, случайная очистка.

5. Чтобы применить преобразование изображения к новому каталогу образа, отправьте конвейер.  

### <a name="module-parameters"></a>Параметры модуля

| Имя | Диапазон | Тип | По умолчанию                   | Описание                              |
| ---- | ----- | ---- | ------------------------- | ---------------------------------------- |
| Режим | Любой   | Режим | (Требуется указать пользователя) | Для целей использования преобразования input. Не следует исключать операции преобразования случайных чисел в выводе, не сохраняя их в обучении. |

### <a name="expected-inputs"></a>Ожидаемые входные данные  

| Имя                       | Тип                    | Описание                       |
| -------------------------- | ----------------------- | --------------------------------- |
| Преобразование "Входное изображение" | трансформатиондиректори | Преобразование "Входное изображение"        |
| Каталог входных изображений      | имажедиректори          | Каталог изображений для преобразования |

### <a name="outputs"></a>Выходные данные  

| Имя                   | Тип           | Описание            |
| ---------------------- | -------------- | ---------------------- |
| Выходной каталог образа | имажедиректори | Выходной каталог образа |

## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с [набором доступных модулей](module-reference.md) в службе Машинного обучения Azure. 
