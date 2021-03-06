---
title: Основные понятия — управление доступом на основе ролей (RBAC)
description: Сведения о ключевых возможностях управления доступом на основе ролей для решения Azure VMware
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 4fbda24ec6a8c1d08570d7f64270a954eb3d8a35
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2020
ms.locfileid: "92440949"
---
# <a name="role-based-access-control-rbac-for-azure-vmware-solution"></a>Управление доступом на основе ролей (RBAC) для решения Azure VMware

В решении VMware для Azure vCenter имеет встроенного локального пользователя с именем cloudadmin и назначается встроенной роли CloudAdmin. Локальный пользователь cloudadmin используется для настройки пользователей в AD. Как правило, роль CloudAdmin создает рабочие нагрузки в частном облаке и управляет ими. В решении VMware для Azure роль CloudAdmin имеет привилегии vCenter, которые отличаются от других облачных решений VMware.     

> [!NOTE]
> Сейчас решение VMware для Azure не предлагает настраиваемые роли в vCenter или на портале решения VMware для Azure. 

В vCenter и ESXi локального развертывания администратор имеет доступ к administrator@vsphere.local учетной записи vCenter. У них также могут быть назначены дополнительные пользователи или группы Active Directory (AD). 

В развертывании решения Azure VMware администратор не имеет доступа к учетной записи администратора. Но они могут назначать пользователей и группы AD роли CloudAdmin в vCenter.  

Пользователь частного облака не имеет доступа к и не может настраивать определенные компоненты управления, поддерживаемые и управляемые корпорацией Майкрософт. Например, кластеры, узлы, хранилища данных и распределенные виртуальные коммутаторы.




## <a name="azure-vmware-solution-cloudadmin-role-on-vcenter"></a>Роль CloudAdmin решения Azure VMware в vCenter

Вы можете просмотреть привилегии, предоставленные роли CloudAdmin решения Azure VMware в частном облаке Azure для решения VMware.

1. Войдите в клиент vSphere SDDC и перейдите в **меню**  >  **Администрирование**.
1. В разделе **Управление доступом**выберите **роли**.
1. В списке ролей выберите **CloudAdmin** и щелкните **привилегии**. 

   :::image type="content" source="media/role-based-access-control-cloudadmin-privileges.png" alt-text="Просмотр привилегий роли CloudAdmin в клиенте vSphere":::

Роль CloudAdmin в решении VMware для Azure имеет следующие права доступа к vCenter. Подробное описание каждой привилегии см. в [документации по продукту VMware](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-ED56F3C4-77D0-49E3-88B6-B99B8B437B62.html) .

