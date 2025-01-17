<!-- Filename: J4.x:SCSS_and_Sass / Display title: Тестирование CSS и JavaScript -->

## Введение

В рабочем сайте Joomla, Joomla доставляет сжатые файлы CSS и Javascript (те, в названии которых присутствует `.min`). Серверы отправят версии, сжатые с помощью gzip, если они доступны (с суффиксом `.gz`). Эти файлы создаются из несжатых источников. В случае файлов CSS они часто создаются путем обработки предварительных SCSS. Для тестирования кода, включающего новые или обновленные CSS или JavaScript, необходимо пересобрать файлы CSS и их сжатые и минимизированные версии из измененных источников.

## Тестирование установки

Для тестирования вам нужен тестовый сервер с установленными Git, Node.js и Composer. Перейдите в репозиторий [Joomla CMS](https://github.com/joomla/joomla-cms) на GitHub и следуйте инструкциям в разделе *How to get a working installation from the source* в конце текста README.

По завершении установки Joomla не удаляйте каталог Installation! Это также удалит каталог `build`, который необходим для пересборки файлов CSS и JavaScript.

Основные файлы `.scss` находятся в следующих каталогах:

- media/templates/site/cassiopeia/scss
- media/templates/administrator/atum/scss

Скрипт генерации CSS, компилятор SCSS и другие подобные инструменты сборки находятся в каталоге `build/`. Каталог сборки доступен только из исходного кода Joomlaǃ, он не включен в официальный релиз Joomlaǃ.

Инструкция по установке включает следующее:

```sh
git checkout 5.2-dev (or whatever branch you wish to work with)
composer install
npm ci
```

Команда `npm ci` является синонимом для `npm clean-install`, используемым в любой ситуации, когда вы хотите удостовериться, что производите чистую установку ваших зависимостей.

## Повседневное использование

Существуют альтернативные команды для использования в ситуациях, когда были изменены только файлы SCSS или JavaScript:

```sh
npm run build:css¶
    This command compiles SCSS files to CSS and also creates the minified files.

npm run build:js¶
    This command compiles and transpiles the JavaScript files to the correct format and creates minified files.
```

Команды просматривают каталоги media и templates и создают окончательные версии файлов, необходимые для установки Joomla.

## Sass, SCSS и CSS

> Sass — это язык стилей, который компилируется в CSS. Он позволяет использовать переменные, вложенные правила, миксины, функции и многое другое, с полностью совместимым с CSS синтаксисом. Sass помогает держать большие таблицы стилей в порядке и упрощает совместное использование дизайна внутри и между проектами. Файлы Sass имеют суффикс `scss`.

### CSS

CSS код выражается следующим образом:

```css
#header {
    margin: 0;
    border: 1px solid blue;
}
#header p {
    font-size: 14px;
    color: blue;
}
#header a {
    text-decoration: none;
}
```

### SCSS

`SCSS` — это расширение синтаксиса CSS. Оно используется в ядре Joomlaǃ

```css
$color:    blue;
#header {
    margin: 0;
    border: 1px solid $color;
    p {
        color: $colorRed;
        font: {
            size: 14px;
        }
    }
    a {
        text-decoration: none;
    }
}
```

Более старый синтаксис `Sass` предоставляет более лаконичный способ написания CSS.

```css
$color:    blue
#header
    margin: 0
    border: 1px solid $color
        p
            color: $colorRed
            font:
                size: 14px
        a
            text-decoration: none
```

Вы можете найти больше информации в [документации Sass](http://sass-lang.com/documentation/syntax/).

## От существующего CSS к SCSS

Если вы хотите настроить шаблон Cassiopeia, хорошей идеей будет скопировать шаблон и настроить его позже, чтобы обновления Joomla! не перезаписали ваши настройки. Чтобы добавить свои CSS файлы в копию шаблона Cassiopeia, просто переименуйте свои файлы `.css` в `.scss`. Тогда вы сможете воспользоваться функциями, которые предлагает SCSS:

- Расширения языка, такие как переменные, вложения и миксины
- Многие полезные функции для манипуляции цветами и другими значениями
- Продвинутые возможности, такие как директивы управления для библиотек
- Хорошо отформатированный, настраиваемый вывод

Посетите [сайт Sass](https://sass-lang.com/) для получения полной информации.

### Импортируйте ваши .css как .scss файлы

Предполагая, что вы хотите включить ваши собственные файлы и чтобы их обработал компилятор SCSS - вы НЕ должны переписывать ваши файлы .css на SCSS, так как обычные `.css` тоже работают. Что вам нужно сделать:

- переименуйте ваши файлы `.css` в `.scss`
- добавьте к ним префикс `_`
- добавьте оператор `@import` в нижней части файла `media/templates/site/copy_of_cassiopeia/scss/template.scss`, чтобы он переопределял существующие декларации:

```css
// Bootstrap functions
@import "../../../media/vendor/bootstrap/scss/functions";
//...
// 50 or more lines of import statements
// Finally add the cassiopeia colours
:root {
  @each $color, $value in $cassiopeia-colors {
    --#{$color}: #{$value};
  }
// More lines containing scss color statements, then your own styles:
@import "mystyles";
}
```

Если вы теперь скомпилируете ваш основной файл template.scss, вы получите один основной файл template.css, оптимизированный и минифицированный. Вы также сократили количество HTTP-запросов, и сайт должен загружаться быстрее.

*Переведено openai.com*

