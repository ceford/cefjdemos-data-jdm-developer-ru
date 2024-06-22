<!-- Filename: Working_with_git_and_github / Display title: Работа с git и github -->

# Вступление

Этот документ предоставит информацию о том, как сделать вклад в Joomla!
CMS с помощью Git и GitHub. Если вы хотите сделать простое изменение
(только в одном файле), то это проще сделать с помощью этой
документации: [использование пользовательского интерфейса (UI) Github
для создания pull
request](https://docs.joomla.org/Using_the_Github_UI_to_Make_Pull_Requests "Special:MyLanguage/Using the Github UI to Make Pull Requests").
Если вы хотите сделать более комплексные изменения или вам это просто
интересно, то продолжайте читать!

## Что такое Git и GitHub?

Git это распределенная система контроля версий. Эта система записывает
изменения в файл и хранит эти изменения в файле истории. Вы всегда
можете взглянуть назад на старые версии вашего кода и восстановить их
если требуется. Так как история записывается, Git очень полезен, когда
вы работаете вместе с множеством людей над одним проектом.

Here is how GitHub can be used.
<a href="https://www.github.com" class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a> is an website that helps
manage Git Projects in a visual way. As owner of a project you can
change the code and compare different versions. As a user of the project
you can contribute by making a Pull Requests. A pull request is a
request to the owner to pull some code into the project. You're offering
a piece of code (perhaps a solution for a bug) and asking if the Project
owner would like to use it. If the owner likes it, he can merge (add) it
to his project.

Joomla! использует GitHub и Git для управления кодом. Каждый может
вносить вклад в программное обеспечение Joomla! Ссылка проекта Joomla!
CMS на GitHub:
<a href="https://github.com/joomla/joomla-cms" class="external free"
target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms</a>

# Начинаем

## Регистрация на GitHub и установка Git

Сперва необходимо зарегистрироваться на
<a href="http://www.github.com" class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a>. Это бесплатно и легко
сделать. Просто следуйте предоставляемым инструкциям.

После регистрации необходимо установить Git. Последнюю версию Git можно
найти здесь
<a href="http://git-scm.com" class="external free" target="_blank"
rel="nofollow noreferrer noopener">http://git-scm.com</a>. Загрузите и
откройте установщик. Git — это программа для CLI (Command Line Interface
(интерфейс командной строки)). В начале может казаться всё запутанным
или может быть немного страшно, но этот документ проведет вас через весь
процесс.

Если вы не продвинутый пользователь, тогда просто запустите установщик и
нажимайте кнопку "next" пока программа не будет установлена. Git не
повредит вашу систему. Позже, если захотите, то сможете удалить его как
и любую другую программу.

После установки Git, запустите программу под названием "Git Bash".
Откроется командная строка. Сообщите Git своё имя (ник) и адрес
электронной почты. Git будет использовать эту информацию, когда вы
будете вносить вклад в проект.

С помощью следующих команд мы дадим эту информацию Git:

    git config --global user.name "Your name"
    git config --global user.email youremail@mail.com

В предыдущих командах и во всех следующих командах из этой документации
каждая новая строка с символами это новая команда. Так что вы вводите
первую строку с командой, нажимаете "Enter" и, затем, вводите следующую
строку и опять "Еnter".

Теперь вы готовы к использованию Git. Следуйте дальше для настройки
тестовой установки.

# Настройка тестовой инсталляции

Для нашей тестовой установки нужен веб-сервер, который позволит вам
установить и запустить Joomla! на вашем компьютере. Существуют множество
программ, которые выполняют функции
<a href="https://www.mamp.info/" class="external text" target="_blank"
rel="nofollow noreferrer noopener">MAMP</a> и
<a href="https://www.apachefriends.org/" class="external text"
target="_blank" rel="nofollow noreferrer noopener">XAMPP</a>. Загрузите
и установите одну из них.

After the installation of such a program (I use MAMP in this
documentation), we going to install the latest version of Joomla!. In
our case, the latest version of Joomla! is not the last stable release.
The last version of Joomla! is the staging branch on GitHub. First, let
me explain a little bit more about GitHub.

## Fork and Clone Joomla!

На GitHub вы можете найти проекты, так называемые репозитории
(repositories). Внутри проекта вы можете найти несколько версий. Такие
версии называются ветками (branch). В Joomla! есть следующие ветки:

- **Staging:** эта ветка содержит последние исправления ошибок и новые
  возможности Joomla!
- **Master:** эта ветка содержит текущую стабильную версию Joomla!
- **3.5-dev** эта ветка содержит файлы Joomla! 3.5, которая не стабильна
  на момент написания статьи.

On our test location we going to use the **Staging** branch but if we
should use this branch directly we have problem. We cannot modify this
branch because we are not the owner of it. We are going to make a copy
of it. On GitHub this is called a Fork. We are the owner of that copy so
we can modify it. After modifying we compare our fork with the original
project. Then we can make a Pull Request for the changes we've made.
More about that later. You can Fork a branch by pressing the Fork button
on the
<a href="https://github.com/joomla/joomla-cms" class="external text"
target="_blank" rel="nofollow noreferrer noopener">Joomla! CMS Github
Repository</a>. This button is located at the right top of the page.

<img src="https://docs.joomla.org/images/6/6c/Github-fork-button.png"
decoding="async" data-file-width="1002" data-file-height="222"
width="1002" height="222" alt="Github-fork-button.png" />

After forking, we are going to install Joomla! on our local Web server.
Go to the folder were you can run files on you Web server. Most of the
program use a folder called `htdocs`. Once we are inside that folder,
press the right mouse button an click on: "Git Bash Here". The command
line will open for this location.

Type the following command to download the files from your Fork of the
Joomla! CMS to your computer. Please replace *username* with the
username you are using on GitHub.

    git clone https://github.com/username/joomla-cms.git

For all commands given in this documentation, you have to open Git via
the above described steps. Please remember that for other commands in
this documentation.

Once Git is ready, open your browser and go to the installation on your
localhost. Usually the URL is something like:
<a href="http://localhost/joomla-cms" class="external free"
target="_blank"
rel="nofollow noreferrer noopener"><code>http://localhost/joomla-cms</code></a>.
You will now see the default Joomla! installation process.

## Установка Joomla!

Установка Joomla! для нашей локальной тестовой установки в основном
такая же как и обычная установка. Есть только два небольших отличия.

В настройках базы данных имя пользователя и пароль по умолчанию. Чаще
всего имя пользователя `root`, а пароль или также `root` или нет пароля.
Если вы все еще не можете подключится к базе данных, то поищите имя
пользователя и пароль в руководство для используемого веб-сервера.

Последнее отличие в последнем шаге установки. Обычно мы должны удалить
установочную папку чтобы перейти к back-end (административной части) или
front-end (внешнему интерфейсу веб-сайта) Joomla! Для тестовой установки
мы можем пропустить эту часть и перейти непосредственно к back-end или
front-end. Не удаляйте установочную папку. Она может быть очень полезной
когда вы будете устанавливать Joomla снова.

# Делаем свои изменения

Теперь настало время сделать свои изменения в Joomla! Все изменения,
которые вы делаете, будут сохранятся в Git. В любой момент вы можете
ввести команду `git status`, чтобы увидеть какие файлы изменены или не
отслеживаются. Не отслеживается значит файл в данном месте новые для
Git.

Если вы сделали ошибку или хотите восстановить файл, используйте
следующую команду:

    git checkout path/to/file

Если вы хотите удалить все изменения которые вы сделали, используйте
следующие команды:

    git checkout .
    git clean -f -d

Первая команда сбрасывает все файлы. Вторая команда удаляет
неотслеживаемые файлы и папки.

## Публикация своих изменений на GitHub

# Push the Change to the Fork

After making our changes, we have to upload our changes to our
repository on GitHub. After that, we can make a pull request with our
changes.

Uploading changes is called `push` in Git terms. Before we can do that,
we have to do something very important. We must create a new branch for
our changes. (A branch is a version of our project, remember?) If we
don't do that and made our change directly to the staging branch, the
first time there won't be a problem. But if we made changes for the
second time, and the change we made the first time are not merged yet,
all these changes will registered too as new changes.

So the first command we are going to run will create a new branch. It
will prevent the above described problem. Replace name-new-branch with
the name of the new branch. This name must be short, and can only
contain lower case letters and numbers. Do **NOT** use spaces. Instead
of spaces, use - (minus).

    git checkout -b name-new-branch

The next command tells git that all made changes are good, and are ready
to commit.

    git add --all

The following command adds our change to the branch. Please replace the
message with a short description of your changes. This description will
be the title of the pull request we are going to make.

    git commit -m "description"

The final command will push (upload) the changes to our fork. Please
replace name-new-branch with the name of the branch you made a few steps
above.

    git push origin name-new-branch

## Compare Forks and Make a Pull Request

After pushing our change to GitHub, go to your fork of the Joomla! CMS.

# Дополнительная информация

Так как staging версия Joomla! может изменится в любой момент, будет
полезно иметь возможность держать ваш форк в обновленном состоянии.
Можно сделать это добавив для своего проекта удаленный репозиторий:

    git remote add upstream https://github.com/joomla/joomla-cms.git

Теперь мы добавили удаленный репозиторий под названием "upstream".
Используя следующую команду мы скажем Git искать новые изменения
(коммиты) в ветке staging, которых еще нет в вашем форке. Если он найдет
какие-либо, он добавит их в ваш форк:

    git pull upstream staging

На данный момент изменения сделаны только в вашем локальном форке.
Используйте следующую команду, чтобы загрузить их на GitHub:

    git push
