FROM nginx:1.21

COPY dockerimage/nginx.conf /etc/nginx/nginx.conf
COPY dockerimage/default.conf /etc/nginx/conf.d/default.conf

COPY build/ /usr/share/nginx/html
