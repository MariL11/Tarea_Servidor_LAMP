# Tarea Servidor LAMP

## 1. Objetivo de la práctica

El objetivo principal de esta práctica es que aprendamos a desplegar una aplicación web
estándar sobre un entorno o stack LAMP. Ya sabemos cómo instalar Apache Web Server, por lo que tendremos que agregar un servidor de bases de datos (MySQL/MariaDB) y el intérprete de PHP. Al final de la práctica tendremos un servidor web Apache con un CMS Wordpress instalado y funcionando.

### 1.1. Qué necesitamos para instalar Wordpress

Antes de realizar el despliegue de una aplicación web, debemos tener claros los requisitos de la misma, tanto de servicios como versiones, usuarios, permisos, etc.
En el caso de Wordpress podemos echar un vistazo en su web oficial.

Si pinchamos en el botón ‘Consigue Wordpress’ nos llevará a una sección en la que además de poder descargarlo, si navegamos hacia abajo, podremos ver un apartado de ‘Requisitos’.

Como vemos, los requisitos más importantes que debemos cumplir son

> PHP 7.4 o superior

> MySQL 5.7 o superior / MariaDB 10.3 o superior


## 2. Instalación de los componentes necesarios.

Una vez que estamos dentro de nuestro servidor, es buena idea actualizar los repositorios antes de empezar con las instalaciones. Para ello haremos un update y un upgrade de nuestro sistema:

~~~
sudo apt update

sudo apt udgrade
~~~

- Entramos a Servidor

![Entrar servidor](./img/Imagen_1.PNG)

- Comando sudo apt update

![Actualizar lista de paquetes disponibles](./img/Imagen_2.PNG)

- Comando sudo apt udgrade

![Realizar actualizaciones disponibles](./img/Imagen_3.PNG)

![Realizar actualizaciones disponibles](./img/Imagen_4.PNG)


### 2.1. Instalación de Apache Web Server

Si ya tienes una máquina con Apache instalado, puedes saltarte este paso. 
Vamos a instalar Apache desde nuestro terminal, con el comando:

~~~
sudo apt install apache2
~~~

![Comprobar Apache2 esté instalado](./img/Imagen_5.PNG)

Una vez que lo hayas instalado, vamos a iniciarlo con el comando:

~~~
sudo systemctl start apache2
~~~

![Iniciar Apache2](./img/Imagen_6.PNG)

Si quisiéramos parar el servicio, ejecutaríamos el comando:

~~~
sudo systemctl stop apache
~~~

Para reiniciar el servicio de Apache, usaremos el comando:

~~~
sudo systemctl restart apache2
~~~

Y si queremos ver en qué estado está el servicio o si da algún mensaje de error, ejecutamos el comando:

~~~
sudo systemctl status apache2
~~~

### 2.2. Instalación de PHP

Vamos a instalar PHP desde nuestro terminal, con el comando:

~~~
sudo apt install php
~~~

![Instalar PHP](./img/Imagen_7.PNG)

![Instalar PHP](./img/Imagen_8.PNG)


Si queremos instalar las librerías más habituales, podemos usar el comando:

~~~
sudo apt install php-common
~~~

![Instalar librerias PHP](./img/Imagen_9.PNG)

![Instalar librerias PHP](./img/Imagen_10.PNG)

![Instalar librerias PHP](./img/Imagen_11.PNG)

NOTA; Si queremos instalar todas las librerías que haya disponibles para Php en nuestro
repositorio:

~~~
sudo apt install php-*
~~~

Si queremos comprobar la versión de Php que tenemos instalada usamos el comando:

~~~
php --version   o    php -v
~~~

![Ver versión de PHP](./img/Imagen_12.PNG)

### 2.3 Instalación de MariaDB Server

Para instalar MariaDB server debemos seguir los siguientes pasos:

**Paso 1. Instalar mariadb-server:**

~~~
sudo apt install mariadb-server
~~~

![Instalar MariaDB Server](./img/Imagen_13.PNG)

