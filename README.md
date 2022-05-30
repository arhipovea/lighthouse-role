Lighthouse Role
=========

Разворачивает Lighthouse.

Requirements
------------

- Git

Role Variables
--------------

`lighthouse_dest`: путь до папки с сайтом

Dependencies
------------

- Nginx or other web-server

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
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

Arhipov Evgeniy
