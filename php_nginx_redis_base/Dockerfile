FROM centos:centos7

MAINTAINER yumancang laifaluo@126.com

ENV PHP_VERSION 7.2.19
ENV NGINX_VERSION 1.16.0
ENV REDIS_VERSION 4.2.0

#用户账号和临时代理环境变量设置
RUN /usr/sbin/groupadd user00 && \
/usr/sbin/useradd -g user00 user00 && \
echo "user00 ALL=(ALL) ALL" >> /etc/sudoers && \
echo 'user00:123465' | chpasswd && \
chage -M 99999 user00 && \
sed -i 's/Defaults    requiretty/#Defaults    requiretty/g' /etc/sudoers && \
mkdir -p /data/soft && mkdir -p /home/user00/web

WORKDIR /data/soft

#安装依赖
RUN yum makecache && \
yum install -y vim \
telnet \
make \
libtool \
libssl-dev \
wget \
zip \
unzip \
net-tools \
gcc \
gcc-c++ \
autoconf \
automake \
cmake \
initscripts \
freetype-devel \
libjpeg \
libjpeg-devel \
libjpeg-devel.x86_64 \
libpng-devel.x86_64 \
libXpm-devel.x86_64 \
bzip2 \
bzip2-devel.x86_64 \
readline-devel.x86_64 \
libxml2-devel.x86_64 \
libxslt-devel.x86_64 \
openssl-devel.x86_64 \
expat-devel.x86_64 \
gettext-devel.x86_64 \
zlib-devel.x86_64 \
gmp-devel.x86_64  \
libcurl-devel.x86_64 \
mysql \
python-setuptools \
python36 \
python36-pip \
libXrender \
libXext \
libSM && \
yum clean all

RUN ln -s /usr/bin/python3.6 /usr/local/python3 && ln -s /usr/bin/pip3.6 /usr/local/pip3

#修改vim中文乱码问题
RUN echo 'set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936' >> /home/user00/.vimrc && \
echo 'set termencoding=utf-8' >> /home/user00/.vimrc && \
echo 'set encoding=utf-8' >> /home/user00/.vimrc


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

#安装libmcrypt
RUN wget -c http://sourceforge.net/projects/mcrypt/files/Libmcrypt/2.5.8/libmcrypt-2.5.8.tar.gz/download -O ./libmcrypt-2.5.8.tar.gz && \
tar zxvf libmcrypt-2.5.8.tar.gz && rm -f libmcrypt-2.5.8.tar.gz &&\
cd libmcrypt-2.5.8 && \
./configure --prefix=/usr/local && \
make && make install &&\
cd .. && rm -rf libmcrypt-2.5.8 && \
/sbin/ldconfig


#重新编译无NSS的curl库
RUN wget https://curl.haxx.se/download/curl-7.29.0.tar.gz && \
tar -zxvf curl-7.29.0.tar.gz && \
cd curl-7.29.0 && \
./configure --prefix=/usr --without-nss && \
make && make install && \
cd .. && rm -rf curl-7.29.0 && \
/sbin/ldconfig

# 安装 PHP7
RUN wget -c http://cn2.php.net/distributions/php-$PHP_VERSION.tar.gz && \
tar zxvf php-$PHP_VERSION.tar.gz && rm -f php-$PHP_VERSION.tar.gz &&\
echo '/usr/local/lib' >> /etc/ld.so.conf && \
echo '/usr/local/lib64' >> /etc/ld.so.conf && \
/sbin/ldconfig && \
cd php-$PHP_VERSION && \
./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-pear=/usr/local/php/PEAR --with-config-file-scan-dir=/usr/local/php/etc/conf.d --with-fpm-user=user00 --with-fpm-group=user00 --enable-libxml --enable-session --with-pcre-regex --enable-xml --enable-simplexml --enable-filter --disable-debug --enable-inline-optimization --disable-rpath --disable-static --enable-shared --with-pic --with-gnu-ld --enable-re2c-cgoto --enable-hash --with-mhash --enable-fpm --with-pdo-mysql --enable-sockets --with-curl --with-zlib --enable-bcmath --enable-ftp --enable-exif --with-bz2 --enable-calendar --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-shmop --enable-soap --with-readline --enable-zip --with-xsl --with-mhash --with-gettext --with-gmp --enable-mbstring --with-mcrypt --enable-calendar --with-openssl --enable-pcntl --with-xmlrpc --enable-wddx --with-mysqli --enable-dba --enable-pcntl --enable-opcache --with-png-dir --with-freetype-dir --with-jpeg-dir --with-zlib-dir --with-gd && \
make clean && make -j 4 && make install && \
cp php.ini-production /usr/local/php/etc/php.ini && \
cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf && \
cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf && \
echo 'export PATH=/usr/local/php/bin:/usr/local/php/sbin:$PATH' >> /etc/profile && \
source /etc/profile


#安装PHP的Redis扩展
RUN wget -c http://pecl.php.net/get/redis-$REDIS_VERSION.tgz &&tar zxvf redis-$REDIS_VERSION.tgz && \
rm -rf redis-$REDIS_VERSION.tgz &&cd redis-$REDIS_VERSION && /usr/local/php/bin/phpize && \
./configure --with-php-config=/usr/local/php/bin/php-config && make && make install && \
mkdir -p /usr/local/php/etc/conf.d && \
echo "extension=redis.so" > /usr/local/php/etc/conf.d/redis.ini && \
cd .. &&rm -rf redis-$REDIS_VERSION

#安装supervisor
RUN easy_install supervisor && \
mkdir /etc/supervisor && \
echo_supervisord_conf > /etc/supervisor/supervisord.conf && \
sed -i "s/\;\[include\]/\[include\]/g" /etc/supervisor/supervisord.conf && \
echo 'files = conf.d/*.conf' >> /etc/supervisor/supervisord.conf && \
mkdir /etc/supervisor/conf.d

#安装composer
RUN curl -sS https://getcomposer.org/installer | /usr/local/php/bin/php && \
mv composer.phar /usr/local/bin/composer

#安装Redis
RUN wget -c http://download.redis.io/releases/redis-5.0.3.tar.gz && \
tar xzf redis-5.0.3.tar.gz && \
mv redis-5.0.3 /etc/redis-5.0.3 && \
rm redis-5.0.3.tar.gz && \
cd /etc/redis-5.0.3 && \
make && make install && \
echo 'export PATH=/etc/redis-5.0.3/src:$PATH' >> /etc/profile



# 开放端口
EXPOSE 81 22 6379 9000
