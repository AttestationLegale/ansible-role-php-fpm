# Ansible Role: php-fpm

[![Build Status](https://travis-ci.org/AttestationLegale/ansible-role-php-fpm.svg?branch=master)](https://travis-ci.org/AttestationLegale/ansible-role-php-fpm) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-php--fpm-blue.svg)](https://galaxy.ansible.com/AttestationLegale/php-fpm/)

The php-fpm role allows you to install php-fpm and create dedicated instances  running under a specified user/group.

Behind the scene it uses [GROG.package](https://galaxy.ansible.com/GROG/package/). It supports multi-level package management by using `with_flattened` statement.

This role performs the following tasks:

  - install php5-fpm
  - create /home/{{ user }}/etc/php5/fpm/php.ini
  - create /home/{{ user }}/etc/php5/fpm/php-fpm.conf
  - create /home/{{ user }}/etc/php5/fpm/env.conf
  - create /home/{{ user }}/etc/php5/fpm/pool.d/{{ user }}.conf
  - create /home/{{ user }}/usr/lib/php5/php5-fpm-checkconf
  - create /home/{{ user }}/usr/lib/php5/php5-fpm-reopenlogs
  - create /home/{{ user }}/usr/lib/php5/sessionclean
  - create /etc/cron.d/php5-{{ user }} (cronjob calling /home/{{ user }}/usr/lib/php5/sessionclean for sessions cleanup)
  - create /etc/logrotate.d/php5-fpm-{{ user }} (to rotate php5-fpm logs for this instance)
  - create /lib/systemd/system/php5-fpm-{{ user }}.service (so that your instance can be managed with systemctl)


## Requirements

### User

The user hosting the php-fpm instance must exist on the target system and his home directory must look like that:

```
/home/user
         \-etc
             \-php5
                  \-fpm
                      \-pool.d
         \-usr
             \-lib
                 \-php5
         \-var
             \-lib
                 \-php5
                      \-sessions
             \-log
                 \-php-fpm
             \-run
```

You can use [AttestationLegale.skel](https://galaxy.ansible.com/AttestationLegale/skel/) and [AttestationLegale.users](https://galaxy.ansible.com/AttestationLegale/users/) to create all the necessary stuff before calling php-fpm role.

## Role Variables

For a complete list of variables, see `default/main.yml`.

    php_fpm_pools: []

## Dependencies

[GROG.package](https://galaxy.ansible.com/GROG/package/)

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
          php_config: []
          php_fpm_config: []
          php_fpm_pool_config: []
```

`php_config` can be used to customize `etc/php5/fpm/php.ini` (see `templates/php.ini.j2` for details).

`php_fpm_config` can be used to customize `etc/php5/fpm/php-fpm.conf` (see `templates/php-fpm.conf.j2` for details).

`php_fpm_pool_config` can be used to customize `etc/php5/fpm/pool.d/{{ user }.conf` (see `templates/pool.conf.j2` for details).

## License

MIT / BSD

## Author Information

This role was created in 2016 by [ALG](https://www.attestationlegale.fr)
