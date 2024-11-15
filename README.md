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

## Cuestiones finales

### ¿Qué pasa si no hago el link simbólico entre `sites-available` y `sites-enabled` de mi sitio web?

El archivo de configuración del servidor `javiWeb` se almacena en el directorio `sites-available`, pero para que Nginx pueda utilizarlo, debe crearse el enlace simbólico al directorio `sites-enabled`, ya que Nginx solo carga los archivos que están dentros de este directorio.
- **Sin el enlace simbólico**, Nginx no podrá encontrar la configuración del sitio, por lo que **el sitio web no podrá ser servido**.
- **Con el enlace simbólico**, Nginx es capaz de habilitar el sitio para que sea servido.

### ¿Qué pasa si no le doy los permisos adecuados a `/var/www/javiWeb`?

Los permisos son necesarios para que el servidor pueda acceder y servir los archivos desde `var/www/javiWeb`. Si no le das los permisos adecuados, surgirán varios problemas:
- **Nginx no podrá acceder a los archivos**: El servidor no podrá leer los archivos HTML u otros para servirlos a los usuarios. Esto dará lugar al error `404 Not Found` o a `403 Forbidden`.
- **Permisos de propietario y grupo**: Si uno no se asegura de que los archivos sean propiedad de `www-data`, los archivos no serán accesibles o modificables por el servidor.