# Instalación y configuración de servidor web Nginx

## Estructura del Proyecto

```bash
InstalacionYConfiguracionNGINX
│   .gitignore
│   README.md
│   Vagrantfile
│
└───files
        javiWeb
```

## Comprobación del correcto funcionamiento

El archivo `/etc/hosts` de la máquina anfitriona se ha editado para que asocie la IP de la máquina virtual (`192.168.57.10`) al nombre de nuestro servidor `javiWeb`.

`C:\\Windows\system32\drivers\etc\hosts`: 
```
192.168.57.10 javiWeb
```

### Así se ve la página web:

![prueba funcionamiento](./images/pruebaFuncionamiento.png)

### Comprobamos los registros del servidor

![access.log](./images/access.log.png)
---
![error.log](./images/error.log.png)

Hay entradas en ambos registros, lo cual demuestra que la información de las peticiones del anfitrión está llegando a la máquina virtual.

### Para finalizar aplicamos el FTP

Es necesario añadir la configuración de la actividad pasiva del cliente al fichero `vsftpd.conf`:
```
pasv_enable=YES
pasv_min_port=40000
pasv_max_port=40100
```
Y aportar una `openssl key` para `vsftpd`:
```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/vsftpd.key \
  -out /etc/ssl/certs/vsftpd.crt \
  -subj "/C=ES/ST=Andalucía/L=Granada/O=IZV/OU=WEB/CN=vsftpd/emailAddress=webmaster@vsftpd.com"
```

Se comprueba el funcionamiento con FileZilla
![filezilla](./images/filezilla.png)


## 2.2 Autenticación

Se añade la página perfectLearn y se hacen cambios a la sección `location` para permitir o denegar acceso a ciertas direcciones IPs, en este caso la de mi ordenador.
La página `javiWeb` no permitirá el acceso y devolverá error `403 - Forbidden`. La página `perfectLearn` pedirá autenticación para acceder a la ruta `contact`.

```
allow all;
deny 80.28.211.131;
```
o
```
allow 80.28.211.131;
deny all;
```

### Este es el error de `403 - Forbidden` en la página `javiWeb`
![403-Forbidden](./images/403-forbidden.png)

### Mensajes de error de `error.log`
![authentication-error.log](./images/authentication-error.log.png)



## Cuestiones finales

### ¿Qué pasa si no hago el link simbólico entre `sites-available` y `sites-enabled` de mi sitio web?

El archivo de configuración del servidor `javiWeb` se almacena en el directorio `sites-available`, pero para que Nginx pueda utilizarlo, debe crearse el enlace simbólico al directorio `sites-enabled`, ya que Nginx solo carga los archivos que están dentros de este directorio.
- **Sin el enlace simbólico**, Nginx no podrá encontrar la configuración del sitio, por lo que **el sitio web no podrá ser servido**.
- **Con el enlace simbólico**, Nginx es capaz de habilitar el sitio para que sea servido.

### ¿Qué pasa si no le doy los permisos adecuados a `/var/www/javiWeb`?

Los permisos son necesarios para que el servidor pueda acceder y servir los archivos desde `var/www/javiWeb`. Si no le das los permisos adecuados, surgirán varios problemas:
- **Nginx no podrá acceder a los archivos**: El servidor no podrá leer los archivos HTML u otros para servirlos a los usuarios. Esto dará lugar al error `404 Not Found` o a `403 Forbidden`.
- **Permisos de propietario y grupo**: Si uno no se asegura de que los archivos sean propiedad de `www-data`, los archivos no serán accesibles o modificables por el servidor.

### Comprueba que si decides cancelar la autenticación, se te negará el acceso al sitio con un error. ¿Qué error es?

El error que sale es 401 Authorization Required