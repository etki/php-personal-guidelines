# Тесты и правила написания кода

## Модульное тестирование (unit testing)

Каждый написанный компонент должен в обязательном порядке изолированно
тестироваться. Это подразумевает следующее:

* На все методы класса, реализующие логику, пишутся модульные тесты.
* Конструктор класса не должен заниматься чем-либо кроме присвоения
переменных свойствам; в идеале этим должны заниматься сеттеры - это
позволяет использовать моки для тестирования, что значительно облегчает
весь процесс поддержки кода.
* На каждый компонент, обеспечивающий связь с внешним сервисом на нижнем
уровне, обязательно пишется интерфейс, который позволяет написать мок на
этот компонент без использования мок-фреймворков и протестировать
приложение на уровне системных и интеграционных тестов без реальных
обращений к внешним сервисам.
* Код пишется без статичных методов даже для тех классов, где состояние
класса не имеет никакого смысла (например, форматтер). Несмотря на то,
что это является правильной практикой, в PHP крайне сложно подменить
статичный метод, что значительно усложняет тестирование.
* По возможности проект пишется на паттернах dependecy injection и
service locator, которые облегчают доставку моков компонентов в
тестируемый класс.
* Тестирование компонентов, работающих с файловой системой,
осуществляется **только** с помощью виртуальной файловой системы (пакеты
[php-vfs/php-vfs][php-vfs], [adlawson/vfs][adlawson-vfs],
[mikey179/vfsStream][mikey179-vfsStream])

Модульное тестирование может осуществляться с помощью любого
современного фреймворка - Codeception, Atoum, Behat.

## Системное тестирование (system / acceptance testing)

Системное тестирование также зачастую называют приемочным (acceptance),
в частности, так оно называется в терминологии фреймворка Codeception).

Для системного тестирования рекомендуется использования Codeception в
связке с Phantom (через Selenium) и встроенного сервера PHP на основе
пользовательских сценариев.

## Интеграционное тестирование

Интеграционное тестирование подразумевает под собой проверку интеграции
приложения с другими сервисами. В этом случае тестирование делится на
две части:

* Каждому сервису посылается набор идемпотентных запросов, в ответе на
которые проверяется верность схемы данных (но не сами данные!) - это
позволяет быть уверенным, что сервис работает также, как и при
разработке, и клиент внезапно не отвалится.
* Каждый клиент прогоняется по всему функционалу с использованием мока
нижнего уровня клиента, который всегда возвращает требуемый вывод.
* При наличии достаточного времени создается мок всего клиента, который
используется для тестирования приложения в целом.

## Функциональное тестирование

Функциональное тестирование в сфере PHP обозначает простое прямое
тестирование контроллеров на выдачу корректного объекта response в ответ
на предварительно заданный объект request. Фактически это является
частью или полноценной заменой системного тестирования, и может
применяться для ускорения системных тестов в тех местах, где нет
необходимости тестировать фронтенд, однако рекомендуется использовать
полноценное системное тестирование.

## Smoke-тесты

При выкладывании приложения в тестинг и продакшен необходимо
удостовериться в том, что оно корректно работает. Для этого некоторым
тестам задается группа smoke, которая в обязательном порядке прогоняется
на тестинге и продакшене - это тот минимальный набор тестов, который
проверяет приложение целиком и все его потенциально опасные места -
авторизация, подключение к БД, запись файлов и т.п.

  [mikey179-vfsStream]: https://packagist.org/packages/mikey179/vfsStream
  [adlawson-vfs]: https://packagist.org/packages/adlawson/vfs
  [php-vfs]: https://packagist.org/packages/php-vfs/php-vfs