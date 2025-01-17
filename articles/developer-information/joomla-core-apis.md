<!-- Filename: J4.x:Joomla_Core_APIs / Display title: Joomla Core API -->

Эта страница перечисляет доступные конечные точки в Joomla на примере команд curl. Она была подготовлена для Joomla 4 и требует проверки на соответствие с текущими версиями Joomla.

Каждый URL-адрес требует аутентификации, если только он не обозначен как публичный URL. Для безопасности в Joomla 4.0.0 мы планируем сделать так, чтобы Api-приложение по умолчанию требовало учетную запись суперпользователя (так как API-приложение совершенно новое), это требование может быть ослаблено по мере стабилизации и тщательного тестирования API в сообществе. Если вы используете плагин основной аутентификации (в настоящее время единственный предоставляемый плагин в Joomla 4 alpha 10), он требует добавления к командам curl ниже с использованием --user user_name:password

Каждому URL необходимо добавить префикс с хостом Joomla перед путем (например, вместо `/api/index.php/v1/article` вам нужно `http://example.com/api/index.php/v1/article`).

Любые имена свойств в фигурных скобках ({}) указывают на то, что свойство является переменной, которую следует заменить.

Если не указано иное, эти API были введены в Joomla 4. Для получения дополнительной информации о спецификации API Joomla (в отличие от этого списка URL и опций) посетите [Joomla Api Specification](https://docs.joomla.org/Joomla_Api_Specification).

Где конечные точки требуют значения {app}, переменная в настоящее время принимает два значения: сайт (front end), или администратор (back end)

## Полезные ресурсы

Было создано несколько дополнительных ресурсов, чтобы познакомить вас с Joomla Web Services и помочь вам изучить, как реализовать веб-сервисы в вашем проекте.

- Коллекция Postman вызовов [Joomla Web Services API](https://github.com/alexandreelise/j4x-api-collection) от Alexandre Elise.
- Видеоурок по Joomla 4 на YouTube: [Использование веб-сервисов API](https://www.youtube.com/watch?v=lT9qodsvfZg) с Eoin Oliver.
- Журнал сообщества Joomla: [Joomla Web Services API 101](https://magazine.joomla.org/all-issues/august-2020/joomla-web-services-api-101-tokens,-testing-and-a-taste-test) от Patrick Jackson.

## Компонент баннеров

### Баннеры

#### Получить список баннеров

curl -X GET /api/index.php/v1/баннеры

#### Получить одиночный баннер

curl -X GET /api/index.php/v1/banners/{banner_id}

#### Удалить баннер

`curl -X DELETE /api/index.php/v1/banners/{banner_id}`

#### Создать баннер

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners -d

```json
{
    "catid": 3,
    "clicks": 0,
    "custombannercode": "",
    "description": "Текст",
    "metakey": "",
    "name": "Имя",
    "params": {
        "alt": "",
        "height": "",
        "imageurl": "",
        "width": ""
    }
}
```

#### Обновить баннер

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/{banner_id} -d

```json
{
    "alias": "имя",
    "catid": 3,
    "description": "Новый текст",
    "name": "Новое имя"
}
```

### Клиенты

#### Получить список клиентов

`curl -X GET /api/index.php/v1/banners/clients`

#### Получить одного клиента

curl -X GET /api/index.php/v1/banners/clients/{client_id}

#### Удалить клиента

curl -X DELETE /api/index.php/v1/banners/clients/{client_id}

#### Создать клиента

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/banners/clients -d
```

```json
{
    "contact": "Имя",
    "email": "email@mail.com",
    "extrainfo": "",
    "metakey": "",
    "name": "Клиенты",
    "state": 1
}
```

#### Обновить клиента

curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/banners/clients/{client_id} -d

{
    "contact": "новое имя",
    "email": "newemail@mail.com",
    "name": "Клиенты"
}

### Категории баннеров

#### Получить список категорий

curl -X GET /api/index.php/v1/banners/categories

#### Получить одну категорию

curl -X GET /api/index.php/v1/banners/categories/{category_id}

#### Удалить категорию

curl -X DELETE /api/index.php/v1/banners/categories/{category_id}

#### Создать категорию

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners/categories -d

```json
{
    "доступ": 1,
    "алиас": "cat",
    "расширение": "com_banners",
    "язык": "*",
    "заметка": "",
    "родительский_ид": 1,
    "опубликовано": 1,
    "заголовок": "Заголовок"
}
```

#### Обновить категорию

```markdown
curl -X PATCH -H "Content-Type: application/json"
```
/api/index.php/v1/banners/categories/{category_id} -d

```json
{
    "alias": "cat",
    "note": "Некоторый текст",
    "parent_id": 1,
    "title": "Новое название"
}
```

### История содержимого баннеров

#### Получить список истории контента

`curl -X GET /api/index.php/v1/banners/contenthistory/{banner_id}`

#### Переключить сохранение истории контента

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/contenthistory/keep/{contenthistory_id}
```

#### Удалить историю контента

curl -X DELETE  
/api/index.php/v1/banners/contenthistory/{contenthistory_id}

## Компонент конфигурации

### Приложение

#### Получить список конфигураций приложения

curl -X GET /api/index.php/v1/config/application

#### Обновить конфигурацию приложения

```markdown
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d
```

```json
{
    "debug": true,
    "sitename": "123"
}
```

### Компонент

#### Получить список конфигураций компонентов

`curl -X GET /api/index.php/v1/config/{component_name}`

Пример «component_name» - это «com_content».

#### Обновление конфигурации приложения компонента

curl -X PATCH -H "Content-Type: application/json"  
/api/index.php/v1/config/application -d

```json
{
    "link_titles": 1
}
```

## Компонент Контакты

### Контакт

#### Получить список контактов

`curl -X GET /api/index.php/v1/contact`

#### Получить одного контакта

`curl -X GET /api/index.php/v1/contact/{contact_id}`

#### Удалить контакт

`curl -X DELETE /api/index.php/v1/contact/{contact_id}`

#### Создать контакт

```markdown
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/contact -d
```

```json
{
    "alias": "contact",
    "catid": 4,
    "language": "*",
    "name": "Контакт"
}
```

#### Обновить контакт

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/contact/{contact_id} -d

```json
{
    "alias": "контакт",
    "catid": 4,
    "name": "Новый контакт"
}
```

#### Отправить контактную форму

```markdown
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact/form/{contact_id} -d
```

```json
{
    "contact_email": "email@mail.com",
    "contact_message": "какой-то текст",
    "contact_name": "имя",
    "contact_subject": "тема"
}
```

### Категории Контактов

1. Маршрут категорий контактов: "v1/contact/categories"
2. Работа с ним аналогична категориям баннеров.

### Контактные данные

#### Получить список полей Контакта

curl -X GET /api/index.php/v1/fields/contact/contact

#### Получить контакт по одному полю

`curl -X GET /api/index.php/v1/fields/contact/contact/{field_id}`

#### Удалить поле Контакт

curl -X DELETE /api/index.php/v1/fields/contact/contact/{field_id}

#### Создать контактное лицо

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/fields/contact/contact -d
```

```
{
    "access": 1,
    "context": "com_contact.contact",
    "default_value": "",
    "description": "",
    "group_id": 0,
    "label": "поле контактов",
    "language": "*",
    "name": "contact-field",
    "note": "",
    "params": {
        "class": "",
        "display": "2",
        "display_readonly": "2",
        "hint": "",
        "label_class": "",
        "label_render_class": "",
        "layout": "",
        "prefix": "",
        "render_class": "",
        "show_on": "",
        "showlabel": "1",
        "suffix": ""
    },
    "required": 0,
    "state": 1,
    "title": "поле контактов",
    "type": "text"
}
```

#### Обновить поле контакта

`curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/fields/contact/contact/{field_id} -d`

```json
{
    "title": "новое контактное поле",
    "name": "контактное-поле",
    "label": "контактное поле",
    "default_value": "",
    "type": "текст",
    "note": "",
    "description": "Некоторый новый текст"
}
```

### Поля контактной почты

1. Поле маршрута Контакт Почта: "v1/fields/contact/mail"
2. Работа с ним аналогична работе с Полями Контакта.

### Поля Категории Контактов

1.  Поля маршрута Категории контактов: "v1/fields/contact/categories"
2.  Работа с ним аналогична работе с полями контакта.

### Поля Групп Контакт

#### Получить список полей контактов группы

curl -X GET /api/index.php/v1/fields/groups/contact/contact

#### Получить поля контакта одной группы

curl -X GET /api/index.php/v1/fields/groups/contact/contact/{group_id}

#### Удалить поля группы контактов

curl -X DELETE  
/api/index.php/v1/fields/groups/contact/contact/{group_id}

#### Создать группу полей Контакт

```markdown
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/fields/groups/contact/contact -d
```

```json
{
    "access": 1,
    "context": "com_contact.contact",
    "default_value": "",
    "description": "",
    "group_id": 0,
    "label": "поле контакта",
    "language": "*",
    "name": "contact-field3",
    "note": "",
    "params": {
        "class": "",
        "display": "2",
        "display_readonly": "2",
        "hint": "",
        "label_class": "",
        "label_render_class": "",
        "layout": "",
        "prefix": "",
        "render_class": "",
        "show_on": "",
        "showlabel": "1",
        "suffix": ""
    },
    "required": 0,
    "state": 1,
    "title": "поле контакта",
    "type": "text"
}
```

#### Обновить поля группы Контакт

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/fields/groups/contact/contact/{group_id} -d
```

```json
{
    "title": "новая контактная группа",
    "note": "",
    "description": "новое описание"
}
```

### Групповые поля контактной почты

1. Поля маршрута группы Контакт Почта: "v1/fields/groups/contact/mail"
2. Работа с ним аналогична Полям группы Контакт.

### Категории контактов полей группы

1. Групповые поля маршрута Категории контактов:
    "v1/fields/groups/contact/categories"
2. Работа с ним аналогична Группам полей контакта.

### История Контактов

1.  История содержимого маршрута: "v1/contact/contenthistory"
2.  Работа с ней аналогична Истории содержимого баннеров.

## Компонент содержимого

### Статьи

#### Получить список статей

`curl -X GET /api/index.php/v1/content/articles`

#### Получить одну статью

curl -X GET /api/index.php/v1/content/articles/{article_id}

#### Удалить статью

curl -X DELETE /api/index.php/v1/content/articles/{article_id}

#### Создать статью

```shell
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/content/articles -d
```

```json
{
    "alias": "my-article",
    "articletext": "Мой текст",
    "catid": 64,
    "language": "*",
    "metadesc": "",
    "metakey": "",
    "title": "Вот статья"
}
```

В настоящее время упомянутые здесь параметры являются обязательными свойствами. Однако в настоящее время планируется сделать как минимум metakey и metadesc необязательными в API.

#### Обновить статью

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/content/articles/{article_id} -d

```json
{
    "catid": 64,
    "title": "Обновленная статья"
}
```

### Категории контента

1. Маршрут для категорий контента: "v1/content/categories"
2. Работа с ним аналогична работе с категориями баннеров. Примечание: если рабочие процессы включены, то необходимо указать рабочий процесс (так же, как в пользовательском интерфейсе).

### Статьи о полях

1. Поля маршрута статьи: "v1/fields/content/articles"
2. Работа с ним аналогична Fields Contact.

### Группы Поля Статьи

1.  Поля для групп маршрутов Статьи это: "v1/fields/groups/content/articles"
2.  Работа с этим аналогична Полям для групп Контакт.

### Категории полей

1. Категории полей маршрута: "v1/fields/groups/content/categories"
2. Работа с ним аналогична работе с полями контакта.

### История содержимого компонента содержания

1. История содержимого маршрута:
    "v1/content/articles/{article_id}/contenthistory"
2. Работа с ней похожа на историю содержимого баннеров.

## Компонент Языки

### Языки

#### Получить список языков

`curl -X GET /api/index.php/v1/languages`

#### Установка языка

`curl -X POST -H "Content-Type: application/json" /api/index.php/v1/languages -d`

```json
{
    "package": "pkg_fr-FR"
}
```

### Языки контента

#### Получить список языков содержимого

`curl -X GET /api/index.php/v1/languages/content`

#### Получить единственный язык содержимого

curl -X GET /api/index.php/v1/v1/languages/content/{language_id}

#### Удалить язык содержимого

curl -X DELETE /api/index.php/v1/languages/content/{language_id}

#### Создать язык контента

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/content -d

```json
{
    "access": 1,
    "description": "",
    "image": "fr_FR",
    "lang_code": "fr-FR",
    "metadesc": "",
    "metakey": "",
    "ordering": 1,
    "published": 0,
    "sef": "fk",
    "sitename": "",
    "title": "Французский (FR)",
    "title_native": "Французский (Франция)"
}
```

#### Обновить язык содержимого

```
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/languages/content/{language_id} -d
```

```json
{
    "description": "",
    "lang_code": "ru-RU",
    "metadesc": "",
    "metakey": "",
    "sitename": "",
    "title": "Русский (ru-RU)",
    "title_native": "Русский (Россия)"
}
```

### Переопределения языков

#### Получить список языковых констант переопределений

curl -X GET /api/index.php/v1/languages/overrides/{app}/{lang_code}

#### Получить единственную переопределенную языковую константу

`curl -X GET /api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id}`

#### Удалить переопределения языка контента

curl -X DELETE
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id}

#### Создать переопределения языка контента

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/{app}/{lang_code} -d

```json
{
    "ключ": "новый_ключ",
    "переопределить": "текст"
}
```

#### Обновить переопределения языка содержимого

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id} -d

```json
{
    "key":"new_key",
    "override": "новый текст"
}
```

`var app - enum {"site", "administrator"}`

var lang_code - строка Пример: “fr-FR“, “en-GB“ вы можете получить lang_code из v1/languages/content

#### Постоянная замена поиска

```plaintext
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search -d
```

```json
{
    "searchstring": "JLIB_APPLICATION_ERROR_SAVE_FAILED",
    "searchtype": "constant"
}
```

var searchtype - enum {“constant”, “value”}. “constant” - поиск по имени константы, “value” - поиск по значению константы

#### Обновить Кэш Поиска с Перекрытием

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search/cache/refresh

## Компонент Навигационные меню

### Меню

#### Получить список меню

curl -X GET /api/index.php/v1/меню/{app}

#### Получить одно меню

curl -X GET /api/index.php/v1/меню/{app}/{menu_id}

#### Удалить меню

curl -X DELETE /api/index.php/v1/меню/{app}/{menu_id}

#### Создать меню

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app} -d
```

```json
{
    "client_id": 0,
    "description": "Меню для сайта",
    "menutype": "меню",
    "title": "Меню"
}
```

#### Обновить меню

```
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/menus/{app}/{menu_id} -d
```

```json
{
    "menutype": "меню",
    "title": "Новое меню"
}
```

### Элементы меню

#### Получить список типов элементов меню

```markdown
curl -X GET /api/index.php/v1/menus/{app}/items/types
```

#### Получить список пунктов меню

`curl -X GET /api/index.php/v1/menus/{app}/items`

#### Получить один элемент меню

curl -X GET /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Удалить элемент меню

```markdown
curl -X DELETE /api/index.php/v1/menus/{app}/items/{menu_item_id}
```

#### Создать элемент меню

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items -d

```json
{
    "access": "1",
    "alias": "",
    "associations": {
        "en-GB": "",
        "fr-FR": ""
    },
    "browserNav": "0",
    "component_id": "20",
    "home": "0",
    "language": "*",
    "link": "index.php?option=com_content&view=form&layout=edit",
    "menutype": "mainmenu",
    "note": "",
    "params": {
        "cancel_redirect_menuitem": "",
        "catid": "",
        "custom_cancel_redirect": "0",
        "enable_category": "0",
        "menu-anchor_css": "",
        "menu-anchor_title": "",
        "menu-meta_description": "",
        "menu-meta_keywords": "",
        "menu_image": "",
        "menu_image_css": "",
        "menu_show": "1",
        "menu_text": "1",
        "page_heading": "",
        "page_title": "",
        "pageclass_sfx": "",
        "redirect_menuitem": "",
        "robots": "",
        "show_page_heading": ""
    },
    "parent_id": "1",
    "publish_down": "",
    "publish_up": "",
    "published": "1",
    "template_style_id": "0",
    "title": "заголовок",
    "toggle_modules_assigned": "1",
    "toggle_modules_published": "1",
    "type": "компонент"
}
```

Пример для "Создать страницу статьи"

#### Обновить пункт меню

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items/{menu_item_id} -d

```json
{
    "component_id": "20",
    "language": "*",
    "link": "index.php?option=com_content&view=form&layout=edit",
    "menutype": "mainmenu",
    "note": "",
    "title": "новый заголовок",
    "type": "компонент"
}
```

Пример для "Создать страницу статьи"

## Компонент сообщений

### Сообщения

#### Получить список сообщений

`curl -X GET /api/index.php/v1/messages`

#### Получить одно сообщение

`curl -X GET /api/index.php/v1/messages/{message_id}`

#### Удалить сообщение

`curl -X DELETE /api/index.php/v1/messages/{message_id}`

#### Создать сообщение

```markdown
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/messages -d
```

```json
{
    "message": "текст",
    "state": 0,
    "subject": "текст",
    "user_id_from": 773,
    "user_id_to": 772
}
```

#### Обновить сообщение

`curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/messages/{message_id} -d`

```markdown
{
    "message": "новый текст",
    "subject": "новый текст",
    "user_id_from": 773,
    "user_id_to": 772
}
```

## Компонент модулей

### Модули

#### Получить список типов модулей

`curl -X GET /api/index.php/v1/modules/types/{app}`

#### Получить список модулей

curl -X GET /api/index.php/v1/modules/{app}

#### Получить один модуль

`curl -X GET /api/index.php/v1/modules/{app}/{module_id}`

#### Удалить модуль

`curl -X DELETE /api/index.php/v1/modules/{app}/{module_id}`

#### Создать модуль

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/modules/{app} -d
```

```json
{
    "доступ": "1",
    "назначенные": [
        "101",
        "105"
    ],
    "назначение": "0",
    "id_клиента": "0",
    "язык": "*",
    "модуль": "mod_articles_archive",
    "заметка": "",
    "порядок": "1",
    "параметры": {
        "bootstrap_размер": "0",
        "кэш": "1",
        "время_кэша": "900",
        "режим_кэша": "static",
        "количество": "10",
        "класс_заголовка": "",
        "тег_заголовка": "h3",
        "макет": "_:default",
        "тег_модуля": "div",
        "модульный_класс_sfx": "",
        "стиль": "0"
    },
    "позиция": "",
    "публикация_окончание": "",
    "публикация_начало": "",
    "опубликовано": "1",
    "показывать_заголовок": "1",
    "заголовок": "Заголовок"
}
```

Пример для «Статьи - Архив»

#### Обновить модуль

```markdown
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/modules/{app}/{module_id} -d
```

```json
{
    "access": "1",
    "client_id": "0",
    "language": "*",
    "module": "mod_articles_archive",
    "note": "",
    "ordering": "1",
    "title": "Новый заголовок"
}
```

Пример для «Статьи - Архив»

## Компонент новостных лент

### Ленты

#### Получить список лент

curl -X GET /api/index.php/v1/newsfeeds/feeds

#### Получить одиночную ленту

curl -X GET /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Удалить ленту

`curl -X DELETE /api/index.php/v1/newsfeeds/feeds/{feed_id}`

#### Создать ленту

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/newsfeeds/feeds -d
```

```json
{
    "access": 1,
    "alias": "псевдоним",
    "catid": 5,
    "description": "",
    "images": {
        "float_first": "",
        "float_second": "",
        "image_first": "",
        "image_first_alt": "",
        "image_first_caption": "",
        "image_second": "",
        "image_second_alt": "",
        "image_second_caption": ""
    },
    "language": "*",
    "link": "http://samoylov/joomla/gsoc19_webservices/index.php",
    "metadata": {
        "hits": "",
        "rights": "",
        "robots": "",
        "tags": {
            "tags": "",
            "typeAlias": null
        }
    },
    "metadesc": "",
    "metakey": "",
    "name": "Имя",
    "ordering": 1,
    "params": {
        "feed_character_count": "",
        "feed_display_order": "",
        "newsfeed_layout": "",
        "show_feed_description": "",
        "show_feed_image": "",
        "show_item_description": ""
    },
    "published": 1
}
```

#### Лента обновлений

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/newsfeeds/feeds/{feed_id} -d
```

```json
{
    "access": 1,
    "alias": "test2",
    "catid": 5,
    "description": "",
    "link": "http://samoylov/joomla/gsoc19_webservices/index.php",
    "metadesc": "",
    "metakey": "",
    "name": "Тест"
}
```

### Категории новостных лент

1. Маршрут для категорий новостных лент: "v1/newsfeeds/categories"
2. Работа с ним аналогична категориям баннеров.

## Компонент конфиденциальности

### Запрос

#### Получить список запросов

`curl -X GET /api/index.php/v1/privacy/request`

#### Получить одиночный запрос

`curl -X GET /api/index.php/v1/privacy/request/{request_id}`

#### Получить данные экспорта отдельного запроса

`curl -X GET /api/index.php/v1/privacy/request/export/{request_id}`

#### Создать Запрос

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/privacy/request -d
```

```
{
    "email":"somenewemail@com.ua",
    "request_type":"экспорт"
}
```

### Согласие

#### Получить список согласий

`curl -X GET /api/index.php/v1/privacy/consent`

#### Получить одиночное согласие

curl -X GET /api/index.php/v1/privacy/consent/{consent_id}

#### Удалить согласие

curl -X DELETE /api/index.php/v1/privacy/consent/{consent_id}

## Компонент перенаправления

### Перенаправление

#### Получить список перенаправлений

`curl -X GET /api/index.php/v1/redirect`

#### Получить один редирект

`curl -X GET /api/index.php/v1/redirect/{redirect_id}`

#### Удалить перенаправление

curl -X DELETE /api/index.php/v1/redirect/{redirect_id}

#### Создать перенаправление

```shell
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/redirect -d
```

```
{
    "comment": "",
    "header": 301,
    "hits": 0,
    "new_url": "/content/art/99",
    "old_url": "/content/art/12",
    "published": 1,
    "referer": ""
}
```

#### Обновить перенаправление

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/redirect/{redirect_id} -d

{
    "new_url": "/content/art/4",
    "old_url": "/content/art/132"
}

## Компонент тегов

### Теги

#### Получить список тегов

`curl -X GET /api/index.php/v1/tags`

#### Получить одиночный тег

`curl -X GET /api/index.php/v1/tags/{tag_id}`

#### Удалить тег

curl -X DELETE /api/index.php/v1/tags/{tag_id}

#### Создать тег

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/tags -d
```

{
    "access": 1,
    "access_title": "Публичный",
    "alias": "тест",
    "description": "",
    "language": "*",
    "note": "",
    "parent_id": 1,
    "path": "тест",
    "published": 1,
    "title": "тест"
}

#### Обновить тег

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/tags/{tag_id} -d

```json
{
    "alias": "тест",
    "title": "новый заголовок"
}
```

## Шаблоны

### Стили шаблонов

#### Получить список стилей шаблонов

curl -X GET /api/index.php/v1/templates/styles/{app}

#### Получить единый стиль шаблона

curl -X GET /api/index.php/v1/templates/styles/{app}/{template_style_id}

#### Удалить стиль шаблона

```markdown
curl -X DELETE
/api/index.php/v1/templates/styles/{app}/{template_style_id}
```

#### Создать стиль шаблона

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app} -d

