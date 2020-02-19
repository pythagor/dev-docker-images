ARG FROM=ubuntu:18.04
FROM ${FROM}

ENV DEBIAN_FRONTEND=noninteractive

RUN apt clean && apt -y update && apt install -y locales && locale-gen en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN apt -y update && apt install -y \
    bash \
    curl \
    git \
    unzip \
    php7.2 \
    php7.2-xml \
    php7.2-zip \
    php7.2-xmlreader \
    libgcrypt11-dev zlib1g-dev \
    php7.2-opcache \
    php7.2-imagick \
#    php7.2-openssl \
    php7.2-curl \
    php7.2-json \
    php7.2-dom \
    php7.2-phar \
    php7.2-mbstring \
    php7.2-ctype \
    openssl \
    tzdata

RUN \
	# Symlinks php7
	ln -s -f /usr/bin/php7.2 /usr/bin/php

RUN \
    # Install Composer
    php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \

# SSH for remote PHP interpreter
RUN apt -y update && apt install -y openssh-server \
    && mkdir /var/run/sshd \
    && echo 'root:root' | chpasswd \
    && sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
    && sed -i 's/#PermitRootLogin yes/PermitRootLogin yes/' /etc/ssh/sshd_config \
# SSH login fix. Otherwise user is kicked off after login
    && sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

# Xdebug
RUN apt install -y php-xdebug

RUN echo 'xdebug.remote_autostart=1' >> /etc/php/7.2/cli/conf.d/25-docker-php-ext-xdebug.ini \
    && echo 'xdebug.remote_enable=1' >> /etc/php/7.2/cli/conf.d/25-docker-php-ext-xdebug.ini \
    && echo 'xdebug.remote_host=phpstorm' >> /etc/php/7.2/cli/conf.d/25-docker-php-ext-xdebug.ini \
    && echo 'xdebug.idekey=PHPSTORM' >> /etc/php/7.2/cli/conf.d/25-docker-php-ext-xdebug.ini

COPY composer.json /opt/behat/composer.json

RUN \
	# Install Behat
	cd /opt/behat && \
	composer install 2>&1

ENV PATH $PATH:/opt/behat/bin

WORKDIR /src

ENTRYPOINT ["behat"]
