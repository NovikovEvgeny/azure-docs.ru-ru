---
title: 'Two-Class логистической регрессии: Справочник по модулям'
titleSuffix: Azure Machine Learning
description: Узнайте, как использовать модуль логистической регрессии Two-Class в Машинное обучение Azure для создания двоичного классификатора.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/22/2020
ms.openlocfilehash: 2e29a666f4d478e11986f834cff94d9743223f22
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2020
ms.locfileid: "93420586"
---
# <a name="two-class-logistic-regression-module"></a>Модуль логистической регрессии Two-Class

В этой статье описывается модуль в конструкторе Машинное обучение Azure.

Этот модуль используется для создания модели логистической регрессии, которая может использоваться для прогнозирования двух (и только двух) результатов. 

Логистическая регрессия — это хорошо известная статистическая методика, которая используется для моделирования многих типов проблем. Этот алгоритм является защищенным методом *обучения* .  Поэтому необходимо предоставить набор данных, который уже содержит результаты для обучения модели.  

### <a name="about-logistic-regression"></a>О логистической регрессии  

Логистическая регрессия — это хорошо известный метод в статистике, который используется для прогнозирования вероятности результата и особенно популярн для задач классификации. Алгоритм прогнозирует вероятность возникновения события путем подгонки данных к логистической функции.
  
В этом модуле алгоритм классификации оптимизирован для дихотомической или двоичных переменных. Если необходимо классифицировать несколько результатов, используйте модуль [логистической регрессии на нескольких классах](./multiclass-logistic-regression.md) .

##  <a name="how-to-configure"></a>Порядок настройки  

Чтобы обучить эту модель, необходимо предоставить набор данных, содержащий метку или столбец класса. Поскольку этот модуль предназначен для проблем с двумя классами, столбец метки или класса должен содержать ровно два значения. 

Например, столбец меток может иметь значение [проголосовавших] с возможными значениями "Yes" или "No". Или это может быть [кредитный риск] с возможными значениями "High" или "Low". 
  
1.  Добавьте модуль **логистической регрессии из двух классов** в конвейер.  
  
2.  Укажите, как должна быть обучена модель, установив параметр " **создать режим инструктора** ".  
  
    -   **Один параметр**. Если вы умеете настраивать модель, вы можете указать конкретный набор значений в качестве аргументов.  

    -   **Диапазон параметров**. Если вы не знаете наилучших параметров, оптимальные параметры можно найти с помощью модуля [Настройка модели параметры](tune-model-hyperparameters.md) . Вы предоставляете некоторый диапазон значений, и преподаватель выполняет итерацию по нескольким сочетаниям параметров, чтобы определить сочетание значений, которое дает наилучший результат.
  
3.  Для параметра **допуск оптимизации** укажите пороговое значение, которое будет использоваться при оптимизации модели. Если увеличение числа итераций выходит за указанное пороговое значение, то алгоритм считается согласованным для решения, и обучение останавливается.  
  
4.  Для весовых коэффициентов и **толщины в 2 уровня** **"Стандартный"** введите значение, которое будет использоваться для параметров деплотности L1 и L2. Для обоих параметров рекомендуется использовать ненулевое значение.  
     *Регулярная* поддержка — это метод предотвращения перегонки моделей пенализинг с использованием значений коэффициента. Процедура обдействуется путем добавления штрафа, связанного со значениями коэффициента, к ошибке гипотезы. Таким образом, точная модель с экстремальными значениями будет более штрафной, но менее точная модель с более консервативными значениями будет меньше.  
  
     Регуляризации L1 и L2 отличаются результатами и способом применения.  
  
    -   L1 можно применять к разреженным моделям, что удобно при работе с многомерными данными.  
  
    -   L2, напротив, предпочтительнее использовать для неразреженных данных.  
  
     Этот алгоритм поддерживает линейное сочетание значений по стандарту L1 и L2. Это означает, что, если <code>x = L1</code> и <code>y = L2</code> , <code>ax + by = c</code> определяет линейный Диапазон терминов в области действия.  
  
    > [!NOTE]
    >  Хотите узнать больше о стандартах L1 и L2? Следующая статья содержит обсуждение различий между уровнями L1 и L2 и их влиянием на подгонку моделей. примеры кода для моделей логистической регрессии и нейронной сети:  [для машинное обучение](/archive/msdn-magazine/2015/february/test-run-l1-and-l2-regularization-for-machine-learning)  
    >
    > Для моделей логистической регрессии были применены различные линейные сочетания значений L1 и L2: например, [эластичную сеть](https://wikipedia.org/wiki/Elastic_net_regularization). Мы рекомендуем сослаться на эти сочетания, чтобы определить линейное сочетание, которое эффективно в модели.
      
5.  Для параметра **l-бфгс (размер памяти** ) укажите объем памяти, который будет использоваться для оптимизации *l-бфгс* .  
  
     L-БФГС означает "ограниченный объем памяти Бройден-Флетчера-Гольдфарб-Шанно". Это алгоритм оптимизации, который широко применяется для оценки параметров. Этот параметр указывает число сохраняемых последних позиций и градиентов для вычисления следующего шага.  
  
     Этот параметр оптимизации ограничивает объем памяти, используемый для вычисления следующего шага и направления. Если указано меньшее количество памяти, обучение проходит быстрее, но является менее точным.  
  
6.  В качестве **начального числа случайных чисел** введите целое значение. Определение начального значения важно, если требуется воспроизвести результаты по нескольким запускам одного конвейера.  
  
  
8. Добавьте в конвейер набор данных с меткой и обучить модель:

    + Если присвоить **параметру** **создать режим инструктора** значение Single, подключить набор данных с тегами и модуль [обучение модели](train-model.md) .  
  
    + Если задать **режим создания инструктора** в **диапазоне параметров** , подключите набор данных с тегами и обучите модель с помощью [параметров настройки модели](tune-model-hyperparameters.md).  
  
    > [!NOTE]
    > 
    > При передаче диапазона параметров для [обучения модели](train-model.md)используется только значение по умолчанию в списке с одним параметром.  
    > 
    > Если передать один набор значений параметров в модуль [Настройка модели настройки](tune-model-hyperparameters.md) , когда он ожидает диапазон параметров для каждого параметра, он пропускает значения и использует значения по умолчанию для учений.  
    > 
    > Если выбрать параметр **диапазон параметров** и ввести одно значение для любого параметра, это единственное заданное значение будет использоваться во время очистки, даже если другие параметры меняются в диапазоне значений.  
  
9. Отправьте конвейер.  
  
## <a name="results"></a>Результаты

После завершения обучения:
 
  
+ Чтобы сделать прогнозы для новых данных, используйте обученную модель и новые данные в качестве входных данных для модуля [Оценка модели](./score-model.md) . 


## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с [набором доступных модулей](module-reference.md) в службе Машинного обучения Azure.