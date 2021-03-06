---
title: Резервное копирование базы данных SAP HANA в Azure с помощью Azure Backup
description: Из этой статьи вы узнаете, как создать резервную копию базы данных SAP HANA на виртуальных машинах Azure с помощью службы Azure Backup.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: a0a03a0d126845b1beba6d247f82950b0a9a35ab
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92172991"
---
# <a name="back-up-sap-hana-databases-in-azure-vms"></a>Резервное копирование баз данных SAP HANA на виртуальных машинах Azure

Базы данных SAP HANA являются критическими рабочими нагрузками, требующими низкой целевой точки восстановления (RPO) и долгосрочного хранения. Вы можете выполнять резервное копирование баз данных SAP HANA, работающих на виртуальных машинах Azure, с помощью [Azure Backup](backup-overview.md).

В этой статье показано, как выполнять резервное копирование баз данных SAP HANA, запущенных на виртуальных машинах Azure, в хранилище Служб восстановления для Azure Backup.

В этой статье вы узнаете, как выполнять следующие задачи.
> [!div class="checklist"]
>
> * Создание и настройка хранилища.
> * Обнаружение баз данных.
> * Настройка резервного копирования.
> * Выполнение задания резервного копирования по запросу

>[!NOTE]
>Резервное копирование SAP HANA для RHEL (7.4, 7.6, 7.7 и 8.1) стало общедоступно 1 августа 2020 г.

