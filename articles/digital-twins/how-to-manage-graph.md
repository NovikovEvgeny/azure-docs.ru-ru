---
title: Управление графом двойника со связями
titleSuffix: Azure Digital Twins
description: Узнайте, как управлять графом цифровых двойников, подключив их к связям.
author: baanders
ms.author: baanders
ms.date: 11/03/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 78e0bfb0af494ecae2865fcc42679b8fcce44916
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2020
ms.locfileid: "94359584"
---
# <a name="manage-a-graph-of-digital-twins-using-relationships"></a>Управление графиком цифровых двойников с помощью связей

Основой Azure Digital двойников является [двойника граф](concepts-twins-graph.md) , представляющий всю среду. Граф двойника состоит из отдельных цифровых двойников, подключенных через **связи**. 

Получив рабочий [экземпляр Azure Digital двойников](how-to-set-up-instance-portal.md) и настроив код [проверки подлинности](how-to-authenticate-client.md) в клиентском приложении, вы можете использовать [**API дигиталтвинс**](/rest/api/digital-twins/dataplane/twins) для создания, изменения и удаления цифровых двойников и их отношений в экземпляре Azure Digital двойников. Вы также можете использовать [пакет SDK для .NET (C#)](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true)или [Azure Digital двойников CLI](how-to-use-cli.md).

Эта статья посвящена управлению связями и графу в целом. для работы с отдельными цифровыми двойниковми см. раздел [*как управлять цифровыми двойников*](how-to-manage-twin.md).

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]
    
[!INCLUDE [visualizing with Azure Digital Twins explorer](../../includes/digital-twins-visualization.md)]

## <a name="create-relationships"></a>Создавать связи

Связи описывают, как разные цифровые двойников соединяются друг с другом, который образует базу двойника графа.

Связи создаются с помощью `CreateOrReplaceRelationshipAsync()` вызова. 

Чтобы создать связь, необходимо указать:
* Идентификатор источника двойника ( `srcId` в образце кода ниже): идентификатор двойника, в которой создается связь.
* Целевой идентификатор двойника ( `targetId` в примере кода ниже): идентификатор двойника, в которой получено отношение.
* Имя связи (в приведенном `relName` ниже примере кода): универсальный тип связи, что подобно _Contains_.
* Идентификатор отношения (в приведенном `relId` ниже примере кода): конкретное имя для этой связи, что вроде _Relationship1_.

ИДЕНТИФИКАТОР связи должен быть уникальным в пределах заданного исходного двойника. Оно не обязательно должно быть глобально уникальным.
Например, для двойника *foo* каждый конкретный идентификатор связи должен быть уникальным. Однако другая двойниканая *черта* может иметь исходящее отношение, совпадающее с идентификатором связи *foo* .

В следующем примере кода показано, как создать связь в вашем экземпляре Digital двойников для Azure.

```csharp
public async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId, string relName)
        {
            var relationship = new BasicRelationship
            {
                TargetId = targetId,
                Name = relName
            };

            try
            {
                string relId = $"{srcId}-{relName}->{targetId}";
                await client.CreateOrReplaceRelationshipAsync<BasicRelationship>(srcId, relId, relationship);
                Console.WriteLine($"Created {relName} relationship successfully");
            }
            catch (RequestFailedException rex)
            {
                Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
            }
            
        }
```
В методе Main теперь можно вызвать `CreateRelationship()` функцию, чтобы создать связь со следующим _contains_ объектом: 

```csharp
await CreateRelationship(client, srcId, targetId, "contains");
```
Если вы хотите создать несколько связей, можно повторить вызовы одного и того же метода, передав разные типы отношений в аргумент. 

Дополнительные сведения о вспомогательном классе `BasicRelationship` см. в разделе Практические руководства. [*Использование интерфейсов API и пакетов SDK для цифровых двойников Azure*](how-to-use-apis-sdks.md#serialization-helpers).

### <a name="create-multiple-relationships-between-twins"></a>Создание нескольких связей между двойников

Отношения можно классифицировать следующим образом: 

* Исходящие связи: отношения, принадлежащие этой двойника, которые, в своюмся, должны соединяться с другими двойников. `GetRelationshipsAsync()`Метод используется для получения исходящих отношений двойника.
* Входящие отношения: отношения, принадлежащие другим двойников, которые указывают на этот двойника для создания ссылки "входящий". `GetIncomingRelationshipsAsync()`Метод используется для получения входящих отношений двойника.

На число связей, которые можно использовать между двумя двойниковами, не накладывается никаких ограничений. Вы можете иметь любое количество отношений между двойников. 

Это означает, что можно выразить несколько различных типов отношений между двумя двойников одновременно. Например, *двойника A* может иметь как *хранимую* связь, так и *производимую* связь с *двойника B*.

При необходимости можно даже создать несколько экземпляров одного типа связи между одними и теми же двумя двойников. В этом примере *двойника A* может иметь два разных *хранимых* отношения с *двойника B* , если связи имеют разные идентификаторы отношений.

## <a name="list-relationships"></a>Список связей

Чтобы получить доступ к списку **исходящих** связей для заданного двойника в графе, можно использовать `GetRelationships()` метод следующим образом:

```csharp
await client.GetRelationships()
```

Он возвращает `Azure.Pageable<T>` или `Azure.AsyncPageable<T>` , в зависимости от того, используется ли синхронная или асинхронная версия вызова.

Ниже приведен пример, в котором извлекается список связей:

```csharp
public static async Task<List<BasicRelationship>> FindOutgoingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin
            try
            {
                // GetRelationshipsAsync will throw if an error occurs
                AsyncPageable<BasicRelationship> rels = client.GetRelationshipsAsync<BasicRelationship>(dtId);
                List<BasicRelationship> results = new List<BasicRelationship>();
                await foreach (BasicRelationship rel in rels)
                {
                    results.Add(rel);
                    Console.WriteLine($"Found relationship-{rel.Name}->{rel.TargetId}");
                }

                return results;
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"**_ Error {ex.Status}/{ex.ErrorCode} retrieving relationships for {dtId} due to {ex.Message}");
                return null;
            }
        }

```
Теперь можно вызвать этот метод, чтобы увидеть исходящие отношения двойников следующим образом:

```csharp
await FindOutgoingRelationshipsAsync(client, twin_Id);
```
Полученные связи можно использовать для перехода к другим двойников в графе. Для этого прочтите `target` поле из возвращаемой связи и используйте его в качестве идентификатора для следующего вызова `GetDigitalTwin()` .

### <a name="find-incoming-relationships-to-a-digital-twin"></a>Поиск входящих отношений для цифрового двойника

В Azure Digital двойников также есть API, который позволяет найти все *Входящие* * связи с заданным двойника. Это часто полезно для обратных переходов или при удалении двойника.

Предыдущий пример кода был сосредоточен на поиске исходящих связей от двойника. Следующий пример структурирован аналогично, но находит *Входящие* связи с двойника.

Обратите внимание, что `IncomingRelationship` вызовы не возвращают полный текст связи.

```csharp
public static async Task<List<IncomingRelationship>> FindIncomingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin
            try
            {
                // GetRelationshipsAsync will throw an error if a problem occurs
                AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

                List<IncomingRelationship> results = new List<IncomingRelationship>();
                await foreach (IncomingRelationship incomingRel in incomingRels)
                {
                    results.Add(incomingRel);
                    Console.WriteLine($"Found incoming relationship-{incomingRel.RelationshipId}");

                }
                return results;
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"*** Error {ex.Status}/{ex.ErrorCode} retrieving incoming relationships for {dtId} due to {ex.Message}");
                return null;
            }
        }
```

Теперь этот метод можно вызвать для просмотра входящих отношений двойников следующим образом:

```csharp
await FindIncomingRelationshipsAsync(client, twin_Id);
```
### <a name="list-all-twin-properties-and-relationships"></a>Вывод списка всех свойств и отношений двойника

Используя описанные выше методы для перечисления входящих и исходящих отношений с двойника, можно создать метод, который выводит полные сведения двойника, включая свойства двойника и оба типа связей. Ниже приведен пример метода с именем `FetchAndPrintTwinAsync()` , демонстрирующий, как это сделать.

```csharp  
private static async Task FetchAndPrintTwinAsync(DigitalTwinsClient client, string twin_Id)
        {
            BasicDigitalTwin twin;
            Response<BasicDigitalTwin> res = client.GetDigitalTwin(twin_Id);
            
            await FindOutgoingRelationshipsAsync(client, twin_Id);
            await FindIncomingRelationshipsAsync(client, twin_Id);

            return;
        }
```

Теперь эту функцию можно вызвать в методе Main следующим образом: 

```csharp
await FetchAndPrintTwinAsync(client, targetId);
```
## <a name="delete-relationships"></a>Удаление связей

Первый параметр указывает исходный двойника (двойника, где исходит связь). Другой параметр — идентификатор связи. Вам потребуется идентификатор двойника и идентификатор связи, так как идентификаторы связей уникальны только в пределах области двойника.

```csharp
private static async Task DeleteRelationship(DigitalTwinsClient client, string srcId, string relId)
        {
            try
            {
                Response response = await client.DeleteRelationshipAsync(srcId, relId);
                await FetchAndPrintTwinAsync(srcId, client);
                Console.WriteLine("Deleted relationship successfully");
            }
            catch (RequestFailedException Ex)
            {
                Console.WriteLine($"Error {Ex.ErrorCode}");
            }
        }
```

Теперь можно вызвать этот метод, чтобы удалить связь следующим образом:

```csharp
await DeleteRelationship(client, srcId, relId);
```
## <a name="create-a-twin-graph"></a>Создание графа двойника 

Следующий готовый к запуску фрагмент кода использует операции связи из этой статьи для создания графа двойника из цифровых двойников и связей.

### <a name="set-up-the-runnable-sample"></a>Настройка готового к запуску примера

В фрагменте кода используется [*Room.jsна*](https://github.com/Azure-Samples/digital-twins-samples/blob/master/AdtSampleApp/SampleClientApp/Models/Room.json) определениях моделей и [*Floor.js*](https://github.com/azure-Samples/digital-twins-samples/blob/master/AdtSampleApp/SampleClientApp/Models/Floor.json) из [*учебника: изучение Azure Digital двойников с примером клиентского приложения*](tutorial-command-line-app.md). Вы можете использовать эти ссылки, чтобы перейти непосредственно к файлам или загрузить их как часть полного [примера проекта.](/samples/azure-samples/digital-twins-samples/digital-twins-samples/) 

Перед запуском образца выполните следующие действия.
1. Скачайте файлы модели, поместите их в свой проект и замените `<path-to>` заполнители в приведенном ниже коде, чтобы сообщить программе, где их можно найти.
2. Замените заполнитель `<your-instance-hostname>` именем узла своего экземпляра цифрового двойников Azure.
3. Добавьте эти пакеты в проект:
    ```cmd/sh
    dotnet add package Azure.DigitalTwins.Core --version 1.0.0-preview.3
    dotnet add package Azure.identity
    ```

Кроме того, необходимо настроить локальные учетные данные, если вы хотите выполнить пример напрямую. В следующем разделе приведено пошаговое руководство.
[!INCLUDE [Azure Digital Twins: local credentials prereq (outer)](../../includes/digital-twins-local-credentials-outer.md)]

### <a name="run-the-sample"></a>Запуск примера

После выполнения описанных выше действий можно запустить следующий пример кода.

```csharp 
using System;
using Azure.DigitalTwins.Core;
using Azure.Identity;
using System.Threading.Tasks;
using System.IO;
using System.Collections.Generic;
using Azure;
using Azure.DigitalTwins.Core.Serialization;
using System.Text.Json;

namespace minimal
{
    class Program
    {

        public static async Task Main(string[] args)
        {
            Console.WriteLine("Hello World!");

            //Create the Azure Digital Twins client for API calls
            DigitalTwinsClient client = createDTClient();
            Console.WriteLine($"Service client created – ready to go");
            Console.WriteLine();

            //Upload models
            Console.WriteLine($"Upload models");
            Console.WriteLine();
            string dtdl = File.ReadAllText("<path-to>/Room.json");
            string dtdl1 = File.ReadAllText("<path-to>/Floor.json");
            var typeList = new List<string>();
            typeList.Add(dtdl);
            typeList.Add(dtdl1);
            // Upload the models to the service
            await client.CreateModelsAsync(typeList);

            //Create new (Floor) digital twin
            BasicDigitalTwin floorTwin = new BasicDigitalTwin();
            string srcId = "myFloorID";
            floorTwin.Metadata = new DigitalTwinMetadata();
            floorTwin.Metadata.ModelId = "dtmi:example:Floor;1";
            //Floor twins have no properties, so nothing to initialize
            //Create the twin
            await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(srcId, floorTwin);
            Console.WriteLine("Twin created successfully");

            //Create second (Room) digital twin
            BasicDigitalTwin roomTwin = new BasicDigitalTwin();
            string targetId = "myRoomID";
            roomTwin.Metadata = new DigitalTwinMetadata();
            roomTwin.Metadata.ModelId = "dtmi:example:Room;1";
            // Initialize properties
            Dictionary<string, object> props = new Dictionary<string, object>();
            props.Add("Temperature", 35.0);
            props.Add("Humidity", 55.0);
            roomTwin.Contents = props;
            //Create the twin
            await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(targetId, roomTwin);
            
            //Create relationship between them
            await CreateRelationship(client, srcId, targetId, "contains");
            Console.WriteLine();

            //Print twins and their relationships
            Console.WriteLine("--- Printing details:");
            Console.WriteLine("Outgoing relationships from source twin:");
            await FetchAndPrintTwinAsync(srcId, client);
            Console.WriteLine();
            Console.WriteLine("Incoming relationships to target twin:");
            await FetchAndPrintTwinAsync(targetId, client);
            Console.WriteLine("--------");
            Console.WriteLine();

            //Delete the relationship
            Console.WriteLine("Deleting the relationship");
            await DeleteRelationship(client, srcId, $"{srcId}-contains->{targetId}");
            Console.WriteLine();

            //Print twins and their relationships again
            Console.WriteLine("--- Printing details:");
            Console.WriteLine("Outgoing relationships from source twin:");
            await FetchAndPrintTwinAsync(srcId, client);
            Console.WriteLine();
            Console.WriteLine("Incoming relationships to target twin:");
            await FetchAndPrintTwinAsync(targetId, client);
            Console.WriteLine("--------");
            Console.WriteLine();
        }

        private static DigitalTwinsClient createDTClient()
        {
            string adtInstanceUrl = "https://<your-instance-hostname>";
            var credentials = new DefaultAzureCredential();
            DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credentials);
            return client;
        }
        private async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId, string relName)
        {
            // Create relationship between twins
            var relationship = new BasicRelationship
            {
                TargetId = targetId,
                Name = relName
            };

            try
            {
                string relId = $"{srcId}-{relName}->{targetId}";
                await client.CreateOrReplaceRelationshipAsync<BasicRelationship>(srcId, relId, relationship);
                Console.WriteLine($"Created {relName} relationship successfully");
            }
            catch (RequestFailedException rex)
            {
                Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
            }

        }

        private static async Task FetchAndPrintTwinAsync(string twin_Id, DigitalTwinsClient client)
        {
            BasicDigitalTwin twin;
            Response<BasicDigitalTwin> res = client.GetDigitalTwin(twin_Id);
            await FindOutgoingRelationshipsAsync(client, twin_Id);
            await FindIncomingRelationshipsAsync(client, twin_Id);

            return;
        }

        private static async Task<List<BasicRelationship>> FindOutgoingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin
            
            try
            {
                // GetRelationshipsAsync will throw if an error occurs
                AsyncPageable<BasicRelationship> rels = client.GetRelationshipsAsync<BasicRelationship>(dtId);
                List<BasicRelationship> results = new List<BasicRelationship>();
                await foreach (BasicRelationship rel in rels)
                {
                    results.Add(rel);
                    Console.WriteLine($"Found relationship-{rel.Name}->{rel.TargetId}");
                }

                return results;
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"*** Error {ex.Status}/{ex.ErrorCode} retrieving relationships for {dtId} due to {ex.Message}");
                return null;
            }
        }

        private static async Task<List<IncomingRelationship>> FindIncomingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin
            
            try
            {
                // GetRelationshipsAsync will throw an error if a problem occurs
                AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

                List<IncomingRelationship> results = new List<IncomingRelationship>();
                await foreach (IncomingRelationship incomingRel in incomingRels)
                {
                    results.Add(incomingRel);
                    Console.WriteLine($"Found incoming relationship-{incomingRel.RelationshipId}");
                }
                return results;
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"**_ Error {ex.Status}/{ex.ErrorCode} retrieving incoming relationships for {dtId} due to {ex.Message}");
                return null;
            }
        }

        private static async Task DeleteRelationship(DigitalTwinsClient client, string srcId, string relId)
        {
            try
            {
                Response response = await client.DeleteRelationshipAsync(srcId, relId);
                await FetchAndPrintTwinAsync(srcId, client);
                Console.WriteLine("Deleted relationship successfully");
            }
            catch (RequestFailedException Ex)
            {
                Console.WriteLine($"Error {Ex.ErrorCode}");
            }
        }
    }
}
```

Ниже приведены выходные данные консоли для приведенной выше программы. 

:::image type="content" source="./media/how-to-manage-graph/console-output-twin-graph.png" alt-text="Выходные данные консоли, отображающие сведения о двойника, входящие и исходящие связи двойников." lightbox="./media/how-to-manage-graph/console-output-twin-graph.png":::

> [!TIP]
> Граф двойника является концепцией создания связей между двойников. Если вы хотите просмотреть визуальное представление графа двойника, см. раздел [_Visualization *](how-to-manage-graph.md#visualization) этой статьи. 

### <a name="create-a-twin-graph-from-a-csv-file"></a>Создание графа двойника из CSV-файла

В практических случаях двойника иерархии часто создаются на основе данных, хранящихся в другой базе данных или в виде CSV-файла. В этом разделе показано, как считать данные из CSV-файла и создать граф двойника из него.

Рассмотрим следующую таблицу данных, описывающую набор цифровых двойников и связей.

|  Идентификатор модели    | Идентификатор двойника (должен быть уникальным) | Имя связи  | Идентификатор целевого двойника  | Данные инициализации двойника |
| --- | --- | --- | --- | --- |
| дтми: пример: Floor; 1    | Floor1 | содержит | Room1 | |
| дтми: пример: Floor; 1    | Floor0 | содержит | Room0 | |
| дтми: пример: комната; 1    | Room1 | | | {"Температура": 80} |
| дтми: пример: комната; 1    | Room0 | | | {"Температура": 70} |

Одним из способов получения этих данных в Azure Digital двойников является преобразование таблицы в CSV-файл и написание кода для интерпретации файла в команды для создания двойников и связей. В следующем примере кода показано чтение данных из CSV-файла и создание графа двойника в Azure Digital двойников.

В приведенном ниже коде CSV-файл называется *data.csv* , а также есть заполнитель, представляющий **имя узла** для своего экземпляра Digital двойников Azure. В примере также используется несколько пакетов, которые можно добавить в проект, чтобы помочь в этом процессе.

```csharp
using System;
using System.Collections.Generic;
using System.Text.Json;
using System.Threading.Tasks;
using Azure;
using Azure.DigitalTwins.Core;
using Azure.Identity;

namespace creating_twin_graph_from_csv
{
    class Program
    {
        static async Task Main(string[] args)
        {
            List<BasicRelationship> RelationshipRecordList = new List<BasicRelationship>();
            List<BasicDigitalTwin> TwinList = new List<BasicDigitalTwin>();
            List<List<string>> data = ReadData();
            DigitalTwinsClient client = createDTClient();

            // Interpret the CSV file data, by each row
            foreach (List<string> row in data)
            {
                string modelID = row.Count > 0 ? row[0].Trim() : null;
                string srcID = row.Count > 1 ? row[1].Trim() : null;
                string relName = row.Count > 2 ? row[2].Trim() : null;
                string targetID = row.Count > 3 ? row[3].Trim() : null;
                string initProperties = row.Count > 4 ? row[4].Trim() : null;
                Console.WriteLine($"ModelID: {modelID}, TwinID: {srcID}, RelName: {relName}, TargetID: {targetID}, InitData: {initProperties}");
                Dictionary<string, object> props = new Dictionary<string, object>();
                // Parse properties into dictionary (left out for compactness)
                // ...

                // Null check for source and target ID's
                if (srcID != null && srcID.Length > 0 && targetID != null && targetID.Length > 0)
                {
                    BasicRelationship br = new BasicRelationship()
                    {
                        SourceId = srcID,
                        TargetId = targetID,
                        Name = relName
                    };
                    RelationshipRecordList.Add(br);
                }
                BasicDigitalTwin srcTwin = new BasicDigitalTwin();
                srcTwin.Id = srcID;
                srcTwin.Metadata = new DigitalTwinMetadata();
                srcTwin.Metadata.ModelId = modelID;
                srcTwin.Contents = props;
                TwinList.Add(srcTwin);
            }

            // Create digital twins 
            foreach (BasicDigitalTwin twin in TwinList)
            {
                try
                {
                    await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(twin.Id, twin);
                    Console.WriteLine("Twin is created");
                }
                catch (RequestFailedException e)
                {
                    Console.WriteLine($"Error {e.Status}: {e.Message}");
                }
            }
            // Create relationships between the twins
            foreach (BasicRelationship rec in RelationshipRecordList)
            {
                try
                {
                    string relId = $"{rec.SourceId}-{rec.Name}->{rec.TargetId}";
                    await client.CreateOrReplaceRelationshipAsync<BasicRelationship>(rec.SourceId, relId, rec);
                    Console.WriteLine("Relationship is created");
                }
                catch (RequestFailedException e)
                {
                    Console.WriteLine($"Error {e.Status}: {e.Message}");
                }
            }
        }

        // Method to ingest data from the CSV file
        public static List<List<string>> ReadData()
        {
            string path = "<path-to>/data.csv";
            string[] lines = System.IO.File.ReadAllLines(path);
            List<List<string>> data = new List<List<string>>();
            int count = 0;
            foreach (string line in lines)
            {
                if (count++ == 0)
                    continue;
                List<string> cols = new List<string>();
                data.Add(cols);
                string[] columns = line.Split(',');
                foreach (string column in columns)
                {
                    cols.Add(column);
                }
            }
            return data;
        }
        // Method to create the digital twins client
        private static DigitalTwinsClient createDTClient()
        {

            string adtInstanceUrl = "https://<your-instance-hostname>";
            var credentials = new DefaultAzureCredential();
            DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credentials);
            return client;
        }
    }
}

```
## <a name="manage-relationships-with-cli"></a>Управление связями с помощью интерфейса командной строки

Двойников и их отношения также можно управлять с помощью цифрового двойников Azure CLI. Команды можно найти в [*этом пошаговом окне. Используйте интерфейс командной строки Azure Digital двойников*](how-to-use-cli.md).

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения о запросах к графу двойников для Azure Digital двойника:
* [*Основные понятия: язык запросов*](concepts-query-language.md)
* [*Пошаговое руководство. запрос графа двойника*](how-to-query-graph.md)