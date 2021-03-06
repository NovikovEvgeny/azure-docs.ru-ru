---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: rogarana
ms.openlocfilehash: 3d4cc17570057f5f37cf38685847afbe38ea6831
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90606265"
---
| Ресурс | Общие файлы уровня "Стандарт" | Общие папки ценовой категории "Премиум" |
|----------|---------------|------------------------------------------|
| Минимальный размер общего файла | Минимальный размер отсутствует, оплата по мере использования | 100 гиб; подготовлено |
| Максимальный размер общей папки | 100 Тиб*, 5 Тиб | 100 Тиб |
| Максимальный размер файла в общей папке | 1 ТиБ | 4 ТиБ |
| Максимальное количество файлов в общей папке | Без ограничений | Без ограничений |
| Максимальное число операций ввода-вывода на общую папку | 10 000 операций ввода-вывода*, 1000 операций ввода-вывода или 100 запросов за 100 мс | 100 000 операций ввода-вывода в секунду |
| Максимальное число хранимых политик доступа на общую папку | 5 | 5 |
| Целевая пропускная способность для одной общей папки | до 300 MиБ/с*, до 60 MиБ/с  | См. значения входящего и исходящего трафика для общей папки категории "Премиум"|
| Максимальное значение исходящего трафика для одной общей папки | См. целевую пропускную способность для общей папки категории "Стандартный" | До 6204 МиБ/с |
| Максимальное значение входящего трафика для одной общей папки | См. целевую пропускную способность для общей папки категории "Стандартный" | До 4136 МиБ/с |
| Максимальное количество открытых дескрипторов на файл или каталог | 2000 открытых маркеров | 2000 открытых маркеров |
| Максимальное число моментальных снимков общих ресурсов | 200 моментальных снимков общих ресурсов | 200 моментальных снимков общих ресурсов |
| Максимальная длина имени объекта максимальное (папок и файлов) | 2048 символов | 2048 символов |
| Максимальное количество компонентов в имени пути (в пути \A\B\C\D каждая буква обозначает компонент) | 255 символов | 255 символов |
| Ограничение жесткой связи (только NFS) | Н/Д | 178 |

\* Размер общих папок категории "Стандартный" по умолчанию — 5 Тиб. Дополнительные сведения о том, как увеличить масштаб стандартных общих папок категории "Стандартный" до 100 Тиб см. в статье [Включение и создание больших общих папок](../articles/storage/files/storage-files-how-to-create-large-file-share.md).
