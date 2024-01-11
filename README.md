# Balanceo de carga con Nginx

## Instalación del Servidor Nginx

Primero, deberás instalar Nginx en todos los servidores. Puedes hacerlo con el siguiente comando:

```bash
# apt-get install nginx -y
```
Una vez instalado, inicia el servicio Nginx y habilítalo para que se inicie al reiniciar el sistema:

```bash
# systemctl start nginx
# systemctl enable nginx
```
## Configuración de los Servidores de Aplicaciones

A continuación, es necesario ajustar la configuración en ambos servidores de aplicaciones.

* En el primer servidor de aplicaciones, procede a eliminar el archivo `index.html` predeterminado y crear uno nuevo:

```bash
# rm -rf /usr/share/nginx/html/index.html
# nano /usr/share/nginx/html/index.html
```
Dentro del editor, agrega las siguientes líneas:

```bash
html
<html>
<title>Servidor de Aplicación Primario</title>
<body>
Esta es mi primera aplicación en el servidor
</body>
</html>
```
Guarda los cambios y cierra el archivo.
* Ahora, en el segundo servidor de aplicaciones, realiza el mismo proceso:

```bash
# rm -rf /usr/share/nginx/html/index.html
# nano /usr/share/nginx/html/index.html
```
Dentro del editor, agrega las siguientes líneas:

```bash
html
<html>
<title>Servidor de Aplicación Primario</title>
<body>
Esta es mi primera aplicación en el servidor
</body>
</html>
```
Guarda los cambios y cierra el archivo.

## Configuración del Servidor de Balanceo de Carga

Para continuar, deberás configurar un servidor balanceador de carga que distribuya la carga entre ambos servidores de aplicaciones. 

Primero, elimina el archivo de configuración predeterminado de Nginx y crea un nuevo archivo de configuración del balanceador de carga:

```bash
# rm -rf /etc/nginx/sites-enabled/default 
# nano /etc/nginx/conf.d/load-balancing.conf
```

Añade las siguientes líneas:

```bash
upstream backend {
    server1 52.91.164.125;
    server2 44.204.91.16;
}

server {
    listen      80;
    server_name jcachaco.ddns.net;

    location / {
        proxy_redirect      off;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    Host $http_host;
        proxy_pass http://backend;
    }
}
```
El nombre de dominio lo registre en NOIP-domain para la prueba

## Protección de Nginx con Let´s Encrypt
## Paso 1: Instalar Certbot

El primer paso para utilizar Let’s Encrypt y obtener un certificado SSL es instalar el software Certbot en tu servidor.

Instala Certbot y su complemento de Nginx utilizando el siguiente comando:

```bash
sudo apt install certbot python3-certbot-nginx
```


