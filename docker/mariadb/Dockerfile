FROM mariadb:10

LABEL maintainer="Alexander van Zyl <vanzyl.alex@gmail.com>"
LABEL version="1.0.0"

COPY my.cnf /etc/mysql/conf.d/my.cnf

EXPOSE 3306

CMD ["mysqld"]