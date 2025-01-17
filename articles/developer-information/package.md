<!-- Filename: https://docs.joomla.org/Package / Display title: Пакеты -->

## Введение в пакеты

Иногда модуль (или плагин, или другое расширение) использует модели или вспомогательные инструменты компонента. В таких случаях лучше иметь возможность устанавливать или удалять зависимый модуль при установке или удалении самого компонента. В Joomla эта функциональность достигается с помощью пакета, хотя использование пакетов не является безошибочным.

Пакет — это расширение, которое используется для установки нескольких расширений одновременно. Объединение их в пакете позволяет пользователю устанавливать и удалять два или более расширений за одно действие.

## Создание пакета

Пакет создается путем объединения всех отдельных файлов расширений `.zip` вместе с манифест-файлом `.xml`. Например, для пакета, состоящего из:

* **компонент** helloworld
* **модуль** helloworld
* **библиотека** helloworld
* **системный плагин** helloworld
* **шаблон** helloworld

Пакет будет иметь следующую структуру в zip-файле:

```sh
  -- pkg_helloworld.xml
  -- packages <dir>
      |-- com_helloworld.zip
      |-- mod_helloworld.zip
      |-- lib_helloworld.zip
      |-- plg_sys_helloworld.zip
      |-- tpl_helloworld.zip
  -- pkg_script.php
```

`pkg_helloworld.xml` может содержать следующее:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<extension type="package" method="upgrade">
<name>Hello World Package</name>
<author>Hello World Package Team</author>
<creationDate>May 2022</creationDate>
<packagename>helloworld</packagename>
<version>1.0.0</version>
<url>http://www.yoururl.com/</url>
<packager>Hello World Package Team</packager>
<packagerurl>http://www.yoururl.com/</packagerurl>
<description>Example package to combine multiple extensions</description>
<update>http://www.updateurl.com/update</update>
<scriptfile>pkg_script.php</scriptfile>
<blockChildUninstall>true</blockChildUninstall>
<files folder="packages">
    <file type="component" id="com_helloworld">com_helloworld.zip</file>
    <file type="module" id="helloworld" client="site">mod_helloworld.zip</file>
    <file type="library" id="helloworld">lib_helloworld.zip</file>
    <file type="plugin" id="helloworld" group="system">plg_sys_helloworld.zip</file>
    <file type="template" id="helloworld" client="site">tpl_helloworld.zip</file>
</files>
</extension>
```

Когда пакет заархивирован и установлен, он будет виден в списке расширений, чтобы пользователь мог удалить все расширения, содержащиеся в пакете.

Помните, что лучше использовать деинсталлятор пакета вместо отдельных деинсталляторов подпакетов, чтобы избежать появления «сиротских» записей расширений в менеджере расширений. Это часть, которая не является надежной!

### Идентификатор файла

Элемент id в теге `<file ..>` **не** является произвольным! `id=` должен быть установлен в значение столбца `element` в таблице `#__extensions`. Если они заданы неправильно, при удалении пакета дочерний `file` **не** будет найден и удален.

### Имя файла манифеста и имя пакета

Именование файла манифеста и возможность удаления самого файла пакета тесно связаны. Файл манифеста должен иметь префикс **pkg_**. Остальная часть имени манифеста (без расширения **.xml**) используется как `<packagename>`. Или наоборот, пакет, который вы хотите идентифицировать как **blurpblurp_J3**, получает это как свое `<packagename>` и должен находиться в файле манифеста с именем **pkg_blurpblurp_J3.xml**. Несоблюдение этого требования сделает невозможным удаление самого пакета.

Дополнительно можно использовать `<pkg_script.php>`, который содержит класс `pkg_<packagename>InstallerScript` с публичной функцией **postflight**.

### Тег расширения

Тег `<extension>` должен включать `method="upgrade"`, чтобы обновление пакета прошло успешно при последующих установках.

### Зависимость удаления расширения

Расширение пакета может объявить, что его дочерние элементы не могут быть удалены независимо, с помощью элемента `<blockChildUninstall>true</blockChildUninstall>` в манифесте пакета. Если вашему пакету необходимо все его расширения для эффективной работы, установите для этого значение true или 1.

*Переведено openai.com*

