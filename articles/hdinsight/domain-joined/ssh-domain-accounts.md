---
title: Управление доступом по протоколу SSH для учетных записей домена в Azure HDInsight
description: Действия по управлению доступом по протоколу SSH для учетных записей Azure AD в HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 02/14/2020
ms.openlocfilehash: 5be992ef8375f98b3c5978d8b71dc92ce9f91123
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86081508"
---
# <a name="manage-ssh-access-for-domain-accounts-in-azure-hdinsight"></a>Управление доступом по протоколу SSH для учетных записей домена в Azure HDInsight

В защищенных кластерах по умолчанию всем пользователям домена в [Azure AD DS](../../active-directory-domain-services/overview.md) разрешено подключаться по протоколу [SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) к головным и граничным узлам. Эти пользователи не входят в группу "Sudo" и не получают доступ к корневому каталогу. Пользователь SSH, созданный во время создания кластера, будет иметь доступ с правами root.

## <a name="manage-access"></a>Управление доступом

Чтобы изменить доступ по протоколу SSH к конкретным пользователям или группам, обновите `/etc/ssh/sshd_config` на каждом из узлов.

1. С помощью команды [ssh command](../hdinsight-hadoop-linux-use-ssh-unix.md) подключитесь к кластеру. Измените приведенную ниже команду, заменив CLUSTERNAME именем своего кластера, а затем введите команду:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Откройте `ssh_confi` файл g.

    ```bash
    sudo nano /etc/ssh/sshd_config
    ```

1. `sshd_config`При необходимости измените файл. Если ограничить пользователей определенными группами, локальные учетные записи не смогут получить доступ к этому узлу по протоколу SSH. Ниже приведен пример синтаксиса.

    ```bash
    AllowUsers useralias1 useralias2

    AllowGroups groupname1 groupname2
    ```

    Затем сохраните изменения: **CTRL + X**, **Y**, **Ввод**.

1. Перезапустите sshd.

    ```bash
    sudo systemctl restart sshd
    ```

1. Повторите описанные выше шаги для каждого узла.

## <a name="ssh-authentication-log"></a>Журнал проверки подлинности SSH

Журнал проверки подлинности SSH записывается в `/var/log/auth.log` . Если вы видите ошибки входа в систему по протоколу SSH для учетных записей локальной организации или домена, для отладки ошибок необходимо пройти журнал. Часто эта ошибка может быть связана с конкретными учетными записями пользователей, и обычно рекомендуется попробовать другие учетные записи пользователей или SSH с помощью пользователя SSH по умолчанию (локальная учетная запись), а затем повторить попытку kinit.

## <a name="ssh-debug-log"></a>Журнал отладки SSH

Чтобы включить подробное ведение журнала, необходимо перезапустить `sshd` с `-d` параметром. Как и в `/usr/sbin/sshd -d` случае с `sshd` пользовательским портом (например, 2222), не нужно останавливаться на главном управляющей программе SSH. Можно также использовать `-v` параметр с клиентом SSH для получения дополнительных журналов (представление ошибок на стороне клиента).

## <a name="next-steps"></a>Дальнейшие действия

* [Управление кластерами HDInsight с помощью корпоративного пакета безопасности](./apache-domain-joined-manage.md)
* [Проверка подлинности при использовании присоединенного к домену кластера HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
