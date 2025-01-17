<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Startup_Files / Display title: MVC Анатомия: Файлы запуска администратора -->

## Обзор файлов администратора

В административной части компонента больше файлов, отчасти потому что есть две таблицы, каждая с представлением списка и редактирования, а отчасти потому что некоторые файлы управляют аспектами поведения компонента как в интерфейсе сайта, так и в интерфейсе администратора.

Создание кода для представления списка было рассмотрено в части руководства, посвященной файлам сайта, поэтому здесь повторяться не будет. Этот раздел будет посвящен файлам, общим для большинства компонентов. В последующем руководстве будет рассмотрена одна из представлений редактирования, необходимых для обновления или добавления новых записей.

## услуги/provider.php

Этот файл используется, когда приложение Joomla вызывает ComponentHelper для отображения компонента. Это самый первый код компонента, выполняемый как для сайта, так и для администратора. Его роль заключается в регистрации компонента:

```php
    public function register(Container $container)
    {
        $container->registerServiceProvider(new MVCFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new RouterFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->set(
            ComponentInterface::class,
            function (Container $container) {
                $component = new CountrybaseComponent($container->get(ComponentDispatcherFactoryInterface::class));
                $component->setMVCFactory($container->get(MVCFactoryInterface::class));
                $component->setRouterFactory($container->get(RouterFactoryInterface::class));

                return $component;
            }
        );
    }
```

## src/Extension/CountrybaseComponent.php

Функция регистрации этого файла вызывается из \$component = new CountrybaseComponent(...); оператора в services/provider.php. Ее цель - запустить компонент как в интерфейсе сайта, так и в интерфейсе администратора.

```php
       public function boot(ContainerInterface $container)
        {
        }
```

## access.xml

Этот файл устанавливает контроль доступа, используемый в этом компоненте. Перечисленные здесь элементы отображаются на вкладке "Параметры разрешений" компонента.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<access component="com_countrybase">
    <section name="component">
        <action name="core.admin" title="JACTION_ADMIN" />
        <action name="core.options" title="JACTION_OPTIONS" />
        <action name="core.manage" title="JACTION_MANAGE" />
        <action name="core.create" title="JACTION_CREATE" />
        <action name="core.delete" title="JACTION_DELETE" />
        <action name="core.edit" title="JACTION_EDIT" />
    </section>
</access>
```

## config.xml

Этот файл используется для управления появлением любых опций в форме компонента *Options* и под каким вкладкой они появляются. Для com_countrybase нет других опций, кроме стандартных опций разрешений Joomla. Здесь можно добавить опции, например, чтобы показать или скрыть определенный столбец в выводе. Это будет располагаться в отдельном наборе полей с именем Options.

```xml
<?xml version="1.0" encoding="utf-8"?>
<config>

    <fieldset
        name="permissions"
        label="JCONFIG_PERMISSIONS_LABEL"
        description="JCONFIG_PERMISSIONS_DESC"
        >

        <field
            name="rules"
            type="rules"
            label="JCONFIG_PERMISSIONS_LABEL"
            filter="rules"
            validate="rules"
            component="com_countrybase"
            section="component"
         />
    </fieldset>
</config>
```

*Переведено openai.com*

