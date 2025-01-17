<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Edit_Files / Display title: MVC Анатомия: Редактирование файлов администратора -->

## Файлы данных по странам

Файлы администратора включают четыре представления: два представления списка для данных о странах и данных о валютах, а также два представления редактирования для управления вводом данных в каждую из двух задействованных таблиц. Представления списка почти идентичны описанному представлению списка стран для интерфейса сайта, поэтому здесь по новой рассматриваться не будут. Подобным образом представления редактирования практически идентичны, поэтому будет рассмотрено только одно - представление ввода данных для одной страны.

## src/Controller/CountryController.php

Это чрезвычайно просто! Помимо объявления класса, он не содержит содержания. Всем управляет родительский класс.

```php
    class CountryController extends FormController
    {
    }
```

## src/View/Country/HtmlView.php

Здесь данные извлекаются из модели для использования в форме ввода данных. Существует значительная разница по сравнению с ранее описанным видом списка: обычно используется панель инструментов с кнопками для Сохранения, Отмены, Помощи и так далее.

Когда вы вызываете форму редактирования, выбирая заголовок элемента списка, URL содержит идентификатор, например, option=com_countrybase&view=country&layout=edit&id=1. Идентификатор — это номер записи в таблице. Для новой записи, вызываемой через кнопку "Новая запись" в списке, идентификатор отсутствует. Если он присутствует, модель заполняет форму ввода данных существующими данными записи.

```php
    public function display($tpl = null)
    {
        $this->form  = $this->get('Form');
        $this->item  = $this->get('Item');
        $this->state = $this->get('State');

        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        $this->addToolbar();

        return parent::display($tpl);
    }
```

### Панель инструментов

Функция addToolbar() обычно используется для установки заголовка страницы и добавления соответствующих кнопок в зависимости от прав доступа пользователя, использующего форму. Обратите внимание на использование переменной \$isNew на основе идентификатора записи для небольшого изменения вывода.

```php
    protected function addToolbar()
    {
        Factory::getApplication()->input->set('hidemainmenu', true);
        $isNew      = ($this->item->id == 0);

        $canDo = ContentHelper::getActions('com_countrybase');

        $toolbar = Toolbar::getInstance();

        ToolbarHelper::title(
            Text::_('COM_COUNTRYBASE_COUNTRY_PAGE_TITLE_' . ($isNew ? 'ADD' : 'EDIT'))
        );

        if ($canDo->get('core.create')) {
            if ($isNew) {
                $toolbar->apply('country.save');
            } else {
                $toolbar->apply('country.apply');
            }
            $toolbar->save('country.save');
        }
        $toolbar->cancel('country.cancel', 'JTOOLBAR_CLOSE');

        ToolbarHelper::inlinehelp();
    }
```

**Заметки:**

- **hidemainmenu** установлено в true для всех форм редактирования, чтобы предотвратить выход из формы через меню администратора без сохранения.
- **ToolbarHelper::title()** устанавливает заголовок в строке заголовка страницы, в этом случае это Редактировать страну или Добавить страну.
- **toolbar->action** кнопки, где action может быть save, apply, cancel или одним из многих других, принимают (controller.function) в качестве аргументов. Таким образом, в этом случае, когда кнопка выбрана, действие передается для функции сохранения или применения в классе CountryController.
- **cancel** принимает дополнительный аргумент, строковый ключ, используемый для обозначения кнопки.
- **inlinehelp()** показывает кнопку, которая включает или выключает описания полей под каждым полем ввода данных.

## формы/country.xml

Этот файл содержит спецификацию полей формы, обычно в порядке, представленном в форме редактирования. Большинство полей не требуют пояснений, поэтому ниже показаны только два. Значения имени являются названиями столбцов, используемых в таблицах базы данных.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<form>
        <field
            name="title"
            type="text"
            label="COM_COUNTRYBASE_COUNTRY_TITLE"
            required="true"
            maxlength="128"
        />

        <field
            name="iso_2"
            type="text"
            label="COM_COUNTRYBASE_COUNTRY_ISO_2"
            description="COM_COUNTRYBASE_COUNTRY_ISO_2_DESC"
            required="true"
            maxlength="3"
        />
