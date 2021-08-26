# Ansible Role: Docker Swarm Management

This Ansible role manages Docker Swarm stacks, volumes, configs, networks and secrets.

## Requirements

We are using the [pre-commit](https://pre-commit.com/) tool in order to perform some checks on the committed files.
Make sure to execute `pre-commit install` before contributing to this repo!

## Role Variables

1. docker_networks
1. docker_volumes
1. docker_configs
1. docker_configs_no_log
1. docker_secrets
1. docker_secrets_no_log
1. docker_stacks

## Dependencies

### Collections

* Docker - <https://galaxy.ansible.com/community/docker> <https://docs.ansible.com/ansible/latest/collections/community/docker/>

Install them by executing `ansible-galaxy install -r requirements.yml`.

## Examples

## License

MIT / BSD

## Author Information

This role was created in 2021 by Evangelos Karvounis.
