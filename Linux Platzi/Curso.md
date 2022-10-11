# Comandos Linux Resumen
## **Gestión del árbol de directorios**

### **Comandos**

* **pwd**: Print Working Directory, me muestra el directorio en el que estoy ubicado. El símbolo ~ significa que estamos en el home
* **ls**: List, muestra los archivos y carpetas en el directorio actual
* **cd**: Change Directory, cambia de directorio
* **cp**: Copy, copia archivos y directorios
* **touch**: Crea un archivo si no existe, en caso contrario le cambia la fecha de modificación
* **mkdir**: Make Directory: Crea carpetas
* **mv**: Move: Mueve archivos y directorios, pero sirve también para renombrar los mismos
* **rm**: Remove: Borra archivos. Para borrar directorios con contenido dentro se debe añadir el modificador -r

Importante: La terminal es sensible a mayúsculas y minúsculas.

## **Diferencias entre LESS, CAT, HEAD y TAIL para lectura de archivos**

### **Comandos**

* **cat**: Muestra un archivo sin paginar.
* **sort**: El comando sort ordenará las líneas alfabéticamente por defecto.
* **less**: Muestra un archivo paginado. Pulsando “/” y escribiendo una palabra, puedo buscar las coincidencias de la misma en el archivo. Con la tecla “n” me muevo entre coincidencias hacia adelante, y con shift + “n” me muevo entre coincidencias hacia atras. Con espacio cambio de página. Salgo con "q"
* **tail**: Muestra las últimas 10 líneas de un archivo específico. Con la opción “-n” puedo modificar la cantidad de líneas que veo. Con la opción -f puedo poner los cambios en escucha
* **head**: Muestra las primeras 10 lineas de un archivo específico. Con la opción “-n” puedo modificar la cantidad de líneas que veo.
* **man**: Muestra ayuda sobre comandos. Ejemplo: ```man ls```.
* **more**: Sirve para leer archivos como less, observando que porcentaje del mismo ya se ha mostrado. Para avanzar en la lectura se utiliza la tecla enter

## **Interacción con archivos y permisos**

### **Comandos**

Permisos estan compuestos por 10 caracteres -rw-rw-r-- 

### Primer caracter corresponde al tipo de archivo
 * '-' = archivo
 * 'd' = directorio
 * 'l' = enlace simbólico

### Asignación de permisos en grupos de 3
 * 'u' = usuarios (corresponde caracter del 2 al 4)
 * 'g' = grupos (corresponde caracter del 5 al 7)
 * 'o' = otros (corresponde caracter del 8 al 10)
 * 'a' = todos

### Tipo de permisos
 * 'r' = lectura
 * 'w' = escritura
 * 'x' = ejecución
 * '-' = sin permiso
 * '+' añade permisos
 * '-' quita permisos

### Formato octal
 * '0' = (0+0+0) = Sin permisos = ---
 * '1' = (0+0+1) = Ejecución = --X
 * '2' = (0+2+0) = Escritura = -w-
 * '4' = (4+0+0) = Lectura = r--
 * '7' = (4+2+1)= = Eje, Esc, Lec = rwx

**chmod**: cambiar permisos (chmod u-r archivo.txt), forma rapida asignar permisos a todos 'chmod +x'.
**chown**: (Change Owner), cambia la propiedad del usuario, grupo y otros.

## **Conociendo las terminales en linux**
### **Comandos**

 * **chvt**: Cambia de terminal
 * **tty**: Muestra la terminal actual
 * **who**: Muestra los usuarios conectados a nuestro sistema
 * **w**: Hace lo mismo que el comando who pero muestra más información
 * **ps**: Muestra los procesos corriendo. Con los modificadores -ft y tty podemos filtrar para ver las conexiones de los usuarios.
 * **kill**: Mata un proceso. Con el modificador -9 fuerzo el cierre del mismo

## **Manejo y monitoreo de procesos y recursos del sistema**

