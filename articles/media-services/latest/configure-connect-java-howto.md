---
title: Подключение к API Служб мультимедиа Azure версии 3 в Java
description: В этой статье описывается подключение к API-интерфейсу Служб мультимедиа Azure версии 3 с помощью Java.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/31/2020
ms.custom: devx-track-java
ms.author: inhenkel
ms.openlocfilehash: 8f5044a5fb43956e857bc7b93b92e15d40225326
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92013242"
---
# <a name="connect-to-media-services-v3-api---java"></a>Подключение к API Служб мультимедиа версии 3 из Java

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

В этой статье показано, как подключиться к пакету SDK для Java Служб мультимедиа Azure версии 3 с помощью метода входа субъекта-службы.

В этой статье для разработки примера приложения используется Visual Studio Code.

## <a name="prerequisites"></a>Предварительные требования

- Следуйте указаниям из статьи [Написание кода Java с помощью Visual Studio Code](https://code.visualstudio.com/docs/java/java-tutorial), чтобы установить:

   - JDK
   - Apache Maven
   - Пакет расширения Java
- Убедитесь, что заданы переменные среды `JAVA_HOME` и `PATH`.
- [Создание учетной записи Служб мультимедиа](./create-account-howto.md). Обязательно запомните имя группы ресурсов и имя учетной записи Служб мультимедиа.
- Выполните действия, описанные в разделе [Доступ к API](./access-api-howto.md). Запишите идентификатор подписки, идентификатор приложения (идентификатор клиента), ключ проверки подлинности (секрет) и идентификатор клиента, которые понадобятся вам на более позднем этапе.

Кроме того, ознакомьтесь со следующими разделами:

- [Java в Visual Studio Code](https://code.visualstudio.com/docs/languages/java)
- [Управление проектами Java в VS Code](https://code.visualstudio.com/docs/java/java-project)

> [!IMPORTANT]
> Проверьте [соглашения об именовании](media-services-apis-overview.md#naming-conventions).

## <a name="create-a-maven-project"></a>Создание проекта Maven

Откройте средство командной строки и `cd` в каталоге, в котором нужно создать проект.
    
```
mvn archetype:generate -DgroupId=com.azure.ams -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

При выполнении команды создаются `pom.xml`, `App.java` и другие файлы. 

## <a name="add-dependencies"></a>Добавление зависимостей

1. В Visual Studio Code откройте папку, в которой находится проект.
1. Найдите и откройте `pom.xml`.
1. Добавьте необходимые зависимости.

    ```xml
   <dependency>
     <groupId>com.microsoft.azure.mediaservices.v2018_07_01</groupId>
     <artifactId>azure-mgmt-media</artifactId>
     <version>1.0.0-beta-3</version>
   </dependency>
   <dependency>
     <groupId>com.microsoft.rest</groupId>
     <artifactId>client-runtime</artifactId>
     <version>1.6.6</version>
   </dependency>
   <dependency>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-client-authentication</artifactId>
     <version>1.6.6</version>
   </dependency>
    ```

## <a name="connect-to-the-java-client"></a>Подключение к клиенту Java

1. Откройте файл `App.java` в разделе `src\main\java\com\azure\ams` и убедитесь, что пакет добавлен в начало:

    ```java
    package com.azure.ams;
    ```
1. Под оператором statement добавьте следующие операторы import:
   
   ```java
   import com.microsoft.azure.AzureEnvironment;
   import com.microsoft.azure.credentials.ApplicationTokenCredentials;
   import com.microsoft.azure.management.mediaservices.v2018_07_01.implementation.MediaManager;
   import com.microsoft.rest.LogLevel;
   ```
1. Чтобы создать учетные данные Active Directory, необходимые для выполнения запросов, добавьте следующий код в метод main класса App и задайте значения, полученные из раздела [Доступ к API](./access-api-howto.md):
   
   ```java
   final String clientId = "00000000-0000-0000-0000-000000000000";
   final String tenantId = "00000000-0000-0000-0000-000000000000";
   final String clientSecret = "00000000-0000-0000-0000-000000000000";
   final String subscriptionId = "00000000-0000-0000-0000-000000000000";

   try {
      ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(clientId, tenantId, clientSecret, AzureEnvironment.AZURE);
      credentials.withDefaultSubscriptionId(subscriptionId);

      MediaManager manager = MediaManager
              .configure()
              .withLogLevel(LogLevel.BODY_AND_HEADERS)
              .authenticate(credentials, credentials.defaultSubscriptionId());
      System.out.println("signed in");
   }
   catch (Exception e) {
      System.out.println("Exception encountered.");
      System.out.println(e.toString());
   }
   ```
1. Запустите приложение.

## <a name="see-also"></a>См. также раздел

- [Основные понятия служб мультимедиа Azure](concepts-overview.md)
- [пакет SDK для Java](https://aka.ms/ams-v3-java-sdk)
- [Справочник по Java](/java/api/overview/azure/mediaservices/management)
- [com.microsoft.azure.mediaservices.v2018_07_01:azure-mgmt-media](https://search.maven.org/artifact/com.microsoft.azure.mediaservices.v2018_07_01/azure-mgmt-media/1.0.0-beta/jar)

## <a name="next-steps"></a>Дальнейшие действия

Теперь можно включить `import com.microsoft.azure.management.mediaservices.v2018_07_01.*;` и начать операции с сущностями.

Дополнительные примеры кода см. в репозитории [Примеры для SDK Java](/samples/azure-samples/media-services-v3-java/azure-media-services-v3-samples-using-java/).