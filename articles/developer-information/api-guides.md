<!-- Filename: API_Guides / Display title: Руководства по API -->

Эта страница содержит указатель на набор руководств по API Joomla. Эти руководства предназначены для того, чтобы помочь вам понять, как использовать эти функции Joomla в ваших собственных расширениях Joomla.

Каждое руководство по API включает в себя пример кода, который вы можете скопировать и установить в свою собственную среду разработки. Обычно эти примеры кода написаны для включения и установки в качестве модуля Joomla, поэтому, если вы не знакомы с разработкой модулей Joomla, вам может быть полезно пройти через короткую серию [Создание простого модуля](https://docs.joomla.org/Creating_a_simple_module).

Вокруг Joomla 3.8 команда разработчиков Joomla начала изменять соглашение об именовании классов Joomla для использования пространств имен, так что, например, JFactory изменился на Factory в пространстве имен Joomla\CMS. Когда вы читаете существующий код и документацию Joomla, вы можете обнаружить, что классы следуют либо новому, либо старому стандарту именования. Вы можете найти соответствие между двумя соглашениями об именовании в файле `libraries/classmap.php` в вашем экземпляре Joomla.

- Базовые классы приложения и их иерархия и назначение описаны в [Understanding the Application classes](https://docs.joomla.org/J3.x:Understanding_the_Application_classes)
- Обработка Ajax в компонентах Joomla описана в [JSON Responses with JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson "JSON Responses with JResponseJson"). Ajax также может использоваться внутри модулей и плагинов Joomla, как описано в [Using Joomla Ajax Interface](https://docs.joomla.org/Using_Joomla_Ajax_Interface).
- [Кэш](https://docs.joomla.org/Cache_Basic_API_Guide) - как использовать кэш обратного вызова в вашем коде.
- [Категории](https://docs.joomla.org/Categories_and_CategoryNodes_API_Guide) Использование классов `Categories` и `CategoryNode` для доступа к данным, связанным с категориями Joomla.
- CSS можно добавить, как описано в [Adding JavaScript and CSS to the page](https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page)
- База данных / JDatabase. Существуют два руководства по API, охватывающие [Выбор данных с помощью JDatabase](https://docs.joomla.org/Selecting_data_using_JDatabase)
  и [Вставка, обновление и удаление данных с помощью JDatabase](https://docs.joomla.org/Inserting,_Updating_and_Removing_data_using_JDatabase)
- [Дата/JDate](https://docs.joomla.org/How_to_use_JDate) это класс даты Joomla.
- Файлы и папки. См. [Как использовать пакет файловой системы](https://docs.joomla.org/How_to_use_the_filesystem_package).
- Форма / JForm. Существует [Основное руководство](https://docs.joomla.org/Basic_form_guide "Basic form guide") по использованию API форм Joomla и интеграции форм в компонент Joomla, а также более [Продвинутое руководство по формам](https://docs.joomla.org/Advanced_form_guide), охватывающее более сложные аспекты API.
- FormField / JFormField. Этот класс и связанные с ним классы, такие как JFormFieldList, которые наследуются от него, в первую очередь полезны для определения пользовательских полей формы, как описано в [Создание типа пользовательского поля формы](https://docs.joomla.org/Creating_a_custom_form_field_type).
- [Ввод / JInput](https://docs.joomla.org/Retrieving_request_data_using_JInput) Использование класса `Input` для получения значений параметров в HTTP-запросах GET и POST
- JavaScript можно добавить, как описано в [Adding JavaScript and CSS to the page](https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page)
- Использование макетов Joomla описано в [Sharing layouts across views or extensions with JLayout](https://docs.joomla.org/J3.x:Sharing_layouts_across_views_or_extensions_with_JLayout "J3.x:Sharing layouts across views or extensions with JLayout"). Гибкость была увеличена в Joomla 3.2, как описано в [Улучшения JLayout для Joomla!](https://docs.joomla.org/J3.x:JLayout_Improvements_for_Joomla!).
- [Лог / JLog](https://docs.joomla.org/Using_JLog) Записывает сообщения (например, сообщения об ошибках, отладочные сообщения) в файл журнала и, при необходимости, в отладочную консоль
- [Меню и элементы меню](https://docs.joomla.org/Menu_and_Menuitems_API_Guide)
- [Вложенные наборы](https://docs.joomla.org/Using_nested_sets), которые позволяют реализовать иерархию деревьев в таблице базы данных, используются меню Joomla, статьями, категориями и т.д.
- [Реестр/JRegistry](https://github.com/joomla-framework/registry) это класс утилиты, который очень полезен для обработки массивов PHP, преобразования в JSON и т.д.
- [JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson) поддерживает ответы в формате JSON на Ajax-запросы.
- Маршрут / JRoute см. [URL в Joomla](https://docs.joomla.org/URLs_in_Joomla)
- Таблица / JTable предоставляет возможности для выполнения операций CRUD (и более) с таблицами базы данных. Руководство разделено на [Основное руководство по API](https://docs.joomla.org/Table_Basic_API_Guide)
  и [Продвинутое руководство по API](https://docs.joomla.org/Table_Advanced_API_Guide)
- [Контроллеры](https://docs.joomla.org/Controllers) (BaseController, AdminController, FormController, ApiController) несут ответственность за анализ запроса пользователя, проверку разрешения пользователя на выполнение этого действия и определение способа удовлетворения запроса.
- [Модели](https://docs.joomla.org/Models) (BaseModel, BaseDatabaseModel, ItemModel, ListModel, FormModel, AdminModel) инкапсулируют данные, используемые компонентом. Они также отвечают за обновление базы данных, если это необходимо.
- [Представления](https://docs.joomla.org/Views) (AbstractView, CategoriesView, CategoryFeedView, CategoryView, FormView, HtmlView, JsonApiView, JsonView, ListView) определяют, что должно появиться на веб-странице, и собирать все данные, необходимые для вывода HTTP-ответа.
- [Теги](https://docs.joomla.org/Tags_API_Guide).
- Uri / JUri смотрите [URL в Joomla](https://docs.joomla.org/URLs_in_Joomla)
- [Пользователь / JUser](https://docs.joomla.org/Accessing_the_current_user_object) API, связанный с пользователем Joomla.

*Переведено openai.com*

