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

## Descripción General

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