# Особености работы Bitrix ENV c Laravel

## Не будут работать сессии

Необходимо установить параметр **mbstring.func\_overload** **0,** иначе сесси не будут работать.

Я прописал параметр

```bash
php_admin_value mbstring.func_overload 0
```

В файле

```
/etc/httpd/bx/conf/bx_ext_ttp.starlabs.su.conf
```

Но есть подозрение, что обновление затрет это.

## Для работы приложения Битрикс24

Необходимо добавить параметры

```
add_header "X-Content-Type-Options" "nosniff";
add_header X-Frame-Options '';
```

В файл

```
/etc/nginx/bx/site_enabled/bx_ext_dev.ttp.starlabs.su.conf
```

И этот же параметр добавить для ssl настройки, это файл

```
/etc/nginx/bx/site_enabled/bx_ext_ssl_dev.ttp.starlabs.su.conf
```



