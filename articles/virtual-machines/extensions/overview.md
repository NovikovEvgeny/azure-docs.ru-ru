---
title: Расширения и компоненты виртуальной машины Azure
description: Дополнительные сведения о расширениях виртуальной машины Azure
services: virtual-machines
author: amjads1
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 08/03/2020
ms.author: amjads
ms.openlocfilehash: 73738f339bea3e8e075530e5de564c1c0854d283
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87552056"
---
# <a name="azure-virtual-machine-extensions-and-features"></a>Расширения и компоненты виртуальной машины Azure
Расширения — это небольшие приложения, которые обеспечивают настройку и автоматизацию после развертывания на виртуальных машинах Azure. Платформа Azure содержит множество расширений, охватывающих конфигурацию виртуальных машин, мониторинг, безопасность и служебные приложения. Издатели принимают приложение, заключает его в расширение и упрощают установку. Все, что нужно сделать, — предоставить обязательные параметры. 

## <a name="how-can-i-find-what-extensions-are-available"></a>Как узнать, какие расширения доступны?
Вы можете просмотреть доступные расширения, выбрав виртуальную машину, выбрав **расширения** в меню слева. Полный список расширений см. в статье [Обнаружение РАСШИРЕНИЙ ВМ для Linux](features-linux.md) и [Обнаружение расширений виртуальных машин для Windows](features-windows.md).

## <a name="how-can-i-install-an-extension"></a>Как установить расширение?
Для управления расширениями виртуальной машины Azure можно использовать шаблоны Azure CLI, PowerShell, диспетчер ресурсов и портал Azure. Чтобы проверить расширение, перейдите в портал Azure, выберите расширение пользовательских сценариев, а затем передайте команду или скрипт для запуска расширения.

Дополнительные сведения см. в разделе [расширение пользовательских сценариев Windows](custom-script-windows.md) и [расширение пользовательских сценариев Linux](custom-script-linux.md).

## <a name="how-do-i-manage-extension-application-lifecycle"></a>Как осуществляется управление жизненным циклом приложений расширений?
Вам не нужно напрямую подключаться к виртуальной машине для установки или удаления расширения. Жизненный цикл расширения Azure управляется за пределами виртуальной машины и интегрируется в платформу Azure.

## <a name="anything-else-i-should-be-thinking-about-for-extensions"></a>Что еще следует знать о расширениях?
Для отдельных приложений расширений виртуальных машин могут действовать собственные предварительные требования, например доступ к конечной точке. Каждое расширение содержит статью, в которой объясняются все предварительные требования, включая поддерживаемые операционные системы.

## <a name="troubleshoot-extensions"></a>Устранение неполадок расширений

Сведения об устранении неполадок для каждого расширения можно найти в разделе **Устранение неполадок и поддержка** в обзоре расширения. Ниже приведен список доступных сведений по устранению неполадок.

| Пространство имен | Устранение неполадок |
|-----------|-----------------|
| Microsoft. Azure. Monitoring. депенденциажент. депенденциажентлинукс | [Зависимость от Azure Monitor для Linux](agent-dependency-linux.md#troubleshoot-and-support) |
| Microsoft. Azure. Monitoring. депенденциажент. депенденциажентвиндовс | [Зависимость Azure Monitor для Windows](agent-dependency-windows.md#troubleshoot-and-support) |
| Microsoft. Azure. Security. azurediskencryptionforlinux | [Шифрование дисков Azure для Linux](azure-disk-enc-linux.md#troubleshoot-and-support) |
| Microsoft. Azure. Security. azurediskencryption | [Шифрование дисков Azure для Windows](azure-disk-enc-windows.md#troubleshoot-and-support) |
| Microsoft. COMPUTE. customscriptextension | [Пользовательский скрипт для Windows](custom-script-windows.md#troubleshoot-and-support) |
| Microsoft. ostcextensions. customscriptforlinux | [Настройка требуемого состояния для Linux](dsc-linux.md#troubleshoot-and-support) |
| Microsoft. PowerShell. DSC | [Настройка требуемого состояния для Windows](dsc-windows.md#troubleshoot-and-support) |
| Microsoft. хпккомпуте. нвидиагпудриверлинукс | [Расширение драйвера GPU NVIDIA для Linux](hpccompute-gpu-linux.md#troubleshoot-and-support) |
| Microsoft. хпккомпуте. нвидиагпудривервиндовс | [Расширение драйвера GPU NVIDIA для Windows](hpccompute-gpu-windows.md#troubleshoot-and-support) |
| Microsoft. Azure. Security. iaasantimalware | [Расширение антивредоносной программы для Windows](iaas-antimalware-windows.md#troubleshoot-and-support) |
| Microsoft. ентерприсеклауд. Monitoring. omsagentforlinux | [Azure Monitor для Linux](oms-linux.md#troubleshoot-and-support)
| Microsoft. ентерприсеклауд. Monitoring. расширение microsoftmonitoringagent | [Azure Monitor для Windows](oms-windows.md#troubleshoot-and-support) |
| stackify. линуксажент. extension. стаккифилинуксажентекстенсион | [Перетрассировка Stackify для Linux](stackify-retrace-linux.md#troubleshoot-and-support) |
| vmaccessforlinux. Microsoft. ostcextensions | [Сброс пароля для Linux](vmaccess.md#troubleshoot-and-support) |
| Microsoft. recoveryservices. vmsnapshot | [Моментальный снимок для Linux](vmsnapshot-linux.md#troubleshoot-and-support) |
| Microsoft. recoveryservices. vmsnapshot | [Моментальный снимок для Windows](vmsnapshot-windows.md#troubleshoot-and-support) |


## <a name="next-steps"></a>Дальнейшие шаги
* Дополнительные сведения о работе агента и расширений Linux см. в статье [расширения и компоненты виртуальной машины Azure для Linux](features-linux.md).
* Дополнительные сведения о работе гостевого агента и расширений Windows см. в статье [расширения и компоненты виртуальной машины Azure для Windows](features-windows.md).  
* Сведения об установке гостевого агента Windows см. в статье [Обзор агента виртуальных машин Windows в Azure](agent-windows.md).  
* Сведения об установке агента Linux см. в статье [Обзор агента виртуальной машины Linux в Azure](agent-linux.md).  

