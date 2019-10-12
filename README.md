# Docker
Docker构建仓库

Docker Hub地址:`https://github.com/wlp7662973/Docke`

## sn-alpine
sn-alpine 目前最新版本:`wlp7662973/sn-alpine:3`

sn-alpine为基础操作系统,安装了必要的工具. 为安装OracleJDK/Tomcat的基础系统

测试:
`docker run -it --rm --name test_os wlp7662973/sn-alpine:3`

## alpine-sshd
alpine-sshd目前最新版本:`wlp7662973/alpine-sshd:3`

alpine-sshd在alpine的基础上,安装了sshd服务,一般用于测试使用.

默认端口:22

默认roor密码:4rfv5tgb3edc

测试:

`docker run -it --rm -p 2222:22 --name test_sshd wlp7662973/alpine-sshd:3`
运行:

`docker run -itd -p 2222:22 --name alpine_sshd wlp7662973/alpine-sshd:3`

`ocker run -itd -p 2222:22 --name alpine_sshd --restart=always wlp7662973/alpine-sshd:3`

仅用于测试使用,

不要基于该镜像构建任何其他镜像.

## sn-openjdk8
sn-openjdk8目前最新版本:`wlp7662973/sn-openjdk8:1`

基于`wlp7662973/sn-alpine`构建

测试:

`docker run -it --rm --name testopenjdk wlp7662973/sn-openjdk8:1`

## sn-oracle_jdk8
sn-openjdk8目前有两个版本:

`wlp7662973/sn-oralcejdk8:2` 使用Oracle JDK8 u181版本创建

`wlp7662973/sn-oralcejdk8:3` 使用Oracle JDK8 u192版本创建,Oracle JDK8 u192为免费可以使用的最后的版本

基于`wlp7662973/sn-alpine`构建

测试:

`docker run -it --rm --name testjdk wlp7662973/sn-oralcejdk8:3`

## sn-tomcat85
sn-tomcat85目前有两个版本:

`wlp7662973/sn-tomcat85:43.0.1`使用基于`wlp7662973/sn-oralcejdk8:2`,即:Oralce JDK 8u181版本

`wlp7662973/sn-tomcat85:43.1.1` 使用基于`wlp7662973/sn-oralcejdk8:3`,即:Oralce JDK 8u192版本

使用`Tomcat 8.5.43`版本创建

默认端口:8080

测试:

`docker run -it --rm -p 8088:8080 --name test_tomcat8 wlp7662973/sn-tomcat85:43.1.1`
