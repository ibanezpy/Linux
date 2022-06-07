Ejecutar el script

Este script sirve para realizar los backups de tus routes mikrotiks y poder almacenarlos en un servidor local utilizando FPT.

Para nuestro caso utilizamos un server FTP.

```
cd /root/ansible/backups_mikrotiks/
ansible-playbook -i hosts mikrotik-backup-config.yml
```