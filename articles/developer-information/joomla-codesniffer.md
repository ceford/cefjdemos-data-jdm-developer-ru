<!-- Filename: Joomla_CodeSniffer / Display title: Joomla CodeSniffer -->

## Анализатор кода для Joomla

Это кастомный стандарт оформления кода для PHP CodeSniffer, который
пытается привести всё к единому стандарту оформления кода в Joomla. В
этом материале описывается, как настроить автоматическую проверку стиля
оформления кода.

1.  Установка PHP CodeSniffer (phpcs).
2.  Клонирование Joomla Code Style (для использования с phpcs).
3.  Настройки вашей IDE для работы с PHP CodeSniffer и Joomla Code
    Style.

<img
src="https://docs.joomla.org/images/thumb/e/e4/CodeSnifferInAction.jpg/300px-CodeSnifferInAction.jpg"
class="thumbimage" decoding="async"
srcset="https://docs.joomla.org/images/thumb/e/e4/CodeSnifferInAction.jpg/450px-CodeSnifferInAction.jpg 1.5x, https://docs.joomla.org/images/thumb/e/e4/CodeSnifferInAction.jpg/600px-CodeSnifferInAction.jpg 2x"
data-file-width="995" data-file-height="641" width="300" height="193" />
<a href="https://docs.joomla.org/File:CodeSnifferInAction.jpg"
class="internal" title="Enlarge"></a>PHP Code Sniffer in action

## Зачем?

- Понятная и устойчивая практика написания кода заставляет выглядеть
  файлы более профессионально. Конфликтующие стили в том же проекте (или
  ещё хуже, в том же файле) не только выглядят небрежно, но и поощряют
  ещё большую небрежность.
- Когда весь код соответствует единому стандарту, плохой код намного
  легче обнаружить.
- Для того, кто только начал разбираться с файлом в проекте, намного
  проще найти и исправить ошибки или расширить функциональность.
- Если нет постоянно поддерживаемого стандарта, разработчики будут
  форматировать текст как им удобнее. Это приводит к широкому диапазону
  изменений в репозитории с кодом. Если проблема обнаружится на
  последней стадии, то значительная часть изменений может быть утеряна
  при выборке во время diff.

## 1. Установка PHP Code Sniffer (phpcs)

### Composer

PHP Code Sniffer доступен через Composer. Он может быть установлен в
рамках всей системы следующей командой:  
`composer global require --dev squizlabs/php_codesniffer "~3.5"` На
**Linux** PHP Code Sniffer (phpcs) будет установлен в папку вашего
пользователя:

- Символическая ссылка: `~/.composer/vendor/bin/phpcs`
- Файл: `~/.composer/vendor/squizlabs/php_codesniffer/scripts/phpcs`

Для создания символической ссылки, используйте командную строку:
` $ sudo ln -s ~/.config/composer/vendor/squizlabs/php_codesniffer/scripts /usr/bin/phpcs `

или (например OpenSuse)

- Символическая ссылка: `~/.config/composer/vendor/bin/phpcs`
- Файл:
  `~/.config/composer/vendor/squizlabs/php_codesniffer/scripts/phpcs`

На **Windows** PHP Code Sniffer (phpcs) будет установлен в папку вашего
пользователя:

- Символическая ссылка:
  `c:\Users\Username\AppData\Roaming\Composer\vendor\bin\phpcs`
- Файл:
  `c:\Users\Username\AppData\Roaming\Composer\vendor\squizlabs\php_codesniffer\scripts\phpcs`

### Добавление директории bin composer к переменной \$PATH

Для того, чтобы запускать PHP Code sniffer откуда угодно в командной
строке Linux или Mac, необходимо добавить `.composer/vendor/bin` к вашей
переменной \$PATH. Для того, чтобы зафиксировать это на постоянной
основе, отредактируйте стартовый файл оболочки, например .profile или
.bash_profile, в зависимости от того, какую оболочку вы используете
(echo \$SHELL покажет вам, какую вы используете).

Добавьте следующую строку в файл:
`export PATH=$PATH:~/.composer/vendor/bin`

В качестве альтернативы вы можете создать симовлическую ссылку в папке
bin: `ln -s ~/.composer/vendor/bin/phpcs /usr/local/bin/phpcs`

## 2. Установка Joomla Coding Standards

Для того, чтобы PHP Code Sniffer (phpcs) начал "нюхать" (проверять) ваши
проекты Joomla на правильный стиль оформления кода, вам необходимо
установить Joomla Coding Standards (стандарты оформления кода в Joomla).

Используя composer, мы можем запустить:
`composer global require --dev joomla/coding-standards "~3.0@alpha"`

