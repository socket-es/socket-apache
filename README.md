# Ansible Role: Apache 2.4

An Ansible Role that installs Apache 2.4 on Debian/Ubuntu.

## Requirements

If you are using SSL/TLS, you will need to provide your own certificate and key files. You can generate a self-signed certificate with a command like `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout example.key -out example.crt`.

## Role Variables

### Apache packages state; use `present` to make sure it's installed, if you like perl, security or jk modules add present to names

apache_packages_state: present
apache_perl_packages_state: absent
apache_mod_security_packages_state: absent
apache_mod_jk_packages_state: absent

Available variables are listed below, along with default values (see `defaults/main.yml`):

    apache_listen_ip: "*"
    apache_listen_port: 80
    apache_listen_port_ssl: 443

The IP address and ports on which apache should be listening, listening on port 80 or 443 and need to change the defaults.

    apache_run_user: "www-data"
    apache_run_group: "www-data"

Default user and group where Apache run as.

    apache_server_tokens: "Prod"

Default value for the Server HTTP response header field.

    apache_timeout: 60

Default request timeout.

    apache_keepalive: "On"
    apache_max_keep_alive_requests: 100
    apache_keep_alive_timeout: 1

Optimal KeepAlive config for best server performance.

    apache_log_dir: "/var/log/apache2"
    apache_log_level: "warn"

Default apache log directory and log level.

    apache_directory_index: "index.html index.cgi index.pl index.php index.xhtml index.htm"

Default directory index for Apache.

    apache_proxy_conf: False
    apache_proxy_requests: "Off"
    apache_proxy_timeout: 120

Default config for mod_proxy.

    apache_ssl_cipher_suite: "ECDHE-RSA-DES-CBC3-SHA:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-DSS-AES128-SHA256:DHE-DSS-AES256-SHA:!AES128-GCM-SHA256:!AES256-GCM-SHA384:!AES128-SHA:!AES256-SHA:AES:!CAMELLIA:!DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA:!DHE-RSA-AES128-GCM-SHA256:!DHE-RSA-AES256-GCM-SHA384:!DHE-RSA-AES128-SHA256:!DHE-RSA-AES256-SHA:!DHE-RSA-AES128-SHA:!DHE-RSA-AES256-SHA256:!DHE-RSA-CAMELLIA128-SHA:!DHE-RSA-CAMELLIA256-SHA:!CAMELLIA128-SHA:!CAMELLIA256-SHA:!AES256-SHA256:!AES128-SHA256"
    apache_ssl_protocol: "all -SSLv2 -SSLv3 -TLSv1 -TLSv1.1"
    apache_ssl_options: False

Apache config for SSL connections configure various SSL engine run-time options if necesary. The SSL protocols and cipher suites that are used/allowed when clients make secure connections to your server. These are secure/sane defaults, have A+ score in SSLLabs for maximum security.

    apache_status_ips:
      - 127.0.0.1

Security config for Apache Status & Info pages, default access to localhost. Write the IPs allowed for access, one per line.

    apache_confs_enabled:
      - charset
      - localized-error-pages
      - other-vhosts-access-log
      - security
    apache_confs_disabled:
      - serve-cgi-bin

List of Apache configs enabled or disabled by default.

    apache_mpm_module:
      - prefork
      - event
      - worker

    mpm_event_start_servers: 2
    mpm_event_min_spare_threads: 25
    mpm_event_max_spare_threads: 75
    mpm_event_thread_limit: 64
    mpm_event_threads_per_child: 25
    mpm_event_max_request_workers: 150
    mpm_event_max_connections_per_child: 0

    mpm_prefork_start_servers: 5
    mpm_prefork_min_spare_servers: 5
    mpm_prefork_max_spare_servers: 10
    mpm_prefork_max_request_workers: 150
    mpm_prefork_max_connections_per_child: 0

    mpm_worker_start_servers: 2
    mpm_worker_min_spare_threads: 25
    mpm_worker_max_spare_threads: 75
    mpm_worker_thread_limit: 64
    mpm_worker_threads_per_child: 25
    mpm_worker_max_request_workers: 150
    mpm_worker_max_connections_per_child: 0

Config for al multi-processing modules of Apache, the first list are templates, and the values that correspond to each. To activate one of them, it will be done in the **apache_mods_ena** section.

    apache_mods_ena:
      - access_compat
      - alias
      - auth_basic
      - authn_core
      - authn_file
      - authz_core
      - authz_host
      - authz_user
      - autoindex
      - deflate
      - dir
      - env
      - filter
      - mime
      - mpm_event
      - negotiation
      - reqtimeout
      - setenvif
      - status

apache_mods_dis: []

List of Apache modules enabled or disabled by default.

    apache_create_vhosts: true
    apache_vhosts_template: "vhosts.conf.j2"
    apache_vhosts_ssl_template: "vhosts.conf.j2"

If set to true, a list of vhosts files, managed by this role's variables (see below), will be created and placed in the Apache configuration folder. If set to false, you can place your own vhosts file into Apache's configuration folder and skip the convenient (but more basic) one added by this role. You can also override the template used and set a path to your own template, if you need to further customize the layout of your VirtualHosts.

    apache_remove_default_vhost: false

