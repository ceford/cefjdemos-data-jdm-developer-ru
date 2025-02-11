<!-- Filename: J4.x:Namespace_Conventions_In_Joomla / Display title: Пространства имён -->

## Введение

Использование **пространств имен PHP** — это способ группировки связанных классов, интерфейсов, функций и констант для избежания конфликтов имен в крупных проектах. Они позволяют разработчикам более эффективно организовывать код, особенно при работе со сторонними библиотеками или большими кодовыми базами, в которых могут встречаться дублирующиеся имена классов.

Больше информации можно найти в разделе [Namespace](jdocmanual?article=docus/namespaces/index) документации для программистов Joomla!.

### Ключевые особенности пространств имен PHP

1. **Избегайте конфликтов имён**:
   - Без пространств имен, если два класса имеют одинаковое имя, PHP выдаст ошибку. Пространства имен предоставляют контекст для уникальной идентификации классов или других элементов.
2. **Организуйте код**:
   - Пространства имен позволяют логически группировать связанные классы или функции, делая код более понятным и легким в поддержке.
3. **Доступ через полное квалифицированное имя**:
   - Классы и функции внутри пространства имен могут быть доступны с использованием их полного квалифицированного имени, которое включает путь пространства имен.

### Общие практики:
- Используйте пространства имен, чтобы отражать структуру папок вашего проекта для согласованности.
- Импортируйте пространства имен с помощью ключевого слова `use` для более чистого кода.

## Файл манифеста

Начальное объявление пространства имен расширения находится в файле манифеста расширения, как в этом примере:

```xml
    <namespace path="src">Acme\Component\Wonderful</namespace>
```

Элементы в этой декларации следующие:

- **path="src"** указывает загрузчику классов, что классы с пространствами имен находятся в папке **src** расширения, например, com_wonderful/src.
- **Acme** — это префикс поставщика. Для основных расширений Joomla это будет *Joomla*. Вам нужно придумать свой уникальный префикс поставщика.
- **Component** обозначает тип расширения (компонент, модуль, плагин, шаблон, библиотека и т. д.).
- **Wonderful** — это имя расширения. Выберите имя расширения, которое будет коротким, понятным и, вероятно, уникальным.

## Файл класса

Объявление пространства имен должно быть первой инструкцией в файле, в котором оно используется. Например:

```php
<?php

/**
 * @package     Wonderful
 * @subpackage  Site
 *
 * @copyright   (C) 2024 Merlin. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Acme\Component\Wonderful\Controller;

use Joomla\CMS\MVC\Controller\BaseController;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Controller to display the default page - display of wonderful items
 *
 * @since  1.0.0
 */
class MagicController extends BaseController
{
    public function dosomething($wonderful)
    {
        // ... output something wonderful!
    }
}
```

## Использование файла класса

Когда вам нужно использовать класс, например в шаблоне, вы можете вставить оператор **use** в верхней части шаблона:

```
use Acme\Component\Wonderful\Controller\MagicController;
```

А затем создайте экземпляр класса и вызовите функцию:

```
$magic = new MagicController;
$result = $magic->dosomething('wonderful');
```

## Файл autoload_psr4.php

Этот файл находится в папке administrator/cache сайта Joomla. Он содержит полную карту пространств имен, извлеченную из файлов манифеста. Генерируется при установке и регенерируется каждый раз, когда устанавливается или переустанавливается расширение. Вы можете удалить этот файл, и он будет сгенерирован заново при следующей загрузке страницы. Иногда это необходимо сделать, если установка расширения была предпринята необычным методом. Файл содержит 275 или более путей к расположениям классов в пространствах имен.

**PSR** — это аббревиатура от [**PHP Standards Recommendation**](https://www.php-fig.org/psr/), а [**PSR-4: Autoloader**](https://www.php-fig.org/psr/psr-4/) — это принятый стандарт для загрузки классов PHP.

*Переведено openai.com*

