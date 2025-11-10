
Скелет WEB интерфейса ( dev #0.2.114 ) :
-----

```
/app            - Ядро.
  /ext          - PHP Классы.
  /includes     - Основные и дополнительные PHP функции.
  /modules      - Каталог с модулями.
  /page         - Основные заготовки и шаблоны WEB интерфейса.
  
/storage        - Хранилище.
  /assets       - CSS, JS, Fonts файлы.
  /cache        - Основной кэш.
    /img        - Кэш изображений.
    /sessions   - Кэш связанный с работой ядра.
      
/index.php      - 'Hello World'
```

Модули:
-----

Каталог с модулями:
```
/app/modules
```

Что представляет из себя модуль ( На примере **module_block_main_stats** ):

```
/app
  /modules
    /module_block_main_stats       - Название папки = ID модуля.
      /ext          		   	   - PHP Классы.
      /assets                      - Ассеты.
        /css                       - CSS ассеты.
        /js                        - JS ассеты.
      /forward                     - Функциональная часть.
        /data.php                  - Пре-инициализация. Скрипт начинает свою работу до загрузки шаблона страницы.
	/data_always.php           	   - Пре-инициализация. Скрипт начинает свою работу до загрузки шаблона и работает на всех страницах.
        /interface.php             - Инициализация. Скрипт начинает свою работу во время загрузки шаблона.
      /temp						   - Кэш файлы.
    /description.json - Описание модуля
    /translation.json - Если модуль имеет мультиязычность, переводы описываются в данном файле.

```


Шаблон:
-----

Директория для работы с шаблонами:
```
app/templates/
```

Для инициализации шаблона, необходим файл description.json, содержащий такую структуру:
```
{
    "name": "Ваше название шаблона",
    "version": "0.1 (Версия вашего шаблона)",
    "author": "Flames"
}
```

Структура папки имеет немного схожую с модулями структуру
```
/templates/name/
```

Условная папка со стилями и js, все вы сможете подключить в head.php, как вашей душе благорасудится
```
- assets/
    - js/   - Папка с JS файлами
    - css/  - Папка с CSS файлами
```
Верстка будет подгружена ПОСЛЕ оригинальной верстки.

Папка, отвечающая за отрисовку контента
```
- interface/
    - navbar.php    //Навбар сайта, его так сказать голова
    - sidebar.php   //Сайдбар.. Просто сайдбар.. Можно будет переделать под любое применение
    - head.php      //Самый высший файл, необходим для подлючения библиотек, к примеру bootstrap
```

Папка, если нужно дополнительно подгрузить JS, CSS файлы в конкретном модуле
```
- modules/
    - module_page_profiles/ - Название папки которое совпадает с названием модуля
        - dop.css - CSS и JS файлы которые нужно подгрузить, будет загружено ПОСЛЕ основных файлов.
        - dop.js
    
    - module_page_forum/    - Тут может быть любой модуль.
        -....
```

Файл с scss переменными, для более удобными работами с цветами
```
colors.json = 
{
    "Ваше название переменной, в моем случае это будет --color-zalupa": "#fff",
    "--sidebar-block": "#0f0f0f0f"
}
```

Порядок загрузки:
-----
<details><summary>Модули:</summary>

Порядок загрузки стилей модулей таков:
```
- data_always.php
- data.php
- interface.php
- interface_always.php
- css / template css
- js / template js
```

</details>
<details><summary>Шаблон:</summary>

Порядок загрузки стилей модулей таков:
```
/Forward
- head.php
- navbar.php
- sidebar.php
- container.php
// JS / CSS
```

</details>
<details><summary>Функции и классы:</summary>
LR WEB подгружает все свои классы и функции в index.php, поэтому объявлять где - либо класс не обязательно.
Классы имеют такую структуру:


```
- AltoRouter.php        - Новый класс с роутингом, нужен для чего? Правильно, роутинга! :)
- Auth.php              - Класс для работы с авторизацией пользователя, запись в сессию данных, если админ авторизировался через L/P
- Db.php                - Класс для работы с базой данных, используется для отправки запросов ( Не рекомендуется ), и подключение к БД.
- General.php           - Класс для работы с основными настройками сайта
- Graphics.php          - Класс для работы с отрисовкой контента и подгрузкой выбранных в админке опций
- LightOpenID.php       - Класс для авторизации через STEAM, единственный класс, который лучше всего не трогать.
- Modules.php           - Класс для распределения модулей и их настройкой
- Notifications.php     - Класс для отрисовки и рендера уведомлений пользователя
- Pdox.php (Interface)  - Новый класс для работы с базой данных. Можно сказать, что это - Query Builder. Единственный класс, который не вызывается в index.php
- Translate.php         - Класс, работающий с языком пользователя, и отрисовкой нужных переводов
```

</details>

Описание каждой публичной функции класса:
----

<details><summary>AltoRouter.php:</summary>
Этот класс уже имеет документацию на другой странице GitHub:
https://github.com/dannyvankooten/AltoRouter
</details>

<details><summary>Auth.php:</summary>

get_admins_list
----
```
Получение списка администраторов
```

get_count_admins
----
```
Подсчет кол - ва администраторов
```

check_session_admin
----
```
Проверяет данные сессии администратора, с данными, входящими в сервер
```

check_session
----
```
Проверка на IP
```

authorization_no_steam
----
```
Запись данных администратора в сессию
```

get_authorization_sidebar_data
----
```
Выходные файлы для вывода данных о пользователе в сайдбар
```
</details>

<details><summary>Db.php:</summary>

query ( int $mod, int $user_id = 0, int $db_id = 0, string $sql, array $params = []  )
----
```
$mod            - Мод, из db.php (Vips, Shop, Core)
$user_id        - Номер базы данных
$db_id          - Номер таблицы базы данных
$sql            - Сам SQL запрос
$params         - Подготовительные значения для PDO, нужно для большей безопасности.

Функция, позволяющая выполнить SQL запрос

return SQL result;
```

queryNum ( int $mod, int $user_id = 0, int $db_id = 0, string $sql, array $params = []  )
----
```
Все то же, как и у query, только на выходе получаем только числовое значение
```

queryAll ( int $mod, int $user_id = 0, int $db_id = 0, string $sql, array $params = []  )
----
```
Да ну, все то же самое? О да! Только теперь возвращает весь массив с данными
```

query_all_key_pair ( int $mod, int $user_id = 0, int $db_id = 0, string $sql, array $params = []  )
---
```
Шаблон запроса отдающий массив со всеми строками, парсирование ключа.
```

queryColumn ( int $mod, int $user_id = 0, int $db_id = 0, string $sql, array $params = []  )
---
```
Шаблон запроса отдающий массив стобца.
```

queryOneColumn ( int $mod, int $user_id = 0, int $db_id = 0, string $sql, array $params = []  )
---
```
Шаблон запроса отдающий данные одного стобца.
```

mysql_column_search ( int $mod, int $user_id = 0, int $db_id = 0, string $tablename, string $column   )
---
```
Запрос проверяющий существование столбика в той или иной таблице.

$tablename      - Название таблицы, которую нужно проверить
$column         - Название столбца, который нужно найти

Возвращает результат проверки, 1 / 0
```

mysql_table_search ( int $mod, int $user_id = 0, int $db_id = 0, string $tablename )
---
```
Запрос проверяющий существование таблицы в той или иной базе данных.

Возвращает результат проверки, 1 / 0
```

lastInsertId ( string $mod, int $user_id = 0, int $db_id = 0 ) 
---
```
Возвращает ID последней вставленной строки.

Возвращает результат ( ID )
```

__destruct 
---
```
"Разрыв соединения с базой данных".
```
</details>

<details><summary>General.php:</summary>

get_default_url_section( string|bool $section, string $default, array|null $arr_true )
----
```
Получает и задает название подраздела из URL по умолчанию, сохраняя результат по умолчанию в сессию.

$section       - Название подраздела.
$default       - Значние по умолчанию.
$arr_true      - Белый список.
```

getAvatar( string $profile, int $type )
----
```
Получает определенного аватара.

$profile        - Steam ID игрока
$type           - Тип/Размер аватара.

Возвращает ссылку на аватар
```

checkAvatar( string $profile, int $type )
----
```
Проверка на существование определеноого аватара и его актуальность.

Выводит итог проверки.
```

checkName( string $profile )
----
```
Получение никнейма игрока.

Вывод его имени, как ни странно
```

sendNote ( string $text, success|error $status, int $time = 4.5 )
----
```
Отправка уведомлений через функцию.

$text           - Текст уведомления
$status         - Тип уведомления
$time           - Время, которое провисит уведомление
```

get_server_list
----
```
Просто возвращает настройки серверов из БД
```

get_icon ( string $group, string $name, string $category = null )
----
```
Получение иконок и работа с ними.

$group          - Название папки из которой будет читаться иконка.
$name           - Название иконки.
$category       - Дополнительное название под-категории, если она имеется. По умолчанию нету.

Выводит содержимое SVG файла. || false
```

get_js_relevance_avatar ( string $id, int $type = 1 )
----
```
Получение иконок и работа с ними.

$id             - Steam ID - 32.
$type           - Тип аватара.

Выводит JS скрипт.
```
</details>

</details>

-----

<p align="right">
        <img height="75px" src="https://raw.githubusercontent.com/levelsranks/levels-ranks-web/alpha/.github/authors.png">
</p>
