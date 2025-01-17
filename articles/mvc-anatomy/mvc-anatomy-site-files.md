<!-- Filename: J4.x:MVC_Anatomy:_Site_Files / Display title: MVC Анатомия: Файлы сайта -->

## Структура файлов

В части компонента Site меньше файлов, чем в части Administrator, поэтому это, кажется, хорошее место для начала. Будут рассмотрены только те части каждого файла, которые нуждаются в объяснении. Лучше всего открыть каждый упоминаемый файл, ознакомиться с общим содержанием, а затем найти части, которые объясняются. Алфавитный порядок структуры файлов следующий:

```bash
    site
    |- forms
        |- filter_countries.xml
    |- language
        |- en-GB
           |- com_countrybase.ini
    |- src
        |- Controller
           |- DisplayController
        |- Model
           |- CountriesModel.php
        |- Service
           |- Router.php
        |- View
           |- Countries
              |- HtmlView.php
    |- tmpl
        |- countries
           |- default.php
           |- default.xml
```

## DisplayContoller.php

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */

namespace Cefjdemos\Component\Countrybase\Site\Controller;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\MVC\Controller\BaseController;
use Joomla\CMS\Router\Route;
use Joomla\CMS\Session\Session;

/**
 * Countrybase Component Controller
 *
 * @since  1.0.0
 */
class DisplayController extends BaseController
{
    /**
     * The default view.
     *
     * @var    string
     */
    protected $default_view = 'countries';
}
```

Части этого файла объяснены следующим образом:

### Уведомление об авторских правах

Каждый php файл должен начинаться с уведомления об авторских правах, как показано ниже:

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */
```

Если вы часто создаете новые файлы и копируете/вставляете этот раздел, не забудьте обновить название компонента и сообщение об авторских правах. Кроме того, анализатор кода требует пустую строку перед блоком документации.

### Пространство имен и проверка определения

После уведомления о авторских правах в каждом php файле должна быть строка, содержащая defined('_JEXEC') или die; за исключением тех случаев, когда файлы с пространством имен должны объявлять пространство имен перед любым другим php кодом, то есть перед проверкой defined. Файлы php с пространством имен — это те, которые содержат php классы компонентов в папке src или ее подпапках.

```php
namespace Cefjemos\Component\Countrybase\Site\Controller;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

Проверка `defined` предотвращает выполнение php файла путем его прямого вызова через его url. Константа `_JEXEC` определяется, когда приложение запускается через корневой или административный файл index.php. Это важная мера безопасности.

Проверка `defined` обернута в `phpcs:disable` и `phpcs:enable` для разрешения нарушения кодового стандарта PSR12.

В сочетании с пространством имен, объявленным в файле манифеста countrybase.xml, Joomla будет искать любой класс, объявленный в текущем файле в папке root/components/com_countrybase/src/Controller - в этом случае добавляя имя этого файла, DisplayController.php.

### use Операторы

Операторы use обычно следуют за определенной проверкой и часто перечисляются в алфавитном порядке. Операторы use определяют местоположения классов, используемых этим php файлом. Иногда операторы use присутствуют по ошибке, будучи объявленными, но не используемыми. Это не причиняет вреда, но должно быть исправлено. Здесь есть два неиспользуемых оператора:

```php
    use Joomla\CMS\Router\Route;
    use Joomla\CMS\Session\Session;
```

### Класс контроллера

Контроллер дисплея почти ничего не делает, так как вся работа выполняется в родительском классе. Единственное важное, что он делает, это устанавливает вид по умолчанию, в данном случае **countries**. Это приведет к тому, что компонент по умолчанию будет использовать файлы Countries/HtmlView.php и tmpl/countries/default.php для отображения данных о странах.

```php
/**
 * Countrybase Component Controller
 *
 * @since  1.0.0 Created for first release.
 */
