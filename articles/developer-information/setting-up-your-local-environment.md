<!-- Filename: J4.x:Setting_Up_Your_Local_Environment / Display title: Настройка локальной среды -->

## Краткое руководство по началу работы

Для тестовой установки вам понадобится программный стек, который включает в себя Apache, MySQL и PHP. Шаги, необходимые для этого, описаны в другой части данного руководства. Если сомневаетесь, используйте программный стек, подходящий для вашей платформы.

## Дополнительные инструменты

Для тестирования запросов на внесение изменений в Joomla и вклада в основной код Joomla вам понадобятся дополнительные инструменты, все из которых легко установить, часто одной строкой команды:

1. **Composer** - для управления PHP-зависимостями Joomla. Ознакомьтесь с [документацией Composer](https://getcomposer.org/doc/00-intro.md) для получения помощи по установке.
2. **Node.js** - для компиляции JavaScript и SASS файлов Joomla. Ознакомьтесь с [документацией Node.js](https://nodejs.org/en/) для получения помощи по установке.
3. **Git** - для управления версиями. Ознакомьтесь с [документацией git](https://git-scm.com/) для получения помощи по установке.

## Шаги по настройке локального тестового сайта

1. Перейдите в [репозиторий Joomla! на GitHub](https://github.com/joomla/joomla-cms).
2. Выберите ветку Joomla, с которой хотите работать. Это выберет соответствующие инструкции в README.
3. Пролистайте страницу вниз до раздела **Steps to setup the local environment:** в тексте README.
4. Следуйте перечисленным там шагам. Вы можете изменить имя клонированной папки joomla-cms, чтобы создать столько клонов, сколько вам нужно, для разных целей.
5. Создайте базу данных и установите Joomla, но не удаляйте папку установки в конце процесса установки.

## Обновление Joomla

Время от времени вам может потребоваться обновить клон Joomla. Это однострочная команда, которая обычно выполняется быстро. Однако, как правило, необходимо заново сгенерировать файлы CSS и JavaScript, что является более сложным и длительным процессом.

Пользователи Linux и OSX могут настроить следующий псевдоним bash, добавив следующее в *файл ~/.bashrc*:

```sh
    alias jclean="rm -rf administrator/templates/atum/css; \
    rm -rf templates/cassiopeia/css; \
    rm -rf administrator/templates/system/css; \
    rm -rf templates/system/css; \
    rm -rf media/; \
    rm -rf node_modules/; \
    rm -rf libraries/vendor/; \
    rm -f administrator/cache/autoload_psr4.php; \
    rm -rf installation/template/css"
    alias jinstall="jclean; composer install; npm ci"
```

Это удалит все скомпилированные файлы и выполнит новую установку одной командой, вызвав `jinstall` внутри вашей установки Joomla. Вы также можете использовать команду `jclean`, чтобы переключиться обратно на другую ветку Joomla.

**Примечание:** Возможно, вам потребуется запустить `composer install` с опцией `--ignore-platform-reqs`, чтобы игнорировать требования платформы, указанные в Composer. Это необходимо, если у вас не установлен LDAP-расширение для PHP.

### Изменения в базе данных

Если обновление Joomla включает изменения в базе данных, вам, возможно, потребуется найти индивидуальные изменения и выполнить их вручную или начать заново с новым клоном.

## Скрипты Node/npm

Установка Joomla из клона репозитория использовала две команды:

- **composer install** Установить все необходимые пакеты composer.
- **npm ci** Установить все необходимые пакеты npm.

Node.js поставляется с менеджером пакетов под названием NPM, у которого есть команда `run`. Некоторые скрипты доступны, чтобы ускорить процесс сборки, если были изменены только файлы CSS или JavaScript.

### npm run build:css

Эта команда компилирует файлы SASS в CSS, а также создает minimизированные файлы.

### npm run build:js

Эта команда компилирует и транспилирует файлы JavaScript в правильный формат и создает минифицированные файлы.

### npm run watch

Эта команда такая же, как команда `build:js`, но будет отслеживать изменения и автоматически создавать обновленные файлы в каталоге media. Файлы SASS пока не включены.

### npm run lint:js

Эта команда выполняет проверку синтаксиса всех файлов JavaScript ES6 в соответствии со стандартом кода JavaScript. Для получения дополнительной информации см. [Руководство по стандартам кодирования Joomla](https://developer.joomla.org/coding-standards/introduction.html).

### npm run test

Эта команда запустит набор тестов JavaScript.

## Возможные проблемы

При выполнении команды composer install вы можете столкнуться с этими ошибками.

```sh
    Problem 1
        - Installation request for joomla/ldap 2.0.0-beta -> satisfiable by joomla/ldap[2.0.0-beta].
        - joomla/ldap 2.0.0-beta requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
    Problem 2
        - Installation request for symfony/ldap v5.1.5 -> satisfiable by symfony/ldap[v5.1.5].
        - symfony/ldap v5.1.5 requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
```

Решением является выполнение команды composer install с опцией `--ignore-platform-reqs`, чтобы игнорировать требования платформы, указанные в Composer. То есть, если у вас не установлен LDAP-расширение PHP.

```sh
    composer install --ignore-platform-reqs
```

Если вы получаете ошибку входа, удалите файл `administrator/cache/autoload_psr4.php`.

*Переведено openai.com*

