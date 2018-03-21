FROM node:8

LABEL maintainer="Alexander van Zyl <vanzyl.alex@gmail.com>"
LABEL version="1.0.0"

RUN apt-get install -y git \
    && apt-get autoclean \
    && apt-get clean

WORKDIR /var/www/html

CMD ["node"]