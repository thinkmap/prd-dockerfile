FROM alpine:latest

# 替换阿里云的源
RUN echo "http://mirrors.aliyun.com/alpine/latest-stable/main/" > /etc/apk/repositories
RUN echo "http://mirrors.aliyun.com/alpine/latest-stable/community/" >> /etc/apk/repositories
RUN apk update --no-cache && apk upgrade --no-cache
# 设置时区
RUN apk --no-cache add tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone

#Alpine安装 Glibc https://github.com/sgerrand/alpine-pkg-glibc
RUN wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub && \
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.31-r0/glibc-2.31-r0.apk && \
    apk add glibc-2.31-r0.apk
    
ADD jdk-8u251-linux-x64.tar.gz /opt/base/
RUN cd /opt/base/ && tar -zxf jdk-8u251-linux-x64.tar.gz && mv jdk1.8.0_251 jdk
    
ENV JAVA_HOME=/opt/base/jdk
ENV JRE_HOME=${JAVA_HOME}/jre  
ENV CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
ENV PATH=${JAVA_HOME}/bin:$PATH
# basic smoke test
	java --version; \
	javac --version

ENV JAVA_OPTS="-server -Xms2g -Xmx2g -Xmn512m -XX:PermSize=128M -XX:MaxNewSize=128m -XX:MaxPermSize=25zh6m -Xss256k  -Djava.awt.headless=true -XX:+CMSClassUnloadingEnabled -XX:+CMSPermGenSweepingEnabled -Duser.timezone=GMT+08"
#APPFILE_PATH="-Dspring.config.location=/usr/local/config/application.properties"
ADD app.jar /home/jar/app.jar

ENTRYPOINT ["sh","-c","java $JAVA_OPTS -jar $APP_PATH/$APP_NAME > out.log 2>&1 &"]
