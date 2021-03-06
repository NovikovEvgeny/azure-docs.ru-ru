---
title: включить файл
titleSuffix: Azure
description: включить файл
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: dbaa0b5fc87cb5393b323b8a9b7a38b72efe9518
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "81680786"
---
Командлет PowerShell **Get-азпиринглокатион** возвращает список расположений пиринга с обязательным параметром `Kind` , который вы будете использовать в последующих шагах.

```powershell
Get-AzPeeringLocation -Kind Direct
```

Расположения с прямым пирингм содержат следующие поля:
* пиринглокатион 
* Страна или регион
* пирингдбфаЦилитид
* пирингдбфаЦилитилинк
* бандвидсофферс

Проверьте наличие нужного средства пиринга, обратившись к [пирингдб](https://wwww.peeringdb.com).

В этом примере показано, как использовать Сиэтле в качестве расположения пиринга для создания прямого пиринга.

```powershell
$peeringLocations = Get-AzPeeringLocation -Kind Direct
$peeringLocation = $peeringLocations | where {$_.PeeringLocation -contains "Seattle"}
$peeringLocation

PeeringLocation       : Seattle
Address               : 2001 Sixth Avenue
Country               : US
PeeringDBFacilityId   : 71
PeeringDBFacilityLink : https://www.peeringdb.com/fac/71
BandwidthOffers       : {10Gbps, 100Gbps}
```