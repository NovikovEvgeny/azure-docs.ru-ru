---
title: Сегменты сети VMware HCX
description: Для VMware HCX требуется четыре сети.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 8137b4383d2a243d53db317db6f5a78b3bc68e67
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92173613"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

Для VMware HCX требуется четыре сети:

- **Сеть управления.** Как правило, это та же сеть управления, которая используется в кластере vSphere. Для VMware HCX нужно определить как минимум два IP-адреса в этом сегменте сети. Может потребоваться и больше, в зависимости от развертывания.

- **Сеть vMotion.** Как правило, это та же сеть, которая используется для vMotion в кластере vSphere.  Для VMware HCX нужно определить как минимум два IP-адреса в этом сегменте сети. Может потребоваться и больше, в зависимости от развертывания.  

   Сеть vMotion должна быть представлена на распределенном виртуальном коммутаторе или vSwitch0. Если это не так, измените среду.

   > [!NOTE]
   > Если эта сеть не маршрутизируется (является частной), то это не проблема.

- **Сеть канала исходящей связи.** Вы хотите создать сеть для канала исходящей связи VMware HCX и предоставить ее кластеру vSphere через группу портов. Для VMware HCX нужно определить как минимум два IP-адреса в этом сегменте сети. Может потребоваться и больше, в зависимости от развертывания.  

   > [!NOTE]
   > Рекомендуемый способ — создать сеть типа /29, но возможен любой размер сети.

- **Сеть репликации.** Вы хотите создать сеть для репликации VMware HCX и предоставить эту сеть кластеру vSphere через группу портов. Для VMware HCX нужно определить как минимум два IP-адреса в этом сегменте сети. Может потребоваться и больше, в зависимости от развертывания.

   > [!NOTE]
   > Рекомендуемый способ — создать сеть типа /29, но возможен любой размер сети.