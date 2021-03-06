---
title: Представление журналов kubelet в Службе Azure Kubernetes (AKS)
description: Узнайте, как просматривать сведения об устранении неполадок в журналах kubelet из узлов службы Kubernetes Azure (AKS).
services: container-service
ms.topic: article
ms.date: 03/05/2019
ms.openlocfilehash: 2ef38aa76f4ef9152e7bc55a1d74c84ef426f0ac
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87056754"
---
# <a name="get-kubelet-logs-from-azure-kubernetes-service-aks-cluster-nodes"></a>Получение журналов kubelet из узлов кластера Службы Azure Kubernetes (AKS)

В рамках работы кластера AKS может потребоваться проверить журналы, чтобы устранить проблему. Встроенная портал Azure — возможность просмотра журналов для [главных компонентов][aks-master-logs] или [контейнеров AKS в кластере AKS][azure-container-logs]. Иногда для устранения неполадок может потребоваться получить журналы *kubelet* из узла AKS.

В этой статье показано, как можно использовать `journalctl` для просмотра журналов *kubelet* на узле AKS.

## <a name="before-you-begin"></a>Перед началом

В этой статье предполагается, что у вас есть кластер AKS. Если вам нужен кластер AKS, обратитесь к краткому руководству по работе с AKS [с помощью Azure CLI][aks-quickstart-cli] или [портала Azure][aks-quickstart-portal].

## <a name="create-an-ssh-connection"></a>Создание SSH-подключения

Сначала создайте SSH-подключение к узлу, из которого необходимо получить журналы *kubelet*. Эта операция описана в руководстве по [входу через SSH в узлы кластера Службы Azure Kubernetes (AKS)][aks-ssh].

## <a name="get-kubelet-logs"></a>Получение журналов kubelet

После подключения к узлу выполните следующую команду, чтобы извлечь журналы *kubelet* :

```console
sudo journalctl -u kubelet -o cat
```

> [!NOTE]
> Для узлов Windows данные журнала находятся в `C:\k` и могут быть просмотрены с помощью команды *More* :
> ```
> more C:\k\kubelet.log
> ```

Пример выходных данных указывает на данные журнала *kubelet*.

```
I0508 12:26:17.905042    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:27.943494    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:28.920125    8672 server.go:796] GET /stats/summary: (10.370874ms) 200 [[Ruby] 10.244.0.2:52292]
I0508 12:26:37.964650    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:47.996449    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:58.019746    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:05.107680    8672 server.go:796] GET /stats/summary/: (24.853838ms) 200 [[Go-http-client/1.1] 10.244.0.3:44660]
I0508 12:27:08.041736    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:18.068505    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:28.094889    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:38.121346    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:44.015205    8672 server.go:796] GET /stats/summary: (30.236824ms) 200 [[Ruby] 10.244.0.2:52588]
I0508 12:27:48.145640    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:58.178534    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:05.040375    8672 server.go:796] GET /stats/summary/: (27.78503ms) 200 [[Go-http-client/1.1] 10.244.0.3:44660]
I0508 12:28:08.214158    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:18.242160    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:28.274408    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:38.296074    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:48.321952    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:58.344656    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об устранении неполадок с главного сервера Kubernetes, см. в статье [Включение и просмотр журналов главного узла Kubernetes в Службе Azure Kubernetes (AKS)][aks-master-logs].

<!-- LINKS - internal -->
[aks-ssh]: ssh.md
[aks-master-logs]: view-master-logs.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-master-logs]: view-master-logs.md
[azure-container-logs]: ../azure-monitor/insights/container-insights-overview.md
