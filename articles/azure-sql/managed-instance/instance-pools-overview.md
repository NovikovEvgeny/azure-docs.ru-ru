---
title: Что такое пул Управляемый экземпляр Azure SQL?
titleSuffix: Azure SQL Managed Instance
description: Узнайте о пулах Управляемый экземпляр Azure SQL (Предварительная версия) — функции, предоставляющей удобный и экономичный способ переноса баз данных меньшего объема SQL Server в облако и управления несколькими управляемыми экземплярами.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein
ms.date: 09/05/2019
ms.openlocfilehash: ab77c8cf563c315768ad1c16089d8d939c085322
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2020
ms.locfileid: "92782660"
---
# <a name="what-is-an-azure-sql-managed-instance-pool-preview"></a>Что такое пул Управляемый экземпляр Azure SQL (Предварительная версия)?
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Пулы экземпляров в Azure SQL Управляемый экземпляр предоставляют удобный и экономичный способ переноса небольших экземпляров SQL Server в облако в масштабе.

Пулы ресурсов позволяют предварительно подготовить к работе вычислительные ресурсы в соответствии с общими требованиями к миграции. Вы можете развернуть несколько отдельных управляемых экземпляров до предварительно подготовленного уровня вычислений. Например, при предварительной инициализации 8 виртуальных ядер можно развернуть экземпляр 2 2-Виртуальное ядро и 1 4-Виртуальное ядро, а затем перенести базы данных в эти экземпляры. До тех пор, пока пулы экземпляров станут доступными, при переходе в облако часто приходится объединять рабочие нагрузки с большим объемом вычислений и без них в более крупный управляемый экземпляр. Необходимость переноса групп баз данных на крупные экземпляры, как правило, требует тщательного планирования ресурсов и управления ресурсами, дополнительных соображений безопасности и некоторых дополнительных операций консолидации данных на уровне экземпляра.

Кроме того, пулы экземпляров поддерживают встроенную интеграцию с виртуальной сетью, что позволяет развертывать несколько пулов экземпляров и несколько отдельных экземпляров в одной подсети.

## <a name="key-capabilities"></a>Ключевые возможности

Пулы экземпляров предоставляют следующие преимущества.

1. Возможность размещения 2-Виртуальное ядро экземпляров. *\* Только для экземпляров в пулах экземпляров* .
2. Время прогнозируемого и быстрого развертывания экземпляра (до 5 минут).
3. Минимальное выделение IP-адресов.

На следующей схеме показан пул экземпляров с несколькими управляемыми экземплярами, развернутыми в подсети виртуальной сети.

![пул экземпляров с несколькими экземплярами](./media/instance-pools-overview/instance-pools1.png)

Пулы экземпляров позволяют развертывать несколько экземпляров на одной виртуальной машине, размер вычислений виртуальной машины зависит от общего числа виртуальных ядер, выделенного для пула. Эта архитектура позволяет *секционировать* виртуальную машину на несколько экземпляров, что может иметь любой поддерживаемый размер, включая 2 виртуальных ядер (2-Виртуальное ядро экземпляры доступны только для экземпляров в пулах).