### **Comandos**

 * **ps**: Muestra los procesos corriendo. Modificadores: **aux**: Muestra todos los procesos.
 * **jobs**: Al igual que el comando anterior, muestra los procesos. A diferencia de ps, es un comando interno de la terminal.
 * **fg**: Abre un proceso que estaba pausado.
 * **nohup**: Genera un archivo llamado “nohup.out” que muestra toda la información que produjo un proceso.
 * **grep**: Nos ayuda a filtrar el resultado de un comando o el contenido de un archivo dependiendo de las palabras (o incluso expresión regular) que le indiquemos.

### **Símbolos especiales**

 * **"|" Pipe**: Envia el standard output de un comando al standard input de otro.
 * **"&"** Ampersand: Envia un proceso al background.
 * **"./"**: Ejecuta un archivo.


## **Monitoreo de recursos del sistema**

### **Comandos**

 * **top**: Muestra la siguiente información del sistema:
    * **load average (carga promedio)**: Provee una representación en números del 1 al número de procesadores que tenga nuestro servidor del uso de los mismos.
    * **Uso de la memoria**
    * **Cantidad de usuarios**
    * **Uso del CPU**
    * **Procesos**
    * **Etc**
 * **free**: Me muestra información sobre la memoria de mi sistema. Con el modificador -h la información es más legible para un humano
* **du**: Muestra información sobre el disco duro. Con el modificador -hsc y un directorio especificado muestra el tamaño de ese directorio
* **htop**: Funciona como top pero funciona de forma más intuitiva

### **Comandos útiles**
Muestra información sobre el CPU
```
cat /proc/cpuinfo | grep "processor"
```
Muestra los 5 procesos que más uso hacen del CPU
```
sudo ps auxf | sort -nr -k 3 | head -5
```
Muestra los 5 procesos que más uso hacen de la memoria RAM
```
sudo ps auxf | sort -nr -k 4 | head -5
```

## **Análisis de los parámetros de red**

Existen dos tipos de direcciones IP: Privadas y Públicas.

### **Comandos**

 * **ifconfig**: Interface Configuration, muestra las tarjetas de red que tenemos y su direccionamiento específico.
 * **ip a**: IP Address Show, muestra las direcciones IP.
 * **hostname**: Como se identifica este equipo en la red.
 * **route -n**: Muestra cual es el dispositivo que me permite conectarme a internet.
 * **nslookup**: Muestra la dirección IP de un dominio determinado.
 * **curl**: Realiza consultas a un servidor.
 * **wget**: Permite descargar contenido de un servidor.
 * **ip -4 a**: Muestra las direcciones IPv4.
 * **ip -6 a**: Muestra las direcciones IPv6.

## **Administración de paquetes acorde a la distribución**

### **Red Hat / CentOS / Fedora**
Significado de rpm (Red Hat Package Manager).

Base de datos RPM, localizada en var/lib/rpm
 * **rpm -qa**: Listar todos los rpms instalados en la máquina. (query all)
 * **rpm -i paquete.rpm**: Realizar la instalación de un paquete. (install)
 * **rpm -e paquete.rpm**: Remover un paquete del sistema. (erase)

Repositorios yum Permite instalar un paquete desde un repositorio sin tener que conocer la ruta del archivo o las dependencias.
 * yum install paquete

### **Debian / Ubuntu**
Significado de deb (Debian package management).

Base de datos DPKG, localizada en /var/lib/dpkg

 * **dpkg -l**: Listar todos los debs instalados en la máquina.
 * **dpkg -i paquete.deb**: Realizar la instalación de un paquete.
 * **dpkg -r paquete.deb**: Remover un paquete del sistema.
 * **dpkg-reconfigure**: Volver a ejecutar el asistente de configuración si está disponible.

Repositorios apt otra forma de instalar.
 * apt install paquete

## **Manejo de paquetes en sistemas basados en Debian**

### **Comandos útiles**

 * **sudo apt update**: Actualiza la información local sobre los repositorios de Ubuntu
 * **sudo apt upgrade**: Actualiza todos los programas que tenemos instalados en la máquina
 * **sudo apt search paquete**: Para buscar el paquete a instalar.
 * **dkpg -l**: muestra todos los paquetes instalado en el SO.
 * **sudo snap install paquete**: Instala un paquete con el nuevo gestor de paquetes de Canonical, snap
 * **date**: Imprime la fecha actual.
 * **sudo dpkg-reconfigure tzdata**: reconfigurar la zona horaria.

