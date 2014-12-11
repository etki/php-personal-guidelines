Приложение обязательно должно обязательно выполнять следующую цепочку
команд:

* `validate`
* `clean`
* `install`
* `build`

В отличие от проектов, построенных с помощью сборщиков, для выполнения
этих команд требуется предварительно выполнить
`composer install --no-dev`. Команды последовательно зависят друг от
друга, т.е. команда `validate` выполнится в одиночку, а `install`
предварительно вызовет `validate` и `clean`, при этом подключение к базе
данных проверяться не будет.

## validate

Проверяет приложение на наличие ошибок: присутствие необходимых
ресурсов, наличие ключей конфигурации, подключение к БД.

## clean

Очищает директорию приложения от сгенерированных файлов, будь то
сгенерированный код, временные или `.lock`-файлы.

## install

Устанавливает приложение, скачивая и копирую необходимые ресурсы,
применяя миграции и генерируя необходимый код.

## build

Упаковывает установленное приложение в zip-архив, удаляя машинозависимую
конфигурацию (подключение к БД, данные для подключения к API).