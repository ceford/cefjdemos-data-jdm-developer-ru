<!-- Filename: XAMPP / Display title: XAMPP -->

## Введение

XAMPP — это простой в установке пакет, который объединяет веб-сервер Apache, PHP, XDEBUG и базу данных MySQL. Это позволяет создать среду, необходимую для запуска Joomla! на вашем локальном компьютере. Последняя версия XAMPP доступна на <a href="http://www.apachefriends.org/en/index.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">сайте XAMPP</a>. Доступны загрузки для Linux, Windows, Mac OS X и Solaris. Скачайте пакет для вашей платформы.

*Важное примечание относительно XAMPP и Skype:* Apache и Skype оба используют порт 80 в качестве альтернативного для входящих соединений. Если вы используете Skype, зайдите в панель Инструменты-Параметры-Расширенные-Соединение и снимите галочку с опции "Использовать 80 и 443 как альтернативные для входящих соединений". Если Apache запускается как служба, он займет порт 80 до запуска Skype, и вы не столкнетесь с проблемой. Но, чтобы быть уверенным, отключите эту опцию в Skype.

### Установка на Windows

Установка для Windows очень проста. Вы можете использовать исполняемый файл установщика XAMPP (например, "xampp-windows-x64-7.4.4-0-VC15-installer.exe"). Подробные инструкции по установке для Windows доступны <a href="https://www.apachefriends.org/download.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">здесь</a>.

Если вы используете Windows XP или 2003, они не поддерживаются основным пакетом, но на странице загрузки перечислены совместимые версии XAMPP для этих платформ (однако вы сможете использовать только PHP 5.4 или более ранние версии - и, следовательно, сможете тестировать только Joomla 3.x и более ранние версии).

Для Windows рекомендуется устанавливать XAMPP в "c:\xampp" (а не в "c:\program files"). Если вы сделаете это, ваша Joomla! (и любые другие локальные папки веб-сайтов) окажутся в папке "c:\xampp\htdocs". (По соглашению, весь веб-контент размещается в папке "htdocs".)

Если у вас есть несколько HTTP-серверов (например, IIS), вы можете изменить порт прослушивания xampp. В \apache\conf\httpd.conf измените строку Listen 80 на Listen \[portnumber\] (например, "Listen 8080").

Учебник журнала сообщества Joomla

Вы можете найти подробное руководство по установке XAMPP на Windows вместе с Joomla 4 Beta, Joomla Patch Tester и Git в этой <a href="https://magazine.joomla.org/all-issues/june-2020/github-installing-git" class="external text" target="_blank" rel="noreferrer noopener">статье Joomla Community Magazine</a>.

### Установка на Linux

#### Установка XAMPP

Откройте Терминал и введите:

```bash
    sudo tar xvfz xampp-linux-1.7.7.tar.gz -C /opt
```

(замените *xampp-linux-1.7.7.tar.gz* на версию xammp, которую вы загрузили). Сообщается, что база данных MYSQL xampp 1.7.4 не работает с Joomla 1.5.22.

Устанавливает ... Apache2, mysql и php5, а также ftp-сервер.

```bash
    sudo /opt/lampp/lampp start
```

и

```bash
    sudo /opt/lampp/lampp stop
```

запускает/останавливает все сервисы

#### Тестирование вашего локального сервера XAMPP

Откройте браузер и перейдите на

```bash
    http://localhost
```

index.php будет перенаправлять на

```bash
    http://localhost/xampp
```

Там вы найдете инструкции о том, как изменить стандартные имена пользователей/пароли. На ПК, который не предоставляет файлы в Интернет или локальную сеть, изменение стандартных параметров является личным решением.

#### Загрузить Joomla

Скачайте последний zip-файл установки Joomla
<a href="https://www.joomla.org/download.html" class="external autonumber" target="_blank" rel="noreferrer noopener">[1]</a>

Распакуйте на жесткий диск

Подключитесь к localhost с помощью FTP-клиента по умолчанию.