Обратите внимание на `@alpha`. Всё потому, что стандарт всё ещё в стадии
альфа, и мы должны позволить установку кода, со статусом alpha. Если мы
не укажем @alpha, то стандарты не будут установлены и выкинут ошибку.

    Your requirements could not be resolved to an installable set of packages.

      Problem 1
        - The requested package joomla/coding-standards ~3.0 is satisfiable by joomla/coding-standards[3.0.0-alpha, 3.0.0-alpha2, 3.x-dev] but these conflict with your requirements or minimum-stability.

Как только установка завершится, стандарт оформления кода будет
установлен в глобальную папку composer. Расположение этой папки зависит
от вашей операционной системы. Вот некоторые популярные места
расположения:

**Mac / Linux**

    ~/.composer/vendor/joomla/coding-standards/

    ~/.config/composer/vendor/joomla/coding-standards/

**Windows**

    c:\Users\Username\AppData\Roaming\Composer\vendor\joomla\coding-standards\

Так это выглядит при использовании Composer в Windows.

Если вы не можете найти ни одну из выше указанных папок, то учите, что
папка, которую вы ищите, заканчивается на **\joomla\coding-standards\\**
И наконец, мы должны дать понятьCode Sniffer, что стандарты оформления
кода Joomla существуют.

**Установка Joomla Coding Standards**

1.  Проверьте, пути установлены или нет, запустив команду
    `phpcs --config-show`
2.  Вы можете получить ответ, который выглядит вот так:
    `installed_paths: /path/to/installation`
3.  На шаге 2, если вы видите **installed_paths**, скопируйте их
4.  Установите путь Joomla Coding Standards в phpcs вызовом команды
    `phpcs --config-set installed_paths /Users/user/.composer/vendor/joomla/coding-standards`.
    Если вы скопировали пути на шаге 2, включите их здесь, разделяя
    запятой. Команда будет выглядеть как
    `phpcs --config-set installed_paths [/to/path1],[/to/path2],[/Users/user/.composer/vendor/joomla/coding-standards]`
5.  Убедитесь, что путь установлен верно, запустив команду
    `phpcs --config-show`
6.  Убедитесь, что phpcs видит стандарт оформления кода Joomla, запустив
    команду `phpcs -i`
7.  Вывод должен выглядеть вот так:
    `The installed coding standards are MySource, PEAR, PHPCS, PSR1, PSR2, Squiz, Zend and Joomla`

## Использование

Вы вызываете кастомный стандарт через
`phpcs --standard=Joomla file/to/sniff`

Для тестирования файлов platform со стандартом разработки platform,
используйте `phpcs --standard=build/phpcs/Joomla path/to/file/or/folder`

Дополнительная документация по использованию phpcs доступна здесь:
<a href="http://pear.php.net/package/PHP_CodeSniffer/docs"
class="external autonumber" target="_blank"
rel="nofollow noreferrer noopener">[1]</a>

## 3. Интеграция с IDE

Все любять консоль. Это, без сомнения, самый эффективный путь делать то,
что захочется. Но иногда и гуру Linux нуждаются в небольшом комфорте.

К счастью, для PhpStorm, Eclipse и Netbeans существует плагин, который
интегрирует CodeSniffer в вашу любимую IDE, поэтому нарушения стандартов
оформления кода показываются как "обычные" ошибки.

### PhpStorm

Code Sniffer поддерживается PhpStorm из коробки. Зайдите в Settings, и в
Editor  **→**  Inspections вы увидите список установленных снифферов.

#### Установите путь до Code Sniffer

1.  Откройте Settings (CTRL-ALT-S / CMD-,)
2.  Перейдите в Languages & Frameworks
3.  Щёлкните на PHP
4.  Щёлкните на Quality Tools
5.  Щёлкните на область PHP CS Fixer
6.  Настройка по умолчанию установлена в Local
7.  Щёлкните на 3 точки за ней для открытия настроечного экрана
8.  Первая опция и есть путь PHP Code Sniffer (phpcs)
9.  Щёлкните на 3 точки за путём для выбора расположения файла phpcs.
    Смотрите выше, где может быть установлен phpcs.
10. Щёлкните на Validate, чтобы убедиться в том, что путь верен и phpcs
    работает
11. Щёлкните OK

##### Активизация Joomla Code Style

1.  Откройте Settings (CTRL-ALT-S / CMD-,)
2.  Перейдите в Editor
3.  Щёлкните на Inspections
4.  В списке перейдите к PHP
5.  Щёлкните на PHP Code Sniffer Validation
6.  Щёлкните на чекбокс за ним, для активации
7.  Щёлкните на кнопку Reload (2 стрелки) для принудительной
    перезагрузки правил с диска