On Debian/Ubuntu, a default virtualhost is included in Apache's configuration. Set this to `true` to remove that default virtualhost configuration file.

You can add or override global Apache configuration settings in the role-provided vhosts file (assuming `apache_create_vhosts` is true) using this variable. By default it only sets the DirectoryIndex configuration.

    apache_vhosts:
      # Additional optional properties: 'serveradmin, serveralias, extra_parameters'.
      - servername: "local.dev"
        documentroot: "/var/www/html"

Add a set of properties per virtualhost, including `servername` (required), `documentroot` (required), `priority` (required), `apache_vhost_user` (required), `apache_vhost_group` (required), `apache_php_fpm_backend` (required), `apache_security_headers` (required), `listen_ip` (required), `wp` (required), `apache_interest_cohort` (required), `customlog` (required), `logpath` (required), `allow_override` (optional: defaults to the value of `apache_allow_override`), `options` (optional: defaults to the value of `apache_options`), `serveradmin` (optional), `serveralias` (optional) and `extra_parameters` (optional: you can add whatever additional configuration lines you'd like in here).

Here's an example using `extra_parameters` to add a RewriteRule to redirect all requests to the `www.` site:

      - servername: "www.local.dev"
        serveralias: "local.dev"
        listen_ip: "*"
        documentroot: "/var/www/html"
        priority: "00"
        wp: "0"
        apache_vhost_user: "www-data"
        apache_vhost_group: "www-data"
        apache_php_fpm_backend: "0"
        apache_security_headers: "0"
        apache_interest_cohort: "0"
        customlog: "1"
        typelog: "combined"
        extra_parameters: |
          RewriteCond %{HTTP_HOST} !^www\. [NC]
          RewriteRule ^(.*)$ http://www.%{HTTP_HOST}%{REQUEST_URI} [R=301,L]

The `|` denotes a multiline scalar block in YAML, so newlines are preserved in the resulting configuration file output.

    apache_vhosts_ssl: []

No SSL vhosts are configured by default, but you can add them using the same pattern as `apache_vhosts`, with a few additional directives, like the following example:

    apache_vhosts_ssl:
      - servername: "local.dev"
        documentroot: "/var/www/html"
        certificate_file: "/home/vagrant/example.crt"
        certificate_key_file: "/home/vagrant/example.key"
        certificate_chain_file: "/path/to/certificate_chain.crt"
        extra_parameters: |
          RewriteCond %{HTTP_HOST} !^www\. [NC]
          RewriteRule ^(.*)$ http://www.%{HTTP_HOST}%{REQUEST_URI} [R=301,L]

    apache_allow_override: "All"
    apache_options: "-Indexes +FollowSymLinks"

The default values for the `AllowOverride` and `Options` directives for the `documentroot` directory of each vhost.  A vhost can overwrite these values by specifying `allow_override` or `options`.

    apache_mods_ena:
      - rewrite
      - ssl
    apache_mods_dis: []

Which Apache mods to enable or disable (these will be symlinked into the appropriate location). See the `mods-available` directory inside the apache configuration directory (`/etc/apache2/mods-available` on Debian/Ubuntu) for all the available mods.

    apache_state: started

Set initial Apache daemon state to be enforced when this role is run. This should generally remain `started`, but you can set it to `stopped` if you need to fix the Apache config during a playbook run or otherwise would not like Apache started at the time this role is run.

    apache_enabled: yes

Set the Apache service boot time status. This should generally remain `yes`, but you can set it to `no` if you need to run Ansible while leaving the service disabled.

    apache_ignore_missing_ssl_certificate: true

If you would like to only create SSL vhosts when the vhost certificate is present (e.g. when using Let’s Encrypt), set `apache_ignore_missing_ssl_certificate` to `false`. When doing this, you might need to run your playbook more than once so all the vhosts are configured (if another part of the playbook generates the SSL certificates).

## .htaccess-based Basic Authorization

If you require Basic Auth support, you can add it either through a custom template, or by adding `extra_parameters` to a VirtualHost configuration, like so:

    extra_parameterl: |
      <Directory "/var/www/password-protected-directory">
        Require valid-user
        AuthType Basic
        AuthName "Please authenticate"
        AuthUserFile /var/www/password-protected-directory/.htpasswd
      </Directory>

To password protect everything within a VirtualHost directive, use the `Location` block instead of `Directory`:

    <Location "/">
      Require valid-user
      ....
    </Location>

You would need to generate/upload your own `.htpasswd` file in your own playbook. There may be other roles that support this functionality in a more integrated way.

## Dependencies

None.

## Example Playbook

    - hosts: webservers
      vars_files:
        - vars/main.yml
      roles:
        - { role: socket.apache }

*Inside `vars/main.yml`*:

    apache_listen_port: 8080
    apache_vhosts:
      - {servername: "example.com", documentroot: "/var/www/vhosts/example_com"}

## License

MIT / BSD

## Author Information

This role was created in 2023 by [Juan Carlos Giménez Moncada](https://www.opensocket.es/).
