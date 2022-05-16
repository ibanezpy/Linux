# Documentación para la creación de repositorio local

Crearemos un repositorio espejo para poder actualizar los páquetes de Oracle Linux 6.

URL = repo.datapar.com

## A continuación debemos de realizar las siguientes configuraciónes

### 1. Creamos el archivo `vim /etc/yum.repos.d/OEL.repo` y agregamos las siguientes lineas

```
[oraclerpms]
name=OracleEnterpriseLinux
baseurl=http://yum.oracle.com/repo/OracleLinux/OL6/latest/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
enabled=1
```
### 2. Instalar el paquete yum-utils y createrepo
```
yum install yum-utils createrepo
```

### 3. Agregamos la carpeta donde vamos a descargar los repositorios
```
mkdir -p /OEL_repo/OEL6/latest
mkdir -p /OEL_repo/logs
mkdir -p /OEL_repo/scripts
```

### 4. Descargamos los rpms en la carpeta 
```
/usr/bin/reposync –newest-only –repoid=oraclerpms -p /OEL_repo/OEL6/latest
```

### 5. Luego de la descarga creamos los repositorios
```
/usr/bin/createrepo /OEL_repo/OEL6/latest/oraclerpms/getPackage/
```

### 6. Creamos un scripts para buscar actualizaciones de los repositorios de forma diaria.

Creamos el archivo `vim /OEL_repo/scripts/repo_sync.sh`
```
#!/bin/bash

LOG_FILE=/OEL_repo/logs/repo_sync_$(date +%Y.%m.%d).log

# Delete old logs
find /OEL_repo/logs/repo_sync* -mtime +5 -delete; >> $LOG_FILE 2>&1

#Clean cache
yum clean all

# Sync repositories
/usr/bin/reposync /usr/bin/reposync –newest-only –repoid=oraclerpms -p /OEL_repo/OEL6/latest >> $LOG_FILE 2>&1

/usr/bin/createrepo /OEL_repo/OEL6/latest/oraclerpms/getPackage/ >> $LOG_FILE 2>&1
```

Agregamos los permisos de ejecución
```
chmod u+x /OEL_repo/scripts/repo_sync.sh
```

Editamos el crontab para ejecución diaria del script creado en vim /etc/crontab
```
0 1 * * * /OEL_repo/scripts/repo_sync.sh > /dev/null 2>&1
```

## Ahora vamos a instalar Apache HTTP server para poder publicar los repositorios
### 1. Desactivamos selinux para eviatar problemas con el Apache

Editar el archivo --- vim /etc/selinux/config.

Modificar la linea `SELINUX=enforcing` por `SELINUX=disabled` y guardamos.

Para no reiniciar el sistema podemos ejecuta el siguente comando para desactivar el Selinux
```
setenforce 0
```


### 2. Agregamos la reglas al iptables en el archivo `vim /etc/sysconfig/iptables`
```
#Permitimos que solo acepte peticiones de nuestra red para el puerto SSH y bloquee el resto
-A INPUT -p tcp -s x.x.x.x/x --dport 22 -j ACCEPT
#Habilitamos para todo el puerto 80
-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
```

### 3. Instalamos Apache HTTP Server

Instalción y configuración de Apache HTTP Server.

``` 
yum install httpd
service httpd start
chkconfig httpd on
```

Creando los directorios y hacemos un link a dicha carpeta.
```
mkdir -p /var/www/html/OEL_repo/OEL6/latest
ln -s /OEL_repo/OEL6/latest/oraclerpms/getPackage/ /var/www/html/OEL_repo/OEL6/latest/x86_64
cd /var/www/html
wget http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
```

¡¡¡Listo, ya temenos configurado el servidor!!!


# Configuración del repositorio lado cliente

### 1. Agregamos el repositorio que hemos creado

Para verificar que nuestro repositorio funciona realizamos lo siguiente:
* Mover `/etc/yum.repos.d/public-yum-ol6.repo` al directorio `HOME`.
* Agregamos el repositorio `vim /etc/yum.repos.d/local-ol6.repo` y agremamos las siguentes líneas:
```
[oraclerpms]
name=Oracle Linux $releasever Latest ($basearch)
baseurl=http://repo.datapar.com/OEL_repo/OEL6/latest/$basearch/
gpgkey=http://repo.datapar.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

* Ejecutamos el comando `yum -v repolist` para verificar el funcionamiento del repositorio.

```
[root@ibanez ~]# yum -v repolist
Not loading "rhnplugin" plugin, as it is disabled
Loading "ulninfo" plugin
Loading "refresh-packagekit" plugin
Loading "security" plugin
Config time: 0.020
Yum Version: 3.2.29
Setting up Package Sacks
pkgsack time: 0.033
Repo-id      : ol6_UEKR4
Repo-name    : Latest Unbreakable Enterprise Kernel Release 4 for Oracle Linux 6Server (x86_64)
Repo-updated : Wed Mar 17 11:23:49 2021
Repo-pkgs    : 151
Repo-size    : 2.0 G
Repo-baseurl : https://yum.oracle.com/repo/OracleLinux/OL6/UEKR4/x86_64/
Repo-expire  : 21.600 second(s) (last: Mon May 16 09:44:58 2022)
Repo-excluded: 40

Repo-id      : ol6_latest
Repo-name    : Oracle Linux 6Server Latest (x86_64)
Repo-updated : Thu Apr 15 12:38:08 2021
Repo-pkgs    : 10.131
Repo-size    : 28 G
Repo-baseurl : https://yum.oracle.com/repo/OracleLinux/OL6/latest/x86_64/
Repo-expire  : 21.600 second(s) (last: Mon May 16 09:45:00 2022)
Repo-excluded: 2.801

Repo-id      : oraclerpms
Repo-name    : Oracle Linux 6Server Latest (x86_64)
Repo-revision: 1652701519
Repo-updated : Mon May 16 08:05:06 2022
Repo-pkgs    : 10.131
Repo-size    : 28 G
Repo-baseurl : http://repo.datapar.com/OEL_repo/OEL6/latest/x86_64/
Repo-expire  : 21.600 second(s) (last: Mon May 16 09:40:38 2022)

repolist: 20.413

```