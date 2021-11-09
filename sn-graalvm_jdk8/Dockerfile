FROM wlp7662973/sn-ubuntu:20.04
# 2021年10月26日 升级为基于强化版sn-ubuntu:20.04构建

MAINTAINER 王立鹏 GraaVM OpenJDK8 <wanglipeng@snsoft.com.cn>
# Build:docker build -t wlp7662973/sn-graalvm:8.302.212 ./sn-graalvm_jdk8
# Test:docker run -it --rm --name testjdk wlp7662973/sn-graalvm:8.302.212
# push:docker push wlp7662973/sn-graalvm:8.302.212
#   .212表示基于graalvm 21.2版本

#设置语言/字符集
ENV LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8

#设置环境变量
ENV JAVA_HOME /opt/jdk
ENV GRAALVM_HOME /opt/jdk
ENV PATH ${PATH}:/opt/jdk/bin

ENV JAVA_VERSION 8u302
ENV JAVA_ALPINE_VERSION 8.8u302.07

#下载JDK8的update311版本
#公司内部地址,上传开源社区注释掉该行地址
ENV JAVA_DOWNLOAD="https://n.esnsoft.cn/jdk/graalvm-ce-java8-linux-amd64-21.2.0.tar.gz"

RUN set -eux; \
    #下载,解压JDK
    mkdir -p /opt && mkdir -p /var/log/snlog && mkdir -p /etc/snconfig && curl -jfkS -H "Cookie: oraclelicense=accept-securebackup-cookie" -H "Connection: keep-alive" \
      -L "${JAVA_DOWNLOAD:-https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-21.2.0/graalvm-ce-java8-linux-amd64-21.2.0.tar.gz}" \
      | tar -xzf - -C /opt \
    && ln -s /opt/graalvm-ce-java8-* /opt/jdk; \
    ## 清理没用到的文件和库,以精简JDK的体积
    find /opt/jdk/ -name "*src.zip"  -printf "Deleting %p\n" -exec rm {} + && \
        { test ! -d /opt/jdk/legal || tar czf /opt/jdk/legal.tgz /opt/jdk/legal/; } && \
        { test ! -d /opt/jdk/legal || rm -r /opt/jdk/legal; } && \
        rm -rf /opt/jdk/man `# does not exist in java11 package` && \
        echo Cleaning up... && \
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

VOLUME ["/etc/snconfig","/var/log/snlog"]

CMD ["/bin/bash"]

WORKDIR /opt/jdk