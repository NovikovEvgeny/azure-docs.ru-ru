---
title: Функции хранилища BLOB-объектов, доступные в Azure Data Lake Storage 2-го поколения | Документация Майкрософт
description: Узнайте, какие функции хранилища BLOB-объектов можно использовать с Azure Data Lake Storage 2-го поколения.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 10/28/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 8ab001636cc6fac921f552070b9b064d9c53a8d7
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93042081"
---
# <a name="blob-storage-features-available-in-azure-data-lake-storage-gen2"></a>Функции хранилища BLOB-объектов, доступные в Azure Data Lake Storage 2-го поколения

Функции хранилища BLOB-объектов, такие как [ведение журнала диагностики](../common/storage-analytics-logging.md), [уровни доступа](storage-blob-storage-tiers.md)и [политики управления жизненным циклом хранилища BLOB-объектов](storage-lifecycle-management-concepts.md) , теперь работают с учетными записями, имеющими иерархическое пространство имен. Таким образом, можно включить иерархические пространства имен в учетных записях хранения BLOB-объектов без потери доступа к этим функциям.

В этой таблице перечислены функции хранилища BLOB-объектов, которые можно использовать с Azure Data Lake Storage 2-го поколения. Элементы в этих таблицах со временем будут меняться, так как поддержка постоянно расширяется. Дополнительные сведения о конкретных проблемах, связанных с состоянием предварительной версии функции, см. в статье [Известные проблемы](data-lake-storage-known-issues.md).

## <a name="supported-blob-storage-features"></a>Поддерживаемые функции хранилища BLOB-объектов

В следующей таблице показано, как каждая функция хранилища BLOB-объектов поддерживается в Data Lake Storage 2-го поколения. Существует столбец для уровней производительности "Стандартный" и " [премиум](premium-tier-for-data-lake-storage.md) ". 

