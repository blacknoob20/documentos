# Dockerfile-php7.4-oci8
```
FROM php:7.4-fpm-alpine

#Instalación del cliente Oracle 11g
RUN mkdir -p /opt/oracle/client/11.2/network/admin

COPY ./instantclient* /opt/oracle/client/

LABEL ora_install="Instalación InstantClient 11g"

ENV ORACLE_HOME=/opt/oracle/client/11.2
ENV LD_LIBRARY_PATH="$ORACLE_HOME:$LD_LIBRARY_PATH"
ENV PATH="$ORACLE_HOME:$PATH" 

RUN \
 unzip /opt/oracle/client/instantclient-basic-linux.x64-11.2.0.4.0.zip -d /opt/oracle/client &&\
 mv /opt/oracle/client/instantclient_11_2/* $ORACLE_HOME/ &&\
 rm -R -f /opt/oracle/client/instantclient_11_2/ &&\
 rm -f /opt/oracle/client/instantclient-basic-linux.x64-11.2.0.4.0.zip &&\
 unzip /opt/oracle/client/instantclient-sqlplus-linux.x64-11.2.0.4.0.zip -d /opt/oracle/client &&\
 mv /opt/oracle/client/instantclient_11_2/* $ORACLE_HOME/ &&\
 rm -R -f /opt/oracle/client/instantclient_11_2/ &&\
 rm -f /opt/oracle/client/instantclient-sqlplus-linux.x64-11.2.0.4.0.zip &&\
 unzip /opt/oracle/client/instantclient-sdk-linux.x64-11.2.0.4.0.zip -d /opt/oracle/client &&\
 mv /opt/oracle/client/instantclient_11_2/sdk/ $ORACLE_HOME/ &&\
 rm -R -f /opt/oracle/client/instantclient_11_2/ &&\
 rm -f /opt/oracle/client/instantclient-sdk-linux.x64-11.2.0.4.0.zip &&\
 ln -s $ORACLE_HOME/libclntsh.so.11.1 $ORACLE_HOME/libclntsh.so &&\
 ln -s $ORACLE_HOME/libocci.so.11.1 $ORACLE_HOME/libocci.so

#Instalar glibc, libaio, libnsl para que funcione el InstantClient 11g
LABEL glibc="Instalación glibc libaio libnsl zip"

RUN apk update &&\
 apk add --no-cache libaio libnsl libzip-dev zip &&\
 wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub &&\
 wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.33-r0/glibc-2.33-r0.apk &&\
 apk add glibc-2.33-r0.apk &&\
 ln -s /usr/lib/libaio.so.1.0.1 /opt/oracle/client/11.2/libaio.so.1 &&\
 ln -s /usr/lib/libnsl.so.2.0.1 /opt/oracle/client/11.2/libnsl.so.1

#Instalar la libreria OCI8
LABEL oci8="Instalación PHP 7.4 ext OCI8"
RUN \
 docker-php-ext-configure oci8 --with-oci8=instantclient,/opt/oracle/client/11.2 &&\
 docker-php-ext-install -j$(nproc) oci8 &&\
 docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/opt/oracle/client/11.2 &&\
 docker-php-ext-install pdo_oci zip &&\
 docker-php-ext-enable oci8 &&\
 docker-php-ext-enable zip


```
