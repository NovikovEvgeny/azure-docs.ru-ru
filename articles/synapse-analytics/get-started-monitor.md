---
title: Руководство по началу работы с Azure Synapse Analytics — мониторинг рабочей области Synapse
description: Из этого учебника вы узнаете, как отслеживать действия в рабочей области Synapse.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: monitoring
ms.topic: tutorial
ms.date: 10/15/2020
ms.openlocfilehash: d497ff1a829902c0623740f01a457e6496db2401
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "92744954"
---
# <a name="monitor-your-synapse-workspace"></a>Мониторинг рабочей области Synapse

Из этого учебника вы узнаете, как отслеживать действия в рабочей области Synapse. Вы можете отслеживать текущие действия и действия журнала SQL, Apache Spark и Pipelines. 

## <a name="introduction-to-the-monitor-hub"></a>Общие сведения о центре мониторинга

Откройте Synapse Studio и перейдите в центр **мониторинга**. Здесь вы можете просмотреть журнал всех действий, выполняемых в рабочей области, и какие из них сейчас активны. 

* В разделе **Интеграция** можно отслеживать конвейеры, триггеры и среды выполнения интеграции.
* В разделе **действий** можно отслеживать действия Spark и SQL. 

## <a name="integration"></a>Интеграция

1. Перейдите в раздел **Интеграция > Конвейер**. В этом представлении можно просмотреть, когда выполнялся конвейер в рабочей области. 
1. Найдите конвейер, который вы выполнили на предыдущем шаге, и щелкните **имя конвейера**.
1. Теперь можно просмотреть, как выполняются отдельные действия в этом конвейере.
1. Щелкните **адресную строку** в верхней части Synapse Studio, затем — **Все запуски конвейера** , чтобы вернуться к предыдущему представлению.

## <a name="apache-spark-activities"></a>Действия Apache Spark

1. Перейдите в раздел **Интеграция > Действия > Приложения Apache Spark**. Теперь вы можете просмотреть все приложения Spark, которые выполняются или выполнились в рабочей области.
1. Найдите приложение, которое уже не выполняется, и щелкните его **имя приложения**. Теперь можно просмотреть сведения о приложении Spark.
1. Если вы знакомы с Apache Spark, можно найти стандартный пользовательский интерфейс сервера журнала Apache Spark, щелкнув **сервер журнала Spark**.

## <a name="sql-activities"></a>Действия SQL

1. Перейдите в раздел **Интеграция > Действия > Запросы SQL**.
1. В этом представлении можно просмотреть запросы SQL.
1. Выберите **Пул** для выполнения мониторинга. Теперь вы можете просмотреть все запросы SQL, которые выполняются или выполнились в рабочей области этого пула.
1. Найдите конкретный запрос SQL и наведите указатель мыши на этот элемент. После наведения указателя мыши появится значок скрипта SQL.
1. Щелкните значок скрипта SQL, чтобы просмотреть полный текст запроса SQL.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Ознакомление с центром знаний](get-started-knowledge-center.md)
