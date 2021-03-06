---
title: Техническая конфигурация тестового диска, Microsoft для коммерческого рынка
description: Сведения о тестовых дисках. Тестовые диски позволяют новым клиентам тестировать ваше предложение перед принятием покупки.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 08/13/2019
author: trkeya
ms.author: trkeya
ms.openlocfilehash: b3f46f934241d924789b97c24cf9b68213d94d63
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94490089"
---
# <a name="test-drive-technical-configuration"></a>Техническая конфигурация тестового выпуска

Параметр тестовый выпуск в коммерческом магазине Майкрософт позволяет настроить практический обзор основных функций продукта. С помощью тестового диска новые клиенты могут опробовать ваше предложение, прежде чем приобретать его. Дополнительные сведения см. в статье о [тестовом выпуске](what-is-test-drive.md).

Если вы больше не хотите предоставлять тестовый выпуск для вашего предложения, вернитесь на страницу **настройки предложения** и снимите флажок **включить тестовый диск** . Не все типы предложений имеют доступ к тестовому диску.

## <a name="azure-resource-manager-test-drive"></a>Azure Resource Manager тестовый накопитель

Это единственный вариант тестового диска для виртуальных машин или предложений приложений Azure, требующий довольно детальной настройки. Ознакомьтесь с приведенными ниже разделами, посвященными [сведениям о подписке на развертывание](#deployment-subscription-details) и [тестированию списков дисков](#test-drive-listings), а затем перейдите к отдельному разделу [Azure Resource Manager конфигурации тестового диска](azure-resource-manager-test-drive.md)

## <a name="hosted-test-drive"></a>Размещенный тестовый диск

Корпорация Майкрософт может удалить сложность настройки тестового диска, разместив и поддерживая подготовку и подготовку службы. Конфигурация для этого типа тестового диска одинакова независимо от того, предназначен ли тестовый выпуск для аудитории Dynamics 365 или Dynamics 365.

- **Максимальное число одновременных тестовых выпусков** (обязательно) — задайте максимальное количество клиентов, которые могут использовать тестовый выпуск одновременно. Каждый одновременный пользователь будет использовать лицензию Dynamics 365, пока тестовый накопитель активен, поэтому убедитесь, что у вас достаточно лицензий для поддержки максимального набора ограничений. Мы рекомендуем использовать значение 3–5.

- **Длительность тестового диска** (обязательно) — введите число часов, в течение которых тестовый диск будет оставаться активным для каждого клиента. По истечении этого периода сеанс завершится и больше не будет использовать одну из ваших лицензий. Рекомендуемое значение: 2–24 часа (в зависимости от сложности предложения). Эта длительность может быть задана только в целых часах (например, "2 часа"). "1,5 ч" — нет). Пользователь всегда может запросить новый сеанс, если ему не хватило времени и требуется снова получить доступ к тестовому выпуску.

- **URL-адрес экземпляра** (обязательно) — URL-адрес, по которому клиент начнет работу с тестовым выпуском. Как правило, это URL-адрес экземпляра Dynamics 365, в котором установлено приложение и содержатся демонстрационные данные (например, `https://testdrive.crm.dynamics.com`).

- **URL-адрес Web API экземпляра** (обязательно). Извлеките URL-адрес для экземпляра Dynamics 365, войдя в учетную запись Microsoft 365 и перейдя к **параметрам**  >  **Настройка** настройки  >  **ресурсов разработчика**  >  **веб-API (корневой URL-адрес службы)** , скопируйте URL-адрес, найденный здесь (например, `https://testdrive.crm.dynamics.com/api/data/v9.0` ).

- **Имя роли** (обязательно) — укажите имя роли безопасности, которую вы определили в пользовательском тестовом выпуске Dynamics 365 и которая будет назначаться пользователю во время работы с тестовым выпуском (например, test-drive-role).