class DisplayController extends BaseController
{
    /**
     * The default view.
     *
     * @var    string
     */
    protected $default_view = 'countries';
}
```

Блок документации (DocBlock), предшествующий оператору `class`, используется для автоматической документации с помощью таких инструментов, как *Doxygen*. Оператор `@since` является *тегом*, который указывает номер версии расширения, в котором был представлен этот элемент. За ним может следовать текстовое объяснение. Любой элемент может иметь DocBlock с любым количеством стандартных тегов. Хорошей практикой является поддержание вашей собственной документации в актуальном состоянии!

Не забудьте установить \$default_view для этого контроллера. Без него DisplayController будет использовать представление по умолчанию, определенное в файле конфигурации компонента, или, если оно не существует, имя компонента.

## src/View/Countries/HtmlView.php

Контроллер установил вид по умолчанию на **страны**, поэтому следующим шагом будет загрузка соответствующего кода HtmlView.php. Некоторые части этого файла требуют объяснения.

### Переменные класса

```php
class HtmlView extends BaseHtmlView
{
    /**
     * The model state
     *
     * @var  \Joomla\CMS\Object\CMSObject
     */
    protected $state = null;
    ...
    protected $items = null;
    ...
    protected $pagination = null;
    ...
    public $filterForm;
    ...
    public $activeFilters;
```

Переменные класса используются для хранения информации о странице, которую необходимо отобразить:

- `$state` - состояние модели, часто инициируемое вводом формы или строки запроса.
- `$items` - список данных о странах, полученных из базы данных.
- `$pagination` - объект, используемый для отображения механизма навигации по страницам, если страниц больше, чем лимит списка, обычно 20 пунктов.
- `$filterForm` - обычно не встречается на страницах сайта, но используется в com_countrybase для фильтрации по названию страны или состоянию публикации.
- `$activeFilters` - используется для отслеживания, какие фильтры находятся в использовании.

### функция отображения

Это довольно стандартно для вида сайта, за исключением частей filterForm и activeFilters:

```php
    public function display($tpl = null)
    {
        $this->state      = $this->get('State');
        $this->items      = $this->get('Items');
        $this->pagination = $this->get('Pagination');
        $this->filterForm    = $this->get('FilterForm');
        $this->activeFilters = $this->get('ActiveFilters');

        // Flag indicates to not add limitstart=0 to URL
        $this->pagination->hideEmptyLimitstart = true;

        // Check for errors.
        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        parent::display($tpl);
    }
```

Утверждения вида `$this->get('Xxxx')` заставляют Joomla искать в `CountriesModel.php` функцию с именем `getXxxx()` и возвращать любые данные, созданные этим кодом, для хранения и использования в представлении. Часто функция находится в родительском классе CountriesModel. Например, функция getItems отсутствует в CountriesModel.php, но присутствует в ListModel, который она расширяет.

В итоге, класс представления извлекает данные из модели, а затем вызывает свой родительский класс для отображения данных.

## Model/CountriesModel.php

В модели имеется небольшое количество функций, которые вы обычно должны завершить самостоятельно. Остальные функции наследуются от родительской модели ListModel.

### __construct

Обычно принято включать в конструктор любые поля фильтров, которые вы можете захотеть использовать. Без них фильтры могут казаться неэффективными. Они часто забываются, когда вы хотите добавить фильтр позже. Каждое поле указывается как имя поля с псевдонимом имени таблицы, обычно a, b, c и так далее, но это может быть любая буква или слово, которое будет совместимо с кодом в других частях.

```php
       public function __construct($config = array())
        {
            if (empty($config['filter_fields']))
            {
                $config['filter_fields'] = array(
                        'id', 'a.id',
                        'title', 'a.title',
                        'iso_2', 'a.iso_2',
                        'iso_3', 'a.iso_3',
                        'country_code', 'a.country_code',
                        'region_code', 'a.region_code',
                        'state', 'a.state',
                        'subregion_code', 'a.subregion_code',
                        'phone_prefix', 'a.phone_prefix',
                        'currency_code', 'a.currency_code',
                );
            }

            parent::__construct($config);
        }
```

### populateState

Эта функция принимает входные параметры и подготавливает их для использования в запросе к базе данных. Некоторые параметры обрабатываются в родительском элементе, поэтому здесь нет необходимости что-либо делать. Например, если поле поиска - это **title**, оно обрабатывается родителем, как и поля **state** и пагинация **start** и **limit**.

```php
       protected function populateState($ordering = 'title', $direction = 'ASC')
        {
            // List state information.
            parent::populateState($ordering, $direction);
        }
```

### getStoreId

Эта функция создает хеш для хранения запроса для использования в другом месте.

```php
       protected function getStoreId($id = '')
        {
            // Compile the store id.
            $id .= ':' . $this->getState('filter.search');
            $id .= ':' . $this->getState('filter.published');

            return parent::getStoreId($id);
        }
```

### getListQuery

Здесь создается запрос для извлечения данных из базы данных. Это требует некоторого понимания, как создаются запросы.

```php
    protected function getListQuery()
    {
        // Create a new query object.
        $db = $this->getDatabase();
        $query = $db->getQuery(true);

        // Select the required fields from the table.
        $query->select(
            $this->getState(
                'list.select',
                [
                    $db->quoteName('a.title'),
                    $db->quoteName('a.iso_2'),
                    $db->quoteName('a.iso_3'),
                    $db->quoteName('a.country_code'),
                    $db->quoteName('a.region_code'),
                    $db->quoteName('a.subregion_code'),
                    $db->quoteName('a.phone_prefix'),
                    $db->quoteName('a.currency_code'),
                    $db->quoteName('a.state'),
                    $db->quoteName('b.title') . ' AS currency_title',
                    $db->quoteName('b.symbol'),
                    $db->quoteName('b.dollar_exchange_rate'),
                ]
            )
        )
            ->from($db->quoteName('#__countrybase_countries', 'a'))
            ->leftjoin($db->quoteName('#__countrybase_currencies', 'b') . 'ON a.currency_code = b.currency_code');

        // Filter by search in title.
        $search = $this->getState('filter.search');

        if (!empty($search)) {
            $search = '%' . str_replace(' ', '%', trim($search) . '%');
            $query->where($db->quoteName('a.title') . ' LIKE :search')
            ->bind(':search', $search, ParameterType::STRING);
        }

        // Filter by published state
        $published = (string) $this->getState('filter.published');

        if ($published !== '*') {
            if (is_numeric($published)) {
                $state = (int) $published;
                $query->where($db->quoteName('a.state') . ' = :state')
                    ->bind(':state', $state, ParameterType::INTEGER);
            }
        }

        // Add the list ordering clause.
        $orderCol = $this->state->get('list.ordering', 'a.title');
        $orderDirn = $this->state->get('list.direction', 'ASC');

        $query->order($db->escape($orderCol) . ' ' . $db->escape($orderDirn));
        return $query;
    }
```

Объяснение

- **getQuery(true)** получает новый пустой объект запроса.
- **\$query-\>select()** добавляет оператор SELECT. Может быть несколько операторов select - Joomla объединяет их.
- **table aliases a and b** обратите внимание, что некоторые столбцы берутся из разных таблиц.
- **-\>from()** определяет, какая таблица является таблицей a. Это можно сделать в отдельном операторе: \$query-\>from();
- **-\>leftjoin()** определяет таблицу b и способ её соединения с таблицей a.
- **\$query-\>where()** использует любые определённые фильтры, один для **поиска** и другой для **состояния**.
- **return \$query** нет вызова родителя, всё в запросе должно быть настроено здесь.

## tmpl/countries/default.php

Это часть кода, где создается HTML-контент. Для списка стран нужна таблица с заголовочной строкой и одной строкой для данных по каждой стране. Поскольку существует 250 стран, необходим механизм пагинации, чтобы отображать подмножество стран по несколько за раз. Для этого требуется форма. И в этом случае стандартная панель **searchtools** Joomla полезна. Вот она:

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;
use Joomla\CMS\Layout\LayoutHelper;
use Joomla\CMS\Router\Route;

$listOrder = $this->escape($this->state->get('list.ordering'));
$listDirn  = $this->escape($this->state->get('list.direction'));

?>
<h1><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES'); ?></h1>

<form action="<?php echo Route::_('index.php?option=com_countrybase'); ?>"
    method="post" name="adminForm" id="adminForm">

<?php echo LayoutHelper::render('joomla.searchtools.default', array('view' => $this)); ?>

<div class="table-responsive">
    <table class="table table-striped">
    <caption><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_TABLE_CAPTION'); ?></caption>
    <thead>
    <tr>
        <th scope="col">
            <?php echo HTMLHelper::_(
                'searchtools.sort',
                'COM_COUNTRYBASE_COUNTRIES_COUNTRY',
                'a.title',
                $listDirn,
                $listOrder
            ); ?>
        </th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_ISO_2'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_ISO_3'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_TITLE'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_SYMBOL'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_CODE'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_XRATE'); ?></th>
    </tr>
    </thead>
    <tbody>
    <?php foreach ($this->items as $id => $item) : ?>
    <tr>
        <td><?php echo $item->title; ?></td>
        <td><?php echo $item->iso_2; ?></td>
        <td><?php echo $item->iso_3; ?></td>
        <td><?php echo $item->currency_title; ?></td>
        <td><?php echo $item->symbol; ?></td>
        <td><?php echo $item->currency_code; ?></td>
        <td><?php echo $item->dollar_exchange_rate; ?></td>
    </tr>
    <?php endforeach; ?>
    </tbody>
    </table>
</div>

<?php echo $this->pagination->getListFooter(); ?>

<input type="hidden" name="task" value="">
<input type="hidden" name="boxchecked" value="0">
<?php echo HTMLHelper::_('form.token'); ?>

</form>
```

Пункты, на которые стоит обратить внимание:

- **\$listOrder** и **\$listDirection** используются для упорядочивания по заголовку столбца. Только заголовок настроен для этого.
- **form action** обычно настраивается для ссылки на самого себя.
- **LayoutHelper::render('joomla.searchtools.default',...)** создает панель поиска, как на страницах списков администратора. **Требуется форма фильтра!**
- **\$this-\>pagination-\>getListFooter()** извлекает html-код для виджета пагинации.
- **task** это скрытое поле заполняется JavaScript при отправке формы.
- **boxchecked** это скрытое поле используется, когда один или несколько флажков строк выделены для пакетной операции. На самом деле здесь не нужно!
- **HTMLHelper::\_('form.token');** получает код для токена формы, используемого в качестве средства безопасности для отправки форм, связанных с передачей данных.

## tmpl/countries/default.xml

Этот файл используется для создания элемента меню. У него такое же имя, как и у php файла, в данном случае default.xml.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<metadata>
    <layout title="COM_COUNTRYBASE_VIEW_DEFAULT_MENU_LABEL"
        option="COM_COUNTRYBASE_VIEW_DEFAULT_OPTION">
        <help
            url="components/com_countrybase/help/en-GB/countrybase.html"
        />
        <message>
            <![CDATA[COM_COUNTRYBASE_VIEW_DEFAULT_MENU_DESC]]>
        </message>
    </layout>

