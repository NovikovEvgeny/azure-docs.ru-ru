---
title: Работа с массивами и объектами в Azure Cosmos DB
description: Сведения о синтаксисе SQL для создания массивов и объектов в Azure Cosmos DB. В этой статье также приведены некоторые примеры выполнения операций с объектами Array.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 80d158e209943d65b9934d5425ccce7d69422bc4
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2020
ms.locfileid: "93338369"
---
# <a name="working-with-arrays-and-objects-in-azure-cosmos-db"></a>Работа с массивами и объектами в Azure Cosmos DB
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Ключевой функцией API Azure Cosmos DB SQL является создание массивов и объектов.

## <a name="arrays"></a>Массивы

Можно создавать массивы, как показано в следующем примере:

```sql
SELECT [f.address.city, f.address.state] AS CityState
FROM Families f
```

Вы получите такие результаты:

```json
[
  {
    "CityState": [
      "Seattle",
      "WA"
    ]
  },
  {
    "CityState": [
      "NY", 
      "NY"
    ]
  }
]
```

Можно также использовать [выражение массива](sql-query-subquery.md#array-expression) для создания массива из результатов [вложенного запроса](sql-query-subquery.md) . Этот запрос получает все уникальные заданные имена дочерних элементов в массиве.

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

## <a name="iteration"></a><a id="Iteration"></a>Итератив

API SQL обеспечивает поддержку итерации по массивам JSON с новой конструкцией, добавленной с помощью [ключевого слова in](sql-query-keywords.md#in) из источника from. В следующем примере:

```sql
SELECT *
FROM Families.children
```

Вы получите такие результаты:

```json
[
  [
    {
      "firstName": "Henriette Thaulow",
      "gender": "female",
      "grade": 5,
      "pets": [{ "givenName": "Fluffy"}]
    }
  ], 
  [
    {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1
    }, 
    {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }
  ]
]
```

Следующий запрос выполняет итерацию `children` в `Families` контейнере. Выходной массив отличается от предыдущего запроса. В этом примере показано разбиение `children` и сведение результатов в один массив:  

```sql
SELECT *
FROM c IN Families.children
```

Вы получите такие результаты:

```json
[
  {
      "firstName": "Henriette Thaulow",
      "gender": "female",
      "grade": 5,
      "pets": [{ "givenName": "Fluffy" }]
  },
  {
      "familyName": "Merriam",
      "givenName": "Jesse",
      "gender": "female",
      "grade": 1
  },
  {
      "familyName": "Miller",
      "givenName": "Lisa",
      "gender": "female",
      "grade": 8
  }
]
```

Можно выполнить фильтрацию для каждой отдельной записи массива, как показано в следующем примере:

```sql
SELECT c.givenName
FROM c IN Families.children
WHERE c.grade = 8
```

Вы получите такие результаты:

```json
[{
  "givenName": "Lisa"
}]
```

Можно также выполнить статистическую обработку по результату итерации массива. Например, следующий запрос подсчитывает количество дочерних элементов во всех семействах:

```sql
SELECT COUNT(child)
FROM child IN Families.children
```

Результаты:

```json
[
  {
    "$1": 3
  }
]
```

## <a name="next-steps"></a>Дальнейшие действия

- [Начало работы](sql-query-getting-started.md)
- [Примеры .NET для Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Joins](sql-query-join.md)
