---
title: Краткое руководство. Настройка брандмауэра для сервера Azure Analysis Services | Документация Майкрософт
description: Это краткое руководство поможет настроить брандмауэр для сервера Azure Analysis Services с помощью портала Azure.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 08/12/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: e4953137cf939c35c6ac73fe51ca43eca6e99edc
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "88192434"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Краткое руководство. Настройка брандмауэра сервера с помощью портала

Это краткое руководство поможет настроить брандмауэр для сервера Azure Analysis Services. Включение брандмауэра и настройка диапазонов IP-адресов только для тех компьютеров, которые имеют доступ к серверу, является важной частью процесса защиты сервера и данных.

## <a name="prerequisites"></a>Предварительные требования

- Подписка на сервер служб Analysis Services. Дополнительные сведения см. в статье [ Краткое руководство по созданию сервера с помощью портала](analysis-services-create-server.md) или [ Краткое руководство по созданию сервера с помощью PowerShell](analysis-services-create-powershell.md).
- Один или несколько диапазонов IP-адресов для клиентских компьютеров (при необходимости).

> [!NOTE]
> Импорт данных (обновление) и подключения отчетов с разбивкой на страницы из Power BI Premium в Microsoft Cloud Germany в настоящее время не поддерживаются, если включен брандмауэр, даже при включенном параметре "Разрешить доступ из Power BI".

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure 

[Войдите на портал](https://portal.azure.com).

## <a name="configure-a-firewall"></a>Настройка брандмауэра

1. Чтобы открыть страницу обзора, щелкните сервер. 
2. В разделе **Параметры** > **Брандмауэр** > **Включить брандмауэр** выберите **Включить**.
3. Чтобы включить подключения из Power BI и Power BI Premium, для параметра **Разрешить доступ из Power BI** выберите значение **Включить**.  
4. (Необязательно) Укажите один или несколько диапазонов IP-адресов. Введите имя, начальный и конечный IP-адрес для каждого из диапазонов. Длина имени правила брандмауэра не должна превышать 128 знаков и может содержать только прописные и строчные буквы, цифры, подчеркивания и дефис. Пробелы и другие специальные символы не допускаются.
5. Щелкните **Сохранить**.

     ![Параметры брандмауэра](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Удалите диапазоны IP-адресов или отключите брандмауэр, когда они больше не нужны.

## <a name="next-steps"></a>Дальнейшие действия
Из этого краткого руководства вы узнали, как настроить брандмауэр для сервера. Теперь, когда сервер защищен брандмауэром, можно добавить к нему базовый образец модели данных с портала. Наличие образца модели полезно, чтобы изучить настройку ролей шаблонов базы данных и тестирование клиентских подключений. Для получения дополнительных сведений перейдите к руководству по добавлению образца модели.

> [!div class="nextstepaction"]
> [Руководство. Добавление образца модели на сервер](analysis-services-create-sample-model.md)
