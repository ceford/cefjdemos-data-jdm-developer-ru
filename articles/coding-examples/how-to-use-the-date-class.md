<!-- Filename: How_to_use_JDate / Display title: Как использовать класс Date -->

## Введение
Класс Date Joomla является вспомогательным классом, расширенным от класса DateTime PHP, который позволяет разработчикам более эффективно обрабатывать форматирование дат. Класс позволяет разработчикам форматировать даты для читаемых строк, взаимодействия с MySQL, расчета UNIX timestamp, а также предоставляет вспомогательные методы для работы в разных часовых поясах.

## Создание экземпляра даты

Все методы-помощники для работы с датой требуют экземпляра класса Date. Чтобы начать, вам необходимо создать его. Объект Date можно создать двумя способами. Один из них — это типичный нативный способ просто создать новый экземпляр:

```php
use Joomla\CMS\Date\Date;

$date = new Date(); // Creates a new Date object equal to the current time.
```

Вы также можете создать экземпляр, используя статический метод, определенный в Date:

```php
use Joomla\CMS\Date\Date;

$date = Date::getInstance(); // Alias of 'new Date();'
```

Нет никакой разницы между этими методами, так как Date::getInstance просто создает новый экземпляр Date точно так же, как первый показанный метод.

В качестве альтернативы можно также получить текущую дату (в виде объекта Date) из объекта Application, используя:
```php
use Joomla\CMS\Factory;

$date = Factory::getDate();
```

## Аргументы

Конструктор Date (и статический метод getInstance) принимает два необязательных параметра: строку с датой для форматирования и часовой пояс. Если не передать строку с датой, будет создан объект Date с текущими датой и временем, а если не передать часовой пояс, объект Date будет использовать часовой пояс, установленный по умолчанию.

Первым аргументом, если он используется, должна быть строка, которую можно разобрать с помощью встроенного конструктора DateTime в PHP. Например:
```php
use Joomla\CMS\Date\Date;

$currentTime = new Date('now'); // Current date and time
$tomorrowTime = new Date('now +1 day'); // Current date and time, + 1 day.
$plus1MonthTime = new Date('now +1 month'); // Current date and time, + 1 month.
$plus1YearTime = new Date('now +1 year'); // Current date and time, + 1 year.
$plus1YearAnd1MonthTime = new Date('now +1 year +1 month'); // Current date and time, + 1 year and 1 month.
$plusTimeToTime = new Date('now +1 hour +30 minutes +3 seconds'); // Current date and time, + 1 hour, 30 minutes and 3 seconds
$plusTimeToTime = new Date('now -1 hour +30 minutes +3 seconds'); // Current date and time, + 1 hour, 30 minutes and 3 seconds
$combinedTimeToTime = new Date('now -1 hour -30 minutes 23 seconds'); // Current date and time, - 1 hour, +30 minutes and +23 seconds

$date = new Date('2012-12-1 15:20:00'); // 3:20 PM, December 1st, 2012
```

К Unix-метке времени (в секундах) также можно передать в качестве первого аргумента, в этом случае она будет внутренне преобразована в дату. Если в качестве второго аргумента конструктора указана временная зона, она будет преобразована в эту временную зону.

## Вывод дат

Одна предосторожность при выводе объектов Date в пользовательском контексте: не выводите их просто на экран. Метод toString() объекта Date просто вызывает метод format() его родителя, без учета временных зон или локализированного формата дат. Это не приведет к хорошему пользовательскому опыту и вызовет несоответствия между форматированием в вашем расширении и где-либо еще в Joomla. Вместо этого всегда выводите даты, используя методы, показанные ниже.

### Общие форматы даты

Ряд форматов даты заранее определен в Joomla как часть базовых языковых пакетов. Это полезно, потому что форматы дат могут легко интернационализироваться. Пример доступных строк форматов приведен ниже, из языкового пакета en-GB. Настоятельно рекомендуется использовать эти строки форматирования при выводе дат, чтобы ваши даты автоматически форматировались в соответствии с локалью пользователя. Их можно получить так же, как и любую строку языка (см. примеры ниже).

