---
title: Получение набирающих популярность изображений с помощью API Bing для поиска изображений
titleSuffix: Azure Cognitive Services
description: Поиск набирающих популярность изображений в Интернете с помощью API Bing для поиска изображений.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: EAB92D35-5C0B-4A0A-8F49-02DF7FAD44B4
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: 9163868848f2fdbd535a6b601077be570b0b01bf
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93076730"
---
# <a name="get-trending-images-from-the-web"></a>Получение набирающих популярность изображений из Интернета

> [!WARNING]
> API-интерфейсы поиска Bing перемещаются из Cognitive Services в Поиск Bing службы. Начиная с **30 октября 2020** , все новые экземпляры Поиск Bing должны быть подготовлены, следуя описанному [здесь](https://aka.ms/cogsvcs/bingmove)процессу.
> API-интерфейсы поиска Bing, подготовленные с помощью Cognitive Services, будут поддерживаться в течение следующих трех лет или до конца Соглашение Enterprise, в зависимости от того, что происходит раньше.
> Инструкции по миграции см. в разделе [Поиск Bing Services](https://aka.ms/cogsvcs/bingmigration).

Чтобы получить изображения, набирающие сегодня популярность, отправьте следующий запрос GET:  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending?mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

В настоящее время API поиска изображений, набирающих популярность, поддерживается только для следующих рынков:  

- en-US (английский, США)  
- en-CA (английский, Канада)  
- en-AU (английский, Австралия)  
- zh-CN (китайский, Китай)

Ответ содержит объект [TrendingImages](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#trendingimages), который представляет собой список изображений, разделенных по категориям. Используйте параметр `title` категории, чтобы сгруппировать изображения для удобства пользователей. Категории могут изменяться каждый день.  

```json
{
    "_type" : "TrendingImages",  
    "categories" : [{  
        "title" : "Popular people searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Smith",  
                "displayText" : "Mr. Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=smith&FORM=..."
            },  
            "image" : {  
                "id" : "C3C60AE779A054D5CD80D3CACF0F3",  
                "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OIP.M2532...",  
                "contentUrl" : "http:\/\/www.contoso.com.au\/assets\/Uploads\/smith-SH01.jpg",  
                "thumbnail" : {  
                    "width" : 288,  
                    "height" : 300  
                }  
            }  
        },  
        . . .  
        ]  
    },  
    . . .  
    {  
        "title" : "Popular Halloween searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Halloween costumes for adults",  
                "displayText" : "Halloween costumes for adults",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Halloween+costumes..."
            },  
            "image" : {  
                "id" : "0F3395F2983003F89DCEE711B55D7FA53E4",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP.Me429c...",  
                "contentUrl" : "http:\/\/images.domain.com\/products\/8179\/1-1\/adult-squirrel...",  
                "thumbnail" : {  
                    "width" : 336,  
                    "height" : 480  
                }  
            }  
        }]  
    }]  
}  
```  

Каждый элемент содержит изображение и способы получения связанных изображений. Для получения связанных изображений можно использовать параметр запроса `text`, чтобы вызвать [API для поиска изображений](./search-the-web.md) и отобразить связанные изображения самостоятельно. Также можно использовать URL-адрес в параметре `webSearchUrl`, чтобы перенаправить пользователя на страницу Bing с результатами поиска изображений, которая содержит связанные изображения.

Если для получения связанных изображений вы вызываете API для поиска изображений, то задайте для параметра запроса [id](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#id) значение идентификатора в поле `id`. Указав идентификатор, вы гарантируете, что ответ будет содержать изображение (это первое изображение в ответе) и связанные с ним изображения. Кроме того, в качестве значения параметра запроса [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) укажите текст из поля `text` объекта `query`.

В приведенном ниже примере показано, как использовать идентификатор изображения для получения изображений, связанных с г-ном Смитом (Mr. Smith) из предыдущего ответа API для поиска изображений, набирающих популярность.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=Smith&id=77FDE4A1C6529A23C7CF0EC073FAA64843E828F2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  
