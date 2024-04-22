# DockerMoodle
Creación de una plataforma de Formación Moodle con Docker Compose

> Para la creación de esta plataforma moodle utilizamos las imagenes de bitnami/moodle

- [Docker Hub de Bitnami](https://hub.docker.com/r/bitnami/moodle)
- [Github de Bitnami Moodle](https://github.com/bitnami/containers/tree/main/bitnami/moodle)

En las variables de entorno tenemos toda la información del sitio, pero cabe destacar estas dos

| Name                          | Description                                             | Default Value          |
|-------------------------------|---------------------------------------------------------|-------------------------
| `MOODLE_USERNAME`             | Moodle user name.                                       | `user`                 |
| `MOODLE_PASSWORD`             | Moodle user password.                                   | `bitnami`              |
| `MOODLE_DATABASE_TYPE`        | Database type to be used for the Moodle installation.   | `mariadb`              |
| `MOODLE_DATABASE_HOST`        | Database server host.                                   | `mariadb`                          |
| `MOODLE_DATABASE_PORT_NUMBER` | Database server port.                                   | `3306`                             |
| `MOODLE_DATABASE_NAME`        | Database name.                                          | `bitnami_moodle`                   |
| `MOODLE_DATABASE_USER`        | Database user name.                                     | `bn_moodle`                        |
| `MOODLE_DATABASE_PASSWORD`    | Database user password.                                 | `nil`                              |

La base de datos se encuentra si contraseña para root (cambiar en las opciones del archivo yaml para un sitio en producción)

Para los siguientes cambios debemos acceder al contenedor:

```console
docker exec -i -t moodle-moodle-1 /bin/bash
```

Los archivos de configuración php.ini se encuentran en la carpetas del contenedor:

- /opt/bitnami/php/lib/php.ini
- /opt/bitnami/php/etc/php.ini
- /opt/bitnami/php/etc.default/php.ini

Se recomienda cambair las variables upload_max_filesize y post_max_size para ampliar el tamaño de archivo a subir, necesario para importar cursos, por defecto esta a 40M, establecer a 900M

El editor de texto nano no se encuentra instalado, por lo que habrá que hacerlo manualmente:

```console
apt update
apt upgrade
apt install nano
```
Editamos el archivo php.ini
```console
nano /opt/bitnami/php/etc/php.ini
```
```diff
- upload_max_filesize = 40M
- post_max_size = 40M

+ upload_max_filesize = 900M
+ post_max_size = 900M
```

Reiniciamos el contenedor de Moodle
```console
docker restart moodle-moodle-1
```


## Otras consideraciones

```diff
   moodle:
     ports:
-      - '80:8080'
-      - '443:8443'
+      - '80:8880'
+      - '443:4443'
     environment:
       ...
+      - PHP_MEMORY_LIMIT=512m
     ...
```


1. El puerto de acceso a Moodle es el 8880 (modificado 80 por defecto)
2. Se ha expuesto el puerto de MariaDB al 33306 para poder acceder con DBeaver o el cliente MariaDB
```console
 mariadb  --protocol=tcp -u root -p -P 3336
```

Simplemente descargamos el archivo docker-compose.yaml a una carpeta llamada moodle y lo lanzamos
```console
docker compose up -d
```

Para acceder a la aplicación:
http://your-ip:8880

Ejecuta la aplicación sin descargar nada simplemente usando este comando:
```console
curl -sSL https://raw.githubusercontent.com/dgmx/DockerMoodle/main/docker-compose.yaml > docker-compose.yml
docker-compose up -d
```





