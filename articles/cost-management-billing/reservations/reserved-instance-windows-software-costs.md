---
title: Затраты на резервирование программного обеспечения для Azure
description: Узнайте, какие счетчики программного обеспечения не учитываются в затратах на зарезервированные экземпляры виртуальных машин Azure.
author: yashesvi
ms.reviewer: yashar
tags: billing
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: banders
ms.openlocfilehash: e302a8459d3092a5543efda7494c68d6660df39d
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2020
ms.locfileid: "88690822"
---
# <a name="software-costs-not-included-with-azure-reserved-vm-instances"></a>Затраты на программное обеспечение, которые не включены в стоимость экземпляров Azure Reserved Virtual Machine Instances

Скидки на зарезервированные экземпляры виртуальных машин и резервную мощность SQL применяются только к расходам на инфраструктуру, а не на программное обеспечение. Если вы используете виртуальную машину Windows и не применяете преимущество гибридного использования Azure для зарезервированных экземпляров виртуальных машин, плата будет взиматься за единицы измерения программного обеспечения Windows (см. раздел ниже). Для развертываний PaaS SQL плата за использование IP-адресов будет все также взиматься по отдельным единицам измерения, если не выбрано преимущество гибридного использования Azure.

## <a name="windows-software-meters-not-included-in-reservation-cost"></a>Единицы измерения программного обеспечения Windows, не учитываемые в затратах на резервирование

| Значение MeterId | Значение MeterName в файле использования | Какой виртуальной машиной используется |
| ------- | ------------------------| --- |
| e7e152ac-f29c-4cce-ad6e-026192c01ef2 | Зарезервированные экземпляры с Windows Server (1 ядро) — пакетная передача | Серия B |
| cac255a2-9f0f-4c62-8bd6-f0fa449c5f76 | Зарезервированные экземпляры с Windows Server (2 ядра) — пакетная передача | Серия B |
| 09756b58-3fb5-4390-976d-9ddd14f9ed18 | Зарезервированные экземпляры с Windows Server (4 ядра) — пакетная передача | Серия B |
| e828cb37-5920-4dc7-b30f-664e4dbcb6c7 | Зарезервированные экземпляры с Windows Server (8 ядер) — пакетная передача | Серия B |
| f65a06cf-c9c3-47a2-8104-f17a8542215a | Зарезервированные экземпляры с Windows Server (1 ядро) | Все, кроме серии B |
| b99d40ae-41fe-4d1d-842b-56d72f3d15ee | Зарезервированные экземпляры с Windows Server (2 ядра) | Все, кроме серии B |
| 1cb88381-0905-4843-9ba2-7914066aabe5 | Зарезервированные экземпляры с Windows Server (4 ядра) | Все, кроме серии B |
| 07d9e10d-3e3e-4672-ac30-87f58ec4b00a | Зарезервированные экземпляры с Windows Server (6 ядер) | Все, кроме серии B |
| 603f58d1-1e96-460b-a933-ce3775ac7e2e | Зарезервированные экземпляры с Windows Server (8 ядер) | Все, кроме серии B |
| 36aaadda-da86-484a-b465-c8b5ab292d71 | Зарезервированные экземпляры с Windows Server (12 ядер) | Все, кроме серии B |
| 02968a6b-1654-4495-ada6-13f378ba7172 | Зарезервированные экземпляры с Windows Server (16 ядер) | Все, кроме серии B |
| 175434d8-75f9-474b-9906-5d151b6bed84 | Зарезервированные экземпляры с Windows Server (20 ядер) | Все, кроме серии B |
| 77eb6dd0-88f5-4a16-ab39-05d1742efb25 | Зарезервированные экземпляры с Windows Server (24 ядра) | Все, кроме серии B |
| 0d5bdf46-b719-4b1f-a780-b9bdfffd0591 | Зарезервированные экземпляры с Windows Server (32 ядра) | Все, кроме серии B |
| f1214b5c-cc16-445f-be6c-a3bb75f8395a | Зарезервированные экземпляры с Windows Server (40 ядер) | Все, кроме серии B |
| 637b7c77-65ad-4486-9cc7-dc7b3e9a8731 | Зарезервированные экземпляры с Windows Server (64 ядра) | Все, кроме серии B |
| da612742-e7cc-4ca3-9334-0fb7234059cd | Зарезервированные экземпляры с Windows Server (72 ядра) | Все, кроме серии B |
| a485cb8c-069b-4cf3-9a8e-ddd84b323da2 | Зарезервированные экземпляры с Windows Server (128 ядер) | Все, кроме серии B |
| 904c5c71-1eb7-43a6-961c-d305a9681624 | Зарезервированные экземпляры с Windows Server (256 ядер) | Все, кроме серии B |
| 6fdab81b-4284-4df9-8939-c237cc7462fe | Зарезервированные экземпляры с Windows Server (96 ядер) | Все, кроме серии B |

## <a name="cloud-services-software-meters-not-included-in-reservation-cost"></a>Единицы измерения программного обеспечения Windows, не учитываемые в затратах на резервирование

| Значение MeterId | Значение MeterName в файле использования |
| ------- | ------------------------|
|ac9d47ff-ff68-4afc-a145-0c321cf8d0d5|Лицензия на использование 1 виртуального ЦП облачных служб|
|e0434559-19ee-4132-9c46-05ad4044f3f7|Лицензия на использование 2 виртуальных ЦП облачных служб|
|6ecc834e-39b3-48b3-8d10-cc5626bacb66|Лицензия на использование 4 виртуальных ЦП облачных служб|
|13103090-ca72-4825-ab12-7f16c4931d95|Лицензия на использование 8 виртуальных ЦП облачных служб|
|ecd2bb6e-45a5-49aa-a58b-3947ba21c364|Лицензия на использование 16 виртуальных ЦП облачных служб|
|de2c7f1d-06dc-4b16-bc8b-c2ec5f4c8aee|Лицензия на использование 20 виртуальных ЦП облачных служб|
|ca1af837-4b35-47f5-8d14-b1988149c4ca|Лицензия на использование 32 виртуальных ЦП облачных служб|
|dc72ee45-2ab7-4698-b435-e2cf10d1f9f6|Лицензия на использование 64 виртуальных ЦП облачных служб|
|7a803026-244c-4659-834c-11e6b2d6b76f|Лицензия на использование 80 виртуальных ЦП облачных служб|

## <a name="get-rates-for-azure-meters"></a>Получение ставок для единиц измерения Azure

Стоимость каждой из этих единиц измерения можно узнать с помощью API Azure RateCard. Сведения о том, как узнать тарифы на единицы измерения в Azure, см. в статье [Get price and metadata information for resources used in an Azure subscription](/previous-versions/azure/reference/mt219004(v=azure.100)) (Получение сведений о ценах и метаданных для ресурсов, используемых в подписке Azure).

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о резервировании для Azure см. в следующих статьях.

- [Сведения о резервированиях для Azure](save-compute-costs-reservations.md)
- [Предоплата виртуальных машин с помощью Azure Reserved Virtual Machine Instances](../../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Управление резервированиями для Azure](manage-reserved-vm-instance.md)
- [Сведения о применении скидки на зарезервированный экземпляр виртуальной машины](../manage/understand-vm-reservation-charges.md)
- [Общие сведения об использовании резервирования Azure для подписки с оплатой по мере использования](understand-reserved-instance-usage.md)
- [Общие сведения об использовании зарезервированных экземпляров Azure с Соглашением о регистрации Enterprise](understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами

Если у вас есть вопросы или вам нужна помощь, [создайте запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).
