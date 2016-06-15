# Ansible Role: Php-fpm

[![Build Status](https://travis-ci.org/AttestationLegale/ansible-role-php-fpm.svg?branch=master)](https://travis-ci.org/AttestationLegale/ansible-role-php-fpm) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-php--fpm-blue.svg)](https://galaxy.ansible.com/AttestationLegale/php-fpm/)

Php-fpm management.

This role will create a dedicated php-fpm instance running under the specified user/group.

## Requirements

### Packages

    GROG.package

### User

The user hosting the php-fpm instance must exist on the target system and his home directory must look like that:

```
/home/user
         \-etc
             \-php5
         \-usr
             \-lib
                 \-php5
                      \-fpm
                          \-pool.d
         \-var
             \-lib
                 \-php5
                      \-sessions
             \-log
                 \-php-fpm
             \-run
```

I use [AttestationLegale.skel](https://galaxy.ansible.com/AttestationLegale/skel/) and [AttestationLegale.users](https://galaxy.ansible.com/AttestationLegale/users/) to create all the necessary stuff before calling php-fpm role.

## Role Variables

For a complete list of variables, see `default/main.yml`.

    php_fpm_pools: []

## Dependencies

None

## Example Playbook

```yaml
---
  - hosts: all
    roles:
      - php-fpm
    vars:
      - php_fpm_pools:
        - owner: foo
          group: bar
          home: /home/foo
```

## License

MIT / BSD

## Author Information

This role was created in 2016 by [ALG](https://www.attestationlegale.fr)
