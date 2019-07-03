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

