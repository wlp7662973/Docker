#基于OracleJDK的版本:
FROM wlp7662973/sn-oralcejdk8:3.11.6.202

MAINTAINER 王立鹏 TOMCAT9.0.x <wanglipeng@snsoft.com.cn>
#Build:docker build -t wlp7662973/sn-tomcat90:35.2.1 ./sn-tomcat90
#Test:docker run -it --rm -p 8088:8080 --name test_tomcat90 wlp7662973/sn-tomcat90:35.2.1
#Test:docker exec -it test_tomcat90 /bin/bash
#push:docker push wlp7662973/sn-tomcat90:35.2.1

ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH:
RUN mkdir -p "$CATALINA_HOME"
WORKDIR $CATALINA_HOME

# let "Tomcat Native" live somewhere isolated
ENV TOMCAT_NATIVE_LIBDIR $CATALINA_HOME/native-jni-lib
ENV LD_LIBRARY_PATH ${LD_LIBRARY_PATH:+$LD_LIBRARY_PATH:}$TOMCAT_NATIVE_LIBDIR

# see https://www.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/KEYS
# see also "update.sh" (https://github.com/docker-library/tomcat/blob/master/update.sh)
ENV GPG_KEYS 05AB33110949707C93A279E3D3EFE6B686867BA6 07E48665A34DCAFAE522E5E6266191C37C037D42 47309207D818FFD8DCD3F83F1931D684307A10A5 541FBE7D8F78B25E055DDEE13C370389288584E7 61B832AC2F1C5A90F0F9B00A1C506407564C17A3 79F7026C690BAA50B92CD8B66A3AD3F4F22C4FED 9BA44C2621385CB966EBA586F72C284D731FABEE A27677289986DB50844682F8ACB77FC2E86E29AC A9C5DF4D22E99998D9875A5110C01C5A2F6059E7 DCFD35E0BF8CA7344752DE8B6FB21E8933C60243 F3A04C595DB5B6A5F1ECA43E3B7BBB100D811BBE F7DA48BB64BCB84ECBA7EE6935CD23C10D498E23

ENV TOMCAT_MAJOR 9
ENV TOMCAT_VERSION 9.0.35
ENV TOMCAT_SHA512 0db27185d9fc3174f2c670f814df3dda8a008b89d1a38a5d96cbbe119767ebfb1cf0bce956b27954aee9be19c4a7b91f2579d967932207976322033a86075f98

ENV TOMCAT_TGZ_URLS \
# 公司内下载地址,上传开源社区注释掉该行地址
#    https://n.esnsoft.cn/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/apache-tomcat-$TOMCAT_VERSION.tar.gz \
# 清华大学镜像
    http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz \
# 中国国家网信办镜像:
    https://mirrors.cnnic.cn/apache/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz \
# Apache官方地址
	https://www.apache.org/dyn/closer.cgi?action=download&filename=tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz \
# 部分国外镜像服务器地址
    https://www-us.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz \
	https://www.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz \
	https://archive.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz

ENV TOMCAT_ASC_URLS \
# 公司内下载地址,上传开源社区注释掉该行地址
#    https://n.esnsoft.cn/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc \
# Apache官方地址
	https://www.apache.org/dyn/closer.cgi?action=download&filename=tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc \
# 部分国外镜像服务器地址
	https://www-us.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc \
	https://www.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc \
	https://archive.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc

# 使用公版镜像,则需要设置安装源. 使用sn-alpine镜像,则删掉下面一行即可
#RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

