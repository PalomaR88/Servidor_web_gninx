# Servidor web nginx

## Instalación de nginnx
~~~
sudo apt update
sudo apt install nginx
~~~

## Instroducción a virtual hosting con nginx
Podemos encontrar los sitios disponibles y activos del servidor nginx, siguiendo la filosofía de Apache2 en los directorios:
~~~
/etc/nginx/sites-availables/
/etc/nginx/sites-enabled/
~~~

Para activar un nuevo sitio tenemos que crear el enlace directo en el directorio **/etc/nginx/sites-enabled/**:
~~~
ln -s /etc/nginx/sites-available/test.com /etc/nginx/sites-enabled/
~~~

En el fichero **/etc/nginx/sites-availables/default** nos encontramos la directiva:
~~~
listen 80 default_server;
~~~

default_server; nos permite indicar el sitio virtual por defecto.
Migrando configuración de Apache2 a nginx

Ejemplo de configuración básdica de un sitio virtual en apache2:
~~~
<VirtualHost *:80>
	ServerName www.example.com
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html

    <Directory /var/www/html/prueba>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride None
        Require all granted
    </Directory>

    Alias /doc/ "/usr/share/doc/"
	<Directory "/usr/share/doc/">
	    Options Indexes MultiViews FollowSymLinks
	    AllowOverride None
	    require ip 127.0.0.0/255.0.0.0 ::1/128
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
~~~

Si traducimos la misma configuración en un servidor nginx:
~~~
server {
    listen 80;

    root /var/www/html;
    index index.html index.htm;

    server_name www.example.com;

    location / {
        try_files $uri $uri/ /index.html;
    }

	location /prueba/ {
        alias /var/www/html/prueba;
        autoindex on;
        allow all;
    }

    location /doc/ {
        alias /usr/share/doc/;
        autoindex on;
        allow 127.0.0.1;
        deny all;
    }
}
~~~

Podríamos resumir las correspondencias en el siguiente cuadro:
~~~
Apache                                 Nginx
------                                 ------
<VirtualHost *:80>                     server {
                                            listen 80;
ServerName yoursite.com	      	       server_name www.yoursite.com;

DocumentRoot /path/to/root             root /path/to/root;

AllowOverride All                      (No Available Alternative)

DirectoryIndex index.php               index index.php;

ErrorLog /path/to/log                  error_log /path/to/log error;

CustomLog /path/to/log combined        access_log /path/to/log main;

Alias /url/ "/path/to/files"           location /url/ {
<Directory "/path/to/files">                 alias /path/to/files;

Options Indexes                        autoindex on

Require all granted                    allow all

allow 127.0.0.1                        allow 127.0.0.1;
deny all                               deny all;
~~~

# PRACTICA
El objetivo de esta práctica es la puesta en marcha de dos sitios web utilizando el mismo servidor web ginx. Hay que tener en cuenta lo siguiente:

    Cada sitio web tendrá nombres distintos.
    Cada sitio web compartirán la misma dirección IP y el mismo puerto (80).

Queremos construir en nuestro servidor web apache dos sitios web con las siguientes características:

- El nombre de dominio del primero será www.iesgn.org, su directorio base será /var/www/iesgn y contendrá una página llamada index.html, donde sólo se verá una bienvenida a la página del Instituto Gonzalo Nazareno.
- En el segundo sitio vamos a crear una página donde se pondrán noticias por parte de los departamento, el nombre de este sitio será www.departamentosgn.org, y su directorio base será /var/www/departamentos. En este sitio sólo tendremos una página inicial index.html, dando la bienvenida a la página de los departamentos del instituto.


~~~
vagrant@servidorginx:~$ sudo nano /etc/nginx/sites-available/iesgn.org
~~~

~~~
server {
    listen 80;

    root /var/www/html/iesgn;
    index index.html index.htm;

    server_name www.iesgn.org;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
~~~

~~~
vagrant@servidorginx:~$ sudo ln -s /etc/nginx/sites-available/iesgn.org /etc/nginx/sites-enabled/
vagrant@servidorginx:~$ sudo ln -s /etc/nginx/sites-available/departamentsgn.org /etc/nginx/sites-enabled/
~~~


