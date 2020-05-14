#基于TOMCAT8.5.x的版本:
FROM wlp7662973/sn-tomcat85:55.1.1
# 2020年05月12日 升级为基于sn-oralcejdk8:3.11.6构建

MAINTAINER 王立鹏 TOMCAT8.5.x 带有 <wanglipeng@snsoft.com.cn>
#Build:docker build -t wlp7662973/sn-tomcat85:55.1.1-ext ./sn-tomcat85/ext
#Test:docker run -it --rm -p 8088:8080 --name test_tomcat8 wlp7662973/sn-tomcat85:55.1.1-ext
#Test:docker exec -it test_tomcat8 /bin/bash
#push:docker push wlp7662973/sn-tomcat85:55.1.1-ext

RUN set -eux; \
    apk add --no-cache graphicsmagick tesseract-ocr; \
    rm -rf /var/cache/apk/*

VOLUME ["/etc/snconfig","/var/log/snlog","/usr/local/tomcat/logs"]

EXPOSE 8080 8444 20880 20881 20882 20883

CMD ["catalina.sh", "run"]