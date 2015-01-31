# Поведение приложения

## Зависимость от названия окружения

Приложение должно работать только в тех окружениях, о которых ему известно. В
случае обнаружения неизвестного окружения приложение должно немедленно завершить
выполнение с соответствующей ошибкой.

Разрешается автоматически конвертировать известные синонимы окружений (например,
develop => dev). Подобные синонимы задаются на уровне кода и могут быть
абсолютно произвольными.

## Исключения

Приложение обязано уметь рапортовать о непойманных исключениях на произвольный
email, hipchat или аналог, записывать информацию в отдельный лог, а также
отправлять payload с информацией об исключении на произвольный URL. При этом
приложение не имеет права допустить еще одно непойманное исключение во время
информирования сервисов.