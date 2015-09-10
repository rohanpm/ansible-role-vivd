vivd
====

This role deploys and configures the
[vivd container testing tool](https://github.com/rohanpm/vivd).

Requirements
------------

- docker must be installed and enabled on the target and must be configured for
  passwordless use by members of the "docker" group

- any desired firewall or port forwarding must be configured separately from
  this role

- this role will ensure a major version of Java is installed, but does not
  specify the minor version (and will not upgrade Java)

Role Variables
--------------

Many variables directly map to vivd configuration items, which are documented
[here](https://github.com/rohanpm/vivd/blob/master/README.asciidoc).

```yaml
---
# MUST set these, no reasonable defaults
vivd_git_url: git://code.example.com/my-project
vivd_docker_source_image: my.registry/my-project:latest
vivd_docker_code_path: /code

# MAY set these:

vivd_docker_entrypoint:
  - /bin/sh
  - -c

vivd_docker_cmd:
  - exec /code/start-my-project

vivd_docker_run_arguments
  - --privileged
  - -v
  - /some-volume

vivd_version: 1.3.2
vivd_sha256sum: fe647f478f53b87d34c34d65e44b1fab609a6c61da4037b34be1b204ea10f22b
vivd_basename: vivd-{{ vivd_version }}-standalone.jar
vivd_url: https://dl.bintray.com/rohanpm/generic/{{ vivd_basename }}

vivd_java_package: java-1.7.0-openjdk-headless
vivd_java_cmd: /usr/lib/jvm/jre-1.7.0/bin/java

vivd_user: vivd
vivd_directory: /srv/vivd
vivd_data_directory: "{{ vivd_directory }}/data"

vivd_startup_timeout: 180
vivd_title: Containers
vivd_default_url: /
vivd_max_containers: 1000
vivd_max_containers_up: 8
vivd_http_host: 0.0.0.0
vivd_http_port: 8080
vivd_docker_http_port: 80
```


Example Playbook
----------------

    - hosts: vivd
      roles:
      - role: rohanpm.vivd
        vivd_git_url: https://my-git.example.com/my-rails-project
        vivd_docker_source_image: docker-registry.example.com/my-rails-project:vivd
        vivd_docker_code_path: /code
        vivd_default_url: /my-app
        vivd_title: [vivd] - My Rails App
        vivd_docker_http_port: 3000
        vivd_docker_entrypoint:
          - /bin/bash
          - -c
        vivd_docker_cmd:
          - exec /code/misc/vivd/start
        vivd_docker_run_arguments:
          - -v
          - /data
          - -v
          - /var/lib/mysql

License
-------

GPLv3