    <!-- Add fields to the parameters object for the layout. -->
    <fields name="params">

        <!-- Options -->
        <fieldset name="options">
        </fieldset>

    </fields>
</metadata>
```

Заметки:

- **help url** указывает на файл справки в папке администратора. Это позволяет создавать собственные локальные файлы справки, вызываемые из формы редактирования меню кнопкой справки после выбора типа меню Countrybase Default View.
- **params** позволяет использовать параметры, например, показывать или нет определенную колонку в списке стран. Параметры еще не указаны.
- **key** переводы должны находиться в файле administrator/language/en-GB/countrybase.sys.ini. Ключ *COM_COUNTRYBASE_VIEW_DEFAULT_MENU_DESC*, переведенный как *Показывает страницу Стран.*, появляется в виде дескриптора в форме выбора типа элемента меню.

## forms/filter_countries.xml

Этот файл необходим для поисковой строки. Без него Joomla выдаст фатальную ошибку. Имя файла должно быть точно таким, как показано: имя представления, перед которым стоит filter\_. Содержимое простое, всего лишь определения для поискового поля и любых других фильтров, которые вы можете захотеть использовать.

```xml
<?xml version="1.0" encoding="utf-8"?>
<form>
    <fields name="filter">
        <field
            name="search"
            type="text"
            label="COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_LABEL"
            description="COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_DESC"
            hint="JSEARCH_FILTER"
        />

