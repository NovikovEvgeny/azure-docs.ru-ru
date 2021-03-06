---
title: Резервное копирование и восстановление — портал Azure — база данных Azure для PostgreSQL — один сервер
description: В этой статье описывается, как восстановить сервер в базе данных Azure для PostgreSQL-Single Server с помощью портал Azure.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 6/30/2020
ms.openlocfilehash: debdbf6e08af7b9005336231abd6c998a871c525
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91708090"
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql---single-server-using-the-azure-portal"></a>Как выполнить резервное копирование и восстановление сервера в базе данных Azure для PostgreSQL — один сервер с помощью портал Azure

## <a name="backup-happens-automatically"></a>Резервное копирование выполняется автоматически
Чтобы обеспечить возможность восстановления, для серверов службы "База данных Azure для PostgreSQL" периодически выполняется резервное копирование. С помощью этой функции можно восстановить сервер и все его базы данных до более ранней точки во времени на новом сервере.

## <a name="set-backup-configuration"></a>Настройка конфигурации резервного копирования

При создании сервер можно настроить для локально избыточного или геоизбыточного резервного копирования. Для это воспользуйтесь окном **Ценовая категория**.

> [!NOTE]
> После создания сервера тип избыточности (локальная или географическая) изменить нельзя.
>

Если вы создаете сервер на портале Azure, тип резервного копирования (**локально избыточное** или **геоизбыточное**) задается в окне **Ценовая категория**. Кроме того, в этом окне указывается **срок хранения резервных копий** — период (в днях), в течение которого должны храниться резервные копии.

   :::image type="content" source="./media/howto-restore-server-portal/pricing-tier.png" alt-text="Окно &quot;Ценовая категория&quot; — выбор типа избыточности для резервного копирования":::

Дополнительные сведения о настройке этих значений при создании см. в [кратком руководстве по серверам службы "База данных Azure для PostgreSQL"](quickstart-create-server-database-portal.md).

