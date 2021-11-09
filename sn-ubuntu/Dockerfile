FROM ubuntu:20.04
# 2021年10月26日 基于20.04构建

MAINTAINER 王立鹏 强化ubuntu <wanglipeng@snsoft.com.cn>
# Build:docker build -t wlp7662973/sn-ubuntu:20.04 ./sn-ubuntu
# 测试:docker run -it --rm --name test_os wlp7662973/sn-ubuntu:20.04 /bin/bash
# push:docker push wlp7662973/sn-ubuntu:20.04

ENV DEBIAN_FRONTEND noninteractive

#安装必要的工具
RUN set -eux; \
    # 设置安装源地址为阿里源
    sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list; \
    sed -i 's/security.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list; \
    apt-get update; \
    # 安装必要的软件包 utilities for keeping Debian and OpenJDK CA certificates in sync \
    apt-get install -y --no-install-recommends tzdata ca-certificates p11-kit curl wget iputils-ping zip unzip bzip2 sysstat tcpdump ethtool nload iftop nethogs vnstat bwm-ng darkstat strace graphicsmagick tesseract-ocr; \
	rm -rf /var/lib/apt/lists/* ; \
	# 设置时区为Shanghai,设置完时区,删除tzdata
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime; \
    # 将本机时区配置文件, 设置为Asia/Shanghai(很多第三方软件, 会读取这个文件)
    echo 'Asia/Shanghai' >/etc/timezone; \
	# 创建ll命令
	{ \
	    echo 'alias ll="ls -l --color=auto"'; \
	    echo 'alias ls="ls --color=auto"'; \
	} > /root/.bashrc \
	&& chmod +x /root/.bashrc

CMD ["/bin/bash"]