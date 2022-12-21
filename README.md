<p align="center">
  <a href="https://github.com/korospace/sql-cheatsheet">
    <img src="img/logo.png" alt="Logo" width="120" height="120">
  </a>

  <h1 align="center">Codeigniter 4 Simple Blog</h1>

</p>

## Installation
```
composer create-project codeigniter4/appstarter ci4-simple-blog
```
_NOTE_ : link composer.exe <a href='https://getcomposer.org/doc/00-intro.md#installation-windows'>here</a>

## Setup .env
* rename your env file to <b>.env</b>
* uncoment line below
    ```
    CI_ENVIRONMENT = development

    app.baseURL    = 'your base url'

    database.default.hostname = 127.0.0.1
    database.default.database = db_ci4_blog
    database.default.username = root
    database.default.password = 
    database.default.DBDriver = MySQLi
    database.default.DBPrefix =
    database.default.port     = 3306
    ```
    _NOTE_ : make sure you have created the database

## Next step

<details open="open">
  <summary>CRUD CATEGORY</summary>
  <ul>
    <li><a href="add-categories.md">ADD CATEGORIES</a></li>
  </ul>
</details>