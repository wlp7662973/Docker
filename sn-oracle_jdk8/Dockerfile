#FROM alpine:3.10.1
FROM wlp7662973/sn-alpine:3.11.6
# 2020年05月12日 升级为基于sn-alpine:3.11.6构建

MAINTAINER 王立鹏 Oracle JDK8 <wanglipeng@snsoft.com.cn>
# Build:docker build -t wlp7662973/sn-oralcejdk8:3.11.6.202 ./sn-oracle_jdk8
# Test:docker run -it --rm --name testjdk wlp7662973/sn-oralcejdk8:3.11.6.202
# push:docker push wlp7662973/sn-oralcejdk8:3.11.6.202

#设置语言/字符集
ENV LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8

#设置环境变量
ENV JAVA_HOME /opt/jdk
ENV PATH ${PATH}:/opt/jdk/bin

ENV JAVA_VERSION 8u202
ENV JAVA_ALPINE_VERSION 8.8u202.08

#下载JDK8的update192版本,该版本为可免费使用的最后版本,稍后切换为亚马逊https://aws.amazon.com/cn/corretto/
#公司内部地址,上传开源社区注释掉该行地址
#ENV JAVA_DOWNLOAD="https://n.esnsoft.cn/jdk/jdk-8u202-linux-x64.tar.gz"

RUN set -eux; \
    #下载,解压JDK
    mkdir -p /opt && mkdir -p /data/packages && mkdir -p /var/log/snlog && mkdir -p /etc/snconfig && curl -jfkS -H "Cookie: oraclelicense=accept-securebackup-cookie" -H "Connection: keep-alive" \
      -L "${JAVA_DOWNLOAD:-https://github.com/wlp7662973/oracle-java/releases/download/8u202/jdk-8u202-linux-x64.tar.gz}" \
      | tar -xzf - -C /opt \
    && ln -s /opt/jdk1.*.0_* /opt/jdk; \
    ## 清理没用到的文件和库,以精简JDK的体积
    rm -rf /opt/jdk/*src.zip \
           /opt/jdk/lib/missioncontrol \
           #/opt/jdk/lib/visualvm \
           /opt/jdk/lib/*javafx* \
           #/opt/jdk/jre/lib/plugin.jar \
           /opt/jdk/jre/lib/ext/jfxrt.jar \
           /opt/jdk/jre/bin/javaws \
           /opt/jdk/jre/lib/javaws.jar \
           /opt/jdk/jre/lib/desktop \
           #/opt/jdk/jre/plugin \
           /opt/jdk/jre/lib/deploy* \
           /opt/jdk/jre/lib/*javafx* \
           /opt/jdk/jre/lib/*jfx* \
           /opt/jdk/jre/lib/amd64/libdecora_sse.so \
           /opt/jdk/jre/lib/amd64/libprism_*.so \
           #/opt/jdk/jre/lib/amd64/libfxplugins.so \
           /opt/jdk/jre/lib/amd64/libglass.so \
           /opt/jdk/jre/lib/amd64/libgstreamer-lite.so \
           /opt/jdk/jre/lib/amd64/libjavafx*.so \
           /opt/jdk/jre/lib/amd64/libjfx*.so; \
    ##解决 新版Tomcat的Native编译后,运行报错:"libc.musl-x86_64.so.1: cannot open shared object file: No such file or directory"等类似错误
    ln -s /lib/libc.musl-x86_64.so.1 /usr/lib/libc.musl-x86_64.so.1; \
    ln -s /lib/libz.so.1 /usr/lib/libz.so.1; \
    ln -s /lib/libuuid.so.1 /usr/lib/libuuid.so.1; \
    #预制一个脚本,tomcat打包使用
    { \
        echo '#!/bin/sh'; \
        echo 'set -e'; \
        echo; \
        echo 'dirname "$(dirname "$(readlink -f "$(which javac || which java)")")"'; \
    } > /usr/local/bin/docker-java-home \
    && chmod +x /usr/local/bin/docker-java-home; \
    # https://github.com/docker-library/openjdk/issues/212#issuecomment-420979840
    # https://openjdk.java.net/jeps/341
    java -Xshare:dump; \
    # 基础测试
    java -version; \
    javac -version

ADD java.security /opt/jdk/jre/lib/security/java.security

EXPOSE 8000 8444 20880 20881 20882 20883

VOLUME ["/data/packages","/etc/snconfig","/var/log/snlog"]

CMD ["/bin/bash"]

WORKDIR /opt/jdk