<!-- Filename: J4.x:Developer:_Required_Software / Display title: Настройка базы данных -->

## О MySQL и MariaDB

Новички могут иметь впечатление, что MySQL и MariaDB — это базы данных, и могут быть сбиты с толку, когда инструкции говорят *создайте новую базу данных*. На самом деле, оба являются системами управления реляционными базами данных (RDBMS), которые могут управлять множеством отдельных баз данных. Ваш локальный тестовый сайт может иметь множество отдельных установок Joomla, каждая из которых имеет свою собственную базу данных. Например, вы можете захотеть протестировать ваше расширение в текущем выпуске Joomla и отдельно в предстоящем кандидате на релиз.

На следующем скриншоте показана часть списка из более чем 30 баз данных, созданных для тестирования различных установок и проектов расширений Joomla.

![Phypadmin screenshot of list of databases](../../../en/images/getting-started/phpmyadmin-databases.png)

Примечание: сортировки в основном utf8mb4_0900_ai_ci:

- utf8mb4 означает, что каждый символ хранится как максимум 4 байта в схеме кодирования UTF-8.
- 0900 относится к версии Алгоритма сортировки Unicode.
- ai обозначает нечувствительность к ударению, нет разницы между e, è, é, ê и ë при сортировке.
- ci обозначает нечувствительность к регистру, нет разницы между p и P при сортировке.

Отдельные таблицы и столбцы могут иметь разную сортировку. Таблицы Joomla обычно имеют сортировку utf8mb4_unicode_ci.

## Настройка базы данных с помощью phpMyAdmin

Перед установкой Joomla вам нужна пустая база данных, готовая к заполнению. Вам также нужен пользователь базы данных.

### Создать базу данных

- **Запустите phpMyAdmin** Введите localhost/phpmyadmin в адресной строке вашего браузера.
- **Вход** В зависимости от того, как он был установлен, имя пользователя будет root или admin и пароль может либо присутствовать, либо отсутствовать.
- Выберите **Базы данных** в верхнем меню главной страницы phpMyAdmin.
- В **Создать базу данных** введите короткое имя вместо подсказки **Имя базы данных**, например, jtest.
- Выберите **utf8mb4_0900_ai_ci** из выпадающего списка Сопоставление.
- Выберите **Создать**, чтобы создать базу данных.

Это приведет вас к пустой базе данных. Вверху отображается сообщение: *В базе данных не найдено таблиц.*

### Создать пользователя

- Выберите значок **Home** в верхнем левом углу phpMyAdmin, чтобы перейти на главную страницу.
- Выберите **Учетные записи пользователей** в верхнем меню главной страницы.
- Выберите **Добавить учетную запись пользователя** в новом разделе под списком текущих пользователей (если они есть).
- Введите **Имя пользователя**, которое может быть коротким именем, которое вам понадобится позже при установке Joomla. Пример: jtest. Вы можете использовать этого же пользователя для других баз данных, созданных позже!
- Выберите **Local** в списке полей Hostname. Это установит localhost в соседнем поле значения.
- Используйте кнопку **Generate**, чтобы создать пароль. Вам нужно скопировать сгенерированное значение и вставить его в текстовый редактор для дальнейшего использования. Вам не нужно запоминать его надолго, так как он будет храниться в виде открытого текста в файле configuration.php Joomla. Пример: 8t8mGQq.gw\[\]8lp(
- **Сохранить** пропуская разделы "База данных для пользователя" и "Глобальные привилегии" формы. Кнопка **Go** (Сохранить) находится внизу.

### Назначить пользователю разрешения на базу данных

- На странице **Учетные записи пользователей** выберите пользователя (jtest), если необходимо, через Главная / Учетные записи пользователей / Имя пользователя.
- Выберите **База данных** в верхней части страницы. Это покажет список баз данных, к которым этому пользователю были предоставлены разрешения на доступ, и внизу список баз данных, к которым пользователю не был предоставлен доступ.
- **Выберите** базу данных, к которой нужно предоставить доступ, и нажмите кнопку **Вперед**.
- **Специфические привилегии базы данных** Выберите **Отметить все**, а затем **Вперед**, чтобы сохранить.

Вот и все! Теперь вы можете выйти из phpMyAdmin. Вы забыли записать пароль пользователя базы данных? Вернитесь и отредактируйте учетную запись пользователя, которую вы создали, чтобы изменить его. Если вы используете одного и того же пользователя базы данных для нескольких баз данных Joomla, вы найдете пароль в файле configuration.php Joomla.

*Переведено openai.com*

