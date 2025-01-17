<!-- Filename: Adding_changelog_to_your_manifest_file / Display title: Добавление журнала изменений -->

Начиная с Joomla 4.0, разработчики расширений могут использовать возможность Joomla считывать файл changelog и предоставлять визуальное представление журнала изменений. Если данная версия не найдена в журнале изменений, кнопка changelog не будет отображаться.

Изменения в выпуске будут представлены следующим образом:

![changelog modal view](../../../en/images/developer-information/adding-changelog-example-1.png)

Журнал изменений используется в двух разных местах.

## Обновить вид

Установщик покажет список изменений версии, которую можно установить, если он доступен.

![changelog installer view](../../../en/images/developer-information/adding-changelog-update-view.png)

Нажатие кнопки Changelog здесь покажет список изменений новой доступной версии.

## Управление представлением

Менеджер расширений покажет список изменений для текущего установленного расширения, если он доступен.

![changelog installer view](../../../en/images/developer-information/adding-changelog-extension-view.png)

Нажатие на номер версии здесь покажет список изменений текущей установленной версии.

## Добавьте тег changelogurl в файлы манифеста

Первым шагом является обновление файлов манифеста, которые сообщают Joomla, где найти детали журнала изменений. Добавьте следующий узел в свои XML-файлы манифеста:

```
<changelogurl>https://example.com/updates/changelog.xml</changelogurl>
```

Пожалуйста, обратите внимание: URL в теге changelogurl не должен иметь пробелов или разрывов строк до или после него. См. примеры кода.

### Обновить манифест сервера

Посмотрите этот пример для файла манифеста сервера обновлений, который информирует Joomla об обновлении компонента с именем "com_lists". Таким образом, вы увидите кнопку "Changelog" в окне обновления.

```
<?xml version="1.0" encoding="utf-8"?>
<updates>
 <update>
  <name>Student List</name>
  <description>List of students</description>
  <element>com_lists</element>
  <type>component</type>
  <version>4.0.0</version>

  <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

  <tags>
   <tag>stable</tag>
  </tags>
  <maintainer>Example Miller</maintainer>
  <maintainerurl>https://example.com/</maintainerurl>
  <section>Updates</section>
  <targetplatform name="joomla" version="4.?" />
  <client>1</client>
  <folder></folder>
 </update>
</updates>
```

### Манифест расширения

Кроме того, добавьте тег changelogurl в XML-файл манифеста расширения. Таким образом, версия расширения будет связана с журналами изменений в режиме управления.

```
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
    <name>COM_LISTS</name>

... Other stuff ...

    <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

    <updateservers>
        <server type="extension" name="My Extension's Updates">https://example.com/lists-updates.xml</server>
    </updateservers>
</extension>
```

## Создать файл описания изменений

Файл журнала изменений должен содержать следующие 3 узла:

* элемент
* тип
* версия

Эта информация используется для определения правильного журнала изменений для данного расширения.

Узел версии внутри любого узла журнала изменений всегда обязателен. В противном случае вы увидите сообщение об ошибке, такое как SyntaxError: JSON.parse: неожиданная символ на строке 1 столбце 1 в данных JSON.

```
<element>com_lists</element>
<type>component</type>
<version>4.0.0</version>
```

Далее журнал изменений заполняется одним или несколькими типами изменений. Поддерживаются следующие типы изменений:

* безопасность: Любые исправленные проблемы с безопасностью
* исправление: Любые исправленные ошибки
* язык: Это для изменений языка
* добавление: Любые добавленные новые функции
* изменение: Любые изменения
* удаление: Любые удаленные функции
* примечание: Любая дополнительная информация для пользователя

Каждый узел может повторяться столько раз, сколько нужно.

Формат текста может быть простым текстом или HTML, но в случае HTML он должен быть заключен в теги CDATA, как показано в примере.

```
<changelogs>
    <changelog>
        <element>com_lists</element>
        <type>component</type>
        <version>4.0.0</version>
        <security>
            <item>Item A</item>
            <item><![CDATA[<h2>You MUST replace this file</h2>]]></item>
        </security>
        <fix>
            <item>Item A</item>
            <item>Item b</item>
        </fix>
        <language>
            <item>Item A</item>
            <item>Item b</item>
        </language>
        <addition>
            <item>Item A</item>
            <item>Item b</item>
        </addition>
        <change>
            <item>Item A</item>
            <item>Item b</item>
        </change>
        <remove>
            <item>Item A</item>
            <item>Item b</item>
        </remove>
        <note>
            <item>Item A</item>
            <item>Item b</item>
        </note>
</changelog>
<changelog>
    <element>com_lists</element>
    <type>component</type>
    <version>0.0.2</version>
    <security>
        <item>Big issue</item>
    </security>
</changelog>
</changelogs>
```

Этот файл содержит 2 журнала изменений:

* Версия 0.0.2 (для тестирования представления управления)
* Версия 4.0.0 (для тестирования представления обновления)

Журнал изменений может иметь столько версий, сколько нужно.

*Переведено openai.com*

