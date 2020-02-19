FROM ubuntu:18.04

RUN apt clean && apt -y update && apt install -y locales && locale-gen en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN apt -y update \
    && apt install -y curl zip unzip git software-properties-common \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y tzdata \
    && add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y \
	php7.0-fpm \
	php7.0-cli \
	php7.0-gd \
	php7.0-mysql \
        php7.0-pgsql \
	php7.0-imap \
	php-memcached \
	php7.0-mbstring \
	php7.0-xml \
	php7.0-curl \
        php7.0-sqlite \
	php7.0-soap \
	php7.0-zip \
	php-mongodb \
	php7.0-gmp \
	php7.0-intl \
	php-redis \
	php7.0-bcmath \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && apt install -y php-xdebug \
    && mkdir /run/php \
    && apt remove -y --purge software-properties-common \
    && apt clean \
    && apt autoremove -y \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN echo 'xdebug.remote_autostart=1' >> /etc/php/7.0/cli/conf.d/25-docker-php-ext-xdebug.ini \
    && echo 'xdebug.remote_enable=1' >> /etc/php/7.0/cli/conf.d/25-docker-php-ext-xdebug.ini \
    && echo 'xdebug.remote_host=phpstorm' >> /etc/php/7.0/cli/conf.d/25-docker-php-ext-xdebug.ini \
    && echo 'xdebug.idekey=PHPSTORM' >> /etc/php/7.0/cli/conf.d/25-docker-php-ext-xdebug.ini

RUN echo 'xdebug.remote_autostart=1' >> /etc/php/7.0/fpm/conf.d/25-docker-php-ext-xdebug.ini \
    && echo 'xdebug.remote_enable=1' >> /etc/php/7.0/fpm/conf.d/25-docker-php-ext-xdebug.ini \
    && echo 'xdebug.remote_host=phpstorm' >> /etc/php/7.0/fpm/conf.d/25-docker-php-ext-xdebug.ini \
    && echo 'xdebug.idekey=PHPSTORM' >> /etc/php/7.0/fpm/conf.d/25-docker-php-ext-xdebug.ini

# SSH for remote PHP interpreter
RUN apt update && apt install -y openssh-server \
    && mkdir /var/run/sshd \
    && echo 'root:root' | chpasswd \
    && sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
    && sed -i 's/#PermitRootLogin yes/PermitRootLogin yes/' /etc/ssh/sshd_config \
# SSH login fix. Otherwise user is kicked off after login
    && sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

ADD config/php-fpm.conf /etc/php/7.0/fpm/php-fpm.conf
ADD config/www.conf /etc/php/7.0/fpm/pool.d/www.conf

EXPOSE 9000

RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

USER 1000:1000

WORKDIR /workspace

ENTRYPOINT ["tail", "-f", "/dev/null"]
