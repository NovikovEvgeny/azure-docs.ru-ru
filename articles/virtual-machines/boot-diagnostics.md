---
title: Диагностика загрузки Azure
description: Общие сведения о диагностике загрузки Azure и управляемой диагностике загрузки
services: virtual-machines
ms.service: virtual-machines
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.date: 11/06/2020
ms.openlocfilehash: 1dcefefe02d91506c494cdf91e75ca951ccf43bb
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2020
ms.locfileid: "94365476"
---
# <a name="azure-boot-diagnostics"></a>Диагностика загрузки Azure

Диагностика загрузки — это функция отладки для виртуальных машин Azure, которая позволяет выполнять диагностику сбоев при загрузке ВИРТУАЛЬНЫХ машин. Диагностика загрузки позволяет пользователю наблюдать за состоянием виртуальной машины при загрузке, собирая данные последовательного журнала и снимки экрана.

## <a name="boot-diagnostics-storage-account"></a>Учетная запись хранения для диагностики загрузки
При создании виртуальной машины в портал Azure Диагностика загрузки включается по умолчанию. Рекомендуемая Диагностика загрузки заключается в использовании управляемой учетной записи хранения, так как она дает значительные улучшения производительности в течение времени для создания виртуальной машины Azure. Это связано с тем, что будет использоваться управляемая учетная запись хранения Azure, которая удаляет время, необходимое для создания новой учетной записи хранения пользователя для хранения данных диагностики загрузки.

Альтернативным средством диагностики загрузки является использование управляемой пользователем учетной записи хранения. Пользователь может либо создать новую учетную запись хранения, либо использовать существующую. 

> [!IMPORTANT]
> Большие двоичные объекты данных диагностики загрузки (которые состоят из журналов и образов моментальных снимков) хранятся в управляемой учетной записи хранения. Клиентам будет выставляться счет только по используемым ГБ BLOB-объектам, а не к подготовленному размеру диска. Счетчики моментальных снимков будут использоваться для выставления счетов за управляемую учетную запись хранения. Так как управляемые учетные записи создаются на уровне Standard LRS или Standard ZRS, клиентам будет выставляться счет за $0,05/ГБ в месяц для размера только больших двоичных объектов данных диагностики. Дополнительные сведения об этих ценах см. в разделе [цены на управляемые диски](https://azure.microsoft.com/pricing/details/managed-disks/). Клиенты будут видеть, что плата привязана к URI ресурса виртуальной машины. 

## <a name="boot-diagnostics-view"></a>Представление диагностики загрузки
В колонке виртуальной машины параметр Диагностика загрузки находится в разделе *Поддержка и устранение неполадок* в портал Azure. При выборе диагностики загрузки будут отображаться снимок экрана и сведения о последовательных журналах. Последовательный журнал содержит сообщения ядра, и снимок экрана является моментальным снимком текущего состояния виртуальных машин. В зависимости от того, что виртуальная машина работает под управлением Windows или Linux, определяет, как будет выглядеть ожидаемый снимок экрана. Для Windows пользователи увидят фон рабочего стола и Linux, пользователи увидят запрос на вход.

:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-linux.png" alt-text="Снимок экрана диагностики загрузки Linux":::
:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-windows.png" alt-text="Снимок экрана системы диагностики загрузки Windows":::

## <a name="enable-managed-boot-diagnostics"></a>Включение управляемой диагностики загрузки 
Управляемую диагностику загрузки можно включить с помощью шаблонов портал Azure, CLI и ARM. Включение через PowerShell пока не поддерживается. 

### <a name="enable-managed-boot-diagnostics-using-the-azure-portal"></a>Включение управляемой диагностики загрузки с помощью портал Azure
При создании виртуальной машины в портал Azure параметр по умолчанию включает диагностику загрузки с помощью управляемой учетной записи хранения. Чтобы просмотреть это, перейдите на вкладку *Управление* во время создания виртуальной машины. 

:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-enable-portal.png" alt-text="Снимок экрана, позволяющий включить диагностику управляемой загрузки во время создания виртуальной машины.":::

### <a name="enable-managed-boot-diagnostics-using-cli"></a>Включение управляемой диагностики загрузки с помощью интерфейса командной строки
Диагностика загрузки с управляемой учетной записью хранения поддерживается в Azure CLI 2.12.0 и более поздних версий. Если не ввести имя или универсальный код ресурса (URI) для учетной записи хранения, будет использоваться управляемая учетная запись. Дополнительные сведения и примеры кода см. в [документации по CLI для диагностики загрузки](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics?view=azure-cli-latest&preserve-view=true).

### <a name="enable-managed-boot-diagnostics-using-azure-resource-manager-arm-templates"></a>Включение диагностики управляемой загрузки с помощью шаблонов Azure Resource Manager (ARM)
Все компоненты API версии 2020-06-01 поддерживают управляемую диагностику загрузки. Дополнительные сведения см. в разделе [представление экземпляра диагностики загрузки](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#bootdiagnostics).

```ARM Template
            "name": "[parameters('virtualMachineName')]",
            "type": "Microsoft.Compute/virtualMachines",
            "apiVersion": "2020-06-01",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Network/networkInterfaces/', parameters('networkInterfaceName'))]"
            ],
            "properties": {
                "hardwareProfile": {
                    "vmSize": "[parameters('virtualMachineSize')]"
                },
                "storageProfile": {
                    "osDisk": {
                        "createOption": "fromImage",
                        "managedDisk": {
                            "storageAccountType": "[parameters('osDiskType')]"
                        }
                    },
                    "imageReference": {
                        "publisher": "Canonical",
                        "offer": "UbuntuServer",
                        "sku": "18.04-LTS",
                        "version": "latest"
                    }
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces', parameters('networkInterfaceName'))]"
                        }
                    ]
                },
                "osProfile": {
                    "computerName": "[parameters('virtualMachineComputerName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "linuxConfiguration": {
                        "disablePasswordAuthentication": true
                    }
                },
                "diagnosticsProfile": {
                    "bootDiagnostics": {
                        "enabled": true
                    }
                }
            }
        }
    ],

```

## <a name="limitations"></a>Ограничения
- Диагностика загрузки доступна только для Azure Resource Manager виртуальных машин.
- Управляемая Диагностика загрузки не поддерживает виртуальные машины, использующие неуправляемые диски ОС.
- Диагностика загрузки не поддерживает учетные записи хранения класса Premium. Если для диагностики загрузки используется учетная запись хранения класса Premium, `StorageAccountTypeNotSupported` при запуске виртуальной машины будут появляться сообщения об ошибке. 
- Управляемые учетные записи хранения поддерживаются в диспетчер ресурсов API версии "2020-06-01" и более поздних версий.
- Последовательная консоль Azure в настоящее время несовместима с управляемой учетной записью хранения для диагностики загрузки. Дополнительные сведения о [последовательной консоли Azure](./troubleshooting/serial-console-overview.md).
- Портал поддерживает использование диагностики загрузки только с управляемой учетной записью хранения для виртуальных машин с одним экземпляром.

## <a name="next-steps"></a>Дальнейшие шаги

Узнайте больше о [последовательной консоли Azure](./troubleshooting/serial-console-overview.md) и об использовании диагностики загрузки для [устранения неполадок виртуальных машин в Azure](./troubleshooting/boot-diagnostics.md).