---
layout: post
title:  "MoViCon: HttpController"
date:   2018-06-28 20:10:00 +0200
author: Gonzalo Chumillas
categories: ['php', 'programming', 'framework']
---

La clase [HttpController](https://github.com/movicon/movicon-http/blob/master/src/http/HttpController.php) representa el corazón del framework [MoViCon](https://github.com/movicon/movicon). Su misión consiste en interceptar y procesar las petiticiones HTTP enviadas desde el 'cliente'. Veámoslo con algunos ejemplos:

```php
header("Content-Type: text/plain; charset=utf-8");
require_once "path/to/vendor/autoload.php";
use movicon\http\HttpController;

class MyController extends HttpController
{
    public function __construct()
    {
        // Adds 'request listeners'
        $this->on("GET", [$this, "get"]);
        $this->on("POST", [$this, "post"]);
        $this->on("DELETE", [$this, "delete"]);
    }

    // This method is called on GET requests.
    public function get()
    {
        echo "Processing GET method...";
    }

    // This method is called on POST requests.
    public function post()
    {
        echo "Processing POST method....";
    }

    // This method is called on DELETE requests.
    public function delete()
    {
        echo "Processing DELETE method...";
    }
}

// Creates an instance and processes the HTTP request.
$c = new MyController();
$c->processRequest();
```

Ya que los métodos `GET` y `POST` son muy comunes, podemos reescribir el ejemplo anterior de la siguiente forma:

```php
class MyController extends HttpController
{
    public function __construct()
    {
        // Adds 'request listeners'
        $this->onGet([$this, "get"]);
        $this->onPost([$this, "post"]);
        $this->on("DELETE", [$this, "delete"]);
    }

    // This method is called on GET requests.
    public function get()
    {
        echo "Processing GET method...";
    }

    // This method is called on POST requests.
    public function post()
    {
        echo "Processing POST method....";
    }

    // This method is called on DELETE requests.
    public function delete()
    {
        echo "Processing DELETE method...";
    }
}
```

Hay dos métodos especiales que no se corresponden con ningún método HTTP: `OPEN` y `CLOSE`. Estos métodos se llaman **antes** y **después** de cualquier otro método, respectivamente. Son muy útiles para iniciar y cerrar recursos, aunque el método `CLOSE` raramente lo usaremos, ya que los recursos suelen cerrarse automáticamente.

```php
class MyController extends HttpController
{
    private $_conn;

    public function __construct()
    {
        // Adds 'request listeners'
        $this->onOpen([$this, "open"]);
        $this->onGet([$this, "get"]);
        $this->onClose([$this, "close"]);
    }

    // This method is called BEFORE any other request listener.
    public function open()
    {
        $this->_conn = mysqli_connect("localhost", "root", "123456", "test");
    }

    // This method is called on GET requests.
    public function get()
    {
        $result = mysqli_query($this->_conn, "select * from item");
        while ($row = mysqli_fetch_assoc($result)) {
            echo "Row ID: $row[id], Title: $row[title]\n";
        }
        mysqli_free_result($result);
    }

    // This method is called AFTER any other request listener.
    public function close()
    {
        mysqli_close($this->_conn);
    }
}
```

Puedes leer los parámetros de la petición actual mediante la función `getParam`:

```php
class MyController extends HttpController
{
    private $_conn;

    public function __construct()
    {
        // Adds 'request listeners'
        $this->onGet([$this, "get"]);
    }

    // This method is called on GET requests.
    public function get()
    {
        $title = $this->getParam("title");

        // The parameter 'name' is required
        $name = $this->getParam("name", ["required" => true]);

        // When omitted, the paramter 'gender' is 'male'
        $gender = $this->getParam("gender", ["default" => "male"]);

        // By default, all parameters are 'trimmed'
        $text = $this->getParam("text", ["required" => true, "trim" => false]);
    }
}
```

También puedes extender controladores fácilmente:

```php
class BaseController extends HttpController
{
    protected $conn;

    public function __construct()
    {
        // This listener is called BEFORE any other 'request listener'
        $this->onOpen(function () {
            $this->conn = mysqli_connect("localhost", "root", "123456", "test");
        });
    }
}

class MyController extends BaseController
{
    public function __construct()
    {
        parent::__construct();
        $this->onGet([$this, "get"]);
    }

    // This method is called on GET requests.
    public function get()
    {
        $result = mysqli_query($this->conn, "select * from item");
        while ($row = mysqli_fetch_assoc($result)) {
            echo "Row ID: $row[id], Title: $row[title]\n";
        }
        mysqli_free_result($result);
    }
}
```
