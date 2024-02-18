# Ansible Role: Nginx
This role installs and configures Nginx, a popular web server and reverse proxy. It is designed to work with both Debian-based and Red Hat-based Linux distributions.

This role is based on [geerlingguy/nginx](https://github.com/geerlingguy/ansible-role-nginx) and was modified to better fit my own needs.

Sample Usage
------------
```yaml
- name: "Setup Nginx as a reverse proxy"
  hosts: web.example.com
  roles:
    - nginx
  vars:
    nginx_vhosts:
      example.com:
        http_listen: 80
        https_listen: 443
        ssl: true
        redirect_ssl: true
        ssl_certificate: "/etc/ssl/certs/example.com.crt"
        ssl_certificate_key: "/etc/ssl/private/example.com.key"
        upstream: "backend.example.com"
        upstream_protocol: http
        client_max_body_size: "500M"

- name: SSL Passtrough
  hosts: web.example.com
  roles:
    - role: nginx
      nginx_vhosts:
        test.dev:
          http_listen: 80
          https_listen: 443
          ssl: true
          redirect_ssl: true
          tls_upstream: 10.32.10.10
```

Role Variables
--------------

The following variables determine the install and configuration behaviour of this role.

### Essential Variables

- `nginx_version`: Specify the version of Nginx to install, or use 'default' to install the distribution's default package.
- `nginx_listen_ipv6`: Enable or disable IPv6 support.
- `nginx_ppa_use`: (Ubuntu only) Whether to use the official Nginx PPA.
- `nginx_ppa_version`: Version of Nginx to use from the PPA.
- `nginx_package_name`: The name of the Nginx package to install.
- `nginx_remove_default_vhost`: Whether to remove the default virtual host that comes with Nginx.

### Logging

- `nginx_error_log`: Path to the Nginx error log.
- `nginx_access_log`: Path to the Nginx access log.

### Performance Settings

- `nginx_sendfile`: Enable or disable the sendfile feature.
- `nginx_tcp_nopush`: Enable or disable the TCP_NOPUSH option.
- `nginx_tcp_nodelay`: Enable or disable the TCP_NODELAY option.
- `nginx_keepalive_timeout`: Timeout for keepalive connections.
- `nginx_keepalive_requests`: Maximum number of requests per keepalive connection.

### Security and File Uploads

- `nginx_server_tokens`: Enable or disable server tokens in response headers.
- `nginx_client_max_body_size`: Maximum allowed size for client uploads.

### Advanced Features

- `nginx_tls_stream`: Enable TLS stream for proxying without termination.
- `nginx_proxy_protocol`: Enable the proxy protocol for enhanced client information preservation.

### Overriding Templates

- `nginx_conf_template`: Path to a custom `nginx.conf` Jinja2 template.
- `nginx_vhost_template`: Path to a custom virtual host Jinja2 template.

### Example Configurations

Refer to the sample usage section above for YAML examples demonstrating how to configure Nginx for various scenarios, including simple reverse proxy setups, static websites, and SSL passthrough configurations.

## Supported Operating Systems

This role supports a wide range of Linux distributions including:

- ArchLinux
- Debian 9 (Stretch), 10 (Buster), 11 (Bullseye)
- Ubuntu 18.04 (Bionic Beaver), 20.04 (Focal Fossa), 22.04 (Jammy Jellyfish)
- Red Hat Enterprise Linux (RHEL) / CentOS 7, 8
- Fedora (latest 2 releases)
- FreeBSD
- OpenBSD

License
-------

MIT / BSD