Вот как изменить срок хранения резервной копии сервера:
1. Войдите на [портал Azure](https://portal.azure.com/).
2. Выберите сервер базы данных Azure для PostgreSQL. Откроется страница **Обзор**.
3. В меню в разделе **Параметры** выберите **Ценовая категория**. С помощью ползунка можно изменить **срок хранения резервных копий** в диапазоне от 7 до 35 дней.
На приведенном ниже снимке экрана он увеличен до 34 дней.
:::image type="content" source="./media/howto-restore-server-portal/3-increase-backup-days.png" alt-text="Окно &quot;Ценовая категория&quot; — выбор типа избыточности для резервного копирования":::

4. Нажмите кнопку **ОК**, чтобы подтвердить изменение.

Срок хранения резервных копий определяет, насколько раннюю точку во времени можно задать для восстановления, так как это зависит от доступных резервных копий. Восстановление до точки во времени описано в следующем разделе. 

## <a name="point-in-time-restore"></a>Восстановление на момент времени
Служба "База данных Azure для PostgreSQL" позволяет восстановить сервер до точки во времени и создать его новую копию. Можно использовать этот новый сервер для восстановления данных или направить на него клиентские приложения.

Например, если таблица была случайно удалена сегодня в полдень, ее можно восстановить до момента сразу перед полуднем и получить отсутствующие таблицы и данные из новой копии сервера. Восстановление до точки во времени выполняется на уровне сервера, а не на уровне базы данных.

Чтобы восстановить сервер до точки во времени, сделайте следующее:
1. На портале Azure выберите нужный сервер службы "База данных Azure для PostgreSQL". 

2. На панели инструментов на странице **Обзор** для сервера выберите **Восстановить**.

   :::image type="content" source="./media/howto-restore-server-portal/2-server.png" alt-text="Окно &quot;Ценовая категория&quot; — выбор типа избыточности для резервного копирования":::

3. Заполните форму "Восстановление", указав следующие сведения.

   :::image type="content" source="./media/howto-restore-server-portal/3-restore.png" alt-text="Окно &quot;Ценовая категория&quot; — выбор типа избыточности для резервного копирования":::
   - **Точка восстановления.** Выберите точку во времени, до которой нужно восстановить сервер.
   - **Целевой сервер.** Укажите имя для нового сервера.
   - **Расположение.** Невозможно выбрать регион. По умолчанию он совпадает с исходным сервером.
   - **Ценовая категория.** При восстановлении до точки во времени эти параметры изменить нельзя. Она совпадает с ценовой категорией исходного сервера. 

4. Чтобы восстановить сервер до выбранной точки во времени, нажмите кнопку **OК**. 

5. После завершения восстановления найдите созданный сервер, чтобы убедиться, что данные были восстановлены, как и ожидалось.

Имя и пароль администратора сервера для входа на сервер, созданный путем восстановления до точки во времени, будут теми же, что и для существующего сервера в выбранной точке во времени. Пароль можно изменить на странице **Обзор** для нового сервера.

На новом сервере, созданном во время восстановления, нет правил брандмауэра или конечных точек службы виртуальной сети, которые настроены на сервере-источнике. Для этого нового сервера правила нужно настроить отдельно.

## <a name="geo-restore"></a>Геовосстановление

Если вы настроили сервер для геоизбыточного резервного копирования, из резервной копии можно создать новый сервер. Его можно создать в любом регионе, где доступна службы "База данных Azure для PostgreSQL".  

1. Нажмите кнопку **Создать ресурс** (+) в левом верхнем углу окна портала. Выберите **Базы данных** > **База данных Azure для PostgreSQL**.

   :::image type="content" source="./media/howto-restore-server-portal/1-navigate-to-postgres.png" alt-text="Окно &quot;Ценовая категория&quot; — выбор типа избыточности для резервного копирования":::

2. Выберите вариант развертывания **Отдельный сервер**.

   :::image type="content" source="./media/howto-restore-server-portal/2-select-deployment-option.png" alt-text="Окно &quot;Ценовая категория&quot; — выбор типа избыточности для резервного копирования":::
 
3. Укажите подписку, группу ресурсов и имя нового сервера. 

4. Выберите **резервное копирование** в качестве **источника данных**. Это действие загружает раскрывающееся меню, содержащее список серверов с включенными географически избыточными резервными копиями.
   
   :::image type="content" source="./media/howto-restore-server-portal/4-geo-restore.png" alt-text="Окно &quot;Ценовая категория&quot; — выбор типа избыточности для резервного копирования":::
    
   > [!NOTE]
   > Сразу после создания сервер может быть еще не готов к географическому восстановлению. Заполнение метаданных может занять несколько часов.
   >

5. Выберите раскрывающийся список **резервная копия** .
   
   :::image type="content" source="./media/howto-restore-server-portal/5-geo-restore-backup.png" alt-text="Окно &quot;Ценовая категория&quot; — выбор типа избыточности для резервного копирования":::

6. Выберите исходный сервер для восстановления.
   
   :::image type="content" source="./media/howto-restore-server-portal/6-select-backup.png" alt-text="Окно &quot;Ценовая категория&quot; — выбор типа избыточности для резервного копирования":::

7. По умолчанию сервер будет иметь значения для параметра количество **виртуальных ядер**, **срок хранения резервной**копии, **параметр избыточности резервной копии**, **Версия подсистемы**и **учетные данные администратора**. Выберите **Continue** (Продолжить). 
   
   :::image type="content" source="./media/howto-restore-server-portal/7-accept-backup.png" alt-text="Окно &quot;Ценовая категория&quot; — выбор типа избыточности для резервного копирования" или "С оптимизацией для операций в памяти") и объем **хранилища** во время восстановления нельзя.

   :::image type="content" source="./media/howto-restore-server-portal/8-create.png" alt-text="Окно &quot;Ценовая категория&quot; — выбор типа избыточности для резервного копирования"::: 

9. Выберите **Просмотр и создание** , чтобы просмотреть выбранные элементы. 

10. Щелкните **Создать**, чтобы подготовить сервер. Это может занять несколько минут.

Имя и пароль администратора сервера для входа на сервер, созданный путем геовосстановления, будут теми же, что и для существующего сервера в момент, когда было инициировано восстановление. Пароль можно изменить на странице **Обзор** для нового сервера.

На новом сервере, созданном во время восстановления, нет правил брандмауэра или конечных точек службы виртуальной сети, которые настроены на сервере-источнике. Для этого нового сервера правила нужно настроить отдельно.


## <a name="next-steps"></a>Дальнейшие шаги
- Подробнее о [резервном копировании](concepts-backup.md) в службе.
- Подробнее о вариантах обеспечения [непрерывности бизнеса](concepts-business-continuity.md).