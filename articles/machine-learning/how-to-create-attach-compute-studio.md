---
title: Создание обучающих & развертывание вычислений (Studio)
titleSuffix: Azure Machine Learning
description: Использование студии для создания ресурсов для обучения и развертывания (целевые объекты вычислений) для машинного обучения
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.date: 08/06/2020
ms.topic: conceptual
ms.custom: how-to, contperfq1
ms.openlocfilehash: 6cb455880852295d7176e813208a93919a2c14bb
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2020
ms.locfileid: "93318257"
---
# <a name="create-compute-targets-for-model-training-and-deployment-in-azure-machine-learning-studio"></a>Создание целевых объектов вычислений для обучения и развертывания моделей в Машинное обучение Azure Studio

Из этой статьи вы узнаете, как создавать целевые объекты вычислений и управлять ими в Azure Machine Studio.  Вы также можете создавать целевые объекты вычислений и управлять ими с помощью:

* Машинное обучение Azure обучающий пакет SDK или расширение CLI для Машинное обучение Azure
  * [Вычислительная операция](how-to-create-manage-compute-instance.md)
  * [Вычислительный кластер](how-to-create-attach-compute-cluster.md)
  * [Кластер службы Kubernetes Azure](how-to-create-attach-kubernetes.md)
  * [Другие ресурсы вычислений](how-to-attach-compute-targets.md)
