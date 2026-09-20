# Ansible Playbook — ClickHouse, Vector, Lighthouse

## Author

**Демин Илья Викторович**

## Description

Ansible playbook for automated installation and configuration of:

- ClickHouse
- Vector
- Lighthouse

The playbook is based on Ansible roles. ClickHouse uses an external Ansible role, while Vector and Lighthouse are custom roles stored in separate Git repositories.

## Roles

### ClickHouse

The playbook uses the external `ansible-clickhouse` role.

Repository:

https://github.com/AlexeySetevoi/ansible-clickhouse

Version:

```text
1.13
