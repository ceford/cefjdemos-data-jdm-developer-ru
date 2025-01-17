<!-- Filename: Joomla_CodeSniffer / Display title: Стандарты Кодирования -->

<div class="alert alert-warning">
Последняя часть этой статьи нуждается в обновлении!
</div>

## Обзор ИИ

Стандарты кодирования важны для разработки программного обеспечения, потому что они:

- **Улучшение качества кода**
  - Стандарты кодирования помогают гарантировать, что код надежен, безопасен и защищен. Они также могут помочь уменьшить проблемы с производительностью и опасения по поводу безопасности, которые могут возникнуть из-за плохих практик кодирования.
- **Сделайте код более понятным и поддерживаемым**
  - Стандарты кодирования помогают сделать код более простым для понимания, чтения и поддержки. Это также может упростить новым разработчикам работу с кодом.
- **Ускорение разработки**
  - Стандарты кодирования могут помочь разработчикам избежать распространенных ошибок, которые могут замедлить процессы кодирования.
- **Улучшение сотрудничества**
  - Стандарты кодирования могут способствовать сотрудничеству среди разработчиков, даже в больших командах.
- **Обеспечение согласованности**
  - Стандарты кодирования помогают обеспечить согласованность кода в разных проектах. Это может упростить совместную работу разработчиков над одними и теми же проектами.
- **Улучшение масштабируемости**
  - Стандарты кодирования могут помочь гарантировать, что код может масштабироваться без утраты управляемости.
- **Предоставление четких критериев для обзора кода**
  - Стандарты кодирования могут помочь обеспечить четкие критерии для обзора кода, что может привести к более эффективной обратной связи.

Стандарты кодирования часто включают правила для отступов, длины строк, размещения фигурных скобок и интервалов.

