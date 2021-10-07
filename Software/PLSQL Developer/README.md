# Conf. PLSQL Developer en Linux con wine.
Pegar la carpeta donde está instalado el PLSQL Developer 8 en wine.
```
cd /home/crguerrero/.wine/drive_c/Program Files (x86)/
```
Crear un archivo de configuración de escritorio para el acceso desde  
el menu.
```
nano /usr/share/applications/plsqldeveloper.desktop
```
### plsqldeveloper.desktop
```
[Desktop Entry]
Name=PLSQLDeveloper
Comment=PL/SQL Developer 8
Categories=Development;IDE;
Exec=wine "C:\\Program Files (x86)\\PLSQL Developer\\8\\PLSQL Developer\\plsqldev.exe"
Icon=/home/crguerrero/.wine/drive_c/Program Files (x86)/PLSQL Developer/8/PLSQL Developer/plsqldeveloper.png
Terminal=false
Type=Application
StartupNotify=true
```
Quitar el Warn "NLS_LANG is not defined on the client" PL/SQL Developer  
Primero debemos abrir el regedit en wine
```
wine regedit
```
Segundo vamos a definir la variable de entorno NLS_LANG en la ruta
```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment
#Creamos el valor de cadena
NLS_LANG con valor SPANISH_SPAIN.WE8ISO8859P1
```
