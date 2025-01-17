<!-- Filename: J4.x:J4_Module_example_-_Mydownmsg / Display title: Пример: Сообщение о недоступности сайта -->

## Введение

Эта статья представляет собой полный модуль сайта Joomla для новых разработчиков, который можно разобрать, чтобы увидеть его структуру и понять, как он работает. Он является заменой предыдущего модуля, использовавшего старые кодовые соглашения. Этот модуль разработан для Joomla 5 и более поздних версий.

В документации для программистов Joomla есть вводные статьи по *Общим концепциям* и *Модулям*, а также они воспроизведены в Jdocmanual для вашего удобства. Эта статья похожа на JPD [Базовый модуль](jdocmanual?article=docus/modules/basic-module), но имеет некоторые дополнительные функции.

## Цель

Модуль предназначен для отображения временного сообщения на одном из нескольких языков в течение короткого периода перед закрытием сайта для системного обслуживания. Это дает пользователям возможность завершить свои действия до момента закрытия.

Название модуля — `mod_downmsg`, и сообщение, которое он отображает на английском, похоже на *Этот сайт будет закрыт на короткий период в 12:0 по Гринвичу*. Время и сообщение можно выбрать в зависимости от предстоящего закрытия сайта.

Модуль можно скачать с GitHub для установки или изучения по мере необходимости.

## Структура исходного кода

Следующее - это схематическая иллюстрация структуры исходного кода, используемого для создания расширения:

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- mod_downmsg (folder compressed to create an installable zip file)
    |-- help
        |-- en-GB
            |-- downmsg.html (this is Help screen used in the backend module edit form)
    |-- language
        |-- de-DE
            |-- mod_downmsg.ini (only frontend strings are included)
        |-- en-GB (language folder, kept with the extension code)
            |-- mod_downmsg.ini
            |-- mod_downmsg.sys.ini
        |-- fr-FR
            |-- mod_downmsg.ini (only frontend strings are included)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Dispatcher (folder for extension specific code)
            |-- Dispatcher.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- mod_downmsg.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

Это структура, как она выглядит в среде разработки VSCode или VSCodium:

![Plugin development file structure in vscodium](../../../en/images/modules/downmsg-module-vscodium.png)

## Файл Манифест

Файл манифеста управляет процессами установки, обновления и удаления расширения:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="module" method="upgrade" client="site">
    <name>mod_downmsg</name>
    <creationDate>December 2024</creationDate>
    <author>Clifford E Ford</author>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <authorUrl>http://jdocmanual.org/</authorUrl>
    <copyright>Copyright (C) 2024 Clifford E Ford, All rights reserved.</copyright>
    <license>GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html</license>
    <!--  The version string is recorded in the components table -->
    <version>0.1.0</version>
    <!-- The description is optional and defaults to the name -->
    <description>MOD_DOWNMSG_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Module\Downmsg</namespace>
    <files>
        <folder>help</folder>
        <folder>language</folder>
        <folder module="mod_downmsg">services</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
    <help url="MOD_DOWNMSG_HELP_URL" />
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Mod Downmsg Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="msg_id"
                    type="list"
                    label="MOD_DOWNMSG_PARAMS_MSG_ID_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MSG_ID_DESC"
                    default="v1"
                    required="true"
                >
                    <option value="v1">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1</option>
                    <option value="v2">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2</option>
                </field>
                <field
                    name="hour"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_HOUR_LABEL"
                    description="MOD_DOWNMSG_PARAMS_HOUR_DESC"
                    default="12"
                    min="0"
                    max="23"
                    required="true"
                />
                <field
                    name="minute"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_MINUTE_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MINUTE_DESC"
                    default="00"
                    min="00"
                    max="59"
                    required="true"
                />
                <field
                    name="tz"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_TZ_LABEL"
                    description="MOD_DOWNMSG_PARAMS_TZ_DESC"
                    default="0"
                    min="-11"
                    max="11"
                    required="true"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

### Заметки об элементах

- Атрибуты **extension**:
    - **type="module"** указывает тип расширения.
    - **method="upgrade"** означает, что это расширение может быть установлено и обновлено, если становятся доступны новые версии.
    - **client="site"** сообщает Joomla, что это модуль *Сайта*, а не *Администратора*.
- Оператор **version** используется для управления обновлением. Если доступен сервер обновлений, версия сравнивается с версией, записанной там. Это также предотвращает перезапись новой версии старой версией.
- Атрибут пути **namespace** говорит Joomla, где искать классы пространства имен.
- Раздел **files** перечисляет папки и файлы для установки. Если элемент не присутствует здесь, он не будет установлен, даже если он присутствует в архивированном установочном файле.
- Атрибут URL **help** позволяет использовать пользовательский файл помощи в форме редактирования модуля. Путь к файлу помощи является значением для данного ключа в файле en-GB/mod_downmsg.ini.
- Раздел **config** показывает, какие параметры нужны этому модулю. Все они должны иметь значения по умолчанию, так как они устанавливаются при установке.

## Языковые файлы

Этот модуль содержит очень мало строк. Те, что в файлах **sys.ini**, используются при установке и для перечисления среди других модулей. Файлы **.ini** используются в форме администратора и при отображении модуля на сайте. Обратите внимание на соглашения по идентификации строк:

MOD\_\[название\]\_\[назначение\]\_\[использование\]

Файлы ниже показывают, что переводы на немецкий и французский языки включают только строки, которые видят посетители сайта, так как форма Параметров всегда будет управляться на английском языке. На случай, если вы не знали: строки en-GB всегда загружаются первыми по умолчанию, чтобы случайно непереведенные строки не отображались как ключи строк. Затем загружаются строки необходимого языка, которые перезаписывают эквивалентные английские строки.