| Privilege | Описание |
| --------- | ----------- |
| **Оповещения** | Подтверждение оповещение<br />Создать оповещение<br />Отключить действие будильника<br />Изменить оповещение<br />Удалить оповещение<br />Установка состояния оповещения |
| **Разрешения** | Изменение разрешений |
| **Библиотека содержимого** | Добавить элемент библиотеки<br />Создание подписки для опубликованной библиотеки<br />Создание локальной библиотеки<br />Создать библиотеку с подпиской<br />Удалить элемент библиотеки<br />Удалить локальную библиотеку<br />Удалить библиотеку с подпиской<br />Удаление подписки на опубликованную библиотеку<br />Загрузка файлов<br />Вытеснение элементов библиотеки<br />Исключить библиотеку с подпиской<br />Импорт хранилища<br />Сведения о подписке на зонды<br />Публикация элемента библиотеки на подписчиках<br />Публикация библиотеки для подписчиков<br />Чтение хранилища<br />Синхронизировать элемент библиотеки<br />Синхронизировать библиотеку с подпиской<br />Введите самоанализ<br />Обновить параметры конфигурации<br />Файлы обновления<br />Обновить библиотеку<br />Обновить элемент библиотеки<br />Обновление локальной библиотеки<br />Обновление библиотеки с подпиской<br />Обновление подписки опубликованной библиотеки<br />Просмотр параметров конфигурации |
| **Криптографические операции** | Прямой доступ |
| **Datastore** | Выделить место<br />Обзор хранилища данных<br />Настройка хранилища данных<br />Файловые операции с низким уровнем<br />"Удалить файлы"<br />Обновление метаданных виртуальной машины |
| **Папка** | Создать папку<br />Удалить папку<br />Переместить папку<br />Переименовать папку |
| **Глобальный** | Отменить задачу<br />Глобальный тег<br />Здравоохранение<br />Событие журнала<br />Управление настраиваемыми атрибутами<br />Диспетчеры служб<br />Задать настраиваемый атрибут<br />Системный тег |
| **Узел** | Репликация vSphere<br />&#160;&#160;&#160;&#160;управление репликацией |
| **Разметка vSphere** | Назначение и отмена назначения тега vSphere<br />Создать тег vSphere<br />Создать категорию тегов vSphere<br />Удалить тег vSphere<br />Удалить категорию тегов vSphere<br />Изменить тег vSphere<br />Изменить категорию тега vSphere<br />Изменить поле Уседби для категории<br />Изменить поле Уседби для тега |
| **Network** | Assign network |
| **Ресурс** | Применить рекомендацию<br />Назначение ВАПП пулу ресурсов<br />Assign virtual machine to resource pool<br />Создание пула ресурсов<br />Миграция виртуальной машины с питанием<br />Миграция на платформе виртуальной машины<br />Изменение пула ресурсов<br />Переместить пул ресурсов<br />Запрос vMotion<br />Удаление пула ресурсов<br />Переименование пула ресурсов |
| **Запланированная задача** | Создание задачи<br />Изменить задачу<br />Удалить задачу<br />Запустить задачу |
| **Сеансы** | Сообщение<br />Проверить сеанс |
| **Профиль** | Представление хранилища, управляемого профилем |
| **Представление хранилища** | Представление |
| **вапп** | Добавить виртуальную машину<br />Назначение пула ресурсов<br />Назначение ВАПП<br />Клонировать<br />Создание<br />Удалить<br />Экспорт<br />Импорт<br />Переместить<br />Выключение питания<br />Включение<br />Переименовать<br />Приостановить<br />Unregister; <br />Просмотр среды OVF<br />Конфигурация приложения ВАПП<br />Конфигурация экземпляра ВАПП<br />Конфигурация managedBy ВАПП<br />Конфигурация ресурсов ВАПП |
| **Виртуальная машина** | Изменить конфигурацию<br />&#160;&#160;&#160;&#160;получить аренду диска<br />&#160;&#160;&#160;&#160;добавить существующий диск<br />&#160;&#160;&#160;&#160;добавить новый диск<br />&#160;&#160;&#160;&#160;добавить или удалить устройство<br />Расширенная конфигурация &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;изменить число ЦП<br />&#160;&#160;&#160;&#160;изменить память<br />&#160;&#160;&#160;&#160;изменить параметры<br />&#160;&#160;&#160;&#160;изменить размещение файл подкачки<br />Ресурс изменения &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;Настройка USB-устройства узла<br />&#160;&#160;&#160;&#160;настроить необработанное устройство<br />&#160;&#160;&#160;&#160;настроить managedBy<br />&#160;&#160;&#160;&#160;отобразить параметры подключения<br />Расширение виртуального диска &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;изменить параметры устройства<br />&#160;&#160;&#160;&#160;совместимость с отказоустойчивостью запросов<br />&#160;&#160;&#160;&#160;запрос несобственных файлов<br />&#160;&#160;&#160;&#160;перезагрузить из путей<br />&#160;&#160;&#160;&#160;удалить диск<br />Переименование &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;сбросить сведения о гостевой системе<br />Заметка &#160;&#160;&#160;&#160;набора<br />&#160;&#160;&#160;&#160;переключить отслеживание изменений на диске<br />&#160;&#160;&#160;&#160;переключить родительскую вилку<br />&#160;&#160;&#160;&#160;обновление совместимости виртуальной машины<br />Изменить инвентаризацию<br />&#160;&#160;&#160;&#160;создать из существующего<br />&#160;&#160;&#160;&#160;создать новый<br />Перемещение &#160;&#160;&#160;&#160;<br />Регистрация &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;удалить<br />Отмена регистрации &#160;&#160;&#160;&#160;<br />Гостевые операции<br />Изменение псевдонима гостевой операции &#160;&#160;&#160;&#160;<br />Запрос псевдонима гостевой операции &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;изменения гостевой операции<br />Выполнение программы гостевой операции &#160;&#160;&#160;&#160;<br />Запросы &#160;&#160;&#160;&#160;гостевой операции<br />Тип взаимодействия<br />&#160;&#160;&#160;&#160;ответ на вопрос<br />&#160;&#160;&#160;&#160;операцию резервного копирования на виртуальной машине<br />&#160;&#160;&#160;&#160;Настройка компакт-носителя<br />&#160;&#160;&#160;&#160;настроить гибкий носитель<br />&#160;&#160;&#160;&#160;подключение устройств<br />&#160;&#160;&#160;&#160;взаимодействие с консолью<br />&#160;&#160;&#160;&#160;создать снимок экрана<br />&#160;&#160;&#160;&#160;дефрагментации всех дисков<br />&#160;&#160;&#160;&#160;перетаскивания<br />Управление операционной системой &#160;&#160;&#160;&#160;на виртуальной машине с помощью API Викс<br />&#160;&#160;&#160;&#160;внедрять коды сканирования USB HID<br />&#160;&#160;&#160;&#160;Установка средств VMware<br />&#160;&#160;&#160;&#160;приостановить или отостановить<br />&#160;&#160;&#160;&#160;выполнять операции очистки или сжатия<br />&#160;&#160;&#160;&#160;выключение<br />&#160;&#160;&#160;&#160;включить<br />Сеанс записи &#160;&#160;&#160;&#160;на виртуальной машине<br />&#160;&#160;&#160;&#160;сеанса воспроизведения на виртуальной машине<br />Приостановка &#160;&#160;&#160;&#160;<br />Отказоустойчивость &#160;&#160;&#160;&#160;приостановки<br />&#160;&#160;&#160;&#160;тестовой отработки отказа<br />Дополнительная виртуальная машина для перезапуска &#160;&#160;&#160;&#160;тестирования<br />&#160;&#160;&#160;&#160;отключить отказоустойчивость<br />&#160;&#160;&#160;&#160;включения отказоустойчивости<br />Подготовка<br />&#160;&#160;&#160;&#160;разрешить доступ к диску<br />&#160;&#160;&#160;&#160;разрешить доступ к файлам<br />&#160;&#160;&#160;&#160;разрешить доступ к диску только для чтения<br />&#160;&#160;&#160;&#160;разрешить скачивание виртуальной машины<br />Шаблон клонирования &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;клонировать виртуальную машину<br />&#160;&#160;&#160;&#160;создать шаблон на основе виртуальной машины<br />&#160;&#160;&#160;&#160;настроить гостя<br />Шаблон развертывания &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;пометить как шаблон<br />&#160;&#160;&#160;&#160;изменить спецификацию настройки<br />&#160;&#160;&#160;&#160;повышения дисков<br />Спецификации &#160;&#160;&#160;&#160;чтение спецификаций настройки<br />Конфигурация службы<br />&#160;&#160;&#160;&#160;разрешить уведомления<br />&#160;&#160;&#160;&#160;Разрешить опрос глобальных уведомлений о событиях<br />&#160;&#160;&#160;&#160;управление конфигурацией службы<br />&#160;&#160;&#160;&#160;изменить конфигурацию службы<br />&#160;&#160;&#160;&#160;конфигурации службы запросов<br />&#160;&#160;&#160;&#160;чтение конфигурации службы<br />Управление моментальными снимками<br />Создание моментального снимка &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;удалить моментальный снимок<br />&#160;&#160;&#160;&#160;переименовать моментальный снимок<br />&#160;&#160;&#160;&#160;восстановить моментальный снимок<br />Репликация vSphere<br />&#160;&#160;&#160;&#160;настроить репликацию<br />&#160;&#160;&#160;&#160;управление репликацией<br />Репликация &#160;&#160;&#160;&#160;монитора |
| **всервице** | Создать зависимость<br />Удалить зависимость<br />Перенастройка конфигурации зависимостей<br />Обновить зависимость |



## <a name="next-steps"></a>Дальнейшие действия

Подробное описание каждой привилегии см. в [документации по продукту VMware](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-ED56F3C4-77D0-49E3-88B6-B99B8B437B62.html) .

<!-- LINKS - internal -->

<!-- LINKS - external-->
[VMware product documentation]: https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-ED56F3C4-77D0-49E3-88B6-B99B8B437B62.html

