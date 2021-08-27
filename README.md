# Ansible Role: Docker Swarm Management

This Ansible role manages Docker Swarm stacks, volumes, configs, networks and secrets.

## Requirements

We are using the [pre-commit](https://pre-commit.com/) tool in order to perform some checks on the committed files.
Make sure to execute `pre-commit install` before contributing to this repo!

## Role Variables

1. docker_networks: List of Docker network objects. Defaults to [].
2. docker_volumes: List of Docker volume objects. Defaults to [].
3. docker_configs: List of Docker config objects. Defaults to [].
4. docker_configs_no_log: Flag that defines whether or not Ansible logs the configs result. Defaults to true.
5. docker_secrets: List of Docker secret objects. Defaults to [].
6. docker_secrets_no_log: Flag that defines whether or not Ansible logs the secrets result. Defaults to true.
7. docker_stacks: List of Docker secret objects. Defaults to [].

## Dependencies

### Collections

* Docker - <https://galaxy.ansible.com/community/docker> <https://docs.ansible.com/ansible/latest/collections/community/docker/>

Install them by executing `ansible-galaxy install -r requirements.yml`.

## Example variables

```yaml
docker_networks:
  - name: network_one
    state: present
    driver: overlay
  - name: network_two
    ipam_config:
      - subnet: 172.3.27.0/24
        gateway: 172.3.27.2
        iprange: 172.3.27.0/26
        aux_addresses:
          host1: 172.3.27.3
          host2: 172.3.27.4
  - name: network_three
    state: absent
    force: yes
  - name: network_four
    labels:
      key1: value1
      key2: value2

docker_volumes:
  - name: volume_one
    state: present
  - name: volume_two
    labels:
      key1: value1
      key2: value2
    driver_options:
      type: btrfs
      device: /dev/sda2
  - name: volume_three
    state: absent

docker_secrets:
  - name: foo
    # If the file is JSON or binary, Ansible might modify it (because
    # it is first decoded and later re-encoded). Base64-encoding the
    # file directly after reading it prevents this to happen.
    data: "{{ lookup('file', '/path/to/secret/file') | b64encode }}"
    data_is_b64: true
    state: present
    # Force the removal/creation of the secret
  - name: foo
    data: Goodnight everyone!
    force: yes
    state: present
    # Remove secret foo
  - name: foo
    state: absent

docker_configs:
  - name: config_one
    data: |
      mongo:
        dsn: mongodb://admin:password@mymongo.dev/admin
        database_name: db
      health-check-endpoint: /healthcheck
      service-port: 9999
  - name: config_two
    data: "{{ lookup('template', 'path_to_config_two_template.yaml') }}"
  - name: config_three
    data: "{{ lookup('file', 'path_to_config_three_file.yaml') }}"

docker_stacks:
  # Deploy stack from a compose file. You can use here the lookup function
  - name: stack_one
    state: present
    compose:
      - /opt/stack_one.yml
  # Deploy stack from base compose file and override the web service
  - name: stack_two
    state: present
    compose:
      - /opt/stack_two.yml
      - version: '3'
        services:
          web:
            image: nginx:latest
            environment:
              ENVVAR: envvar
  # Remove stack
  - name: stack_three
    state: absent
  # Deploy stack from a j2 compose file
  - name: stack_four
    state: present
    compose:
      - "{{ lookup('template', 'path_to_stack_four_template_file.yml.j2') | from_yaml }}"
```

## Advanced examples

You can find an advanced tutorial in the repo <https://github.com/karvounis/ansible-docker-swarm-management-role-example>.

## License

MIT

## Author Information

This role was created in 2021 by Evangelos Karvounis.