Чтобы получить справку по настройке среды Dynamics 365 для тестового диска и предоставлении разрешения AppSource для подготовки и отмены подготовки пользователей тестовых дисков в клиенте, следуйте [этим инструкциям](https://github.com/Microsoft/AppSource/blob/patch-1/Microsoft%20Hosted%20Test%20Drive/Setup-your-Azure-subscription-for-Dynamics365-Microsoft-Hosted-Test-Drives.md).

## <a name="logic-app-test-drive"></a>Тестовый накопитель приложения логики

Этот тип тестового диска не размещается корпорацией Майкрософт. Используйте его для подключения к предложению Dynamics 365 или другому настраиваемому ресурсу, охватывающему множество сложных архитектур решений. Дополнительные сведения о настройке тестовых выпусков приложения логики см. в разделах [Operations](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) и [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) на GitHub.

- **Регион** (обязательно, раскрывающийся список в одиночным выбором) — в настоящее время Azure поддерживает 26 регионов для тестового выпуска. Ресурсы для приложения логики будут развернуты в выбранном регионе. Если приложение логики содержит пользовательские ресурсы, которые хранятся в определенном регионе, здесь следует указать этот регион. Рекомендуется полностью развернуть приложение логики локально в подписке Azure на портале и проверить, правильно ли оно работает, прежде чем делать выбор.

- **Максимальное число одновременных тестовых выпусков** (обязательно) — задайте максимальное количество клиентов, которые могут использовать тестовый выпуск одновременно. Эти тестовые выпуски уже развернуты, что позволяет клиентам мгновенно получать доступ к ним, не ожидая развертывания.

- **Продолжительность тестового выпуска** (обязательно) — укажите время, в течение которого тестовый выпуск будет оставаться активным, в часах. Тестовый выпуск автоматически завершит работу по истечении этого периода.

- **Имя группы ресурсов Azure** (обязательно) — введите имя [группы ресурсов Azure](../azure-resource-manager/management/overview.md#resource-groups), в которой будет сохранен тестовый диск приложения логики.

- **Имя приложения логики Azure** (обязательно) — введите имя приложения логики, которое назначает пользователю тестовый выпуск. Это приложение логики должно быть сохранено в группе ресурсов Azure выше.

- **Отменить подготовку имени приложения логики** (обязательно) — введите имя приложения логики, которое отменяет подготовку тестового диска после завершения работы клиента. Это приложение логики должно быть сохранено в группе ресурсов Azure выше.

## <a name="power-bi-test-drive"></a>Тестовый выпуск Power BI

Продукты, в которых должен содержаться визуальный объект Power BI, могут использовать встроенную ссылку для совместного использования настраиваемой панели мониторинга в качестве тестового выпуска. Дополнительная техническая конфигурация не требуется. Вам нужно всего лишь отправить URL-адрес внедренного Power BI.

Дополнительные сведения о настройке Power BI приложений см. в статье [что такое Power BI приложения?](/power-bi/service-template-apps-overview)

## <a name="deployment-subscription-details"></a>Сведения о подписке для развертывания

Чтобы разрешить Майкрософт развертывание тестового выпуска от вашего имени, создайте и предоставьте отдельную уникальную подписку Azure (не требуется для тестовых выпусков Power BI).

- **Идентификатор подписки Azure** (требуется для Azure Resource Manager и Logic Apps) — введите идентификатор подписки, чтобы предоставить доступ к службам учетных записей Azure для отчетов об использовании ресурсов и выставления счетов. Рекомендуется [создать отдельную подписку Azure](../cost-management-billing/manage/create-subscription.md), которая будет использоваться для тестовых выпусков, если у вас ее еще нет. Чтобы найти идентификаторы подписок Azure, войдите на [портал Azure](https://portal.azure.com/), а затем перейдите на вкладку **Подписки** в меню слева. При выборе вкладки отобразится идентификатор подписки (например, a83645ac-1234-5ab6-6789-1h234g764ghty).

- **Идентификатор клиента Azure AD** (обязательно) — введите [идентификатор клиента](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in)Azure Active Directory (AD). Чтобы найти этот идентификатор, войдите на [портал Azure](https://portal.azure.com/), выберите вкладку Azure Active Directory на панели слева, выберите **Свойства** , а затем найдите номер **Идентификатор каталога** (например, 50c464d3-4930-494c-963c-1e951d15360e). Вы также можете найти идентификатор клиента своей организации, используя адрес доменного имени: [https://www.whatismytenantid.com](https://www.whatismytenantid.com).

- **Имя клиента Azure AD** (обязательно для Dynamic 365) — введите имя Azure Active Directory (AD). Чтобы найти это имя, войдите на [портал Azure](https://portal.azure.com/), в правом верхнем углу имя клиента будет указано под именем учетной записи.

- **Идентификатор приложения Azure AD** (обязательно) — введите [идентификатор приложения](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in)Azure Active Directory (AD). Чтобы найти этот идентификатор, войдите в [портал Azure](https://portal.azure.com/), выберите вкладку Active Directory в меню слева, выберите **Регистрация приложений** , а затем найдите **идентификатор приложения** в списке (например, `50c464d3-4930-494c-963c-1e951d15360e` ).

- **Секрет клиента приложения Azure AD** (обязательно) — введите [секрет клиента](../active-directory/develop/howto-create-service-principal-portal.md#option-2-create-a-new-application-secret)приложения Azure AD. Чтобы найти это значение, войдите на [портал Azure](https://portal.azure.com/). Выберите вкладку **Azure Active Directory** в меню слева, выберите **Регистрация приложений** и выберите приложение тестового диска. Затем выберите **Сертификаты и секреты** , затем **новый секрет клиента** , введите описание, выберите **никогда** в поле **срок действия** и нажмите кнопку **Добавить**. Обязательно скопируйте значение. Не выйдите из страницы перед копированием значения.

## <a name="test-drive-listings"></a>Списки тестовых дисков

Параметр **листингов тестового диска** , расположенный на вкладке **тестовый** выпуск в центре партнеров, отображает языки (и рынки), где доступен тестовый выпуск. в настоящее время доступен английский язык (США) — это единственное доступное расположение. Кроме того, на этой странице отображается состояние публикации для конкретного языка, а также его дата и время добавления. Для каждого языка и рынка необходимо определить сведения о тестовом диске (описание, руководство пользователя, видео и т. д.).

- **Описание** (обязательно): Опишите свой тестовый диск, то, что будет продемонстрировано, цели для экспериментов с, функции для изучения и любые важные сведения, помогающие пользователю определить, следует ли приобрести ваше предложение. В этом поле можно ввести до 3000 символов.

- **Сведения о доступе** (обязательные для Azure Resource Manager и логических тестовых накопителей). Объясните, что необходимо узнать клиенту для доступа к этому тестовому диску и его использования. Опишите сценарий использования вашего предложения и укажите, что должен знать клиент для доступа к функциям в тестовом выпуске. В этом поле можно ввести до 10 000 символов.

- **Руководство пользователя** (обязательно). подробное пошаговое руководство по работе с тестовыми дисками. Руководство пользователя должно описывать, что получит пользователь в тестовом выпуске, и содержать ответы на возможные вопросы. Файл должен быть в формате PDF. Присвойте ему имя (не более 255 символов) после отправки.

- **Видео. Добавление видео** (необязательно). видео, размещенное на другой странице, можно указать с помощью ссылки и эскиза (533 x 324 пикселей), чтобы пользователь мог просмотреть пошаговые инструкции, чтобы помочь им лучше понять тестовый выпуск, включая способы успешного использования функций вашего предложения и понимания сценариев, которые выделяют их преимущества.
  - **Имя** (обязательно)
  - **URL-адрес** (только YouTube или Vimeo; требуется)
  - **Эскиз** (533 x 324 пикселей) — изображение должно быть в формате PNG.

Если в данный момент вы создаете тестовый диск в центре партнеров, выберите **Сохранить черновик** , прежде чем продолжить.

## <a name="next-steps"></a>Следующие шаги

- [Рекомендации по тестированию](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Обзор](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf)(PDF; убедитесь, что блокирование всплывающих окон отключено)
- [Обновление имеющегося предложения на коммерческой платформе Marketplace](partner-center-portal/update-existing-offer.md)
- [Форум обратной связи Azure Marketplace](https://feedback.azure.com/forums/216369-azure-marketplace)