FROM ubuntu:focal

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install wget openjdk-11-jdk openssh-server -y

SHELL ["/bin/bash", "-c"]

WORKDIR /tmp

RUN echo -e "root\nroot" | passwd && \
    mkdir /run/sshd && \
    chmod 755 /run/sshd && \
    sed "s/#PermitRootLogin prohibit-password/PermitRootLogin yes/g" -i /etc/ssh/sshd_config

RUN apt-get install telnet bind9-dnsutils -y

COPY ./entrypoint.sh .

ENTRYPOINT ["/bin/bash", "entrypoint.sh"]