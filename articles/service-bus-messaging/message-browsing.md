---
title: Служебная шина Azure — Просмотр сообщений
description: Просмотр и просмотр сообщений служебной шины позволяет клиенту служебной шины Azure перечислять все сообщения, которые находятся в очереди или подписке.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 6e50fc737f6c81c07854ff07d8cc64061306749b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91827452"
---
# <a name="message-browsing"></a>Просмотр сообщений

Обзор сообщений ("просмотр") позволяет клиенту служебной шины перечислить все сообщения, находящиеся в очереди или подписке, как правило, для диагностики и отладки.

Операции просмотра возвращают все сообщения, которые существуют в журнале сообщений очереди или подписки, а не только те, что доступны для немедленного получения посредством операции `Receive()` или цикла `OnMessage()`. Свойство `State` каждого сообщения указывает, что сообщение активно (доступно для получения), [отложено](message-deferral.md) или [запланировано](message-sequencing.md).

Использованные и просроченные сообщения очищаются при асинхронном выполнении"сборки мусора". Это не обязательно происходит сразу же после истечения срока действия сообщения, поэтому операция `Peek` действительно может возвращать сообщения, срок действия которых уже истек и которые будут удалены или отправлены в очередь недоставленных сообщений при последующем вызове операции получения для очереди или подписки.

Это особенно важно учитывать при попытке восстановления отложенных сообщений из очереди. Сообщение, для которого момент [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc#Microsoft_Azure_ServiceBus_Message_ExpiresAtUtc) прошел, больше не доступно для обычного получения какими-либо другими способами, даже если оно возвращается при операции просмотра. Эти сообщения возвращаются намеренно, так как операция просмотра — это инструмент диагностики, отражающий текущее состояние журнала.

Эта операция также возвращает сообщения, которые были заблокированы и в настоящий момент обрабатываются другими получателями, но эта обработка еще не завершена. Тем не менее, поскольку операция просмотра возвращает отключенный моментальный снимок, состояние блокировки просматриваемого сообщения просмотреть невозможно, и при попытке приложения прочитать свойства [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.lockeduntilutc) и [LockToken](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.locktoken#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_LockToken) они порождают исключение [InvalidOperationException](/dotnet/api/system.invalidoperationexception).

## <a name="peek-apis"></a>Интерфейсы API просмотра

Методы [Peek/пикасинк](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_PeekAsync) и [PeekBatch/пикбатчасинк](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatchasync#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatchAsync_System_Int64_System_Int32_) существуют во всех клиентских библиотеках .NET и Java, а также во всех объектах-получателях: **MessageReceiver**, **MessageSession**. Операция просмотра работает для всех очередей, подписок и их соответствующих очередей недоставленных сообщений.

При повторяющихся вызовах метод Peek перечисляет все сообщения, которые существуют в журнале очереди или подписки, по возрастанию порядкового номера. Это порядок, в котором сообщения были поставлены в очередь, но не порядок, в котором сообщения со временем могут быть получены.

[PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) получает несколько сообщений и возвращает их в виде перечисления. Если доступных сообщений нет, объект перечисления является пустым, а не содержащим значения NULL.

Кроме того, можно использовать перегрузку метода с [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) , с которой следует начать, а затем вызвать перегрузку метода без параметров для дальнейшего перечисления. **PeekBatch** работает аналогично, но получает набор сообщений за раз.

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения об обмене сообщениями через служебную шину см. в следующих статьях:

* [Очереди, разделы и подписки служебной шины](service-bus-queues-topics-subscriptions.md)
* [Начало работы с очередями служебной шины](service-bus-dotnet-get-started-with-queues.md)
* [Как использовать разделы и подписки служебной шины](service-bus-dotnet-how-to-use-topics-subscriptions.md)
