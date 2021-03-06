---
title: Устранение неполадок развертывания веб-службы
titleSuffix: Azure Machine Learning
description: Узнайте, как обойти и устранить распространенные ошибки развертывания DOCKER с помощью службы Kubernetes Azure и экземпляров контейнеров Azure.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: gvashishtha
ms.author: gopalv
ms.reviewer: jmartens
ms.date: 11/02/2020
ms.topic: troubleshooting
ms.custom: contperfq4, devx-track-python, deploy
ms.openlocfilehash: dfbfea22738e6aeb0df31ad941b2ff10e53795a4
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2020
ms.locfileid: "93311294"
---
# <a name="troubleshoot-model-deployment"></a>Устранение неполадок при развертывании моделей

Узнайте, как устранить и устранить распространенные ошибки развертывания DOCKER в службе "экземпляры контейнеров Azure" (ACI) и службу Kubernetes Azure (AKS) с помощью Машинное обучение Azure.

## <a name="prerequisites"></a>Предварительные требования

* **Подписка Azure**. Попробуйте [бесплатную или платную версию Машинного обучения Azure](https://aka.ms/AMLFree).
* [Пакет SDK для Машинного обучения Azure](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py).
* [Интерфейс командной строки Azure](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest).
* [Расширение CLI для Машинного обучения Azure](reference-azure-machine-learning-cli.md).
* Для локальной отладки вам необходима рабочая установка Docker в локальной системе.

    Чтобы проверить установку Docker, выполните команду `docker run hello-world` в терминале или командной строке. Сведения об установке Docker или устранении ошибок Docker см. в [документации Docker](https://docs.docker.com/).

## <a name="steps-for-docker-deployment-of-machine-learning-models"></a>Шаги для развертывания DOCKER в моделях машинного обучения

При развертывании модели для нелокальных вычислений в Машинное обучение Azure происходят следующие действия.

1. Dockerfile, указанный в объекте среды в Инференцеконфиг, отправляется в облако вместе с содержимым исходного каталога.
1. Если ранее созданный образ в реестре контейнеров недоступен, новый образ DOCKER создается в облаке и сохраняется в реестре контейнеров по умолчанию для рабочей области.
1. Образ DOCKER из реестра контейнеров загружается в целевой объект вычислений.
1. Хранилище больших двоичных объектов по умолчанию подключено к целевому объекту вычислений, предоставляя доступ к зарегистрированным моделям.
1. Веб-сервер инициализируется путем запуска функции сценария записи. `init()`
1. Когда развернутая модель получает запрос, `run()` функция обрабатывает этот запрос.

Основное различие при использовании локального развертывания состоит в том, что образ контейнера строится на локальном компьютере, поэтому для локального развертывания необходимо установить DOCKER.

Понимание этих общих шагов должно помочь понять, где возникают ошибки.

## <a name="get-deployment-logs"></a>Получение журналов развертывания

Первым шагом при отладке ошибок является получение журналов развертывания. Сначала следуйте [инструкциям](how-to-deploy-and-where.md#connect-to-your-workspace) по подключению к рабочей области.

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

Чтобы получить журналы из развернутой службы WebService, сделайте следующее:

```bash
az ml service get-logs --verbose --workspace-name <my workspace name> --name <service name>
```

# <a name="python"></a>[Python](#tab/python)


При условии, что имеется объект типа с `azureml.core.Workspace` именем `ws` , можно сделать следующее:

```python
print(ws.webservices)

# Choose the webservice you are interested in

from azureml.core import Webservice

service = Webservice(ws, '<insert name of webservice>')
print(service.get_logs())
```

---

## <a name="debug-locally"></a>Отладка в локальной среде

Если при развертывании модели в ACI или AKS возникли проблемы, разверните ее как локальную веб-службу. Использование локальной веб-службы упрощает устранение неполадок.

Пример [локального развертывания](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/deploy-to-local/register-model-deploy-local.ipynb) можно найти в репозитории  [мачинелеарнингнотебукс](https://github.com/Azure/MachineLearningNotebooks) , чтобы просмотреть пример готового к запуску примера.

> [!WARNING]
> Развертывания локальных веб-служб не поддерживаются для рабочих сред.

Для развертывания в локальной среде измените код, чтобы использовать `LocalWebservice.deploy_configuration()` для создания конфигурации развертывания. Затем используйте `Model.deploy()` для развертывания службы. В следующем примере модель (содержащаяся в переменной моделей) развертывается как локальная веб-служба:

```python
from azureml.core.environment import Environment
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import LocalWebservice


# Create inference configuration based on the environment definition and the entry script
myenv = Environment.from_conda_specification(name="env", file_path="myenv.yml")
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)
# Create a local deployment, using port 8890 for the web service endpoint
deployment_config = LocalWebservice.deploy_configuration(port=8890)
# Deploy the service
service = Model.deploy(
    ws, "mymodel", [model], inference_config, deployment_config)
# Wait for the deployment to complete
service.wait_for_deployment(True)
# Display the port that the web service is available on
print(service.port)
```

Если вы определяете собственную спецификацию conda YAML, то List azureml-Defaults версии >= 1.0.45 в качестве зависимости PIP. Этот пакет необходим для размещения модели в качестве веб-службы.

На этом этапе вы можете использовать службу обычным образом. В следующем примере кода демонстрируется отправка данных в службу:

```python
import json

test_sample = json.dumps({'data': [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
]})

test_sample = bytes(test_sample, encoding='utf8')

prediction = service.run(input_data=test_sample)
print(prediction)
```

Дополнительные сведения о настройке среды Python см. на [этой странице](how-to-use-environments.md). 

### <a name="update-the-service"></a>Обновление службы

Во время локального тестирования, возможно, нужно будет обновить файл `score.py`, чтобы добавить ведение журнала или попытаться решить любые обнаруженные проблемы. Чтобы перезагрузить изменения в файл `score.py`, используйте `reload()`. Например, следующий код перезагружает сценарий для службы, а затем отправляет в нее данные. Данные оцениваются с помощью обновленного файла `score.py`:

> [!IMPORTANT]
> Метод `reload` доступен лишь для локальных развертываний. Сведения об обновлении развертывания на другом целевом объекте вычислений см. [в разделе Обновление веб-службы](how-to-deploy-update-web-service.md).

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> Сценарий перезагружается из расположения, указанного объектом `InferenceConfig`, используемым службой.

Чтобы изменить модель, зависимости Conda или конфигурацию развертывания, используйте метод [update()](/python/api/azureml-core/azureml.core.webservice%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=trueupdate--args-). В следующем примере обновляется модель, используемая службой:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Удаление службы

Чтобы удалить службу, воспользуйтесь методом [delete()](/python/api/azureml-core/azureml.core.webservice%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truedelete--).

### <a name="inspect-the-docker-log"></a><a id="dockerlog"></a> Проверка журнала Docker

Можно распечатать подробные сообщения журнала ядра Docker из объекта службы. Вы можете просмотреть журнал для ACI, AKS и локальных развертываний. В следующем примере показано, как вывести данные журналов.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```
Если строка `Booting worker with pid: <pid>` в журналах встречается несколько раз, это означает, что недостаточно памяти для запуска рабочей роли.
Эту ошибку можно устранить, увеличив значение `memory_gb` в `deployment_config`
 
## <a name="container-cannot-be-scheduled"></a>Контейнер невозможно запланировать

При развертывании службы в целевой объект вычислений Службы Azure Kubernetes Машинное обучение Azure попытается запланировать службу с запрошенным количеством ресурсов. Если в кластере нет доступных узлов с соответствующим объемом ресурсов через 5 минут, развертывание завершится сбоем. Сообщение об ошибке: `Couldn't Schedule because the kubernetes cluster didn't have available resources after trying for 00:05:00` . Эту ошибку можно решить, добавив дополнительные узлы, изменив номера SKU узлов или изменив требования к ресурсам службы. 

Сообщение об ошибке обычно указывает, какой ресурс требуется больше, например, если отображается сообщение об ошибке, указывающее на то, `0/3 nodes are available: 3 Insufficient nvidia.com/gpu` что для службы требуются GPU, а в кластере есть три узла, на которых нет доступных графических процессоров. Эту проблему можно решить, добавив больше узлов при использовании SKU-номера GPU. В противном случае переключитесь на SKU с поддержкой GPU или измените среду, убрав необходимость использования GPU.  

## <a name="service-launch-fails"></a>Сбой запуска службы

После успешной сборки образа система пытается запустить контейнер с использованием конфигурации развертывания. В ходе процесса запуска контейнера системой вызывается функция `init()` в скрипте оценки. При наличии неперехваченных исключений в функции `init()` в сообщении об ошибке может появиться ошибка **CrashLoopBackOff**.

Используя сведения из [этого раздела](#dockerlog), проверьте журналы.

## <a name="function-fails-get_model_path"></a>Ошибка выполнения функции: get_model_path()

Часто в рамках функции `init()` в скрипте оценки вызывается функция [Model.get_model_path()](/python/api/azureml-core/azureml.core.model.model?preserve-view=true&view=azure-ml-py#&preserve-view=trueget-model-path-model-name--version-none---workspace-none-), чтобы найти файл модели или папку с файлами модели в контейнере. Если файл или папку модели найти не удается, происходит сбой функции. Самый простой способ устранить эту ошибку — это выполнить приведенный ниже код Python в оболочке контейнера.

```python
from azureml.core.model import Model
import logging
logging.basicConfig(level=logging.DEBUG)
print(Model.get_model_path(model_name='my-best-model'))
```

Этот пример выводит локальный путь (относительно `/var/azureml-app`) в контейнер, где скрипт оценки ожидает найти файл модели или папку с файлами. Затем можно проверить, действительно ли файл или папка находится там, где нужно.

При установке уровня ведения журнала DEBUG (для отладки) в нем может регистрироваться дополнительная информация, которая может быть полезна для определения причины сбоя.

## <a name="function-fails-runinput_data"></a>Ошибка выполнения функции: run(input_data)

Если служба успешно развернута, но аварийно завершает работу при публикации данных в конечную точку оценки, можно добавить оператор для перехвата ошибок в функцию `run(input_data)`, чтобы она возвращала подробное сообщение об ошибке. Пример:

```python
def run(input_data):
    try:
        data = json.loads(input_data)['data']
        data = np.array(data)
        result = model.predict(data)
        return json.dumps({"result": result.tolist()})
    except Exception as e:
        result = str(e)
        # return error message back to the client
        return json.dumps({"error": result})
```

**Примечание.** Возврат сообщений об ошибках из вызова `run(input_data)` следует выполнять только в целях отладки. Из соображений безопасности сообщения об ошибках не нужно возвращать таким способом в рабочей среде.

## <a name="http-status-code-502"></a>Код состояния HTTP 502

Код состояния 502 указывает, что в службе создано исключение или в методе `run()` файла score.py произошла ошибка. Используйте сведения из этой статьи для отладки файла.

## <a name="http-status-code-503"></a>Код состояния HTTP 503

Развертывания Службы контейнеров Azure поддерживают автомасштабирование, что позволяет добавлять реплики для поддержки дополнительной загрузки. Автомасштабирование предназначено для управления **постепенной** сменой нагрузки. В случае высоких скачков в запросах в секунду клиенты могут получить код состояния HTTP 503. Несмотря на то, что Автомасштабирование реагирует быстро, для создания дополнительных контейнеров требуется AKS значительное количество времени.

Решения для увеличения или уменьшения масштаба основываются на использовании текущих реплик контейнеров. Количество занятых реплик (обработка запроса), деленное на общее число текущих реплик, является текущим использованием. Если это число превышает `autoscale_target_utilization` , то создаются дополнительные реплики. Если он меньше, то реплики будут сокращены. Решения по добавлению реплик выполняются быстро и быстрее (около 1 секунды). Решения об удалении реплик являются консервативными (около 1 минуты). По умолчанию для автомасштабирования используется значение **70%**. Это означает, что служба может обслуживать пиковые значения в запросах в секунду (RPS) **до 30%**.

Предотвратить появление кода состояния 503 можно двумя способами:

> [!TIP]
> Эти два подхода можно использовать по отдельности или в сочетании.

* Измените уровень использования, при котором автоматическое масштабирование создает новые реплики. Вы можете настроить целевое использование, задав для `autoscale_target_utilization` более низкое значение.

    > [!IMPORTANT]
    > Это изменение не *ускорит* создание реплик. Просто они будут создаваться с более низким порогом использования. Вместо того, чтобы ждать, пока служба использует реплики на 70 %, измените значения на 30 %. Таким образом реплики будут создаваться при использовании 30 %.
    
    Если веб-служба уже использует все текущие реплики, а код состояния 503 не исчезает, увеличьте значение `autoscale_max_replicas`, чтобы повысить максимальное количество реплик.

* Измените минимальное количество реплик. Увеличение минимального количества реплик обеспечивает больший пул для обработки входящих пиков.

    Чтобы увеличить минимальное количество реплик, задайте для `autoscale_min_replicas` более высокое значение. Вы можете рассчитать необходимое количество реплик с помощью следующего кода, указав необходимые для вашего проекта значения:

    ```python
    from math import ceil
    # target requests per second
    targetRps = 20
    # time to process the request (in seconds)
    reqTime = 10
    # Maximum requests per container
    maxReqPerContainer = 1
    # target_utilization. 70% in this example
    targetUtilization = .7

    concurrentRequests = targetRps * reqTime / targetUtilization

    # Number of container replicas
    replicas = ceil(concurrentRequests / maxReqPerContainer)
    ```

    > [!NOTE]
    > Если пики запроса будут превышать новое минимальное количество реплик, снова отобразится код 503. Например, по мере увеличения трафика, поступающего в вашу службу, может потребоваться увеличение минимального количества реплик.

Дополнительные сведения о настройке `autoscale_target_utilization`, `autoscale_max_replicas` и `autoscale_min_replicas` см. на [этой странице](/python/api/azureml-core/azureml.core.webservice.akswebservice?preserve-view=true&view=azure-ml-py).

## <a name="http-status-code-504"></a>Код состояния HTTP 504

Код состояния 504 указывает на истечение времени ожидания запроса. Время ожидания по умолчанию составляет 1 минуту.

Вы можете увеличить время ожидания или попытаться ускорить службу, изменив файл score.py для удаления ненужных вызовов. Если проблема не исчезнет, используйте сведения из этой статьи для отладки файла score.py. Код может находиться в неотвечающем состоянии или в бесконечном цикле.

## <a name="advanced-debugging"></a>Расширенная отладка

Возможно, потребуется интерактивная отладка кода Python, содержащегося в развертывании модели. Например, если начальный сценарий не работает и причину невозможно определить с помощью дополнительного ведения журнала. С помощью Visual Studio Code и дебугпи можно присоединяться к коду, выполняющемуся в контейнере DOCKER.

Дополнительные сведения см. в [разделе Интерактивная Отладка в VS Code Guide](how-to-debug-visual-studio-code.md#debug-and-troubleshoot-deployments).

## <a name="model-deployment-user-forum"></a>[Форум пользователя по развертыванию модели](/answers/topics/azure-machine-learning-inference.html)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о развертывании см. в статьях, представленных ниже.

* [Развертывание моделей с помощью Службы машинного обучения Azure](how-to-deploy-and-where.md)
* [Руководство. Обучение и развертывание моделей](tutorial-train-models-with-aml.md)