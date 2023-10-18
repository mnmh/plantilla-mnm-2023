Desplegar WordPress dockerizado en un servidor Ubuntu a través de SSH implica varios pasos. Aquí tienes una guía paso a paso para hacerlo:

**Paso 1: Conectar al Servidor a través de SSH**

Asegúrate de que puedes acceder a tu servidor Ubuntu mediante SSH desde tu máquina local. Utiliza el siguiente comando para conectarte (sustituye `usuario` y `ip-del-servidor` por tus credenciales):

```bash
ssh usuario@ip-del-servidor
```

**Paso 2: Actualizar el Sistema**

Es importante mantener el sistema actualizado:

```bash
sudo apt update
sudo apt upgrade
```

**Paso 3: Instalar Docker y Docker Compose**

Si aún no tienes Docker instalado, puedes hacerlo ejecutando los siguientes comandos:

```bash
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

Luego, instala Docker Compose:

```bash
sudo apt install docker-compose
```

**Paso 4: Crear un Directorio para WordPress**

Crea un directorio donde se almacenarán los archivos de configuración de Docker Compose y los datos de WordPress:

```bash
mkdir ~/wordpress
cd ~/wordpress
```

**Paso 5: Crear un archivo `docker-compose.yml`**

Crea un archivo `docker-compose.yml` en el directorio que acabas de crear. Este archivo definirá la configuración de tu contenedor de WordPress. Puedes usar un editor de texto, como `nano` o `vim`, para crear el archivo:

```bash
nano docker-compose.yml
```

Agrega el siguiente contenido al archivo `docker-compose.yml`:

```yaml
version: '3'
services:
  wordpress:
    image: wordpress:latest
    ports:
      - "80:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: your_db_user
      WORDPRESS_DB_PASSWORD: your_db_password
      WORDPRESS_DB_NAME: your_db_name
    volumes:
      - ./wp-data:/var/www/html
    depends_on:
      - db
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: your_db_root_password
      MYSQL_DATABASE: your_db_name
      MYSQL_USER: your_db_user
      MYSQL_PASSWORD: your_db_password
    volumes:
      - ./db-data:/var/lib/mysql
```

Asegúrate de reemplazar `your_db_user`, `your_db_password`, `your_db_name`, y `your_db_root_password` con tus propios valores.

**Paso 6: Ejecutar Docker Compose**

Ejecuta Docker Compose para crear y ejecutar los contenedores de WordPress y MySQL:

```bash
docker-compose up -d
```

**Paso 7: Acceder a WordPress**

Una vez que los contenedores estén en funcionamiento, puedes acceder a WordPress desde tu navegador web ingresando la dirección IP del servidor en la barra de direcciones. Deberías ver la configuración inicial de WordPress. Sigue las instrucciones para configurar tu sitio.

**Paso 8: Configurar un Servidor Web Reverso (Opcional)**

Si deseas que tu sitio WordPress sea accesible a través de un nombre de dominio, puedes configurar un servidor web reverso como Nginx o Apache. Asegúrate de configurar la proxy_pass o el virtual host para que apunte al puerto 80 del servidor Docker donde se ejecuta WordPress.

Con estos pasos, habrás desplegado WordPress dockerizado en tu servidor Ubuntu a través de SSH. Puedes personalizar aún más tu sitio WordPress instalando temas y complementos según tus necesidades.
