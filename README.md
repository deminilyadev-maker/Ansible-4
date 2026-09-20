# Ansible Playbook — ClickHouse, Vector, Lighthouse

## Author

**Демин Илья Викторович**

## Description

Ansible playbook for automated installation and configuration of:

- ClickHouse
- Vector
- Lighthouse

The playbook is based on Ansible roles.

ClickHouse uses an external Ansible role, while Vector and Lighthouse are custom roles stored in separate Git repositories.

## Roles

### ClickHouse

The playbook uses the external `ansible-clickhouse` role.

Repository:

https://github.com/AlexeySetevoi/ansible-clickhouse

Version:

```text
1.13
```

### Vector

Vector is installed using a custom Ansible role.

Repository:

https://github.com/deminilyadev-maker/vector-role

Version:

```text
1.0.0
```

The role performs the following actions:

- creates the Vector installation directory;
- downloads the specified Vector version;
- extracts the Vector archive.

### Lighthouse

Lighthouse is installed and configured using a custom Ansible role.

Repository:

https://github.com/deminilyadev-maker/lighthouse-role

Version:

```text
1.0.0
```

The role performs the following actions:

- installs NGINX;
- configures NGINX;
- installs Git;
- downloads Lighthouse from the VKCOM repository;
- configures NGINX to serve Lighthouse.
