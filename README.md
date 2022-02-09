# Webarchitects Mailcatcher Ansible Role

An Ansible role to install [Mailcatcher](https://mailcatcher.me/) and optionally [msmtp](https://marlam.de/msmtp/) on Debian and Ubuntu.

If you only want to intercept email sent via SMPT to the Mailcatcher host and port and / or Sendmail path (as defined in the [defaults/main.yml](defaults/main.yml) then you don't need to install msmtp, for example for PHP-FPM set:

```conf
php_admin_value[sendmail_path] = /usr/local/bin/catchmail -fwww-data@example.org
php_admin_value[sendmail_from] = www-data@example.org
```

If you want to intercept all mail sent to the `localhost` on port 25 and any email sent via `/usr/sbin/sendmail` then you will want to install msmtp.

Note that this role doesn't create a user account for the mailcatcher user or a reverse proxy for Mailcatcher.

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
