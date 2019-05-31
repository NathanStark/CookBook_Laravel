# Виды \(Представления\)

[Видео](https://youtu.be/IjrF3p2wOfw)

Для подключения шаблона, используется метод **view\(\)**

```
<?php
namespace App\Http\Controllers;

class FistController extends Controller
{
    public function show(){
        return view('news')
    }
}
```

**news** - Название файла шаблона.

Если файл находится в подпапке, то для его указания в методе view, в качестве разделителя используется **".". **К примеру в у нас есть файл resources/views/news/main.php, вызовим его в контролере.

```
<?php
namespace App\Http\Controllers;

class FistController extends Controller
{
    public function show(){
        return view('news.main')
    }
}
```

Если при вызове метода **view **не указывать шаблон, то вернется объект класса ViewFactory - объект сущности шаблоны.

Если название шаблона было указано, вернется объект текущего шаблона.

## Передача данных в шаблон.

Для передачи переменых в шаблон, вторым параметром метода **view, **указывается массив, где ключ массива это название переменой, а значение массива, это значеие переменой.

```
<?php
namespace App\Http\Controllers;

class FistController extends Controller
{
    public function show(){
        $data = ["id"=>10,"title"=>"Новости"]
        return view('news.main',$data)
    }
}
```

Альтернативный способ передачи переменной через метод **with**

```
<?php
namespace App\Http\Controllers;

class FistController extends Controller
{
    public function show(){
        return view('news.main')->with("id",10)->with("title","Новости")
    }
}
```

Еще вариант вызов метода имя которого с **префиксом with**

```
<?php
namespace App\Http\Controllers;

class FistController extends Controller
{
    public function show(){
        return view('news.main')->withId(10)->withTitle("Новости")
    }
}
```



