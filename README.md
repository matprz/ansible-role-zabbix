Zabbix Role
=========

Role for installing and configuring Zabbix 5.0

Requirements
------------

At this moment the script only works on Redhat/Centos 8.x. This script works with MySQL, MariaDB, Nginx and PHP.

Role Variables
--------------

All variables can be altered in the defaults directory.

defaults/main.yml
vars/RedHat.yml

Dependencies
------------

* Database like mysql or postgresql needs to be installed
* Nginx or another webserver needs to be installed

Example Playbook
----------------

```yaml
# file: test.yml
- hosts: zabbixserver
  become: yes
  remote_user: vagrant
  roles:
    - ansible-role-zabbix
```

License
-------

GPLv3

Author Information
------------------

This role was created in 2021 by Matheus Perez.