{
    "home": "0",
    "params": {
        "fluidContainer": "0",
        "logoFile": "",
        "sidebarLeftWidth": "3",
        "sidebarRightWidth": "3"
    },
    "template": "cassiopeia",
    "title": "cassiopeia - Некоторый текст"
}

#### Обновить стиль шаблона

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app}/{template_style_id} -d
```

```json
{
    "template": "cassiopeia",
    "title": "new cassiopeia - По умолчанию"
}
```

## Компонент Пользователи

### Пользователи

#### Получить список пользователей

curl -X GET /api/index.php/v1/users

#### Получить одного пользователя

`curl -X GET /api/index.php/v1/users/{user_id}`

#### Удалить пользователя

curl -X DELETE /api/index.php/v1/users/{user_id}

#### Создать пользователя

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/users -d

{
    "блок": "0",
    "электронная почта": "test@mail.com",
    "группы": [
        "2"
    ],
    "идентификатор": "0",
    "время последнего сброса": "",
    "дата последнего визита": "",
    "имя": "nnn",
    "параметры": {
        "язык администратора": "",
        "стиль администратора": "",
        "редактор": "",
        "справочный сайт": "",
        "язык": "",
        "часовой пояс": ""
    },
    "пароль": "qwertyqwerty123",
    "пароль2": "qwertyqwerty123",
    "дата регистрации": "",
    "требовать сброс": "0",
    "счетчик сбросов": "0",
    "отправить электронное письмо": "0",
    "имя пользователя": "ad"
}

#### Обновить пользователя

```markdown
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/users/{user_id} -d
```

```
{
    "email": "new@mail.com",
    "groups": [
        "2"
    ],
    "name": "имя",
    "username": "имя_пользователя"
}
```

### Поля пользователей

1. Маршрут полей пользователей: "v1/fields/users"
2. Работа с ним аналогична работе с полями контакта.

### Группы Поля Пользователи

1. Поле групп маршрутов Пользователи: "v1/fields/groups/users"
2. Работа с ним аналогична Группам Полей Контакта.

*Переведено openai.com*