8.  Теперь в списке должна стать доступной Joomla. Смотрите на
    картинке:<img
    src="https://docs.joomla.org/images/thumb/9/9c/PhpStorm8CodeSniffer.png/500px-PhpStorm8CodeSniffer.png"
    decoding="async"
    srcset="https://docs.joomla.org/images/thumb/9/9c/PhpStorm8CodeSniffer.png/750px-PhpStorm8CodeSniffer.png 1.5x, https://docs.joomla.org/images/thumb/9/9c/PhpStorm8CodeSniffer.png/1000px-PhpStorm8CodeSniffer.png 2x"
    data-file-width="2446" data-file-height="1624" width="500" height="332"
    alt="PHPStorm CodeSniffer" />
9.  Щёлкните OK

#### PHP PSR-0, PSR-1 и PSR-2

- <a
  href="https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md</a>
- <a
  href="https://github.com/pmjones/fig-standards/blob/psr-1-style-guide/proposed/PSR-1-basic.md"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/pmjones/fig-standards/blob/psr-1-style-guide/proposed/PSR-1-basic.md</a>
- <a
  href="https://github.com/pmjones/fig-standards/blob/psr-1-style-guide/proposed/PSR-2-advanced.md"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/pmjones/fig-standards/blob/psr-1-style-guide/proposed/PSR-2-advanced.md</a>

##### Использование PHP PSR-1 и PSR-2 Sniff

CodeSniffer также может быть использован и для PSR-x Coding Standard. В
этом случае вы просто выбираете другой стандарт, а не Joomla.

##### Использование стиля офрмления кода Joomla

Было бы неплохо проверять, что стандарты действительно учитываются. А
ещё лучше, если бы PhpStorm помогал вам с форматированием по мере того,
как вы пишете код. Для использования стиля оформления кода в PhpStorm

1.  Откройте Settings (CTRL-ALT-S / CMD-,)
2.  Пройдите в Editor
3.  Щёлкните на Code Style
4.  Выберите Joomla в списке Scheme

#### PhpStorm / Альтернативный метод

**Примечание**: Метод **просрочен**, но может быть полезен, если вы
ищете возможности другой интегрции. Он также демонстрирует использование
внешних инструментов в PhpStorm - так что *не стоит его удалять*.

Code Sniffer также может быть легко интегрирован как внешний инструмент.
PhpStorm отобразит вывод в консоли, в том числе ссылки с возможностью
щелчка, содержащие номера строк и столбцов с файлами, которые содержат
ошибки.

- Щёлкните на "Settings" и найдите "External tools"
- Щёлкните "Add..."
- Выберите "Name", "Group" and "Description".
- Щёлкните "Output Filters"
  - Щёлкните "Add...", выберите название и введите в "Regular expression
    to match output" значение: `$FILE_PATH$:$LINE$:$COLUMN$`
- "Program": найдите в вашей системе исполняемый файл phpcs. Вы должны
  выбрать путь до *phpcs.bat* из установленного пакета PHP_CodeSniffer
  PEAR
  - Для Unix систем, путём будет что-то вроде /usr/bin/phpcs
  - В XAMPP (windows), вы можете найти файл в корне папки PHP (например,
    C:\xampp\php\phpcs.bat)
- "Parameters":
  - `--standard=/build/phpcs/Joomla` Путь до стандартов оформления кода
    Joomla.
  - `--report=emacs` Это сгенерирует простой список, содержащий ссылки
    на файлы ошибок
  - Опционально вы можете указать `-p` для "progress" или `-n` для
    "errors only".
  - Последним параметром должен быть `$FilePath$`, который должен
    указывать на папку или файл, которые вы хотите проверить.

Типичная строка "Parameters" на Linux системах может выглядеть вот так:

    -np --standard=/home/elkuku/libs/joomla/build/phpcs/Joomla --report=emacs $FilePath$

Теперь вы можете сделать правый щелчок на любом файле или папке и
выбрать сниффер из контекстного меню. Или можете добавить кнопку на
панель инструментов с красивым лого Joomla.
<img src="https://docs.joomla.org/images/6/67/Icon-16-joomla.png"
decoding="async" data-file-width="16" data-file-height="16" width="16"
height="16" alt="Icon-16-joomla.png" />.

### Netbeans

У Netbeans функциональность сниффера встроена в ядро системы.

1.  Запустите Netbeans IDE
2.  Откройте Tools =\> Options =\> PHP =\> Code Analysis =\> Code
    Sniffer
3.  Вы должны установить путь до *phpcs.bat* из установленного пакета
    PHP_CodeSniffer PEAR
    - Для Unix систем, путём будет что-то вроде /usr/bin/phpcs
    - В XAMPP (windows), вы можете найти файл в корне папки PHP
      (например, C:\xampp\php\phpcs.bat)
