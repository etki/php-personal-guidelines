# Профилирование и тесты на производительность

Профилирование конкретных участков кода приложения выполняется с помощью
используемого фреймворка; профилирование отдельных компонентов
выполняется с помощью пакета [athletic/athletic][]. В случае, если
требуется выполнить профилирование приложения целиком, используется
расширение xdebug для создания cachegrind-вывода.

   [athletic/athletic]: https://packagist.org/packages/athletic/athletic