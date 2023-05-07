Создать Ansible роль для обновления операционной системы:
Для этого создадим директорию ansible в домашней директории и перейдем в нее:


mkdir ~/ansible
cd ~/ansible
Создадим структуру директорий для нашей роли:


mkdir -p roles/os_update/{tasks,handlers}
Затем создадим файл tasks/main.yml, где опишем задачи, которые необходимо выполнить для обновления операционной системы:


touch roles/os_update/tasks/main.yml
Откроем файл main.yml в текстовом редакторе и добавим следующий код:

yaml
Copy code
---
- name: Update OS packages
  become: true
  apt:
    upgrade: dist
  when: ansible_os_family == 'Debian'

- name: Update OS packages
  become: true
  yum:
    name: '*'
    state: latest
  when: ansible_os_family == 'RedHat'

Здесь мы используем модули apt или yum для обновления операционной системы в зависимости от ее семейства.

Создать inventory файл и добавить в него группу servers:
Для этого создадим файл inventory в директории ansible:


cd ~/ansible
touch inventory
Откроем файл в текстовом редакторе и добавим в него следующее содержимое:

[servers]
localhost ansible_connection=local
Здесь мы определяем группу servers и указываем localhost как хост, к которому будет применяться наша роль. Мы также указываем ansible_connection=local, чтобы сообщить Ansible, что наш хост находится на локальной машине.

Создать playbook для запуска роли:

Для этого создадим файл playbook.yml в директории ansible:


touch playbook.yml
Откроем файл в текстовом редакторе и добавим следующий код:

yaml
Copy code
---
- name: Update OS
  hosts: servers
  roles:
    - role: os_update
Здесь мы определяем имя нашего playbook, группу хостов и роль, которую мы хотим применить.

Запустить playbook:

ansible-playbook -i inventory playbook.yml
Эта команда запустит наш playbook и применит роль os_update к хосту localhost. Ansible выполнит задачи из файла tasks/main.yml, обновит операционную систему и завершит свою работу.
