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

```
root@cadcixztkv:~# docker pull ubuntu/nginx
Using default tag: latest
latest: Pulling from ubuntu/nginx
c9270d8556cf: Pull complete
4f59a1458174: Pull complete
83da71339431: Pull complete
c7f0723302c6: Pull complete
Digest: sha256:12914e20b3ad6d2ef7504b525dcfd24e8ce258286f245783b6760f26b67f7020
Status: Downloaded newer image for ubuntu/nginx:latest
docker.io/ubuntu/nginx:latest
root@cadcixztkv:~# docker images
REPOSITORY     TAG       IMAGE ID       CREATED       SIZE
ubuntu/nginx   latest    33345394c4b7   2 weeks ago   144MB
root@cadcixztkv:~# docker run -d -p 1234:80 ubuntu/nginx:latest
bec29259087b0670173dc5c39d3a6bc97e67c6a0c289564368fe5f6a67777b39
root@cadcixztkv:~# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS         PORTS                                   NAMES
bec29259087b   ubuntu/nginx:latest   "/docker-entrypoint.…"   10 seconds ago   Up 9 seconds   0.0.0.0:1234->80/tcp, :::1234->80/tcp   brave_kalam
root@cadcixztkv:~# docker commit bec29259087b  testimage:v1
sha256:920779d70bba226d43684efabaca871d47ecd61b0ce7cbbd94abe4e03870ee40
root@cadcixztkv:~# docker images
REPOSITORY     TAG       IMAGE ID       CREATED          SIZE
testimage      v1        920779d70bba   22 seconds ago   144MB
ubuntu/nginx   latest    33345394c4b7   2 weeks ago      144MB
root@cadcixztkv:~# cd /opt/docker/mynginx/
root@cadcixztkv:/opt/docker/mynginx# mkdir html
root@cadcixztkv:/opt/docker/mynginx# docker build -t mytestimage:v2 .
Sending build context to Docker daemon  3.584kB
Step 1/2 : FROM testimage:v1
 ---> 920779d70bba
Step 2/2 : COPY ./html/ /usr/share/nginx/html/
 ---> 8ae6da668658
Successfully built 8ae6da668658
Successfully tagged mytestimage:v2
root@cadcixztkv:/opt/docker/mynginx# docker run -t -i mytestimage:v2 /bin/bash
root@83fb652665b8:/# cat /usr/share/nginx/html/index.html
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I’m DevOps Engineer!</h1>
</body>
</html>

```

## Задача 2

Посмотрите на сценарий ниже и ответьте на вопрос: "Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:

(-) Высоконагруженное монолитное java веб-приложение;

Ответ: физический сервер, т.к. монолитное, в микросерверах не реализуемо,а так же если высоконагруженное -  то нужно иметь физический доступ к ресурсами, без использования гипервизора виртуалки.

(-) Nodejs веб-приложение;

Ответ: для веб приложения Docker подходит отлично для реализации микросервисов.

(-) Мобильное приложение c версиями для Android и iOS;

Ответ: здесь больше подходит ВМ, так как Docker не имеет графического интерфейса.

(-) Шина данных на базе Apache Kafka;

Ответ: думаю Docker вполне подойдет, с использованием Volume для хранения данных.

(-) Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;

Ответ: вполне реализуемо с помощью контейнеризации, данные хранить в Volume.

(-) Мониторинг-стек на базе Prometheus и Grafana;

Ответ: сами системы данные не хранят, противопоказаний для развертывания в Docker не вижу.

(-) MongoDB, как основное хранилище данных для java-приложения;

Ответ: больше подойдет ВМ или физический сервер.

(-) Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.

Ответ: эти сервисы разворачивать на базе Docker мне видится хорошей идеей, с использованием отдельных контейнеров под компоненты систем.


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
