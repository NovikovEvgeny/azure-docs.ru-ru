---
title: Долгосрочное хранение резервных копий
titleSuffix: Azure SQL Database & Azure SQL Managed Instance
description: Узнайте, как база данных SQL Azure & Azure SQL Управляемый экземпляр поддерживает хранение полных резервных копий баз данных до 10 лет с помощью политики долгосрочного хранения.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, sstein
ms.date: 05/18/2019
ms.openlocfilehash: 8250fc39fe58168ddc13b7bcf5c040b57d5e92fb
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2020
ms.locfileid: "92782626"
---
# <a name="long-term-retention---azure-sql-database-and-azure-sql-managed-instance"></a>Долгосрочное хранение — база данных SQL Azure и Azure SQL Управляемый экземпляр

Многие приложения имеют нормативные требования, соответствие требованиям или другие бизнес-цели, для которых требуется хранить резервные копии баз данных за пределами 7-35 дней, предоставляемых базой данных SQL Azure и Azure SQL Управляемый экземпляр [автоматических резервных копий](automated-backups-overview.md). Используя функцию долгосрочного хранения (LTR), можно хранить указанную базу данных SQL и полную резервную копию SQL Управляемый экземпляр в хранилище BLOB-объектов Azure с [настроенной избыточностью](automated-backups-overview.md#backup-storage-redundancy) до 10 лет. Впоследствии любую резервную копию можно восстановить как новую базу данных.

Длительное время хранения можно включить для базы данных SQL Azure и в ограниченном общедоступной предварительной версии для Управляемый экземпляр Azure SQL. В этой статье приводятся общие сведения о долгосрочном хранении. Чтобы настроить долгосрочное хранение, см. статью [Настройка базы данных SQL Azure LTR](long-term-backup-retention-configure.md) и [Настройка управляемый экземпляр LTR для Azure SQL](../managed-instance/long-term-backup-retention-configure.md). 

> [!NOTE]
> Задания агентов SQL можно использовать для планирования [резервных копий только для копирования базы данных](/sql/relational-databases/backup-restore/copy-only-backups-sql-server) как альтернативу LTR за 35 дней.


## <a name="how-long-term-retention-works"></a>Как работает долгосрочное хранение
     
При долгосрочном хранении (LTR) резервных копий используются полные резервные копии базы данных, которые [создаются автоматически](automated-backups-overview.md) для восстановления до точки во времени (PITR). Если настроена политика LTR, эти резервные копии копируются в разные большие двоичные объекты для долгосрочного хранения. Копирование является фоновым заданием, которое не влияет на производительность рабочей нагрузки базы данных. Политика LTR для каждой базы данных в базе данных SQL также может указывать, как часто создаются резервные копии LTR.

Чтобы включить LTR, можно определить политику, используя сочетание четырех параметров: еженедельное хранение резервных копий (W), ежемесячное хранение резервных копий (M), ежегодное хранение резервных копий (Y) и неделю года (WeekOfYear). Если указать W, одна резервная копия каждую неделю будет копироваться для долгосрочного хранения. При указании M Первая резервная копия каждого месяца будет скопирована в долгосрочное хранение. Если указать Y, одна резервная копия в течение недели года, указанной в параметре WeekOfYear, копируется для долгосрочного хранения. Если указанное значение WeekOfYear находится в прошлом после настройки политики, первая резервная копия LTR будет создана в следующем году. Каждая резервная копия будет храниться в долгосрочном хранилище в соответствии с параметрами политики, настроенными при создании резервной копии LTR.

> [!NOTE]
> Любое изменение политики LTR применяется только к дальнейшим резервным копиям. Например, если будет изменено еженедельное хранение резервных копий (W), ежемесячное хранение резервных копий (M) или ежегодное хранение резервных копий (Y), новый параметр хранения будет применяться только к новым резервным копиям. Хранение существующих резервных копий не будет изменено. Если вы хотите удалить старые резервные копии LTR до истечения срока хранения, потребуется [вручную удалить резервные копии](./long-term-backup-retention-configure.md#delete-ltr-backups).
> 

Примеры политики LTR:

-  W = 0, M = 0, Y = 5, WeekOfYear = 3

   Третья полная резервная копия каждого года будет храниться в течение пяти лет.
   
- W = 0, M = 3, Y = 0

   Первая полная резервная копия каждого месяца будет храниться в течение трех месяцев.

- W = 12, M = 0, Y = 0

   Каждая полная недельная резервная копия хранится в течение 12 недель.

- W = 6, M = 12, Y = 10, WeekOfYear = 16

   Каждая еженедельная полная резервная копия будет храниться в течение шести недель. За исключением первой полной месячной резервной копии, которая подлежит хранению в течение 12 месяцев. За исключением полной резервной копии, сделанной на 16-ю неделю года, которая подлежит хранению в течение 10 лет. 

В следующей таблице приведена периодичность и сроки долгосрочного хранения резервных копий согласно следующей политике:

W = 12 недель (84 дня), M = 12 месяцев (365 дней), Y = 10 лет (3650 дней), WeekOfYear = 15 (неделя после 15 апреля)

   ![Пример долгосрочного хранения](./media/long-term-retention-overview/ltr-example.png)


Если изменить указанную выше политику и установить W = 0 (без еженедельных резервных копий), ритмичность резервных копий изменится, как показано в приведенной выше таблице на выделенные даты. Объем хранилища, необходимый для хранения этих резервных копий, сократится соответственно. 

> [!IMPORTANT]
> Время создания резервных копий отдельных LTR управляется Azure. Нельзя вручную создать резервную копию LTR или управлять временем создания резервной копии. После настройки политики LTR может пройти до 7 дней, прежде чем первая резервная копия будет отображаться в списке доступных резервных копий.  


## <a name="geo-replication-and-long-term-backup-retention"></a>Георепликация и долгосрочное хранение резервных копий

Если вы используете активную георепликацию или группы отработки отказа в качестве решения непрерывности бизнес-процессов, необходимо подготовиться к окончательным отработке отказа и настроить ту же политику LTR в базе данных или экземпляре получателя. Затраты на хранение на LTR не увеличиваются, так как резервные копии не формируются из баз данных-получателей. Резервные копии создаются только в том случае, если база данных-получатель становится первичной, а резервные копии будут созданы. Он обеспечивает непрерванное создание резервных копий на LTR, когда запускается отработка отказа и основное перемещение в дополнительный регион. 

> [!NOTE]
> Когда исходная база данных-источник восстанавливается из-за сбоя, вызвавшего отработку отказа, он становится новым получателем. Таким образом создание резервной копии не будет возобновлено, и существующая политика LTR не вступит в силу, пока она снова станет первичной. 

## <a name="sql-managed-instance-support"></a>Поддержка Управляемого экземпляра SQL

Использование долгосрочного хранения резервных копий с Azure SQL Управляемый экземпляр имеет следующие ограничения.

- **Ограниченная общедоступная Предварительная версия** . Эта предварительная версия доступна только для подписок EA и CSP и ограничена доступностью.  
- [**Только PowerShell**](../managed-instance/long-term-backup-retention-configure.md) . в настоящее время портал Azure не поддерживается. LTR должен быть включен с помощью PowerShell. 

Чтобы запросить регистрацию, создайте запрос в [службу поддержки Azure](https://azure.microsoft.com/support/create-ticket/). В поле Тип проблемы выберите пункт техническая проблема, для службы выберите SQL Управляемый экземпляр и для типа проблемы выберите **резервное копирование, восстановление и непрерывность бизнес-процессов/долгосрочное хранение резервных копий** . В запросе необходимо указать, что вы хотите зарегистрировать в ограниченном общедоступном предварительном просмотре LTR для SQL Управляемый экземпляр.

## <a name="configure-long-term-backup-retention"></a>Настройка долгосрочного хранения резервных копий

Вы можете настроить долгосрочное хранение резервных копий с помощью портал Azure и PowerShell для базы данных SQL Azure, а также PowerShell для Azure SQL Управляемый экземпляр. Восстановление базы данных из хранилища долгосрочного хранения (LTR) можно выполнить, выбрав необходимую резервную копию по метке времени. Базу данных можно восстановить на любой существующий сервер или управляемый экземпляр в той же подписке, что и исходная база данных.

Сведения о настройке долгосрочного хранения или восстановлении базы данных из резервной копии базы данных SQL с помощью портал Azure или PowerShell см. в статье [Управление долгосрочным хранением резервных копий базы данных SQL Azure](long-term-backup-retention-configure.md) .

Чтобы узнать, как настроить долгосрочное хранение или восстановить базу данных из резервной копии SQL Управляемый экземпляр с помощью PowerShell, см. статью [Управление Azure SQL управляемый экземпляр долгосрочного хранения резервных копий](../managed-instance/long-term-backup-retention-configure.md).

Восстановление базы данных из хранилища долгосрочного хранения (LTR) можно выполнить, выбрав необходимую резервную копию по метке времени. Базу данных можно восстановить на любой доступный сервер с той же подпиской, что и у исходной базы данных. Сведения о восстановлении базы данных из резервной копии LTR с помощью портал Azure или PowerShell см. в статье [Управление долгосрочным хранением резервных копий базы данных SQL Azure](long-term-backup-retention-configure.md). В запросе следует указать, что вы хотите зарегистрировать в ограниченном общедоступном предварительном просмотре LTR для SQL Управляемый экземпляр.

## <a name="next-steps"></a>Дальнейшие действия

Поскольку резервные копии базы данных защищают данные от случайного повреждения или удаления, они являются важной частью любой стратегии непрерывности бизнес-процессов и аварийного восстановления. Дополнительные сведения о других решениях для базы данных SQL, обеспечивающих непрерывность бизнес-процессов, см. в статье [Обзор обеспечения непрерывности бизнес-процессов с помощью базы данных SQL Azure](business-continuity-high-availability-disaster-recover-hadr-overview.md).