Joomla использует стандарт кодирования [PSR-12](https://www.php-fig.org/psr/psr-12/). Вы можете включить этот стандарт кодирования в своей IDE и получать подсказки, если вы не следуете стандарту кодирования или используете автоматическую исправление. Мы рекомендуем следовать этому стандарту при разработке собственных расширений, чтобы оставаться совместимыми с основой и гарантировать, что ваш код, возможно, будет работать с будущими версиями.

## PHP Code Sniffer

Пожалуйста, обратитесь к текущему источнику [PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer/) для получения информации об этом утилите и о том, как ее установить. Учтите, что исходный источник заброшен, но он может отображаться в поисковых системах. Есть два скрипта:

- **phpcs** обнаруживает нарушения стандартов кодирования и формирует отчет.
- **phpcbf** пытается исправить нарушения стандартов кодирования и формирует отчет о том, что было сделано.

Вы можете установить отдельные скрипты или использовать composer для их установки. Вы также можете найти их в клоне Joomla, расположенном в libraries/vendor/bin, наряду с некоторыми другими полезными утилитами.

## Тестирование PHP Code Sniffer

Если вы добавите глобальную установку в ваш `$PATH`, вы сможете запускать сканер кода из командной строки в корневой директории проекта. Попробуйте это, чтобы отобразить список стандартов кодирования:

```sh
phpcs -i
The installed coding standards are MySource, PEAR, PSR1, PSR2, PSR12, Squiz and Zend
```

Например, следующая команда использовалась в старой папке проекта, которая использовала прежний стандарт пользовательского кодирования Joomla. Среди прочего, она использовала табуляции, а не пробелы для оформления. Многие подобные строки были опущены ниже для краткости.

```sh
phpcs --standard=PSR12 .

FILE: /Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 132 ERRORS AND 3 WARNINGS AFFECTING 116 LINES
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   1 | WARNING | [ ] A file should declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it should execute logic with side effects, but should not do both. The
     |         |     first symbol is defined on line 22 and the first side effect is on line 10.
   1 | ERROR   | [x] Header blocks must be separated by a single blank line
  22 | ERROR   | [ ] Each class must be in a namespace of at least one level (a top-level vendor name)
  24 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  25 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  26 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  ...
  42 | ERROR   | [x] Expected 1 space after closing parenthesis; found newline
...
  48 | ERROR   | [x] No space found after comma in argument list
  48 | ERROR   | [x] Expected 1 space after closing parenthesis; found newline
...
  67 | ERROR   | [x] Expected 0 spaces before closing parenthesis; 1 found
...
  75 | ERROR   | [x] Space after opening parenthesis of function call prohibited
  75 | ERROR   | [x] Expected 0 spaces before closing parenthesis; 1 found
...
 138 | WARNING | [ ] Line exceeds 120 characters; contains 168 characters
 139 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
 139 | WARNING | [ ] Line exceeds 120 characters; contains 141 characters
...
 156 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
 157 | ERROR   | [x] Expected 1 newline at end of file; 0 found
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
PHPCBF CAN FIX THE 131 MARKED SNIFF VIOLATIONS AUTOMATICALLY
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Time: 123ms; Memory: 8MB
```

Предыдущий пример проекта содержал один php-файл без файлов css или js. phpcs генерирует отчет для каждого php, css и js файла в проекте. Ниже приведены некоторые примеры для файлов css и js:

```sh
FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/css/mediacat.css
----------------------------------------------------------------------------------
FOUND 33 ERRORS AFFECTING 32 LINES
----------------------------------------------------------------------------------
  3 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
...
 51 | ERROR | [x] Whitespace found at end of line
...
 79 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
----------------------------------------------------------------------------------
PHPCBF CAN FIX THE 33 MARKED SNIFF VIOLATIONS AUTOMATICALLY
----------------------------------------------------------------------------------

FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/css/file-icon-classic.min.css
-----------------------------------------------------------------------------------------------
FOUND 0 ERRORS AND 1 WARNING AFFECTING 1 LINE
-----------------------------------------------------------------------------------------------
 1 | WARNING | File appears to be minified and cannot be processed
-----------------------------------------------------------------------------------------------

FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/js/mediacat-site.js
-------------------------------------------------------------------------------------
FOUND 38 ERRORS AFFECTING 30 LINES
-------------------------------------------------------------------------------------
  3 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
  5 | ERROR | [x] Expected 1 space after FUNCTION keyword; 0 found
  6 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
...
 13 | ERROR | [x] Opening brace should be on a new line
...
 29 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
 29 | ERROR | [x] Expected at least 1 space before "+"; 0 found
 29 | ERROR | [x] Expected at least 1 space after "+"; 0 found
...
 36 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
-------------------------------------------------------------------------------------
PHPCBF CAN FIX THE 38 MARKED SNIFF VIOLATIONS AUTOMATICALLY
-------------------------------------------------------------------------------------
```

## Вариации команд

Вы можете получить помощь с командами phpcs:

```sh
phpcs --help
```

### Исключить один или несколько файлов

Используйте список шаблонов файлов, разделенных запятыми, чтобы исключить файлы из проверки стиля кода. Пример

```php
phpcs --standard=PSR12 --ignore='libraries/*' .
```

### Исключить одно или несколько правил

Joomla позволяет использовать более длинные строки, чем стандарт PSR, 560 вместо 120. Для исключения предупреждений о длинных строках можно использовать следующую команду:

```sh
phpcs --standard=PSR12 --ignore='libraries/*' --exclude=Generic.Files.LineLength .
```

Вы можете найти правило, которое нарушается, с помощью этой команды:

```sh
phpcs -s yourfile.php
```

### Исключения Joomla

Для разработки расширения с использованием IDE вы можете решить использовать стандарт PSR12 без каких-либо исключений. Joomla имеет [пользовательский набор правил](https://github.com/joomla/joomla-cms/blob/5.2-dev/ruleset.xml), который допускает многие исключения. Он используется для проверки всей установки Joomla во время системного тестирования.

## Исправление нарушений

Единый файл PHP, в котором `FOUND 132 ERRORS AND 3 WARNINGS AFFECTING 116 LINES`, указанный выше, можно в основном исправить следующим образом:

```sh
phpcbf --standard=PSR12 .

PHPCBF RESULT SUMMARY
-----------------------------------------------------------------------------------
FILE                                                               FIXED  REMAINING
-----------------------------------------------------------------------------------
/Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php     131    4
-----------------------------------------------------------------------------------
A TOTAL OF 131 ERRORS WERE FIXED IN 1 FILE
-----------------------------------------------------------------------------------

Time: 232ms; Memory: 8MB
```

Запуск утилиты phpcs снова показывает, какие проблемы остались:

```sh
phpcs --standard=PSR12 .

FILE: /Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 1 ERROR AND 3 WARNINGS AFFECTING 4 LINES
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   1 | WARNING | A file should declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it should execute logic with side effects, but should not do both. The first
     |         | symbol is defined on line 23 and the first side effect is on line 11.
  23 | ERROR   | Each class must be in a namespace of at least one level (a top-level vendor name)
 132 | WARNING | Line exceeds 120 characters; contains 168 characters
 133 | WARNING | Line exceeds 120 characters; contains 141 characters
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Time: 127ms; Memory: 8MB
```

Заключение: расширение, разработанное для более ранней версии Joomla и использующее другой стандарт кодирования, теперь требует доработки!

## Установка в IDE

Большинство интегрированных сред разработки могут использовать анализатор кода во время работы или при сохранении файла.

### Установка в VSCode

В VSCode (и/или VSCodium) выберите значок Расширения, найдите PHP_CodeSniffer и установите его. Требуется конфигурация:

1. В разделе **Настройки** найдите PHP_CodeSniffer
2. Установите значение в поле **PHP Code Sniffer → Exec:** в соответствии с расположением вашего бинарного файла phpcs.
3. В списке **PHP Code Sniffer: Standard** выберите **PSR12**.

Закрыть, и вы готовы к действию.

Некоторые из приведенных выше проблем можно исправить с помощью директив PSR1.

```php
<?php

/**
 * @package     Whatever
 *
 * @phpcs:disable PSR1.Classes.ClassDeclaration.MissingNamespace
 */

use joomla\CMS\...

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

Другим нужно немного подумать!

После установки в VSCode нарушения стиля кода будут обнаружены и отмечены красной волнистой линией. Наведите курсор на проблему, чтобы увидеть сообщение и возможное решение, как в этом примере:

```txt
Header blocks must be separated by a single blank line
PHP_CodeSniffer(PSR12.Files.FileHeader.SpacingAfterBlock)
```

<div class="alert alert-warning">
Следующие разделы необходимо обновить!
</div>

### Установка в PhpStorm

Code Sniffer поддерживается в PhpStorm по умолчанию. Перейдите в Настройки и в разделе **Редактор → Проверки** вы увидите список установленных у вас анализаторов.

#### Установить путь к Code Sniffer

1. Откройте Настройки (CTRL-ALT-S / CMD-,)
2. Перейдите в Языки и Фреймворки
3. Нажмите на PHP
4. Нажмите на Инструменты Качества
5. Нажмите на стрелку раскрытия PHP cs fixer
6. По умолчанию конфигурация установлена на Локально
7. Нажмите на 3 точки позади неё, чтобы открыть экран конфигурации
8. Первая опция - это путь к PHP Code Sniffer (phpcs)
9. Нажмите на 3 точки позади пути, чтобы выбрать местоположение файла phpcs. Смотрите выше, где может быть установлен phpcs на вашем сайте
10. Нажмите на Подтвердить, чтобы убедиться, что путь правильный и phpcs работает
11. Нажмите ОК

#### Активация стиля кода

1. Откройте настройки (CTRL-ALT-S / CMD-,)
2. Перейдите в Редактор
3. Нажмите на Проверки
4. В списке перейдите к PHP
5. Нажмите на Проверка синтаксиса PHP Code Sniffer
6. Нажмите на флажок рядом с ним, чтобы активировать
7. Нажмите кнопку Перезагрузить (2 стрелки), чтобы принудительно перезагрузить правила с диска
8. Joomla теперь должна быть доступна в списке. См. следующее изображение.
9. Нажмите OK

![CodeSniffer in PHPStorm](../../../en/images/getting-started/core-phpstorm-code-sniffer.png)

### Установка в Netbeans

Netbeans имеет функционал сниффера, интегрированный в основную систему.

1. Запустите вашу Netbeans IDE
2. Откройте **Инструменты → Параметры → PHP → Анализ кода → Кодовый анализатор**
3. Вам нужно установить путь к *phpcs.bat* из установленного пакета PHP_CodeSniffer PEAR
    - Для Unix-систем путь будет чем-то вроде /usr/bin/phpcs
    - В XAMPP (windows) вы можете найти файл в корневой папке PHP (например, C:\xampp\php\phpcs.bat)
4. В качестве "Стандарт по умолчанию" выберите "Joomla", чтобы использовать стандарт Joomla!
5. теперь вы можете нажать *OK*, чтобы начать анализ
6. Откройте из верхнего меню **Источник → Проверить...**
7. Наслаждайтесь

### Установка в Eclipse

1. **Справка → Установить новое программное обеспечение...**
2. **Работать с** Введите один из URL-адресов обновлений.
3. Выберите нужные инструменты.
4. Перезапустите Eclipse.
5. **Окно → Настройки**
6. **PHP Инструменты → PHP CodeSniffer**

![Eclipse PTI settings](../../../en/images/getting-started/core-eclipse-pti-settings.png)

Теперь вы можете обнаруживать нарушения кода относительно общих стандартов.

![Codesniffer in Eclipse](../../../en/images/getting-started/core-eclipse-pti.png)

### Установка в Geany

- Откройте PHP-файл. (В противном случае меню сборки не будет доступно.)
- В верхнем меню выберите **Сборка → Установить команды сборки**.
- Выберите второе поле и назовите его по желанию. Введите следующий код в команду:<br>
  `phpcs --standard=Joomla "%f" | sed -e 's/^/%f |/' | egrep 'WARNING|ERROR'`
- Введите следующий код в поле регулярного выражения для ошибок: `(.+) [|]\s+([0-9]+)`
- Выберите ОК.
- Если окно сообщений не открыто, отобразите его, выбрав в верхнем меню Вид.
- При просмотре любого PHP-файла нажмите F9, чтобы увидеть найденные ошибки.

### Установка в Atom

- Установите phpcs и стандарты Joomla, как описано выше.
- В **Atom → Preferences → Install** установите **linter-phpcs** и все его требования.
- В **Atom → Preferences → Packages → linter-phpcs → Settings** настройте
  - **Executable Path** на путь к вашему phpcs
  - **Code Standard Or Config File**: PSR12
  - **Tab Width**: 4

*Переведено openai.com*

