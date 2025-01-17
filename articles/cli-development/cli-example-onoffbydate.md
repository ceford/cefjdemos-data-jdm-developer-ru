<!-- Filename: J4.x:CLI_example_-_Onoffbydate / Display title: CLI пример - Onoffbydate -->

## Введение

Есть случаи, когда сайту необходимо отображать или скрывать модуль в зависимости от даты. Например, пользовательский HTML-модуль, показывающий сообщение в зимний период. Другой пример — чередование пользовательских модулей в зависимости от дня недели, например один для будних дней и другой для выходных. В представленном здесь примере используется плагин, команда CLI и планировщик cron для достижения желаемого эффекта.

Код доступен на [GitHub](https://github.com/ceford/cefjdemos-plg-onoffbydate)

Существует больше статей об использовании CLI в [Руководстве пользователя](http://localhost/jdm4/jdocmanual?article=user/command-line-interface/using-the-cli) и в Документации программистов Joomla! [локальная копия](jdocmanual?article=docus/plugins/plugin-examples-console-plugin-sqlfile) или [оригинальный источник](https://manual.joomla.org/docs/building-extensions/plugins/plugin-examples/console-plugin-sqlfile/);

## Стандарты Joomla

В более ранних версиях Joomla система плагинов использовала реализацию паттерна Observable/Observer. В результате каждый загружаемый плагин сразу имел все свои публичные методы, зарегистрированные как обозреватели. Это могло вызывать проблемы.

Joomla 4 и более поздние версии используют пакет события Joomla Framework для работы с событиями плагина. Это обеспечивает лучшую производительность и безопасность. На практике это означает, что файловая структура плагина Joomla 4/5/6 отличается от структуры устаревшего плагина из более ранних версий.

## Структура плагина

Плагин называется **onoffbydate**. Следующая схема показывает структуру файлов, используемую для локальной разработки плагина:

```
cefjdemos-plg-onoffbydate
|-- .vscode (folder for build settings)
|-- cefjdemos-plg-onoffbydate (folder compressed to create an installable zip file)
    |-- language
        |-- en-GB (language folder, kept with the extension code)
            |-- plg_system_onoffbydate.ini
            |-- plg_system_onoffbydate.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Console (folder for extension specific code)
            |-- OnoffbydateCommand.php (the plugin execution code)
        |-- Extension
            |-- Onoffbydate.php (the plugin register code)
    |-- onoffbydate.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (a record of changes in each release)
|-- README.md (brief description and instructions on use)
|-- updates.xml (the update server specification)
```

## Файл манифеста onoffbydate.xml

Это файл установки - стандартный элемент для любого расширения.

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="plugin" group="console" method="upgrade">
    <name>plg_console_onoffbydate</name>
    <author>Clifford E Ford</author>
    <creationDate>Jamuary 2025</creationDate>
    <copyright>(C) Clifford E Ford</copyright>
    <license>GNU General Public License version 3 or later</license>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <version>0.3.0</version>
    <description>PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Console\Onoffbydate</namespace>
    <files>
        <folder>language</folder>
        <folder plugin="onoffbydate">services</folder>
        <folder>src</folder>
    </files>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-onoffbydate/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemosonoffbydate Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-onoffbydate/main/updates.xml</server>
    </updateservers>
    <config>
        <fields>

        </fields>
    </config>
</extension>
```

- Строка **namespace** указывает Joomla, где найти код с пространствами имен для этого плагина.
- Папка **language** находится вместе с кодом плагина.
- **updateserver** необходим для соблюдения требований JED.
- Для этого плагина нет параметров **config**, но это может быть изменено в будущем. Например, пользователь может предпочесть устанавливать зимние месяцы с помощью флажков, а не хардкодить их.

## Регистрация: services/provider.php

Это точка входа для кода плагина.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\CMS\Factory;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Console\Onoffbydate\Extension\Onoffbydate;

return new class implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.2.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $dispatcher = $container->get(DispatcherInterface::class);
                $plugin     = new Onoffbydate(
                    $dispatcher,
                    (array) PluginHelper::getPlugin('console', 'onoffbydate')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## Подписка на событие: src/Extension/Onoffbydate.php

Это место, где регистрируется событие, запускающее плагин, и задается расположение кода, который будет обрабатывать событие.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

namespace Cefjdemos\Plugin\Console\Onoffbydate\Extension;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\SubscriberInterface;
use Cefjdemos\Plugin\Console\Onoffbydate\Console\OnoffbydateCommand;

final class Onoffbydate extends CMSPlugin implements SubscriberInterface
{
    /**
     * Returns the event this subscriber will listen to.
     *
     * @return  array
     */
    public static function getSubscribedEvents(): array
    {
        return [
                \Joomla\Application\ApplicationEvents::BEFORE_EXECUTE => 'registerCommands',
        ];
    }

    /**
     * Returns the command class for the Onoffbydate CLI plugin.
     *
     * @return  void
     */
    public function registerCommands(): void
    {
        $myCommand = new OnoffbydateCommand();
        $myCommand->setParams($this->params);
        $this->getApplication()->addCommand($myCommand);
    }
}
```

Класс **OnoffbydateCommand** находится в папке *src/Console*, так как встроенные команды консоли Joomla находятся в папке Console. Он мог бы быть помещен в папку *Extension*.

## Файл команды: src/Console/OnoffbydateCommand.php

Этот файл содержит код, который выполняет всю работу для этого расширения.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

namespace Cefjdemos\Plugin\Console\Onoffbydate\Console;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Factory;
use Joomla\CMS\Language\Text;
use Joomla\Console\Command\AbstractCommand;
use Joomla\Database\DatabaseInterface;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\Console\Style\SymfonyStyle;
use Joomla\Database\DatabaseAwareTrait;

class OnoffbydateCommand extends AbstractCommand
{
    use DatabaseAwareTrait;

    /**
     * The default command name
     *
     * @var    string
     *
     * @since  4.0.0
     */
    protected static $defaultName = 'onoffbydate:action';

    /**
     * @var InputInterface
     * @since version
     */
    private $cliInput;

    /**
     * SymfonyStyle Object
     * @var SymfonyStyle
     * @since 4.0.0
     */
    private $ioStyle;

    /**
     * The params associated with the plugin, plus getter and setter
     * These are injected into this class by the plugin instance
     */
    protected $params;

    protected function getParams()
    {
        return $this->params;
    }

    public function setParams($params)
    {
        $this->params = $params;
    }

    /**
     * Initialise the command.
     *
     * @return  void
     *
     * @since   4.0.0
     */
    protected function configure(): void
    {
        $lang = Factory::getApplication()->getLanguage();
        $test = $lang->load(
            'plg_console_onoffbydate',
            JPATH_BASE . '/plugins/console/onoffbydate'
        );

        $this->addArgument(
            'season',
            InputArgument::REQUIRED,
            'winter or weekend'
        );
        $this->addArgument(
            'action',
            InputArgument::REQUIRED,
            'publish or unpublish'
        );

        $this->addArgument(
            'module_id',
            InputArgument::REQUIRED,
            'module id'
        );

        $help = Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_1');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_2');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_3');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_4');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_5');

        $this->setDescription(Text::_('PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION'));
        $this->setHelp($help);
    }

    /**
     * Internal function to execute the command.
     *
     * @param   InputInterface   $input   The input to inject into the command.
     * @param   OutputInterface  $output  The output to inject into the command.
     *
     * @return  integer  The command exit code
     *
     * @since   4.0.0
     */
    protected function doExecute(InputInterface $input, OutputInterface $output): int
    {
        $this->configureIO($input, $output);

        $season = $this->cliInput->getArgument('season');
        $action = $this->cliInput->getArgument('action');
        $module_id = $this->cliInput->getArgument('module_id');

        switch ($season) {
            case 'winter':
                $result = $this->winter($module_id, $action);
                break;
            case 'weekend':
                $result = $this->weekend($module_id, $action);
                break;
            default:
                $result = Text::_('PLG_CONSOLE_ONOFFBYDATE_ERROR', $season);
                $this->ioStyle->error($result);
                return 0;
        }

        return 1;
    }

    protected function publish($module_id, $published)
    {
        $db = Factory::getContainer()->get(DatabaseInterface::class);
        //$db    = $this->getDatabase();
        $query = $db->getQuery(true);
        $query->update('#__modules')
            ->set('published = ' . $published)
            ->where('id = ' . $module_id);
        $db->setQuery($query);
        $db->execute();
    }

    protected function weekend($module_id, $action)
    {
        // get the day of the week
        $day = date('w');
        if (in_array($day, array(0,6))) {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_A_WEEKEND');
            $published = $action === 'publish' ? 1 : 0;
        } else {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_A_WEEKEND');
            $published = $action === 'publish' ? 0 : 1;
        }

        $this->publish($module_id, $published);

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');
        $result = Text::sprintf('PLG_CONSOLE_ONOFFBYDATE_SUCCESS', $msg, $module_id, $state);

        $this->ioStyle->success($result);
    }

    protected function winter($module_id, $action)
    {
        // get the month of the month
        $month = date('n');
        if (in_array($month, array(1,2,11,12))) {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_WINTER');
            $published = $action === 'publish' ? 1 : 0;
        } else {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_WINTER');
            $published = $action === 'publish' ? 0 : 1;
        }

        $this->publish($module_id, $published);

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');
        $result = Text::sprintf('PLG_CONSOLE_ONOFFBYDATE_SUCCESS', $msg, $module_id, $state);

        $this->ioStyle->success($result);
    }

    /**
     * Configures the IO
     *
     * @param   InputInterface   $input   Console Input
     * @param   OutputInterface  $output  Console Output
     *
     * @return void
     *
     * @since 4.0.0
     *
     */
    private function configureIO(InputInterface $input, OutputInterface $output)
    {
        $this->cliInput = $input;
        $this->ioStyle = new SymfonyStyle($input, $output);
    }
}
```

- Функция `configure` устанавливает, что требуются три аргумента командной строки:
    - `season` должен быть одним из `weekend` или `winter`.
    - `action` должен быть одним из `publish` или `unpublish`.
    - `module_id` должен быть целочисленным идентификатором модуля, который будет опубликован или снят с публикации.
- Функция `doExecute` - это место, где выполняется работа. Признаются две опции действий:
    - `winter` вызывает функцию для публикации или снятия с публикации модуля, если сегодня зимние месяцы.
    - `weekend` вызывает функцию для публикации или снятия с публикации модуля, если сегодня выходной день.
- Функция `publish` вызывает базу данных для установки значения поля `publishes` модуля в `0` или `1` в зависимости от ситуации.
- Функции `winter` и `weekend` выполняют некоторые вычисления даты, чтобы определить, является ли сегодня зимним днем (или нет) или выходным (или нет), для передачи соответствующих параметров функции публикации.
- Вызов функции `$this->ioStyle->success()` выводит текстовое сообщение на консоли с черным текстом на зеленом фоне. `$this->ioStyle->error()` выводит сообщение ОБ ОШИБКЕ с белым текстом на темно-красном фоне.

## языковые файлы

В процессе установки и настройки плагина используются два языковых файла. Файл en-GB/plg_console_onoffbydate.sys.ini содержит строки, которые отображаются во время установки и управления:

```ini
PLG_CONSOLE_ONOFFBYDATE="Console - Onoffbydate"
PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION="Set date-dependent enabled state of a module via cli"
```

Файл en-GB/plg_console_onoffbydate.ini содержит строки, используемые в:

```ini
PLG_CONSOLE_ONOFFBYDATE="Console - Onoffbydate"
PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION="Set date-dependent enabled state of a module via cli"
PLG_CONSOLE_ONOFFBYDATE_ERROR="Unknown season: %s."
PLG_CONSOLE_ONOFFBYDATE_HELP_1="<info>%command.name%</info> Toggles module Enabled/Disabled state\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_2="Usage: <info>php %command.full_name% season action module_id where\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_3="    season is one of winter or weekend,\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_4="    action is one of publish or unpublish and\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_5="    module_id is the id of the module to publish or unpublish.</info>\n"
PLG_CONSOLE_ONOFFBYDATE_SUCCESS="That seemed to work. %s Module %s has been %s."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_A_WEEKEND="Today is in a weekend."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_WINTER="Today is in winter."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_A_WEEKEND="Today is not in a weekend."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_WINTER="Today is not in winter."
```

Обратите внимание, что вывод команды виден только вам на вашей установке Joomla.

## Модуль

Этот код просто изменяет состояние опубликованности модуля в зависимости от некоторой функции даты. Поэтому вам нужен идентификатор модуля, как показано в правом столбце страницы списка модулей (сайт).

## Командная строка

Установите плагин в вашу тестовую установку Joomla и включите его. Если он вызывает сбой вашего сайта, используйте phpMyAdmin, чтобы найти новый установленный плагин в таблице #__extensions и установите его параметр enabled в 0. Исправьте проблему и попробуйте снова.

В окне терминала перейдите в корневой каталог вашего сайта и используйте следующую команду:

```sh
php cli/joomla.php list
```

Если что-то пойдет не так на этом этапе, проверьте, что версия php, вызываемая командной строкой, а не та, что используется веб-сервером. Вы можете подавить сообщения о депрекации с этой версией командной строки.

```sh
php -d error_reporting="E_ALL & ~E_DEPRECATED" cli/joomla.php onoffbydate:action garbage publish 133
```

Если код работает, вы увидите `onoffbydate` среди списка доступных команд, и вы можете вызвать справку, чтобы узнать, как его использовать:

```sh
php cli/joomla.php onoffbydate:action --help
Usage:
  onoffbydate:action <season> <action> <module_id>

