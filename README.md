# 2RecuperacionDocker.  

## Parte 1  (docker-compose.yml)  
Creamos un archivo docker-compose.yml:  

services:  
  owncloud:  
    image: owncloud/server  
    container_name: owncloud_server  
    restart: always  
    ports:  
      - 8080:8080  
    depends_on:  
      - mariadb  
    environment:  
      - OWNCLOUD_DB_HOST: mariadb  
      - OWNCLOUD_DB_NAME: sxe_owncloud_db #Nombre de la base de datos  
      - OWNCLOUD_DB_USERNAME: sxe_user_db #Nombre del usuario de la base de datos  
      - OWNCLOUD_DB_PASSWORD: sxe_user_password_db #Contraseña del usuario de la base de datos  
      - OWNCLOUD_ADMIN_USERNAME: sxe_admin #Nombre   del usuario administrador de owncloud    
      - OWNCLOUD_ADMIN_PASSWORD: sxe_admin_password #Contraseña del usuario administrador de owncloud  
    volumes:  
      - oc_vol  

  mariadb:  
    image: mariadb:11.1  
    container_name: owncloud_mariadb  
    restart: always  
    environment:  
      - MYSQL_ROOT_PASSWORD: sxe_root_password #Password del usuario root de la base de datos  
      - MYSQL_USER: sxe_user_db  
      - MYSQL_PASSWORD: sxe_user_db_password  
      - MYSQL_DATABASE: sxe_owncloud_db  
    volumes:  
      - db_vol  

  adminer:  
    image: adminer  
    restart: always  
    ports:  
      - 8080:80  

















## Parte 2 (Preguntas de teoría)  
1. Si ejecutas un contenedor con el siguiente comando: docker run -it alpine,  
¿qué sucederá cuando salgas de la sesión interactiva del contenedor?  
¿Por qué el contenedor se detiene automáticamente?

Al salir de la sesión interactiva del contenedor, este dejará de ejecutarse.  
Se detiene automáticamente porque:  
- i: significa que el proceso del contenedor lanzado está en modo interactivo, por lo que al salir de
  la sesión interactiva del contenedor, dejará de ejecutarse.
- t: asigna al proceso lanzado al arrancar el contenedor una pseudo terminal, que facilita el acceso a
  dicho contenedor desde nuestra terminal.  

2. Si lanzas un contenedor de Docker sin usar volúmenes ni “bind mount”, y  
luego haces cambios dentro del contenedor (como crear archivos o  
carpetas), ¿se mantendrán esos cambios cuando detengas el contenedor y  
lo vuelvas a arrancar? ¿Por qué?  

No. Porque no tienen ningún sitio donde puedan estar guardados.  

3. Si ejecutas  
docker run -p 8080:79 nginx  
(imagen nginx)  
E intentas acceder con el navegador a:  
<Dirección-ip del anfitrión del contenedor>:8080  
¿Qué sucederá?¿Por qué?  

No funcionará y, por lo tanto, no se podrá acceder al servidor.
El problema radica en que el puerto 79 especificado en el comando no es un puerto utilizado por el
servidor nginx. Por defecto, nginx escucha en el puerto 80 dentro del contenedor, pero el comando  
redirige las conexiones del puerto 8080 del anfitrión al puerto 79 del contenedor, donde no hay nada
escuchando.  