```
    nobody
    lampp
```

Создайте папку для вашего Joomla на локальном сервере

Передайте по FTP распакованные установочные файлы Joomla в вновь созданную папку Joomla.

**Важно:**

- Установка xampp устанавливает правильные права собственности на файлы и разрешения.
- Использование **команды CHOWN** приведет к **проблемам с правами собственности в xampp**.
- **Использование nautilus** для управления папками/файлами на локальном хосте приведет к **проблемам с правами собственности в xampp**.

**Информация о базе данных**

- Хост: localhost
- Имя базы данных по умолчанию: test
- Пользователь базы данных по умолчанию: root
- **Нет** пароля по умолчанию.

Пароль администратора выбираете вы.

Установка примерных данных рекомендуется для начинающего пользователя.

После установки удалите каталог установки и укажите ваш браузер на:

```
    http://localhost/yournewjoomlafolder
```

Пожалуйста, переведите следующий текст Markdown с английского на русский:

```
    http://localhost/yournewjoomlafolder/administrator
```

#### Создание ссылки в меню Ubuntu

**Чтобы создать графический интерфейс для xammp, подключенного к вашему меню Ubuntu**

Откройте Терминал и введите

```bash
    sudo gedit /usr/share/applications/xampp-control-panel.desktop
```

Затем скопируйте следующее в gedit и сохраните.

```bash
[Desktop Entry]
Encoding=UTF-8
Name=XAMPP Control Panel
Comment=Start and Stop XAMPP
Exec=gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"
Icon=/usr/share/icons/Tango/scalable/devices/network-wired.svg
Terminal=false
Type=Application
Categories=GNOME;Application;Network;
StartupNotify=true
```

Если панель управления не запускается, попробуйте выполнить команду Exec непосредственно в терминале:

```bash
    gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"
```

Если вы получаете ошибку:

```bash
    Error importing pygtk2 and pygtk2-libglade
```

Установите недостающие библиотеки:

```bash
    sudo apt-get install python-glade2
```

#### Отладчик PHP XDebug

Пакет XAMPP для Linux не включает отладчик PHP XDebug.  
Чтобы установить XDebug на Debian или Ubuntu:

- Установите пакет *build-essential*:
```bash
    sudo apt-get update
    sudo apt-get install build-essential
    sudo apt-get install autoconf
```

- Скачайте <a href="http://www.apachefriends.org/en/xampp-linux.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">пакет для разработки</a> для вашей версии XAMPP и извлеките его поверх вашей существующей установки:
```bash
    sudo tar xvfz xampp-linux-devel-1.7.7.tar.gz -C /opt 
```

- Соберите XDebug:
```bash
    wget http://xdebug.org/files/xdebug-2.1.3.tgz
    tar xzf xdebug-2.1.3.tgz
    cd xdebug-2.1.3/
    /opt/lampp/bin/phpize
```

После этого у вас будет следующий вывод на вашей консоли…

```bash
    Configuring for:
    PHP Api Version:         20090626
    Zend Module Api No:      20090626
    Zend Extension Api No:   20090626 

    ./configure --with-php-config=/opt/lampp/bin/php-config
    make
    sudo make install 
```

Затем вывод будет следующим... пожалуйста, следите за указанной директорией.

```bash
    Installing shared extensions:     /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/ 
```

Создайте папку во временной директории, которая будет содержать файл данных, сгенерированный XDebug:

```bash
    sudo mkdir /opt/lampp/tmp/xdebug
    sudo chmod a+rwx -R /opt/lampp/tmp/xdebug 
```

Альтернативные установки:

Установить, используя библиотеку расширений PHP (PECL), входящую в комплект с xampp:

```bash
    sudo /opt/lampp/bin/pecl install xdebug
```

На Ubuntu/Debian вы можете установить, используя:

```bash
    apt-get install php5-xdebug 
```

(предупреждение: это также установит Apache и PHP из репозиториев apt).

