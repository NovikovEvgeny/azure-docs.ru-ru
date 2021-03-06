---
title: Сведения о документе — Custom Translator
titleSuffix: Azure Cognitive Services
description: На странице списка документов отображаются первые 10 документов в рабочей области. Для каждого из документов отображается имя, тип связывания, тип, язык, метка времени передачи и адрес электронной почты пользователя, который отправил документ.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 08/17/2020
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 87b999069ef9088a731a4e972c5d548cac0b917c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88509584"
---
# <a name="view-document-details"></a>Просмотр сведений о документе

На странице списка документов отображаются первые 10 документов в рабочей области. Для каждого из документов отображается имя, тип связывания, тип, язык, метка времени передачи и адрес электронной почты пользователя, который отправил документ.

Щелкните отдельный документ, чтобы просмотреть страницу со сведениями о нем. На странице сведений о документе отображается список извлеченных из документа предложений.

- По умолчанию в раскрывающемся списке отображается значение "параллельно" на исходном и целевом языках, но вы можете переключиться на просмотр предложений на исходном или целевом языке.
- По умолчанию для каждой страницы отображаются 20 предложений. Вы можете использовать элемент разбиения на страницы для перехода между страницами.

![сведения о документе](media/how-to/how-to-view-document-details.png)

## <a name="delete-a-document"></a>Удаление документа

Пользователь должен быть владельцем рабочей области, чтобы удалить документ. Кроме того, если документ используется моделью в любой части процесса обучения или развертывания, документ удалить нельзя.

1. Перейдите на страницу документа.
2. Наведите указатель мыши на любую запись документа и щелкните значок корзины.

    ![Удаление документа](media/how-to/how-to-delete-document-1.png)

3. Подтвердите удаление.

    ![Подтверждение удаления](media/how-to/how-to-delete-document-confirm.png)

## <a name="next-steps"></a>Дальнейшие шаги

- Сведения об обучении модели см. в [этой статье](how-to-train-model.md).
