FROM centos:centos7

MAINTAINER yumancang laifaluo@126.com

#后端环境构建

ENV NGINX_VERSION 1.16.0
ENV GO_VERSION 1.13.3


#用户账号和临时代理环境变量设置
RUN /usr/sbin/groupadd user00 && \
    /usr/sbin/useradd -g user00 user00 && \
    echo "user00 ALL=(ALL) ALL" >> /etc/sudoers && \
    echo 'user00:123465' | chpasswd && \
    chage -M 99999 user00 && \
    sed -i 's/Defaults requiretty/#Defaults requiretty/g' /etc/sudoers && \
    mkdir -p /data/soft && mkdir -p /home/user00/web

WORKDIR /data/soft


# 更新YUM && 安装依赖 && 蓝盾Locale设置
RUN yum clean all && \
    yum makecache && \
    yum update -y && \
    yum install -y glibc-common net-tools wget curl nss make telnet gcc gcc-c++ git pango.x86_64 libXcomposite.x86_64 libXcursor.x86_64 libXdamage.x86_64 libXext.x86_64 libXi.x86_64 libXtst.x86_64 cups-libs.x86_64 libXScrnSaver.x86_64 libXrandr.x86_64 GConf2.x86_64 alsa-lib.x86_64 atk.x86_64 gtk3.x86_64 ipa-gothic-fonts xorg-x11-fonts-100dpi xorg-x11-fonts-75dpi xorg-x11-utils xorg-x11-fonts-cyrillic xorg-x11-fonts-Type1 xorg-x11-fonts-misc vim gcc gcc-c++ autoconf automake cmake initscripts libjpeg-devel.x86_64 libpng-devel.x86_64 libXpm-devel.x86_64 bzip2-devel.x86_64 readline-devel.x86_64 libxml2-devel.x86_64 libxslt-devel.x86_64 openssl-devel.x86_64 expat-devel.x86_64 gettext-devel.x86_64 zlib-devel.x86_64 gmp-devel.x86_64 libcurl-devel.x86_64 mysql python-setuptools python34 python34-pip && \
    localedef -c -f UTF-8 -i zh_CN zh_CN.UTF-8 && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

ENV LANG=zh_CN.UTF-8 \
    LANGUAGE=zh_CN:zh \
    LC_ALL=zh_CN.UTF-8



#安装NGINX

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


#前端环境构建


# 下载Node.js
RUN source /etc/profile && \
    wget 'https://nodejs.org/dist/v10.16.3/node-v10.16.3-linux-x64.tar.xz' && \
    tar -xvf ./node-v10.16.3-linux-x64.tar.xz && \
    rm -rf ./node-v10.16.3-linux-x64.tar.xz && \
    cd ./node-v10.16.3-linux-x64/bin && \
    echo 'export PATH='`pwd`':$PATH' >> /etc/profile && \
    cd /

# 配置NPM和Yarn
RUN source /etc/profile && \
    npm install -g yarn && \
    yarn config set strict-ssl=false && \
    yarn config set disable-self-update-check true


# 执行后续的清理工作，减小体积
RUN source /etc/profile && \
    npm cache clean --force && \
    yarn cache clean --force && \
    yum clean all && \
    rm -rf /tmp/* && \
    rm -rf /var/cache/yum/*

#docker exec -it test_profile bash



# 开放端口
EXPOSE 80 22 8080
