FROM nginx:latest as builder

LABEL maintainer="shubnikov.av@gmail.com"

RUN apt-get update \
    && apt-get install -y \
      libmaxminddb-dev  \
      mmdb-bin \
      git \
      build-essential \
      libpcre3-dev \
      zlib1g-dev

RUN cd /opt \
    && git clone https://github.com/leev/ngx_http_geoip2_module \
    && curl -L http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz | tar -xz \
    && cd nginx-$NGINX_VERSION \
    && ./configure --with-compat --add-dynamic-module=/opt/ngx_http_geoip2_module \
    && make modules

FROM nginx:latest

COPY --from=builder /opt/nginx-$NGINX_VERSION/objs/ngx_http_geoip2_module.so /usr/lib/nginx/modules

RUN apt-get update \
    && apt-get install --no-install-recommends --no-install-suggests -y libmaxminddb0 \
    && apt-get purge -y --auto-remove
