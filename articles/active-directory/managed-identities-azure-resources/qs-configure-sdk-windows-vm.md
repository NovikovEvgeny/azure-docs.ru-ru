---
title: Настройка управляемых удостоверений на виртуальной машине с помощью пакета SDK — Azure AD
description: Пошаговые инструкции по настройке и использованию управляемых удостоверений для ресурсов Azure на виртуальной машине Azure с помощью пакета SDK для Azure.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/28/2017
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6b4b55fa5a21ce56d27505a724bcf08f1a893c71
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "89003462"
---
# <a name="configure-a-vm-with-managed-identities-for-azure-resources-using-an-azure-sdk"></a>Настройка управляемых удостоверений для ресурсов Azure на виртуальной машине Azure с помощью пакета SDK для Azure

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Функция управляемых удостоверений для ресурсов Azure предоставляет службам Azure автоматически управляемые удостоверения в Azure Active Directory (AD). Это удостоверение можно использовать для аутентификации в любой службе, которая поддерживает аутентификацию Azure AD, не храня какие-либо учетные данные в коде. 

В этой статье вы узнаете, как включать и удалять управляемые удостоверения для ресурсов Azure на виртуальной машины Azure с помощью пакета SDK для Azure.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-managed-identities-for-azure-resources-support"></a>Пакеты SDK для Azure с поддержкой управляемых удостоверений для ресурсов Azure 

Благодаря [пакетам SDK Azure](https://azure.microsoft.com/downloads) в Azure реализована поддержка разных платформ программирования. Некоторые из них обновлены для поддержки управляемых удостоверений для ресурсов Azure. Кроме того, для них предоставляются соответствующие примеры использования. Этот список обновляется по мере добавления поддержки дополнительных платформ:

| SDK | Образец |
| --- | ------ | 
| .NET   | [Управление ресурсами из виртуальной машины, на которой включены управляемые удостоверения для ресурсов Azure](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Управление хранилищем из виртуальной машины, на которой включены управляемые удостоверения для ресурсов Azure](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Создание виртуальной машины с включенным управляемым удостоверением, назначаемым системой](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Создание виртуальной машины с включенным управляемым удостоверением, назначаемым системой](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Создание виртуальной машины Azure с включенным управляемым удостоверением, назначаемым системой](https://github.com/Azure-Samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Дальнейшие действия

- Сведения о том, как использовать портал Azure, PowerShell, интерфейс командной строки и шаблоны ресурсов, приведены в соответствующих статьях раздела **Configure Identity for an Azure VM** (Настройка удостоверения для виртуальной машины Azure).
