<!-- Filename: J4.x:Using_Bootstrap_Components_in_Joomla_4 / Display title: Использование компонентов Bootstrap в Joomla 4 -->

## Введение

### Компоненты Bootstrap

Некоторые из компонентов, описанных в документации Bootstrap, используют только CSS. Например, компонент "Хлебные крошки" отображается с помощью CSS и не требует поддержки JavaScript. Другие компоненты реагируют на действия пользователя, такие как клик или наведение, и нуждаются в поддержке JavaScript. Последние здесь называются "Интерактивные компоненты". В этой статье объясняется, как использовать их в статьях и в пользовательском модуле.

См. [Документацию Bootstrap](https://getbootstrap.com/docs/5.0/getting-started/introduction/)

### Joomla 4 представляет модульный подход для интерактивных компонентов

- Что такое модульный подход?
- Функциональность разбита на отдельные компоненты, поддерживаемые отдельными файлами. Нет подхода с **одним файлом**, как это было с Bootstrap в Joomla 3. Модульный подход более эффективен и обеспечивает повышение производительности (отправлять только тот код, который нужен, вместо передачи всего на случай, если какая-то страница будет нуждаться в каком-то компоненте).

### Использование интерактивных компонентов: Программисты

- Загружайте, что нужно для каждого случая! Есть вспомогательные функции для настройки отдельных компонентов с подходящими аргументами.
- См. список интерактивных компонентов Bootstrap.

### Использование интерактивных компонентов: для тех, кто не является программистом

- Использование компонентов в статьях не так просто, так как вспомогательные функции нельзя вызвать из статьи или стандартного модуля Custom HTML. Три возможных решения предложены далее в этом руководстве:
  - Пользовательский модуль
  - Пользовательский плагин
  - Переопределение пользовательского модуля
- Пропустите или просмотрите список интерактивных компонентов Bootstrap. Вы не будете использовать эти вызовы функций напрямую.

## Интерактивные компоненты Bootstrap

### Оповещение

Предполагая, что у вас уже есть часть HTML в вашем макете, вам также понадобится включить интерактивность (часть JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.alert', '.selector');
```

- **.selector** относится к CSS селектору для оповещения. Вы можете вызывать эту функцию несколько раз с разными CSS селекторами.
- Дополнительные параметры недоступны

### Кнопка

Предположим, что у вас уже есть HTML-часть в вашем макете, вам также нужно будет добавить интерактивность (часть JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.button', '.selector');
```

- **.selector** относится к CSS-селектору для кнопки. Вы можете вызвать эту функцию несколько раз с разными CSS-селекторами.
- Дополнительные параметры отсутствуют.

### Карусель

Предполагая, что у вас уже есть HTML часть в вашем макете, вам также нужно включить интерактивность (часть с JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.carousel', '.selector', []);
```

- **.selector** относится к CSS-селектору для карусели. Вы можете вызвать эту функцию несколько раз с различными CSS-селекторами
- Третий аргумент относится к параметрам, доступным для карусели

Опции для карусели могут быть:

- **interval**, число, по умолчанию:**5000**, Время задержки между автоматическим переключением элементов. Если установить значение false, карусель не будет автоматически переключаться.
- **keyboard**, логическое значение, по умолчанию:**true** Должна ли карусель реагировать на события клавиатуры.
- **pause**, строка\|логическое значение, **hover** Приостанавливает циклирование карусели при наведении курсора и возобновляет циклирование карусели при уходе курсора.
- **slide**, строка\|логическое значение, по умолчанию:**false** Автоматически запускает карусель после того, как пользователь вручную переключит первый элемент. Если указано "carousel", автоматически запускает карусель при загрузке.

### Свернуть

Предположим, что HTML часть уже добавлена в ваш макет, вам также нужно будет включить интерактивность (часть JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.collapse', '.selector', []);
```

- **.selector** относится к CSS-селектору для сворачивания. Вы можете вызывать эту функцию несколько раз с различными CSS-селекторами.
- Третий аргумент относится к доступным параметрам для сворачивания.

Варианты для сворачивания могут быть:

- **parent**, string, по умолчанию:**false** Если указан родительский элемент, то все элементы, которые могут быть свернуты под указанным родителем, будут закрыты, когда этот элемент будет показан.
- **toggle**, логическое значение по умолчанию:**true** Переключает элемент, который может быть свернут, при вызове.

### Выпадающий список

Предполагая, что у вас уже есть HTML-часть в вашем макете, вам также необходимо будет включить интерактивность (JavaScript-часть):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.dropdown', '.selector', []);
```

- **.selector** относится к CSS селектору для выпадающего списка. Вы можете вызывать эту функцию несколько раз с разными CSS селекторами.
- Третий аргумент относится к доступным опциям для выпадающего списка.

Варианты для сворачивания могут быть:

- **flip**, boolean, по умолчанию:**true** Разрешает выпадающему списку переворачиваться в случае перекрытия с опорным элементом.
- **boundary**, string, по умолчанию:**scrollParent** Граница ограничения переполнения для выпадающего меню.
- **reference**, string, по умолчанию:**toggle** Опорный элемент выпадающего меню. Принимает значения **toggle** или **parent**.
- **display**, string, по умолчанию:**dynamic** По умолчанию мы используем Popper для динамического позиционирования. Отключите это с помощью **static**.

### Модальное окно

Предполагая, что у вас уже есть HTML часть в вашем макете, вам также нужно будет включить интерактивность (часть на JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.modal', '.selector', []);
```

- **.selector** относится к CSS-селектору для модального окна. Вы можете вызвать эту функцию несколько раз с разными CSS-селекторами.
- Третий аргумент относится к доступным параметрам для модального окна.

Параметры для модального окна могут быть:

- **backdrop**, строка\|логический тип по умолчанию:**true** Включает элемент модальной подложки. В качестве альтернативы укажите static для подложки, которая не закрывает модальное окно при клике.
- **keyboard**, логический тип по умолчанию:**true** Закрывает модальное окно при нажатии клавиши escape.
- **focus**, логический тип по умолчанию:**true** Закрывает модальное окно при нажатии клавиши escape.

### Всплывающее меню

Предположим, что HTML-часть уже есть в вашем макете, вам также нужно будет включить интерактивность (часть на JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.offcanvas', '.selector', []);
```

- **.selector** относится к CSS-селектору для offcanvas. Вы можете вызвать эту функцию несколько раз с разными CSS-селекторами.
- Третий аргумент относится к доступным параметрам для offcanvas.

Варианты для offcanvas могут быть:

- **backdrop**, boolean, по умолчанию:**true** Накладывает фон на тело документа, когда offcanvas открыт.
- **keyboard**, boolean, по умолчанию:**true** Закрывает offcanvas при нажатии клавиши Escape.
- **scroll**, boolean, по умолчанию:**false** Позволяет скроллить тело документа, когда offcanvas открыт.

### Всплывающее окно

Предполагая, что у вас уже есть HTML-часть в вашем макете, вам также нужно будет добавить интерактивность (часть JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', '.selector', []);
```

- **.selector** относится к CSS селектору для всплывающей подсказки. Вы можете вызывать эту функцию несколько раз с разными CSS селекторами.
- Третий аргумент относится к доступным опциям для всплывающей подсказки.

Параметры для всплывающей подсказки могут быть:

- **animation**, boolean, по умолчанию:**true** Применить CSS-переход плавного появления к всплывающей подсказке.
- **container**, string\|boolean, по умолчанию:**false** Присоединяет всплывающую подсказку к определенному элементу. Например: **body**.
- **content**, string, по умолчанию:**null** Значение содержимого по умолчанию, если атрибут data-bs-content не присутствует.
- **delay**, number, по умолчанию:**0** Задержка показа и скрытия всплывающей подсказки (мс) не применяется к ручному типу срабатывания.
- **html**, boolean, по умолчанию:**true** Вставить HTML в всплывающую подсказку. Если **false**, будет использоваться свойство innerText для вставки содержимого в DOM.
- **placement**, string, по умолчанию:**right** Как позиционировать всплывающую подсказку - **auto** \| **top** \| **bottom** \| **left** \| **right**. Когда указано auto, оно будет динамически изменять ориентацию всплывающей подсказки.
- **selector**, string, по умолчанию:**false** Если предоставлен селектор, объекты всплывающей подсказки будут делегированы указанным целям.
- **template**, string, по умолчанию:**null** Базовый HTML для использования при создании всплывающей подсказки.
- **title**, string, по умолчанию:**null** Значение заголовка по умолчанию, если тег **title** отсутствует.
- **trigger**, string, по умолчанию:**click** Как срабатывает всплывающая подсказка - **click** \| **hover** \| **focus** \| **manual**.
- **offset**, integer, по умолчанию:**0** Смещение всплывающей подсказки относительно ее цели.

### Прокрутка-шпион

Предполагая, что у вас уже есть HTML-часть в вашем макете, вам также потребуется включить интерактивность (часть на JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.scrollspy', '.selector', []);
```

- **.selector** относится к CSS-селектору для scrollspy. Вы можете вызвать эту функцию несколько раз с разными CSS-селекторами.
- Третий аргумент относится к параметрам, доступным для scrollspy.

Параметры для Scrollspy могут быть:

- **offset** number Пиксели для смещения от верха при вычислении позиции прокрутки.
- **method** string Определяет, в каком разделе находится элемент слежения.
- **target** string Указывает элемент для применения плагина Scrollspy.

### Вкладка

Предполагая, что у вас уже есть HTML-часть в вашем макете, вам также нужно будет добавить интерактивность (JavaScript-часть):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tab', '.selector', []);
```

- **.selector** относится к CSS-селектору для вкладки. Вы можете вызвать эту функцию несколько раз с разными CSS-селекторами.
- Третий аргумент относится к доступным параметрам для вкладки.

Варианты для вкладки могут быть:

### Подсказка

Предполагая, что у вас уже есть HTML-часть в вашем макете, вам также нужно включить интерактивность (часть JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', '.selector', []);
```

- **.selector** относится к CSS селектору для всплывающей подсказки. Вы можете вызывать эту функцию несколько раз с разными CSS селекторами.
- Третий аргумент относится к доступным параметрам для всплывающей подсказки.

Параметры для всплывающей подсказки могут быть:

- **animation**, boolean применяет CSS-переход с затуханием к всплывающей подсказке.
- **container**, string\|boolean Добавляет всплывающую подсказку к определенному элементу: { container: **body** }.
- **delay**, number\|object задержка показа и скрытия всплывающей подсказки (мс) — не применяется к типу ручного срабатывания. Если указано число, задержка применяется как для скрытия, так и для показа. Структура объекта: delay: { show: 500, hide: 100 }.
- **html**, boolean Вставляет HTML во всплывающую подсказку. Если false, метод text jQuery будет использован для вставки содержимого в DOM.
- **placement**, string\|function как позиционировать всплывающую подсказку - **top** \| **bottom** \| **left** \| **right**.
- **selector**, string Если предоставлен селектор, объекты всплывающих подсказок будут делегированы указанным целям.
- **template**, string Основной HTML для использования при создании всплывающей подсказки.
- **title**, string\|function значение заголовка по умолчанию, если тег **title** отсутствует.
- **trigger**, string как активируется всплывающая подсказка - hover \| focus \| manual.
- **constraints**, array Массив ограничений - передается в Popper.
- **offset**, string Смещение всплывающей подсказки относительно её цели.

### Тост

Если вы уже добавили часть HTML в свой макет, вам также необходимо включить интерактивность (часть JavaScript):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.toast', '.selector', []);
```

- **.selector** относится к CSS-селектору для тоста. Вы можете вызывать эту функцию несколько раз с разными CSS-селекторами.
- Третий аргумент относится к доступным вариантам для тоста.

Опции для тоста могут быть:

- **animation**, boolean, по умолчанию:**true** Применить CSS-переход затухания к всплывающему уведомлению.
- **autohide**, boolean, по умолчанию:**true** Автоматически скрывать всплывающее уведомление.
- **delay**, number, по умолчанию:**5000** Задержка скрытия всплывающего уведомления (мс).

### См. также

- **Аккордеон** использует Collapse для отображения панелей данных.
- **Группа списка** может быть объединена с Вкладкой для отображения содержимого с вкладками.

## Использование компонентов Bootstrap в статьях

HTML-разметку для большинства компонентов можно включить в статью или модуль, который сам по себе может быть включён в статью. Проблема в том, что вызов HTMLHelper для настройки поддержки JavaScript не может быть включён туда. Существует несколько возможных подходов к решению этой проблемы. Здесь предлагаются три подхода: использование пользовательского модуля, использование плагина или использование переопределения шаблона.

**Внимание:** Редакторы TinyMCE и JCE удаляют пробелы при сохранении и затрудняют редактирование кода! Простое решение - перейти в правый верхний угол экрана администратора и выбрать **Меню пользователя → Редактировать аккаунт** и установить редактор Code Mirror.

## Подход 1: Использование пользовательского модуля

Это, вероятно, наименее подверженный ошибкам подход, потому что параметры поддержки компонентов Bootstrap выбираются с помощью флажков. Шаги включают следующее:

- Загрузите, установите и активируйте этот [Custom Module](https://github.com/ceford/j4xdemos-mod-custom-bscompos/raw/master/mod_custom_bscompos.zip)
- В меню Администратора перейдите в **Контент **→** Модули сайта → Новый**
- Выберите **Custom BS Components**
- Введите заголовок
- Переключите редактор в режим обычного текста и вставьте или введите HTML-код для компонента, который вы хотите использовать.
- На вкладке Опции прокрутите вниз до списка BS Компонентов и выберите тип компонента в этом экземпляре модуля. Обратите внимание, что вы можете выбрать более одной, если используете более одного компонента.
- Выберите позицию модуля: либо
  - позицию, определенную шаблоном, если хотите использовать модуль в определенном месте, либо
  - введите позицию, если хотите использовать модуль внутри определенной статьи: в статье введите {loadposition whatever}
- Сохраните и перейдите на сайт для тестирования!

### Селекторы

Для некоторых компонентов действие JavaScript запускается определённым **классом** в HTML-коде. В других компонентах действие запускается атрибутом **data-bs-whatever**. Следующие триггеры являются текущими и могут измениться:

- **Alert** активируется с помощью class="alert ..."
- **Button** активируется с помощью data-bs-toggle="button"
- **Carousel** активируется с помощью data-bs-ride="whatever"
- **Collapse** активируется с помощью data-bs-toggle="collapse"
- **Dropdown** активируется с помощью data-bs-toggle="dropdown"
- **Modal** активируется с помощью data-bs-toggle="modal"
- **Offcanvas** активируется с помощью data-bs-toggle="offcanvas"
- **Popover** активируется с помощью class="btn ..." или
  - тег (может быть изменен на class="haspopover ...") И
  - data-bs-toggle="popover"
- **Scrollspy** активируется с помощью data-bs-spy="scroll"
- **Tab** активируется с помощью data-bs-toggle="tab"
- **Toast** активируется с помощью class="toast ..."
- **Tooltip** активируется с помощью class="btn ..." или
  - тег (может быть изменен на class="hastooltip ...") И
  - data-bs-toggle="tooltip"

### Пример 1: Оповещение

Оповещения могут использоваться в HTML-коде без поддержки JavaScript. Это необходимо только для возможности закрытия. Пример HTML-кода:

```html
<div class="alert alert-warning alert-dismissible fade show" role="alert">
  <strong>Holy guacamole!</strong> You should check in on some of those fields below.
  <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
</div>
```

Пример результата включения модуля в статью:

![Bootstrap alert](../../../en/images/coding-examples/coding-examples-alert.png)

Обратите внимание, что без поддержки JavaScript оповещение будет отображаться точно так же, как выше, но нажатие на кнопку закрытия \[X\] не закроет его. Кроме того, уведомление будет появляться при каждой загрузке страницы.

### Пример 2: Кнопки

Кнопки могут использоваться в HTML-коде без поддержки JavaScript. Это необходимо только для иногда едва заметного изменения стиля, применяемого к кнопкам при изменении состояния, в стиле активный. Пример кода Bootstrap:

```html
<p><button type="button" class="btn btn-primary" data-bs-toggle="button" autocomplete="off">Toggle button</button>
<button type="button" class="btn btn-primary active" data-bs-toggle="button" autocomplete="off" aria-pressed="true">Active toggle button</button>
<button type="button" class="btn btn-primary" disabled data-bs-toggle="button" autocomplete="off">Disabled toggle button</button></p>
```

```html
<p><a href="#" class="btn btn-primary" role="button" data-bs-toggle="button">Toggle link</a>
<a href="#" class="btn btn-primary active" role="button" data-bs-toggle="button" aria-pressed="true">Active toggle link</a>
<a href="#" class="btn btn-primary disabled" tabindex="-1" aria-disabled="true" role="button" data-bs-toggle="button">Disabled toggle link</a></p>
```

С этим стилем в шаблоне user.css:

```css
    .btn.btn-primary.active {
        background-color: green;
    }
```

![Bootstrap buttons](../../../en/images/coding-examples/coding-examples-buttons.png)

Кнопки переключаются между синим и зеленым.

### Пример 3: Карусель

Карусель предлагает слайд-шоу, проходящее через серию изображений или текстовых панелей. Следующий пример использует изображения из образца Joomla 4. Код Bootstrap:

```html
<div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
    <div class="carousel-indicators">
        <button class="active" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" aria-current="true" aria-label="Slide 1"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
    </div>
    <div class="carousel-inner">
        <div class="carousel-item active"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa1-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>First slide label</h5>
                <p>Some representative placeholder content for the first slide.</p>
            </div>
        </div>
        <div class="carousel-item"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa2-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>Second slide label</h5>
                <p>Some representative placeholder content for the second slide.</p>
            </div>
        </div>
        <div class="carousel-item"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa3-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>Third slide label</h5>
                <p>Some representative placeholder content for the third slide.</p>
            </div>
        </div>
    </div>
    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
        <span class="visually-hidden">Previous</span>
    </button>
    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
        <span class="visually-hidden">Next</span>
    </button>
</div>
```

Результат:

![Bootstrap carousel](../../../en/images/coding-examples/coding-examples-carousel.jpg)

### Пример 4: Сворачивание

Кнопка свертывания широко используется в Joomla, и вам может не понадобиться использовать модуль или плагин для выполнения действия. Щелчок открывает панель с дополнительной информацией. Пример кода Bootstrap:

```html
<p>
    <a class="btn btn-primary" role="button" href="#collapseExample" data-bs-toggle="collapse" aria-expanded="false" aria-controls="collapseExample"> Link with href </a>
    <button class="btn btn-primary" type="button" data-bs-toggle="collapse" data-bs-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
        Button with data-bs-target
    </button>
</p>
<div id="collapseExample" class="collapse">
    <div class="card card-body">
        Some placeholder content for the collapse component. This panel is hidden by default but revealed when the user activates the relevant trigger.
    </div>
</div>
```

Результат:

![Bootstrap collapse](../../../en/images/coding-examples/coding-examples-collapse.png)

### Пример 5: Выпадающий список

Выпадающие списки – это переключаемые контекстные наложения для отображения списков ссылок и другого. Пример кода Bootstrap:

```html
<div class="btn-group">
    <button class="btn btn-danger dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false"> Action </button>
    <ul class="dropdown-menu">
        <li><a class="dropdown-item" href="#">Action</a></li>
        <li><a class="dropdown-item" href="#">Another action</a></li>
        <li><a class="dropdown-item" href="#">Something else here</a></li>
        <li><hr class="dropdown-divider" /></li>
        <li><a class="dropdown-item" href="#">Separated link</a></li>
    </ul>
</div>
```

Результат:

![Bootstrap dropdown](../../../en/images/coding-examples/coding-examples-dropdown.png)

### Пример 6: Модальное окно

Модальное окно открывает диалоговое окно в центре экрана. Существует множество опций для управления размером и содержимым модального окна. Подробности см. в документации Bootstrap. Пример кода Bootstrap:

```html
<p><button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button></p>
<div id="exampleModal" class="modal fade" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 id="exampleModalLabel" class="modal-title">Modal title</h5>
                <button class="btn-close" type="button" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" type="button" data-bs-dismiss="modal">Close</button>
                <button class="btn btn-primary" type="button">Save changes</button>
            </div>
        </div>
    </div>
</div>
```

Результат:

![Bootstrap modal](../../../en/images/coding-examples/coding-examples-modal.png)

### Пример 7: Offcanvas

В данный момент этот компонент не поддерживается в Joomla. Следите за обновлениями - скоро появится!

### Пример 8: Всплывающее окно

Поповеры похожи на всплывающие подсказки, но с заголовком. Они имеют некоторые проблемы с доступностью и производительностью, поэтому их следует использовать с осторожностью. Пример кода Bootstrap:

```html
<p><button class="btn btn-lg btn-danger" title="Popover title" type="button" data-bs-toggle="popover" data-bs-content="And here's some amazing content. It's very engaging. Right?">Click to toggle popover</button></p>
```

Результат:

![Bootstrap alert](../../../en/images/coding-examples/coding-examples-popover.png)

### Пример 9: Scrollspy

Пример кода:

```html
<div class="row">
    <div class="col-4">
        <nav id="navbar-example3" class="navbar navbar-light bg-light flex-column">
            <a class="navbar-brand" href="#">Navbar</a><nav class="nav nav-pills flex-column">
            <a class="nav-link active" href="#item-1">Item 1</a>
            <nav class="nav nav-pills flex-column">
                <a class="nav-link ms-3 my-1" href="#item-1-1">Item 1-1</a>
                <a class="nav-link ms-3 my-1" href="#item-1-2">Item 1-2</a>
            </nav>
            <a class="nav-link" href="#item-2">Item 2</a>
            <a class="nav-link" href="#item-3">Item 3</a>
            <nav class="nav nav-pills flex-column">
                <a class="nav-link ms-3 my-1" href="#item-3-1">Item 3-1</a>
                <a class="nav-link ms-3 my-1" href="#item-3-2">Item 3-2</a>
            </nav>
        </nav>
    </div>
    <div class="col-8">
        <div class="scrollspy-example" tabindex="0" data-bs-spy="scroll" data-bs-target="#navbar-example3" data-bs-offset="0">
            <h4 id="item-1">Item 1</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 1. Takes you miles high, so high, 'cause she’s got that one international smile. There's a stranger in my bed, there's a pounding in my head. Oh, no. In another life I would make you stay. ‘Cause I, I’m capable of anything. Suiting up for my crowning battle. Used to steal your parents' liquor and climb to the roof. Tone, tan fit and ready, turn it up cause its gettin' heavy. Her love is like a drug. I guess that I forgot I had a choice.</p>
            <h5 id="item-1-1">Item 1-1</h5>
            <p>Placeholder content for the scrollspy example. This one relates to the item 1-1. You got the finest architecture. Passport stamps, she's cosmopolitan. Fine, fresh, fierce, we got it on lock. Never planned that one day I'd be losing you. She eats your heart out. Your kiss is cosmic, every move is magic. I mean the ones, I mean like she's the one. Greetings loved ones let's take a journey. Just own the night like the 4th of July! But you'd rather get wasted.</p>
            <h5 id="item-1-2">Item 1-2</h5>
            <p>Placeholder content for the scrollspy example. This one relates to the item 1-2. Her love is like a drug. All my girls vintage Chanel baby. Got a motel and built a fort out of sheets. 'Cause she's the muse and the artist. (This is how we do) So you wanna play with magic. So just be sure before you give it all to me. I'm walking, I'm walking on air (tonight). Skip the talk, heard it all, time to walk the walk. Catch her if you can. Stinging like a bee I earned my stripes.</p>
            <h4 id="item-2">Item 2</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 2. Don't need apologies. There is no fear now, let go and just be free, I will love you unconditionally. Last Friday night. Don't be a shy kinda guy I'll bet it's beautiful. Summer after high school when we first met. 'Cause she's the muse and the artist. What? Wait. No, no, no, no. Thought that I was the exception.</p>
            <h4 id="item-3">Item 3</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 3. Word on the street, you got somethin' to show me, me. All this money can't buy me a time machine. Make it like your birthday everyday. So we hit the boulevard. You make me feel like I'm livin' a teenage dream, the way you turn me on Skip the talk, heard it all, time to walk the walk. Word on the street, you got somethin' to show me, me. It's no big deal, it's no big deal, it's no big deal.</p>
            <h5 id="item-3-1">Item 3-1</h5>
            <p>Placeholder content for the scrollspy example. This one relates to item 3-1. Baby do you dare to do this? This is no big deal. Yeah, you're lucky if you're on her plane. Just own the night like the 4th of July! Standing on the frontline when the bombs start to fall. So just be sure before you give it all to me.</p>
            <h5 id="item-3-2">Item 3-2</h5>
            <p>Placeholder content for the scrollspy example. This one relates to item 3-2. You're original, cannot be replaced. All night they're playing, your song. California girls we're undeniable. Like a bird without a cage. There is no fear now, let go and just be free, I will love you unconditionally. I can see the writing on the wall. You could travel the world but nothing comes close to the golden coast.</p>
        </div>
    </div>
</div>
```

Результат:

![Bootstrap scrollspy](../../../en/images/coding-examples/coding-examples-scrollspy.png)

Также нужно внести некоторые стилистические изменения в user.css:

```css
    .scrollspy-example {
        height: 350px;
        overflow-y: scroll;
    }
```

Закавыка: меню в этом примере плохо сочетается с содержимым!

### Пример 10: Таблица

Вкладки часто используются как элементы навигации в сочетании с выпадающими списками. Пример кода Bootstrap:

```html
<ul class="nav nav-tabs">
    <li class="nav-item"><a class="nav-link active" href="#" aria-current="page">Active</a></li>
    <li class="nav-item dropdown"><a class="nav-link dropdown-toggle" role="button" href="#" data-bs-toggle="dropdown" aria-expanded="false">Dropdown</a>
        <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Action</a></li>
            <li><a class="dropdown-item" href="#">Another action</a></li>
            <li><a class="dropdown-item" href="#">Something else here</a></li>
            <li><hr class="dropdown-divider" /></li>
            <li><a class="dropdown-item" href="#">Separated link</a></li>
        </ul>
    </li>
    <li class="nav-item"><a class="nav-link" href="#">Link</a></li>
    <li class="nav-item"><a class="nav-link disabled" tabindex="-1" href="#" aria-disabled="true">Disabled</a></li>
</ul>
```

Результат:

![Bootstrap tab](../../../en/images/coding-examples/coding-examples-tab.png)

Не забудьте проверить оба варианта: вкладку и выпадающий список, чтобы выпадающая часть работала.

### Пример 11: Тост

Тосты – это легковесные уведомления, созданные для имитации push-уведомлений, которые были популяризированы мобильными и настольными операционными системами. Они построены с использованием flexbox, поэтому их легко выровнять и позиционировать. Пример кода Bootstrap:

```html
<div class="toast fade show" role="alert" aria-live="assertive" aria-atomic="true">
    <div class="toast-header">
        <img class="rounded me-2" src="..." alt="..." />
        <strong class="me-auto">Bootstrap</strong> <small>11 mins ago</small>
        <button class="btn-close" type="button" data-bs-dismiss="toast" aria-label="Close"></button>
    </div>
    <div class="toast-body">Hello, world! This is a toast message.</div>
</div>
```

Результат:

![Bootstrap toast](../../../en/images/coding-examples/coding-examples-toast.png)

Обратите внимание, что демо-версия Bootstrap, использующая кнопку для отображения сообщения Toast, требует дополнительного JavaScript. Похоже, этому компоненту нужен программист для эффективного использования!

### Пример 12: Подсказка

Подсказка — это небольшой фрагмент текста, который появляется при наведении курсора на кнопку или ссылку, чтобы объяснить, что это такое или что она делает. Подсказка может располагаться выше, ниже, слева или справа от элемента. Если не указано, то по умолчанию подсказка расположена сверху. Подсказка переключится на другую позицию, если в указанной позиции недостаточно места. Пример кода Bootstrap:

```html
<p><button class="btn btn-secondary" title="Tooltip on left" type="button" data-bs-toggle="tooltip" data-bs-placement="left"> Tooltip on left </button>
<button class="btn btn-secondary" title="Tooltip" type="button" data-bs-toggle="tooltip"> Tooltip </button>
<button class="btn btn-secondary" title="Tooltip on top" type="button" data-bs-toggle="tooltip" data-bs-placement="top"> Tooltip on top </button>
<button class="btn btn-secondary" title="Tooltip on right" type="button" data-bs-toggle="tooltip" data-bs-placement="right"> Tooltip on right </button>
<button class="btn btn-secondary" title="Tooltip on bottom" type="button" data-bs-toggle="tooltip" data-bs-placement="bottom"> Tooltip on bottom </button>
<button class="btn btn-secondary" title="<em>Tooltip</em> <u>with</u> <b>HTML</b>" type="button" data-bs-toggle="tooltip" data-bs-html="true"> Tooltip with HTML </button></p>
<p>Tooltip triggered by class: <button class="btn btn-warning" title="Tooltip Message">Alert!</button></p>
```

Результат:

![Bootstrap tooltip](../../../en/images/coding-examples/coding-examples-tooltip.png)

## Подход 2: Использование плагина контента

Этапы:

- Загрузите, установите и активируйте этот [плагин](https://github.com/ceford/j4xdemos-plg-bscompos/raw/master/plg_j4xdemos_bscompos.zip)
- В статье добавьте текст, на который действует плагин, например, {bscompos modal carousel}, который вызовет загрузку JavaScript, необходимого для поддержания модального диалога и карусели. Плагин удаляет текст триггера и заключающие (теперь) пустые теги.
- Включите HTML-код компонента Bootstrap непосредственно в статью или в модуль, включенный в статью. Ниже приведен пример HTML-кода для простого модального окна и модального окна с каруселью. Обратите внимание, что это не сработает, если HTML-код находится в модуле в местоположении шаблона.
- Это также будет работать для стандартного пользовательского модуля, если параметр «Подготовить содержимое» установлен в значение «Да».
- Протестируйте это!

## Подход 3: Использование переопределения шаблона

Этапы, включенные в процесс:

- Создайте переопределение шаблона mod_custom.
- Добавьте модуль mod_custom, содержащий разметку компонента и вызывающие классы.
- Включите модуль в статью.

### Переопределение шаблона mod_custom

- В интерфейсе администратора перейдите в **Система → Шаблоны сайта → Cassiopeia Подробности и файлы**.
- Выберите **Создать переопределения **→** mod_custom **→** default.php**.
- На строке после defined('\_JEXEC') или die; добавьте следующий код:

```php
    $module_class = $params->get('moduleclass_sfx');
    if (!empty($module_class))
    {
        $classes = explode(' ', $module_class);
        foreach ($classes as $class)
        {
        switch ($class)
            {
              case 'bs-alert':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.alert', '.alert');
                break;
              case 'bs-button':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.button', '.btn');
                break;
              case 'bs-carousel':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.carousel', '.selector', []);
                break;
              case 'bs-collapse':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.collapse', '.selector', []);
                break;
              case 'bs-dropdown':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.dropdown', '.selector', []);
                break;
              case 'bs-modal':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.modal', '.selector', []);
                break;
              case 'bs-offcanvas':
                // Not Found
                //\Joomla\CMS\HTML\HTMLHelper::_('bootstrap.offcanvas', '.btn', []);
                break;
              case 'bs-popover':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', '.btn', []);
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', 'a', []);
                break;
              case 'bs-scrollspy':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.scrollspy', '.selector', []);
                break;
              case 'bs-tab':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tab', '.selector', []);
                break;
              case 'bs-tooltip':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', '.btn', []);
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', 'a', []);
                break;
              case 'bs-toast':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.toast', '.selector', []);
                break;
              default:
                // do nothing
            }
        }
    }
```

Этот код ищет имена классов, установленные в mod_custom, и вызывает HTMLHelper для настройки поддержки JavaScript. Обратите внимание, что последний элемент в каждом вызове является селектором, который может использоваться или не использоваться для запуска действия. Многие компоненты срабатывают по атрибутам данных в разметке и не используют селекторы здесь. Для некоторых селектор необходим. Например, логично использовать класс **.btn** и тег **a**, чтобы запускать всплывающие подсказки.

### Модуль mod_custom для модального компонента

- Перейдите в **Content**→**Site Modules**→**New**.
- Выберите модуль **Custom**.
- Заполните форму:
  - Заголовок: Demo Modal
  - В поле Position введите **demomodal** для использования в статье;
  - Содержимое модуля: Переключите редактор для ввода обычного текста.
  - Вставьте следующий код из документации Bootstrap:

```html
<h2>Modal</h2>
<!-- Button trigger modal -->
<p><button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button></p>
<!-- Modal -->
<div id="exampleModal" class="modal fade" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 id="exampleModalLabel" class="modal-title">Modal title</h5>
                <button class="btn-close" type="button" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" type="button" data-bs-dismiss="modal">Close</button>
                <button class="btn btn-primary" type="button">Save changes</button>
            </div>
        </div>
    </div>
</div>
```

- Выберите вкладку «Дополнительно» и в поле «Класс модуля» введите **bs-modal**
- Необязательно: установите для заголовка значение «Скрыть», чтобы использовать H2 в вставленном коде.
- Сохраните и закройте (не волнуйтесь, если модальное окно выглядит неправильно в редакторе).

### Создать статью и элемент меню

- Создайте новую статью, Demo Modal, и в режиме ввода обычного текста установите содержимое на

```html
<div>{loadposition demomodal}</div>
```

- Создайте пункт меню для одной статьи.
- Проверьте это:

![Bootstrap modal module in article](../../../en/images/coding-examples/coding-examples-modal-module.png)

### Модальный компонент, содержащий карусель

- Создайте новый Пользовательский модуль с новым заголовком: Демонстрационный модальный карусель
- Позиция: demomodalcarousel
- **Закладка "Дополнительно"**→**Класс модуля**: bs-modal bs-carousel
- Пользовательский контент модуля в виде простого текста:

```html
<h2>Modal with Carousel</h2>
<div class="mod-custom custom">
    <!-- Button trigger modal -->
    <button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button>
</div>
<div id="exampleModal" class="modal fade" style="display: none;" tabindex="-1" aria-hidden="true">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <button class="close" type="button" data-bs-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">×</span>
                </button>
            </div>
            <div class="modal-body">
                <div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
                    <div class="carousel-indicators">
                        <button class="active" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" aria-current="true" aria-label="Slide 1"></button>
                        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
                        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
                    </div>
                    <div class="carousel-inner">
                        <div class="carousel-item active">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa1-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>First slide label</h5>
                                <p>Some representative placeholder content for the first slide.</p>
                            </div>
                        </div>
                        <div class="carousel-item">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa2-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>Second slide label</h5>
                                <p>Some representative placeholder content for the second slide.</p>
                            </div>
                        </div>
                        <div class="carousel-item">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa3-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>Third slide label</h5>
                                <p>Some representative placeholder content for the third slide.</p>
                            </div>
                        </div>
                    </div>
                    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
                        <span class="visually-hidden">Previous</span>
                    </button>
                    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
                        <span class="visually-hidden">Next</span>
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>
```

- Создайте новую статью с {loadposition demomodalcarousel} в содержимом.
- Создайте новый пункт меню для одной статьи: Demo Modal Carousel
- Проверьте это:

![Bootstrap modal carousel](../../../en/images/coding-examples/coding-examples-modal-carousel.png)

*Переведено openai.com*
