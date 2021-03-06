---
title: Серверы — база данных Azure для MariaDB
description: В этой статье приведены рекомендации и указания по работе с серверами базы данных Azure для MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 444d7f1574cf1517b01250bcb9d810731030182d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "79527798"
---
# <a name="server-concepts-in-azure-database-for-mariadb"></a>Основные понятия работы с сервером в базе данных Azure для MariaDB
В этой статье приведены рекомендации и указания по работе с серверами базы данных Azure для MariaDB.

## <a name="what-is-an-azure-database-for-mariadb-server"></a>Что такое сервер базы данных Azure для MariaDB?

Сервер базы данных Azure для MariaDB является центральной точкой администрирования нескольких баз данных. Подобная серверная конструкция MariaDB может быть вам знакома по работе в локальной среде. В частности, служба базы данных Azure для MariaDB является управляемой, предоставляет гарантии производительности, обеспечивает доступ и функциональность на уровне сервера.

База данных Azure для сервера MariaDB:

- создается в подписке Azure;
- выступает в качестве родительского ресурса для баз данных;
- предоставляет пространство имен для баз данных;
- представляет собой контейнер со строгой семантикой времени существования (при удалении сервера удаляются также все расположенные на нем базы данных);
- выравнивает ресурсы в регионе;
- предоставляет конечную точку подключения к серверу и базе данных;
- обеспечивает область для политик управления, применяемых к базам данных: имена входа, брандмауэр, пользователи, роли, конфигурации и т. д.;
- доступна в версии ядра MariaDB 10.2. Дополнительные сведения см. в статье [Поддерживаемые версии в базе данных Azure для MariaDB](./concepts-supported-versions.md).

На сервере базы данных Azure для MariaDB можно создать одну или несколько баз данных. Можно создать по одной базе данных на каждом сервере, чтобы использовать все ресурсы, или несколько баз данных, чтобы предоставить общий доступ к ресурсам. Цена формируется для каждого сервера исходя из конфигурации ценовой категории, количества виртуальных ядер и объема хранилища (ГБ). Дополнительные сведения см. в разделе [Ценовые категории](./concepts-pricing-tiers.md).

## <a name="how-do-i-secure-an-azure-database-for-mariadb-server"></a>Как защитить сервер базы данных Azure для MariaDB?

Ниже перечислены элементы, которые помогают обеспечить безопасный доступ к базе данных.

|||
| :--| :--|
| **Аутентификация и авторизация** | Сервер базы данных Azure для MariaDB поддерживает собственную аутентификацию MySQL. Подключиться к серверу и выполнить аутентификацию можно с помощью учетных данных администратора сервера. |
| **протокол**; | Служба поддерживает протокол на основе сообщений, используемый MySQL. |
| **TCP/IP** | Протокол работает через TCP/IP, а также через сокеты домена Unix. |
| **Брандмауэр** | Для защиты данных правило брандмауэра запрещает любой доступ к серверу базы данных, пока не будут указаны компьютеры, которые имеют разрешение. См. статью [Правила брандмауэра сервера базы данных Azure для MariaDB](./concepts-firewall-rules.md). |
| **SSL** | Служба поддерживает применение SSL-соединений между приложениями и сервером базы данных. См. [Настройка SSL-подключений в приложении для безопасного подключения к базе данных Azure для MariaDB](./howto-configure-ssl.md). |

## <a name="how-do-i-manage-a-server"></a>Как управлять сервером?
Управлять серверами базы данных Azure для MariaDB можно с помощью портала Azure или Azure CLI.

## <a name="next-steps"></a>Дальнейшие действия
- Общие сведения об этой службе см. в статье [Обзор базы данных Azure для MariaDB](./overview.md).
- Сведения о квотах и ограничениях для конкретных ресурсов, основанных на **уровне служб**, см. в разделе [уровни служб](./concepts-pricing-tiers.md) .

<!-- - For information about connecting to the service, see [Connection libraries for Azure Database for MariaDB](./concepts-connection-libraries.md). -->
