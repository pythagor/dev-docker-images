FROM ubuntu:18.04

WORKDIR /home

RUN apt update && \
    apt install -y \
    wget openjdk-8-jre \
    libxext-dev \
    libxrender-dev \
    libxtst-dev \
    git \
    fonts-hack-ttf \
    inetutils-ping && \
    apt clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/*

RUN useradd -m -s /bin/bash developer \
    && mkdir /opt/phpstorm \
    && chown -R developer.developer /opt/phpstorm \
    && wget -O - https://download-cf.jetbrains.com/webide/PhpStorm-2019.3.3.tar.gz | tar xzf - --strip-components=1 -C "/opt/phpstorm"

RUN mkdir /home/developer/.PhpStorm2019.3 \
    && touch /home/developer/.PhpStorm2019.3/.keep \
    && mkdir /home/developer/.java \
    && chown -R developer.developer /home/developer

RUN usermod -u 1000 developer && groupmod -g 1000 developer

USER 1000:1000

VOLUME ["/home/developer/.java", "/home/developer/.PhpStorm2019.2", "/home/developer/.PhpStorm2019.3"]

# Keep container running
ENTRYPOINT ["tail", "-f", "/dev/null"]
