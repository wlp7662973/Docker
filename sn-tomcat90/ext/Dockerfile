#基于TOMCAT9.0.x的版本:
FROM wlp7662973/sn-tomcat90:35.2.2
# 2020年05月12日 升级为基于sn-oralcejdk8:3.11.6构建

MAINTAINER 王立鹏 TOMCAT8.5.x 带有扩展本地工具包 <wanglipeng@snsoft.com.cn>
#Build:docker build -t wlp7662973/sn-tomcat90:35.2.1-ext ./sn-tomcat90/ext
#Test:docker run -it --rm -p 8088:8080 --name test_tomcat90 wlp7662973/sn-tomcat90:35.2.1-ext
#Test:docker exec -it test_tomcat90 /bin/bash
#push:docker push wlp7662973/sn-tomcat90:35.2.1-ext

RUN set -eux; \
    apk add --no-cache graphicsmagick tesseract-ocr; \
    rm -rf /var/cache/apk/*

VOLUME ["/etc/snconfig","/var/log/snlog","/usr/local/tomcat/logs"]

EXPOSE 8080 8444 20880 20881 20882 20883

CMD ["catalina.sh", "run"]