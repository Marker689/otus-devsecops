# Домашнее задание
Cборка первого Docker образа

## Цель:
Научиться конвейеризировать приложения


## Описание/Пошаговая инструкция выполнения домашнего задания:
```bash
docker build -t otusapp .
```
Лог:
```bash
Sending build context to Docker daemon 2.193MB
Step 1/3 : FROM scratch
--->
Step 2/3 : COPY app.bin /
---> Using cache
---> 66535cd5be8d
Step 3/3 : CMD /app.bin
---> Using cache
---> b2c25c8fe693
Successfully built b2c25c8fe693
Successfully tagged otusapp:latest
REPOSITORY TAG IMAGE ID CREATED SIZE
otusapp latest b2c25c8fe693 4 minutes ago 2.19MB
```
```dockerfile
# Инструкция CMD используется для указания исполняемого файла, который будет запускаться при старте контейнера, то есть когда вы делаете docker container run <имя_образа>:
FROM scratch
COPY app.bin /
CMD ["/app.bin"]
```
### Итоговый Dockerfile
Подробная информация о командах Docker есть в официальной документации.

Необходимо собрать образ на базе /app.bin

app.bin взять здесь: https://drive.google.com/file/d/19dImTW_d-JPKqy2TCRws_D1HH9dPba0C/view?usp=sharing

(Это простой бинарник собранный из Go для тестового испольозвания в рамках докер образа без зависимостей)


В реузльтате необходимо прислать правильно написанный Dockerfile
### Успешный результат сборки
Успешный результат запуска Docker контейнера

2*) docker-compose - необходимо оформить compose-файл: docker-compose.yml

Содержимое должно выполнять задачу аналогично первому задания, однако должно поднимать 2 таких контейнера с разными именами

Успешный результат запуска Docker-compose и docker-compose файл

# Ответ
Dockerfile
```dockerfile
FROM scratch
COPY app.bin /
CMD ["/app.bin"]
```
Сборка контейнера:
```bash
root@otus:~/otus-docker# docker build -t otusapp .
Sending build context to Docker daemon  2.205MB
Step 1/3 : FROM scratch
 --->
Step 2/3 : COPY app.bin /
 ---> 26bbe52c62f8
Step 3/3 : CMD ["/app.bin"]
 ---> Running in d5daa79f1e25
Removing intermediate container d5daa79f1e25
 ---> 25bf67c44b4b
Successfully built 25bf67c44b4b
Successfully tagged otusapp:latest
```
Запуск контейнера:
```bash
root@otus:~/otus-docker# docker run --rm otusapp
2023/01/14 14:21:43 it looks like you not in container
```

docker-compose.yaml:
```yaml
version: '3.1'

services:

  app1:
    image: otusapp
  app2:
    image: otusapp
```
Запуск docker-compose:
```bash
root@otus:~/otus-docker# docker-compose up
Creating network "otus-docker_default" with the default driver
Creating otus-docker_app2_1 ... done
Creating otus-docker_app1_1 ... done
Attaching to otus-docker_app1_1, otus-docker_app2_1
app1_1  | 2023/01/14 14:23:11 it looks like you not in container
app2_1  | 2023/01/14 14:23:11 it looks like you not in container
otus-docker_app1_1 exited with code 1
otus-docker_app2_1 exited with code 1
```