![Instalar MariaDB Server](./img/Imagen_14.PNG)

Para arrancar/parar/reiniciar/ver estado del servicio utilizaremos los siguientes comandos:

~~~
sudo systemctl start mysql

sudo systemctl stop mysql

sudo systemctl restart mysql

sudo systemctl status mysql
~~~

**Paso 2. Establecer la seguridad del servidor de bases de datos:**

~~~
sudo mysql_secure_installation
~~~

Si queremos crear una nueva contraseña (o si es la primera vez que lo ejecutamos) damos a intro y continuamos.

Cuando nos pregunte si deseamos cambiar la contraseña de root, le contestamos ‘y’, e introducimos la nueva contraseña (recuerda que debe cumplir las directrices de seguridad: longitud, mayúsculas, minúsculas, números y caracteres especiales).

Eliminamos la posibilidad de usar usuarios anónimos y de conectarse de forma remota, y borramos las bases de datos de prueba. Le confirmamos que recargue la tabla de privilegios.

Ya tenemos instalado todo lo necesario para instalar cualquier aplicación basada en PHP + MySQL.

![Establecer seguridad del servidor](./img/Imagen_15.PNG)

![Establecer seguridad del servidor](./img/Imagen_16.PNG)

## 3. Instalación de Wordpress

Para instalar Wordpress, como para la mayoría de aplicaciones web, necesitaremos seguir los siguientes pasos:

- Crear un directorio en Apache donde pondremos los ficheros de instalación, asociado a un ‘virtualhost’ que gestionará las peticiones a nuestra aplicación.

- Crear un usuario y una base de datos para que la aplicación pueda desplegar todas las tablas necesarias durante el proceso de despliegue.


### 3.1 Creación del virtualhost para nuestro sitio web

Vamos a crear un espacio para nuestro gestor de contenidos o sitio web y crearemos un  virtualhost en Apache para gestionarlo. Suponemos que vamos a trabajar con un dominio ficticio **tuApellido-wp.com**.

Creamos nuestro sitio en el directorio de configuración de Apache:

~~~
sudo nano /etc/apache2/sites-available/tuApellido-wp.com.conf
~~~

El contenido del fichero será

~~~
<VirtualHost *:80>
    ServerAdmin tuApellido@misitio.com 
    ServerName tuApellido-wp.com
    ServerAlias www.tuApellido-wp.com
    DocumentRoot /var/www/tuApellido-wp.com
</VirtualHost>
~~~

Guardamos y salimos.

Si activamos ahora el sitio, puede que nos devuelva un error, ya que aún no existe la carpeta wordpress. Podemos hacer la activación una vez que hayamos descomprimido los ficheros de instalación de wordpress.

![Crear nuevo sitio en directorio Apache](./img/Imagen_17.PNG)

![Editar fichero](./img/Imagen_18.PNG)

### 3.2 Descarga y despliegue de los ficherps de instalación

Para descargar wordpress en nuestro servidor vamos a utilizar **wget**, por lo que debemos saber la url de descarga del paquete de Wordpress para poder usar esta utilidad.

Si nos fijamos en el botón de descarga de la página de Wordpress y seleccionamos con el botón derecho del ratón la opción ‘copiar dirección de enlace’, obtendremos la url de descarga: https://es.wordpress.org/latest-es_ES.zip

Descargamos los ficheros de wordpress. Con la opción -P nos creará la carpeta.

~~~
sudo wget https://es.wordpress.org/latest-es_ES.zip -P /var/www
~~~

![Descargar ficheros Wordpress](./img/Imagen_19.PNG)

Descomprimimos el paquete descargado usando la herramienta unzip. Usamos la opción -d para indicar la carpeta destino:

~~~
sudo unzip /var/www/latest-es_ES.zip -d /var/www
~~~

- Instalar unzip

![Instalar unzip](./img/Imagen_20.PNG)

![Instalar unzip](./img/Imagen_21.PNG)

- Descomprimir paquete

![Instalar unzip](./img/Imagen_22.PNG)

