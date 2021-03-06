---
title: Миграция из управляемого образа в версию образа с помощью Azure CLI
description: Узнайте, как выполнить миграцию с управляемого образа на версию образа в общей коллекции образов с помощью Azure CLI.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.custom: devx-track-azurecli
ms.openlocfilehash: 8631a411b26f91bc72e23ac7ff9fb2278f61168c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87502891"
---
# <a name="migrate-from-a-managed-image-to-an-image-version-using-the-azure-cli"></a>Миграция из управляемого образа в версию образа с помощью Azure CLI
Если у вас есть управляемый образ, который вы хотите перенести в коллекцию общих образов, можно создать образ из коллекции общих образов непосредственно из управляемого образа. После тестирования нового образа можно удалить исходный управляемый образ. Вы также можете выполнить миграцию из управляемого образа в общую коллекцию образов с помощью [PowerShell](image-version-managed-image-powershell.md).

Изображения в коллекции образов имеют два компонента, которые будут созданы в этом примере:
- **Определение образа** содержит сведения о образе и требования для его использования. Сюда входит, является ли образ Windows или Linux, специализированным или обобщенным, заметками о выпуске, а также минимальным и максимальным требованиями к памяти. Это определение типа образа. 
- **Версия образа** — это то, что используется для создания виртуальной машины при использовании общей коллекции образов. В зависимости от требований для вашей среды, у вас может быть несколько версий образа. При создании виртуальной машины используется версия образа для создания новых дисков для виртуальной машины. Версии образов можно использовать несколько раз.


## <a name="before-you-begin"></a>Перед началом

Для работы с этой статьей необходимо иметь существующую [коллекцию общих образов](shared-images-cli.md). 

Чтобы выполнить пример из этой статьи, вам нужен существующий управляемый образ универсальной виртуальной машины. Дополнительные сведения см. [в разделе запись управляемого образа](./linux/capture-image.md). Если управляемый образ содержит диск данных, размер диска данных не может превышать 1 ТБ.

При работе со статьей по мере необходимости заменяйте имена групп ресурсов и виртуальных машин.



## <a name="create-an-image-definition"></a>Создание определения образа

Поскольку управляемые образы всегда являются обобщенными образами, вы создадите определение образа с помощью `--os-state generalized` для обобщенного образа.

В имени определения образа можно использовать прописные и строчные буквы, цифры, точки и дефисы. 

Дополнительные сведения о значениях, которые можно указать для определения образа, см. в разделе [Определения образов](./linux/shared-image-galleries.md#image-definitions).

Создайте в коллекции определение образа, используя команду [az sig image-definition create](/cli/azure/sig/image-definition#az-sig-image-definition-create).

В этом примере определение образа называется *мимажедефинитион*и предназначено для [обобщенного](./linux/shared-image-galleries.md#generalized-and-specialized-images) образа ОС Linux. Чтобы создать определение для образов с помощью ОС Windows, используйте `--os-type Windows`. 

```azurecli-interactive 
resourceGroup=myGalleryRG
gallery=myGallery
imageDef=myImageDefinition
az sig image-definition create \
   --resource-group $resourceGroup \
   --gallery-name $gallery \
   --gallery-image-definition $imageDef \
   --publisher myPublisher \
   --offer myOffer \
   --sku mySKU \
   --os-type Linux \
   --os-state generalized
```


## <a name="create-the-image-version"></a>Создание версии образа

Создайте версии с помощью команды [AZ Image Gallery создать-Image-Version](/cli/azure/sig/image-version#az-sig-image-version-create). Вам нужно ввести идентификатор управляемого образа, чтобы использовать его в качестве базового плана для создания версии образа. Для получения идентификаторов образов можно использовать команду [AZ Image List](/cli/azure/image?view#az-image-list) . 

```azurecli-interactive
az image list --query "[].[name, id]" -o tsv
```

Допустимыми знаками для имени версии образа являются цифры и точки. Числа должны быть в диапазоне 32-битного целого числа. Формат: *основной номер версии*.*дополнительный номер версии*.*исправление*.

В этом примере версия нашего образа — *1.0.0* , и мы создадим 1 реплику в *юго-центральном регионе США* и 1 реплику в регионе " *Восточная часть США 2* " с помощью хранилища, избыточного в пределах зоны. При выборе целевых регионов для репликации Помните, что также необходимо включить *Исходный* регион в качестве цели для репликации.

Передайте идентификатор управляемого образа в `--managed-image` параметре.


```azurecli-interactive 
imageID="/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
az sig image-version create \
   --resource-group $resourceGroup \
   --gallery-name $gallery \
   --gallery-image-definition $imageDef \
   --gallery-image-version 1.0.0 \
   --target-regions "southcentralus=1" "eastus2=1=standard_zrs" \
   --replica-count 2 \
   --managed-image $imageID
```

> [!NOTE]
> Прежде чем использовать тот же управляемый образ для создания другой версии образа, необходимо дождаться завершения сборки и репликации версии образа.
>
> Вы также можете хранить все реплики версии образа в хранилище, [избыточном](../storage/common/storage-redundancy.md) в виде зоны, путем добавления `--storage-account-type standard_zrs` при создании версии образа.
>

## <a name="next-steps"></a>Дальнейшие действия

Создайте виртуальную машину из [обобщенной версии образа](vm-generalized-image-version-cli.md).

Сведения о том, как предоставить сведения о плане покупки, см. в разделе [предоставление сведений о плане покупки Azure Marketplace при создании образов](marketplace-images.md).
