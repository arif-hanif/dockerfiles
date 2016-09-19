# PHP 7

Includes:

- PHP-FPM + CLI
- bcmath, curl, ctype, dom, gd, iconv, intl, json, mbstring, mcrypt, mysqli, opcache, openssl, pcntl, pdo, pdo_mysql, pdo_sqlite, phar, session, sockets, xml, redis, xdebug
- [composer](https://getcomposer.org/doc/)
- [ssmtp](http://linux.die.net/man/8/ssmtp) to catch php mail() and forwards it to MailHog

## Commands
```sh
$ docker exec -ti <container_name> /bin/sh
$ docker exec -ti <container_name> composer <arguments>
```
