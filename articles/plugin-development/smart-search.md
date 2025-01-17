<!-- Filename: Creating_a_Smart_Search_plug-in / Display title: Пример: Умный поиск -->

## Введение

Плагины Smart Search находятся в группе `finder` (в plugins/finder/xxxx). Кодирование плагинов finder значительно изменилось в Joomla 4 и 5. Для создания нового плагина finder, вероятно, лучше всего скопировать существующий основной плагин и изменить его содержимое в соответствии с новой целью.

Эта статья описывает плагин поиска, разработанный для поддержки расширения *Jdocmanual*. Содержание, которое должно быть индексировано, находится в таблице `#__jdm_articles`. Код можно найти на [GitHub](https://github.com/ceford/cefjdemos-plg-finder-jdocmanual).

## Настройка IDE - VSCode или VSCodium

В моей локальной среде разработки исходный код этого расширения находится в каталоге ~/git/cefjdemos-plg-jdm-finder. Название этой папки не имеет значения; это просто мой способ упорядочивания собственных расширений. Схематическая структура папки выглядит следующим образом:

```
cefjdemos-plg-finder-jdocmanual
|-- .vscode (folder for build settings)
|-- plg_finder_jdocmanual (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_finder_jdocmanual.ini
        |-- plg_finder_jdocmanual.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Jdocmanual.php (the extension class code)
    |-- jdocmanual.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

В VSCodium это выглядит так:

![Plugin development file structure in vscodium](../../../en/images/plugins/jdocmanual-vscodium.png)

## Настройте код

Начав с основного плагина поиска, первой задачей является изменение всех ссылок на оригинальный плагин на подходящие значения для нового плагина. В данном случае есть 63 вхождения слова `jdocmanual` в 7 файлах. Есть хорошая вероятность, что некоторые будут пропущены при первом проходе, и плагин не будет работать.

## Создание расширения

Сборка состоит из двух частей. У меня есть файл build.xml, который содержит инструкции для [`phing`](https://www.phing.info/), инструмента сборки для PHP проектов. Его можно вызывать из VSCode/VSCodium или Eclipse или любой другой IDE.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="jdocmanual" basedir="." default="main">

    <property name="joomladir" value="/Users/ceford/public_html/jdm3"  override="true" />
    <property name="sitecomdir" value="${joomladir}/components" override="true" />
    <property name="admincomdir" value="${joomladir}/administrator/components" override="true" />
    <property name="adminmediadir" value="${joomladir}/media/com_jdocmanual" override="true" />
    <property name="plgdir" value="${joomladir}/plugins/finder/jdocmanual" override="true" />
    <property name="srcdir" value="${project.basedir}" override="true" />

    <property name="zipsdir" value="/Users/ceford/git/zips/cefjdemos"  override="true" />

    <!-- Fileset for plugin files -->
    <fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
    </fileset>

    <!-- fileset for zip -->
    <fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">

        <copy todir="${plgdir}">
            <fileset refid="plgfiles" />
        </copy>

        <zip destfile="${zipsdir}/plg_finder_jdocmanual.zip">
            <fileset refid="plgfiles" />
        </zip>

    </target>
</project>
```

Вторая часть - это файл `tasks.json` в папке .vscode. Он указывает VSCode, где найти файл phing.

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
      {
        "label": "Build plg_finder_jdocmanual",
        "type": "shell",
        "command": "php ~/bin/phing-latest.phar",
        "windows": {
          "command": "php ~/bin/phing-latest.phar"
        },
        "group": "build",
        "presentation": {
          "reveal": "always",
          "panel": "shared"
        }
      }
    ]
}
```

В VSCode/VScodium я выбираю **Терминал / Выполнить задачу сборки...** из строки меню и выбираю соответствующую задачу.

## Установите расширение

После сборки zip-файл расширения должен быть установлен так же, как и любое другое расширение. Его необходимо включить, а также задать все необходимые параметры. Jdocmanual имеет три варианта *Таксономии для индексации*:

- **Руководство** одно или все доступные руководства (Пользовательское, Разработчика, ...)
- **Язык** поиск ограничен статьями на том же языке, что и язык страницы.
- **Тип** один или все типы контента (Jdocmanual, Содержимое, ...)

В случае сайта Jdocmanual другие плагины поиска отключены, потому что больше нет контента, который нужно индексировать.

После первой установки обычно достаточно снова запустить задачу сборки после исправления кода. Установка zip-файла требуется только в случае изменений в файле manifest.xml.

## Индексировать контент

Jdocmanual настроен на индексацию своих статей по запросу суперпользователя. Это отличается от обычного плагина контента, который индексирует содержимое при каждом сохранении статьи. Первый запуск занимает несколько минут для индексации ~5000 статей Jdocmanual на 8 языках.

## Создание модуля умного поиска

Из **Модули контента / сайта** выберите **Новый** и установите новый модуль **Умный поиск**. Настройте параметры для получения подходящего внешнего вида.

## Тестирование

В конце концов, ваш пользовательский плагин для умного поиска должен заработать. Это пример страницы результатов для Jdocmanual, который выполняет поиск термина на этой странице. Страница с результатами опускает форму поиска из заголовка, так как она присутствует в теле страницы.

![Smart search result](../../../en/images/plugins/jdocmanual-search-result.png)

Небольшое отступление: плагин System - Joomla Accessibility Checker показывает, что в форме ввода данных *Условия поиска* есть 3 ошибки. Это требует основного исправления или переопределения.

*Переведено openai.com*

