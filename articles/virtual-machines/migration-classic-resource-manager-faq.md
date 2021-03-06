---
title: Часто задаваемые вопросы о миграции из классической модели в модель Azure Resource Manager
description: Часто задаваемые вопросы о миграции из классической модели в модель Azure Resource Manager
author: tanmaygore
manager: vashan
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: tagore
ms.openlocfilehash: ccbf7e8aaab5463abf8fa9e129ba9f0ea8599c4e
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "91969846"
---
# <a name="frequently-asked-questions-about-classic-to-azure-resource-manager-migration"></a>Часто задаваемые вопросы о миграции из классической модели в модель Azure Resource Manager

> [!IMPORTANT]
> Сегодня около 90% виртуальных машин IaaS используют [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/). По состоянию на 28 февраля 2020, классические виртуальные машины являются устаревшими и будут полностью сняты с 1 марта 2023. [Узнайте больше]( https://aka.ms/classicvmretirement) об этой нерекомендуемости и [о том, как она влияет на вас](./classic-vm-deprecation.md#how-does-this-affect-me).

## <a name="what-is-azure-service-manager-and-what-does-it-mean-by-classic"></a>Что такое Service Manager Azure и что это значит на классической модели?

Слово "Классическая" в виртуальной машине IaaS (классическая модель) относится к виртуальным машинам, управляемым Azure Service Manager (ASM). Azure Service Manager (ASM) — это старая плоскость управления Azure, отвечающая за создание, управление и удаление виртуальных машин, а также выполнение других операций плоскости управления. 

## <a name="what-is-azure-resource-manager"></a>Azure Resource Manager

[Azure Resource Manager](../azure-resource-manager/management/overview.md) — это последняя плоскость управления Azure, отвечающая за создание, управление и удаление виртуальных машин, а также выполнение других операций плоскости управления. 

## <a name="what-is-the-time-required-for-migration"></a>Сколько времени потребуется для миграции?

Планирование и выполнение миграции сильно зависит от сложности архитектуры и может занять несколько месяцев.  

## <a name="what-is-the-definition-of-a-new-customer-on-iaas-vms-classic"></a>Кто считается новым клиентом для виртуальных машин IaaS (классических)?

Клиенты, у которых нет виртуальных машин IaaS (классические) в своих подписках за Февраль 2020 (месяц до начала срока действия), считаются новыми клиентами. 

## <a name="what-is-the-definition-of-an-existing-customer-on-iaas-virtual-machines-classic"></a>Кто считается существующим клиентом для виртуальных машин IaaS (классических)?

Клиенты, у которых в подписке были активные или остановленные, но выделенные виртуальные машины IaaS (классические) в течение февраля 2020 года, считаются существующими клиентами. Только этим клиентам предоставляется отсрочка до 1 марта 2023 года для переноса виртуальных машин из Azure Service Manager в Azure Resource Manager. 

## <a name="why-am-i-getting-an-error-stating-newclassicvmcreationnotallowedforsubscription"></a>Почему я получаю сообщение об ошибке "NewClassicVMCreationNotAllowedForSubscription"?

По условиям процесса прекращения поддержки виртуальная машина IaaS (классическая) больше не доступна для новых клиентов. Мы считаем вас новым клиентом, поэтому ваша операция не разрешается. Мы настоятельно рекомендуем использовать [Виртуальные машины Azure на основе ARM](./windows/quick-create-powershell.md). Если вы не можете использовать виртуальные машины Azure с помощью ARM, обратитесь в службу поддержки, чтобы добавить подписку в список разрешений.

## <a name="does-this-migration-plan-affect-any-of-my-existing-services-or-applications-that-run-on-azure-virtual-machines"></a>Влияет ли этот план миграции на существующие службы или приложения, которые выполняются на виртуальных машинах Azure? 

В период до 1 марта 2023 г. это никак не повлияет на виртуальные машины IaaS (классические). Виртуальные машины IaaS (классические) полностью поддерживаются в общедоступной версии. Можно продолжать использовать эти ресурсы, чтобы расширить занимаемый объем в Microsoft Azure. 1 марта 2023 года будет полностью прекращено использование таких виртуальных машин, а все сохранившиеся активные или выделенные виртуальные машины будут остановлены и освобождены. Это никак не затронет другие классические ресурсы, в том числе Облачные службы (классические), учетные записи хранения (классические) и т. д.   

## <a name="what-happens-to-my-vms-if-i-dont-plan-on-migrating-in-the-near-future"></a>Что произойдет с моей виртуальной машиной, если я не планирую перенос в ближайшем будущем? 

1 марта 2023 года будет полностью прекращено использование виртуальных машин IaaS (классических), а все сохранившиеся активные или выделенные виртуальные машины будут остановлены и освобождены. Чтобы это не повлияло на ваш бизнес, мы настоятельно рекомендуем начать планирование миграции уже сегодня и завершить миграцию не позднее 1 марта 2023 г. Мы не прекращаем поддержку существующих классических интерфейсов API, Облачных служб и модели ресурсов. Мы хотим максимально упростить миграцию с помощью расширенных функций, доступных в модели развертывания с помощью Resource Manager. Мы рекомендуем немедленно приступить к планированию миграции этих ресурсов в Azure Resource Manager. 

## <a name="what-does-this-migration-plan-mean-for-my-existing-tooling"></a>Как изменятся существующие средства с этим планом миграции? 

Одним из важнейших изменений, которое следует учесть в плане миграции, является обновление набора инструментов до модели развертывания Resource Manager.

## <a name="how-long-will-the-management-plane-downtime-be"></a>Сколько времени продлится простой плоскости управления? 

Это зависит от числа переносимых ресурсов. В случае развертываний небольшого размера (с несколькими десятками виртуальных машин) вся миграция должна занимать не больше часа. В случае крупномасштабных развертываний (с сотнями виртуальных машин) миграция может длиться несколько часов.

## <a name="can-i-roll-back-after-my-migrating-resources-are-committed-in-resource-manager"></a>Можно ли выполнить откат после фиксации переносимых ресурсов в Resource Manager? 

Миграцию можно прервать, пока ресурсы находятся в состоянии подготовки. Откат после миграции ресурсов с фиксацией не поддерживается.

## <a name="can-i-roll-back-my-migration-if-the-commit-operation-fails"></a>Можно ли откатить миграцию при сбое фиксации? 

Прервать миграцию при сбое фиксации нельзя. Все операции миграции, включая фиксацию, являются идемпотентными. Поэтому рекомендуется повторить операцию спустя некоторое время. Если вы по-прежнему видите ошибку, создайте запрос в службу поддержки.

## <a name="do-i-have-to-buy-another-express-route-circuit-if-i-have-to-use-iaas-under-resource-manager"></a>Нужно ли приобретать еще один канал ExpressRoute, если нужно использовать ресурсы IaaS в модели Resource Manager? 

Нет. Мы недавно включили [перемещение каналов ExpressRoute из классической модели развертывания в модель развертывания с помощью Resource Manager](../expressroute/expressroute-move.md). Поэтому при наличии существующего канала ExpressRoute не нужно приобретать еще один.

## <a name="what-if-i-had-configured-role-based-access-control-policies-for-my-classic-iaas-resources"></a>Что произойдет, если для классических ресурсов IaaS настроено управление доступом на основе ролей? 

Во время миграции ресурсы преобразуются из классической модели в модель Resource Manager. Поэтому рекомендуется спланировать обновления политики RBAC, которые следует применить после миграции.

## <a name="i-backed-up-my-classic-vms-in-a-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Резервные копии классических виртуальных машин находятся в хранилище. Можно ли переключить виртуальные машины из классического режима в режим Resource Manager и защитить их в хранилище служб восстановления?

При переключении виртуальной машины из классического режима в режим Resource Manager резервные копии, сделанные до переноса, не переносятся в перемещенную виртуальную машину Resource Manager. Тем не менее, если вы хотите хранить резервные копии классических виртуальных машин, выполните указанные ниже действия до переноса. 

1. В хранилище служб восстановления на вкладке **Защищенные элементы** выберите нужную виртуальную машину. 
2. Щелкните "Остановить защиту". *Снимите* флажок **Удаление связанных архивируемых данных**.

> [!NOTE]
> Плата за экземпляр резервной копии взимается, пока вы храните данные. Резервные копии удаляются в соответствии с диапазоном хранения. Тем не менее последняя резервная копия хранится, пока вы явно не удалите данные резервных копий. Мы советуем проверить диапазон хранения виртуальной машины и триггер удаления резервных копий защищенного элемента в хранилище после окончания диапазона хранения. 
>
>

Чтобы перенести виртуальную машину в режим Resource Manager, сделайте следующее: 

1. Удалите расширение резервного копирования или создания моментальных снимков с виртуальной машины.
2. Теперь эту виртуальную машину можно перевести из классического режима в режим Resource Manager. Сведения о сети и хранилище, связанные с виртуальной машиной, также следует перенести в режим Resource Manager.

Кроме того, если вы хотите создать резервную копию перенесенной виртуальной машины, перейдите к колонке управления виртуальной машины, чтобы [включить резервное копирование](../backup/quick-backup-vm-portal.md#enable-backup-on-a-vm).

## <a name="can-i-validate-my-subscription-or-resources-to-see-if-theyre-capable-of-migration"></a>Можно ли проверить подписку или ресурсы, чтобы узнать, подходят ли они для миграции? 

Да. На первом этапе подготовки к поддерживаемой платформой миграции проверяется, можно ли перенести ресурсы. В случае сбоя операции проверки вы получите сообщения о всех причинах, из-за которых невозможно выполнить миграцию.

## <a name="what-happens-if-i-run-into-a-quota-error-while-preparing-the-iaas-resources-for-migration"></a>Что произойдет, если при подготовке ресурсов IaaS к миграции произойдет ошибка, связанная с квотой? 

Рекомендуется прервать миграцию и отправить в службу поддержки запрос на увеличение квот в регионе, в который переносятся виртуальные машины. После утверждения запроса на увеличение квоты начните этапы миграции заново.

## <a name="how-do-i-report-an-issue"></a>Как сообщить о проблеме? 

Сведения о проблемах и вопросы, связанные с переносом, публикуйте на [странице вопросов и ответов о виртуальных машинах на сайте Майкрософт](/answers/topics/azure-virtual-machines.html), указывая ключевое слово ClassicIaaSMigration. Рекомендуется публиковать на этом форуме все свои вопросы. Если у вас есть контракт на техническую поддержку, то вы всегда можете зарегистрировать запрос в службу поддержки.

## <a name="what-if-i-dont-like-the-names-of-the-resources-that-the-platform-chose-during-migration"></a>Что делать, если мне не нравятся имена, которые платформа выбрала для ресурсов во время миграции? 

Имена всех ресурсов, которые указаны явным образом в классической модели развертывания, при миграции будут сохранены. В некоторых случаях будут создаваться новые ресурсы. Например, для каждой виртуальной машины создается сетевой интерфейс. В данный момент возможность управления именами этих новых ресурсов, создаваемых во время миграции, не поддерживается. Проголосуйте за эту функцию на [форуме отзывов об Azure](https://feedback.azure.com).

## <a name="can-i-migrate-expressroute-circuits-used-across-subscriptions-with-authorization-links"></a>Можно ли перенести каналы ExpressRoute, используемые в подписках со ссылками авторизации? 

Каналы ExpressRoute, использующие ссылки авторизации между подписками, невозможно перенести автоматически без простоев. У нас есть рекомендации о том, как их можно перенести вручную. Дополнительные сведения см. в статье [Перенос каналов ExpressRoute и связанных виртуальных сетей из классической модели развертывания на модель Resource Manager](../expressroute/expressroute-migration-classic-resource-manager.md).

## <a name="i-got-the-message-vm-is-reporting-the-overall-agent-status-as-not-ready-hence-the-vm-cannot-be-migrated-ensure-that-the-vm-agent-is-reporting-overall-agent-status-as-ready-or-vm-contains-extension-whose-status-is-not-being-reported-from-the-vm-hence-this-vm-cannot-be-migrated"></a>Получено сообщение *"VM is reporting the overall agent status as Not Ready. Hence, the VM cannot be migrated. Добейтесь того, чтобы сообщаемое агентом виртуальной машины общее состояние имело значение "Готов"."* или *"VM contains Extension whose Status is not being reported from the VM. Hence, this VM cannot be migrated"* (Виртуальная машина содержит расширение, о состоянии которого она не сообщает. Поэтому виртуальную машину нельзя перенести).

Это сообщение отображается в том случае, когда у виртуальной машины нет исходящего подключения к Интернету. Агент виртуальной машины использует исходящее подключение для обращения к учетной записи хранения Azure, чтобы обновлять состояние агента каждые пять минут.


## <a name="next-steps"></a>Дальнейшие действия

Для Linux:

* [Обзор поддерживаемого платформой переноса ресурсов IaaS из классической модели в модель Azure Resource Manager](./linux/migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Техническое руководство по поддерживаемому платформой переносу из классической модели в модель Azure Resource Manager](./migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Planning for migration of IaaS resources from classic to Azure Resource Manager](./linux/migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Планирование переноса ресурсов IaaS из классической модели в модель Azure Resource Manager)
* [Перенос ресурсов IaaS из классической модели в модель Azure Resource Manager с помощью Azure PowerShell](./windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Перенос ресурсов IaaS из классической модели в модель Azure Resource Manager с помощью интерфейса командной строки Azure](./linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Community tools to migrate IaaS resources from classic to Azure Resource Manager](./windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Инструменты сообщества для перемещения ресурсов IaaS из классической модели в модель Azure Resource Manager)
* [Распространенные ошибки миграции](./linux/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Для Windows:

* [Обзор поддерживаемого платформой переноса ресурсов IaaS из классической модели в модель Azure Resource Manager](./windows/migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Техническое руководство по поддерживаемому платформой переносу из классической модели в модель Azure Resource Manager](./migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Planning for migration of IaaS resources from classic to Azure Resource Manager](./windows/migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Планирование переноса ресурсов IaaS из классической модели в модель Azure Resource Manager)
* [Перенос ресурсов IaaS из классической модели в модель Azure Resource Manager с помощью Azure PowerShell](./windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Перенос ресурсов IaaS из классической модели в модель Azure Resource Manager с помощью интерфейса командной строки Azure](./linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Community tools to migrate IaaS resources from classic to Azure Resource Manager](./windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Инструменты сообщества для перемещения ресурсов IaaS из классической модели в модель Azure Resource Manager)
* [Распространенные ошибки миграции](./windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)