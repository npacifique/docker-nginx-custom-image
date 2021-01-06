FROM ubuntu:latest AS builder

LABEL maintainer="np@npacifique.us"

ENV NGINX_VERSION 1.18.0


RUN apt-get update \
    && apt-get install -y wget \
    && apt-get install -y nginx \
    && apt-get install build-essential checkinstall zlib1g-dev -y 

RUN apt-get install -y libpcre3 \ 
    && apt-get install -y libssl-dev \
    && apt-get install -y libpcre3-dev \
    && apt-get install -y zlib1g \
    && apt-get install -y zlib1g-dev  \
    && apt-get install -y libgd-dev \ 
    && apt-get install -y curl \
    && apt-get install nano -y



#download sources http://nginx.org/en/download.html
#list of all modules from source http://nginx.org/en/docs/configure.html
RUN wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz \
    && tar -zxvf nginx-${NGINX_VERSION}.tar.gz && \
    cd nginx-${NGINX_VERSION} && \
    ./configure \
    #module to be removed 
    --without-http_autoindex_module \
    # add new modules here with backslash at the end 
    --sbin-path=/usr/bin/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --error-log-path=/var/logs/nginx/error.log \
    --http-log-path=/var/logs/nginx/access.log \
    --with-pcre \
    --pid-path=/var/run/nginx.pid \
    --with-http_ssl_module \
    --with-http_v2_module \
    --with-http_image_filter_module=dynamic && \
    make && make install

# remove the default site 
RUN mkdir /etc/nginx/ssl 
RUN rm -rf /etc/nginx/sites-available
RUN rm -rf /etc/nginx/sites-enabled
RUN rm -rf /etc/nginx/nginx.conf.default
RUN mv /etc/nginx/nginx.conf /etc/nginx/nginx.disbaled


FROM ubuntu:latest



#copy assets files
COPY ./nginx.conf /etc/nginx/ 
COPY self.crt /etc/nginx/ssl/
COPY self.key /etc/nginx/ssl/
COPY ./assets/www/html/index.html /var/www/html/
COPY ./assets/www/html/image01.jpg /var/www/html/
COPY ./assets/conf.d/default.conf /etc/nginx/conf.d/

#copy the entry point scrupt to the bin directory 
COPY entrypoint.sh /bin/

COPY --from=builder / /

ENTRYPOINT ["bin/entrypoint.sh"]

EXPOSE 80 443

STOPSIGNAL SIGQUIT

CMD ["nginx", "-g", "daemon off;"]
