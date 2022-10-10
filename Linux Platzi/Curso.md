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