## **Nagios: Desempaquetado, descompresión, compilación e instalación de paquetes**

### **Instalar los siguientes paquetes**
```
sudo apt install build-essential libgd-dev openssl libssl-dev unzip apache2 php gcc libdbi-perl libdbd-mysql-perl
```

Descargar Nagios 
```
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.4.tar.gz -O nagioscore.tar.gz
```

Descomprimir y desempaquetar el archivo descargado
```
tar xvzf nagioscore.tar.gz
```

Configurar e instalar (en la carpeta donde se descomprimieron los archivos) a Nagios
```
cd nagios-4.4.4/
sudo ./configure --with-https-conf=/etc/apache2/sites-enabled
sudo make all
sudo make install
```

Si no les funciona "sudo make install" continuar con los siguientes comandos y ejecutar luego "sudo make install"
```
sudo make install-groups-users
sudo usermod -a -G nagios www-data
```

```
sudo make install-init
sudo make install-commandmode
sudo make install-config
sudo make install-webconf
```

Activar el servicio de Nagios
```
sudo systemctl start nagios
```

BONUS: instalar plugins de Nagios
```
wget https://nagios-plugins.org/download/nagios-plugins-2.2.1.tar.gz -O nagios-plugins.tar.gz
tar xzvf nagios-plugins.tar.gz
cd nagios-plugins-2.2.1
sudo ./configure
sudo make all
sudo make install
```


## **Los usuarios, una tarea vital en el proceso de administración del sistema operativo**
### **Comandos**

* **id**: Muestra el identificador único de mi usuario, del grupo al que pertenezco y los grupos de los cuales formo parte
* **whoami**: Muestra que usuario soy
* **passwd**: Cambia la contraseña del usuario actual

### **Comandos útiles**
Muestra todos los usuarios del sistema operativo
```
cat /etc/passwd
```
Muestra las contraseñas del sistema operativo
```
cat /etc/shadow
```

## **Creando y manejando cuentas de usuario en el sistema operativo**

### **Comandos**

 * **sudo useradd usuario**: Crea un usuario
 * **sudo adduser usuario**: Crea un usuario y solicita un password, además de otros datos
 * **sudo userdel usuario**: Borra un usuario
 * **history**: Muestra todos los comandos usados anteriormente
 * **sudo usermod**: Modifica un usuario

### **Comandos útiles**
Muestra el contenido de un comando.
```
cat /usr/sbin/nombre_de_comando
```

## **Entendiendo la membresía de los grupos**

### **Comandos**

* **su - usuario**: Switch User, cambia de usuario
* **groups usuario**: Muestra a que grupos pertenece cierto usuario
* **sudo gpasswd -a usuario grupo**: Agrega un usuario a un grupo
* **sudo gpasswd -d usuario grupo**: Quita a un usuario de un grupo
* **usermod -aG grupo usuario**: Agrega un usuario a un grupo
* **sudo -l**: Muestra que permisos tiene el usuario actual

## **Usando PAM para el control de acceso de usuarios**

### **Comandos**

 * **pwscore**: Evalúa si una contraseña es buena o mala del 0 al 100
 * **ulimit**: Muestra los permisos que tiene el usuario actual. Modificadores: -u numero: Cambia la cantidad de procesos que mi usuario puede ejecutar.

### **Comandos útiles**
Modifica el archivo que indica en que horarios pueden conectarse ciertos usuarios.
```
sudo vi /etc/security/time.conf

#Agregamos está linea.
*;*;ponky|nodejs;Wk0800-1800
```


## **Autenticación de clientes y servidores sobre SSH**


**SSH**: Secure Shell, es un protocolo que permite conectar dos computadoras de forma remota sin necesidad de un password, únicamente con la interacción de una llave pública y una llave privada (aunque podemos colocar una contraseña sobre las llaves)

