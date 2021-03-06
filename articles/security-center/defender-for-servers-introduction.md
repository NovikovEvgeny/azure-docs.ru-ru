---
title: Преимущества и функции Azure Defender для серверов
description: Сведения о преимуществах и функциях Azure Defender для серверов.
author: memildin
ms.author: memildin
ms.date: 9/23/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 711963a60d5c75031ff676a9c7f1db47f20fe895
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2020
ms.locfileid: "92275246"
---
# <a name="introduction-to-azure-defender-for-servers"></a>Общие сведения об Azure Defender для серверов

Azure Defender для серверов предоставляет функции обнаружения угроз и расширенной защиты на компьютерах Windows и Linux.

Azure Defender для Windows интегрируется со службами Azure для мониторинга и защиты компьютеров под управлением Windows. Он предоставляет оповещения и рекомендации по исправлению изо всех этих служб в удобном для использования формате.

Azure Defender для Linux собирает записи аудита с компьютеров Linux, используя **auditd**  — одну из наиболее распространенных платформ аудита в Linux. Решение auditd размещено в магистральном ядре. 


## <a name="what-are-the-benefits-of-azure-defender-for-servers"></a>Каковы преимущества Azure Defender для серверов?

Azure Defender для серверов предоставляет следующие возможности обнаружения угроз и защиты:

