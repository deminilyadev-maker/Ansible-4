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

## Project Structure

```text
.
├── inventory/
│   └── prod.yml
│
├── group_vars/
│   ├── clickhouse.yml
│   ├── lighthouse.yml
│   └── vector.yml
│
├── roles/
│   ├── clickhouse/
│   ├── lighthouse-role/
│   └── vector-role/
│
├── requirements.yml
├── site.yml
├── ansible.cfg
└── README.md
```

## Requirements

The following software is required on the control node:

- Ansible
- Git
- SSH client

Target hosts must:

- be accessible via SSH;
- have a supported Linux distribution;
- have Internet access for downloading packages and application files.

## Installation

Install all required Ansible roles from `requirements.yml`:

```bash
ansible-galaxy install -r requirements.yml -p roles
```

To force reinstall the specified role versions:

```bash
ansible-galaxy install -r requirements.yml -p roles --force
```

## Requirements File

The `requirements.yml` file contains all roles required by the playbook:

```yaml
---
- src: https://github.com/AlexeySetevoi/ansible-clickhouse.git
  scm: git
  version: "1.13"
  name: clickhouse

- src: https://github.com/deminilyadev-maker/vector-role.git
  scm: git
  version: "1.0.0"
  name: vector-role

- src: https://github.com/deminilyadev-maker/lighthouse-role.git
  scm: git
  version: "1.0.0"
  name: lighthouse-role
```

## Inventory

The inventory is located in:

```text
inventory/prod.yml
```

The following host groups are used:

```text
clickhouse
lighthouse
vector
```

Example:

```yaml
clickhouse:
  hosts:
    clickhouse-1:
      ansible_host: <CLICKHOUSE_IP>

lighthouse:
  hosts:
    lighthouse-1:
      ansible_host: <LIGHTHOUSE_IP>

vector:
  hosts:
    vector-1:
      ansible_host: <VECTOR_IP>
```

## Configuration

Role-specific variables are stored in `group_vars`.

### Vector

The Vector version is configured using:

```yaml
vector_version: "0.34.1"
```

### Lighthouse

The Lighthouse role uses the following variables:

```yaml
nginx_user_name: nginx
lighthouse_vcs: "https://github.com/VKCOM/lighthouse.git"
lighthouse_location_dir: "/opt/lighthouse"
```

## Playbook

The main playbook is `site.yml`.

It applies the corresponding role to each inventory group:

```yaml
---
- name: Install Lighthouse
  hosts: lighthouse
  become: true

  roles:
    - lighthouse-role

- name: Install ClickHouse
  hosts: clickhouse
  become: true

  roles:
    - clickhouse

- name: Install Vector
  hosts: vector
  become: true

  roles:
    - vector-role
```

## Running the Playbook

### Check mode

Run the playbook in check mode:

```bash
ansible-playbook -i inventory/prod.yml site.yml --check
```

### Apply configuration

Run the playbook normally:

```bash
ansible-playbook -i inventory/prod.yml site.yml
```

### Show changes

Run the playbook with diff output:

```bash
ansible-playbook -i inventory/prod.yml site.yml --diff
```

### Run a specific role

Run only Lighthouse:

```bash
ansible-playbook -i inventory/prod.yml site.yml --limit lighthouse
```

Run only Vector:

```bash
ansible-playbook -i inventory/prod.yml site.yml --limit vector
```

Run only ClickHouse:

```bash
ansible-playbook -i inventory/prod.yml site.yml --limit clickhouse
```

## Role Repositories

### Vector

https://github.com/deminilyadev-maker/vector-role

### Lighthouse

https://github.com/deminilyadev-maker/lighthouse-role

### ClickHouse

https://github.com/AlexeySetevoi/ansible-clickhouse

## Versioning

Custom roles use Semantic Versioning.

Current versions:

- Vector: `1.0.0`
- Lighthouse: `1.0.0`
- ClickHouse: `1.13`

## Result

The playbook provides automated deployment of the following components:

```text
ClickHouse
    │
    └── ansible-clickhouse role

Vector
    │
    └── custom vector-role

Lighthouse
    │
    ├── NGINX
    └── custom lighthouse-role
```
