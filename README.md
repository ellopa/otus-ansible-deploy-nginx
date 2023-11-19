ДЗ-3. Автоматизация администрирования. Ansible
 
 Подготовлен стенд на Vagrant с одним сервером . 
 На этом сервере используя Ansible разворачивается nginx со следующими условиями:

    Используется модуль yum (RedHat Linux)
    Конфигурационные файлы берутся из шаблона jinja2 с перемененными (templates)
    После установки nginx должен быть в режиме enabled в systemd
    Используется notify для старта nginx после установки
    Сайт слушает на нестандартном порту - 8080 (сделано через переменные в Ansible)

Для запуска требуется установленный Ansible на хосте.
    - Создан каталог Ansible и в нем размещен Vagrantfile
    - Поднят управлāемый хост командой vagrant up, проверен, что все прошло успешно и есть доступ по ssh.
    
### Roles
Создание каталога с ролями:
```
ansible-galaxy init deploy_nginx
ansible-galaxy init deploy_epel

d── roles
│   ├── deploy_epel
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── files
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── meta
│   │   │   └── main.yml
│   │   ├── README.md
│   │   ├── tasks
│   │   │   └── main.yml
│   │   ├── templates
│   │   ├── tests
│   │   │   ├── inventory
│   │   │   └── test.yml
│   │   └── vars
│   │       └── main.yml
│   └── deploy_nginx
│       ├── defaults
│       │   └── main.yml
│       ├── files
│       ├── handlers
│       │   └── main.yml
│       ├── meta
│       │   └── main.yml
│       ├── README.md
│       ├── tasks
│       │   └── main.yml
│       ├── templates
│       │   └── nginx.conf.j2
│       ├── tests
│       │   ├── inventory
│       │   └── test.yml
│       └── vars
│           └── main.yml


```

### Playbook:

nginx_old.yml
nginx.yml #с ролями

```
```

### Запуск:

ansible-playbook nginx.yml

PLAY [NGINX | Install and configure NGINX] ****************************************************************************

TASK [Gathering Facts] ************************************************************************************************
ok: [nginx]

TASK [deploy_epel : NGINX_EPEL | Install EPEL Repo package from standart repo] ****************************************
ok: [nginx]

TASK [deploy_nginx : NGINX | Install NGINX package from EPEL Repo] ****************************************************
changed: [nginx]

TASK [deploy_nginx : NGINX | Create NGINX config file from template] **************************************************
changed: [nginx]

RUNNING HANDLER [deploy_nginx : restart nginx] ************************************************************************
changed: [nginx]

RUNNING HANDLER [deploy_nginx : reload nginx] *************************************************************************
changed: [nginx]

PLAY RECAP ************************************************************************************************************
nginx                      : ok=6    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  

```
```
### Проверка порт 8080

elena_leb@ubuntunbleb:~/Ansible$ curl http://192.168.56.2:8080/
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
  <title>Welcome to CentOS</title>
  <style rel="stylesheet" type="text/css"> 

	html {
	background-image:url(img/html-background.png);
	background-color: white;
	font-family: "DejaVu Sans", "Liberation Sans", sans-serif;
	font-size: 0.85em;
	line-height: 1.25em;
....

```

