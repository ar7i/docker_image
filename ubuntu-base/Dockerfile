FROM ubuntu:18.04

# Surpress Upstart errors/warning
RUN dpkg-divert --local --rename --add /sbin/initctl \
    && ln -sf /bin/true /sbin/initctl

# Install packages and Oh My Zsh and cleanup
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y supervisor \
        bash-completion \
        tzdata \
    && apt-get autoremove -y \
    && apt-get clean \
    && apt-get autoclean
RUN cp /usr/share/zoneinfo/Europe/Warsaw /etc/localtime
