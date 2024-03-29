#基于Oracle GraalVM的版本:
FROM wlp7662973/sn-graalvm:8.302.212

MAINTAINER 王立鹏 TOMCAT9.0.x <wanglipeng@snsoft.com.cn>
#Build:docker build -t wlp7662973/sn-graal-tomcat90:54 ./sn-graal-tomcat90
#Test:docker run -it --rm -p 8088:8080 --name test_tomcat90 wlp7662973/sn-graal-tomcat90:54
#Test:docker exec -it test_tomcat90 /bin/bash
#push:docker push wlp7662973/sn-graal-tomcat90:54


ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH
RUN mkdir -p "$CATALINA_HOME"
WORKDIR $CATALINA_HOME

# let "Tomcat Native" live somewhere isolated
ENV TOMCAT_NATIVE_LIBDIR $CATALINA_HOME/native-jni-lib
ENV LD_LIBRARY_PATH ${LD_LIBRARY_PATH:+$LD_LIBRARY_PATH:}$TOMCAT_NATIVE_LIBDIR

# see https://www.apache.org/dist/tomcat/tomcat-9/KEYS
# see also "versions.sh" (https://github.com/docker-library/tomcat/blob/master/versions.sh)
ENV GPG_KEYS 48F8E69F6390C9F25CFEDCD268248959359E722B A9C5DF4D22E99998D9875A5110C01C5A2F6059E7 DCFD35E0BF8CA7344752DE8B6FB21E8933C60243


ENV TOMCAT_MAJOR 9
ENV TOMCAT_VERSION 9.0.54
ENV TOMCAT_SHA512 83430f24d42186ce2ff51eeef2f7a5517048f37d9050c45cac1e3dba8926d61a1f7f5aba122a34a11ac1dbdd3c1f6d98671841047df139394d43751263de57c3

