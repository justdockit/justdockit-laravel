FROM nginx:1.11

MAINTAINER Alexander van Zyl <vanzyl.alex@gmail.com>

RUN mkdir -p /var/www/html

# Permissions
RUN usermod -u 1000 www-data

COPY default.conf /etc/nginx/conf.d/default.conf

WORKDIR /var/www/html

EXPOSE 80 443
CMD ["nginx", "-g", "daemon off;"]