FROM alpine:3.10.1

MAINTAINER 王立鹏 安装sshd服务 <wanglipeng@snsoft.com.cn>
# Build:docker build -t wlp7662973/alpine-sshd:3 ./alpine-sshd
# 测试:docker run -it --rm -p 2222:22 --name test_sshd wlp7662973/alpine-sshd:3
# 运行:docker run -itd -p 2222:22 --name alpine_sshd wlp7662973/alpine-sshd:3
#     :docker run -itd -p 2222:22 --name alpine_sshd --restart=always wlp7662973/alpine-sshd:3
# 连接:docker exec -it alpine_sshd /bin/bash

## glibc的下载地址
ENV GLIBC_TGZ_URLS \
# 公司内下载地址,上传开源社区注释掉该地址
#    https://n.esnsoft.cn/alpine-pkg-glibc/glibc-2.29-r0.apk
# sgerrand下载地址
    https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.29-r0/glibc-2.29-r0.apk

#安装必要的工具
RUN set -eux; \
    # 设置apk安装源地址为阿里源
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories;\
    # 安装必要的软件包
    apk add --no-cache openrc openssh ca-certificates curl wget bash openssl tar zip unzip tzdata; \
    #apk add --no-cache supervisor; \
	# 设置时区为Shanghai,设置完时区,删除tzdata包
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
	&& echo "Asia/Shanghai" > /etc/timezone \
	&& apk del tzdata; \
	# 下载sgerrand的公钥文件.
	wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub; \
    # 下载glibc:
    success=; \
    for url in $GLIBC_TGZ_URLS; do \
        if wget -O /tmp/glibc-2.29-r0.apk "$url"; then \
            success=1; \
            break; \
        fi; \
    done; \
    [ -n "$success" ]; \
    # 安装glibc,并删除安装包
    apk add --allow-untrusted /tmp/glibc-2.29-r0.apk; \
	rm -rf /tmp/glibc-2.29-r0.apk; \
	#创建.ssh目录,创建证书
	mkdir -p /root/.ssh && chmod 700 /root/.ssh/; \
	ssh-keygen -A; \
	#修改配置文件,开启使用Root用户通过ssh登录的权限
	sed -i "s/#PermitRootLogin.*/PermitRootLogin yes/g" /etc/ssh/sshd_config; \
	#sed -i 's/#Port 22/Port 22/g' /etc/ssh/sshd_config; \
	#sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config; \
	#修改系统root用户密码为4rfv5tgb3edc
	echo "root:4rfv5tgb3edc" | chpasswd

#预制一个运行脚本
#RUN { \
#		echo '#!/bin/sh'; \
#		echo 'set -e'; \
#		echo; \
#		echo '/usr/bin/supervisord -c /etc/supervisord.conf'; \
#		echo '/bin/bash'; \
#	} > /root/start.sh \
#	&& chmod +x /root/start.sh

#复制supervisor的配置文件目录
#COPY supervisor.d/ /etc/supervisor.d
#COPY supervisord.conf /etc/supervisord.conf

EXPOSE 22

#CMD ["/root/start.sh"]
CMD ["/usr/sbin/sshd", "-Dd"]
