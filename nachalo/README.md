# Начало

**Исходные данные:**  
БД: homestead/secret

* Установка работа с Homestand

### **Автокомплит для  IDE**

По ssh заходим на сервер homestand

Устанавливаем пакет **composer require --dev barryvdh/laravel-ide-helper**

Заходим в сервис провайдер **/app/Providers/AppServiceProvider.php** и добалвем код

```
public function register()
{
    if( $this->app->environment() == "local"){
        $this->app->register('Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider');
    }
}
```

В файле composer.json добавляем:

```
"scripts":{
    "post-update-cmd": [
        "Illuminate\\Foundation\\ComposerScripts::postUpdate",
        "php artisan ide-helper:generate",
        "php artisan ide-helper:meta"
    ]
},
```

В командной строке выполняем: **artisan ide-helper:generate** и **artisan ide-helper:meta**

### Еще один вспомогательный пакет

> **composer require --dev doctrine/dbal**

Он позволяет прописывать прямо в модели некоторые атрибуты.

> // Кажется надо выполнить чо бы в модель упали атрибуты  
> artisan ide-helper:models -W



