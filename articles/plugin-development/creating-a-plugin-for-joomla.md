<!-- Filename: J4.x:Creating_a_Plugin_for_Joomla / Display title: Создание Плагина для Joomla -->

Руководство
Как создать Плагин для Joomla 4 Joomla!  4.x series

Структура плагина в Joomla! 1.5, 2.5 и 3.x была очень гибкой и мощной.
Плагины могут быть использованы не только для обработки вызовов событий
приложения и расширений ядра, но и для того, чтобы сделать сторонние
расширения более мощными. В Joomla 4.x система диспетчера была
переписана практически полностью. Это ещё больше увеличило гибкость,
когда вы изменяете параметры, переданные как события, а также увеличило
производительность плагинов.

В этом руководстве описаны основы разработки своего плагина. Большинство
плагинов состоит из одного файла с кодом, но для правильной установки он
должен быть упакован в установочный файл, который будет обработан
установщиком Joomla.

### Создание установочного файла

Так же как и все расширения Joomla, плагины могут быть легко установлены
как .zip (поддерживается и .tar.gz), но при этом в архив должен быть
включён корректно отформатированный XML файл.
В качестве примера приведём установочный XML файл для поискового плагина
категорий:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="plugin" group="search" method="upgrade">
	<name>plg_search_categories</name>
	<author>Joomla! Project</author>
	<creationDate>November 2005</creationDate>
	<copyright>Copyright (C) 2005 - 2018 Open Source Matters. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<authorEmail>admin@joomla.org</authorEmail>
	<authorUrl>www.joomla.org</authorUrl>
	<version>3.0.0</version>
	<description>PLG_SEARCH_CATEGORIES_XML_DESCRIPTION</description>
	<files>
		<filename plugin="categories">categories.php</filename>
	</files>
	<languages>
		<language tag="en-GB">en-GB.plg_search_categories.ini</language>
		<language tag="en-GB">en-GB.plg_search_categories.sys.ini</language>
	</languages>
	<config>
		<fields name="params">

			<fieldset name="basic">
				<field
					name="search_limit"
					type="number"
					label="JFIELD_PLG_SEARCH_SEARCHLIMIT_LABEL"
					default="50"
				/>

				<field
					name="search_content"
					type="radio"
					label="JFIELD_PLG_SEARCH_ALL_LABEL"
					layout="joomla.form.field.radio.switcher"
					default="0"
					>
					<option value="1">JYES</option>
					<option value="0">JNO</option>
				</field>

				<field
					name="search_archived"
					type="radio"
					label="JFIELD_PLG_SEARCH_ARCHIVED_LABEL"
					layout="joomla.form.field.radio.switcher"
					default="0"
					>
					<option value="1">JYES</option>
					<option value="0">JNO</option>
				</field>
			</fieldset>

		</fields>
	</config>
</extension>
```

Как вы можете видеть, система схожа с другими установочными XML файлами
Joomla. Вы только должны обратить внимание на запись `group="xxx"`, тег
и расширенную информацию в теге . Эта информация говорит Joomla, в какую
папку скопировать файл и в какую группу должен быть добавлен плагин.

Если вы создаете плагин, который реагирует на существующие события ядра,
атрибут `group="xxx"` должен быть изменен в соответствии с именем уже
существующей папки для события, которое вы хотите использовать.
Например, `group="authentication"` или `group="user"`. Смотрите
[Plugin/Events](https://docs.joomla.org/Plugin/Events "Special:MyLanguage/Plugin/Events")
полный список существующих категорий событий. При создании нового
плагина для событий ядра важно, чтобы его название было уникальным, и
чтобы он не конфликтовал с уже существующими плагинами, которые могут
реагировать на то же событие, какое используете вы.

Если вы создаёте плагин для событий не ядра, то тег `group="xxx"` должен
отличаться от существующих категорий ядра.

**Совет:** Если вы добавляете атрибут `method="upgrade" ` к тэгу
`extension`, то плагин можно будет устанавливать без удаления предыдущей
версии. Все существующие файлы будут перезаписаны, но оставшиеся файлы
удалены не будут.

### Создание плагина

In Joomla 4 you can also have namespaced code in your plugin - please
read [Namespace Conventions in Joomla
4](https://docs.joomla.org/J4.x:Namespace_Conventions_In_Joomla "J4.x:Namespace Conventions In Joomla").
Note the main entry file is not namespaced but fields and any other
supporting code is.

Объектно-ориентированный подход к написанию плагинов включает в себя
создание подклассов от <a
href="https://api.joomla.org/cms-4/classes/Joomla.CMS.Plugin.CMSPlugin.html"
class="external text" target="_blank"
rel="noreferrer noopener">CMSPlugin</a>, базового класса, реализующего
основные свойства плагинов. Для ваших методов доступны следующие
свойства:

- `$this->params`:
  [параметры](https://docs.joomla.org/Parameter "Special:MyLanguage/Parameter"),
  устанавливаемые администратором для этого плагина
- `$this->_name`: название плагина
- `$this->_type`: группа (тип) плагина
- `$this->db`: объект базы данных (db)
- `$this->app`: объект приложения (application)

**Совет:** Для использования `$this->db` и `$this->app`, `CMSPlugin`
проверяет, существует ли свойство и что оно не приватное. Если вы хотите
использовать дефолтные объекты, создайте неинстанцированные свойства
класса плагина (например `protected $db; protected $app;`, там же где
расположено `protected $autoloadLanguage = true;`). Свойства не будут
существовать, пока не будут явно созданы.

In the following code example, represents the group (type) of the
plugin, and represents its name. Note that class and function names in
PHP are case-insensitive.

Здесь мы также реализуем <a
href="https://api.joomla.org/framework-2/classes/Joomla.Event.SubscriberInterface.html"
class="external text" target="_blank"
rel="noreferrer noopener">SubscriberInterface</a>, что является самым
большим изменением по сравнению с Joomla 1.5-3.x. Вместо привычного
подхода, когда имя функции автоматически определялось и было таким же,
как имя события, этот подход позволяет задавать свои имена функции. Это
позволяет нам показать, какие плагины реализуют какие функции, а так как
парсинг публичных методов в PHP довольно медленный, это даёт
значительное повышение производительности.

Здесь стоит заметить, что в линейке Joomla 4 существует слой
совместимости, позволяющий работать плагинам, которые используют старую
стратегию именования и не реализуют SubscriberInterface.

```php
<?php
// no direct access
defined( '_JEXEC' ) or die;