**Предупреждение для пользователей 64-битных систем**

При компиляции XDebug или установке через apt-get вы получите
ошибку при (пере)запуске xampp:

```bash
    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/xdebug.so: wrong ELF class: ELFCLASS64
```

Это потому, что xampp работает на 32-битной версии, а XDebug - на 64-битной. Чтобы устранить эту проблему, либо скомпилируйте xdebug.so на 32-битной машине, либо скачайте его с:

```bash
    http://code.activestate.com/komodo/remotedebugging/
```

Скачайте файл: "PHP Remote Debugging Client" для "Linux (x86)".  
Извлеките содержимое файла на свой компьютер, этот сжатый файл содержит несколько папок с номерами версий, например: 4.4, 5.0, 5.1 ... 5.3 и так далее, войдите в папку с более высокой версией или ту, которая вам подходит, затем вручную скопируйте файл "xdebug.so" в следующее местоположение, при необходимости перезапишите.

```bash
    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/
```

Помните, что это местоположение может отличаться на вашем компьютере.

### Установка на Mac OS X

Mac OS X на самом деле включает сервер Apache из коробки, но большинство разработчиков предпочтет использовать интегрированные инструменты и настраиваемость, предоставляемые XAMPP.

Как и в большинстве программ на Mac, установка проходит легко. Посетите <a href="https://www.apachefriends.org/en/download.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">Apache Friends - Mac OS X</a> для загрузки универсального бинарного файла.

После завершения загрузки файла просто откройте образ диска и перетащите папку XAMPP в ярлык папки «Программы».

Чтобы запустить сервер, откройте "XAMPP Control.app" и нажмите кнопку запуска рядом с Apache.

##### Немного устранения неполадок

Многие пользователи Mac испытывают небольшие трудности на этом этапе, пытаясь настроить другой экземпляр Apache на своем компьютере. Если вы не можете запустить Apache в XAMPP, у вас есть два варианта:  
**Вы можете изменить порт прослушивания XAMPP.** В файле \Applications\XAMPP\xamppfiles\etc\httpd.conf измените строку, которая говорит "Listen 80", на Listen \[номерПорта\]. Например:

```bash
    Listen 8080
```

**Вы можете изменить порт прослушивания предварительно установленного сервера Apache.** В Finder перейдите в "/etc" (CMD+SHIFT+G); отсюда вы сможете перейти к обычно скрытым файлам Apache. Найдите папку под названием Apache2 и отредактируйте файл "http.conf". Измените строку "Listen 80" на Listen \[portNumber\]. Например:

```bash
    Listen 8080
```

*Примечание: если вы решите изменить порт предустановленного сервера Apache, вам, возможно, потребуется перезагрузить компьютер, чтобы изменения вступили в силу. Вам также потребуется аутентификация в качестве администратора, чтобы изменить эти файлы.*

### Тестирование установки XAMPP

После установки XAMPP и запуска службы Apache с помощью инструмента XAMPP Control Panel, вы можете протестировать его, открыв браузер и перейдя по адресу "<a href="http://localhost" class="external free" target="_blank" rel="nofollow noreferrer noopener">http://localhost</a>". Вы должны увидеть приветственный экран XAMPP, похожий на приведенный ниже.

Запустите `phpinfo()` на вашем локальном сервере XAMPP, чтобы отобразить информацию о конфигурации PHP.

Выберите ссылку под названием "phpinfo()" в верхнем меню. Это отобразит длинный экран с информацией о конфигурации PHP, как показано ниже.

<img
src="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/800px-Phpinfo.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/1200px-Phpinfo.png 1.5x, https://docs.joomla.org/images/d/db/Phpinfo.png 2x"
data-file-width="1432" data-file-height="1282" width="800" height="716"
alt="Phpinfo.png" />

На этом этапе XAMPP установлен успешно. Обратите внимание на "Загруженный файл конфигурации". В следующем разделе мы будем редактировать этот файл для настройки XDebug.

*Переведено openai.com*

