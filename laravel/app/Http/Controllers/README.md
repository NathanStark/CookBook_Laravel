# Controllers

Контролеры будущего приложения. По умолчанию присутсвует один файл **Controller.php** в котором описан базовый класс, которы необходимо наследовать при создании своих контролеров.

Создадим свой тестовый контролер: Путь **app/Http/Controllers/FistController.php**

```
<?php
namespace App\Http\Controllers;

class FistController extends Controller
{
    public function show(){
        echo 'is News';
    }
}
```

Для передачи маршрута контролеру, в маршрте вторым параметром передаем в качестве строки имя контролера, после имени контролера через **@** указываем метод контролера. \(файл **routes/web.php**\)

```
Route::get('/news/','FistController@show')->name('news');
```

Если контроле находится не в корневом пространисве имен **Controllers** а в директории, к примеру **app/Http/Controllers/Auth**, тогда в роутере необходимо указать название папки пространства имен. К примеру

```
Route::get('/news/','Auth\LoginController@show')->name('news');
```

Если во втором параметр передается массив, тогда контролер имеет ключ **uses**

```
Route::get('/news/',['as'=>'home','uses'=>'Auth\LoginController@show'])->name('news');
```

Если в маршруте передается параметр,  то и в метод передается значение этого параметра.  
**Маршрут**

```
Route::get('/news/{id}','FistController@show')->name('news');
```

**Контролер**

```
<?php
namespace App\Http\Controllers;

class FistController extends Controller
{
    public function show($id){
        echo 'is News id: '.$id;
    }
}
```

**Создание единого контролера для обработки маршрута**. В маршруте прописываем:

```
Route::controller('/news','FistController');
```

Сам контролер состоит из методов,названия которых соотвествуют вызываемым страницам. К примеру запрос страницы **/news/add/** будет вызывать метод **getAdd** данно контролера.

```
<?php
namespace App\Http\Controllers;

class FistController extends Controller
{
    public function getAdd(){
    }
}
```

Для вызова метода страницы "**/news/**" создадим метод **getIndex. **Метод всегда начинается с префикса, который обозначает какой тим запроса надо обрабатывать, **get, post, any, и т.д. **

## Создание контролера через cli

Команда:

> **artisan make:controller SecondController**

Создаст контролер **SecondController**

## RESTFull

[Смотреть](https://youtu.be/2YvwC6F8ysc?t=21m2s)

## Посредники в контролере {#controller-middleware}

Работая с контролером, в его конструкторе можно определить список применяемых посредников для данного маршрута.

```
<?php
namespace App\Http\Controllers;

class FistController extends Controller
{
    public function __construktor(){
        $this->middleware('auth);
    }
    public function getAdd(){
    }
}
```



