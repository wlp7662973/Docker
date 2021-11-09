#
# NOTE: THIS DOCKERFILE IS GENERATED VIA "update.sh"
#
# PLEASE DO NOT EDIT IT DIRECTLY.
#
FROM mysql:8.0.27

MAINTAINER 王立鹏 mysql8扩展,支持创建俩数据库 <wanglipeng@snsoft.com.cn>
# Build:docker build -t wlp7662973/sn-mysql8:8.0.27 ./sn-mysql8
# 测试:docker run -it --rm --name sn-mysql wlp7662973/sn-mysql8:8.0.27
# 测试:docker run -it --rm --name sn-mysql  -e MYSQL_ROOT_PASSWORD=snsoft -e MYSQL_DATABASE=em -e MYSQL_DATABASE_1=em1 wlp7662973/sn-mysql8:8.0.27
# push:docker push wlp7662973/sn-mysql8:8.0.27



# Config files
#COPY config/ /etc/mysql/
COPY docker-entrypoint.sh /usr/local/bin/
RUN rm -rf /entrypoint.sh; \
    ln -s usr/local/bin/docker-entrypoint.sh /entrypoint.sh # backwards compat
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 3306 33060
CMD ["mysqld"]
