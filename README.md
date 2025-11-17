# Ansible role apache
[![CI](https://github.com/supertarto/ansible-role-apache/actions/workflows/ci.yml/badge.svg)](https://github.com/supertarto/ansible-role-apache/actions/workflows/ci.yml)

Install and configure apache with Ansible on Debian.

## Requirements
None

## Tested plateform
* Debian 12 (Bookworm)
* Debian 13 (Trixie)

## Role variables

The apache service, the apache conf path and packages to install.
```yml
apache_service: apache2
apache_server_conf: /etc/apache2

apache_packages:
  - apache2
  - apache2-utils
```

Ports configuration, to be loaded in ports.conf
```yml
apache_listen_port: 80
apache_ports_configuration_items:
  - regexp: "^Listen "
    line: "Listen {{ apache_listen_port }}"
```

Modifications in security.conf, for production purpose.
```yml
apache_server_token: Prod
apache_server_signature: "Off"
apache_trace_enabled: "Off"
apache_security_configuration_items:
  - regexp: "^ServerTokens "
    line: "ServerTokens {{ apache_server_token }}"
  - regexp: "^ServerSignature "
    line: "ServerSignature {{ apache_server_signature }}"
  - regexp: "^TraceEnable "
    line: "TraceEnable {{ apache_trace_enabled }}"
```

A list of mod to enable and a list of mode to disable. Default value is "empty".
```yml
apache_mods_enabled: []
apache_mods_disabled: []
```

Do you want to create a new vhosts file? If set to true, how will the conf file be called?
```yml
apache_create_vhosts: true
apache_vhosts_filename: "my-vhosts.conf"
```

Do you need to remove the default hosts? If set to true, wich conf file need to be deleted? It can also be use to remove custom vhosts.
```yml
apache_remove_default_vhost: true
apache_default_vhost_filename:
 - 000-default.conf
```

**apache_vhost_config** is used to configure your virtualhost. You can have multiple vhosts. If you don't want to set a specific parameter, just delete it. For exemple, if you don't want to define a serveralias, or if you don't need a "location", remove those lines.

Other variables are meant to be multilined: **apache_vhost_config.custom_param**, **apache_vhost_config.directory.config**, **apache_vhost_config.location.config**, **apache_vhost_config.file.config**. Don't forger the leadin "|".
You can see here an exemple. By default, **apache_vhost_config** is empty. you **MUST** define it yourself, to suits your needs.
```yml
apache_vhost_config:
  - listen_ip: "*"
    listen_port: 80
    server_name: host1
    custom_param: |
        Redirect / https://host1
    
  - listen_ip: "*"
    listen_port: 443
    server_name: host1
    serveralias: alias1
    documentroot: "/var/www/html"
    serveradmin: admin@localhost
    custom_param: |
      ProxyRequests Off
      ProxyPreserveHost On
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
      LogLevel warn
    ssl_engine: "on"
    ssl_certificate_file: /etc/ssl/certs/certif.crt
    ssl_certificate_key_file: /etc/ssl/private/certif.key
    ssl_certificate_chain_file: /etc/ssl/certs/chain
    directory:
      - path: "/var/www/html"
        config: |
          AllowOverride All
          Order deny,allow
          allow from all
      - path: "/usr/lib/cgi-bin"
        config: |
          SSLOptions +StdEnvVars
    location:
      - path: "/"
        config: |
          Options -Indexes
          Options -Includes
          Options -FollowSymLinks
          ProxyPass http://localhost:8080/ min=0 max=100 smax=50 ttl=10
          ProxyPassReverse http://localhost/
    file:
      - path: '\.(cgi|shtml|phtml|php)$'
        config: |
          SSLOptions +StdEnvVars
```

## License
GPL V3.0
