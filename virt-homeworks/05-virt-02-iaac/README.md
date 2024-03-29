## Применение принципов IaaC в работе с виртуальными машинами

### Задача 1

> Основные преимущества применения на практике IaaC-паттернов:  
#### 1. T2M (Time-to-market) & ТСО (Total cost of ownership):   
* Быстрое конфигурирование инфраструктуры, чтобы помочь командам работать эффективнее.
* Уменьшение расходов, затрачиваемое на рутинные операции.
* Эффективное использование существующих ресурсов и минимизация риска возникновения человеческой ошибки.
#### 2. Масштабируемость и стандартизация:
* Развертывания инфраструктуры с помощью IaaC повторяемы и предотвращают проблемы во время выполнения.
* IaaC полностью стандартизирует развертывания инфраструктуры, что снижает вероятность ошибок или отклонений.
#### 3. Безопасность и история изменений:
* Все службы разворачиваются и функционируют одинаково, что позволяет переиспользовать шаблоны безопасности.
* Код версионируется, соответственно IaaC позволяет документировать, регистрировать и отслеживать каждое изменение конфигурации.
#### 4. Восстановление:
Повторное развертывание последнего работоспособного состояния после сбоя.

> Какой из принципов IaaC является основополагающим?
#### Основополагающим принципом применения IaaC является "Идемпотентность".
Это свойство объекта или операции, при повторном выполнении которой мы получаем результат идентичный предыдущему и всем последующим выполнениям.

### Задача 2

> Чем Ansible выгодно отличается от других систем управление конфигурациями?
* Без установки агентов и дополнительного ПO;
* Для написания команд используется YAML (достаточно популярный, хорошо документирован, много примеров);
* Работа инструмента выполняется через SSH;
* Разработан на Python, можно написать новые модули;  
* Инструменты Chef и Puppet созданы на Ruby - сложнее в освоении, выше порог входа.
> Какой, на ваш взгляд, метод работы систем конфигурации более надёжный — push или pull?
* Метод push более надежный, т.к. в данном методе мастер-сервер сам управляет конфигурированием целевых систем. По идее это должно позволить избежать возможности ручных изменений в конфигурациях систем напрямую (нет бэкапа изменений, их последующей тиражируемости, прохождения настроенных шаблонов безопасности).

### Задача 3
```text
Установите на личный компьютер:

VirtualBox,
Vagrant,
Terraform,
Ansible.
Приложите вывод команд установленных версий каждой из программ, оформленный в Markdown.
```
Использую Mac на M1, в связке Vagrant + Parallels Desktop.  
Ранее в новостях появлялось, что в VirtualBox появится поддержка Apple silicon, но пока есть только Developer preview.
* Vagrant version:
```text
❯ vagrant --version
Vagrant 2.3.4
```
* Vagrantfile:
```text
Vagrant.configure("2") do |config|
  config.vm.box_download_insecure = true
  config.vm.box = "jeffnoxon/ubuntu-20.04-arm64"
  config.vm.hostname = "virt-basics"

  config.vm.provider "parallels" do |vp|
    vp.name = "virt-basics"
    vp.memory = 2048
    vp.cpus = 1
  end

  config.vm.provision "shell", inline: <<-SHELL
      sudo apt update && apt upgrade -y
      sudo apt install ansible -y
      sudo apt install snap -y
      sudo snap install terraform --classic
  SHELL
end
```
* Ansible version:
```text
vagrant@virt-basics:~$ ansible --version
ansible 2.9.6
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.8.10 (default, May 26 2023, 14:05:08) [GCC 9.4.0]
```
* Terraform version:
```text
vagrant@virt-basics:~$ terraform --version
Terraform v1.5.2
on linux_arm64
```

### Задача 4

```text
Создайте виртуальную машину.
```
Машинку удалось создать только после модификации файлов-примеров для связки Vagrant + Parallels Desktop.
* inventory: ansible все время выдавал ошибку недоступности хоста, перебрав разные варианты, заменил host ip на ansible_local
* Vagrantfile: как следствие совсем убрал прокидывание порта, в нем уже не было смысла
* Все файлы добавлены в репозиторий
```text
Зайдите внутрь ВМ, убедитесь, что Docker установлен с помощью команды ps a
```
![Скриншот](https://github.com/aleksey-raevich/devops-netology/blob/master/virt-homeworks/05-virt-02-iaac/lab_05-virt-02-iaac_img1.png)