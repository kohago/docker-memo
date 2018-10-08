FROM alpine:latest
LABEL maintainer="kouha.shu@gmail.com"
LABEL description="This is just a docker file sample"

RUN apk add --update nginx && \
    rm -rf /var/cache/apk/* && \
    mkdir -p /tmp/nginx/

COPY files/nginx.conf /etc/nginx/nginx.conf
COPY file/default.conf /etc/nginx/conf.d/default.conf
COPY files/html.tar.gz /usr/share/nginx/

EXPOSE 80/tcp

# docker run comand will be run by nginx
# example: docker container run -d --name nginx imagename -v
#==>will be[ nginx -v ]
# nginx -g ==>global configuration
# daemon off -->nginx will not be daemon
ENTRYPOINT ["nginx"]
CMD ["-g","daemon off;"]