        <field
            name="published"
            type="status"
            label="JOPTION_SELECT_PUBLISHED"
            onchange="this.form.submit();"
            >
            <option value="">JOPTION_SELECT_PUBLISHED</option>
        </field>

    </fields>

    <fields name="list">
        <field
            name="fullordering"
            type="list"
            label="JGLOBAL_SORT_BY"
            default="a.title ASC"
            onchange="this.form.submit();"
            >
            <option value="">JGLOBAL_SORT_BY</option>
            <option value="a.title ASC">COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_ASC</option>
            <option value="a.title DESC">COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_DESC</option>
            <option value="a.iso_2 ASC">ISO2 ASC</option>
            <option value="a.iso_2 DESC">ISO2 DESC</option>
            <option value="a.iso_3 ASC">ISO3 ASC</option>
            <option value="a.iso_3 DESC">ISO3 DESC</option>
            <option value="a.currency_code ASC">COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_ASC</option>
            <option value="a.currency_code DESC">COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_DESC</option>
            <option value="a.id ASC">JGRID_HEADING_ID_ASC</option>
            <option value="a.id DESC">JGRID_HEADING_ID_DESC</option>
        </field>

        <field
            name="limit"
            type="limitbox"
            label="JGLOBAL_LIST_LIMIT"
            default="25"
            onchange="this.form.submit();"
        />
    </fields>
</form>
```

Обратите внимание, что любые строковые ключи, начинающиеся с J, определены Joomla, и вы не должны включать их в ваши языковые файлы.

## language/ru-RU/com_countrybase.ini

Joomla всегда загружает переводы ключей на английский язык перед любым другим языком. Это гарантирует, что ключи не появятся в выводе, если язык переведен не полностью. Поскольку языковые ключи представляют собой длинные слова на псевдо-английском, считается лучше иметь смесь английского и другого языка, чем смесь ключей и другого языка. Если используется другой язык, Joomla заменяет английские строки строками на этом языке.

Обратите внимание, что общепринятой практикой является перечисление строк в алфавитном порядке ключей:

```ini
    ; Joomla! Project
    ; (C) 2005 Open Source Matters, Inc. https://www.joomla.org
    ; License GNU General Public License version 2 or later; see LICENSE.txt
    ; Note : All ini files need to be saved as UTF-8