* [Расширение VS Code](how-to-manage-resources-vscode.md#compute-clusters) для машинное обучение Azure.


## <a name="prerequisites"></a>Предварительные требования

* Если у вас еще нет подписки Azure, создайте бесплатную учетную запись, прежде чем начинать работу. Опробуйте [бесплатную или платную версию машинное обучение Azure](https://aka.ms/AMLFree) уже сегодня
* [Рабочая область машинное обучение Azure](how-to-manage-workspace.md)

## <a name="whats-a-compute-target"></a>Что такое целевой объект вычислений? 

С помощью решения "Машинное обучение Azure" вы можете обучать модель, используя разные вычислительные ресурсы или среды, которые вместе называются [__целевыми объектами вычислений__](concept-azure-machine-learning-architecture.md#compute-targets). Они могут быть локальными или облачными. Например, это может быть Вычислительная среда Машинного обучения Azure, Azure HDInsight или удаленная виртуальная машина.  Можно также создать целевые объекты вычислений для развертывания моделей, как описано в статье [Развертывание моделей с помощью Службы машинного обучения Azure](how-to-deploy-and-where.md).

## <a name="view-compute-targets"></a><a id="portal-view"></a>Просмотр целевых объектов вычислений

Чтобы просмотреть все целевые объекты вычислений для рабочей области, выполните следующие действия.

1. Перейдите в [Студию машинного обучения Azure](https://ml.azure.com).
 
1. В разделе __Управление__ выберите пункт __вычислить__.

1. Выберите вкладки вверху, чтобы отобразить каждый тип целевого объекта вычислений.

    :::image type="content" source="media/how-to-create-attach-studio/view-compute-targets.png" alt-text="Просмотреть список целевых объектов вычислений":::

## <a name="create-compute-target"></a><a id="portal-create"></a>Создание целевого объекта вычислений

Выполните указанные выше шаги, чтобы просмотреть список целевых объектов вычислений. Затем выполните следующие действия, чтобы создать целевой объект вычислений.

1. Выберите вкладку вверху, соответствующую типу создаваемого вычислений.

1. Если целевые объекты вычислений отсутствуют, выберите  **создать** в середине страницы.
  
    :::image type="content" source="media/how-to-create-attach-studio/create-compute-target.png" alt-text="Создание целевого объекта вычислений":::

1. Если отображается список ресурсов вычислений, выберите **+ создать** над списком.

    :::image type="content" source="media/how-to-create-attach-studio/select-new.png" alt-text="Выберите &quot;создать&quot;":::


1. Заполните форму для типа вычислений:

  * [Вычислительная операция](#compute-instance)
  * [Вычислительные кластеры](#amlcompute)
  * [Кластеры вывода](#inference-clusters)
  * [Присоединенные вычислений](#attached-compute)

1. Нажмите кнопку __создания__.

1. Чтобы увидеть состояние операции создания, выберите целевой объект вычислений из списка.

    :::image type="content" source="media/how-to-create-attach-studio/view-list.png" alt-text="Просмотр состояния вычислений из списка":::


### <a name="compute-instance"></a>Вычислительная операция

Выполните [описанные выше действия](#portal-create) , чтобы создать вычислительный экземпляр.  Затем заполните форму следующим образом:

:::image type="content" source="media/concept-compute-instance/create-compute-instance.png" alt-text="Создание нового вычислительного экземпляра":::


|Поле  |Описание  |
|---------|---------|
|Имя вычислительной среды     |  <li>Имя является обязательным и должно иметь длину от 3 до 24 символов.</li><li>Допустимыми символами являются буквы верхнего и нижнего регистра, цифры и  **-** символ.</li><li>Имя должно начинаться с буквы</li><li>Имя должно быть уникальным среди всех существующих вычислений в регионе Azure. Вы увидите оповещение, если выбранное имя не является уникальным</li><li>Если **-**  используется символ, необходимо следовать по крайней мере одну букву в имени.</li>     |
|Тип виртуальной машины |  Выберите ЦП или GPU. Этот тип нельзя изменить после создания     |
|размер виртуальной машины;     |  Поддерживаемые размеры виртуальных машин могут быть ограничены в вашем регионе. Проверка [списка доступности](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines)     |
|Включить или отключить доступ по протоколу SSH     |   По умолчанию доступ по протоколу SSH отключен.  Доступ по протоколу SSH не может быть. изменено после создания. Обязательно включите доступ, если планируется интерактивное Отладка с помощью [VS Code Remote](how-to-set-up-vs-code-remote.md)   |
|Дополнительные параметры     |  Необязательный параметр. Настройте виртуальную сеть. Укажите **группу ресурсов** , **виртуальную сеть** и **подсеть** , чтобы создать вычислительный экземпляр в виртуальной сети Azure. Дополнительные сведения см. в описании этих [сетевых требований](./how-to-secure-training-vnet.md) для виртуальной сети.  |

### <a name="compute-clusters"></a><a name="amlcompute"></a> Кластеры вычислений

Создайте кластер с одним или несколькими узлами для обучения, обработки пакетов или рабочих нагрузок подкреплением Learning. Выполните [описанные выше действия](#portal-create) , чтобы создать кластер вычислений.  Затем заполните форму следующим образом:


|Поле  |Описание  |
|---------|---------|
|Имя вычислительной среды     |  <li>Имя является обязательным и должно иметь длину от 3 до 24 символов.</li><li>Допустимыми символами являются буквы верхнего и нижнего регистра, цифры и  **-** символ.</li><li>Имя должно начинаться с буквы</li><li>Имя должно быть уникальным среди всех существующих вычислений в регионе Azure. Вы увидите оповещение, если выбранное имя не является уникальным</li><li>Если **-**  используется символ, необходимо следовать по крайней мере одну букву в имени.</li>     |
|Тип виртуальной машины |  Выберите ЦП или GPU. Этот тип нельзя изменить после создания     |
|Приоритет виртуальной машины | Выберите **выделенный** или **низкий приоритет**.  Виртуальные машины с низким приоритетом являются более дешевыми, но не гарантируют расчетные узлы. Задание может быть вытеснено.
|размер виртуальной машины;     |  Поддерживаемые размеры виртуальных машин могут быть ограничены в вашем регионе. Проверка [списка доступности](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines)     |
|Минимальное количество узлов | Минимальное число узлов, которые вы хотите подготавливать. Если необходимо выделить выделенное количество узлов, установите это число здесь. Сэкономьте деньги, установив минимальное значение 0, чтобы не платить за какие-либо узлы, когда кластер бездействует. |
|Максимальное число узлов | Максимальное количество узлов, которые вы хотите подготавливать. Вычисление автоматически масштабируется до максимального значения этого числа узлов при отправке задания. |
|Дополнительные параметры     |  Необязательный параметр. Настройте виртуальную сеть. Укажите **группу ресурсов** , **виртуальную сеть** и **подсеть** , чтобы создать вычислительный экземпляр в виртуальной сети Azure. Дополнительные сведения см. в описании этих [сетевых требований](./how-to-secure-training-vnet.md) для виртуальной сети.   Также присоедините [управляемые удостоверения](#managed-identity) для предоставления доступа к ресурсам.     |

#### <a name="set-up-managed-identity"></a><a name="managed-identity"></a> Настройка управляемого удостоверения

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-managed-identity-intro.md)]

Во время создания кластера или при изменении сведений о кластере вычислений в **дополнительных параметрах** **установите переключатель назначить управляемое удостоверение** и укажите назначенное системой удостоверение или удостоверение, назначенное пользователем.

#### <a name="managed-identity-usage"></a>Использование управляемого удостоверения

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-managed-identity-default.md)]

### <a name="inference-clusters"></a>Кластеры вывода

> [!IMPORTANT]
> Использование службы Kubernetes Azure с Машинное обучение Azure имеет несколько параметров конфигурации. В некоторых сценариях, например в сети, требуется дополнительная установка и настройка. Дополнительные сведения об использовании AKS с Azure ML см. в статье [Создание и подключение кластера службы Kubernetes Azure](how-to-create-attach-kubernetes.md).

Создание или присоединение кластера Azure Kubernetes Service (AKS) для крупномасштабного увеличения масштаба. Выполните [описанные выше действия](#portal-create) , чтобы создать кластер AKS.  Затем заполните форму следующим образом:


|Поле  |Описание  |
|---------|---------|
|Имя вычислительной среды     |  <li>Требуется указать имя. Имя должно содержать от 2 до 16 символов. </li><li>Допустимыми символами являются буквы верхнего и нижнего регистра, цифры и  **-** символ.</li><li>Имя должно начинаться с буквы</li><li>Имя должно быть уникальным среди всех существующих вычислений в регионе Azure. Вы увидите оповещение, если выбранное имя не является уникальным</li><li>Если **-**  используется символ, необходимо следовать по крайней мере одну букву в имени.</li>     |
|Служба Kubernetes | Выберите **создать** и заполните оставшуюся часть формы.  Или выберите **использовать существующий** , а затем выберите существующий кластер AKS из подписки.
|Регион |  Выберите регион, в котором будет создан кластер |
|размер виртуальной машины;     |  Поддерживаемые размеры виртуальных машин могут быть ограничены в вашем регионе. Проверка [списка доступности](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines)     |
|Назначение кластера  | Выбор **рабочей среды** или **разработки — тестирование** |
|Количество узлов | Число узлов, умноженное на число ядер виртуальной машины (виртуальных ЦП), должно быть больше или равно 12. |
| Сетевая конфигурация | Выберите **Дополнительно** , чтобы создать вычисление в существующей виртуальной сети. Дополнительные сведения о AKS в виртуальной сети см. в разделе [Сетевая изоляция во время обучения и вывода с частными конечными точками и виртуальными сетями](./how-to-secure-inferencing-vnet.md). |
| Включить конфигурацию SSL | Используйте этот параметр, чтобы настроить SSL-сертификат на основе вычислений |

### <a name="attached-compute"></a>Присоединенные вычислений

Чтобы использовать целевые объекты вычислений, созданные за пределами рабочей области Машинного обучения Azure, необходимо их присоединить. Присоединение целевого объекта вычислений сделает его доступным для рабочей области.  Используйте **присоединенные вычислений** для присоединения целевого объекта вычислений для **обучения**.  Используйте **кластеры вывода** , чтобы ПОДКЛЮЧИТЬ кластер AKS для **вывода.**

Чтобы присоединить вычисление, выполните [описанные выше действия](#portal-create) .  Затем заполните форму следующим образом:

1. Введите имя для целевого объекта вычислений. 
1. Выберите тип высоединяемых вычислений. Не все типы вычислительных сред можно присоединить из Студии машинного обучения Azure. Типы вычислительных сред, которые в настоящее время можно подключить для обучения:
    * Удаленная виртуальная машина.
    * Azure Databricks (для использования в конвейерах машинного обучения).
    * Azure Data Lake Analytics (для использования в конвейерах машинного обучения).
    * Azure HDInsight

1. Заполните форму и укажите значения для обязательных свойств.

    > [!NOTE]
    > Корпорация Майкрософт рекомендует использовать ключи SSH, так как они безопаснее, чем пароли. Пароли подвержены атакам методом подбора. Ключи SSH задействуют криптографические подписи. Сведения о создании ключей SSH для использования на Виртуальных машинах Azure см. в следующих документах:
    >
    > * [Создание и использование ключей SSH в Linux или macOS](../virtual-machines/linux/mac-create-ssh-keys.md)
    > * [Создание и использование ключей SSH в Windows](../virtual-machines/linux/ssh-from-windows.md)

1. Выберите __Подключить__. 


## <a name="next-steps"></a>Дальнейшие действия

После создания целевого объекта и его присоединения к рабочей области его можно использовать в [конфигурации запуска](how-to-set-up-training-targets.md) с `ComputeTarget` объектом:

```python
from azureml.core.compute import ComputeTarget
myvm = ComputeTarget(workspace=ws, name='my-vm-name')
```

* Используйте ресурс COMPUTE для [отправки обучающего запуска](how-to-set-up-training-targets.md).
* [Учебник. Обучение модели](tutorial-train-models-with-aml.md) использует управляемый целевой объект вычислений для обучения модели.
* Узнайте, как [эффективно настроить гиперпараметры](how-to-tune-hyperparameters.md) для создания улучшенных моделей.
* После обучения модели узнайте о [способах и расположениях развертывания моделей](how-to-deploy-and-where.md).
* [Использование Машинного обучения Azure с виртуальными сетями Microsoft Azure](./how-to-network-security-overview.md)