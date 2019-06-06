# Общее

Сервис провайдер + сервис контейнеры можно сравнить с композером. Композер это сервис провайдер, он обеспечивает закачку пакетов \(сервис контейнеров\) и их автоподключени.

# Сервис-провайдеры

[Видео](https://youtu.be/udHZatfg8aU)

Создает связь, между функционалом \(который был создан мною\) и сервис контейнером

Мржно сказать что сервис провайдер, добавляет конкретный функционал в сервис контейнир

Пример

```
namespace App\Providers;

use Bitrix24\Bitrix24;
use Illuminate\Support\ServiceProvider;

class B24 extends ServiceProvider
{
    /**
     * Bootstrap the application services.
     *
     * @return void
     */
    public function boot()
    {
        //
    }

    /**
     * Register the application services.
     *
     * @return void
     */
    public function register()
    {
        $this->app->singleton('bitrix24', function ($app) {
            return new Bitrix24($app->config->get('b24', []));
        });
    }
}
```

Объект **this-&gt;app** и есть контейне, в котором храняться все сервисы.

Метод **register** позволяет зарегестрировать класс в сервис контейнерре

# Сервис-контейнер

[Видео](https://youtu.be/qWDcMfJ_7oE)

Хранилище функционала, которое доступно при загрузке

