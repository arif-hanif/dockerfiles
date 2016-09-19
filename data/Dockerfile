FROM busybox
MAINTAINER Steven Truesdell <steven@strues.io>

RUN \

# Create main directories for Consul, MySQL (MariaDB), Nginx, PHP-FPM and SSH

	mkdir -p /data/db/mysql \
	&& mkdir -p /data/log/mysql \
	&& mkdir -p /var/lib/redis \
	&& mkdir -p /var/lib/postgresql/data

VOLUME \
	/data \
	/var/lib/postgresql/data \
	/var/lib/redis \
	/data/log/mysql \
	/data/db/mysql
