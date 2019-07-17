#FROM alpine:3.10.1
FROM wlp7662973/sn-alpine:3

MAINTAINER 王立鹏 JDK8 <wanglipeng@snsoft.com.cn>
# Build:docker build -t wlp7662973/sn-oralcejdk8:2 ./sn-oracle_jdk8
# Test:docker run -it --rm --name testjdk wlp7662973/sn-oralcejdk8:2
# push:docker push wlp7662973/sn-oralcejdk8:2

#设置语言/字符集
ENV LANG C.UTF-8

#下载JDK8的update192版本,该版本为可免费使用的最后版本,稍后切换为亚马逊https://aws.amazon.com/cn/corretto/
#公司内部地址,上传开源社区注释掉该行地址
#ENV JAVA_DOWNLOAD="https://n.esnsoft.cn/jdk-8u192-linux-x64.tar.gz"
ENV JAVA_DOWNLOAD="http://47.89.49.183:9990/java/jdk-8u192-linux-x64.tar.gz"

RUN set -eux; \
    #下载,解压JDK
    mkdir -p /opt && curl -jfkS -H "Cookie: oraclelicense=accept-securebackup-cookie" -H "Connection: keep-alive" \
      -L "${JAVA_DOWNLOAD:-https://github.com/frekele/oracle-java/releases/download/8u181-b13/jre-8u181-linux-x64.tar.gz}" \
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
    ln -s /lib/libuuid.so.1 /usr/lib/libuuid.so.1

ADD java.security /opt/jdk/jre/lib/security/java.security

#预制一个脚本,tomcat打包使用
RUN { \
		echo '#!/bin/sh'; \
		echo 'set -e'; \
		echo; \
		echo 'dirname "$(dirname "$(readlink -f "$(which javac || which java)")")"'; \
	} > /usr/local/bin/docker-java-home \
	&& chmod +x /usr/local/bin/docker-java-home

#设置环境变量
ENV JAVA_HOME /opt/jdk 
ENV PATH ${PATH}:/opt/jdk/bin

#ENV JAVA_VERSION 8u192
#ENV JAVA_ALPINE_VERSION 8.8u192.12

ENV JAVA_VERSION 8u181
ENV JAVA_ALPINE_VERSION 8.8u181.13

CMD ["/bin/bash"]

WORKDIR /opt/jdk