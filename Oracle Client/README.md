# Oracle Client 11.2
Crear las variables de entorno
```
ENV ORACLE_HOME=/opt/oracle/client/11.2
ENV LD_LIBRARY_PATH="$ORACLE_HOME:$LD_LIBRARY_PATH"
ENV PATH="$ORACLE_HOME:$PATH" 
```
Luego descomprimir los zip en la ruta del ORACLE_HOME
```
/opt/oracle/client/11.2
```
Crear enlaces simbólicos para el funcionamiento del SQLPLUS
```
ln -s $ORACLE_HOME/libclntsh.so.11.1 $ORACLE_HOME/libclntsh.so &&\
ln -s $ORACLE_HOME/libocci.so.11.1 $ORACLE_HOME/libocci.so
```
