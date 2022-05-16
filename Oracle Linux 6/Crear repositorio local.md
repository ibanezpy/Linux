# Documentación para la creación de repositorio local

Crearemos un repositorio espejo para poder actualizar los páquetes de Oracle Linux 6.

URL = repo.datapar.com

## A continuación debemos de realizar las siguientes configuraciónes

### 1. Creamos el archivo /etc/yum.repos.d/OEL.repo y agregamos las siguientes lineas

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

Creamos el archivo /OEL_repo/scripts/repo_sync.sh
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

Modificar la linea SELINUX=enforcing por SELINUX=disabled y guardamos.

Para no reiniciar el sistema podemos ejecuta el comando para desactivar el Selinux
```
setenforce 0
```


### 2. Agregamos la reglas al iptables en el archivo -- vim /etc/sysconfig/iptables
```
#Permitimos que solo acepte peticiones de nuestra red para el puerto SSH y bloquee el resto
-A INPUT -p tcp -s x.x.x.x/x --dport 22 -j ACCEPT
#Habilitamos para todo el puerto 80
-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
```

1. Instalamos Apache y 
