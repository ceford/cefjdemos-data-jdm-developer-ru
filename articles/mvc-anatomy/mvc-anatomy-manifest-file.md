<!-- Filename: J4.x:MVC_Anatomy:_Manifest_File / Display title: MVC анатомия: Файл манифеста -->

## Метаданные

Файл манифеста компонента должен называться manifest.xml или componentname.xml, в данном случае countrybase.xml. Обратите внимание, что часть com_ не включена. Первая часть файла содержит метаданные:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
    <name>com_countrybase</name>
    <author>Clifford E Ford</author>
    <authorEmail>john.doe@example.com</authorEmail>
    <authorUrl>example.com</authorUrl>
    <creationDate>2025-01-02</creationDate>
    <copyright>(C) 2025 Clifford E Ford</copyright>
    <license>GNU General Public License version 3 or later; see LICENSE.txt</license>
    <version>0.0.1</version>
    <description>COM_COUNTRYBASE_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Component\Countrybase</namespace>
```

Большая часть метаданных очевидна. Стоит отметить следующее:

- Тип расширения в этом случае — **компонент**. Существуют и другие типы расширений, такие как модуль или плагин.
- Метод может быть install или upgrade. Последний позволяет повторную установку после обновления кода.
- Номер версии важен! Он должен сделать невозможной переустановку более старой версии. Также он используется для контроля необходимости обновления базы данных.
- Путь пространства имен src состоит из трех частей:
  - Первая часть — это определенный поставщиком префикс, например, Joomla для кода, предоставляемого Joomla, Acme для кода, предоставляемого сторонним поставщиком расширений Acme, и в этом случае Cefjdemos, имя, выбранное для демонстрации кода Joomla.
  - Вторая часть указывает тип расширения. Это может быть Компонент или Модуль или Плагин и т. д.
  - Третья часть — это конкретное имя расширения, в данном случае Countrybase.

## База данных

Файл манифеста определяет расположение любых sql файлов для установки, обновления или удаления. Они находятся в папке sql в папке администратора.

```xml
    <install>
        <sql>
            <file driver="mysql" charset="utf8">sql/install.mysql.sql</file>
        </sql>
    </install>
    <uninstall>
        <sql>
            <file driver="mysql" charset="utf8">sql/uninstall.mysql.sql</file>
        </sql>
    </uninstall>
    <update>
        <schemas>
            <schemapath type="mysql">sql/updates/mysql</schemapath>
        </schemas>
    </update>
```

## Файл скрипта

Сценарий может использоваться для целей предварительного или последующего выполнения. Например, он может использоваться для проверки минимальных системных требований перед установкой или для установки параметров после установки.

## Медиафайлы

Компонент com_countrybase не требует какого-либо специфичного css или javascript, но код для их установки включен на всякий случай. Папки css и js содержат только пустые файлы index.html. Без этих файлов папки не создаются.

```xml
    <scriptfile>script.php</scriptfile>

    <media destination="com_countrybase" folder="media">
        <file>joomla.asset.json</file>
        <folder>css</folder>
        <folder>js</folder>
    </media>
```

Обратите внимание, что файл joomla.asset.json используется менеджером веб-активов, обычно вызываемым из файла шаблона.

## Файлы сайта

Это файлы, которые копируются в root/components/com_countrybase. Обратите внимание, что папка языка копируется в папку компонента, а не в папку root/language.

```xml
    <files folder="site">
        <folder>language</folder>
        <folder>forms</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
```

## Администраторские файлы

В части манифестного файла, относящейся к администрированию, больше файлов, потому что ей нужно выполнять больше задач:

```xml
    <administration>
        <files folder="admin">
            <filename>access.xml</filename>
            <filename>config.xml</filename>
            <folder>forms</folder>
            <folder>help</folder>
            <folder>language</folder>
            <folder>layouts</folder>
            <folder>services</folder>
            <folder>sql</folder>
            <folder>src</folder>
            <folder>tmpl</folder>
        </files>
        <menu img="class:default">countrybase</menu>
        <submenu>
            <!--
                Note that all & must be escaped to &amp; for the file to be valid
                XML and be parsed by the installer
            -->
            <menu
                link="option=com_countrybase&amp;view=countries"
                img="default"
            >
                COM_COUNTRYBASE_COUNTRIES
            </menu>
            <menu
                link="option=com_countrybase&amp;view=currencies"
                img="default"
            >
                COM_COUNTRYBASE_CURRENCIES
            </menu>
        </submenu>
    </administration>
```

Обратите внимание на метод создания пунктов меню администратора Joomla. Есть пункт меню, который не имеет ссылки. И два пункта меню, которые ссылаются на два доступных представления администрирования: список стран и список валют. Также переводы строковых ключей должны находиться в файле com_countrybase.sys.ini.

## Журнал изменений

Журнал изменений используется для ведения учета изменений с каждой версией расширения. См. статью [Changelogs](jdocmanual?article=docus/install-update/installation-change-log) для получения информации о содержании журнала изменений.

```xml
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-com-countrybase/master/changelog.xml</changelogurl>
```

## Обновить сервер

Код сервера обновлений сообщает Joomla, где найти информацию об обновлениях. Он выполняется через регулярные промежутки времени, чтобы проверить, доступно ли обновление. Пропустите этот раздел, если вы не используете сервер обновлений для вашего собственного компонента.

```xml
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" name="Countrybase Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-com-countrybase/master/updates.xml</server>
    </updateservers>
```

*Переведено openai.com*

