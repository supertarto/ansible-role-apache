# Ansible role apache
[![CI](https://github.com/supertarto/ansible-role-apache/actions/workflows/ci.yml/badge.svg)](https://github.com/supertarto/ansible-role-apache/actions/workflows/ci.yml)

Install and configure apache with Ansible on Debian. For apache 2.4

## Requirements
None

## Tested plateform
* Debian 12 (Bookworm)
* Debian 13 (Trixie)

## Role variables

The apache service, the apache conf path
```yml
apache_service: apache2
apache_server_conf: /etc/apache2

```

Modifications in security.conf, for production purpose.
```yml
apache_server_token: Prod
apache_server_signature: "Off"
apache_trace_enabled: "Off"
```

A list of mod to enable and a list of mode to disable. Default value is "empty".
```yml
apache_mods_enabled: []
apache_mods_disabled: []
```

A list of vhost to enable, a list of vhost to definitively remove, a list of vhost de disable. 
```yml
apache_vhost_to_enable: []
#  - "my-vhosts.conf"

apache_vhost_to_remove: []
#  - "000-default.conf"

apache_vhost_to_disable: []
#  - "000-default.conf"
```

**apache_vhost_to_add** is used to add and configure your virtualhost. You can have multiple vhosts. If you don't want to set a specific parameter, dont use it. For exemple, if you don't want to define a server_alias, or if you don't need a "location", remove those lines from the exemple
For instructions that are not defined in those predefined param, you can use **general_extra_parameters** or (**extra_parameters** in directory, location or file)
and write directly your instructions (ProxyPass, ProxyRevers etc) Don't forger the leadin "|" - see example below.

By default, **apache_vhost_to_add** is empty. you **MUST** define it yourself, to suits your needs.
```yml
apache_vhost_to_add: []
#  - name: "my-vhosts_1.conf"
#    configs:
    # - listen_ip: "*"
    #   listen_port: "80"
    #   server_name: "mysite_1.exemple.com"

#  - name: "my-vhosts_2.conf"
#    configs:
    # - listen_ip: "*"
    #   listen_port: "80"
    #   server_name: "mysite_2.exemple.com"
    #
    # - listen_ip: "*"
    #   listen_port: "443"
    #   server_name: "mysite_2.exemple.com"
    #   server_alias: "myalias"
    #   server_admin: "admin@exemple.com"
    #   document_root: "/var/www/exemple"
    #   ssl_engine: "on"
    #   ssl_protocol: "-all +TLSv1.3"
    #   ssl_certificate_file: "/path/to/my/cert.pem"
    #   ssl_certificate_key_file: "/path/to/my/cert.key"
    #   ssl_certificate_chain_file: "/path/to/my/cert.chain"
    #   log_level: "warn"
    #   log_format: "your custom log format"
    #   error_log_param: "${APACHE_LOG_DIR}/error.log"
    #   custom_log_param: "${APACHE_LOG_DIR}/access.log combined"
    #   general_extra_parameters: |
    #       every other useful parameter in your general conf (ProxyRequest, ProxyPreserve etc)
    #   directory:
    #      - path: "/var/www/exemple"
    #        allow_override: "All"
    #        options: "None"
    #        require: "all granted"
    #        extra_parameters: |
    #           every other useful parameter in your directory conf
    #   location:
    #      - path: "/location/path/exemple"
    #        options: "None"
    #        require: "all granted"
    #        extra_parameters: |
    #           every other useful parameter in your location conf
    #   file:
    #      - path: "/path/to/my/file"
    #        extra_parameters: |
    #           every other useful parameter in your file conf
```

## License
GPL V3.0
