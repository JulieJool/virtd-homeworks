
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

### Инструкция к выполению

1. Для выполнения заданий обязательно ознакомьтесь с [инструкцией](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD) по экономии облачных ресурсов. Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
2. Практические задачи выполняйте на личной рабочей станции или созданной вами ранее ВМ в облаке.
3. Своё решение к задачам оформите в вашем GitHub репозитории в формате markdown!!!
4. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

## Задача 1

Сценарий выполнения задачи:
- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
- Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: ```{"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}```
- Зарегистрируйтесь и создайте публичный репозиторий  с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
- скачайте образ nginx:1.21.1;
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП). 
- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .
---
### Решение ###

[My repo with nginx image](https://hub.docker.com/repository/docker/juliejool/custom-nginx/general)   

---  

## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
![2.1](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/2.1.jpg)    
2. Переименуйте контейнер в "custom-nginx-t2"
![2.2](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/2.2.jpg)    
3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
![2.3](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/2.3.jpg)    
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.
![2.4.1](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/2.4.1.jpg)    

![2.4.2](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/2.4.2.jpg)    

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.
   ![3.1-3](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/3.1-3.jpg)    
Команда attach подключает по сути терминал к запущенному контейнеру, соответсвенно, команды выполненные непосредственно в терминале (не внутри контейнера) будут передоваться контейру. Получается, нажав данную комбинацию клавиш в терминале (не войдя в оболочку самого контейнера), мы передали контейнеру сингал остановиться, что он и сделал.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
 ![3.4-5](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/3.4-5.jpg)    
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
 ![3.6](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/3.6.jpg)    
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
 ![3.7](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/3.7.jpg)    
8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.    
 ![3.8](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/3.8.jpg)    
9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.
 ![3.9-10](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/3.9-10.jpg)
Изменив конфигурацию, мы завставили контейнер перестать прослушивать свой 80 порт, вместо него теперь прослушивается 81 порт. На хосту попрежнему открыт порт 8080, но так как он все еще связан на данный момент с 80 портом контейнера, ответа от контейнера nginx мы не получаем. Изменим эту ситуацию в следующем пункте, не удаляя и не создавая новый контейнер, изменив лишь конфигурационные файлы.
12. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
 ![3.11.1](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/3.11.1.jpg)    
 ![3.11.2](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/3.11.2.jpg)    
 ![3.11.3](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/3.11.3.jpg)    
 ![3.11.4](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/3.11.4.jpg)    
 ![3.11.5](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/3.11.5.jpg)    
13. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)
 ![3.12](https://github.com/JulieJool/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/img/3.12.jpg)    

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.
- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2
    network_mode: host
    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

---

### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.