```ini
DATE_FORMAT_LC="l, d F Y"
DATE_FORMAT_LC1="l, d F Y"
DATE_FORMAT_LC2="l, d F Y H:i"
DATE_FORMAT_LC3="d F Y"
DATE_FORMAT_LC4="Y-m-d"
DATE_FORMAT_LC5="Y-m-d H:i"
DATE_FORMAT_LC6="Y-m-d H:i:s"
DATE_FORMAT_JS1="y-m-d"
DATE_FORMAT_CALENDAR_DATE="%Y-%m-%d"
DATE_FORMAT_CALENDAR_DATETIME="%Y-%m-%d %H:%M:%S"
DATE_FORMAT_FILTER_DATE="Y-m-d"
DATE_FORMAT_FILTER_DATETIME="Y-m-d H:i:s"
```

### Метод HtmlHelper (рекомендуется)

Как и многие общие элементы вывода, класс HtmlHelper здесь чтобы... помочь! Метод date() класса HtmlHelper принимает любую строку даты и времени, которую может принять конструктор Date, вместе со строкой форматирования и выводит дату с учетом настроек часового пояса текущего пользователя. Таким образом, это рекомендованный метод для вывода дат для пользователя.

```php
use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;

$myDateString = '2012-12-1 15:20:00';
echo HtmlHelper::date($myDateString, Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

### Метод format() объекта Date

Другой вариант — форматировать дату вручную. Если используется этот метод, вам также придется вручную получить и установить часовой пояс пользователя. Этот метод более полезен для форматирования дат вне пользовательского интерфейса, например в системных журналах или при вызовах API.

```php
use Joomla\CMS\Language\Text;
use Joomla\CMS\Date\Date;
use Joomla\CMS\Factory;

$myDateString = '2012-12-1 15:20:00';
$timezone = Factory::getUser()->getTimezone();

$date = new Date($myDateString);
$date->setTimezone($timezone);
echo $date->format(Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

## Другие Полезные Примеры Кода

### Быстрый вывод текущего времени

Есть два простых способа сделать это.
- Метод date() класса HtmlHelper, если значение даты не указано, по умолчанию будет использовать текущее время.
- Factory::getDate() получает текущую дату в виде объекта Date, который мы можем затем форматировать.

```php
use Joomla\CMS\Factory;
use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;

// These two are functionally equivalent
echo HtmlHelper::date('now', Text::_('DATE_FORMAT_FILTER_DATETIME'));

$timezone = Factory::getUser()->getTimezone();
echo Factory::getDate()->setTimezone($timezone)->format(Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

### Добавление и вычитание из дат

Поскольку объект Joomla Date расширяет объект PHP DateTime, он предоставляет методы для добавления и вычитания дат. Самый простой из этих методов - метод modify(), который принимает любую строку относительного изменения, которую принял бы метод PHP [strtotime](https://www.php.net/manual/en/function.strtotime.php). Например:

```php
use Joomla\CMS\Date\Date;

$date = new Date('2012-12-1 15:20:00');
$date->modify('+1 year');
echo $date->toSQL(); // 2013-12-01 15:20:00
```

Существуют также отдельные методы add() и sub(), для добавления или вычитания времени из объекта даты соответственно. Они принимают объекты [DateInterval](https://www.php.net/manual/en/class.dateinterval.php) в формате PHP.

```php
use Joomla\CMS\Date\Date;

$interval = new \DateInterval('P1Y1D'); // Interval represents 1 year and 1 day

$date1 = new Date('2012-12-1 15:20:00');
$date1->add($interval);
echo $date1->toSQL(); // 2013-12-02 15:20:00

$date2 = new Date('2012-12-1 15:20:00');
$date2->sub($interval);
echo $date2->toSQL(); // 2011-11-30 15:20:00
```

### Вывод дат в формате ISO 8601

```php
$date = new Date('2012-12-1 15:20:00');
$date->toISO8601(); // 20121201T152000Z
```

### Вывод дат в формате RFC 822

```php
$date = new Date('2012-12-1 15:20:00');
$date->toRFC822(); // Sat, 01 Dec 2012 15:20:00 +0000
```

### Вывод дат в формате даты-времени SQL

```php
$date = new Date('20121201T152000Z');
$date->toSQL(); // 2012-12-01 15:20:00
```

### Вывод дат в виде Unix-меток времени

Метка времени Unix выражается как количество секунд, прошедших с эпохи Unix (первая секунда 1 января 1970 года).

*Переведено openai.com*

