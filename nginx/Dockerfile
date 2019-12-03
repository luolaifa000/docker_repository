# This my first nginx Dockerfile
# Version 1.0

# Base images 基础镜像
FROM centos:centos7

#MAINTAINER 维护者信息
MAINTAINER yumancang laifaluo@126.com

ENV NGINX_VERSION 1.16.0

#用户账号和临时代理环境变量设置
RUN /usr/sbin/groupadd user00 && \
/usr/sbin/useradd -g user00 user00 && \
echo "user00 ALL=(ALL) ALL" >> /etc/sudoers && \
echo 'user00:123465' | chpasswd && \
mkdir -p /data/soft && mkdir -p /home/user00/web

#修改vim中文乱码问题
RUN echo 'set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936' >> /home/user00/.vimrc && \
echo 'set termencoding=utf-8' >> /home/user00/.vimrc && \
echo 'set encoding=utf-8' >> /home/user00/.vimrc

WORKDIR /data/soft

RUN yum install -y pcre-devel wget net-tools gcc zlib zlib-devel make openssl-devel

#安装nginx
RUN wget -c http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz && \
    tar zxvf nginx-$NGINX_VERSION.tar.gz && rm -f nginx-$NGINX_VERSION.tar.gz && cd nginx-$NGINX_VERSION && \
    mkdir -p /var/cache/nginx && \
    ./configure --prefix=/etc/nginx \
    --sbin-path=/usr/sbin/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --pid-path=/var/run/nginx.pid \
    --lock-path=/var/run/nginx.lock \
    --http-client-body-temp-path=/var/cache/nginx/client_temp \
    --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
    --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
    --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
    --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
    --user=user00 \
    --group=user00 \
    --with-http_ssl_module \
    --with-http_realip_module \
    --with-http_addition_module \
    --with-http_sub_module \
    --with-http_dav_module \
    --with-http_flv_module \
    --with-http_mp4_module \
    --with-http_gunzip_module \
    --with-http_gzip_static_module \
    --with-http_random_index_module \
    --with-http_secure_link_module \
    --with-http_stub_status_module \
    --with-http_auth_request_module \
    --with-threads \
    --with-stream \
    --with-stream_ssl_module \
    --with-http_slice_module \
    --with-mail \
    --with-mail_ssl_module \
    --with-file-aio \
    --with-http_v2_module && \
    make -j 4 && make install && \
    mkdir -p /etc/nginx/conf.d/ && \
    mkdir -p /etc/nginx/logs



