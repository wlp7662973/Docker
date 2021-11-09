FROM wlp7662973/sn-graal-tomcat90:54

MAINTAINER 王立鹏 snapp <wanglipeng@snsoft.com.cn>
#Build:docker build -t wlp7662973/sn-app-cloud:1.0.0 ./sn-app-cloud
#Test:docker run -it --rm -p 8080:8080 --name test_cloud wlp7662973/sn-app-cloud:1.0.0
#Test:docker exec -it test_cloud /bin/bash
#push:docker push wlp7662973/sn-app-cloud:1.0.0

RUN set -eux; \
    # 不要ROOT目录
    rm -rf webapps/ROOT;

#将war包放在当前目录中, 执行build
ADD cloud.war $CATALINA_HOME/webapps/

EXPOSE 8080 8444 20880 20881 20882 20883 20884 20886

CMD ["catalina.sh", "run"]