# Docker
Docker构建仓库

Docker Hub地址:`https://hub.docker.com/u/wlp7662973`

## sn-alpine
sn-alpine 目前最新版本:`wlp7662973/sn-alpine:3.11.6`<br/>
sn-alpine为基础操作系统,安装了必要的工具. 为安装OracleJDK/Tomcat的基础系统<br/>
2020年05月12日 增加了多语言环境的支持,支持了en_US.UTF-8字符集<br/>
测试:<br/>
`docker run -it --rm --name test_os wlp7662973/sn-alpine:3.11.6`

## alpine-sshd
alpine-sshd目前最新版本:`wlp7662973/alpine-sshd:3`<br/>
alpine-sshd在alpine的基础上,安装了sshd服务,一般用于测试使用.<br/>
默认端口:22<br/>
默认roor密码:4rfv5tgb3edc<br/>
测试:<br/>
`docker run -it --rm -p 2222:22 --name test_sshd wlp7662973/alpine-sshd:3`<br/>
运行:<br/>
`docker run -itd -p 2222:22 --name alpine_sshd wlp7662973/alpine-sshd:3`<br/>
`ocker run -itd -p 2222:22 --name alpine_sshd --restart=always wlp7662973/alpine-sshd:3`<br/>
仅用于测试使用,不要基于该镜像构建任何其他镜像.

## sn-openjdk8
sn-openjdk8目前最新版本:`wlp7662973/sn-openjdk8:1`<br/>
基于`wlp7662973/sn-alpine`构建<br/>
测试:<br/>
`docker run -it --rm --name testopenjdk wlp7662973/sn-openjdk8:1`

# Oracle JDK 8
活跃版本:<br/>
JDK8 Update202:`wlp7662973/sn-oralcejdk8:3.11.6.202` `wlp7662973/sn-oralcejdk8:3.11.6.202-ext`<br/>
JDK8 Update192:`wlp7662973/sn-oralcejdk8:3.11.6` `wlp7662973/sn-oralcejdk8:3.11.6-ext`<br/>

## sn-oracle_jdk8
### `sn-oralcejdk8:3`的最新版本:<br/>
`wlp7662973/sn-oralcejdk8:3.11.6.202` 使用Oracle JDK8 u202版本创建 基于最新的sn-alpine:3.11.6构建,支持了en_US.UTF-8字符集<br/>
`wlp7662973/sn-oralcejdk8:3.11.6.202-ext`使用基于`wlp7662973/sn-oralcejdk8:3.11.6.202`<br/>
安装了GraphicsMagick和Tesseract-OCR两个软件包<br/>

#### sn-openjdk8目前有三个JDK版本:<br/>
`wlp7662973/sn-oralcejdk8:2` 使用Oracle JDK8 u181版本创建;系统为alpine3.10<br/>
`wlp7662973/sn-oralcejdk8:3` 使用Oracle JDK8 u192版本创建;系统为alpine3.10<br/>
`wlp7662973/sn-oralcejdk8:3.11.6.202` 使用Oracle JDK8 u202版本创建,Oracle JDK8 u202为免费可以使用的最后的版本;系统为alpine3.11.6<br/>
基于`wlp7662973/sn-alpine`构建<br/>

#### 测试:<br/>
`docker run -it --rm --name testjdk wlp7662973/sn-oralcejdk8:3.11.6.202`<br/>

# tomcat 8.5
最新活跃版本:<br/>
`wlp7662973/sn-tomcat85:55.2.1` `wlp7662973/sn-tomcat85:55.2.1-ext`<br/>

## sn-tomcat85
### `sn-tomcat85:xx.2.x`的最新版本:<br/>
使用`Tomcat 8.5.55`版本创建<br/>
`wlp7662973/sn-tomcat85:55.2.1`使用基于`wlp7662973/sn-oralcejdk8:3.11.6.202`.<br/>
使用Oralce JDK 8u202版本,支持了en_US.UTF-8字符集<br/>
`wlp7662973/sn-tomcat85:55.2.1-ext`使用基于`wlp7662973/sn-tomcat85:55.2.1`.<br/>
安装了GraphicsMagick和Tesseract-OCR两个软件包<br/>

###  `sn-tomcat85:xx.1.x`的最新版本:<br/>
使用`Tomcat 8.5.55`版本创建<br/>
`wlp7662973/sn-tomcat85:55.1.1`使用基于`wlp7662973/sn-oralcejdk8:3.11.6`.<br/>
使用Oralce JDK 8u192版本,支持了en_US.UTF-8字符集<br/>
`wlp7662973/sn-tomcat85:55.1.1-ext`使用基于`wlp7662973/sn-tomcat85:55.1.1`.<br/>
安装了GraphicsMagick和Tesseract-OCR两个软件包<br/>

### sn-tomcat85目前有两个基础版本:<br/>
使用`Tomcat 8.5.43`版本创建<br/>
`wlp7662973/sn-tomcat85:43.0.1`使用基于`wlp7662973/sn-oralcejdk8:2`,即:Oralce JDK 8u181版本<br/>
`wlp7662973/sn-tomcat85:43.1.1`使用基于`wlp7662973/sn-oralcejdk8:3`,即:Oralce JDK 8u192版本<br/>

默认端口:8080<br/>

#### 测试:<br/>
`docker run -it --rm -p 8088:8080 --name test_tomcat85 wlp7662973/sn-tomcat85:55.2.1`<br/>
`docker run -it --rm -p 8088:8080 --name test_tomcat85 wlp7662973/sn-tomcat85:55.2.1-ext`<br/>
`docker exec -it test_tomcat85 /bin/bash`<br/>
#### GraphicsMagick命令:<br/>
`gm version`<br/>
`gm convert -list formats`<br/>
`gm convert a.jpg -resize 100x100 b.png`<br/>


# tomcat 9.0
最新活跃版本:<br/>
`wlp7662973/sn-tomcat90:35.2.1` `wlp7662973/sn-tomcat90:35.2.1-ext`<br/>

## sn-tomcat90
### `sn-tomcat90:xx.2.x`的最新版本:<br/>
使用`Tomcat tomcat9.0.35`版本创建<br/>
`wlp7662973/sn-tomcat90:35.2.1`使用基于`wlp7662973/sn-oralcejdk8:3.11.6.202`.<br/>
使用Oralce JDK 8u202版本,支持了en_US.UTF-8字符集<br/>
`wlp7662973/sn-tomcat90:35.2.1-ext`使用基于`wlp7662973/sn-tomcat90:35.2.1`.<br/>
安装了GraphicsMagick和Tesseract-OCR两个软件包<br/>

默认端口:8080<br/>

#### 测试:<br/>
`docker run -it --rm -p 8088:8080 --name test_tomcat90 wlp7662973/sn-tomcat90:35.2.1`<br/>
`docker run -it --rm -p 8088:8080 --name test_tomcat90 wlp7662973/sn-tomcat90:35.2.1-ext`<br/>
`docker exec -it test_tomcat90 /bin/bash`<br/>
#### GraphicsMagick命令:<br/>
`gm version`<br/>
`gm convert -list formats`<br/>
`gm convert a.jpg -resize 100x100 b.png`<br/>