Французская и немецкая версии строк здесь были получены с помощью инструментов Google Translation. Это может лучше работать для предложений, чем для отдельных слов!

### en-GB/mod_downmsg.ini

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."

MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"

MOD_DOWNMSG_MSG_V1="This site will close for a short period at %s"
MOD_DOWNMSG_MSG_V2="Please log out. This site will close for one hour or more at %s"

MOD_DOWNMSG_PARAMS_MSG_ID_LABEL="Select Message version"
MOD_DOWNMSG_PARAMS_MSG_ID_DESC="The short downtime or long downtime message vesion."

MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1="Short < 1 hour"
MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2="Long > 1 hour"

MOD_DOWNMSG_PARAMS_HOUR_LABEL="Hour"
MOD_DOWNMSG_PARAMS_HOUR_DESC="Set the hour when the site will close"

MOD_DOWNMSG_PARAMS_MINUTE_LABEL="Minute"
MOD_DOWNMSG_PARAMS_MINUTE_DESC="Set the minutes when the site will close"

MOD_DOWNMSG_PARAMS_TZ_LABEL="Time Zone"
MOD_DOWNMSG_PARAMS_TZ_DESC="Set your time zone offset from GMT"
```

### en-GB/mod_downmsg.sys.ini

Этот файл используется для управления системой.

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."
```

### de-DE/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Diese Seite wird für kurze Zeit um %s geschlossen"
MOD_DOWNMSG_MSG_V2="Bitte melden Sie sich ab. Diese Seite wird für eine Stunde oder länger um %s geschlossen"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

### fr-FR/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Ce site sera fermé pour une courte période à %s"
MOD_DOWNMSG_MSG_V2="Veuillez vous déconnecter. Ce site sera fermé pendant une heure ou plus à %s"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

## Код модуля

Код модуля невероятно прост. Существует три PHP файла:

- services/provider.php (код внедрения зависимостей).
- src/Dispatcher/Dispatcher.php (собирает данные для отображения).
- tmpl/default.php (шаблон для отображения данных, который может быть переопределен).

### services/provider.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\Service\Provider\Module;
use Joomla\CMS\Extension\Service\Provider\ModuleDispatcherFactory;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

/**
 * The module Downmsg service provider.
 *
 * @since  4.4.0
 */
return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.4.0
     */
    public function register(Container $container): void
    {
        $container->registerServiceProvider(new ModuleDispatcherFactory('\\Cefjdemos\\Module\\Downmsg'));
        $container->registerServiceProvider(new Module());
    }
};
```

### src/Dispatcher/Dispatcher.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Cefjdemos\Module\Downmsg\Site\Dispatcher;

use Joomla\CMS\Dispatcher\AbstractModuleDispatcher;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Dispatcher class for mod_downmsg
 *
 * @since  4.4.0
 */
class Dispatcher extends AbstractModuleDispatcher
{
    /**
     * Returns the layout data.
     *
     * @return  array
     *
     * @since   4.4.0
     */
    protected function getLayoutData()
    {
        $data = parent::getLayoutData();

        return $data;
    }
}
```

### tmpl/default.php

```php
<?php

/**
 * @package     Cefjdemos.Module
 * @subpackage  mod_downmsg
 *
 * @copyright   Copyright (C) 2019 Clifford E Ford. All rights reserved.
 * @license     GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

// the $msg string contains a %s placeholder to be replaced in a sprintf statement
$msg = Text::_('MOD_DOWNMSG_MSG_' . strtoupper($params->get('msg_id')));

$tod = $params->get('hour') . ':' . $params->get('minute');
$tz = $params->get('tz');

if ($tz > 0) {
    $tz = '(+' . $tz . ')';
} elseif ($tz < 0) {
    $tz = '(-' . $tz . ')';
} else {
    $tz = '';
}

$tod .= ' GMT ' . $tz;

?>

<div class="alert alert-warning text-center" role="alert">
    <?php echo sprintf($msg, $tod); ?>
</div>
```

## Установка

Из меню Администратора перейдите на страницу Система / Установка / Расширения и используйте вкладку Загрузить файл пакета для загрузки zip-файла. Затем перейдите в Контент / Модули сайта и отредактируйте недавно установленный модуль.

В форме редактирования модуля:

1. Введите заголовок. Помните, что модуль может иметь более одного экземпляра, поэтому они могут появляться в разных местах или с разными параметрами.
2. Установите время, когда сайт будет отключен.
3. Установите часовой пояс (вероятно, есть более хороший способ, чем использование GMT +- n часов).

В правом списке общих параметров

1. Установите **Заголовок** в положение **скрыть**.
2. Выберите **Позицию** шаблона - в Cassiopeia **topbar** размещает сообщение над содержимым страницы, идеально.
3. Установите **Статус** в положение **Опубликовано**.
4. На вкладке **Назначение меню** выберите **На всех страницах**.
5. **Сохраните**, и вы готовы проверить внешний вид сайта.

![the module edit form](../../../en/images/modules/downmsg-module-edit-form.png)

## Тестирование

Вот как сообщение выглядит на английском языке:

![site down message in english](../../../en/images/modules/downmsg-module-result-en.png)

На немецком:

![site down message in english](../../../en/images/modules/downmsg-module-result-de.png)

И на французский:

![site down message in english](../../../en/images/modules/downmsg-module-result-fr.png)

## Обновление сайта и журнал изменений

Эти элементы включены в источник, но не рассматриваются здесь.

*Переведено openai.com*

