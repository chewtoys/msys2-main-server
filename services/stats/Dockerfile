FROM ubuntu:24.04

# to test locally, run `vnstatd -n` once in the container

RUN \
    apt update && \
    env DEBIAN_FRONTEND=noninteractive apt install -y --no-install-recommends \
        apache2 \
        composer \
        curl \
        libapache2-mod-php8.3 \
        patchutils \
        unzip \
        vnstat \
    && \
    rm -Rf /var/{cache/apt,lib/apt/lists,log/{alternatives.log,apt,dpkg.log}} && \
true

RUN ln -sf /dev/stdout /var/log/apache2/access.log && \
    ln -sf /dev/stderr /var/log/apache2/error.log

WORKDIR /var/www/html/
RUN rm index.html
RUN curl -sL https://github.com/msys2-contrib/vnstat-dashboard/archive/master.tar.gz | tar -xzf - --wildcards --strip-components=2 'vnstat-dashboard-master/app/*'
RUN composer install
RUN mkdir -m=0777 templates_c/

VOLUME /var/lib/vnstat/
EXPOSE 80
RUN sed -e '/^*)/ { n; s/$HTTPD/exec &/; }' -i /usr/sbin/apache2ctl
ENTRYPOINT [ "apache2ctl" ]
CMD [ "-DFOREGROUND" ]
