FROM centos:7.2.1511
MAINTAINER Mathieu BODIN <mathieu.bodin@gmail.com>

ENV NGINX_VERSION 1.10.1

ENV NGINX_HOME "/opt/nginx-${NGINX_VERSION}"

RUN yum install -q -y --nogpgcheck make gcc pcre pcre-devel zlib-devel openssl-devel

RUN curl -L -S -s "http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz" -o "/opt/nginx-${NGINX_VERSION}-src.tar.gz" \
	&& mkdir -p "/opt/nginx-${NGINX_VERSION}-src" \
	&& tar xzf "/opt/nginx-${NGINX_VERSION}-src.tar.gz" -C "/opt/nginx-${NGINX_VERSION}-src" --strip-components=1 \
 	&& cd "/opt/nginx-${NGINX_VERSION}-src" \
 	&& ./configure --prefix=$NGINX_HOME --with-http_ssl_module \
 	&& make \
 	&& make install \
 	&& cd / \
 	&& rm -rf "/opt/nginx-${NGINX_VERSION}-src"

ENV PATH $NGINX_HOME/sbin:$PATH

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]