---
title: Копирование данных из хранилища BLOB-объектов в базу данных SQL Azure
description: В этом учебнике рассказывается, как использовать действие копирования в конвейере фабрики данных Azure для копирования данных из хранилища BLOB-объектов в базу данных SQL.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: e4035060-93bf-4e8d-bf35-35e2d15c51e0
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 15bce219b96268124729de2f475e33fc386348a8
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "92631739"
---
# <a name="tutorial-copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Руководство. Копирование данных из хранилища BLOB-объектов Azure в базу данных SQL с помощью фабрики данных
> [!div class="op_single_selector"]
> * [Обзор и предварительные требования](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Мастер копирования](data-factory-copy-data-wizard-tutorial.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Шаблон Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [API для .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> В этой статье рассматривается служба "Фабрика данных Azure" версии 1. Если вы используете текущую версию Фабрики данных, ознакомьтесь с руководством по [применению действия копирования](../quickstart-create-data-factory-dot-net.md).

В этом руководстве вы создадите фабрику данных с конвейером для копирования данных из хранилища BLOB-объектов в базу данных SQL.

Действие копирования перемещает данные в фабрике данных Azure. Это действие выполняется с помощью глобально доступной службы, обеспечивающей безопасное, надежное и масштабируемое копирование данных между разными хранилищами. Дополнительные сведения о действии копирования см. в статье [Перемещение данных с помощью действия копирования](data-factory-data-movement-activities.md).  

> [!NOTE]
> Подробный обзор службы фабрики данных см. в статье [Общие сведения о службе фабрики данных Azure, службе интеграции данных в облаке](data-factory-introduction.md).
>
>

## <a name="prerequisites-for-the-tutorial"></a>Предварительные требования для прохождения этого учебника
Для работы с этим учебником необходимо следующее:

* **Подписка Azure** .  Если у вас нет подписки, вы можете создать бесплатную пробную версию учетной записи всего за несколько минут. Дополнительные сведения см. в статье [Бесплатная пробная версия](https://azure.microsoft.com/pricing/free-trial/).
* **Учетная запись хранения Azure** . В этом учебнике в качестве **источника** будет использоваться хранилище BLOB-объектов. Если у вас нет учетной записи хранения Azure, создайте ее, как описано в статье [Создание учетной записи хранения](../../storage/common/storage-account-create.md) .
* **База данных SQL Azure** . В этом руководстве вы используете базу данных SQL Azure в качестве **целевого** хранилища данных. Если у вас нет базы данных в базе данных SQL Azure, которую можно использовать в этом руководстве, см. статью [Создание и Настройка базы данных в базе данных SQL Azure](../../azure-sql/database/single-database-create-quickstart.md) для ее создания.
* **SQL Server 2012/2014 или Visual Studio 2013** . Для создания образца базы данных и просмотра итоговых данных в базе данных используется SQL Server Management Studio или Visual Studio .  

## <a name="collect-blob-storage-account-name-and-key"></a>Получение имени и ключа учетной записи хранения BLOB-объектов
В ходе изучения этого учебника потребуются имя и ключ вашей учетной записи хранения Azure. Запишите **имя** и **ключ учетной записи** для своей учетной записи хранения Azure.

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Щелкните **Все службы** в меню слева и выберите **Учетные записи хранения** .

    !["Обзор" — "Учетные записи хранения"](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. В колонке **Учетные записи хранения** выберите **учетную запись хранения Azure** , которая будет использоваться в ходе изучения этого учебника.
4. В разделе **Параметры** выберите ссылку **Ключи доступа** .
5. Нажмите кнопку **Копировать** (изображение) рядом с текстовым полем **Имя учетной записи хранения** , вставьте скопированный текст, например, в текстовый файл, и сохраните его.
6. Повторите предыдущий шаг для ключа **key1** .

    ![Ключ доступа к хранилищу](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. Закройте все колонки, нажав **X** .

## <a name="collect-sql-server-database-user-names"></a>Получение имени сервера SQL, имени базы данных и имени пользователя
Для работы с этим руководством требуются имена логического сервера SQL Server, базы данных и пользователя. Запишите имена **сервера** , **базы данных** и **пользователя** для базы данных SQL Azure.

1. На **портале Azure** щелкните **Все службы** слева и выберите **Базы данных SQL** .
2. В **колонке баз данных SQL** выберите **базу данных** , которую вы планируете использовать для этого учебника. Запишите **имя базы данных** .  
3. В колонке **База данных SQL** в разделе **Параметры** щелкните **Свойства** .
4. Запишите значения параметров **Имя сервера** и **Имя для входа администратора сервера** .
5. Закройте все колонки, нажав **X** .

## <a name="allow-azure-services-to-access-sql-server"></a>Предоставление службам Azure доступа к серверу SQL
Убедитесь, что параметр **Разрешить доступ к службам Azure** **включен для сервера** , чтобы служба фабрики данных могла получить доступ к серверу. Чтобы проверить и при необходимости включить этот параметр, сделайте следующее.

1. Щелкните **Все службы** слева и выберите **Серверы SQL** .
2. Выберите сервер и щелкните **Брандмауэр** в разделе **Параметры** .
3. В колонке **Параметры брандмауэра** щелкните **ВКЛ** для параметра **Разрешить доступ к службам Azure** .
4. Закройте все колонки, нажав **X** .

## <a name="prepare-blob-storage-and-sql-database"></a>Подготовка хранилища BLOB-объектов и базы данных SQL
Теперь Подготовьте хранилище BLOB-объектов Azure и базу данных SQL Azure для учебника, выполнив следующие действия.  

1. Запустите Блокнот. Скопируйте следующий текст и сохраните его в файл **emp.txt** в папке **C:\ADFGetStarted** на диске.

    ```
    John, Doe
    Jane, Doe
    ```
2. При помощи таких инструментов, как [Azure Storage Explorer](https://storageexplorer.com/), создайте контейнер **adftutorial** и передайте файл **emp.txt** в этот контейнер.

3. Используйте следующий скрипт SQL, чтобы создать таблицу **emp** в базе данных SQL Azure.  

    ```SQL
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

    **Если на компьютере установлено SQL Server 2012/2014:** следуйте инструкциям по [управлению базой данных SQL Azure с помощью SQL Server Management Studio](../../azure-sql/database/single-database-manage.md) , чтобы подключиться к серверу и запустить скрипт SQL.

    Если клиенту не разрешен доступ к логическому серверу SQL Server, необходимо настроить брандмауэр для сервера, чтобы разрешить доступ с компьютера (IP-адрес). Инструкции по настройке брандмауэра для сервера см. в [этой статье](../../azure-sql/database/firewall-configure.md) .

## <a name="create-a-data-factory"></a>Создание фабрики данных
Необходимые условия выполнены. Для создания фабрики данных можно использовать один из приведенных ниже способов. Выберите в раскрывающемся списке в верхней части страницы один из вариантов или щелкните одну из следующих ссылок, чтобы изучить руководство.     

* [Мастер копирования](data-factory-copy-data-wizard-tutorial.md)
* [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
* [Шаблон Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
* [API для .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> В этом руководстве конвейер данных копирует данные из исходного хранилища данных в целевое. Он не преобразовывает входные данные в выходные. Инструкции по преобразованию данных с помощью фабрики данных Azure см. в [учебнике по созданию первого конвейера для преобразования данных с помощью кластера Hadoop](data-factory-build-your-first-pipeline.md).
>
> Можно объединить в цепочку два действия (выполнить одно действие вслед за другим), настроив выходной набор данных одного действия как входной набор данных другого действия. Подробные сведения см. в статье [Планирование и исполнение с использованием фабрики данных](data-factory-scheduling-and-execution.md).