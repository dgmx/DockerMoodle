# DockerMoodle
Creación de una plataforma de Formación Moodle con Docker Compose

Para la creación de esta plataforma moodle utilizamos las imagenes de bitnami/moodle

[Docker Hub de Bitnami](https://hub.docker.com/r/bitnami/moodle)

En las variables de entorno tenemos toda la información del sitio, pero cabe destacar estas dos

* MOODLE_USERNAME 	Moodle user name. 	user
* MOODLE_PASSWORD 	Moodle user password. 	bitnami

La base de datos tiene la siguioente configuración:

* MOODLE_DATABASE_USER 	Database user name. 	bn_moodle

sin contraseñas (cambiar en las opciones del archivo yaml para un sitio en producción)

Los archivos de configuración php.ini se encuentran en la carpetas:

/opt/bitnami/php/lib/php.ini
/opt/bitnami/php/etc/php.ini
/opt/bitnami/php/etc.default/php.ini

Se recomienda cambair las variables upload_max_filesize y post_max_size para ampliar el tamaño de archivo a subir, necesario para importar cursos, por defecto esta a 40M, establecer a 900M

El editor de texto nano no se encuentra instalado, por lo que habrá que hacerlo manualmente:

```
apt update
apt upgrade
apt install nano
```
Editamos en archivo
```
nano /opt/bitnami/php/etc/php.ini
upload_max_filesize = 900M
post_max_size = 900M
```
Reiniciamos el contenedor de Moodle
```
docker restart moodle-moodle-1
```


## Otras consideraciones
El puerto de acceso a Moodle es el 8880 (modificado 80 por defecto)
Se ha expuesto el puerto de MariaDB al 33306 para poder acceder con DBeaver o el cliente MariaDB
```
 mariadb  --protocol=tcp -u root -p -P 3336
```