>[!NOTE]
>**Обратимое удаление для SQL Server на виртуальной машине Azure и обратимое удаление для SAP HANA в рабочих нагрузках виртуальных машин Azure** теперь доступно в предварительной версии.<br>
>Чтобы зарегистрироваться для просмотра, напишите нам по адресу [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com) .

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить базу данных для резервного копирования, см. [необходимые условия](tutorial-backup-sap-hana-db.md#prerequisites) и раздел [Функции скрипта предварительной регистрации](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does).

### <a name="establish-network-connectivity"></a>Установка сетевого подключения

Для всех операций базе данных SAP HANA на виртуальной машине Azure требуется подключение к службе Azure Backup, службе хранилища Azure и Azure Active Directory. Для этого можно использовать частные конечные точки или разрешить доступ к необходимым общедоступным IP-адресам или полным доменным именам. Неправильное подключение к необходимым службам Azure может привести к сбою в таких операциях, как обнаружение базы данных, настройка резервного копирования, выполнение резервного копирования и восстановление данных.

В следующей таблице перечислены различные альтернативы, которые можно использовать для подключения:

| **Параметр**                        | **Преимущества**                                               | **Недостатки**                                            |
| --------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Частные конечные точки                 | Разрешение резервного копирования через частные IP-адреса в виртуальной сети  <br><br>   Обеспечение детального контроля над сетью и хранилищем | Стандартные [затраты](https://azure.microsoft.com/pricing/details/private-link/) на частную конечную точку |
| Теги службы NSG                  | Упрощенное управление благодаря автоматическому объединению изменений диапазона   <br><br>   Нет дополнительных затрат | Может использоваться только с NSG  <br><br>    Предоставляет доступ ко всей службе |
| Теги полного доменного имени службы "Брандмауэр Azure"          | Проще управлять, так как требуемые FQDN управляются автоматически | Можно использовать только с брандмауэром Azure                         |
| Разрешение доступа к FQDN/IP-адресам службы | Нет дополнительных затрат   <br><br>  Работает со всеми устройствами безопасности сети и брандмауэрами | Может потребоваться доступ к широкому набору IP-адресов или FQDN   |
| Использование прокси-сервера HTTP                 | Единая точка доступа к виртуальным машинам через Интернет                       | Дополнительные затраты для запуска виртуальной машины с программным обеспечением прокси-сервера         |

Ниже приведены дополнительные сведения об использовании этих параметров:

#### <a name="private-endpoints"></a>Частные конечные точки

Частные конечные точки можно использовать для безопасного подключения с серверов внутри виртуальной сети к хранилищу Служб восстановления. Частная конечная точка использует IP-адрес из адресного пространства виртуальной сети для вашего хранилища. Сетевой трафик между ресурсами в виртуальной сети и хранилищем проходит по виртуальной сети и частный канал в магистральную сеть Майкрософт. Это устраняет уязвимость в общедоступном Интернете. Дополнительные сведения о частных конечных точках для Azure Backup можно узнать [здесь](./private-endpoints.md).

#### <a name="nsg-tags"></a>Теги NSG

Если вы используете группы безопасности сети (NSG), используйте тег службы *AzureBackup*, чтобы разрешить исходящий доступ к Azure Backup. Кроме тегов Azure Backup, вам нужно разрешить подключение для проверки подлинности и передачи данных, создав аналогичные [правила NSG](../virtual-network/network-security-groups-overview.md#service-tags) для *Azure Active Directory* и *службы хранилища Azure*.  Ниже описан процесс создания правила для тега Azure Backup:

1. В разделе **Все службы** перейдите к **группам сетевой безопасности** и выберите группу сетевой безопасности.

1. Выберите **Правила безопасности для исходящего трафика** в разделе **Параметры**.

1. Выберите **Добавить**. Введите все необходимые сведения для создания нового правила, как описано в разделе [параметры правила безопасности](../virtual-network/manage-network-security-group.md#security-rule-settings). Убедитесь, что параметр **Назначение** имеет значение *Тег службы*, а **Тег целевой службы** имеет значение *AzureBackup*.

1. Щелкните **Добавить**, чтобы сохранить только что созданное исходящее правило безопасности.

Аналогичным образом можно создавать правила безопасности для исходящего трафика NSG для службы хранилища Azure и Azure AD. Дополнительные сведения о тегах служб см. в [этой статье](../virtual-network/service-tags-overview.md).

#### <a name="azure-firewall-tags"></a>Теги Брандмауэра Azure

Если вы используете Брандмауэр Azure, создайте правило приложения с помощью[тега AzureBackup FQDN](../firewall/fqdn-tags.md) *AzureBackup*. Это разрешает весь исходящий трафик к службам Azure Backup.

#### <a name="allow-access-to-service-ip-ranges"></a>Разрешение доступа к диапазонам IP-адресов службы

Если вы решили разрешить доступ к IP-адресам службы, ознакомьтесь со сведениями о диапазонах IP-адресов в файле JSON [здесь](https://www.microsoft.com/download/confirmation.aspx?id=56519). Вам потребуется разрешить доступ к IP-адресам, соответствующим Azure Backup, службе хранилища Azure и Azure Active Directory.

#### <a name="allow-access-to-service-fqdns"></a>Разрешение доступа к FQDN службы

Чтобы разрешить доступ к необходимым службам с серверов, можно также использовать следующие FQDN:

| Служба    | Доменные имена для доступа                             |
| -------------- | ------------------------------------------------------------ |
| Azure Backup  | `*.backup.windowsazure.com`                             |
| Служба хранилища Azure | `*.blob.core.windows.net` <br><br> `*.queue.core.windows.net` |
| Azure AD      | Разрешение доступа к FQDN в разделах 56 и 59 в соответствии с [этой статьей](/office365/enterprise/urls-and-ip-address-ranges#microsoft-365-common-and-office-online) |

#### <a name="use-an-http-proxy-server-to-route-traffic"></a>Использование прокси-сервера HTTP для маршрутизации трафика

Когда создается резервная копия базы данных SAP HANA на виртуальной машине Azure, расширение резервного копирования на виртуальной машине использует API HTTPS для отправки команд управления к Azure Backup, а данных — в службу хранилища Azure. Расширение резервного копирования также использует Azure AD для аутентификации. Направьте трафик расширения резервного копирования для этих трех служб через прокси-сервер HTTP. Используйте список IP-адресов и FQDN, указанных выше, чтобы разрешить доступ к необходимым службам. Проверенные прокси-серверы не поддерживаются.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>Обнаружение баз данных

1. В разделе хранилища **Приступая к работе** щелкните **Архивация**. В разделе **Где выполняется рабочая нагрузка?** выберите **SAP HANA в виртуальной машине Azure**.
2. Выберите пункт **Начать обнаружение**. Это инициирует обнаружение незащищенных виртуальных машин Linux в регионе хранилища.

   * После обнаружения на портале появятся незащищенные виртуальные машины, упорядоченные по имени и группе ресурсов.
   * Если виртуальные машины перечислены не так, как вы ожидали, проверьте наличие их резервной копии в хранилище.
   * Несколько виртуальных машин могут иметь одно имя, но они будут принадлежать к разным группам ресурсов.

3. В разделе **Выбор виртуальных машин** щелкните ссылку, чтобы скачать скрипт, предоставляющий службе Azure Backup разрешения на доступ к виртуальным машинам SAP HANA для обнаружения баз данных.
4. Запустите скрипт на каждой виртуальной машине, где размещены базы данных SAP HANA, для которых требуется создать резервную копию.
5. После выполнения скрипта на виртуальной машине в разделе **Выбор виртуальных машин** выберите виртуальные машины. Затем выберите **Обнаружить базы данных**.
6. Azure Backup обнаруживает все базы данных SAP HANA на виртуальной машине. Во время обнаружения Azure Backup регистрирует виртуальную машину в хранилище и устанавливает расширение на виртуальной машине. В базе данных агент не устанавливается.

    ![Обнаружение баз данных SAP HANA](./media/backup-azure-sap-hana-database/hana-discover.png)

## <a name="configure-backup"></a>Настройка резервного копирования  

Теперь включите резервное копирование.

1. На шаге 2 выберите **Настройка резервного копирования**.

    ![Настройка резервного копирования](./media/backup-azure-sap-hana-database/configure-backup.png)
2. В разделе **Выбрать элементы для резервного копирования** выберите все базы данных, которые нужно защитить, и нажмите кнопку **ОК**.

    ![Выбор элементов для резервного копирования](./media/backup-azure-sap-hana-database/select-items.png)
3. В разделе **Политика резервного копирования** > **Выбрать политику резервного копирования** создайте политику резервного копирования для баз данных в соответствии с инструкциями ниже.

    ![Выбор политики резервного копирования](./media/backup-azure-sap-hana-database/backup-policy.png)
4. После создания политики в меню **резервного копирования** выберите **включить резервное копирование**.

    ![Включение резервного копирования](./media/backup-azure-sap-hana-database/enable-backup.png)
5. В области **Уведомления** портала можно отслеживать ход настройки резервного копирования.

### <a name="create-a-backup-policy"></a>создание политики архивации;

Политика резервного копирования определяет, когда выполняется резервное копирование и длительность хранения резервных копий.

* Политика создается на уровне хранилища.
* Несколько хранилищ могут использовать одну и ту же политику резервного копирования, но тогда необходимо применить эту политику резервного копирования к каждому хранилищу.

>[!NOTE]
>Azure Backup не выполняет автоматический переход на летнее время при резервном копировании базы данных SAP HANA на виртуальной машине Azure.
>
>Измените политику вручную, если это необходимо.

Укажите параметры политики следующим образом:

1. Введите имя новой политики в поле **Имя политики**.

   ![Введите имя политики](./media/backup-azure-sap-hana-database/policy-name.png)
2. В меню **Full Backup policy** (Политика полного резервного копирования) для параметра **Частота архивации** выберите **Ежедневно** или **Еженедельно**.
   * **Ежедневно.** Выберите часовой пояс и час для запуска задания резервного копирования.
       * Необходимо запустить полную резервную копию. Этот параметр нельзя отключить.
       * Щелкните **Полная резервная копия**, чтобы просмотреть политику.
       * При ежедневном создании полных резервных копий невозможно создавать разностные резервные копии.
   * **Еженедельно**. Выберите день недели, час и часовой пояс для запуска задания резервного копирования.

   ![Выбор частоты резервного копирования](./media/backup-azure-sap-hana-database/backup-frequency.png)

3. В разделе **Диапазон хранения** настройте параметры периода удержания для полной резервной копии.
    * По умолчанию выбраны все варианты. Очистите любые ограничения периода хранения, которые вы не хотите использовать, и установите соответствующие ограничения.
    * Минимальный период хранения для резервной копии любого типа (полная/разностная/журнал) составляет семь дней.
    * Точки восстановления отмечены для хранения исходя из их диапазона хранения. Например, если выбран параметр "Ежедневно", то каждый день активируется создание только одной полной резервной копии.
    * Резервная копия за определенный день помечается и хранится в соответствии с недельным диапазоном хранения и параметром.
    * Месячный и годовой диапазоны хранения действуют аналогичным образом.

4. В меню **Full Backup policy** (Политика полного резервного копирования) щелкните **ОК**, чтобы подтвердить параметры.
5. Чтобы добавить политику разностного резервного копирования, щелкните **Разностная резервная копия**.
6. В меню **Differential Backup policy** (Политика разностной резервной копии) выберите **Включить** для открытия элементов управления периодичностью и хранением.
    * Максимально в день можно активировать одну разностную резервную копию.
    * Максимальный срок хранения разностных резервных копий составляет 180 дней. Если требуется более длительное хранение, необходимо использовать полные резервные копии.

    ![Политика разностного резервного копирования](./media/backup-azure-sap-hana-database/differential-backup-policy.png)

    > [!NOTE]
    > Добавочные резервные копии сейчас не поддерживаются.

7. Для сохранения политики и возврата к главному меню **Политика архивации** нажмите кнопку **ОК**.
8. Чтобы добавить политику резервного копирования журналов транзакций, выберите **Резервная копия журналов**.
    * В разделе **Резервная копия журналов** выберите **Включить**.  Это невозможно отключить, так как SAP HANA управляет всеми резервными копиями журналов.
    * Задайте частоту и элементы управления хранением.

    > [!NOTE]
    > Резервные копии журналов начнут создаваться только после завершения одного успешного полного резервного копирования.

9. Для сохранения политики и возврата к главному меню **Политика архивации** нажмите кнопку **ОК**.
10. После завершения определения политики резервного копирования нажмите кнопку **ОК**.

> [!NOTE]
> Каждая резервная копия журнала связана с предыдущей полной резервной копией, образуя цепочку восстановления. Эта полная резервная копия будет храниться до истечения срока хранения последней резервной копии журнала. Это может означать, что полная резервная копия сохраняется в течение дополнительного периода, чтобы гарантировать возможность восстановления всех журналов. Предположим, что у пользователя есть еженедельное полное резервное копирование, ежедневное разностное и 2-часовой журналы. Все они сохраняются в течение 30 дней. Однако еженедельное полное удаление может быть действительно очищено только после того, как будет доступна следующая полная резервная копия, то есть через 30 + 7 дней. Например, еженедельное полное резервное копирование происходит в 16 ноября. Согласно политике хранения, она должна храниться до Dec 16. Последняя резервная копия журнала для этой полной копии создается до следующего запланированного резервного копирования 22 ноября. Пока этот журнал доступен до 22 декабря, полная копия от 16 ноября не удаляется. Таким образом, полная копия от 16 ноября будет храниться до 22 декабря.

## <a name="run-an-on-demand-backup"></a>Выполнение резервного копирования по запросу

Резервное копирование выполняется в соответствии с расписанием политики. Резервное копирование по запросу можно выполнить следующим образом.

1. В меню хранилище выберите пункт **архивные элементы**.
2. В поле **архивные элементы**выберите виртуальную машину, на которой выполняется SAP HANAная база данных, а затем щелкните **создать резервную копию сейчас**.
3. В поле **BACKUP Now (Архивация**) выберите тип резервной копии, которую вы хотите выполнить. Нажмите кнопку **ОК**. Эта резервная копия будет храниться в соответствии с политикой, связанной с этим элементом архивации.
4. Отслеживайте уведомления на портале. Вы можете отслеживать ход выполнения задания на панели мониторинга хранилища в разделе **Задания резервного копирования** > **Выполняется**. В зависимости от размера базы данных создание начального архива может занять некоторое время.

По умолчанию срок хранения резервных копий по запросу составляет 45 дней.

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>Запуск резервного копирования SAP HANA Studio в базе данных с включенной службой Azure Backup

Если вы хотите создать локальную резервную копию (с помощью HANA Studio ) базы данных, для которой создается резервная копия с помощью Azure Backup, выполните следующие действия.

1. Дождитесь завершения всех полных резервных копий или резервных копий журналов для базы данных. Проверьте состояние в SAP HANA Studio или Cockpit.
1. Отключите для соответствующей базы данных резервное копирование журналов и задайте в качестве каталога резервного копирования файловую систему.
1. Для этого дважды щелкните **systemdb** > **Конфигурация** > **Выбрать базу данных** > **Фильтр (журнал)** .
1. Установите параметр **enable_auto_log_backup** в значение **No**.
1. Установите параметр **log_backup_using_backint** в значение **False**.
1. Задайте **catalog_backup_using_backint** для Catalog_backup_using_backint **значение false**.
1. Создайте полную резервную копию базы данных по запросу.
1. Дождитесь завершения полного резервного копирования и резервного копирования каталога.
1. Верните предыдущие значения параметров в Azure.
    * Установите параметр **enable_auto_log_backup** в значение **Yes**.
    * Установите параметр **log_backup_using_backint** в значение **True**.
    * Задайте **catalog_backup_using_backint** для Catalog_backup_using_backint **значение true**.

## <a name="next-steps"></a>Дальнейшие действия

* Узнайте, как [восстановить базы данных SAP HANA, запущенные на виртуальных машинах Azure](./sap-hana-db-restore.md).
* Узнайте, как [управлять базами данных SAP HANA, резервное копирование которых выполняется с помощью Azure Backup](./sap-hana-db-manage.md).