## PostgreSQL

### Задача 1

<details><summary>Описание задачи 1</summary>
Легенда

Через час совещание, на котором менеджер расскажет о новом проекте. Начать работу над проектом нужно будет уже сегодня. Сейчас известно, что это будет сервис, который ваша компания будет предоставлять внешним заказчикам. Первое время, скорее всего, будет один внешний клиент, со временем внешних клиентов станет больше.

Также по разговорам в компании есть вероятность, что техническое задание ещё не чёткое, что приведёт к большому количеству небольших релизов, тестирований интеграций, откатов, доработок, то есть скучно не будет.

Вам как DevOps-инженеру будет нужно принять решение об инструментах для организации инфраструктуры. В вашей компании уже используются следующие инструменты:

остатки Сloud Formation,
некоторые образы сделаны при помощи Packer,
год назад начали активно использовать Terraform,
разработчики привыкли использовать Docker,
уже есть большая база Kubernetes-конфигураций,
для автоматизации процессов используется Teamcity,
также есть совсем немного Ansible-скриптов,
ряд bash-скриптов для упрощения рутинных задач.
На совещании нужно будет выяснить подробности о проекте, чтобы определиться с инструментами:

Какой тип инфраструктуры будем использовать для этого проекта: изменяемый или не изменяемый?
Будет ли центральный сервер для управления инфраструктурой?
Будут ли агенты на серверах?
Будут ли использованы средства для управления конфигурацией или инициализации ресурсов?
Так как проект стартует уже сегодня, на совещании нужно будет определиться со всеми этими вопросами.

Вам нужно:

Ответить на четыре вопроса из раздела «Легенда».
Решить, какие инструменты из уже используемых вы хотели бы применить для нового проекта.
Определиться, хотите ли рассмотреть возможность внедрения новых инструментов для этого проекта.
Если для ответов на эти вопросы недостаточно информации, напишите, какие моменты уточните на совещании.
</details>

* Какой тип инфраструктуры будем использовать для этого проекта: изменяемый или не изменяемый?

Предложу неизменяемый тип инфраструктуры.
В ТЗ написано, что будет много релизов и есть информация, что команда активно использует docker.
Значит можно с использованием Packer/Terraform и разворачивать ВМ под kubernetes.
Два шаблона ВМ под ноду-coordinator и второй под worker ноды.
Раскатка сервисов Kubernetes может быть выполнена посредством kubespray - готовых ansible ролей.
Если будем упираться по ресурсам по мере развития проекта, то новые worker ноды добавляются по уже готовым шаблонам.

* Будет ли центральный сервер для управления инфраструктурой?

Острой необходимости в этом нет.
Шаблоны конфигураций обычно сохраняются в репозитории git.
Непосредственное управление можно выполнять и с рабочих пользовательских машин.

* Будут ли агенты на серверах?

Нет необходимости, можно использовать ssh

* Будут ли использованы средства для управления конфигурацией или инициализации ресурсов?

Команда использует Terraform, его и будем использовать.

* Решить, какие инструменты из уже используемых вы хотели бы применить для нового проекта:

Packer, Terraform, Docker, Kubernetes, Kubespray/Ansible

* Определиться, хотите ли рассмотреть возможность внедрения новых инструментов для этого проекта:

С TeamCity никогда не работал.
Лучше отказаться в пользу GitLab CI/CD - большое комьюнити, много готовых шаблонов, примеров.
Специалистов на рынке, умеющих работать с GitLab, думаю, на порядок больше.


### Задача 2 Установка Terraform

<details><summary>Описание задачи 2</summary>
Официальный сайт Terraform.
В связи с недоступностью ресурсов для загрузки Terraform на территории РФ вы можете воспользоваться VPN или использовать зеркало YandexCloud:
ссылки для установки открытого ПО

Установите Terraform при помощи менеджера пакетов, используемого в вашей операционной системе. В виде результата этой задачи приложите вывод команды terraform --version.
</details>

* В виде результата этой задачи приложите вывод команды terraform --version:

```text
Terraform v1.5.3
on darwin_arm64
```


### Задача 3 Поддержка legacy-кода

<details><summary>Описание задачи 3</summary>
В какой-то момент вы обновили Terraform до новой версии, например с 0.12 до 0.13. Код одного из проектов настолько устарел, что не может работать с версией 0.13. Нужно сделать так, чтобы вы могли одновременно использовать последнюю версию Terraform, установленную при помощи штатного менеджера пакетов, и устаревшую версию 0.12.

В виде результата этой задачи приложите вывод --version двух версий Terraform, доступных на вашем компьютере или виртуальной машине.
</details>

Инструкция по установке и использованию <a href="https://jhooq.com/install-terrafrom/">tfenv</a>

![Скриншот](https://github.com/aleksey-raevich/devops-netology/blob/master/virt-homeworks/07-terraform-01-intro/lab_07-terraform-01-intro_img1.png)
