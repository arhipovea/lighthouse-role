Lighthouse
=========

Устанавлвает Lighthouse.

Requirements
------------

ОС: ubuntu

Role Variables
--------------

`lighthouse_dest`: путь до файлов 

Dependencies
------------

Необходим web-server

Example Playbook
----------------

```ansible
- name: Install Vector
  hosts: vector
  roles: 
    - vector
  post_tasks:
    - name: Vector | Add log directory
      ansible.builtin.file:
        path: /home/ansible/logs
        state: directory
        mode: '0755'

- name: Install Nginx
  hosts: lighthouse
  
  handlers:
    - name: Nginx | Start nginx service
      become: true
      ansible.builtin.service:
        name: nginx
        state: restarted

  pre_tasks:
    - name: Nginx | Install nginx
      become: true
      ansible.builtin.package:
        name:
          - nginx
        state: present

    - name: Nginx | Create nginx config
      become: true
      ansible.builtin.template:
        src: nginx.conf.j2
        dest: /etc/nginx/sites-available/default
        mode: "0644"
      notify: Nginx | Start nginx service
    
    - name: Nginx | Flush handlers
      ansible.builtin.meta: flush_handlers
  roles:
    - lighthouse
```
License
-------

MIT

Author Information
------------------

Arkhipov Evgeniy
