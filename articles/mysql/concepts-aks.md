---
title: Подключение к службе Azure Kubernetes. база данных Azure для MySQL
description: Сведения о подключении Службы Azure Kubernetes к Базе данных Azure для MySQL
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 07/14/2020
ms.openlocfilehash: bc40903de09ebfe04858c631eee0233561784418
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2020
ms.locfileid: "92546914"
---
# <a name="connecting-azure-kubernetes-service-and-azure-database-for-mysql"></a>Подключение Службы Azure Kubernetes к Базе данных Azure для MySQL

Служба Azure Kubernetes (AKS) предоставляет управляемый кластер Kubernetes для использования в Azure. Ниже приведены некоторые настройки, которые можно использовать при создании приложения с помощью AKS и Базы данных Azure для MySQL.


## <a name="accelerated-networking"></a>Ускорение работы в сети
Используйте в кластере AKS базовые виртуальные машины с поддержкой ускоренной работы в сети. Если на виртуальной машине включена ускоренная работа в сети, на машине наблюдается снижение задержки, дрожания и загрузки ЦП. Дополнительные сведения об ускоренной работе в сети, поддерживаемых версиях ОС и экземплярах виртуальных машин Linux см. в статье [Создание виртуальной машины Linux с ускоренной сетью](../virtual-network/create-vm-accelerated-networking-cli.md).

С ноября 2018 года AKS поддерживает ускоренные сети на поддерживаемых экземплярах виртуальных машин. Для новых кластеров AKS, которые используют эти виртуальные машины, ускоренная работа в сети включена по умолчанию.

Чтобы проверить, использует ли ваш кластер AKS ускоренную сеть, сделайте следующее:
1. Перейдите на портал Azure и выберите свой кластер AKS.
2. Выберите вкладку "Свойства".
3. Скопируйте имя **группы ресурсов инфраструктуры** .
4. С помощью панели поиска на портале найдите и откройте группу ресурсов инфраструктуры.
5. Выберите в этой группе ресурсов виртуальную машину.
6. Перейдите на вкладку **Сети** для виртуальной машины.
7. Проверьте, включена ли **ускоренная сеть** .

Или с помощью Azure CLI выполните следующие две команды.
```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query "nodeResourceGroup"
```
Будет получена группа ресурсов, созданная службой AKS и содержащая сетевой интерфейс. Скопируйте имя nodeResourceGroup и используйте его в следующей команде. Значением **EnableAcceleratedNetworking** будет true или false.
```azurecli
az network nic list --resource-group nodeResourceGroup -o table
```


## <a name="next-steps"></a>Дальнейшие действия
- [Создание кластера службы Kubernetes Azure](../aks/kubernetes-walkthrough.md)
- Узнайте, как [установить WordPress из чарта Helm с помощью OSBA и Базы данных Azure для MySQL](../aks/index.yml).