4.  Как "Default Standard," выберите "Joomla"
5.  Теперь можете щёлкнуть на *OK*
6.  Откройте из верхнего меню Source =\> Inspect...
7.  Наслаждайтесь

### Eclipse

<img
src="https://docs.joomla.org/images/thumb/4/44/Eclipse_pti.png/200px-Eclipse_pti.png"
class="thumbimage" decoding="async"
srcset="https://docs.joomla.org/images/thumb/4/44/Eclipse_pti.png/300px-Eclipse_pti.png 1.5x, https://docs.joomla.org/images/thumb/4/44/Eclipse_pti.png/400px-Eclipse_pti.png 2x"
data-file-width="1388" data-file-height="762" width="200"
height="110" />
<a href="https://docs.joomla.org/File:Eclipse_pti.png" class="internal"
title="Enlarge"></a>1) Eclipse PTI

Установка следует обычному шаблону:

1.  `Help => Install New Software...`
2.  `Work with:` Заполните одним из адресов URLs, которые можно найти
    здесь:
    <a href="http://www.phpsrc.org/eclipse/pti/" class="external free"
    target="_blank"
    rel="nofollow noreferrer noopener">http://www.phpsrc.org/eclipse/pti/</a>
3.  Выберите нужный инструмент
4.  Перезапустите Eclipse.

<img
src="https://docs.joomla.org/images/thumb/a/a1/Eclipse_pti_settings.png/150px-Eclipse_pti_settings.png"
class="thumbimage" decoding="async"
srcset="https://docs.joomla.org/images/thumb/a/a1/Eclipse_pti_settings.png/225px-Eclipse_pti_settings.png 1.5x, https://docs.joomla.org/images/thumb/a/a1/Eclipse_pti_settings.png/300px-Eclipse_pti_settings.png 2x"
data-file-width="973" data-file-height="819" width="150" height="126" />
<a href="https://docs.joomla.org/File:Eclipse_pti_settings.png"
class="internal" title="Enlarge"></a>2) Настройки Eclipse PTI

Теперь вы можете выполнять проверку кода согласно таким стандартам как
PEAR, Zend и т.п..

Для выполения проверки собственных стандартов, укажите расположение и
активизируйте (смотрите скриншот 2)

1.  `Window => Preferences`
2.  `PHP Tools => PHP CodeSniffer`

Удачного сниффинга.

### Geany

- Откройте PHP файл. (Иначе меню build недоступно.) Смотрите <a
  href="http://static.xscreenshot.com/2016/07/13/08/screen_6bc0692cf995702a1e379b39643d0c2d"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Screenshot</a>
- В верхнем меню выберите Build-\>Set Build Commands.
- Выберите второе поле и назовите его по своему желанию. Введите этот
  код в Command:
  `phpcs --standard=Joomla "%f" | sed -e 's/^/%f |/' | egrep 'WARNING|ERROR'`
- Введите этот код в поле Error Regular Expression:
  `(.+) [|]\s+([0-9]+)` See this
  <a href="http://view.xscreenshot.com/ef00820cf7c017ce659c8e9f0b02d3ae"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Screenshot</a>
- Выберите OK.
- Если окно Message не открыто, отобразите его, выбрав в верхнем меню
  View.
- При просмотре любого PHP файла, нажмите F9 для просмотра найденных
  ошибок.
  <a href="http://view.xscreenshot.com/8704b9fd1bda3f841364a0ffa28a54ae"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Screenshot</a>

### Atom

- Установите phpcs и стандарты Joomla, как описано выше.
- В **Atom \> Preferences \> Install** установите **linter-phpcs** и все
  его требования.
- В **Atom \> Preferences \> Packages \> linter-phpcs \> Settings**
  настройте
  - **Executable Path** путь до вашего phpcs
  - **Code Standard Or Config File**: Joomla
  - **Tab Width**: 4

### Ссылки

- <a href="https://github.com/joomla/coding-standards"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla/coding-standards</a> -
  Joomla! Coding Standards
- <a href="http://www.phpsrc.org/" class="external free" target="_blank"
  rel="nofollow noreferrer noopener">http://www.phpsrc.org/</a> PTI -
  инструменты интеграции PHP для Eclipse
- <a href="http://sourceforge.net/projects/phpmdnb/" class="external free"
  target="_blank"
  rel="nofollow noreferrer noopener">http://sourceforge.net/projects/phpmdnb/</a>
  Плагин Netbeans
- <a
  href="http://hakre.wordpress.com/2010/03/06/php-code-sniffer-eclipse-and-wordpress/"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">http://hakre.wordpress.com/2010/03/06/php-code-sniffer-eclipse-and-wordpress/</a> -
  Отличный материал. Просто поменяйте "Wordpress" на "Joomla!"
