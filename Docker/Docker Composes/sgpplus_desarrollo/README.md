# sgpplus_desarrollo
```
version: '2.2'
services:
  tomcat:
    hostname: crguerrero
    container_name: tomcat
    image: blacknoob20/tomcat-alpine-cli11g
    cpuset: '2'
    mem_limit: 256m
    volumes:
      - $PWD/:/var/www/html/
      - $PWD/../tmp/sgpplus/:/tmp/sgpplus/
      - $PWD/../tomcat9/usr/local/tomcat/webapps/:/usr/local/tomcat/webapps/
      - $PWD/../tomcat9/usr/local/tomcat/logs/:/usr/local/tomcat/logs/
      - $PWD/../tomcat9/usr/local/tomcat/conf/context.xml:/usr/local/tomcat/conf/context.xml
      - $PWD/../tomcat9/usr/local/tomcat/conf/tomcat-users.xml:/usr/local/tomcat/conf/tomcat-users.xml
    ports:
      - '8080:8080'
    networks:
      - net
  php:
    hostname: crguerrero
    container_name: fpm
    image: blacknoob20/php5.6-fpm-alpine-oci8
    cpuset: '0'
    mem_limit: 64m
    volumes:
      - $PWD/:/var/www/html/
      - $PWD/../tmp/sgpplus/:/tmp/sgpplus/
      - $PWD/../php-5.6-fpm/usr/local/etc/php/:/usr/local/etc/php/
      - $PWD/../php-5.6-fpm/opt/oracle/client/11.2/network/admin/:/opt/oracle/client/11.2/network/admin/
      - $PWD/../php-5.6-fpm/var/log/sgpplus/:/var/log/sgpplus/
    networks:
      - net
  sgp:
    depends_on:
      - php
    container_name: plus
    image: webdevops/apache:alpine-3
    cpuset: '1'
    mem_limit: 32m
    environment:
      - WEB_PHP_SOCKET=php:9000
      - WEB_DOCUMENT_ROOT=/var/www/html/
    volumes:
      - $PWD/:/var/www/html
      - $PWD/../apache2/etc/apache2/:/etc/apache2/
      - $PWD/../apache2/var/log/apache2/:/var/log/apache2/
      #- $PWD/../apache2/opt/docker/etc/httpd/:/opt/docker/etc/httpd/
    ports:
      - '80:80'
      - '443:443'
    networks:
      - net
  wildfly:
    container_name: wildfly-20
    image: jboss/wildfly:20.0.0.Final
    cpuset: '3'
    mem_limit: 512m
    volumes:
      - $PWD/../jboss/wildfly/:/opt/jboss/wildfly/
      - $PWD/../jboss/deployments/:/opt/jboss/wildfly/standalone/deployments/
      - $PWD/../jboss/log/:/opt/jboss/wildfly/standalone/log/
    ports:
      - '8081:8080'
      - '9990:9990'
    environment:
      - TZ=America/Guayaquil
    command: bash -c "/opt/jboss/wildfly/bin/standalone.sh -b 0.0.0.0 -bmanagement 0.0.0.0 "
    networks:
      - net
  pgsql:
    container_name: postgres-9
    image: postgres:alpine
    cpuset: '1'
    mem_limit: 32m
    environment:
      - TZ=America/Guayaquil
      - POSTGRES_USER=firmadigital
      - POSTGRES_PASSWORD=firmadigital
      - POSTGRES_DATABASE=firmadigital
      - PGDATA=/var/lib/postgresql/data/pgdata
      - API_KEY=e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
      - API_KEY_HASH=cd372fb85148700fa88095e3492d3f9f5beb43e555e5ff26d95f5a6adc36f8e6
    volumes:
      - $PWD/../pgsql/:/var/lib/postgresql/data
    ports:
      - '5432:5432'
    networks:
      - net
networks:
  net:

```
