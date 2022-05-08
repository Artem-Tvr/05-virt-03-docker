# Домашнее задание к занятию "5.3. Введение. Экосистема. Архитектура. Жизненный цикл Docker контейнера"

## Задача 1

Сценарий выполения задачи:

* создайте свой репозиторий на [https://hub.docker.com](https://hub.docker.com/);
* выберете любой образ, который содержит веб-сервер Nginx;
* создайте свой fork образа;
* реализуйте функциональность: запуск веб-сервера в фоне с индекс-страницей, содержащей HTML-код ниже:

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

Опубликуйте созданный форк в своем репозитории и предоставьте ответ в виде ссылки на [https://hub.docker.com/username_repo](https://hub.docker.com/username_repo).

Решение:

https://hub.docker.com/repository/docker/artemdevops/netology


## Задача 2

Посмотрите на сценарий ниже и ответьте на вопрос: "Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:

* Высоконагруженное монолитное java веб-приложение;
* Nodejs веб-приложение;
* Мобильное приложение c версиями для Android и iOS;
* Шина данных на базе Apache Kafka;
* Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;
* Мониторинг-стек на базе Prometheus и Grafana;
* MongoDB, как основное хранилище данных для java-приложения;
* Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.

## Задача 3

* Запустите первый контейнер из образа ***centos*** c любым тэгом в фоновом режиме, подключив папку `/data` из текущей рабочей директории на хостовой машине в `/data` контейнера;
* Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив папку `/data` из текущей рабочей директории на хостовой машине в `/data` контейнера;
* Подключитесь к первому контейнеру с помощью `docker exec` и создайте текстовый файл любого содержания в `/data`;
* Добавьте еще один файл в папку `/data` на хостовой машине;
* Подключитесь во второй контейнер и отобразите листинг и содержание файлов в `/data` контейнера.
  Решение:

```
root@cadcixztkv:~# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
root@cadcixztkv:~# docker run -d -i -v /opt/docker/data:/data centos:latest /bin/bash
ce8dc673d1e5110471d679a33cac75801db68d895121777a4bf2ab1bd1f997f0
root@cadcixztkv:~# docker ps
CONTAINER ID   IMAGE           COMMAND       CREATED         STATUS         PORTS     NAMES
ce8dc673d1e5   centos:latest   "/bin/bash"   8 seconds ago   Up 6 seconds             unruffled_elbakyan
root@cadcixztkv:~# docker run -d -i -v /opt/docker/data:/data debian:latest /bin/bash
969b694e3bb952c20830d13e77722960ba53fe073e4e7f2e882029274fb20b73
root@cadcixztkv:~# docker ps
CONTAINER ID   IMAGE           COMMAND       CREATED              STATUS              PORTS     NAMES
969b694e3bb9   debian:latest   "/bin/bash"   5 seconds ago        Up 4 seconds                  funny_bartik
ce8dc673d1e5   centos:latest   "/bin/bash"   About a minute ago   Up About a minute             unruffled_elbakyan
root@cadcixztkv:~# docker exec -i -t ce8dc673d1e5 bash
[root@ce8dc673d1e5 /]# ls
bin   dev  home  lib64       media  opt   root  sbin  sys  usr
data  etc  lib   lost+found  mnt    proc  run   srv   tmp  var
[root@ce8dc673d1e5 /]# cd data
[root@ce8dc673d1e5 data]# touch testfile.txt
[root@ce8dc673d1e5 data]# ls
testfile.txt
[root@ce8dc673d1e5 data]# echo Hello > testfile.txt
[root@ce8dc673d1e5 data]# cat testfile.txt
РуHello
[root@ce8dc673d1e5 data]# exit
exit
root@cadcixztkv:~# docker exec -i -t 969b694e3bb9 bash
root@969b694e3bb9:/# ls
bin   data  etc   lib    media  opt   root  sbin  sys  usr
boot  dev   home  lib64  mnt    proc  run   srv   tmp  var
root@969b694e3bb9:/# cd data
root@969b694e3bb9:/data# ls -a
.  ..  testfile.txt  testfile_host.txt
root@969b694e3bb9:/data# cat testfile_host.txt
Paparapara

```

## Задача 4 (*)

Воспроизвести практическую часть лекции самостоятельно.

Соберите Docker образ с Ansible, загрузите на Docker Hub и пришлите ссылку вместе с остальными ответами к задачам.
