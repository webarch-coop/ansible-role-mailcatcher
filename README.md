# Webarchitects Mailcatcher Ansible Role

An Ansible role to install [Mailcatcher](https://mailcatcher.me/) and optionally [msmtp](https://marlam.de/msmtp/) on Debian and Ubuntu.

If you only want to intercept email sent via SMPT to the Mailcatcher host and port and / or Sendmail path then you don't need to install msmtp, for example for PHP-FPM set:

```
php_admin_value[sendmail_path] = /usr/local/bin/catchmail -fwww-data@example.org
php_admin_value[sendmail_from] = www-data@example.org
```

If you want to intercept all mail sent to the `localhost` on port 25 and any email sent via `/usr/sbin/sendmail` then you will want to install msmtp.

Note that this role doesn't create a user account for the `mailcatcher_user` or a reverse proxy for Mailcatcher.

The following [defaults](defaults/main.yml) are defined:


| Variable name                    | Default value                      | Comment                                                                                                                                                                                             |
|----------------------------------|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `mailcatcher`                    | `true`                             | Set `mailcatcher` to `false` to remove mailcatcher                                                                                                                                                  |
| `mailcatcher_path`               | `/usr/local/bin/mailcatcher`       | Setting the `bindir` was added in 3.3.0 of `community.general.gems` and Debian Bullseye Ansible is too old to use this setting so the `mailcatcher_path` can't be properly changed from the default |
| `mailcatcher_user`               | `mailcatcher`                      |                                                                                                                                                                                                     |
| `mailcatcher_group`              | `{{ mailcatcher_user }}`           |                                                                                                                                                                                                     |
| `mailcatcher_smtp_ip`            | `127.0.0.1`                        |                                                                                                                                                                                                     |
| `mailcatcher_smtp_port`          | `1025`                             |                                                                                                                                                                                                     |
| `mailcatcher_http_ip`            | `127.0.0.1`                        |                                                                                                                                                                                                     |
| `mailcatcher_http_port`          | `1080`                             |                                                                                                                                                                                                     |
| `mailcatcher_grant_capabilities` | `false`                            | When `mailcatcher_user` is not `root` and needs to bind to a port number less than 1024 then `mailcatcher_grant_capabilities` will automatically be set to True                                     |
| `mailcatcher_msmtp`              | `false`                            | Set `mailcatcher_msmtp` to true to replace `/usr/sbin/sendmail` with the `msmtp-mta` package to pass all outgoing email to Mailcatcher                                                              |
| `mailcatcher_msmtp_from`         | `www-data@{{ inventory_hostname }}`| It isn't entirly clear that this needs to be set                                                                                                                                                    |

## Apache reverse proxy

Apache directives such as these can be used for a reverse proxy with HTTP authentication:

```apache
<IfModule proxy_module>
  ProxyRequests Off
  ProxyPass "/" "http://127.0.0.1:1080/"
  ProxyPassReverse "/" "http://127.0.0.1:1080/"
  <Proxy "http://127.0.0.1:1080/">
    AuthType Basic
    AuthName "Authorised Users"
    AuthUserFile "/etc/apache2/.htpasswd"
    Require valid-user
  </Proxy>
</IfModule>
```
