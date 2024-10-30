
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
### Ответ
https://hub.docker.com/repository/docker/ranelgilyazov/custom-nginx/general

## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.
### Ответ
![Screenshot 2024-10-27 132314](https://github.com/user-attachments/assets/e3fda983-5aea-440d-b8cd-b92b9951b291)

## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.
9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.
11. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.
### Ответ
![Screenshot 2024-10-27 135447](https://github.com/user-attachments/assets/3bca5a25-4088-4498-a32e-7ae55bb76c7f)
Согласно документации комбинация клавиш Ctrl-C останавливает контейнер
![Screenshot 2024-10-27 140652](https://github.com/user-attachments/assets/b4da3469-cfd0-49c6-a995-da4606646a13)
![Screenshot 2024-10-27 141102](https://github.com/user-attachments/assets/7f3911b8-7ec1-4373-b928-f19ab13b8a72)
![Screenshot 2024-10-27 141136](https://github.com/user-attachments/assets/e5a9c935-91b6-4be7-8e36-889b211f0dff)
Из за того что мы поменяли порт с 80 на 81, nginx обслуживается на 81 порту, и при потыке подключится к 80 порту контейнер выдает не дает нам это сделать и выдает ошибку

## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.
- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

### Ответ 

![Screenshot 2024-10-27 160028](https://github.com/user-attachments/assets/eee0c9ff-c802-4ce4-b4bd-e22e9996a196)
![Screenshot 2024-10-27 160109](https://github.com/user-attachments/assets/b75a6f2e-16d5-4754-b110-980b279607d8)
![Screenshot 2024-10-27 160208](https://github.com/user-attachments/assets/8ff7fb46-b884-4010-90f5-9dfdc8e45c04)

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
### Ответ 
![Screenshot 2024-10-27 161122](https://github.com/user-attachments/assets/eea7246e-d07a-4f87-bb17-f03b4b945f87)
![Screenshot 2024-10-27 161332](https://github.com/user-attachments/assets/834922bb-1e6d-4d40-86c5-c4449cd378b7)


#### Дополнения

По адресу 127.0.0.1:9000 в браузере:
![image](https://github.com/user-attachments/assets/3715c08a-0abe-4823-8de9-182d72db3036)
По коду  из директории где у меня лежат compose.yaml и docker-compose.yaml, выглядят они так
![image](https://github.com/user-attachments/assets/37b36c3b-c06e-4dce-8db8-f64dfaa1923a)
запускаю командой  docker compose up, получаю сообщение что все нормально запущено
![image](https://github.com/user-attachments/assets/83834d08-9765-4a50-995d-da471b97887f)
далее ![image](https://github.com/user-attachments/assets/0795f886-caad-415b-8607-2f3c6694f9cc)
После перезапуска контейнера portainer 
![image](https://github.com/user-attachments/assets/4fdefd8f-de7b-4082-bc91-ef5d6ec026c6)
Почему то мне удалось попасть в мой portainer по IP своей виртуальной машины, а не через порт 127.0.0.1:9000, почему так произошло мне к сожалению не понятно
![image](https://github.com/user-attachments/assets/9b506e46-2d4b-4a7e-87ed-8fb7910832b0)

Удалив файл compose.yaml, остались контенеры которы не кому не принадлежат, судя из ошибки называются "Потеренные" контейнеры, благодоря ключу --remove-orphans мы их удалим
![image](https://github.com/user-attachments/assets/21ef6e0c-0c79-4265-be8a-2f4a74f3d7dc)


### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.