RUN set -eux; \
    # 安装必要的依赖,主要是OpenJDK的需要,sn-oralcejdk8镜像已经安装过了.
	\
	apk add --no-cache --virtual .fetch-deps \
		gnupg \
		\
		ca-certificates \
		openssl \
	; \
	#导入GPG_KEY,用于验证Tomcat包的签名.
	\
	export GNUPGHOME="$(mktemp -d)"; \
	for key in $GPG_KEYS; do \
		gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
	done; \
	#从地地址列表中下载Tomcat的压缩包.
	\
	success=; \
	for url in $TOMCAT_TGZ_URLS; do \
		if wget -O tomcat.tar.gz "$url"; then \
			success=1; \
			break; \
		fi; \
	done; \
	[ -n "$success" ]; \
	#比较Tomcat下载包的SHA512摘要,以确保下载完整并且版本一致
	\
	echo "$TOMCAT_SHA512 *tomcat.tar.gz" | sha512sum -c -; \
	#下载相应Tomcat版本的asc密钥文件,并验证
	\
	success=; \
	for url in $TOMCAT_ASC_URLS; do \
		if wget -O tomcat.tar.gz.asc "$url"; then \
			success=1; \
			break; \
		fi; \
	done; \
	[ -n "$success" ]; \
	#验证tar.gz包的签名信息
	\
	gpg --batch --verify tomcat.tar.gz.asc tomcat.tar.gz; \
	#解压缩
	tar -xvf tomcat.tar.gz --strip-components=1; \
	#删除Windows系统才会用到的bat文件,删除压缩包,删除验签密钥的临时文件夹
	rm bin/*.bat; \
	rm tomcat.tar.gz*; \
	command -v gpgconf && gpgconf --kill all || :; \
	rm -rf "$GNUPGHOME"; \
	#创建Tomcat的Native(c语言代码)程序的解压临时目录,并解压,安装编译Native相应的依赖
	\
	nativeBuildDir="$(mktemp -d)"; \
	tar -xvf bin/tomcat-native.tar.gz -C "$nativeBuildDir" --strip-components=1; \
	apk add --no-cache --virtual .native-build-deps \
		apr-dev \
		coreutils \
		dpkg-dev dpkg \
		gcc \
		libc-dev \
		make \
		openssl-dev \
	; \
	#配置并编译/安装Native程序
	( \
		export CATALINA_HOME="$PWD"; \
		cd "$nativeBuildDir/native"; \
		gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; \
		./configure \
		    --build="$gnuArch" \
			--libdir="$TOMCAT_NATIVE_LIBDIR" \
			--prefix="$CATALINA_HOME" \
			--with-apr="$(which apr-1-config)" \
			--with-java-home="$(docker-java-home)" \
			--with-ssl=yes; \
		make -j "$(nproc)"; \
		make install; \
	); \
	#删除Native程序的源码,删除Native程序的压缩包
	rm -rf "$nativeBuildDir"; \
	rm bin/tomcat-native.tar.gz; \
	#搜索运行Native程序需要的依赖库,将其保留.
	\
	runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive "$TOMCAT_NATIVE_LIBDIR" \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add --virtual .tomcat-native-rundeps $runDeps; \
	#删除下载程序需要的依赖,删除编译Native程序需要的依赖
	apk del .fetch-deps .native-build-deps; \
	\
	# 移除临时变量,因sh的Shell有bug,使用bash,参见:https://github.com/docker-library/tomcat/issues/77
	apk add --no-cache bash; \
	find ./bin/ -name '*.sh' -exec sed -ri 's|^#!/bin/sh$|#!/usr/bin/env bash|' '{}' +; \
	# 修正权限问题 bug参见https://github.com/docker-library/tomcat/issues/35
	\
	chmod -R +rX .; \
	chmod 777 logs work; \
	#删除Tomcat默认的应用和管理应用,以防被人通过manager和host-manager植入或删除文件.
	\
	rm -rf webapps/manager; \
	rm -rf webapps/examples; \
	rm -rf webapps/host-manager; \
	rm -rf webapps/docs; \
	rm -rf webapps/ROOT/*; \
	#删除默认的配置文件
	\
	rm -rf conf/server.xml; \
	#创建snsoft的日志目录
	mkdir -p /var/log/snlog; \
	#创建snsoft的标准配置目录
	mkdir -p /etc/snconfig;


#替换默认配置文件,替换默认的ROOT全部内容,只放一个静态的index.html页面
ADD server.xml $CATALINA_HOME/conf/server.xml
ADD index.html $CATALINA_HOME/webapps/ROOT/index.html

# 验证安装是否正确,正式Build的时候,需要注释掉该功能. ----王立鹏
#RUN set -e \
#	&& nativeLines="$(catalina.sh configtest 2>&1)" \
#	&& nativeLines="$(echo "$nativeLines" | grep 'Apache Tomcat Native')" \
#	&& nativeLines="$(echo "$nativeLines" | sort -u)" \
#	&& if ! echo "$nativeLines" | grep 'INFO: Loaded APR based Apache Tomcat Native library' >&2; then \
#		echo >&2 "$nativeLines"; \
#		exit 1; \
#	fi

VOLUME ["/etc/snconfig","/var/log/snlog","/usr/local/tomcat/logs"]

EXPOSE 8080 8444 20880 20881 20882 20883

CMD ["catalina.sh", "run"]