![Instalar unzip](./img/Imagen_23.PNG)


Te creará una carpeta llamada **wordpress** que cambiaremos su nombre nuestra carpeta pública **tuApellido.com**:

~~~
sudo mv /var/www/wordpress /var/www/tuApellido-wp.com
~~~

![Crear carpeta](./img/Imagen_24.PNG)

Para que Apache tenga el control de las carpetas durante la instalación, hay que hacerlo propietario de todas las carpetas de Wordpress:

~~~
sudo chown -R www-data:www-data /var/www/tuApellido-wp.com
~~~

![Establecer privilegios](./img/Imagen_25.PNG)

### 3.3 Creación de la base de datos y el usuario parar la instalación

Vamos a crear una base de datos llamada ‘wordpress’ y un usuario llamado ‘wordpress’ en  MariaDB.

Accedemos a la consola de MariaDB:

~~~
mysql -u root -p
~~~

![Acceder consola de MariaDB](./img/Imagen_26.PNG)

~~~
MariaDB[ ]> CREATE DATABASE IF NOT EXISTS wordpress CHARACTER SET = ‘utf8mb4’;

MariaDB[ ]> CREATE USER IF NOT EXISTS‘wordpress’@’localhost’ IDENTIFIED BY ‘W0rdpress22#’;

MariaDB[ ]> GRANT ALL PRIVILEGES ON wordpress.* to ‘wordpress’@’localhost’;

MariaDB[ ]> FLUSH PRIVILEGES;

MariaDB[ ]> QUIT;
~~~

![Escribir comandos](./img/Imagen_27.PNG)


Accedemos a la consola de MariaDB (versión alternativa):

~~~
mysql -u root -p
~~~

~~~
MariaDB[ ]> CREATE DATABASE wordpress;

MariaDB[ ]> GRANT ALL PRIVILEGES ON wordpress.* to ‘wordpress’@’localhost’ IDENTIFIED BY  ‘W0rdpress22#’;
~~~

### 3.4 Activación del sitio web

Una vez que tenemos todos los ficheros en sus carpetas y hemos creado la base de datos para  la instalación de Wordpress, vamos a activar el sitio web. Para ello utilizaremos el comando a2ensite de Apache:

~~~
sudo a2ensite tuApellido-wp.com.conf

sudo systemctl reload apache2
~~~

- Activar sitio web (sudo a2ensite tuApellido-wp.com.conf)

![Activar sitio web](./img/Imagen_28.PNG)

- Recargar configuración

![Recargar configuración](./img/Imagen_29.PNG)

- Editar hosts

![Editar hosts](./img/Imagen_30.PNG)

![Editar hosts](./img/Imagen_31.PNG)

### 3.5 Desplegando la aplicación desde el navegador

Si todo ha ido bien, al teclear nuestro dominio en el navegador, nos aparecerá la pantalla de bienvenida del instalador de Wordpress:

![Escribir dominio](./img/Imagen_32.PNG)

Hacemos clic en ‘Vamos a ello!’ para iniciar el proceso, rellenando los datos que nos pide:

![Rellenar datos](./img/Imagen_33.PNG)

Si ha conseguido conectar con la base de datos y no ha habido ninguna incidencia, nos mostrará un mensaje de confirmación para iniciar la instalación:

![Ver mensaje](./img/Imagen_34.PNG)

Ahora nos solicitará información de personalización de nuestro sitio web y la creación de un usuario administrador del CMS:

![Rellenar datos](./img/Imagen_35.PNG)

Una vez hecho esto, le damos a ‘Instalar Wordpress’ y el proceso se realizará de forma automática. Al finalizar nos mostrará un mensaje de confirmación:

![Confirmar instalación](./img/Imagen_36.PNG)

Ya podemos entrar en el panel del CMS pulsando en el botón ‘Acceder’.

![Acceder a Wordpress](./img/Imagen_37.PNG)

![Ver menú](./img/Imagen_38.PNG)

![Ver página web](./img/Imagen_39.PNG)