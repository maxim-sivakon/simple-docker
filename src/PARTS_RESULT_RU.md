## Part 1. Готовый докер



##### Взять официальный докер образ с **nginx** и выкачать его при помощи `docker pull`

![s](assets/images/part1_01.png)

##### Проверить наличие докер образа через `docker images`

![s](assets/images/part1_02.png)

##### Запустить докер образ через `docker run -d [image_id|repository]`

![s](assets/images/part1_03.png)

##### Проверить, что образ запустился через `docker ps`

![s](assets/images/part1_04.png)

##### Посмотреть информацию о контейнере через `docker inspect [container_id|container_name]`

![s](assets/images/part1_05.png)

##### По выводу команды определить и поместить в отчёт размер контейнера, список замапленных портов и ip контейнера

![s](assets/images/part1_06.png)
![s](assets/images/part1_07.png)
![s](assets/images/part1_08.png)

##### Остановить докер образ через `docker stop [container_id|container_name]`

![s](assets/images/part1_09.png)

##### Проверить, что образ остановился через `docker ps`

![s](assets/images/part1_10.png)

##### Запустить докер с портами 80 и 443 в контейнере, замапленными на такие же порты на локальной машине, через команду *run*

![s](assets/images/part1_11.png)
![s](assets/images/part1_12.png)

##### Проверить, что в браузере по адресу *localhost:80* доступна стартовая страница **nginx**

![s](assets/images/part1_13.png)

##### Перезапустить докер контейнер через `docker restart [container_id|container_name]`

![s](assets/images/part1_14.png)

##### Проверить любым способом, что контейнер запустился

![s](assets/images/part1_15.png)

## Part 2. Операции с контейнером



##### Прочитать конфигурационный файл *nginx.conf* внутри докер контейнера через команду *exec*

![s](assets/images/part2_01.png)

##### Создать на локальной машине файл *nginx.conf*
##### Настроить в нем по пути */status* отдачу страницы статуса сервера **nginx**

![s](assets/images/part2_02.png)

##### Скопировать созданный файл *nginx.conf* внутрь докер образа через команду `docker cp`

![s](assets/images/part2_03.png)

##### Перезапустить **nginx** внутри докер образа через команду *exec*

![s](assets/images/part2_04.png)

##### Проверить, что по адресу *localhost:80/status* отдается страничка со статусом сервера **nginx**

![s](assets/images/part2_05.png)

##### Экспортировать контейнер в файл *container.tar* через команду *export*

![s](assets/images/part2_06.png)

##### Остановить контейнер

![s](assets/images/part2_07.png)


##### Удалить образ через `docker rmi [image_id|repository]`, не удаляя перед этим контейнеры

![s](assets/images/part2_08.png)

##### Удалить остановленный контейнер

![s](assets/images/part2_09.png)

##### Импортировать контейнер обратно через команду *import*

![s](assets/images/part2_10.png)

##### Запустить импортированный контейнер

![s](assets/images/part2_11.png)

##### Проверить, что по адресу *localhost:80/status* отдается страничка со статусом сервера **nginx**

![s](assets/images/part2_12.png)

## Part 3. Мини веб-сервер

##### Написать мини сервер на **C** и **FastCgi**, который будет возвращать простейшую страничку с надписью `Hello World!`
![s](assets/images/part3_01.png)

##### Написать свой *nginx.conf*, который будет проксировать все запросы с 81 порта на *127.0.0.1:8080*
![s](assets/images/part3_02.png)

Запускаем контейнер
> docker run -d -p 81:81 nginx

Копируем в контейнер nginx.conf и main.c
> docker cp nginx/nginx.conf flamboyant_feynman:/etc/nginx

> docker cp main.c flamboyant_feynman:/home

![s](assets/images/part3_03.png)

Подсключаемся к консоли контейнера
> docker exec -it [image_id|repository] bash

Для выхода из консоли используем
> exit

Обновляем образ и устанавливаем gcc, spawn-fcig и libfcgi-dev
> apt-get update && install -y gcc spawn-fcgi libfcgi-dev

##### Запустить написанный мини сервер через *spawn-fcgi* на порту 8080
![s](assets/images/part3_04.png)
##### Проверить, что в браузере по *localhost:81* отдается написанная вами страничка
![s](assets/images/part3_05.png)

## Part 4. Свой докер

#### Написать свой докер образ, который:
##### 1) собирает исходники мини сервера на FastCgi из [Части 3](#part-3-мини-веб-сервер)
##### 2) запускает его на 8080 порту
##### 3) копирует внутрь образа написанный *./nginx/nginx.conf*
##### 4) запускает **nginx**.

![s](assets/images/part3_06.png)

Копируем в наш контейнер файлы

![s](assets/images/part3_07.png)

##### Собрать написанный докер образ через `docker build` при этом указав имя и тег

![s](assets/images/part3_08.png)

##### Проверить через `docker images`, что все собралось корректно

![s](assets/images/part3_09.png)

##### Запустить собранный докер образ с маппингом 81 порта на 80 на локальной машине и маппингом папки *./nginx* внутрь контейнера по адресу, где лежат конфигурационные файлы **nginx**'а (см. [Часть 2](#part-2-операции-с-контейнером))

![s](assets/images/part3_10.png)

##### Проверить, что по localhost:80 доступна страничка написанного мини сервера

![s](assets/images/part3_11.png)

##### Дописать в *./nginx/nginx.conf* проксирование странички */status*, по которой надо отдавать статус сервера **nginx**

![s](assets/images/part3_12.png)

##### Перезапустить докер образ

![s](assets/images/part3_13.png)

##### Проверить, что теперь по *localhost:80/status* отдается страничка со статусом **nginx**

![s](assets/images/part3_14.png)



## Part 5. **Dockle**

##### Просканировать образ из предыдущего задания через `dockle [image_id|repository]`

Если нет dockle
> brew install goodwithtech/r/dockle

![s](assets/images/part3_15.png)

##### Исправить образ так, чтобы при проверке через **dockle** не было ошибок и предупреждений

![s](assets/images/part5_01.png)
![s](assets/images/part5_02.png)
![s](assets/images/part5_03.png)

## Part 6. Базовый **Docker Compose**

##### Написать файл *docker-compose.yml*, с помощью которого:
##### 1) Поднять докер контейнер из [Части 5](#part-5-инструмент-dockle) _(он должен работать в локальной сети, т.е. не нужно использовать инструкцию **EXPOSE** и мапить порты на локальную машину)_
##### 2) Поднять докер контейнер с **nginx**, который будет проксировать все запросы с 8080 порта на 81 порт первого контейнера
##### Замапить 8080 порт второго контейнера на 80 порт локальной машины

![s](assets/images/part6_01.png)
![s](assets/images/part6_02.png)

##### Остановить все запущенные контейнеры

> docker stop [image_id|repository]

![s](assets/images/part6_04.png)

##### Собрать и запустить проект с помощью команд `docker-compose build` и `docker-compose up`

![s](assets/images/part6_05.png)
![s](assets/images/part6_06.png)

##### Проверить, что в браузере по *localhost:80* отдается написанная вами страничка, как и ранеe

![s](assets/images/part3_11.png)