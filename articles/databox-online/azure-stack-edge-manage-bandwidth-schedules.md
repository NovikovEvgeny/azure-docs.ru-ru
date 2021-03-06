---
title: Azure Stack пограничных Pro Управление расписаниями пропускной способности | Документация Майкрософт
description: Описывает, как использовать портал Azure для управления расписаниями пропускной способности на Azure Stack пограничных Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/22/2019
ms.author: alkohli
ms.openlocfilehash: e73a02c93807072e30c8ce2a1a7feb30e9d3c8c6
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "91978974"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-stack-edge-pro"></a>Использование портал Azure для управления расписаниями пропускной способности на Azure Stack. Pro  

В этой статье описывается, как управлять пользователями на Azure Stack пограничным Pro. Расписания пропускной способности позволяют настраивать использование пропускной способности сети в разных графиках в разное время дня. Эти расписания можно применить к операциям передачи и загрузки с устройства в облако.

Вы можете добавлять, изменять или удалять расписания пропускной способности для Azure Stack пограничных Pro с помощью портал Azure.

Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Добавление расписания
> * изменение расписания;
> * Удаление расписания


## <a name="add-a-schedule"></a>Добавление расписания

Чтобы добавить пользователя, выполните на портале Azure приведенные ниже шаги.

1. В портал Azure для ресурса Azure Stackного периметра перейдите в раздел **пропускная способность**.
2. В правой области нажмите кнопку **+ Добавить расписание**.

    ![Выбор пропускной способности](media/azure-stack-edge-manage-bandwidth-schedules/add-schedule-1.png)

3. В разделе **Добавить расписание** выполните действия ниже. 

   1. Укажите **начальный день**, **день окончания**, **время начала**и **время окончания** расписания.
   2. Установите флажок " **весь день** ", если расписание должно выполняться весь день.
   3. **Пропускная способность** — это пропускная способность в мегабит в секунду (Мбит/с), используемая устройством в операциях, в которых участвует облако (отправка и загрузка). Укажите в этом поле число от 20 до 1 000 000 007.
   4. Установите флажок в поле **Неограниченная пропускная способность**, чтобы избежать регулирования передачи и скачивания данных.
   5. Выберите **Добавить**.

      ![Добавление расписания](media/azure-stack-edge-manage-bandwidth-schedules/add-schedule-2.png)

3. Расписание будет создано с указанными параметрами. Это расписание затем появится в списке расписаний пропускной способности на портале.

    ![Обновленный список расписаний пропускной способности](media/azure-stack-edge-manage-bandwidth-schedules/add-schedule-3.png)

## <a name="edit-schedule"></a>Изменение расписания

Выполните следующие действия, чтобы изменить расписание пропускной способности.

1. В портал Azure перейдите к ресурсу Azure Stackного периметра и перейдите к **пропускной способности**. 
2. В списке расписаний пропускной способности выберите расписание, которое требуется изменить.
    ![Выбор расписания пропускной способности](media/azure-stack-edge-manage-bandwidth-schedules/modify-schedule-1.png)

3. Внесите необходимые изменения и сохраните их.

    ![Изменение пользователя](media/azure-stack-edge-manage-bandwidth-schedules/modify-schedule-2.png)

4. После изменения расписания список расписаний обновится.

    ![Изменение пользователя 2](media/azure-stack-edge-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Удаление расписания

Выполните следующие действия, чтобы удалить расписание пропускной способности, связанное с устройством Azure Stack ребра Pro.

1. В портал Azure перейдите к ресурсу Azure Stackного периметра и перейдите к **пропускной способности**.  

2. В списке расписаний пропускной способности выберите расписание, которое требуется удалить. В окне **Изменить расписание** выберите **Удалить**. При появлении запроса на подтверждение нажмите кнопку **Да**.

   ![Удаление пользователя](media/azure-stack-edge-manage-bandwidth-schedules/delete-schedule-2.png)

3. После удаления расписания список расписаний обновится.


## <a name="next-steps"></a>Дальнейшие действия

- Сведения об управлении общими папками см. [здесь](azure-stack-edge-manage-shares.md).
