---
title: Краткое руководство по Azure. Создание концентратора событий с помощью портала Azure
description: В этом кратком руководстве показано, как создать концентратор событий с помощью портала Azure и как затем отправлять и получать данные используя .NET Standard.
ms.topic: quickstart
ms.date: 06/23/2020
ms.openlocfilehash: 84cafcc86142cb9b97639c023971e7d290fc79fc
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "88927890"
---
# <a name="quickstart-create-an-event-hub-using-azure-portal"></a>Краткое руководство. Создание концентратора событий с помощью портала Azure
Центры событий Azure — это платформа потоковой передачи больших данных и служба приема событий, принимающая и обрабатывающая миллионы событий в секунду. Центры событий могут обрабатывать и сохранять события, данные и телеметрию, созданные распределенным программным обеспечением и устройствами. Данные, отправляемые в концентратор событий, можно преобразовывать и сохранять с помощью любого поставщика аналитики в реальном времени, а также с помощью адаптеров пакетной обработки или хранения. Подробный обзор Центров событий см. в статьях [Что такое Центры событий Azure?](event-hubs-about.md) и [Обзор функций Центров событий](event-hubs-features.md).

В этом кратком руководстве вы создадите концентратор событий на [портале Azure](https://portal.azure.com).

## <a name="prerequisites"></a>Предварительные требования

В рамках этого краткого руководства вам потребуются:

- Подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/), прежде чем начать работу.

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Группа ресурсов — это логическая коллекция ресурсов Azure. Все ресурсы развертываются и управляются в группе ресурсов. Чтобы создать группу ресурсов:

1. Войдите на [портал Azure](https://portal.azure.com).
1. В области навигации слева щелкните **Группа ресурсов**. Нажмите кнопку **Добавить**.

   ![Кнопка добавления группы ресурсов](./media/event-hubs-quickstart-portal/resource-groups1.png)

1. В поле **Подписка** выберите имя подписки Azure, в которой необходимо создать группу ресурсов.
1. Введите уникальное **имя группы ресурсов**. Система мгновенно проверит, доступно ли имя в текущей выбранной подписке Azure.
1. Выберите **регион** для группы ресурсов.
1. Выберите **Review + Create** (Просмотреть и создать).

   ![Создание группы ресурсов](./media/event-hubs-quickstart-portal/resource-groups2.png)
1. На странице **Отзыв и создание** выберите **Создать**. 

## <a name="create-an-event-hubs-namespace"></a>Создание пространства имен в Центрах событий

Пространство имен Центров событий предоставляет уникальный контейнер, ограничивающий область действия. Вы можете обращаться к этому контейнеру по полному доменному имени и создавать в нем концентраторы событий. Чтобы создать пространство имен в группе ресурсов с использованием портала, выполните следующие действия:

1. На портале Azure щелкните **Создать ресурс** в левой верхней части экрана.
1. Выберите **Все службы** в меню слева и щелкните **звездочку (`*`)** рядом с пунктом **Центры событий** в категории **Аналитика**. Убедитесь, что **Центры событий** были добавлены в раздел **Избранное** в меню навигации слева. 
    
   ![Поиск Центров событий](./media/event-hubs-quickstart-portal/select-event-hubs-menu.png)
1. Выберите **Центры событий** в разделе **Избранное** в меню навигации слева и щелкните **Добавить** на панели инструментов.

   ![Кнопка "Добавить"](./media/event-hubs-quickstart-portal/event-hubs-add-toolbar.png)
1. На странице **Создание пространства имен** выполните следующие действия.  
   1. Выберите **подписку**, в которой нужно создать пространство имен.  
   1. Выберите **группу ресурсов**, созданную на предыдущем шаге.   
   1. Введите **имя** для пространства имен. Система немедленно проверяет, доступно ли оно.  
   1. Выберите **расположение** для пространства имен.      
   1. Выберите **ценовую категорию** ("Базовый" или "Стандартный").    
   1. Не изменяйте параметр **единиц пропускной способности**. Дополнительные сведения о единицах пропускной способности см. в статье [Масштабирование с помощью концентраторов событий](event-hubs-scalability.md#throughput-units).  
   1. В нижней части страницы выберите **Просмотреть и создать**.
      
      ![Создание пространства имен концентратора событий](./media/event-hubs-quickstart-portal/create-event-hub1.png)
   1. На странице **Просмотр и создание** проверьте параметры и нажмите кнопку **Создать**. Дождитесь завершения развертывания. 
      
      ![Страница "Просмотр и создание"](./media/event-hubs-quickstart-portal/review-create.png)
      
   1. На странице **Развертывание** нажмите **Перейти к ресурсу**, чтобы открыть страницу пространства имен. 
      
      ![Развертывание завершено — перейдите к ресурсу](./media/event-hubs-quickstart-portal/deployment-complete.png)  
   1. Убедитесь, что отображаемая страница **Пространство имен Центров событий** имеет похожий на следующий вид:   
      
      ![Домашняя страница для пространства имен](./media/event-hubs-quickstart-portal/namespace-home-page.png)       

      > [!NOTE]
      > Центры событий Azure предоставляют конечную точку Kafka. Эта конечная точка позволяет вашему пространству имен Центров событий распознавать протокол [Apache Kafka](https://kafka.apache.org/intro) и API в собственном коде. Эта возможность позволяет подключиться к концентраторам событий, как к темам Kafka, не изменяя клиентов протокола или запустив собственные кластеры. Центры событий поддерживают [Apache Kafka 1.0.](https://kafka.apache.org/10/documentation.html) и более поздние версии. Дополнительные сведения см. в статье [Использование Центров событий из приложений Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md).
    
## <a name="create-an-event-hub"></a>Создание концентратора событий

Чтобы создать концентратор событий в пространстве имен, выполните следующие действия:

1. На странице "Пространство имен Центров событий" выберите **Центры событий** в меню слева.
1. Щелкните **+Концентратор событий** в верхней части окна.
   
    ![Кнопка добавления концентратора событий](./media/event-hubs-quickstart-portal/create-event-hub4.png)
1. Введите имя концентратора событий, а затем щелкните **Создать**.
   
    ![Создание концентратора событий](./media/event-hubs-quickstart-portal/create-event-hub5.png)
1. Вы можете проверить состояние создания концентратора событий в оповещениях. После создания концентратора событий вы увидите его в соответствующем списке, как показано на следующем рисунке.

    ![Концентратор событий создан](./media/event-hubs-quickstart-portal/event-hub-created.png)

## <a name="next-steps"></a>Дальнейшие действия

В этой статье вы создали группу ресурсов, пространство имен Центров событий и концентратор событий. Пошаговые инструкции по отправке событий в концентратор и получении событий из него см. в следующих руководствах по **отправке и получению событий**: 

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [JavaScript](event-hubs-node-get-started-send.md)
- [GO](event-hubs-go-get-started-send.md)
- [C (только отправка)](event-hubs-c-getstarted-send.md)
- [Apache Storm (только получение)](event-hubs-storm-getstarted-receive.md)


[Azure portal]: https://portal.azure.com/
[3]: ./media/event-hubs-quickstart-portal/sender1.png
[4]: ./media/event-hubs-quickstart-portal/receiver1.png
