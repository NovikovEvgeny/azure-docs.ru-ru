---
title: Поддерживаемые хранилища данных в Azure Data Share
description: Сведения о хранилищах данных, которые поддерживаются для использования общей папки данных Azure.
ms.service: data-share
author: jifems
ms.author: jife
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: f3ecf8ef22d3f1d66b7148b809475a830c7e9f13
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2020
ms.locfileid: "92318580"
---
# <a name="supported-data-stores-in-azure-data-share"></a>Поддерживаемые хранилища данных в Azure Data Share

Общая папка данных Azure предоставляет доступ к открытым и гибким данным, включая возможность совместного использования с различными хранилищами данных. Поставщики данных могут обмениваться данными из одного типа хранилища данных, и их потребители данных могут выбрать хранилище данных для получения данных. 

В этой статье вы узнаете о обширном наборе хранилищ данных Azure, которые поддерживаются в общем ресурсе данных Azure. Также можно найти сведения о комбинациях хранилищ данных, которые можно использовать поставщиками данных и потребителями данных. 

## <a name="what-data-stores-are-supported-in-azure-data-share"></a>Какие хранилища данных поддерживаются в общем ресурсе данных Azure? 

В таблице ниже приведены поддерживаемые источники данных для общей папки данных Azure. 

| Хранилище данных | Общий доступ на основе моментальных снимков | Общий доступ на месте 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Хранилище BLOB-объектов Azure |✓ | |
| Хранилище Azure Data Lake Storage 1-го поколения |✓ | |
| Azure Data Lake Storage 2-го поколения |✓ ||
| База данных SQL Azure |✓ | |
| Azure синапсе Analytics (ранее — хранилище данных SQL Azure) |✓ | |
| Azure Data Explorer | |✓ |

## <a name="data-store-support-matrix"></a>Матрица поддержки хранилища данных

Общая папка данных Azure предоставляет потребителям данных гибкие возможности при выборе хранилища данных для приема данных в. Например, данные, общие для базы данных SQL Azure, можно получить в Azure Data Lake Store Gen2, базе данных SQL Azure или Azure синапсе Analytics. Клиенты могут выбрать формат для получения данных при настройке полученного общего ресурса данных. 

В таблице ниже приведены различные сочетания и варианты, которые пользователи данных имеют при принятии и настройке общей папки данных. Дополнительные сведения о настройке сопоставлений наборов данных см. в разделе [Настройка сопоставлений наборов данных](how-to-configure-mapping.md).

| Хранилище данных | хранилище BLOB-объектов Azure | Хранилище Azure Data Lake Storage 1-го поколения | Azure Data Lake Storage 2-го поколения | База данных SQL Azure | Azure Synapse Analytics | Azure Data Explorer
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| Хранилище BLOB-объектов Azure | ✓ || ✓ ||
| Хранилище Azure Data Lake Storage 1-го поколения | ✓ | | ✓ ||
| Azure Data Lake Storage 2-го поколения | ✓ | | ✓ ||
| База данных SQL Azure | ✓ | | ✓ | ✓ | ✓ ||
| Azure синапсе Analytics (ранее — хранилище данных SQL Azure) | ✓ | | ✓ | ✓ | ✓ ||
| Azure Data Explorer |||||| ✓ |

## <a name="share-from-a-storage-account"></a>Общий доступ из учетной записи хранения
Общая папка данных Azure поддерживает общий доступ к файлам, папкам и файловым системам Azure Data Lake Gen1 и Azure Data Lake Gen2. Он также поддерживает общий доступ к BLOB-объектам, папкам и контейнерам из хранилища BLOB-объектов Azure. В настоящее время поддерживается только блочный BLOB-объект. Если файловые системы, контейнеры или папки используются совместно с общим доступом на основе моментальных снимков, потребитель данных может создать полную копию общих данных или использовать функцию добавочного моментального снимка для копирования только новых или обновленных файлов. Добавочный моментальный снимок основан на времени последнего изменения файлов. Существующие файлы с тем же именем будут перезаписаны.

Дополнительные сведения см. в разделе [общий доступ и получение данных из хранилища BLOB-объектов Azure и Azure Data Lake Storage](how-to-share-from-storage.md) .

## <a name="share-from-a-sql-based-source"></a>Общий доступ из источника на основе SQL
Общий ресурс данных Azure поддерживает общий доступ к таблицам и представлениям из базы данных SQL Azure и Azure синапсе Analytics (ранее — хранилище Azure SQL). Потребители данных могут принять данные в Azure Data Lake Storage 2-го поколения или хранилище BLOB-объектов Azure в виде CSV-или Parquet-файла, а также в базу данных SQL Azure и Azure синапсе Analytics в качестве таблиц.

При принятии данных в Azure Data Lake Store Gen2 или хранилище BLOB-объектов Azure полные моментальные снимки перезаписывают содержимое целевого файла, если он уже существует.
При получении данных в таблицу и если Целевая таблица еще не существует, Общая папка данных Azure создает таблицу SQL с исходной схемой. Если целевая таблица с таким именем уже существует, она будет удалена и переписана с использованием последнего полного моментального снимка. Добавочные моментальные снимки в настоящее время не поддерживаются.

Дополнительные сведения см. в статье [общий доступ и получение данных из базы данных SQL Azure и Azure синапсе Analytics](how-to-share-from-sql.md) .

## <a name="share-from-azure-data-explorer"></a>Предоставление общего доступа к данным из Azure Data Explorer
Общий ресурс данных Azure поддерживает возможность совместного использования баз данных на месте из кластеров Azure обозреватель данных. Поставщик данных может совместно использоваться на уровне базы данных или кластера. При общем доступе на уровне базы данных потребитель данных может получить доступ только к тем базам данных, которые совместно используются поставщиком данных. При совместном использовании на уровне кластера потребитель данных может получить доступ ко всем базам данных из кластера поставщика, включая все будущие базы данных, созданные поставщиком данных.

Чтобы получить доступ к общим базам данных, потребитель данных должен иметь собственный кластер Azure обозреватель данных. Кластер Azure обозреватель данных для потребителя данных должен находиться в том же центре обработки данных Azure, что и кластер Azure обозреватель данных поставщика данных. Если установлено отношение общего доступа, Общая папка данных Azure создает символьную связь между поставщиком и кластерами Azure обозреватель данных потребителя. Данные, полученные с помощью пакетного режима, в исходный кластер Azure обозреватель данных будут отображаться в целевом кластере в течение нескольких секунд до нескольких минут.

Дополнительные сведения см. в разделе [общий доступ и получение данных из обозреватель данных Azure](/azure/data-explorer/data-share) . 

## <a name="next-steps"></a>Дальнейшие действия

Чтобы узнать, как начать совместное использование данных, перейдите к руководству по [совместному использованию данных](share-your-data.md) .
