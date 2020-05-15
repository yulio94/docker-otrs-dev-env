FROM mariadb:latest

LABEL maintainer="Julio Cesar Estrada <j.cesarestrada@hotmail.com>"

USER root
ENV MYSQL_ROOT_PASSWORD=root

#mariadb image runs as mysql user (uid 27) but we need to do some configuration
#changes so we need to temporarly switch to root

#Change db configuration as required by official install docs and Enable utf8 support
RUN sed -i.bk -r '/^\[mysqld\]$/a max_allowed_packet=64M' /etc/mysql/my.cnf && \
    sed -i.bk -r '/^\[mysqld\]$/a query_cache_size=32M' /etc/mysql/my.cnf && \
    sed -i.bk -r '/^\[mysqld\]$/a innodb_log_file_size=256M' /etc/mysql/my.cnf && \
    sed -i.bk -r '/^\[mysqld\]$/a skip-character-set-client-handshake' /etc/mysql/my.cnf && \
    sed -i.bk -r "/^\[mysqld\]$/a init_connect='SET collation_connection = utf8_unicode_ci'" /etc/mysql/my.cnf && \
    sed -i.bk -r '/^\[mysqld\]$/a collation-server = utf8_general_ci' /etc/mysql/my.cnf && \
    sed -i.bk -r "/^\[mysqld\]$/a init-connect=\'SET NAMES utf8\'" /etc/mysql/my.cnf && \
    sed -i.bk -r '/^\[mysqld\]$/a character-set-server = utf8' /etc/mysql/my.cnf

EXPOSE 3360