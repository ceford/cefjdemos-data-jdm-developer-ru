<!-- Filename: J4.x:J4_Plugin_example_-_Table_of_Contents / Display title: Пример: Оглавление -->

## Введение

Эта статья предоставляет пример плагина контента для иллюстрации кодирования с использованием последних соглашений кодирования Joomla. Полный код доступен на [GitHub](https://github.com/ceford/cefjdemos-plg-content-toc). Вы можете скачать и установить его или просто распаковать, чтобы изучить код.

В Joomla Programmers Documentation есть раздел по [разработке плагинов](https://manual.joomla.org/docs/building-extensions/plugins/) и больше примеров, [скопированных на этот сайт](jdocmanual?article=docus/plugins/index) для удобства.

Этот плагин генерирует *Оглавление* из заголовков в любой статье, содержащей плейсхолдер `{cefjdemostoc}`. Результат иллюстрируется на последнем скриншоте в этой статье.

## Структура исходного кода

Ниже приведена схема структуры исходного кода, используемого для создания расширения:

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- plg_content_cefjdemostoc (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_content_cefjdemostoc.ini
        |-- plg_content_cefjdemostoc.sys.ini
    |-- media
        |-- css
            |-- cefjdemostoc.css (layout styles)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Cefjdemostoc.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- cefjdemostoc.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

Это структура, как она выглядит в среде разработки VSCode или VSCodium:

![Plugin development file structure in vscodium](../../../en/images/plugins/cefjdemostoc-vscodium.png)

## Файл манифеста

Каждое расширение должно иметь манифест-файл в формате xml. Он используется Joomla для целей установки, обновления и удаления. Это манифест-файл для этого расширения:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="plugin" group="content" method="upgrade">
    <name>plg_content_cefjdemostoc</name>
    <author>Clifford E Ford</author>
    <creationDate>2024-12-23</creationDate>
    <copyright>(C) 2024 Clifford E Ford</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>cliff@xxxx.yyyy.co.uk</authorEmail>
    <authorUrl>jdocmanual.org</authorUrl>
    <version>0.2.2</version>
    <description>PLG_CONTENT_CEFJDEMOSTOC_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Content\Cefjdemostoc</namespace>
    <files>
        <folder plugin="cefjdemostoc">services</folder>
        <folder>src</folder>
        <folder>language</folder>
    </files>
    <media destination="plg_content_cefjdemostoc" folder="media">
        <folder>css</folder>
    </media>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemostoc Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="help"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DESC"
                    default="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT"
                >
                    <option value="">PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT</option>
                </field>

                <field
                    name="toc_class"
                    type="text"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_DESC"
                />

                <field
                    name="toc_head_class"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_DESC"
                    default="center"
                >
                    <option value="text-center">text-center</option>
                    <option value="text-start">text-start</option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

## Файл services/provider.php

Цель этого файла - зарегистрировать поставщика услуг и объявить любые инструменты, которые ему нужны. Этот конкретный плагин не требует многого. Если ваш пользовательский плагин требует выполнения запросов к базе данных, укажите это здесь. Взгляните на плагин аутентификации. Им требуются инструменты для работы с базой данных и пользователями.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjdemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford <https://jdocmanual.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Factory;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc;

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
                $plugin     = new Cefjdemostoc(
                    $container->get(DispatcherInterface::class),
                    (array) PluginHelper::getPlugin('content', 'cefjdemostoc')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## Класс Расширения

Цель файла src/Extension/Cefjdemostoc.php — подготовка данных для вывода. Длина файла составляет 91 строку, поэтому он не приводится здесь. Тем не менее, некоторые его части заслуживают дальнейшего объяснения.

### Директивы Codesniffer

Строки с 16 по 18 содержат следующее:

```php
// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

Закомментированные строки предотвращают сообщения об ошибках от CodeSniffer. Они не выполняют никаких других функций.

### public function onContentPrepare()

Это событие, на которое реагирует плагин. Оно передается с *контекстом*, данными статьи и параметрами статьи. *Оглавление* должно генерироваться только в том случае, если отображаемая страница представляет собой одну статью. В противном случае, необходимо удалить заполнитель `{cefjdemostoc}`.

Чтобы сгенерировать оглавление, устанавливаются некоторые параметры статьи, и каждое заголовок в статье изменяется, чтобы добавить порядковый номер **id**, так `<h2>Introduction</h2>` становится `<h2 id="cefjemostoc0">Introduction</h2>`. Затем загружается шаблон, чтобы добавить сгенерированное оглавление.

### Файл tmpl/default.php

Цель этого файла — отобразить вывод с предоставленными данными. Файл может быть переопределен, и вы увидите его среди переопределений в **plugins/contents** для шаблона Cassiopeia.

Файл воспроизведен ниже. Обратите внимание, что некоторые элементы имеют жестко закодированные CSS-классы, а некоторые получены из параметров статьи. Классы **fs-** — это размеры шрифта Bootstrap. Заголовки хранятся в массиве `$headings2`.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

/**
 * @var Joomla\CMS\WebAsset\WebAssetManager $wa
 * @var \Joomla\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc $this
 */
$wa = $this->getApplication()->getDocument()->getWebAssetManager();
$wa->registerAndUseStyle('plg_content_cefjdemostoc', 'plg_content_cefjdemostoc/cefjdemostoc.css');

?>
<div class="card cefjdemostoc <?php echo $toc_class; ?>">
    <div class="card-header">
        <div class="<?php echo $toc_head_class; ?> fs-2">
            <?php echo Text::_('PLG_CONTENT_CEFJDEMOSTOC_CONTENTS'); ?>
        </div>
    </div>
    <div class="card-body">
        <ul class="list-group">
            <?php foreach ($headings2 as $i => $heading) : ?>
                <li class="list-group-item fs-<?php echo $heading[1] + 2; ?> ps-<?php echo $heading[1] + 2; ?>">
                    <a href="#cefjemostoc<?php echo $i; ?>" class="link-underline-light">
                        <?php echo $heading[2]; ?>
                    </a>
                </li>
            <?php endforeach; ?>
        </ul>
    </div>
</div>
```

#### Менеджер Веб-Активов

Без дополнительного оформления оглавление (ToC) будет занимать всю ширину экрана в месте расположения заполнителя `{cefjemostoc}` в исходном тексте. Это иногда встречается в технических публикациях, но может быть не тем, что вы хотите. На широких экранах часто лучше сделать оглавление плавающим и позволить тексту статьи обтекать его. Однако на узких экранах это может стать непригодным. Лучшее решение - предоставить таблицу стилей с пользовательскими медиа-запросами. На широких экранах оглавление плавает справа, но на узких экранах этого не происходит.

Код `default.php` выше показывает, как подключить собственный стиль для плагина. CSS находится в файле `media/css/cefjdemostoc.css`. Значения там используются для иллюстрации данного учебного пособия.

## Результат

![The resulting table of contents](../../../en/images/plugins/cefjdemostoc-result.png)

*Переведено openai.com*

