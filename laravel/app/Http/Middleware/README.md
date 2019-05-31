# middleware - посредники

[Видео](https://www.youtube.com/watch?v=hQZ46KPaqLQ&list=PL9ogRqeIEMzntnGiOj0tHu0o2ldxWWtJR&index=10)

**Посредники** \(англ. middleware\) предоставляют удобный механизм для фильтрации HTTP-запросов вашего приложения. Например, в Laravel есть посредник для проверки аутентификации пользователя. Если пользователь не аутентифицирован, посредник перенаправит его на экран входа в систему. Если же пользователь аутентифицирован, посредник позволит запросу пройти далее в приложение.

Конечно, посредники нужны не только для авторизации. CORS-посредник может пригодиться для добавления особых заголовков ко всем ответам в вашем приложении. А посредник логов может зарегистрировать все входящие запросы.

В Laravel есть несколько стандартных посредников, включая посредники для аутентификации и CSRF-защиты.

**Главное что надо понять**, посредники позволяют выполнить ряд действий, до обработки запроса пользователя и формирования ответа.

![](https://www.zimuel.it/img/slides/midwestphp2016/middleware.png)

## Создание посредника через cli

Команда:

> **artisan make:middleware TstMiddleware**

Создаст порсденика **TstMiddleware**

Пример посредника

```
<?php

namespace App\Http\Middleware;

use Closure;

class TstMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        return $next($request);
    }
}
```

После создания посредника, его необходимо привязать к конкретному маршруту, либо ко всем.  
Открываем файл **app/Http/Kernel.php  
protected $middleware - **Указывается список глобальных посредников. Это посредники не маршрутов.  
**protected $middlewareGroups -** Группы посредников. Используется для объединения нескольких посредников в одну группу. Указав в маршруте группу, все посредники этой группы будут применены к маршруту.  
**protected $routeMiddleware -** Список посредников, которые могу использвоаться в маршуртах. Ключ масива, явялется псевдонимом посредника, значение есть тот посредник который будет задействован.

Заносим наш посредник, в списке посредников:

```
<?php

namespace App\Http;

use Illuminate\Foundation\Http\Kernel as HttpKernel;

class Kernel extends HttpKernel
{
    /**
     * The application's global HTTP middleware stack.
     *
     * These middleware are run during every request to your application.
     *
     * @var array
     */
    protected $middleware = [
        \Illuminate\Foundation\Http\Middleware\CheckForMaintenanceMode::class,
        \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
        \App\Http\Middleware\TrimStrings::class,
        \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
        \App\Http\Middleware\TrustProxies::class,
    ];

    /**
     * The application's route middleware groups.
     *
     * @var array
     */
    protected $middlewareGroups = [
        'web' => [
            \App\Http\Middleware\EncryptCookies::class,
            \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
            \Illuminate\Session\Middleware\StartSession::class,
            // \Illuminate\Session\Middleware\AuthenticateSession::class,
            \Illuminate\View\Middleware\ShareErrorsFromSession::class,
            \App\Http\Middleware\VerifyCsrfToken::class,
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
        ],

        'api' => [
            'throttle:60,1',
            'bindings',
        ],
    ];

    /**
     * The application's route middleware.
     *
     * These middleware may be assigned to groups or used individually.
     *
     * @var array
     */
    protected $routeMiddleware = [
        'auth' => \Illuminate\Auth\Middleware\Authenticate::class,
        'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
        'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
        'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
        'can' => \Illuminate\Auth\Middleware\Authorize::class,
        'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
        'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
        'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
        'test'=> \App\Http\Middleware\TstMiddleware::class,

    ];
}
```

**"Вешаем"** на маршрут, любой из доступных посредников.  
Открывает списко маршрутов **routes/web.php** и у нужного маршрута в качестве яйчейки второго параметра добавляем **"middleware" =&gt; "test"**

```
<?php

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

Route::group(['prefix'=>"admin"],function(){
    Route::get('/page/add',function(){
        echo "add";
    });
    Route::get('/page/edit',function(){
        echo "edit";
    });
});

Route::get('/newss/{id}',['as'=>'news','uses'=>'FistController@show',"middleware" => "test"]);

Route::get('/', ['as'=>'home',function () {
    echo route('news',['id'=>23]);
}]);
```

Для применения нескольких посредников к маршруту, указываем в таком виде **"middleware" =&gt; \[ 'test', 'guest'\]**

## Глобальные посредники

Как говорилось ранее **protected $middleware **это глобальные посредники. К примеру наш посредник **TstMiddleware** будет глобальным.

```
namespace App\Http;

use Illuminate\Foundation\Http\Kernel as HttpKernel;

class Kernel extends HttpKernel
{
    /**
     * The application's global HTTP middleware stack.
     *
     * These middleware are run during every request to your application.
     *
     * @var array
     */
    protected $middleware = [
        \Illuminate\Foundation\Http\Middleware\CheckForMaintenanceMode::class,
        \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
        \App\Http\Middleware\TrimStrings::class,
        \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
        \App\Http\Middleware\TrustProxies::class,
        \App\Http\Middleware\TstMiddleware::class,
    ];
```

Тогда доступа к маршруту запроса у нас не будет, потому как программа не дошла до обработки маршрутов.

```
<?php

namespace App\Http\Middleware;

use Closure;

class TstMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        $route = $Request->route();
        return $next($request);
    }
}
```

---

Задавать посредник для маршрута можно по одноименному методу.

```
Route::get('/news/','Auth\LoginController@show')->name('news')->middleware('test');
```

### [Посредник в контроллере](#посредник-в-контроллере)

### Передача параметра в посредника

Для передачи параметра в просредника, после его название ставятся **":"**

```

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

Route::get('/newss/{id}',['as'=>'news','uses'=>'FistController@show',"middleware" => "test:home"]);

```

В самом посреднике третим входящим параметром указываем входяшие параметры

```

namespace App\Http\Middleware;

use Closure;

class TstMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next, $param)
    {
        $route = $Request->route();
        return $next($request);
    }
}
```



