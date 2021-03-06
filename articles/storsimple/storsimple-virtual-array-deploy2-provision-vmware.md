---
title: Инициализация виртуального массива StorSimple в VMware
description: Это второе руководство из серии, посвященной развертыванию виртуального массива StorSimple, в котором описывается подготовка виртуального устройства в VMware.
author: alkohli
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9810a34021aa039354aad24f84aff373229c0190
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87021483"
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a>Развертывание виртуального массива StorSimple — подготовка в VMware
![Схема, на которой показаны шаги, необходимые для развертывания виртуального массива.Вторая часть второго шага помечается как готовая к работе с VMware и выделяется.](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Обзор

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

В руководстве описана подготовка и подключение виртуального массива StorSimple в главной системе под управлением VMware ESXi 5.0, 5.5, 6.0 или 6.5. Сведения, приведенные в этой статье, относятся к развертыванию виртуальных массивов StorSimple на портале Azure, а также в облаке Microsoft Azure для государственных организаций.

Чтобы подготовить виртуальное устройство и подключиться к нему, требуются права администратора. Подготовка и начальная настройка могут занять около 10 минут.

## <a name="provisioning-prerequisites"></a>Предварительные требования к подготовке
Ниже приведены предварительные требования для подготовки виртуального устройства в главной системе под управлением VMware ESXi 5.0, 5.5, 6.0 или 6.5.

### <a name="for-the-storsimple-device-manager-service"></a>Предварительные требования для службы диспетчера устройств StorSimple
Перед тем как начать, убедитесь в следующем.

* Выполнены все инструкции из статьи [Deploy StorSimple Virtual Array — Prepare the portal](storsimple-virtual-array-deploy1-portal-prep.md).
* Образ виртуального устройства для VMware скачан с портала Azure. Дополнительные сведения см. в разделе **Step 3: Download the virtual array image** (Шаг 3. Скачивание образа виртуального устройства) руководства по [подготовке портала к развертыванию виртуального массива StorSimple](storsimple-virtual-array-deploy1-portal-prep.md).

### <a name="for-the-storsimple-virtual-device"></a>Для виртуального устройства StorSimple
Перед развертыванием виртуального устройства нужно выполнить указанные ниже условия.

* У вас есть доступ к главной системе под управлением Hyper-V (2008 R2 или более поздней версии), которую можно использовать для подготовки устройства.
* ОС сервера виртуальных машин должна быть в состоянии выделить указанные ниже ресурсы для подготовки виртуального устройства к работе.

  * Не менее 4 ядер.
  * Не менее 8 ГБ ОЗУ. Если планируется настроить виртуальный массив как файловый сервер, то 8 ГБ ОЗУ обеспечат поддержку до 2 млн файлов. Для поддержки 2–4 млн файлов потребуется 16 ГБ ОЗУ.
  * Один сетевой интерфейс.
  * Виртуальный диск размером 500 ГБ для системных данных.

### <a name="for-the-network-in-datacenter"></a>Для сети в центре обработки данных
Перед тем как начать, убедитесь в следующем.

* Вы изучили сетевые требования к развертыванию виртуального устройства StorSimple и настроили сеть центра обработки данных в соответствии с требованиями. 

## <a name="step-by-step-provisioning"></a>Пошаговая подготовка
Чтобы подготовить виртуальное устройство и подключиться к нему, сделайте следующее:

1. Убедитесь, что главная система обладает достаточными ресурсами для соответствия минимальным требованиям к виртуальному устройству.
2. Подготовьте виртуальное устройство в низкоуровневой оболочке.
3. Запустите виртуальное устройство и получите IP-адрес.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Шаг 1. Обеспечение соответствия главной системы минимальным требованиям к виртуальному устройству
Для создания виртуального устройства необходимы такие компоненты:

* Доступ к главной системе под управлением VMware ESXi Server 5.0, 5.5, 6.0 или 6.5.
* Наличие в системе клиента VMware vSphere для управления узлом ESXi.

  * Не менее 4 ядер.
  * Не менее 8 ГБ ОЗУ. Если планируется настроить виртуальный массив как файловый сервер, то 8 ГБ ОЗУ обеспечат поддержку до 2 млн файлов. Для поддержки 2–4 млн файлов потребуется 16 ГБ ОЗУ.
  * Один сетевой интерфейс, подключенный к сети с маршрутизацией трафика в Интернет. Для оптимальной работы устройства минимальная пропускная способность интернет-канала должна составлять 5 Мбит/с.
  * Виртуальный диск данных размером 500 ГБ.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Шаг 2. Подготовка виртуального устройства в низкоуровневой оболочке
Для подготовки виртуального устройства в низкоуровневой оболочке выполните указанные ниже действия.

1. Скопируйте образ виртуального устройства в систему. Этот образ необходимо скачать с портала Azure.

   1. Убедитесь, что это последний скачанный вами файл образа. Если вы уже скачивали образ, скачайте его снова, чтобы убедиться, что у вас его последняя версия. Последний образ состоит из двух файлов (вместо одного).
   2. Запишите расположение, в которое был скопирован образ, так как он потребуется вам позднее.

2. Войдите на сервер ESXi, используя клиент vSphere. Чтобы создать виртуальную машину, требуются права администратора.

   ![Снимок экрана со страницей входа клиента vSphere. Поля IP-адрес, имя пользователя и пароль содержат значения, а кнопка входа выделяется.](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. В разделе Inventory (Инвентаризация) клиента vSphere на панели слева выберите сервер ESXi.

   ![Снимок экрана: Главная страница клиента vSphere. В разделе Inventory (Инвентаризация) сервер ESXi выделен.](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. Отправьте файл VMDK на сервер ESXi. Перейдите на вкладку **Конфигурация** на панели справа. В списке **Hardware** (Оборудование) выберите пункт **Storage** (Хранилище).

   ![Снимок экрана, на котором показана вкладка "Конфигурация" клиента vSphere. В разделе Hardware (оборудование) выделяется хранилище.](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. На панели справа в разделе **Datastores**(Хранилища данных) выберите хранилище данных, в которое необходимо отправить VMDK-файл. В хранилище данных должно быть достаточно свободного пространства для дисков операционной системы и данных.

   ![Снимок экрана, показывающий страницу хранилища клиента vSphere. Вкладка хранилища данных открыта и содержит список хранилищ данных. Выбрано одно хранилище данных.](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. Щелкните правой кнопкой мыши и выберите пункт **Browse Datastore** (Обзор хранилища данных).

   ![Снимок экрана, показывающий контекстное меню выбранного хранилища данных. Выбран элемент "Обзор хранилища данных".](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. Отобразится окно **Datastore Browser** (Браузер хранилища данных).

   ![Снимок экрана обозревателя хранилища данных. Папки в хранилище данных видимы.](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. На панели инструментов щелкните значок :::image type="icon" source="./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png":::, чтобы создать папку. Укажите имя папки и запишите его. Это имя будет использоваться позднее при создании виртуальной машины (рекомендуется). Нажмите кнопку **ОК**.

   ![Снимок экрана: браузер хранилища данных с выделенным значком "создать папку". В диалоговом окне имеется имя папки, в котором есть выделенная кнопка ОК.](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. На панели слева в окне **Datastore Browser** (Браузер хранилища данных) появится новая папка.

   ![Снимок экрана: браузер хранилища данных с новой папкой, видимой в иерархии папок.](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. Щелкните значок отправки :::image type="icon" source="./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png"::: и выберите **отправить файл**.

    ![Снимок экрана, показывающий контекстное меню значка отправки. Выбран элемент "отправить файл".](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. Найдите скачанные VMDK-файлы и укажите их. Вы увидите два файла. Выберите файл для передачи.

    ![Снимок экрана: диалоговое окно с папками и двумя файлами в версии м D КБ. Один из файлов выделен.](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. Нажмите кнопку **Открыть**. Начнется передача файла VMDK в указанное хранилище данных. Отправка файла может занять несколько минут.
13. По завершении передачи файл появится в созданной папке в хранилище данных.

    ![Снимок экрана обозревателя хранилища данных. Новая папка выделяется в иерархии папок, а отправленный файл отображается в этой папке.](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    Теперь в то же хранилище данных необходимо передать второй VMDK-файл.
14. Вернитесь в окно клиента vSphere. Выбрав сервер ESXi, щелкните правой кнопкой мыши и выберите пункт **New Virtual Machine**(Создать виртуальную машину).

    ![Снимок экрана контекстного меню сервера ESXi. Будет выбран новый элемент виртуальной машины.](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. Отобразится окно **Create New Virtual Machine** (Создание виртуальной машины). На странице **Configuration** (Конфигурация) выберите параметр **Custom** (Настраиваемая). Щелкните **Далее**.
    ![Снимок экрана: страница "Конфигурация" окна "Создание новой виртуальной машины". Параметр Custom выбран, а кнопка Далее выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. На странице **Name and Location** (Имя и расположение) укажите имя виртуальной машины. Имя должно соответствовать имени папки (рекомендуется), указанному ранее на шаге 8.

    ![Снимок экрана со страницей "имя и расположение" в окне "Создание новой виртуальной машины". Поле имя будет заполнено, а кнопка Далее будет выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. На странице **Storage** (Хранилище) выберите хранилище данных, которое будет использоваться для подготовки виртуальной машины.

    ![Снимок экрана: страница "хранилище" окна "Создание новой виртуальной машины". Выбрано хранилище данных, и кнопка Далее будет выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. На странице **Virtual Machine Version** (Версия виртуальной машины) выберите параметр **Virtual Machine Version: 8** (Версия виртуальной машины: 8).

    ![Снимок экрана со страницей версии виртуальной машины. Параметр виртуальной машины версии 8 выбран, а кнопка Далее выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. На странице **Guest Operating System** (Гостевая операционная система) задайте для параметра **Guest Operating System** (Гостевая операционная система) значение **Windows**. Для параметра **Version** (Версия) в раскрывающемся списке выберите значение **Microsoft Windows Server 2012 (64-bit)** (Microsoft Windows Server 2012 (64-разрядная версия)).

    ![Снимок экрана: страница операционной системы на виртуальной машине с выбранной системой Windows, версия, установленная на Microsoft Windows Server 2012 (64-бит), и выделена ниже.](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. На странице **CPUs** (Процессоры) настройте параметры **Number of virtual sockets** (Количество виртуальных сокетов) и **Number of cores per virtual socket** (Количество ядер на виртуальный сокет) таким образом, чтобы значение параметра **Total number of cores** (Общее количество ядер) было не меньше 4. Щелкните **Далее**.

    ![Снимок экрана: страница "ЦП" с одним виртуальным сокетом, четырьмя ядрами на виртуальный сокет и четырьмя общими ядрами. Кнопка Далее будет выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. На странице **Memory** (Память) укажите 8 ГБ (или больше) ОЗУ. Щелкните **Далее**.

    ![Снимок экрана страницы "память". Для размера памяти задается значение размером 8 ГБ.](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. На странице **Network** (Сеть) укажите количество сетевых интерфейсов. Минимальное требование — один сетевой интерфейс.

    ![Снимок экрана: страница "сеть". Число сетевых интерфейсов равно одному, а кнопка Далее выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. На странице **SCSI Controller** (SCSI-контроллер) примите значение по умолчанию **LSI Logic SAS controller** (SAS-контроллер с логикой LSI).

    ![Снимок экрана со страницей SCSI-контроллера. Выбран параметр L s Logic S I, и кнопка Далее будет выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. На странице **Select a Disk** (Выбор диска) выберите параметр **Use an existing virtual disk** (Использовать существующий виртуальный диск). Щелкните **Далее**.

    ![Снимок экрана: страница "Выбор диска" с выбранным параметром использовать существующий виртуальный диск и Выделенная кнопка Далее.](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. На странице **Select Existing Disk** (Выбор существующего диска) в разделе **Disk File Path** (Путь к файлу диска) нажмите кнопку **Browse** (Обзор). Откроется диалоговое окно **Browse Datastores** (Обзор хранилищ данных). Перейдите к расположению, в которое был передан файл VMDK. Теперь вы увидите в хранилище данных только один файл, так как два файла, которые вы передали, были объединены. Выберите файл и нажмите кнопку **ОК**. Щелкните **Далее**.

    ![Снимок экрана страницы "Выбор существующего диска". Кнопка Обзор выделена, а диалоговое окно содержит один файл и выделенную кнопку ОК.](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. На странице **Advanced Options** (Дополнительные параметры) примите параметры по умолчанию и нажмите кнопку **Next** (Далее).

    ![Снимок экрана со страницей "Дополнительные параметры". Кнопка Далее будет выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. На странице **Ready to Complete** (Все готово к выполнению) проверьте все параметры, связанные с новой виртуальной машиной. Установите флажок **Edit the virtual machine settings before completion**(Изменить параметры виртуальной машины перед выполнением). Нажмите кнопку **Продолжить**.

    ![Снимок экрана: страница "готово к завершению" с выделенной кнопкой "продолжить". Флажок Изменить параметры виртуальной машины перед завершением установлен.](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. На странице **Virtual Machines Properties** (Свойства виртуальной машины) на вкладке **Hardware** (Оборудование) найдите оборудование устройства. Выберите пункт **New Hard Disk**(Новый жесткий диск). Нажмите кнопку **Добавить**.

    ![Снимок экрана: вкладка "оборудование" на странице свойств виртуальных машин. Новый жесткий диск выбран в списке оборудования. Кнопка Добавить выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. Откроется окно **Add Hardware** (Установка оборудования). На странице **тип устройства** в разделе **выберите тип устройства, который вы хотите добавить**, выберите **жесткий диск**и нажмите кнопку **Далее**.

    ![Снимок экрана: страница "тип устройства" в окне "Добавление оборудования". Выбрано устройство жесткого диска, и кнопка Далее будет выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. На странице **Select a Disk** (Выбор диска) выберите параметр **Create a new virtual disk** (Создать новый виртуальный диск). Щелкните **Далее**.

    ![Снимок экрана со страницей выбора диска. Выбран параметр создать новый виртуальный диск, и кнопка Далее будет выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. На странице **Create a Disk** (Создание диска) измените значение параметра **Disk Size** (Размер диска) на 500 ГБ (или более). Минимальный размер диска — 500 ГБ, но можно подготовить диск большего размера. Обратите внимание, что после подготовки размер диска изменить нельзя. Дополнительные сведения о размере диска, подготове к работе, см. в разделе о размерах [документа рекомендации](storsimple-ova-best-practices.md). В разделе **Disk Provisioning** (Подготовка диска) выберите пункт **Thin Provision** (Тонкая подготовка). Щелкните **Далее**.

    ![Снимок экрана со страницей создания диска. Размер диска устанавливается равным 500 ГБ, выбирается параметр тонкой инициализации, а кнопка Далее выделяется.](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. На странице **Advanced Options** (Дополнительные параметры) примите параметры по умолчанию.

    ![Снимок экрана со страницей "Дополнительные параметры". Для узла виртуального устройства задано значение SCSI (0:0), а кнопка Далее выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. На странице **Ready to Complete** (Все готово к выполнению) проверьте параметры диска. Нажмите кнопку **Готово**.

    ![Снимок экрана со страницей готовности к завершению. Отображается сводка параметров диска, и кнопка Готово выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. Вернитесь на страницу свойств виртуальной машины. На виртуальную машину будет добавлен новый жесткий диск. Нажмите кнопку **Готово**.

    ![Снимок экрана со страницей свойств виртуальной машины. Список оборудования содержит новый жесткий диск, и кнопка Готово выделена.](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. Выбрав виртуальную машину в правой области, перейдите на вкладку **Сводка** . Проверьте параметры виртуальной машины.

    ![Снимок экрана: вкладка "vSphere Client Summary" (сводка клиента). Новая виртуальная машина выделена, ее ресурсы и общие свойства видимы.](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

Теперь виртуальная машина подготовлена. Следующий шаг — запуск машины и получение IP-адреса.

> [!NOTE]
> Корпорация Майкрософт не рекомендует устанавливать средства VMware в виртуальный массива (как в подготовке выше). Установка средств VMware создаст неподдерживаемую конфигурацию.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Шаг 3. Запуск виртуального устройства и получение IP-адреса
Чтобы запустить виртуальное устройство и подключиться к нему, сделайте следующее.

#### <a name="to-start-the-virtual-device"></a>Запуск виртуального устройства
1. Запустите виртуальное устройство. В диспетчере конфигураций vSphere на панели слева выберите устройство и щелкните правой кнопкой мыши, чтобы открыть контекстное меню. Выберите **Power** (Питание), а затем — **Power on** (Включить). Виртуальная машина включится. Сведения о состоянии можно просмотреть в нижней панели **Recent Tasks** (Последние задачи) клиента vSphere.

   ![Снимок экрана с контекстным меню устройства. Элемент управления питанием выбран. Отображается смежное меню с выбранным элементом включения питания.](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. Настройка может занять несколько минут. После запуска устройства перейдите на вкладку **Console (консоль** ). чтобы войти на устройство, нажмите клавиши CTRL + ALT + DELETE. Также можно установить курсор в окно консоли и нажать CTRL+ALT+INS. По умолчанию используются имя пользователя *StorSimpleAdmin* и пароль *Password1*.

   ![Снимок экрана: вкладка "консоль клиента vSphere". Поле пароля пусто.](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. В целях безопасности срок действия пароля администратора устройства истекает после первого входа в систему. Вам будет предложено изменить пароль.

   ![Снимок экрана: вкладка "консоль клиента vSphere". Текст на странице указывает, что пароль должен быть изменен.](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. Введите пароль длиной не менее 8 символов. Пароль должен соответствовать по крайней мере 3 из 4 следующих требований: наличие букв в верхнем и нижнем регистре, цифр и специальных символов. Повторно введите пароль для подтверждения. Вы получите уведомление об изменении пароля.

   ![Снимок экрана: вкладка "консоль клиента vSphere". Текст на странице указывает, что пароль был изменен.](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. После успешного изменения пароля может быть выполнен перезапуск виртуального устройства. Дождитесь завершения перезапуска. Возможно, отобразится консоль устройства Windows PowerShell с индикатором выполнения.

   ![Снимок экрана, показывающий окно консоли с индикатором выполнения. Текст в окне указывает, что первоначальная установка выполняется и запрашивает у пользователя ожидание.](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. Шаги 6–8 применяются только при загрузке в среде без DHCP. Если вы работаете в среде DHCP, пропустите эти шаги и перейдите к шагу 9. Если устройство загружено в среде без DHCP, отобразится следующий экран.

   ![Снимок экрана, показывающий окно консоли с текстом, описывающим устройство. Командная строка считывает "Controller" и отображается как готовая для ввода данных.](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   Теперь необходимо настроить сеть.
7. Используйте команду `Get-HcsIpAddress` для получения списка сетевых интерфейсов, включенных на виртуальном устройстве. Если на устройстве включен один сетевой интерфейс, по умолчанию ему назначается имя `Ethernet`.

   ![Снимок экрана, показывающий окно консоли с выходными данными команды Get-HcsIpAddress. В качестве имени устройства указывается "Ethernet".](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. Для настройки сети используйте командлет `Set-HcsIpAddress` . Пример приведен ниже.

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![Снимок экрана, показывающий окно консоли с выходными данными команды Get-Help Set-HcsIpAddress и правильным использованием команды Set-HcsIpAddress.](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. По завершении начальной установки и загрузки устройства отобразится текст его баннера. Для управления устройством запишите IP-адрес и URL-адрес, отображающиеся в тексте баннера. Этот IP-адрес используется для подключения к веб-интерфейсу виртуального устройства, а также для выполнения локальной установки и регистрации.

   ![Снимок экрана, показывающий окно консоли с текстом баннера устройства. Этот текст включает IP-адрес и URL-адрес устройства.](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. (Необязательно.) Выполните этот шаг только в том случае, если вы развертываете устройство в облаке для государственных организаций. Теперь на устройстве нужно включить режим FIPS (федеральный стандарт обработки информации США). Стандарт FIPS 140 определяет криптографические алгоритмы, утвержденные для использования в компьютерных системах федеральной власти США для защиты конфиденциальных данных.

    1. Чтобы включить режим FIPS, выполните следующий командлет.

        `Enable-HcsFIPSMode`
    2. Перезагрузите устройство после включения режима FIPS, чтобы криптографические проверки вступили в силу.

       > [!NOTE]
       > Можно включить или отключить режим FIPS на устройстве. Поочередное использование устройства в режиме FIPS и без него без перезагрузки не поддерживается.
       >
       >

Если устройство не соответствует минимальным требованиям к конфигурации, в тексте баннера отобразится сообщение об ошибке (см. ниже). Потребуется изменить конфигурацию устройства и привести его ресурсы в соответствие с минимальными требованиями. Затем можно выполнить перезапуск и установить подключение к устройству. Минимальные требования к конфигурации см. в разделе [Шаг 1. Обеспечение соответствия главной системы минимальным требованиям к виртуальному устройству](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![Снимок экрана, показывающий окно консоли с текстом баннера устройства. Этот текст содержит сообщение об ошибке, содержащее URL-адрес для устранения проблемы.](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

При возникновении любой другой ошибки во время начальной настройки с помощью локального веб-интерфейса см. сведения о соответствующих рабочих процессах.

* Выполните диагностические тесты для [устранения неполадок настройки пользовательского веб-интерфейса](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Создайте пакет журнала и просмотрите файлы журнала](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Дальнейшие действия
* [Deploy StorSimple Virtual Array - Set up as file server (Preview) (Развертывание виртуального массива StorSimple — настройка в качестве файлового сервера (предварительная версия))](storsimple-virtual-array-deploy3-fs-setup.md)
* [Deploy StorSimple Virtual Array – Set up your virtual device as an iSCSI server (preview) (Развертывание виртуального массива StorSimple — настройка в качестве сервера iSCSI (предварительная версия))](storsimple-virtual-array-deploy3-iscsi-setup.md)