Arguments:
  season                       winter or summer or weekday or weekend
  action                       publish or unpublish
  module_id                    module id

Options:
  -h, --help                   Display the help information
  -q, --quiet                  Flag indicating that all output should be silenced
  -V, --version                Displays the application version
      --ansi                   Force ANSI output
      --no-ansi                Disable ANSI output
  -n, --no-interaction         Flag to disable interacting with the user
      --live-site[=LIVE-SITE]  The URL to your site, e.g. https://www.example.com
  -v|vv|vvv, --verbose         Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Help:
  onoffbydate:action Toggles module Enabled/Disabled state
  Usage: php joomla.php onoffbydate:action season action module_id where
      season is one of winter or summer or weekday or weekend,
      action is one of publish or unpublish and
      module_id is the id of the module to publish or unpublish.
```

А затем просто попробуйте:

```sh
php cli/joomla.php onoffbydate:action weekend publish 133

     [OK] That seemed to work. Today is not a weekend. Module 133 has been Unpublished
```

### В использовании

Логика использования немного нелогична! Если у вас есть модуль, который должен публиковаться только в выходные, то параметры для его использования каждый день — `weekend publish module_id`. Это публикует модуль в выходные дни и снимает с публикации в дни, которые не являются выходными. Для модуля, который нужно публиковать в будние дни, параметры для использования каждый день — `weekend unpublish module_id`. Это снимает модуль с публикации в выходные и публикует в дни, которые не являются выходными. Та же логика применяется к версии `winter`.

## Планировщик cron

Команду можно протестировать в окне терминала, но, вероятно, вы захотите использовать её через cron. Опция `winter` может запускаться в первый день каждого месяца. Опция `weekend` будет запускаться ежедневно. Важный момент заключается в том, что вы можете иметь столько cron-задач, сколько необходимо, чтобы изменять состояние публикации стольких модулей, сколько вы хотите, с любыми подходящими интервалами. Один и тот же код работает для всех.

На хостинговой службе вам нужно указать полные пути к исполняемому файлу php и команде joomla cli. Пример:

```sh
/usr/local/bin/php /home/username/public_html/pathtojoomla/cli/joomla.php onoffbydate:action winter publish 130
```

В зависимости от того, как вы настроили ваш cron и вашу систему, вы можете получить ободряющее электронное письмо, содержащее точно такую же информацию, которую вы видите в командной строке.

## Проверка

И, конечно: перейдите на свою домашнюю страницу и проверьте, действительно ли модуль был опубликован или снят с публикации.

*Переведено openai.com*

