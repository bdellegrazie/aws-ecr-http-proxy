FROM openresty/openresty:1.15.8.3-alpine
USER root

RUN apk add -v --no-cache bind-tools python3 py3-pip supervisor \
 && mkdir /cache \
 && addgroup -g 101 nginx \
 && adduser -u 100  -D -S -h /cache -s /sbin/nologin -G nginx nginx \
 && pip3 install --upgrade pip awscli==1.18.32 \
 && apk -v --purge del py3-pip

COPY files/startup.sh files/renew_token.sh /
COPY files/ecr.ini /etc/supervisor.d/ecr.ini
COPY files/root /etc/crontabs/root

COPY files/nginx.conf /usr/local/openresty/nginx/conf/nginx.conf
COPY files/ssl.conf /usr/local/openresty/nginx/conf/ssl.conf

ENV PORT 5000
ENV AWS_SDK_LOAD_CONFIG true
RUN chmod a+x /startup.sh /renew_token.sh

ENTRYPOINT ["/startup.sh"]
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