use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\Event;
use Joomla\Event\SubscriberInterface;

class Plg<PluginGroup><PluginName> extends CMSPlugin implements SubscriberInterface
{
	/**
	 * Load the language file on instantiation
	 *
	 * @var    boolean
	 * @since  3.1
	 */
	protected $autoloadLanguage = true;

	/**
	 * Returns an array of events this subscriber will listen to.
	 *
	 * @return  array
	 */
	public static function getSubscribedEvents(): array
	{
		return [
			'<EventName>' => 'myFunctionName',
		];
	}

	/**
	 * Plugin method is the array value in the getSubscribedEvents method
	 * The plugin then modifies the Event object (if it's not immutable)
	 */
	 public function myFunctionName(Event $event)
	 {
		/*
		 * Plugin code goes here.
		 * You can access parameters via $this->params
		 */
		return true;
	}
}
?>
```

### Использование плагинов в вашем коде

Если вы создаете плагин для нового события, а не для встроенного события
ядра, то не забудьте активировать плагин после установки. Перед ссылкой
на ваш новый плагин должна стоять команда
`JPluginHelper::importPlugin()`.

Теперь, когда вы создали свой плагин, то вам наверняка захочется вызвать
его в своем коде. Но не всегда: ядро Joomla имеет набор встроенных
событий, которые вы можете использовать для регистрации плагина (тогда
нижеописанное делать не нужно).

#### Новый подход в Joomla 4

Новый подход в Joomla 4 - получить диспетчер и отправить именованное
событие.

```php
    use Joomla\CMS\Event\AbstractEvent;
    use Joomla\CMS\Factory;

    $dispatcher = Factory::getApplication()->getDispatcher();

    // Here we create an event however as long as you implement EventInterface you can create your own
    // custom classes
    $event = AbstractEvent::create(
        '',
        [
            'name' => $value,
        ]
    );

    $eventResult = $dispatcher->dispatch('', $event);
```

Если вы хотите позволить пользователю изменять значения, вы можете
использовать результат события и getResults для его получения. Взгляните
на

```php
    defined('_JEXEC') or die;

    use BadMethodCallException;
    use Joomla\CMS\Event\AbstractImmutableEvent;
    use Joomla\CMS\Table\TableInterface;

    /**
     * Event class for an event
     */
    class MyCustomEvent extends AbstractImmutableEvent
    {
        /**
         * Constructor.
         *
         * @param   string  $name       The event name.
         * @param   array   $arguments  The event arguments.
         *
         * @throws  BadMethodCallException
         */
        public function __construct($name, array $arguments = array())
        {
            if (!array_key_exists('myProperty', $arguments))
            {
                throw new BadMethodCallException("Argument 'myProperty' is required for event $name");
            }

            parent::__construct($name, $arguments);
        }

        /**
         * Setter for the myProperty argument
         *
         * @param   mixed  $value  The value to set
         *
         * @return  mixed
         *
         * @throws  BadMethodCallException  if the argument is not of the expected type
         */
        protected function setMyProperty($value)
        {
            if (!empty($value) && !is_object($value) && !is_array($value))
            {
                throw new BadMethodCallException("Argument 'src' of event {$this->name} must be empty, object or array");
            }

            return $value;
        }
    }
```

Почему мы ввели класс вместо параметров? Это даёт возможность вводить
собственные сеттеры и геттеры для свойств - на данный момент плагин
может полностью изменить свойство по своему усмотрению, и для
компонентов нет способа наложить какие-либо ограничения. Дополнительно
разработчикам проще добавлять и убирать параметры в событии без потери
обратной совместимости (так как вы теперь вызываете определённые методы
и не привязаны к свойству, являющимся вторым аргументом вашей функкции).

#### Как достигнуть максимальной совместимости с Joomla 3

Если вы хотите вызвать событие по аналогии с удалённым J3.x
JEventDispatcher, то ваш код должен выглядеть вот так:

```php
    $results = \Joomla\CMS\Factory::getApplication()->triggerEvent( '',  );
```

Важно отметить, что параметры должны быть массивом. Функция плагина сама
по себе получает параметры как объект Event, если он реализует
SubscriberInterface, и как индивидуальные значения, если нет. Но этот
метод всегда возвращает массив, который возвращает плагин.

Обратите внимание на то, что если хотя бы один плагин в группе не
реализует SubscriberInterface, то тогда свойство результата (так как
одновременно является именованным параметром и результатом из плагина)
используется как специальное свойство и не может быть использовано.
