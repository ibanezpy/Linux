### Lista de comandos utiles que utilizamos día a día

## RSYNC

Uso de RSYNC remoto con el uso de ssh y llaves ssh.
```
rsync -avzPe "ssh -i /.ssh/key.pem" /home/samba/ root@172.27.14.67:/home/samba
```