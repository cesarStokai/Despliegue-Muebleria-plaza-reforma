# Guía para verificar y configurar Nginx, Docker y Docker Compose en un servidor Ubuntu

## Conexión al servidor
1. Abre tu terminal o línea de comandos.
2. Conéctate al servidor mediante SSH:
   ```bash
   ssh root@74.208.167.90
   ```
   - Si necesitas usar una clave privada, utiliza el siguiente comando:
     ```bash
     ssh -i ruta_a_tu_clave_privada.pem root@LA_IP
     ```

## Verificar si Nginx está instalado
1. Una vez conectado al servidor, ejecuta el siguiente comando para verificar si Nginx está instalado:
   ```bash
   nginx -v
   ```
   - Si Nginx está instalado, este comando mostrará la versión.
   - Si no está instalado, verás un mensaje indicando que el comando no se encuentra.

2. Si necesitas instalar Nginx, ejecuta los siguientes comandos:
   ```bash
   sudo apt update
   sudo apt install nginx -y
   ```

3. Verifica que el servicio de Nginx esté activo:
   ```bash
   sudo systemctl status nginx
   ```
   - Si el servicio no está activo, puedes iniciarlo con:
     ```bash
     sudo systemctl start nginx
     ```

## Verificar Docker y Docker Compose
1. Verifica que Docker esté instalado:
   ```bash
   docker --version
   ```
   - Si Docker no está instalado, puedes instalarlo siguiendo la [documentación oficial de Docker](https://docs.docker.com/engine/install/ubuntu/).

2. Verifica que Docker Compose esté instalado:
   ```bash
   docker-compose --version
   ```
   - Si Docker Compose no está instalado, puedes instalarlo con:
     ```bash
     sudo curl -L "https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
     sudo chmod +x /usr/local/bin/docker-compose
     ```

## Configuración básica de Nginx con Docker
1. Crea un archivo de configuración para Nginx (por ejemplo, `default.conf`):
   ```bash
   sudo nano /etc/nginx/conf.d/default.conf
   ```
   - Ejemplo de configuración básica:
     ```nginx
     server {
         listen 80;
         server_name localhost;

         location / {
             proxy_pass http://127.0.0.1:8080;
             proxy_set_header Host $host;
             proxy_set_header X-Real-IP $remote_addr;
         }
     }
     ```

2. Reinicia Nginx para aplicar los cambios:
   ```bash
   sudo systemctl restart nginx
   ```

## Configuración básica de Docker Compose
1. Crea un archivo `docker-compose.yml` en el directorio deseado:
   ```bash
   nano docker-compose.yml
   ```
   - Ejemplo de configuración básica:
     ```yaml
     version: '3.8'
     services:
       app:
         image: tu_imagen_docker
         ports:
           - "8080:80"
     ```

2. Inicia los servicios definidos en el archivo:
   ```bash
   docker-compose up -d
   ```

## Verificación final
1. Abre un navegador y accede a la dirección IP de tu servidor para verificar que Nginx está funcionando correctamente.
2. Si configuraste Docker Compose, asegúrate de que los contenedores estén corriendo:
   ```bash
   docker ps
   ```

---

Con estos pasos, deberías tener un servidor configurado con Nginx, Docker y Docker Compose funcionando correctamente.

