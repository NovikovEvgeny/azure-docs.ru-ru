---
ms.openlocfilehash: c87b9dc22ecd937abb27417aeddc1e30c0d724e7
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "85114574"
---

## <a name="using-this-example-knowledge-base"></a>Использование этого примера базы знаний

База знаний, используемая в этом кратком руководстве, начинается с двух пар разговорных вопросов и ответов. Это сделано, чтобы упростить пример и обеспечить строго предсказуемые идентификаторы, используемые в методе Update, который сопоставляет дальнейшие запросы, содержащие вопросы, с новыми парами. Такой подход был запланирован и реализован в определенном порядке для целей данного краткого руководства.

Если вы планируете со временем разработать базу знаний с дальнейшими запросами, которые зависят от имеющихся пар вопросов и ответов, вы можете воспользоваться одним из следующих вариантов.
* Для управления более крупными базами знаний следует воспользоваться текстовым редактором или поддерживающим автоматизацию средством TSV, а затем полностью заменить всю базу знаний обновленной версией.
* Для небольших баз знаний дальнейшими запросами можно полностью управлять на портале QnA Maker.

Ниже приведены сведения о парах вопросов и ответов, используемых в этом кратком руководстве.
* Типы пар вопросов и ответов. После обновления в этой базе знаний содержатся пары вопросов и ответов двух типов: для беседы и по предметной области. Это типичное содержание для баз знаний, привязанных к приложению для общения, например чат-боту.
* Хотя можно фильтровать ответы из базы знаний по метаданным или использовать дальнейшие запросы, в этом кратком руководстве эти возможности не рассматриваются. Такие независящие от языка примеры generateAnswer можно найти [здесь](../Quickstarts/get-answer-from-knowledge-base-using-url-tool.md).
* Текст ответа — это данные на языке Markdown, которые могут содержать [широкий спектр элементов Markdown](../reference-markdown-format.md), например изображения (общедоступные изображения из Интернета), ссылки (на общедоступные URL-адреса) и пункты маркированного списка. Все это многообразие в данном кратком руководстве не используется.
