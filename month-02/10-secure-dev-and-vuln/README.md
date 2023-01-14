# Домашнее задание
Найти уязвимости в С/С++ коде

## Цель:
Научиться использовать инструменты статического и динамического анализа для поиска уязвимостей в коде на языках C/C++.


# Описание/Пошаговая инструкция выполнения домашнего задания:
## Инструкция

Для запуска окружения установите docker - get docker

1. Скачайте docker образ sdukshis/devsecops_homework 

docker pull sdukshis/devsecops_homework
2. Запустите docker контейнер

docker run --rm -ti sdukshis/devsecops_homework
3. Склонируйте репозиторий с примерами уязвимых программ
```bash
git clone https://github.com/sdukshis/devsecops-homework.git
cd devsecops-homework
```
4. С помощью code review, статического и динамического анализа найдите все уязвимости в директории src

Используйте примеры и задания с вебинара

5. В чат по ДЗ скопируйте отчет в виде таблицы:

| имя файла | номер строки | тип уязвимости |
|-----------|--------------|----------------|
