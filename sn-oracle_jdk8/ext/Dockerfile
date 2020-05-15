#FROM alpine:3.10.1
FROM wlp7662973/sn-oralcejdk8:3.11.6.202
# 2020年05月12日 升级为基于sn-alpine:3.11.6构建

MAINTAINER 王立鹏 Oracle JDK8 <wanglipeng@snsoft.com.cn>
# Build:docker build -t wlp7662973/sn-oralcejdk8:3.11.6.202-ext ./sn-oracle_jdk8/ext
# Test:docker run -it --rm --name testjdk wlp7662973/sn-oralcejdk8:3.11.6.202-ext
# push:docker push wlp7662973/sn-oralcejdk8:3.11.6.202-ext

RUN set -eux; \
    apk add --no-cache graphicsmagick tesseract-ocr; \
    rm -rf /var/cache/apk/*

EXPOSE 8000 8444 20880 20881 20882 20883

VOLUME ["/data/packages","/etc/snconfig","/var/log/snlog"]

CMD ["/bin/bash"]