- **Интегрированная лицензия для Microsoft Defender для конечной точки (только для Windows).** Azure Defender для серверов включает [Microsoft Defender для конечной точки](https://www.microsoft.com/microsoft-365/security/endpoint-defender). Вместе они обеспечивают широкие возможности обнаружения конечных точек и реагирования. [Дополнительные сведения](security-center-wdatp.md).

    Когда Defender для конечной точки обнаруживает угрозу, активируется оповещение. Оповещение отображается в Центре безопасности. Из Центра безопасности вы можете перейти к консоли Defender для конечной точки, отобразить сводные данные и тщательно их изучить для определения области атаки. Ознакомьтесь с дополнительными сведениями о Microsoft Defender для конечной точки.

    > [!IMPORTANT]
    > Датчик **Microsoft Defender для конечной точки** автоматически включается для серверов Windows, которые используют Центр безопасности.

- **Проверка с оценкой уязвимостей для виртуальных машин.** Средство проверки уязвимостей, которое входит в состав Центра безопасности Azure, разработано компанией Qualys. 

    Средство проверки от Qualys — это один из ведущих инструментов для идентификации уязвимостей на виртуальных машинах Azure в режиме реального времени. Вам не потребуется лицензия или учетная запись Qualys: вся обработка выполняется в Центре безопасности. [Дополнительные сведения](deploy-vulnerability-assessment-vm.md).

- **JIT-доступ к виртуальным машинам.** Злоумышленники активно ищут доступные компьютеры с открытыми портами управления, например RDP или SSH. Все ваши виртуальные машины являются потенциальными мишенями для атак. Если злоумышленники получают доступ к виртуальной машине, они используют ее в качестве точки входа для атак на другие ресурсы в вашей среде.

    Если вы включили Azure Defender для серверов, вы можете использовать JIT-доступ к виртуальной машине, чтобы блокировать входящий трафик к виртуальным машинам, снизить уязвимость к атакам и обеспечить удобный доступ к виртуальным машинам, когда это необходимо. [Дополнительные сведения](just-in-time-explained.md).

- **Мониторинг целостности файлов (FIM).** Эта функция, которая также называется мониторингом изменений, анализирует файлы и реестры в операционной системе, приложениях и других компонентах, чтобы обнаруживать потенциальные атаки. Метод сравнения позволяет определить, отличается ли текущее состояние файла от состояния на момент последней проверки. Такое сравнение позволяет узнать, вносились ли в файлы допустимые или подозрительные изменения.

    Включив Azure Defender для серверов, вы можете использовать функцию FIM для проверки целостности файлов Windows и Linux, а также реестров Windows. [Дополнительные сведения](security-center-file-integrity-monitoring.md).

- **Адаптивные элементы управления приложениями (AAC).** Это интеллектуальное и автоматизированное решение для определения списка разрешений с известными приложениями для компьютеров.

    Если вы включите и настроите адаптивные элементы управления приложениями, вы сможете получать оповещения безопасности при запуске в системе приложений, которые не были определены вами как безопасные. [Дополнительные сведения](security-center-adaptive-application.md).

- **Адаптивная защита сети (ANH).** Использование групп безопасности сети (NSG) позволяет фильтровать входящий и исходящий трафик ресурсов и улучшает состояние сетевой безопасности. Но в некоторых случаях фактический трафик, проходящий через группу безопасности сети, является подмножеством определенных правил NSG. Чтобы улучшить состояние безопасности в таком случае, можно ужесточить правила NSG с учетом фактического распределения трафика.

    Функция адаптивной защиты сети предоставляет рекомендации по дополнительному ужесточению правил групп безопасности сети. Она использует алгоритм машинного обучения, который учитывает фактический трафик, известную доверенную конфигурацию, аналитику угроз и другие показатели компрометации, а затем предоставляет рекомендации по разрешению трафика только от определенных IP-адресов или кортежей портов. [Дополнительные сведения](security-center-adaptive-network-hardening.md).

- **Защита узлов Docker.** Центр безопасности Azure выявляет неуправляемые контейнеры, размещенные на виртуальных машинах IaaS Linux или других компьютерах Linux с контейнерами Docker. Центр безопасности постоянно оценивает конфигурации этих контейнеров. Затем он сравнивает их с эталонным контейнером Docker для Центра Интернет-безопасности (CIS). Центр безопасности применяет весь набор правил эталонного контейнера Docker для CIS и выдает предупреждения, если контейнеры не соответствуют любому из этих элементов управления. [Дополнительные сведения](harden-docker-hosts.md).

- **Обнаружение бесфайловых атак (только для Windows).** При бесфайловых атаках вредоносный атакующий код внедряется в память, чтобы избежать обнаружения инструментами проверки дисков. Атакующий код сохраняется в памяти скомпрометированных процессов и выполняет широкий спектр вредоносных действий.

  При обнаружении атак такого рода используются автоматизированные средства криминалистического анализа, которые позволяют выявить наборы средств, методики и поведение, применяемые для осуществления бесфайловых атак. Это решение периодически проверяет компьютер во время выполнения и извлекает аналитические сведения непосредственно из памяти процессов. Такие аналитические сведения включают данные об идентификации: 

  - хорошо известных наборов средств и программного обеспечения для майнинга криптовалют; 

  - кода оболочки, который представляет собой небольшой фрагмент кода, обычно используемый в качестве атакующего кода для обнаружения уязвимости программного обеспечения;

  - внедренных вредоносных исполняемых файлов в память процесса.

  Функция обнаружения бесфайловых атак создает подробные оповещения безопасности, содержащие описание с дополнительными метаданными процесса, например о сетевой активности. Это ускоряет рассмотрение оповещений, корреляцию и ускоряет реагирование. Такой подход дополняет решения EDR на основе событий, а также расширяет охват при обнаружении.

  Дополнительные сведения об оповещениях об обнаружении бесфайловых атак см. в [справочной таблице оповещений](alerts-reference.md#alerts-windows).

- **Интеграция оповещений auditd в Linux с агентом Log Analytics (только для Linux).** . Система auditd включает подсистему уровня ядра, которая отвечает за мониторинг системных вызовов. Она фильтрует их по указанному набору правил и записывает для них сообщения в сокет. Центр безопасности интегрирует функциональные возможности из пакета auditd с агентом Log Analytics. Эта интеграция обеспечивает сбор событий аудита во всех поддерживаемых дистрибутивах Linux без каких бы то ни было обязательных компонентов.

    Записи auditd собираются, обогащаются и объединяются в события с помощью агента Log Analytics для агента Linux. В Центр безопасности постоянно добавляется новая аналитика, которая использует сигналы Linux для обнаружения вредоносного поведения на облачных и локальных компьютерах с Linux. Как и в случае с Windows, эти аналитические данные охватывают все подозрительные процессы, сомнительные попытки входа, загрузку модуля ядра и другие действия. Эти действия могут указывать на то, что компьютер атакуют злоумышленники либо он взломан.  

    Список оповещений Linux см. в [справочной таблице оповещений](alerts-reference.md#alerts-linux).


## <a name="simulating-alerts"></a>Имитация оповещений

Вы можете имитировать оповещения, скачав один из следующих сборников схем:

- Для Windows: [Azure Security Center Playbook: Security Alerts](https://github.com/Azure/Azure-Security-Center/blob/master/Simulations/Azure%20Security%20Center%20Security%20Alerts%20Playbook_v2.pdf) (Сборник схем для центра безопасности Azure: оповещения безопасности)

- Для Linux: [Azure Security Center Playbook: по обнаружению в Linux](https://github.com/Azure/Azure-Security-Center/blob/master/Simulations/Azure%20Security%20Center%20Linux%20Detections_v2.pdf).




## <a name="next-steps"></a>Дальнейшие действия

Из этой статьи вы узнали об Azure Defender для серверов. 

Связанные материалы см. в следующих статьях: 

- Вы можете экспортировать оповещение, созданное Центром безопасности или полученное им от другого продукта для обеспечения безопасности. Чтобы экспортировать оповещения в Azure Sentinel, любое стороннее средство SIEM или другие внешние средства, следуйте инструкциям в статье [Экспорт оповещений о безопасности и рекомендаций](continuous-export.md).

- > [!div class="nextstepaction"]
    > [Включение Azure Defender](security-center-pricing.md)