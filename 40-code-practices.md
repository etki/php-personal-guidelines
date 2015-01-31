# Практики написания кода

Эта секция посвящена тому, как надо и как не надо писать код в
непосредственно функциональном плане.

## Общие практики

### Статичные переменные и методы, а также синглтоны

Всех трех вещей стоит избегать, потому что статичные вызовы и синглтоны
увеличивают связность кода (за счет того, что вызовы производятся к
фиксированному классу или экземпляру), что мешает при рефакторинге и
тестировании. С точки зрения архитектуры, конечно, все вызовы, которых
не интересует текущее состояние объекта, должны быть объявлены
статичными, но из-за увеличения связности кода их стоит избегать.

### Множественные точки выхода

Разрешены и приветствуются в случае использования для уменьшения
вложенности.

### Принимаемые и возвращаемые типы данных

Метод всегда должен принимать и возвращать одинаковые типы данных. Под
этим подразумевается, что метод не может принимать в одном аргументе
строку или булев: этим аргументом метод должен принимать либо только
строку, либо только булев. Аналогично и с возвращаемыми значениями:
метод не может возвращать объект или булев, он должен либо всегда
возвращать объект, либо всегда возвращать булев. Исключениеми являются
`null`, который может быть использован для обозначения "ничего" (запись
не найдена, аргумент не передан) и разработческий сахар, позволяющий
вместо передачи массива из одного элемента передавать просто элемент,
который автоматом будет сконвертирован в массив из одного элемента.
Например, вызов

    dummyFunc(array('test',))
    
Может быть равноценен вызову

    dummyFunc('test')
    
Подобный подход, однако, все равно не приветствуется.

### Feature flags

Feature flags - это набор флагов (булевых значений), которые включают
или выключают отдельные блоки функционала приложения. Введение feature
flags похволяет облегчить итерационную разработку с внедрением нового
функционала: в случае, если мастер-ветка содержит полностью
реализованную на бэкенде фичу, к которой не дописана фронтенд-обработка
(т.е. бэкенд выводит готовый HTML, к которому пока нет JS-обработки), то
эта фича может быть отключена с помощью соответствующего feature-флага,
что позволит задеплоить приложение даже в промежуточном варианте.


### Development traits


Dev-traits - это набор флагов, который позволяет управлять поведением
приложения, пока оно находится не в prod-окружении. В отличие от feature
flags, dev traits не включают и выключают целые блоки функционала, а
меняют поведение приложения: включают пре-заполнение форм для ручного
тестирования, включают профилирование, выводят граф прав при каждом
запросе к защищенному ресурсу, подменяют отсутствующие переводы на
подсвеченные токены и т.п. Из-за своей специфики сама поддержка dev
traits должна осуществляться на верхних уровнях приложения, в то время
как низкоуровневые компоненты должны просто поддерживать альтернативное
поведение.

### Использование окружений (dev, test, prod)

Любое приложение должно поддерживать dev, test и prod-окружения.
Test-окружение подразумевает полную подмену всех сервисов, которые
невозможно полноценно развернуть на локальной машине, на моки,
имитирующие работу (или отсутствие работоспособности) этих сервисов.
Dev-окружение также реализует аналогичную подмену сервисов, которая
может быть отключена с помощью соответствующего dev-trait (о
dev-trait'ах немного ниже), кроме того, в этом окружении dev-trait'ы
могут влиять на поведение приложения. Prod-окружение подразумевает
использование реальных сервисов без каких-либо моков, отключенный вывод
расширенной информации об ошибках и либо логирование уровней `ERROR` и
выше, либо полноценное логирование только в случае возникновения ошибки
(`FingersCrossedHandler` в `monolog/monolog`).

Любая библиотека должна предполагать использование в этих трех режимах;
в случае, если библиотека представляет собой API-клиент - предоставлять
простую возможность внедрения произвольных ответов сервиса.