```

## src/Model/CountryModel.php

Функции в этом файле являются довольно стандартными. Однако функция getTable() требует некоторого объяснения.

```php
    public function getTable($name = '', $prefix = '', $options = array())
    {
        $name = 'Countries';
        $prefix = 'Table';

        if ($table = $this->_createTable($name, $prefix, $options)) {
            return $table;
        }

        throw new \Exception(Text::sprintf('JLIB_APPLICATION_ERROR_TABLE_NAME_NOT_SUPPORTED', $name), 0);
    }
```

Эта функция не ссылается на саму таблицу! Она ссылается на конструктор из src/Table/CountriesTable.php. Это может быть немного запутанно, потому что вызывается из класса CountryModel.

## src/Table/CountriesTable.php

Итак, здесь определяется название таблицы для списка стран.

```php
    class CountriesTable extends Table
    {
        /**
         * Constructor
         *
         * @param   DatabaseDriver  $db  Database connector object
         *
         * @since   1.6
         */
        public function __construct(DatabaseDriver $db)
        {
            parent::__construct('#__countrybase_countries', 'id', $db);

            $this->setColumnAlias('published', 'state');
        }
    }
```

Обратите внимание на объявление псевдонима столбца. Это используется потому, что в форме используется поле ввода данных с именем **published**, но поле, используемое для хранения, называется **status**.

## tmpl/country/edit.php

Это файл, который создает HTML-вывод. Он начинается с двух вспомогательных функций: HTMLHelper::_('behavior.formvalidator'); загружает JavaScript, необходимый для клиентской валидации формы. Хотя современные браузеры применяют валидацию форм, это не предотвращает отправку формы. HTMLHelper::_('behavior.keepalive'); загружает JavaScript для пинга сервера, чтобы предотвратить истечение сессии при заполнении сложных форм.

**Заметки:**

- LayoutHelper::render('joomla.edit.title_alias', \$this); отображает стандартное поле заголовка Joomla.
- \$this-\>form-\>renderField('iso_2'); отображает указанное поле, в данном случае iso_2.
- LayoutHelper::render('joomla.edit.global', \$this); отображает поля на правой стороне вкладки "Детали".

```php
<?php

/**
 * @package     Countrybase.Administrator
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

HTMLHelper::_('behavior.formvalidator');
HTMLHelper::_('behavior.keepalive');

?>

<form action="<?php echo Route::_('index.php?option=com_countrybase&view=country&layout=edit&id=' .
        (int) $this->item->id); ?>"
    method="post" name="adminForm" id="country-form" class="form-validate">

    <?php echo LayoutHelper::render('joomla.edit.title_alias', $this); ?>

    <div>
        <?php echo HTMLHelper::_('uitab.startTabSet', 'myTab', array('active' => 'details')); ?>

        <?php echo HTMLHelper::_('uitab.addTab', 'myTab', 'details', Text::_('COM_COUNTRYBASE_COUNTRY_TAB_DETAILS')); ?>
        <div class="row">
            <div class="col-md-9">
                <div class="row">
                    <div class="col-md-6">
                        <?php echo $this->form->renderField('iso_2'); ?>
                        <?php echo $this->form->renderField('iso_3'); ?>
                        <?php echo $this->form->renderField('country_code'); ?>
                        <?php echo $this->form->renderField('region_code'); ?>
                        <?php echo $this->form->renderField('subregion_code'); ?>
                        <?php echo $this->form->renderField('phone_prefix'); ?>
                        <?php echo $this->form->renderField('currency_code'); ?>
                        <?php echo $this->form->renderField('id'); ?>
                    </div>
                </div>
            </div>
            <div class="col-md-3">
                <div class="card card-light">
                    <div class="card-body">
                        <?php echo LayoutHelper::render('joomla.edit.global', $this); ?>
                    </div>
                </div>
            </div>
        </div>
        <?php echo HTMLHelper::_('uitab.endTab'); ?>

        <?php echo HTMLHelper::_('uitab.endTabSet'); ?>
    </div>
    <input type="hidden" name="task" value="">
    <?php echo HTMLHelper::_('form.token'); ?>
</form>
```

Вы можете переключаться между наборами полей формы и полями внутри каждого набора. Это может упростить вывод сложных форм с многими вкладками.

![country edit form](../../../en/images/mvc-anatomy/com-countrybase-edit-country.png)

*Переведено openai.com*

