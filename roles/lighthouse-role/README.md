# Lighthouse Role

Ansible role for installing and configuring Lighthouse with NGINX.

## Description

This role:

- installs NGINX;
- configures NGINX;
- installs Git;
- downloads Lighthouse from the VKCOM repository;
- configures NGINX to serve Lighthouse.

## Requirements

- Ansible
- Linux host with DNF package manager
- Internet access from the target host

## Role Variables

### `nginx_user_name`

User used by NGINX.

Default:

```yaml
nginx_user_name: nginx