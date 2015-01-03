# Исключения

Любой проект должен реализовывать систему исключений, игнорируя
функционал стандартных ошибок PHP. При этом обязательно вводится одно-
или многоуровневая система, позволяющая обработчикам отлавливать все
исключения одного пакета или уровня; в такой системе наследуются все
базовые исключения PHP с включением пустого интерфейса, по которому и
возможен отлов отдельного типа исключений. Пример:

Vendor/Lib/Exception/VendorLibExceptionInterface.php
```php
namespace Vendor/Lib/Exception;

interface VendorLibExceptionInterface
{
}
```

Vendor/Lib/Exception/LogicException.php
```php
namespace Vendor/Lib/Exception;

use LogicException as SplLogicException;

class LogicException extends SplLogicException implements
    VendorLibExceptionInterface
{
}
```

Vendor/Lib/Exception/TrueIsNotFalseException.php
```php
namespace Vendor/Lib/Exception;

class TrueIsNotFalseException extends LogicException
{
}
```

После этого `TrueIsNotFalseException` можно будет отловить по интерфейсу
`VendorLibExceptionInterface`. В случае реализации многоуровневой
системы исключений очередной уровень должен также реализовывать свой
интерфейс и повторно наследовать базовые исключения уже от предыдущего
уровня.

Сами исключения пишутся на каждую конкретную ошибку, допускается
написание агрегирующих исключений, которые не используются сами по себе,
но от которых наследуются используемые исключения.
