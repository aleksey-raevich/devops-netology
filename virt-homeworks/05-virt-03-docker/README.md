## Введение. Экосистема. Архитектура. Жизненный цикл Docker-контейнера

### Задача 1
```text
Сценарий выполнения задачи:

создайте свой репозиторий на https://hub.docker.com;
выберите любой образ, который содержит веб-сервер Nginx;
создайте свой fork образа;
реализуйте функциональность: запуск веб-сервера в фоне с индекс-страницей, содержащей HTML-код ниже:
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I’m DevOps Engineer!</h1>
</body>
</html>
```

https://hub.docker.com/repository/docker/araevich/nginx/general


### Задача 2
```text
Посмотрите на сценарий ниже и ответьте на вопрос: «Подходит ли в этом сценарии использование Docker-контейнеров или лучше подойдёт виртуальная машина, физическая машина? Может быть, возможны разные варианты?»
Детально опишите и обоснуйте свой выбор.
```

Сценарий:  
* высоконагруженное монолитное Java веб-приложение:
```text
Если приложение может масштабироваться, т.е. работать через балансировщик, то в контейнерах будет удобнее.
Плюсом контейнеризации является так же возможность добавления нужных библиотек или версии JVM непосредственно в контейнер.
При выкатке нового релиза, не придется обновлять все библиотеки на ВМ или физическом сервере.

Выбирая ВМ или физический сервер я бы выбрал ВМ. Во первых можно добавлять ресурсы при росте нагрузки.
Во вторых ВМ может быть запущена внутри кластера (Hyper-V, OpenStack), что позволит ей быть высокодоступной.

Запускать одно приложение, даже высоконагруженное на целом физическом сервере не вижу смысла.
Есть проблемы со сменой библиотек и версии JVM. Нет отказоустойчивости / высокодоступности приложения.
Возможность масштабирования приложения ограничено физическими ресурсами самого сервера.
```
* Nodejs веб-приложение:
```text
Для веб-приложение docker подойдет хорошо: приложение и все нужные библиотеки изолируются в контейнере.
Можно масштабировать приложение с использованием механизмов балансировки нагрузки.
Удобство использования CI/CD пайплайнов для выкатки новых версий кода. 
```
* мобильное приложение c версиями для Android и iOS;
```text
То что нашел:
Android приложения: Running Docker on Android is definitely not supported officially and you cannot run any official version on Android without modifying Docker or Android.
iOs приложения: для запуска приложения нужен macOs с Xcode, доступность образов macOs даже при использовании Docker на Mac ограничена

Использование docker не регламентировано, т.к. для запуска и тестирования мобильных приложений используются специализированные программные продукты - эмуляторы.
```
* шина данных на базе Apache Kafka;
```text
Для разработки и тестирования приложений для работы с kafka использовать docker очень удобно: Spring Boot + Apache Kafka и SSL.
Для production сред, Kafka устанавливается непосредственно на сервер физический или виртуальный и собирается в высокодоступный кластер.
```
* Elasticsearch-кластер для реализации логирования продуктивного веб-приложения — три ноды elasticsearch, два logstash и две ноды kibana;
```text
Можно использовать docker и собрать кластер сервисов в docker.
Не нужно создавать отдельную ВМ под каждый сервис.
```
* мониторинг-стек на базе Prometheus и Grafana;
```text
Оптимально использовать docker / docker-compose.
```
* MongoDB как основное хранилище данных для Java-приложения;
```text
Вполне подойдет Docker, есть много готовых образов которые можно использовать.
```
* Gitlab-сервер для реализации CI/CD-процессов и приватный (закрытый) Docker Registry.
```text
При запуске пайплайнов и сборке образов GitLab потребляет довольно много вычислительных ресурсов, плюс он не масштабируется.
После сборки образов из registry копируются в целевые среды (dev, preprod, prod) соответственно между машиной с GibLab и нужными средами должна быть организована сетевая доступность и открыты нужные порты.
Оптимальнее будет разместить его на отдельной ВМ, для который может быть обеспечена высокодоступность и выполнены необходимые сетевые настройки / ограничения.
```


### Задача 3
```text
Запустите первый контейнер из образа centos c любым тегом в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера.
Запустите второй контейнер из образа debian в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера.
Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data.
Добавьте ещё один файл в папку /data на хостовой машине.
Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.
```
* Запускаем контейнеры:
```text
docker run -it -d --name centos -v /data:/data centos:latest
docker run -it -d --name debian -v /data:/data debian:stable
```
![Скриншот](https://github.com/aleksey-raevich/devops-netology/blob/master/virt-homeworks/05-virt-03-docker/lab_05-virt-03-docker_img1.png)

* Подключаемся к первому контейнеру и создаем файл
```text
vagrant@server1:/$ docker exec -it centos bash
[root@0f3ec10f8473 /]# echo "Container with Centos" > /data/message
[root@0f3ec10f8473 /]# cat /data/message
Container with Centos
```
![Скриншот](https://github.com/aleksey-raevich/devops-netology/blob/master/virt-homeworks/05-virt-03-docker/lab_05-virt-03-docker_img2.png)

* Добавляем новый файл с хостовой машины
```text
vagrant@server1:/$ echo "Test message from host" | sudo tee /data/message_host
Test message from host
vagrant@server1:/$ cat /data/message_host
Test message from host
```
![Скриншот](https://github.com/aleksey-raevich/devops-netology/blob/master/virt-homeworks/05-virt-03-docker/lab_05-virt-03-docker_img3.png)

* Подключаемся во второй контейнер и выводим листинг файлов
```text
vagrant@server1:/$ docker exec -it debian bash
root@9312497b05fb:/# cat /data/message
Container with Centos
root@9312497b05fb:/# cat /data/message_host
Test message from host
```
![Скриншот](https://github.com/aleksey-raevich/devops-netology/blob/master/virt-homeworks/05-virt-03-docker/lab_05-virt-03-docker_img4.png)


### Задача 4
```text
Воспроизведите практическую часть лекции самостоятельно.
Соберите Docker-образ с Ansible, загрузите на Docker Hub и пришлите ссылку вместе с остальными ответами к задачам.
```
https://hub.docker.com/repository/docker/araevich/ansible/general