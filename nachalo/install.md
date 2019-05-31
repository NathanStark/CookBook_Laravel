# Установка работа с Homestand

1. Создаем новый проект из композер laravel/homestand
2. Создаем папку в корне проекта папку  **web**
3. Выполняем **bash init.sh**
4. Редактируем файл настроек **Homestead.yaml**
   1. **folders** - Указываем папки которые буду синхронизироваться межу локальной машиной и вирутальной. Указваем в **map** - путь на локальной машине **./web **, **to** оставляем как есть
   2. **sites** - Указываем домены нашего сайта. Может быть несколько
5. Если указан путь /home/vagrant/code/public в настрйоках site, тогда необходимо в папке **web** создать папку **public**. 

Конфиг **Homestead.yaml **будет иметь следующий вид.

```
---
ip: "192.168.10.10"
memory: 512
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ./web
      to: /home/vagrant/code

sites:
    - map: sv.starlabs.su
      to: /home/vagrant/code/public

databases:
    - homestead

# blackfire:
#     - id: foo
#       token: bar
#       client-id: foo
#       client-token: bar

# ports:
#     - send: 50000
#       to: 5000
#     - send: 7777
#       to: 777
#       protocol: udp
```

В **Homestead.yaml **можно добавить параметр **name** и указать имя виртуальной машины

### Вариант установки №1 \(черерз установщик\)

1. Заходим на вирутальную машину в папке homesead выполняем **composer global require "laravel/installer"**. Данной командой мы скачиваем установщик **Larvel.**
2. Выполняем команду **export PATH="~/.composer/vendor/bin:$PATH"**
3. Переходим в папку **code** и выполняем **laravel new**, устанавилваем в папку code сам laravel.

### Вариант установки №2 \(через composer\)

1. В папке **code** вводим команду "**composer create-project --prefer-dist laravel/laravel ./" .**Тем самым создаем проект в текущем каталоге, если надо указать другой каталог, то в конце дописываем путь. К примеру **composer create-project --prefer-dist laravel/laravel NewFolder/blog**



