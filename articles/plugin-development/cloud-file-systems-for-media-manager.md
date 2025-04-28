<!-- Filename: J4.x:Cloud_File_Systems_for_Media_Manager / Display title: Облачные файловые системы для медиаменеджера -->

<span id="main-portal-heading">GSoC 2017
Облачные файловые системы для Менеджера медиафайлов
Документация</span> 
Joomla!  4.x

## Введение

**Joomla! 4.x** поставляется с облачными файловыми системами для **Медиаменеджера** по умолчанию. С предыдущим API создание пользовательских файловых систем было сложной задачей. Благодаря новому API теперь легко создать пользовательскую файловую систему. Если вы хотите использовать облачную службу с новым Медиаменеджером, рекомендуется использовать **OAuth2.0**.

Этот документ проведет вас через важные шаги по созданию вашего собственного **плагина файловой системы** для расширения **менеджера медиафайлов**. Перед тем как продолжить, пожалуйста, убедитесь, что у вас есть базовые знания о том, как разрабатывать плагины для Joomla. [Это руководство](https://docs.joomla.org/J3.x:Creating_a_Plugin_for_Joomla) должно помочь.

## Создайте свой плагин файловой системы

### Конфигурация

Прежде всего, нам нужно создать плагин **файловой системы**, чтобы расширить Менеджер Медиафайлов. Этот плагин должен содержать некоторые важные атрибуты, чтобы он мог работать с Менеджером Медиафайлов.

Убедитесь, что ваш плагин содержит `group="filesystem"`. Поэтому в вашем `[plugin-name].xml` должно быть:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension version="4.0" type="plugin" group="filesystem" method="upgrade">
    <name>plg_filesystem_myplugin</name>
    <author>Joomla! Project</author>
    <creationDate>April 2017</creationDate>
    <copyright>Copyright (C) 2005 - 2017 Open Source Matters. All rights reserved.</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>admin@joomla.org</authorEmail>
    <authorUrl>www.joomla.org</authorUrl>
    <version>__DEPLOY_VERSION__</version>
    <description>Description</description>
    <files>
        <filename plugin="myplugin">myplugin.php</filename>
        <folder>SomeFolder</folder>
    </files>

    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="display_name"
                    type="text"
                    label="YOUR_LABEL"
                    description="YOUR_DESCRIPTION"
                    default="DEFAULT_VALUE"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

Параметр **display_name** помогает Менеджеру медиафайлов отображать имя вашей **Файловой системы** как корневой узел в браузере файлов. Любой адаптер, принадлежащий вашей файловой системе, будет отображаться в качестве дочерних узлов под корнем.

Включите любые необходимые параметры, такие как `App Secret`, с подходящими полями формы.

### События плагинов

- `onFileSystemGetAdapters()`
- `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`

#### onFileSystemGetAdapters()

**Любой** плагин файловой системы должен содержать событие `onFileSystemGetAdapters()` для функционирования. Это событие должно возвращать **массив** `Joomla\Component\Media\Administrator\Adapter\AdapterInterface` при вызове. Событие вызывается, когда вы открываете менеджер медиафайлов. Каждый `AdapterInterface` будет смонтирован под корневым узлом вашей файловой системы.

#### onFileSystemOAuthCallback()

Это событие вызывается, когда вы используете **OAuthCallbackHandler** менеджера медиафайлов для авторизации и аутентификации OAuth2.0, описанных ниже в документе. Событие вызывается только для запрашиваемого плагина. Таким образом, в типичных сценариях нет необходимости проверять `$context`.

Пример использования данного события выглядит так:

```php
    public function onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)
    {
        // Your context
        $context = $event->getContext();

        // Get the input
        $data = $event->getInput();

        // Your code goes here

        // Set result to be returned
        $result = [
            "action" => "control-panel"
        ];

        // Pass back the result to event
        $event->setArgument('result', $result);
    }
```

**OAuthCallbackEvent** содержит входные данные, перенаправленные на URI Media Manager OAuthCallback. `$event->getInput()` возвращает объект Input.

`$result` определяет, как Joomla ведет себя после перенаправления на обратный вызов. Доступно несколько возможных решений:

- `action`
  - close: Закрывает окно, которое открыто с помощью JavaScript **using window.open()**
  - redirect: Перенаправляет на страницу, указанную в **redirect_uri**
  - control-panel: Перенаправление на панель управления
  - media-manager: Перенаправление в Менеджер медиафайлов
- `redirect_uri`
  - Используется с действием **redirect** (обязательно)
- `message`
  - Сообщение, которое нужно показать после действия
- `message_type`
  - warning
  - notice
  - error
  - message(или оставить пустым)

После настройки всего установите аргумент `$result` в `$event`, чтобы передать его обратно вызывающему.

Пример отображения сообщения пользователю может быть следующим:

```php
    $result = [
        "action" => "media-manager",
        "message" => "Some message",
        "message-type" => "notice"
    ];
```

Это перенаправит в Диспетчер медиафайлов и поднимет уведомление с текстом в **сообщении**.

Этот метод обычно может использоваться для получения кодов авторизации в процессе **OAuth2.0**. В случае **ошибки** Joomla! вернется к контрольной панели и отобразит сообщение об ошибке.

## Аутентификация и авторизация

Joomla! 4.x рекомендует использовать OAuth2.0 для этого процесса. Это распространенный сценарий, когда OAuth2.0 требует **адреса перенаправления** для передачи данных аутентификации в приложение. Обычно это делается с помощью обработчика обратных вызовов. Для вашего плагина нет необходимости изобретать велосипед, вы можете использовать **обработчик обратных вызовов** в **менеджере медиа** для выполнения задачи.

Все, что вам нужно сделать, это установить **URI перенаправления** на следующий в вашем OAuth2.0 Provide и реализовать `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)` в вашем плагине.

`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=[your-plugin-name]`

В этом примере:
`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=myplugin`

Теперь, когда вы выполняете перенаправление от вашего провайдера, как только оно достигает предоставленного URL, **Контроллер** для Медиа Менеджера обеспечит, что ваш плагин реализует `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)` перед передачей любых данных ему. Если нет, вы будете перенаправлены в Панель управления с сообщением об ошибке.

Если вы реализовали все входные данные, которые передаются, поставщик облачных услуг будет включен в `$event`. Вы можете получить к ним доступ, используя `$event->getInput()` и рассматривать его как обычный `input` в Joomla.

После получения `input` вы можете использовать его для получения сведений о вашем облачном сервисе, таких как **Access Token**, **Refresh Token** и т.д.

Если вы хотите различать несколько аккаунтов, вы можете использовать `Session` для этого.

**Примечание**: рекомендуется проверить наличие **CSRF-токена** перед тем, как продолжить запрос. Большинство облачных провайдеров поддерживают параметр `state`, который можно использовать для выполнения этой задачи.

### Распространенные ошибки

Требуется использовать `urlencode()` для вашего redirect uri, когда вы отправляете его облачному провайдеру. Пожалуйста, используйте это, чтобы избежать неправильного поведения вашего облака.

## Передача файлов из вашего адаптера

Чтобы обслуживать ваши мультимедийные файлы из Менеджера медиа на **Joomla! Сайт** `Joomla\Component\Media\Administrator\Adapter\AdapterInterface` поможет вам. Этот интерфейс содержит специальный метод, называемый `getUrl($path)`.

`$path : Путь относительно вашего корня`

Цель метода состоит в том, чтобы предоставить **Публичный Абсолютный URL** для файла, который вы хотите вставить на сайт. Например, предположим, что путь к вашему файлу — `/path/to/me.png` на облачном сервере. Теперь публичный URL может выглядеть как `mycloud.com/share/u/myusername/path/to/me.png`. То, как он будет предоставляться, полностью зависит от вас. Когда пользователь хочет вставить URL-адрес сгенерированного медиафайла, будет использоваться этот метод.

Более подробную информацию об `Adapter Interface` можно найти в `administrator/componenents/com_media/Adapter/AdapterInterface.php`.

*Переведено openai.com*

