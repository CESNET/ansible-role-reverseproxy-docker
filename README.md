ansible-role-reverseproxy-docker
=========

Ansible role for running reverseproxy based on nginx in docker.

* In default configuration logs to syslog.

Requirements
------------

* community.docker collection is required. Can be installed by `ansible-galaxy collection install community.docker`
* For use docker containers from another repository than from docker hub, you can use [ansible-role-aai-docker-environment](https://github.com/CESNET/ansible-role-aai-docker-environment)

Role Variables
--------------

* reverseproxy_path - The main path where all files will be stored
  * Default value: "/opt/reverseproxy"
* reverseproxy_docker_image - Docker image with tag (Used only by the default-docker-compose.yml.j2 template)
  * Default value: "nginx:latest"
* reverseproxy_docker_network - External docker network, that will be used for connect (Used only by the default-docker-compose.yml.j2 template)
  * Default value: "network"
* reverseproxy_docker_compose_template - The docker-compose template which will be used
  * Default value: "default-docker-compose.yml"
* reverseproxy_dirs - List of directories, that will be created on host
  * Example value:
  ```
  - { path: "{{ reverseproxy_path }}" }
  - { path: "{{ reverseproxy_path }}/etc" }
    where:
      path - Directive path; required
      Default owner: root
      Default group: root
      Default mode: 0755
  ```
* reverseproxy_files - List of files, that will be copyied to host
  * Example value:
  ```
  - { src: "reverseproxy/etc/nginx/nginx-tls.conf", dest: "/etc/nginx/nginx-tls.conf" }
    where:
      src - path to source file; required
      dest - relative to {{ reverseproxy_path }} - default value {{ src }}
      Default owner: root
      Default group: root
      Default mode: 0644
  ```
  - { src: "etc/nginx/nginx.conf", dest: "/etc/nginx/nginx.conf"}
* reverseproxy_templates - List of files, that will be created by template
  * Example value:
  ```
  - { src: "reverseproxy/etc/nginx/nginx.conf", dest: "/etc/nginx/nginx.conf", owner: "root", group: "root", mode: 0755 }
    where:
      src - path to the template file without .j2; required
      dest - relative to {{ reverseproxy_path }} - default value {{ src }}
      Default owner: root
      Default group: root
      Default mode: 0644
  ```
* reverseproxy_site_specific_tasks - Path to the additional file with specific tasks
  * Not required
  * Default: None

Dependencies
------------

* collection `community.docker`
* [ansible-role-aai-docker-environment](https://github.com/CESNET/ansible-role-aai-docker-environment) (Optional)

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: ansible-role-reverseproxy-docker }

