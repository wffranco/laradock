FROM webdevops/apache@sha256:ad02a6897f26fa089b773ee675971c3c27d5bf231a3945009f0c1f893f62a6a5

MAINTAINER Eric Pfeiffer <computerfr33k@users.noreply.github.com>

ARG PHP_SOCKET=php-fpm:9000

ENV WEB_PHP_SOCKET=$PHP_SOCKET

ENV WEB_DOCUMENT_ROOT=/var/www/tms/public

EXPOSE 80 443

WORKDIR /var/www/tms/public

ENTRYPOINT ["/opt/docker/bin/entrypoint.sh"]

CMD ["supervisord"]