### **Configuración**

 * En el servidor, abrir el archivo /etc/ssh/sshd_config con algún editor. Leer el archivo y configurar a gusto.
 * En la consola de la máquina cliente abrir ssh-keygen para generar las llaves
 * Elegir ubicación para guardar la llave privada
 * Ejecutar 
   ```
   ssh-copy-id -i directorio_de_llave/id_rsa.pub nombre_usuario@direccion_ip_del_servidor
   ```
 * Ejecutar 
   ```
   ssh nombre_usuario@direccion_ip_del_servidor 
   ```

### **Tips**


* En lugar de descargar Putty en Windows podemos utilizar el emulador de consola Unix llamado Cmder para ejecutar los comandos vistos en clase. Incluso si esto falla, lo que personalmente recomiendo es instalar un subsistema de linux si tenemos Windows 10. Platzi tiene incluso un artículo sobre como hacer eso: https://platzi.com/clases/1378-python-practico/19200-importante-instalando-ubuntu-bash-en-windows-para-/
* Si la conexión falla, podemos usar el modificador -v (verbose) en el comando ssh para poder ver la información que envían las máquinas que intentan conectarse. La “v” puede repetirse hasta cuatro veces, quedando el comando, por ejemplo, como(a mas “v” pongamos, más información se mostrará):
   ```
   ssh -vvvv nombre_usuario@direccion_ip_del_servidor
   ```

### **BONUS**

**Reto**: Restringir el acceso al usuario root por ssh, y permitir solo un usuario determinado conectado

**Solución**:

Colocar en el archivo /etc/ssh/sshd_config del servidor las siguientes líneas:
```
PermitRootLogin no
AllowUsers nombre_usuario
```
Ejecutar el siguiente comando para reiniciar el servicio de ssh:
```
sudo service sshd restart
```

## **Arranque, detención y recarga de servicios**

### **Comandos**

* **sudo systemctl status servicio**: Estado de un servicio.
* **sudo systemctl enable servicio**: Habilita un servicio.
* **sudo systemctl disable servicio**: Deshabilita un servicio.
* **sudo systemctl start servicio**: Enciende un servicio.
* **sudo systemctl stop servicio**: Apaga un servicio.
* **sudo systemctl restart servicio**: Reinicia un servicio.
* **sudo systemctl list-units -t service --all**: Lista los servicios del sistema.
* **sudo journalctl -fu servicio**: Muestra el log de un servicio.
* **sudo journalctl --disk-usage**: Muestra cuanto pesan los logs en el sistema operativo.
* **sudo journalctl --list-boots**: Muestra los booteos de la computadora.
* **sudo journalctl -p critic|notice|info|warning|error**: Muestra mensajes de determinada categoría de nuestros logs.
* **sudo journalctl -o json**: Muestra los logs en formato json.

## **Los logs, nuestros mejores amigos**

### **Comandos**

 * **find [ruta]**: Buscar algo en el sistema operativo.
   * **-type**: Indica que tipo estamos buscando; archivos, directorios y enlaces
   * **-name**: Indica el nombre de lo que estamos buscando
   * **-iname**: Indica el nombre de lo que estamos buscando, pero sin tener en cuenta mayúsculas y minúsculas
   * **!**: Niega la expresión que buscamos (es decir, busca lo contrario)
   * **-mtime**: Muestra archivos con cambios en los últimos n minutos
 * **grep [string] [archivo]**: Busca una cadena de caracteres o expresión regular en un archivo determinado. Si ejecutamos por ejemplo algo como comando | grep [string] vamos a filtrar el resultado de un comando por la cadena o regex que especifiquemos
 * **awk**: Es un lenguaje que nos ayuda a filtrar patrones en un archivo, organizarlos y darles formato

### **Comandos útiles**
Muestra los archivos de log que tenemos en el sistema
```
find /var/log/ -iname "*.log" -type f**: 
```
Muestra los archivos de configuración que tuvieron salidas de error en los últimos diez minutos.
```
sudo find /etc/ -mtime 10 2
```
Muestra las IP’s que se conectaron con nuestro servidor nginx.
```
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr
```
Muestra los errores que surgieron en nuestro servidor nginx
```
awk '{print $9}' /var/log/nginx/access.log | sort | uniq -c | sort -nr
```
