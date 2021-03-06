---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 10/26/2020
ms.author: larryfr
ms.openlocfilehash: 1fde2947719079e411bc233bcce426feec8fa996
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "92631055"
---
> [!IMPORTANT]
> Cosmos DB экземпляр создается в группе ресурсов, управляемой корпорацией Майкрософт, в __вашей подписке__ вместе с любыми нужными ресурсами. Это означает, что вы платите за этот экземпляр Cosmos DB. Имя управляемой группы ресурсов указано в формате `<AML Workspace Resource Group Name><GUID>`. Если Рабочая область Машинное обучение Azure использует закрытую конечную точку, для экземпляра Cosmos DB также создается виртуальная сеть. Эта виртуальная сеть используется для защиты обмена данными между Cosmos DB и Машинное обучение Azure.
> 
> * __Не удаляйте группу ресурсов__ , содержащую этот Cosmos DB экземпляр, или любые ресурсы, автоматически созданные в этой группе. Если необходимо удалить группу ресурсов, Cosmos DB экземпляр и т. д., необходимо удалить рабочую область Машинное обучение Azure, которая его использует. Группа ресурсов, Cosmos DB экземпляр и другие автоматически созданные ресурсы удаляются при удалении связанной рабочей области.
> * По умолчанию [__единицы запросов__](../articles/cosmos-db/request-units.md) для этой учетной записи Cosmos DB устанавливаются в количестве  __8000__ . __Изменение этого значения не поддерживается__ .
> * Вы __не можете предоставить собственную виртуальную сеть для использования с созданным экземпляром Cosmos DB__ . Вы также __не можете изменить виртуальную сеть__ . Например, нельзя изменить используемый диапазон IP-адресов.