    COM_COUNTRYBASE_COUNTRIES_COUNTRY="Country"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_CODE="Code"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_SYMBOL="Symbol"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_TITLE="Currency"
    COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_ASC="Country ASC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_DESC="Country DESC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_ASC="Currency code ASC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_DESC="Currency code DESC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_DESC="Search in Country Name"
    COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_LABEL="Search"
    COM_COUNTRYBASE_COUNTRIES_ISO_2="ISO2"
    COM_COUNTRYBASE_COUNTRIES_ISO_3="ISO3"
    COM_COUNTRYBASE_COUNTRIES_TABLE_CAPTION="Table of Country Currencies"
    COM_COUNTRYBASE_COUNTRIES_XRATE="Exchange Rate"
    COM_COUNTRYBASE_COUNTRIES="Countries"
```

## src/Service/Router.php

Маршрутизатор необходим для SEO URL-адресов. Без него ссылка в меню может выглядеть как option=com_countrybase&view=countries. С ним ссылка будет отображаться как country-base.html или любое другое имя, которое вы выберете для псевдонима заголовка ссылки.

```php
    public function __construct(
        SiteApplication $app,
        AbstractMenu $menu
    ) {

        $countries = new RouterViewConfiguration('countries');
        $countries->setKey('id');
        $this->registerView($countries);

        parent::__construct($app, $menu);

        $this->attachRule(new MenuRules($this));
        $this->attachRule(new StandardRules($this));
        $this->attachRule(new NomenuRules($this));
    }
```

Если есть больше представлений, например, таблица валют, вы должны определить каждое представление здесь перед оператором parent::__construct().

*Переведено openai.com*