RUN set -eux; \
    # 安装必要的依赖,主要是OpenJDK的需要,sn-oralcejdk8镜像已经安装过了.
	\
    savedAptMark="$(apt-mark showmanual)"; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        dirmngr \
        gnupg \
    ; \
    # 下载函数,包含下载地址...
    \
    ddist() { \
        local f="$1"; shift; \
        local distFile="$1"; shift; \
        local mvnFile="${1:-}"; \
        local success=; \
        local distUrl=; \
        for distUrl in \
# https://issues.apache.org/jira/browse/INFRA-8753?focusedCommentId=14735394#comment-14735394 \
            "https://repo1.maven.org/maven2/org/apache/tomcat/tomcat/$mvnFile" \
            "https://www.apache.org/dyn/closer.cgi?action=download&filename=$distFile" \
# if the version is outdated (or we're grabbing the .asc file), we might have to pull from the dist/archive :/
            "https://downloads.apache.org/$distFile" \
            "https://www-us.apache.org/dist/$distFile" \
            "https://www.apache.org/dist/$distFile" \
            "https://archive.apache.org/dist/$distFile" \
# if all else fails, let's try Maven (https://www.mail-archive.com/users@tomcat.apache.org/msg134940.html; https://mvnrepository.com/artifact/org.apache.tomcat/tomcat; https://repo1.maven.org/maven2/org/apache/tomcat/tomcat/)
            ${mvnFile:+"https://repo1.maven.org/maven2/org/apache/tomcat/tomcat/$mvnFile"} \
        ; do \
            if curl -fL -o "$f" "$distUrl" && [ -s "$f" ]; then \
                success=1; \
                break; \
            fi; \
        done; \
        [ -n "$success" ]; \
    }; \
    #下载, 解压, 验证...
    \
    ddist 'tomcat.tar.gz' "tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz" "$TOMCAT_VERSION/tomcat-$TOMCAT_VERSION.tar.gz"; \
    echo "$TOMCAT_SHA512 *tomcat.tar.gz" | sha512sum --strict --check -; \
    ddist 'tomcat.tar.gz.asc' "tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc" "$TOMCAT_VERSION/tomcat-$TOMCAT_VERSION.tar.gz.asc"; \
    export GNUPGHOME="$(mktemp -d)"; \
    for key in $GPG_KEYS; do \
        gpg --batch --keyserver keyserver.ubuntu.com --recv-keys "$key"; \
    done; \
    gpg --batch --verify tomcat.tar.gz.asc tomcat.tar.gz; \
    tar -xf tomcat.tar.gz --strip-components=1; \
    rm bin/*.bat; \
    rm tomcat.tar.gz*; \
    command -v gpgconf && gpgconf --kill all || :; \
    rm -rf "$GNUPGHOME"; \
    #出于安全考虑, 将webapps改名
    \
# https://tomcat.apache.org/tomcat-9.0-doc/security-howto.html#Default_web_applications
    mv webapps webapps.dist; \
    mkdir webapps; \
# we don't delete them completely because they're frankly a pain to get back for users who do want them, and they're generally tiny (~7MB)
	#配置并编译/安装Native程序
	\
    nativeBuildDir="$(mktemp -d)"; \
    tar -xf bin/tomcat-native.tar.gz -C "$nativeBuildDir" --strip-components=1; \
    apt-get install -y --no-install-recommends \
        dpkg-dev \
        gcc \
        libapr1-dev \
        libssl-dev \
        make \
    ; \
    ( \
        export CATALINA_HOME="$PWD"; \
        cd "$nativeBuildDir/native"; \
        gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; \
        aprConfig="$(command -v apr-1-config)"; \
        ./configure \
            --build="$gnuArch" \
            --libdir="$TOMCAT_NATIVE_LIBDIR" \
            --prefix="$CATALINA_HOME" \
            --with-apr="$aprConfig" \
            --with-java-home="$JAVA_HOME" \
            --with-ssl=yes \
        ; \
        nproc="$(nproc)"; \
        make -j "$nproc"; \
        make install; \
    ); \
    rm -rf "$nativeBuildDir"; \
    rm bin/tomcat-native.tar.gz; \
    #搜索运行Native程序需要的依赖库,将其保留.
    \
# reset apt-mark's "manual" list so that "purge --auto-remove" will remove all build dependencies
    apt-mark auto '.*' > /dev/null; \
    [ -z "$savedAptMark" ] || apt-mark manual $savedAptMark > /dev/null; \
    find "$TOMCAT_NATIVE_LIBDIR" -type f -executable -exec ldd '{}' ';' \
        | awk '/=>/ { print $(NF-1) }' \
        | xargs -rt readlink -e \
        | sort -u \
        | xargs -rt dpkg-query --search \
        | cut -d: -f1 \
        | sort -u \
        | tee "$TOMCAT_NATIVE_LIBDIR/.dependencies.txt" \
        | xargs -r apt-mark manual \
    ; \
    # 删除下载程序需要的依赖,删除编译Native程序需要的依赖
	\
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
    rm -rf /var/lib/apt/lists/*; \
    # 修改sh脚本的执行头,Docker中运行有问题 \
    # 移除临时变量,因sh的Shell有bug,使用bash,参见:https://github.com/docker-library/tomcat/issues/77
    \
	find ./bin/ -name '*.sh' -exec sed -ri 's|^#!/bin/sh$|#!/usr/bin/env bash|' '{}' +; \
    # 设置目录权限 bug参见https://github.com/docker-library/tomcat/issues/35
    \
# fix permissions (especially for running as non-root)
# https://github.com/docker-library/tomcat/issues/35
    chmod -R +rX .; \
    chmod 777 logs temp work; \
    chmod +x /wait; \
	# 创建Root目录
	\
	mkdir -p webapps/ROOT; \
	#删除默认的配置文件
	\
	rm -rf conf/server.xml; \
	#创建snsoft的日志目录
	mkdir -p /var/log/snlog; \
	#创建snsoft的标准配置目录
	mkdir -p /etc/snconfig; \
    # 测试,输出Tomcat版本 \
    \
# smoke test
    catalina.sh version;


#替换默认配置文件,替换默认的ROOT全部内容,只放一个静态的index.html页面
ADD server.xml $CATALINA_HOME/conf/server.xml
ADD index.html $CATALINA_HOME/webapps/ROOT/index.html

#验证安装是否正确,正式Build的时候,需要注释掉该功能. ----王立鹏
#RUN set -eux; \
#	nativeLines="$(catalina.sh configtest 2>&1)"; \
#	nativeLines="$(echo "$nativeLines" | grep 'Apache Tomcat Native')"; \
#	nativeLines="$(echo "$nativeLines" | sort -u)"; \
#	if ! echo "$nativeLines" | grep -E 'INFO: Loaded( APR based)? Apache Tomcat Native library' >&2; then \
#		echo >&2 "$nativeLines"; \
#		exit 1; \
#	fi

VOLUME ["/etc/snconfig","/data","/var/log/snlog","/usr/local/tomcat/logs"]

EXPOSE 8000 8080 8444 20880 20881 20882 20883 20884 20886

CMD ["catalina.sh", "run"]