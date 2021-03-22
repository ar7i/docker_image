FROM loomen/ubuntu-base:latest

# Install packages
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update

RUN apt-get install -y software-properties-common 

RUN add-apt-repository ppa:nginx/stable

RUN apt-get update
RUN apt-get upgrade -y

RUN apt-get install -y nginx

# Cleanup
RUN apt-get remove --purge -y software-properties-common

RUN apt-get autoremove -y
RUN apt-get clean
RUN apt-get autoclean

#tweek nginx config
RUN sed -i -e '/worker_processes/c\worker_processes  5;' /etc/nginx/nginx.conf
RUN sed -i -e '/keepalive_timeout/c\keepalive_timeout  2;' /etc/nginx/nginx.conf
RUN sed -i -e '/client_max_body_size/c\client_max_body_size 100m;' /etc/nginx/nginx.conf
RUN echo "daemon off;" >> /etc/nginx/nginx.conf

# nginx site conf
RUN mkdir -p /etc/nginx/ssl/
ADD ssl/nginx.crt /etc/nginx/ssl/nginx.crt
ADD ssl/nginx.key /etc/nginx/ssl/nginx.key

RUN rm -Rf /etc/nginx/conf.d/*
RUN rm -Rf /etc/nginx/sites-available/default
RUN rm -Rf /etc/nginx/sites-enabled/default

ADD conf/nginx-site.conf /etc/nginx/sites-available/default.conf
RUN ln -s /etc/nginx/sites-available/default.conf /etc/nginx/sites-enabled/default.conf

# Supervisor Config
ADD conf/supervisord.conf /etc/supervisord.conf

# Start Supervisord
ADD scripts/start.sh /start.sh
RUN chmod 755 /start.sh

# Setup Volume
VOLUME ["/app"]

# change nginx directory
RUN mkdir -p /app/src/public
RUN mkdir -p /var/www/html/logs

# add test PHP file
ADD src/index.html /app/src/public/index.html
RUN chown -Rf www-data.www-data /app
RUN chown -Rf www-data.www-data /var/www/html


# Expose Ports
EXPOSE 443
EXPOSE 80

CMD ["/bin/bash", "/start.sh"]
