# 05-virt-03-docker
Карпов Андрей DEVOPS-21


## Задача 1

Сценарий выполения задачи:

- создайте свой репозиторий на https://hub.docker.com;
- выберете любой образ, который содержит веб-сервер Nginx;
- создайте свой fork образа;
- реализуйте функциональность:
запуск веб-сервера в фоне с индекс-страницей, содержащей HTML-код ниже:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I’m DevOps Engineer!</h1>
</body>
</html>
```
Опубликуйте созданный форк в своем репозитории и предоставьте ответ в виде ссылки на https://hub.docker.com/username_repo.

### Ответ:
https://hub.docker.com/repository/docker/askarpoff/05-virt-03-docker


## Задача 2

Посмотрите на сценарий ниже и ответьте на вопрос:
"Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:

- Высоконагруженное монолитное java веб-приложение;
- Nodejs веб-приложение;
- Мобильное приложение c версиями для Android и iOS;
- Шина данных на базе Apache Kafka;
- Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;
- Мониторинг-стек на базе Prometheus и Grafana;
- MongoDB, как основное хранилище данных для java-приложения;
- Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.

### Ответ:

- Высоконагруженное монолитное java веб-приложение;
```
Высоконагруженное - это не про докер. На виртуалку или даже на физическую машину (на виртуалку, конечно, удобнее для масштабирования и бэкапа).
```

- Nodejs веб-приложение;
```
Приложения на Nodejs хорошо поддаются докеризации, поэтому - докер.
```

- Мобильное приложение c версиями для Android и iOS;
```
Если имеется ввиду сервер для мобильного приложения - то можно и на виртуальной машине, и на докере сделать - взависимости от нагрузки, количества клиентов
```

- Шина данных на базе Apache Kafka;
```
У меня на работе есть такая реализация для витрины данных на контейнерах Docker
```

- Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;
```
Делал примерно такое на Docker Swarm
```

- Мониторинг-стек на базе Prometheus и Grafana;
```
Prometheus и Grafana отлично работают в докер-контейнерах.
```

- MongoDB, как основное хранилище данных для java-приложения;
```
У MongoDB десть официальный образ на Docker Hub, главное - данные держать вне контейнера.
```

- Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.
```
На виртуальную машину. Gitlab - он довольно "тяжелый".
```
## Задача 3

- Запустите первый контейнер из образа ***centos*** c любым тэгом в фоновом режиме, подключив папку ```/data``` из текущей рабочей директории на хостовой машине в ```/data``` контейнера;
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив папку ```/data``` из текущей рабочей директории на хостовой машине в ```/data``` контейнера;
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```;
- Добавьте еще один файл в папку ```/data``` на хостовой машине;
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера

### Ответ

```
root@debian:/home/debian/docker-nginx# docker pull centos:latest
latest: Pulling from library/centos
a1d0c7532777: Pull complete
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest
root@debian:/home/debian/docker-nginx# docker pull debian:latest
latest: Pulling from library/debian
17c9e6141fdb: Pull complete
Digest: sha256:bfe6615d017d1eebe19f349669de58cda36c668ef916e618be78071513c690e5
Status: Downloaded newer image for debian:latest
docker.io/library/debian:latest

root@debian:/home/debian# docker run --name askarpoff-debian -d -v /home/debian/data:/data -t debian:latest
027c74fbe1e0061716ca8d983f7c914a16084478bf9d9170febb733816d8c217
root@debian:/home/debian# docker run --name askarpoff-centos -d -v /home/debian/data:/data -t centos:latest
be1b451c31516404f46d791c67102633d2ea7f7e7b0e24cb5e2f59408628d4fc

root@debian:/home/debian# docker exec -it askarpoff-centos /bin/bash
[root@be1b451c3151 /]# cd /data
[root@be1b451c3151 data]# echo test_test_test 1.txt
test_test_test 1.txt
[root@be1b451c3151 data]# echo test_test_test > 1.txt
[root@be1b451c3151 data]# exit
exit
root@debian:/home/debian# cd \data
root@debian:/home/debian/data# ls
1.txt
root@debian:/home/debian/data# mkdir new_dir
exit
root@debian:/home/debian/data# docker exec -it askarpoff-debian /bin/bash
root@027c74fbe1e0:/# cd /data
root@027c74fbe1e0:/data# ls
1.txt  new_dir
root@027c74fbe1e0:/data# cat 1.txt
test_test_test
root@027c74fbe1e0:/data# exit

```
