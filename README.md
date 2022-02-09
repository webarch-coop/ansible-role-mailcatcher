# Webarchitects Mailcatcher Ansible Role

An Ansible role to install [Mailcatcher](https://mailcatcher.me/) and optionally [msmtp](https://marlam.de/msmtp/) on Debian and Ubuntu.

If you only want to intercept email sent via SMPT to the Mailcatcher host and port as defined (see the [defaults/main.yml) file you don't need to install msmtp.

If you want to intercept all mail sent to the `localhost` on port 25 and any emmail sent via `/usr/sbin/sendmail` then you will want to install msmtp.

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
