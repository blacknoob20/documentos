# Dockerfile-tomcat-javabridge-cli11g
```
FROM tomcat:alpine

#Instalación del cliente Oracle 11g
RUN mkdir -p /opt/oracle/client/11.2/network/admin

COPY ./oracle_client/instantclient* /opt/oracle/client/

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
 rm -f /opt/oracle/client/instantclient-sdk-linux.x64-11.2.0.4.0.zip\
 /opt/oracle/client/instantclient-jdbc-linux.x64-11.2.0.4.0.zip &&\
 ln -s $ORACLE_HOME/libclntsh.so.11.1 $ORACLE_HOME/libclntsh.so &&\
 ln -s $ORACLE_HOME/libocci.so.11.1 $ORACLE_HOME/libocci.so

#Instalar glibc para que funcione el InstantClient 11g
LABEL glibc="Instalación glibc libaio libnsl libzip-dev zip"

RUN apk update &&\
 apk add --no-cache\
 libaio\
 libnsl\
 libzip-dev\
 zip\
 libxml2-dev\
 ttf-dejavu\
 msttcorefonts-installer &&\
 update-ms-fonts &&\
 fc-cache -f &&\
 wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub &&\
 wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.33-r0/glibc-2.33-r0.apk &&\
 apk add glibc-2.33-r0.apk &&\
 rm -f glibc-2.33-r0.apk &&\
 ln -s /lib/libkeyutils.so.1.8 /opt/oracle/client/11.2/libkeyutils.so.1 &&\
 ln -s /usr/lib/libkrb5support.so.0.1 /opt/oracle/client/11.2/libkrb5support.so.0 &&\
 ln -s /lib/libcom_err.so.2.1 /opt/oracle/client/11.2/libcom_err.so.2 &&\
 ln -s /usr/lib/libk5crypto.so.3.1 /opt/oracle/client/11.2/libk5crypto.so.3 &&\
 ln -s /usr/lib/libkrb5.so.3.3 /opt/oracle/client/11.2/libkrb5.so.3 &&\
 ln -s /usr/lib/libgssapi_krb5.so.2.2 /opt/oracle/client/11.2/libgssapi_krb5.so.2 &&\
 ln -s /lib/libc.musl-x86_64.so.1 /opt/oracle/client/11.2/libc.musl-x86_64.so.1 &&\
 ln -s /usr/lib/libtirpc.so.3.0.0 /opt/oracle/client/11.2/libtirpc.so.3 &&\
 ln -s /usr/lib/libintl.so.8.1.5 /opt/oracle/client/11.2/libintl.so.8 &&\
 ln -s /usr/lib/libaio.so.1.0.1 /opt/oracle/client/11.2/libaio.so.1 &&\
 ln -s /usr/glibc-compat/lib/libnsl-2.33.so /opt/oracle/client/11.2/libnsl.so.1
```
