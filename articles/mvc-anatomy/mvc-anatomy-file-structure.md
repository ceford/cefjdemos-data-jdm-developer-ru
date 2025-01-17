<!-- Filename: J4.x:MVC_Anatomy:_File_Structure / Display title: Анатомия MVC: Структура файлов -->

## Настройка для разработчиков

Существует несколько способов организации файлов для целей разработки. Один из методов — использовать клон репозитория GitHub, как показано в следующей схематической структуре:

```
cefjdemos-com-countrybase
|-- .vscode (folder for build settings)
|-- com_countrybase (folder compressed to create an installable zip file)
    |-- admin (the administrator files)
        |-- forms
        |-- help
            |-- countrybase.html (this is a Help screen used in the backend module edit form)
        |-- language
            |-- en-GB (language folder, kept with the extension code)
                |-- mod_downmsg.ini
                |-- mod_downmsg.sys.ini
        |-- services (folder for dependency injection)
            |-- provider.php (the DI code)
        |-- sql
            |-- updates
                |-- mysql
                    |-- index.html (an empty file required to avoid an installation error if there are no sql updates)
        |-- src (folder for namespaced classes)
            |-- more folders: Controller, Extension, Helper, Model, Table and View
        |-- tmpl (folder for layouts)
            |-- more folders: countries, country, currencies and currency
        |-- access.xml (standard list of access permissions)
        |-- config.xml (options parameters)
    |-- media
        |-- css (placeholder for CSS files - contains an empty index.html file)
        |-- js (placeholder for JavaScript files - contains an empty index.html file)
    |-- site (the site files, abbreviated here)
        |-- forms
        |-- language
        |-- src
        |-- tmpl
    |-- countrybase.xml (the manifest file)
    |-- script.php (a script to run on install, update or uninstall)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

Это структура в IDE VSCodium:

![Vscodium file structure view](../../../en/images/mvc-anatomy/com-countrybase-vscodium.png)

При установке части компонента `com_countrybase` распределяются по различным местам в структуре файлов Joomla:
- Файлы администратора помещаются в `root/administrator/components/com_countrybase`.
- Файлы сайта помещаются в `root/components/com_countrybase`.
- Медиафайлы, файлы javascript и css (если есть) помещаются в `root/media/com_countrybase`.
- Языковые файлы помещаются в структуры компонентов администратора и сайта. Ранее они размещались в основных языковых папках.

Где именно размещаются элементы, контролируется файлом манифеста компонента, в данном случае countrybase.xml.

Обратите внимание, что zip-файл содержит все внутри папки com_countrybase. Вы можете создать zip просто, сжав эту папку. За пределами этой папки находятся build.xml, который является файлом, используемым для сборки компонента при внесении изменений, и README.md, который является стандартным Markdown-файлом в формате Github, описывающим компонент.

## Заметки

- В этом простом компоненте более 40 файлов!
- При установке файл countrybase.xml копируется в administrator/components/com_countrybase, где он необходим для целей обновления или удаления.

*Переведено openai.com*

