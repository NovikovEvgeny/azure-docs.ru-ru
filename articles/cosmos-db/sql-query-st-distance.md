---
title: ST_DISTANCE языка запросов Azure Cosmos DB
description: Сведения о ST_DISTANCE системных функций SQL в Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9d486f5c9a4ccc1737a051762f64bc712192df44
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2020
ms.locfileid: "93335285"
---
# <a name="st_distance-azure-cosmos-db"></a>ST_DISTANCE (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Возвращает расстояние между двумя геоточечными выражениями, многоугольниками, несколькими многоугольниками и LineString. Дополнительные сведения см. в статье [сведения о геопространственном и географическом расположении данных расположения](sql-query-geospatial-intro.md) .
  
## <a name="syntax"></a>Синтаксис
  
```sql
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Аргументы
  
*spatial_expr*  
   Любое допустимое выражение объекта GeoJSON (точка, многоугольник или LineString).  
  
## <a name="return-types"></a>Типы возвращаемых данных
  
  Возвращает числовое выражение, указывающее расстояние. При использовании эталонной системы по умолчанию это значение указывается в метрах.  
  
## <a name="examples"></a>Примеры
  
  В следующем примере показано, как вернуть все документы семейства, которые находятся в пределах 30 км из указанного расположения, используя `ST_DISTANCE` встроенную функцию. .  
  
```sql
SELECT f.id
FROM Families f
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 Результирующий набор:  
  
```json
[{  
  "id": "WakefieldFamily"  
}]  
```

## <a name="remarks"></a>Remarks

Эта системная функция будет полезна из [геопространственных индексов](index-policy.md#spatial-indexes).

## <a name="next-steps"></a>Дальнейшие шаги

- [Пространственные функции Azure Cosmos DB](sql-query-spatial-functions.md)
- [Системные функции Azure Cosmos DB](sql-query-system-functions.md)
- [Знакомство со службой Azure Cosmos DB. API DocumentDB](introduction.md)
