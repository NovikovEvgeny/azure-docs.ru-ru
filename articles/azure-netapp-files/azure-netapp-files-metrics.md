---
title: Метрики для службы Azure NetApp Files | Документация Майкрософт
description: Azure NetApp Files предоставляет метрики для выделенного хранилища, фактического использования хранилища, операций ввода-вывода томов и задержки. Используйте эти метрики для анализа использования и производительности.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/13/2020
ms.author: b-juche
ms.openlocfilehash: c79586703c49fe37d4d0915f49b69e6aa842083e
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92017528"
---
# <a name="metrics-for-azure-netapp-files"></a>Метрики для Azure NetApp Files

Azure NetApp Files предоставляет метрики для выделенного хранилища, фактического использования хранилища, операций ввода-вывода томов и задержки. Проанализировав эти метрики, вы сможете узнать больше о производительности томов и шаблонах использования учетных записей NetApp.  

## <a name="usage-metrics-for-capacity-pools"></a><a name="capacity_pools"></a>Использование метрик для пулов емкости

- *Выделенный размер пула*   
    Подготовленный размер пула.

- *Пул, выделенный для размера тома*  
    Суммарная квота тома (гиб) в заданном пуле емкости (то есть общая сумма подготовленных объемов томов в пуле емкости).  
    Этот размер равен размеру, выбранному при создании тома.  

- *Размер использованного пула*  
    Общий объем логического пространства (гиб), используемого на томах в пуле емкости.  

- *Общий размер моментального снимка для пула*    
    Сумма размера моментального снимка из всех томов в пуле.

## <a name="usage-metrics-for-volumes"></a><a name="volumes"></a>Показатели использования для томов

<!-- ANF-5023: fixed version: 2020.08, 2020.09
- *Percentage Volume Consumed Size*    
    The percentage of the volume consumed, including snapshots.  
-->
- *Размер выделенного тома*   
    Подготовленный размер тома
- *Размер квоты тома*    
    Размер квоты (гиб), с которой подготавливается том.   
- *Объем использованного объема тома*   
    Логический размер тома (используется байт).  
    Сюда можно отнести логическое пространство используемое активными файлами системы и моментальными снимками.  
- *Размер моментального снимка тома*   
   Размер всех моментальных снимков в томе.  

## <a name="performance-metrics-for-volumes"></a>Метрики производительности для томов

- *Средняя задержка чтения*   
    Среднее время чтения с тома в миллисекундах.
- *Средняя задержка записи*   
    Среднее время записи с тома в миллисекундах.
- *Чтение операций ввода-вывода*   
    Количество операций чтения в томе в секунду.
- *Запись в секунду*   
    Число операций записи в том в секунду.
<!-- These two metrics are not yet available, until ~ 2020.09
- *Read MiB/s*   
    Read throughput in bytes per second.
- *Write MiB/s*   
    Write throughput in bytes per second.
--> 
<!-- ANF-4128; 2020.07
- *Pool Provisioned Throughput*   
    The total throughput a capacity pool can provide to its volumes based on "Pool Provisioned Size" and "Service Level".
- *Pool Allocated to Volume Throughput*   
    The total throughput allocated to volumes in a given capacity pool (that is, the total of the volumes' allocated throughput in the capacity pool).
-->

<!-- ANF-6443; 2020.11
- *Pool Consumed Throughput*    
    The total throughput being consumed by volumes in a given capacity pool.
-->


## <a name="volume-replication-metrics"></a><a name="replication"></a>Метрики репликации томов

> [!NOTE] 
> Размер передаваемой сети (например, метрики *общего объема репликации тома* ) может отличаться от исходных или целевых томов репликации между регионами. Это поведение является результатом эффективного механизма репликации, используемого для снижения затрат на сетевую пересылку.

- *Состояние репликации тома — работоспособное*   
    Условие отношения репликации. Работоспособное состояние обозначается `1` . Неработоспособное состояние обозначается `0` .

- *Передача репликации тома*    
    Указывает, является ли состояние репликации тома "передачей". 
 
- *Время задержки репликации тома*   
    Время в секундах, на которое данные на зеркале отстают позади источника. 

- *Длительность последней пересылки репликации тома*   
    Количество времени в секундах, затраченного на завершение последней пересылки. 

- *Размер последней пересылки репликации тома*    
    Общее число байтов, переданных в ходе последней передачи. 

- *Ход репликации тома*    
    Общий объем данных, передаваемых для текущей операции передачи. 

- *Всего передач репликации тома*   
    Совокупное число байтов, передаваемых для связи. 

## <a name="next-steps"></a>Дальнейшие действия

* [Общие сведения об иерархии хранилища Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Настройка пула емкости](azure-netapp-files-set-up-capacity-pool.md)
* [Создание тома для Azure NetApp Files](azure-netapp-files-create-volumes.md)
