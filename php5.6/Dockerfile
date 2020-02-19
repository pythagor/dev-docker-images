FROM ubuntu:16.04

RUN apt clean && apt -y update && apt install -y locales && locale-gen en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN apt -y update \
    && apt install -y \
	curl \
	zip \
	unzip \
	git \
	software-properties-common \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y tzdata

RUN add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y \
	php5.6-fpm \
	php5.6-cli \
	php5.6-gd \
	php5.6-mysql \
        php5.6-pgsql \
	php5.6-imap \
	php-memcached \
	php5.6-mbstring \
	php5.6-xml \
	php5.6-curl \
        php5.6-sqlite \
	php5.6-soap \
	php5.6-zip \
	php-mongodb \
	php5.6-gmp \
	php5.6-intl \
	php-redis \
        php5.6-mcrypt \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && apt install -y php-xdebug \
    && mkdir /run/php \
    && apt remove -y --purge software-properties-common \
    && apt clean \
    && apt autoremove -y \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN echo 'xdebug.remote_autostart=1' >> /etc/php/5.6/cli/php.ini \
    && echo 'xdebug.remote_enable=1' >> /etc/php/5.6/cli/php.ini \
    && echo 'xdebug.remote_host=phpstorm' >> /etc/php/5.6/cli/php.ini \
    && echo 'xdebug.idekey=PHPSTORM' >> /etc/php/5.6/cli/php.ini

RUN echo 'xdebug.remote_autostart=1' >> /etc/php/5.6/fpm/php.ini \
    && echo 'xdebug.remote_enable=1' >> /etc/php/5.6/fpm/php.ini \
    && echo 'xdebug.remote_host=phpstorm' >> /etc/php/5.6/fpm/php.ini \
    && echo 'xdebug.idekey=PHPSTORM' >> /etc/php/5.6/fpm/php.ini

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

ADD config/php-fpm.conf /etc/php/5.6/fpm/php-fpm.conf
ADD config/www.conf /etc/php/5.6/fpm/pool.d/www.conf

EXPOSE 9000

RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

USER 1000:1000

WORKDIR /workspace

ENTRYPOINT ["tail", "-f", "/dev/null"]
