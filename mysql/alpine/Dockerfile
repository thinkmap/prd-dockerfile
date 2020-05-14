FROM alpine:latest
MAINTAINER Aplha

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added

RUN addgroup -S -g 1000 mysql && adduser -S -G mysql -u 999 mysql

# 替换阿里云的源
RUN echo "http://mirrors.aliyun.com/alpine/latest-stable/main/" > /etc/apk/repositories
RUN echo "http://mirrors.aliyun.com/alpine/latest-stable/community/" >> /etc/apk/repositories
RUN apk update --no-cache && apk upgrade --no-cache
# 设置时区
RUN apk --no-cache add tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone
# 安装常用软件
RUN apk add --update --no-cache curl openssh vim wget bash python3 && rm -rf /var/cache/apk/*
# 配置vim(右键不能粘贴，反而进入了visual模式)
#RUN sed -i "s/set mouse=a/set mouse-=a/g" /usr/share/vim/vim82/defaults.vim
RUN sed -i "s/set mouse=a/set mouse-=a/g" `find / -name defaults.vim`

RUN apk add --no-cache mysql-client
ENTRYPOINT ["mysql"]