|Функция хранилища BLOB-объектов |Standard |Premium |Связанные статьи |
|---------------|-------------------|---|
|"Горячий" уровень доступа|Общедоступная версия|Не поддерживается|[Хранилище BLOB-объектов Azure: горячий, холодный и архивный уровни доступа](storage-blob-storage-tiers.md)|
|"Холодный" уровень доступа|Общедоступная версия|Не поддерживается|[Хранилище BLOB-объектов Azure: горячий, холодный и архивный уровни доступа](storage-blob-storage-tiers.md)|
|События|Общедоступная версия|Общедоступная версия|[Reacting to Blob storage events (preview)](storage-blob-event-overview.md) (Реагирование на события хранилища BLOB-объектов)|
|Метрики (классические)|Общедоступная версия|Общедоступная версия|[Метрики Аналитики Службы хранилища Azure (классические)](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Метрики в Azure Monitor|Общедоступная версия|Preview (Предварительный просмотр)|[Метрики службы хранилища Azure в Azure Monitor](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Команды PowerShell для хранилища BLOB-объектов|Общедоступная версия|Общедоступная версия|[Краткое руководство. Отправка, скачивание и составление списка больших двоичных объектов с помощью PowerShell](storage-quickstart-blobs-powershell.md)|
|Команды Azure CLI для хранилища BLOB-объектов|Общедоступная версия|Общедоступная версия|[Краткое руководство. Создание, скачивание и составление списка больших двоичных объектов с помощью Azure CLI](storage-quickstart-blobs-cli.md)|
|API хранилища BLOB-объектов|Общедоступная версия|Общедоступная версия|[Краткое руководство. Использование библиотеки хранилища BLOB-объектов Azure версии 12 для .NET](storage-quickstart-blobs-dotnet.md)<br>[Краткое руководство. Управление большими двоичными объектами с помощью пакета SDK для Java версии 12](storage-quickstart-blobs-java.md)<br>[Краткое руководство. Управление большими двоичными объектами с помощью пакета SDK для Python версии 12](storage-quickstart-blobs-python.md)<br>[Краткое руководство. Управление большими двоичными объектами с помощью пакета SDK для JavaScript версии 12 в Node.js](storage-quickstart-blobs-nodejs.md)|
|Журналы диагностики|Общедоступная версия|Preview (Предварительный просмотр) |[Ведение журнала аналитики службы хранилища Azure](../common/storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Архивный уровень доступа|Общедоступная версия|Не поддерживается|[Хранилище BLOB-объектов Azure: горячий, холодный и архивный уровни доступа](storage-blob-storage-tiers.md)|
|Политики управления жизненным циклом (многоуровневое управление)|Общедоступная версия|Еще не поддерживается|[Управление жизненным циклом хранилища BLOB-объектов Azure (предварительная версия)](storage-lifecycle-management-concepts.md)|
|Политики управления жизненным циклом (удаление большого двоичного объекта)|Общедоступная версия|Общедоступная версия|[Управление жизненным циклом хранилища BLOB-объектов Azure (предварительная версия)](storage-lifecycle-management-concepts.md)|
|Вход в Azure Monitor|Preview (Предварительный просмотр) |Preview (Предварительный просмотр)|[Мониторинг службы хранилища Azure](../common/monitor-storage.md)|
|Моментальные снимки|Preview (Предварительный просмотр)|Preview (Предварительный просмотр)|[Моментальные снимки BLOB-объектов](snapshots-overview.md)|
|Статические веб-сайты|Preview (Предварительный просмотр)|Preview (Предварительный просмотр)|[Размещение статических веб-сайтов в службе хранилища Azure](storage-blob-static-website.md)|
|Неизменяемое хранилище|Preview (Предварительный просмотр)|Preview (Предварительный просмотр)|[Хранение критически важных для бизнеса данных большого двоичного объекта с помощью неизменяемого хранилища](storage-blob-immutable-storage.md)|
|Обратимое удаление контейнера|Preview (Предварительный просмотр)|Preview (Предварительный просмотр)|[Обратимое удаление для контейнеров (Предварительная версия)](soft-delete-container-overview.md)|
|Обратимое удаление BLOB-объекта|Еще не поддерживается|Еще не поддерживается|[Обратимое удаление для больших двоичных объектов](storage-blob-soft-delete.md)|
|Blobfuse|Preview (Предварительный просмотр)|Preview (Предварительный просмотр)|[Как подключить хранилище BLOB-объектов в качестве файловой системы с использованием blobfuse](storage-how-to-mount-container-linux.md)|
|Отработка отказа учетной записи|Еще не поддерживается|Еще не поддерживается|[Аварийное восстановление и отработка отказа учетной записи](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Список управления доступом для контейнера больших двоичных объектов|Не поддерживается<div role="complementary" aria-labelledby="blob-container-ACL"><sup>1</sup></div>|Не поддерживается<div role="complementary" aria-labelledby="blob-container-ACL"><sup>2</sup></div>|См. Примечание ниже этой таблицы.|
|Ключи, предоставляемые клиентом|Еще не поддерживается|Еще не поддерживается|[Укажите ключ шифрования для запроса к хранилищу BLOB-объектов](encryption-customer-provided-keys.md)|
|Личные домены|Еще не поддерживается|Еще не поддерживается|[Сопоставление личного домена с конечной точкой хранилища BLOB-объектов Azure](storage-custom-domain-name.md)|
|Области шифрования|Еще не поддерживается|Еще не поддерживается|[Создание и управление областями шифрования (Предварительная версия)](encryption-scope-manage.md)|
|Канал изменений|Еще не поддерживается|Еще не поддерживается|[Поддержка канала изменений в хранилище BLOB-объектов Azure](storage-blob-change-feed.md)|
|Репликация объектов|Еще не поддерживается|Еще не поддерживается|[Настройка репликации объектов для блочных BLOB-объектов (предварительная версия)](object-replication-configure.md)|
|Управление версиями BLOB-объектов|Еще не поддерживается|Еще не поддерживается|[Включение управления версиями BLOB-объектов и управление ими (Предварительная версия)](versioning-enable.md)|

<div id="blob-container-ACL"><sup>1</sup> можно задать списки управления доступом для корневой папки контейнера, но не для самого контейнера.</div><br>

<div id="preview-form"><sup>2</sup> Чтобы использовать моментальные снимки, неизменяемое хранилище или статические веб-сайты с Data Lake Storage 2-го поколения, необходимо зарегистрироваться в предварительной версии, выполнив эту <a href=https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2EUNXd_ZNJCq_eDwZGaF5VUOUc3NTNQSUdOTjgzVUlVT1pDTzU4WlRKRy4u>форму</a>.  </div>

## <a name="see-also"></a>См. также раздел

- [Известные проблемы с Azure Data Lake Storage 2-го поколения](data-lake-storage-known-issues.md)
- [Службы Azure, поддерживающие Azure Data Lake Storage 2-го поколения](data-lake-storage-supported-azure-services.md)
- [Платформы с открытым кодом, поддерживающие Azure Data Lake Storage 2-го поколения](data-lake-storage-supported-open-source-platforms.md)
- [Multi-protocol access on Azure Data Lake Storage (preview)](data-lake-storage-multi-protocol-access.md) (Доступ с использованием нескольких протоколов на Azure Data Lake Storage (предварительная версия))
