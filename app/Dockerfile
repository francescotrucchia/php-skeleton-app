FROM php:8.2-cli as php

ENV TZ UTC

ARG USER_ID=1000
ARG GROUP_ID=1000

RUN apt-get update && \
    apt-get install -y git procps libicu-dev && \
    docker-php-ext-install bcmath intl

RUN echo "Using GID $GROUP_ID and UID $USER_ID" && \
    mkdir /app && \
    groupadd -g $GROUP_ID app && \
    useradd -m -s /bin/bash -g $GROUP_ID -u $USER_ID app && \
    chown app:app /app && \
    chown app:app -R /usr/local/etc/ && \
    chown app:app -R /home/app

RUN cd /app && \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer

CMD cd /app && php -S 0.0.0.0:8080 -t public public/index.php

WORKDIR /app

