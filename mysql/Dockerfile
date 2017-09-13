FROM alpine:latest

ARG MYSQL_ROOT_PASSWORD
ARG MYSQL_DATABASE
ARG MYSQL_USER 
ARG MYSQL_PASSWORD

# Install required packages
RUN apk --no-cache add mysql mysql-client bash supervisor

# Configure supervisord
COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

#Install MySQL
RUN /usr/bin/mysql_install_db --user=mysql \
    && cp /usr/share/mysql/mysql.server /etc/init.d/mysqld

# Ports
EXPOSE 3306

VOLUME /var/lib/mysql

# Entrypoint to prepare mysql-database
COPY entrypoint.sh /entrypoint.sh
RUN chmod 754 /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

# EXEC
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]