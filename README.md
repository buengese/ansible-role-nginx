# Ansible Role: Nginx

This role installs and configures Nginx, a popular web server and reverse proxy. It is designed to work with both Debian-based and Red Hat-based Linux distributions.

Based on [geerlingguy/nginx](https://github.com/geerlingguy/ansible-role-nginx)

## Role Variables

The following are some of the global configuration options that can be set for this role.
A full list along with the default values can be found in `defaults/main.yml`:

* `nginx_listen_ipv6`: Whether to listen on IPv6.

* `nginx_ppa_use`: Use the official Nginx PPA for Ubuntu, and the version to use if so.
* `nginx_ppa_version`: The version of the official Nginx PPA to use.
* `nginx_package_name`: The name of the Nginx package to install.

* `nginx_error_log`: The location of the error log file.
* `nginx_access_log`: The location of the access log file.

* `nginx_sendfile`: Whether to use the sendfile system call for serving files.
* `nginx_tcp_nopush`: Whether to enable the TCP_NOPUSH socket option.
* `nginx_tcp_nodelay`: Whether to enable the TCP_NODELAY socket option.

* `nginx_keepalive_timeout`: The timeout for keepalive connections.
* `nginx_keepalive_requests`: The maximum number of requests that can be made over a single keepalive connection.

* `nginx_server_tokens`: Whether to include server information in response headers.
* `nginx_client_max_body_size`: This value determines the largest file upload possible

* `nginx_remove_default_vhost`: Whether to remove the default Nginx virtual host.
* `nginx_tls_stream`: Whether to use the stream backend for TLS proxying without termination.
* `nginx_proxy_protocol`: Whether to enable the proxy protocol for the stream backend.

## Configuration examples

### Example 1: Simple reverse proxy
```yaml
- name: Setup Simple reverse proxy
  hosts: web
  roles:
    - role: nginx
      nginx_vhosts:
        test.dev:
          http_listen: 80
          https_listen: 443
          ssl: true
          redirect_ssl: true
          ssl_certificate: "/etc/ssl/certs/test.dev.crt"
          ssl_certificate_key: "/etc/ssl/private/test.dev.key"
          upstream: 10.32.10.10
          upstream_porotocol: http
          client_max_body_size: 500M

```

### Example 2: Static Website
```yaml
- name: Setup Simple reverse proxy
  hosts: web
  roles:
    - role: nginx
      nginx_vhosts:
        service.test.dev:
          http_listen: "80"
          https_listen: "443"
          ssl: true
          name_redirect: "www.service.test.dev"
          root: "/var/www/service.test.dev"
          index: "index.html index.htm"
          filename: "service.test.dev.conf"
          error_page: ""
          access_log: ""
          error_log: ""
          extra_parameters: ""
```

### Example 3: SSL Passtrough
```yaml
- name: SSL Passtrough
  hosts: web
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

## Overriding configuration templates

If you can't customize via variables because an option isn't exposed, you can override the template used to generate the virtualhost configuration files or the `nginx.conf` file.

```yaml
nginx_conf_template: "nginx.conf.j2"
nginx_vhost_template: "vhost.j2"
```

If necessary you can also set the template on a per vhost basis.

You can either copy and modify the provided template, or extend it with [Jinja2 template inheritance](http://jinja.pocoo.org/docs/2.9/templates/#template-inheritance) and override the specific template block you need to change.

## Dependencies

None.

## Example Playbook

    - hosts: server
      roles:
        - { role: geerlingguy.nginx }

## License

MIT / BSD
