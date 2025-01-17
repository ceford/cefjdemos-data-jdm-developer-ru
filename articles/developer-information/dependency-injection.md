<!-- Filename: J4.x:Dependency_Injection_in_Joomla_4 / Display title: Внедрение зависимостей -->

## Введение

Инъекция зависимостей (Dependency injection, DI) — это метод разработки программного обеспечения, при котором класс получает свои зависимости извне, вместо того чтобы создавать их внутренне. Это улучшает гибкость, сопровождаемость и тестируемость кода. Однако, он сложен для понимания!

В документации [Joomla Framework DI](https://github.com/joomla-framework/di/blob/4.x-dev/docs/why-dependency-injection.md) есть объяснение DI и контейнеров внедрения зависимостей. Также есть [Обзор](https://github.com/joomla-framework/di/blob/4.x-dev/docs/overview.md) его использования.

Объяснения в разделе [Dependency Injection](jdocmanual?article=docus/dependency-injection/index) документации для программистов Joomla! не так легко понять!

## Примеры

Если вы хотите начать создавать расширение, следующие примеры, взятые из основных расширений Joomla, могут помочь.

Код для внедрения зависимостей находится в services/provider.php, но точное расположение этого файла зависит от типа расширения.

### Пример компонента

Полный путь к файлу services/provider.php: administrator/components/com_example/services/provider.php. Этот пример — com_tags, который не использует категории. Если ваш компонент использует категории, посмотрите код services/provider.php для com_content. Вам потребуется несколько операторов, связанных с категориями.

```php
<?php

/**
 * @package     Joomla.Administrator
 * @subpackage  com_tags
 *
 * @copyright   (C) 2018 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Component\Router\RouterFactoryInterface;
use Joomla\CMS\Dispatcher\ComponentDispatcherFactoryInterface;
use Joomla\CMS\Extension\ComponentInterface;
use Joomla\CMS\Extension\Service\Provider\ComponentDispatcherFactory;
use Joomla\CMS\Extension\Service\Provider\MVCFactory;
use Joomla\CMS\Extension\Service\Provider\RouterFactory;
use Joomla\CMS\MVC\Factory\MVCFactoryInterface;
use Joomla\Component\Tags\Administrator\Extension\TagsComponent;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

/**
 * The tags service provider.
 *
 * @since  4.0.0
 */
return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.0.0
     */
    public function register(Container $container)
    {
        $container->registerServiceProvider(new MVCFactory('\\Joomla\\Component\\Tags'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\Joomla\\Component\\Tags'));
        $container->registerServiceProvider(new RouterFactory('\\Joomla\\Component\\Tags'));
        $container->set(
            ComponentInterface::class,
            function (Container $container) {
                $component = new TagsComponent($container->get(ComponentDispatcherFactoryInterface::class));
                $component->setMVCFactory($container->get(MVCFactoryInterface::class));
                $component->setRouterFactory($container->get(RouterFactoryInterface::class));

                return $component;
            }
        );
    }
};
```

### Пример модуля

Это модуль версии, который показывает версию Joomla в строке заголовка страниц администратора. Код находится в administrator/modules/mod_version/services/provider.php

```php
<?php

/**
 * @package     Joomla.Administrator
 * @subpackage  mod_version
 *
 * @copyright   (C) 2024 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\Service\Provider\HelperFactory;
use Joomla\CMS\Extension\Service\Provider\Module;
use Joomla\CMS\Extension\Service\Provider\ModuleDispatcherFactory;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   5.1.0
     */
    public function register(Container $container)
    {
        $container->registerServiceProvider(new ModuleDispatcherFactory('\\Joomla\\Module\\Version'));
        $container->registerServiceProvider(new HelperFactory('\\Joomla\\Module\\Version\\Administrator\\Helper'));

        $container->registerServiceProvider(new Module());
    }
};
```

### Пример плагина

Это плагин, который предоставляет контактную информацию на страницах контента. Код находится в plugins/content/contact/services/provider.php

```php
<?php

/**
 * @package     Joomla.Plugin
 * @subpackage  Content.contact
 *
 * @copyright   (C) 2022 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Factory;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\Database\DatabaseInterface;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\Event\DispatcherInterface;
use Joomla\Plugin\Content\Contact\Extension\Contact;

return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.3.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $plugin     = new Contact(
                    $container->get(DispatcherInterface::class),
                    (array) PluginHelper::getPlugin('content', 'contact')
                );
                $plugin->setApplication(Factory::getApplication());
                $plugin->setDatabase($container->get(DatabaseInterface::class));

                return $plugin;
            }
        );
    }
};
```

*Переведено openai.com*

