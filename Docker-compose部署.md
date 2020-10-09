# Docker-compose部署

```yaml
version: '3.3'
services:
    slcwebjar:
        build:
          context: /usr/slc/docker-compose-slc-java-dir/java-jar-deploy
          dockerfile: Dockerfile
        restart: always
        image: slc_web_from_compose
        container_name: slc-web
        depends_on:
          - mysql
        ports:
        - "80:8080"
    mysql:
        restart: always
        image: mysql
        container_name: slc_mysql
        ports:
        - 3308:3306
        environment:
          MYSQL_ROOT_PASSWORD: root
        volumes:
        - /usr/slc/docker-compose-slc-java-dir/mysql-slc:/var/lib/mysql
        #- /usr/slc/docker-compose-slc-java-dir/mysql-init:/docker-entrypoint-initdb.d/
```



Dockerfile内容

```yaml
FROM mir/java8-min
MAINTAINER slc <1848424208@qq.com>
RUN echo "hello"
RUN ls -l
EXPOSE 8080
WORKDIR /app
COPY back.jar back.jar
ENTRYPOINT ["java","-jar","back.jar"]
```

