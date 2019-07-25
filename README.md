Cloudweeb Alerta
=========

[![Build Status](https://travis-ci.com/cloudweeb/cloudweeb.alerta.svg?branch=master)](https://travis-ci.com/cloudweeb/cloudweeb.alerta)

Ansible role to install Alerta

Requirements
------------

None

Role Variables
--------------

A description of the settable variables for this role should go here, including
any variables that are in defaults/main.yml, vars/main.yml, and any variables
that can/should be set via parameters to the role. Any variables that are read
from other roles and/or the global scope (ie. hostvars, group vars, etc.) should
be mentioned here as well.

Dependencies
------------

- geerlingguy.nginx
- cloudweeb.mongodb

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

    - hosts: servers
      vars:

        nginx_remove_default_vhost: true

        nginx_vhosts:
          - listen: "80"
            server_name: "alerta.cloudweeb.com"
            extra_parameters: |
              location /api { try_files $uri @api; }
              location @api {
                  include uwsgi_params;
                  uwsgi_pass unix:/tmp/uwsgi.sock;
                  proxy_set_header Host $host:$server_port;
                  proxy_set_header X-Real-IP $remote_addr;
                  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
              }

              location / {
                  root /opt/alerta/app;
                  try_files $uri $uri/ /index.html;
              }

      roles:

        - role: cloudweeb.mongodb

        - role: geerlingguy.nginx

        - role: cloudweeb.alerta

License
-------

MIT

Author Information
------------------

Agnesius Santo Naibaho
