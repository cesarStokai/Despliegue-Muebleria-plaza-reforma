# Guía para levantar un servicio de MySQL con Docker y conectarse a la base de datos

## Requisitos previos
1. Tener Docker y Docker Compose instalados en tu servidor.
2. Asegurarte de que los puertos necesarios (por defecto el 3306 para MySQL) estén abiertos en el firewall del servidor.

---

## Crear un archivo `docker-compose.yml`
1. En el directorio donde deseas configurar el servicio, crea un archivo llamado `docker-compose.yml`:
   ```bash
   nano docker-compose.yml
   ```

2. Escribe la siguiente configuración básica para levantar un contenedor con MySQL:
   ```yaml
   version: '3.8'
   services:
     mysql:
       image: mysql:8.0
       container_name: mysql_service
       restart: always
       environment:
         MYSQL_ROOT_PASSWORD: tu_contraseña_segura
         MYSQL_DATABASE: nombre_base_datos
         MYSQL_USER: usuario
         MYSQL_PASSWORD: contraseña_usuario
       ports:
         - "3306:3306"
       volumes:
         - mysql_data:/var/lib/mysql
   
   volumes:
     mysql_data:
   ```
   - **MYSQL_ROOT_PASSWORD**: Contraseña para el usuario root de MySQL.
   - **MYSQL_DATABASE**: Nombre de la base de datos que se creará automáticamente al iniciar el contenedor.
   - **MYSQL_USER** y **MYSQL_PASSWORD**: Credenciales para un usuario adicional que se creará automáticamente.
   - **volumes**: Permite persistir los datos de MySQL en el servidor.

3. Guarda y cierra el archivo.

---

## Levantar el servicio de MySQL
1. Ejecuta el siguiente comando para iniciar el servicio:
   ```bash
   docker-compose up -d
   ```
   - El flag `-d` asegura que el servicio se ejecute en segundo plano.

2. Verifica que el contenedor esté corriendo:
   ```bash
   docker ps
   ```
   - Deberías ver un contenedor con el nombre `mysql_service` en la lista.

---

## Conectarse al servicio de MySQL
### Desde el servidor (localmente):
1. Accede al contenedor de MySQL:
   ```bash
   docker exec -it mysql_service mysql -u root -p
   ```
   - Ingresa la contraseña que configuraste en `MYSQL_ROOT_PASSWORD`.

2. Una vez dentro del cliente MySQL, puedes ejecutar comandos como:
   - Crear una base de datos:
     ```sql
     CREATE DATABASE mi_base_datos;
     ```
   - Ver las bases de datos existentes:
     ```sql
     SHOW DATABASES;
     ```
   - Usar una base de datos:
     ```sql
     USE mi_base_datos;
     ```

### Desde tu máquina local:
1. Usa un cliente MySQL como MySQL Workbench, DBeaver o la línea de comandos.
2. Configura la conexión con los siguientes datos:
   - **Host**: Dirección IP de tu servidor.
   - **Puerto**: 3306 (o el que configuraste en el archivo `docker-compose.yml`).
   - **Usuario**: El usuario configurado en `MYSQL_USER`.
   - **Contraseña**: La contraseña configurada en `MYSQL_PASSWORD`.

---

## Importar una base de datos
1. Copia el archivo `.sql` que deseas importar al servidor (si está en tu máquina local):
   ```bash
   scp archivo.sql root@LA_IP:/ruta/deseada
   ```

2. Accede al contenedor de MySQL:
   ```bash
   docker exec -it mysql_service mysql -u root -p
   ```

3. Dentro del cliente MySQL, selecciona la base de datos donde deseas importar los datos:
   ```sql
   USE nombre_base_datos;
   ```

4. Sal del cliente MySQL y ejecuta el siguiente comando para importar el archivo:
   ```bash
   docker exec -i mysql_service mysql -u root -p nombre_base_datos < /ruta/deseada/archivo.sql
   ```
   - Ingresa la contraseña cuando se te solicite.

---

## Verificación final
1. Asegúrate de que los datos se hayan importado correctamente:
   ```sql
   SHOW TABLES;
   ```
2. Si todo está correcto, tu servicio de MySQL estará listo para usarse.

---

Con estos pasos, puedes levantar un servicio de MySQL, conectarte a él y gestionar tus bases de datos de manera eficiente.
