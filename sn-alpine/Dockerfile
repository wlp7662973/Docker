FROM alpine:3.10.1

MAINTAINER 王立鹏 强化alpine <wanglipeng@snsoft.com.cn>
# Build:docker build -t wlp7662973/sn-alpine:3 ./sn-alpine
# 测试:docker run -it --rm --name test_os wlp7662973/sn-alpine:3
# push:docker push wlp7662973/sn-alpine:3

## glibc的下载地址
ENV GLIBC_TGZ_URLS \
# 公司内下载地址,上传开源社区注释掉该地址
#    https://n.esnsoft.cn/alpine-pkg-glibc/glibc-2.29-r0.apk
# sgerrand下载地址
    https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.29-r0/glibc-2.29-r0.apk

#安装必要的工具
RUN set -eux; \
    # 设置安装源地址为阿里源
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories;\
    # 安装必要的软件包
    apk add --no-cache tzdata curl ca-certificates openssl libuuid bash tar zip unzip bzip2 wget busybox-extras sysstat tcpdump ethtool nload iftop nethogs bmon vnstat bwm-ng darkstat nmap strace; \
	# 设置时区为Shanghai,设置完时区,删除tzdata
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
    # 安装glibc,删除安装包
    apk add --allow-untrusted /tmp/glibc-2.29-r0.apk; \
	rm -rf /tmp/glibc-2.29-r0.apk

CMD ["/bin/bash"]