После первоначального развертывания операции управления экземплярами в пуле выполняются гораздо быстрее. Это связано с тем, что развертывание или расширение [виртуального кластера](connectivity-architecture-overview.md#high-level-connectivity-architecture) (выделенный набор виртуальных машин) не является частью подготовки управляемого экземпляра.

Поскольку все экземпляры в пуле совместно используют одну и ту же виртуальную машину, общее выделение IP-адресов не зависит от числа развернутых экземпляров, что удобно для развертывания в подсетях с узким диапазоном IP-адресов.

Каждый пул имеет фиксированное выделение IP-адреса только девяти IP-адресов (не включая пять IP-адресов в подсети, которые зарезервированы для собственных нужд). Дополнительные сведения см. в статье [требования к размеру подсети для отдельных экземпляров](vnet-subnet-determine-size.md).

## <a name="application-scenarios"></a>Сценарии приложений

В следующем списке приводятся основные варианты использования, в которых следует учитывать пулы экземпляров.

- Миграция *группы экземпляров SQL Server* в то же время, где большинство из них имеет меньший размер (например, 2 или 4 виртуальных ядер).
- Сценарии, в которых важно *прогнозируемое и короткое создание экземпляра или масштабирование* . Например, развертывание нового клиента в среде приложения SaaS с несколькими клиентами, для которого требуются возможности уровня экземпляра.
- Сценарии, в которых важна *фиксированная стоимость* или *предельная сумма расходов* . Например, выполнение общих сред разработки и тестирования или демонстрационной среды с фиксированным (или нечасто изменяющ меняющимся) размером, где при необходимости периодически развертываются управляемые экземпляры.
- Сценарии, в которых важно *выделение минимального количества IP-адресов* в подсети VNet. Все экземпляры в пуле совместно используют виртуальную машину, поэтому количество выделенных IP-адресов ниже, чем в случае с отдельными экземплярами.

## <a name="architecture"></a>Architecture

Пулы экземпляров имеют схожую архитектуру для обычных ( *одиночных* ) управляемых экземпляров. Для поддержки [развертываний в виртуальных сетях Azure](../../virtual-network/virtual-network-for-azure-services.md) и обеспечения изоляции и безопасности для клиентов пулы экземпляров также используют [виртуальные кластеры](connectivity-architecture-overview.md#high-level-connectivity-architecture). Виртуальные кластеры представляют выделенный набор изолированных виртуальных машин, развернутых внутри подсети виртуальной сети клиента.

Основное различие между двумя моделями развертывания состоит в том, что пулы экземпляров допускают развертывание нескольких SQL Server процессов на одном узле виртуальной машины, который управляется с помощью [объектов заданий Windows](/windows/desktop/ProcThread/job-objects), тогда как отдельные экземпляры всегда являются единственными на узле виртуальной машины.

На следующей схеме показан пул экземпляров и два отдельных экземпляра, развернутых в одной подсети, и показаны основные сведения об архитектуре для обеих моделей развертывания:

![Пул экземпляров и два отдельных экземпляра](./media/instance-pools-overview/instance-pools2.png)

Каждый пул экземпляров создает отдельный виртуальный кластер под. Экземпляры в пуле и одиночные экземпляры, развернутые в одной подсети, не используют ресурсы вычислений, выделенные для процессов SQL Server и компонентов шлюза, что обеспечивает предсказуемость производительности.

## <a name="resource-limitations"></a>Ограничения ресурсов

Относительно пулов экземпляров и экземпляров внутри пула действует несколько ограничений ресурсов.

- Пулы экземпляров доступны только на го поколения оборудовании.
- Управляемые экземпляры в пуле имеют выделенный ЦП и оперативную память, поэтому Агрегированное количество виртуальных ядер на всех экземплярах должно быть меньше или равно количеству виртуальных ядер, выделенному для пула.
- Все [ограничения уровня экземпляра](resource-limits.md#service-tier-characteristics) применяются к экземплярам, созданным в пуле.
- Помимо ограничений уровня экземпляра, на *уровне пула экземпляров* накладываются два ограничения:
  - Общий размер хранилища на пул (8 ТБ).
  - Общее число баз данных на пул (100).
- Невозможно задать администратора AAD для экземпляров, развернутых в пуле экземпляров, поэтому невозможно использовать проверку подлинности AAD.

Общий объем выделяемого хранилища и количество баз данных во всех экземплярах должны быть меньше или равны ограничениям, предоставляемым пулами экземпляров.

- Пулы экземпляров поддерживают 8, 16, 24, 32, 40, 64 и 80 виртуальных ядер.
- Управляемые экземпляры внутри пулов поддерживают 2, 4, 8, 16, 24, 32, 40, 64 и 80 виртуальных ядер.
- Управляемые экземпляры внутри пулов поддерживают размеры хранилищ от 32 ГБ до 8 ТБ, за исключением:
  - 2 экземпляра Виртуальное ядро поддерживают размеры от 32 ГБ до 640 ГБ
  - 4 экземпляра Виртуальное ядро поддерживают размеры от 32 ГБ до 2 ТБ.

[Свойство уровня службы](resource-limits.md#service-tier-characteristics) связано с ресурсом пула экземпляров, поэтому все экземпляры в пуле должны быть того же уровня служб, что и уровень служб пула. В настоящее время доступен только уровень служб общего назначения (см. следующий раздел об ограничениях в текущей предварительной версии).

### <a name="public-preview-limitations"></a>Ограничения общедоступной предварительной версии

Общедоступная Предварительная версия имеет следующие ограничения.

- В настоящее время доступен только уровень служб общего назначения.
- Нельзя масштабировать пулы экземпляров во время общедоступной предварительной версии, поэтому важно тщательно спланировать емкость перед развертыванием.
- Портал Azure поддержка создания пула экземпляров и конфигурации еще недоступна. Все операции с пулами экземпляров поддерживаются только через PowerShell. Развертывание исходного экземпляра в предварительно созданном пуле также поддерживается только с помощью PowerShell. После развертывания в пул управляемые экземпляры можно обновить с помощью портал Azure.
- Управляемые экземпляры, созданные за пределами пула, нельзя переместить в существующий пул, а экземпляры, созданные внутри пула, нельзя перемещать за пределы одного экземпляра или другого пула.
- Стоимость экземпляра [резервирования емкости](../database/reserved-capacity-overview.md) недоступна.

## <a name="sql-features-supported"></a>Поддерживаемые функции SQL

Управляемые экземпляры, созданные в пулах, поддерживают те же [уровни совместимости и функции, которые поддерживаются в одиночных управляемых экземплярах](sql-managed-instance-paas-overview.md#sql-features-supported).

Каждый управляемый экземпляр, развернутый в пуле, имеет отдельный экземпляр агента SQL.

Дополнительные функции или функции, требующие выбора конкретных значений (например, параметры сортировки уровня экземпляра, часовой пояс, общедоступная конечная точка для трафика данных, групп отработки отказа), настраиваются на уровне экземпляра и могут различаться для каждого экземпляра в пуле.

## <a name="performance-considerations"></a>Вопросы производительности

Несмотря на то, что управляемые экземпляры в пулах имеют выделенные Виртуальное ядро и ОЗУ, они совместно используют локальный диск (для использования базы данных tempdb) и сетевые ресурсы. Скорее всего, это не так, но можно столкнуться с *помехами* , если несколько экземпляров в пуле имеют высокое потребление ресурсов одновременно. Если вы следите за этим поведением, рассмотрите возможность развертывания этих экземпляров в более крупном пуле или в виде отдельных экземпляров.

## <a name="security-considerations"></a>Вопросы безопасности

Поскольку экземпляры, развернутые в пуле, совместно используют одну и ту же виртуальную машину, можно рассмотреть возможность отключения функций, которые применяют более высокие риски безопасности, или для надежного управления правами доступа к этим функциям. Например, интеграция со средой CLR, встроенное резервное копирование и восстановление, электронная почта базы данных и т. д.

## <a name="instance-pool-support-requests"></a>Запросы на поддержку пула экземпляров

Создание запросов на поддержку для пулов экземпляров в [портал Azure](https://portal.azure.com)и управление ими.

При возникновении проблем, связанных с развертыванием пула экземпляров (созданием или удалением), убедитесь, что в поле **подтип проблемы** указаны **Пулы экземпляров** .

![Запрос на поддержку пулов экземпляров](./media/instance-pools-overview/support-request.png)

При возникновении проблем, связанных с одним управляемым экземпляром или базой данных в пуле, следует создать обычный запрос в службу поддержки для Управляемый экземпляр Azure SQL.

Для создания крупных развертываний SQL Управляемый экземпляр (с пулами экземпляров или без них) может потребоваться получить более крупную региональную квоту. Дополнительные сведения см. в статье [запрос увеличения квоты для базы данных SQL Azure](../database/quota-increase-request.md). Логика развертывания для пулов экземпляров сравнивает Общее виртуальное ядро потребление *на уровне пула* с квотой, чтобы определить, разрешено ли создавать новые ресурсы без дальнейшего увеличения квоты.

## <a name="instance-pool-billing"></a>Выставление счетов за пул экземпляров

Пулы экземпляров позволяют независимо масштабировать вычислительные ресурсы и хранилища. Клиенты платят за вычислительные ресурсы, связанные с ресурсом пула, измеренным в виртуальных ядер, и хранилище, связанное с каждым экземпляром, измеренным в гигабайтах (первые 32 ГБ по бесплатному тарифу на каждый экземпляр).

плата за Виртуальное ядро за пул насчитывается независимо от того, сколько экземпляров развернуто в этом пуле.

Для цены на вычисления (измеряется в виртуальных ядер) доступны два варианта ценообразования:

  1. *Включенная лицензия* : Цена SQL Server лицензий включена. Это относится к клиентам, которые не применяют существующие лицензии SQL Server с Software Assurance.
  2. *Преимущество гибридного использования Azure* : сниженная цена, которая включает Преимущество гибридного использования Azure для SQL Server. Клиенты могут принять участие в этой цене, используя существующие лицензии на SQL Server с Software Assurance. Сведения о допустимости и других деталях см. в разделе [преимущество гибридного использования Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

Установка различных параметров ценообразования невозможна для отдельных экземпляров в пуле. Все экземпляры в родительском пуле должны быть либо по цене, либо по прейскуранту, либо по цене Преимущество гибридного использования Azure. Модель лицензии для пула можно изменить после создания пула.

> [!IMPORTANT]
> При указании модели лицензии для экземпляра, который отличается от значения в пуле, используется цена пула, а значение уровня экземпляра игнорируется.

Если вы создаете пулы экземпляров в [подписках, доступных для преимуществ разработки и тестирования](https://azure.microsoft.com/pricing/dev-test/), вы автоматически получаете скидку со скидками до 55% на управляемый экземпляр Azure SQL.

Полные сведения о ценах на пул экземпляров см. в разделе *Пулы экземпляров* на [странице цен на SQL управляемый экземпляр](https://azure.microsoft.com/pricing/details/sql-database/managed/).

## <a name="next-steps"></a>Дальнейшие действия

- Чтобы приступить к работе с пулами экземпляров, см. [руководство по использованию пулов SQL управляемый экземпляр](instance-pools-configure.md).
- Сведения о создании первого управляемого экземпляра см. в [этом руководстве по началу работы](instance-create-quickstart.md).
- Список функций и сравнительных списков см. в статье [Общие функции SQL Azure](../database/features-comparison.md).
- Дополнительные сведения о конфигурации виртуальной сети см. в разделе [Архитектура подключения к Управляемому экземпляру SQL Azure](connectivity-architecture-overview.md).
- Сведения о создании управляемого экземпляра и восстановлении базы данных из файла резервной копии см. в разделе [Краткое руководство. Создание управляемого экземпляра Управляемого экземпляра SQL](instance-create-quickstart.md).
- Сведения о миграции с помощью Azure Database Migration Service см. в разделе [Руководство. Миграция из SQL Server в Управляемый экземпляр базы данных SQL Azure в автономном режиме с помощью DMS](../../dms/tutorial-sql-server-to-managed-instance.md).
- Сведения о расширенном мониторинге производительности базы данных Управляемого экземпляра SQL с использованием встроенных средств анализа проблем см. в статье о [мониторинге Управляемого экземпляра SQL Azure с помощью Аналитики SQL Azure](../../azure-monitor/insights/azure-sql.md).
- Сведения о ценах см. на странице [цен на SQL управляемый экземпляр](https://azure.microsoft.com/pricing/details/sql-database/managed/).