FROM wlp7662973/sn-graalvm:8.302.212

MAINTAINER 王立鹏 snapp <wanglipeng@snsoft.com.cn>
#Build:docker build -t wlp7662973/sn-app-cm:1.0.0 ./sn-app-cm
#Test:docker run -it --rm -p 8080:8080 --name test_cm wlp7662973/sn-app-cm:1.0.0
#Test:docker exec -it test_cm /bin/bash
#push:docker push wlp7662973/sn-app-cm:1.0.0

#将war包放在当前目录中, 执行build
ADD cm.jar /srv

WORKDIR /srv

CMD ["/srv/runjar.sh"]