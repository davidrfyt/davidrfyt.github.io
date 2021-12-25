---
layout: single
title: Crea tu propio reverse proxy
excerpt: "Un reverse proxy como indica el nombre es un proxy y este sirve para esconder la IP real de tu servidor. Es decir si alguien le hace un ping a tu dominio devolverá tu IP de servidor. Instalándole un proxy devolverá la IP del proxy y nunca la IP original.

Esto puede servidor para evitar ataques directos a tu servidor, para evitar denuncias DMCA en webs de películas, etc.

Esta guía esta orientada para el sistema operativo CentOS 7, nos conectaremos como root y seguiremos estos pasos."
date: 2021-12-25
classes: wide
header:
  teaser: /assets/images/12_2021/reverse_proxy.png
  teaser_home_page: true
  icon: /assets/images/linux.png
categories:
  - LINUX
tags:
  - LINUX, REVERSE PROXY, PROXY
---

![](/assets/images/12_2021/reverse_proxy.png)

Un reverse proxy como indica el nombre es un proxy y este sirve para esconder la IP real de tu servidor. Es decir si alguien le hace un ping a tu dominio devolverá tu IP de servidor. Instalándole un proxy devolverá la IP del proxy y nunca la IP original.

Esto puede servidor para evitar ataques directos a tu servidor, para evitar denuncias DMCA en webs de películas, etc.

Esta guía esta orientada para el sistema operativo CentOS 7, nos conectaremos como root y seguiremos estos pasos.

1). Empezaremos instalando el panel de control VestaCP.

```php
yum upgrade -y; yum install -y wget curl curl-devel nano perl perl-core epel-release; curl -O http://vestacp.com/pub/vst-install.sh; bash vst-install.sh --nginx yes --apache yes --phpfpm no --named yes --remi yes --vsftpd no --proftpd no --iptables no --fail2ban no --quota no --exim no --dovecot no --spamassassin no --clamav no --softaculous no --mysql no --postgresql no --hostname server.hostname.tld --email tucorreo@gmail.com --password InsertaUnaPassworld
```

Ejecutaremos este comando en consola, pero antes deberemos cambiar los siguientes datos del comando: --hostname, --email y --password. Este proceso puede durar unos 10 minutos aproximadamente, depende del servidor y la red del mismo.

Cuando lleguemos a la siguiente ventana:

![](/assets/images/12_2021/vesta_1.png)

Presionaremos la tecla Y, continuaremos con el proceso de instalación del panel de control.

Una vez que el panel de control este instalado en la consola nos aparecerán los datos de acceso como el link, el usuario y la contraseña que anteriormente habíamos puesto en el comando de instalación.

Seguidamente, tenemos que modificar las plantillas que trae VestaCP por defecto, para ello ejecutaremos el siguiente comando en consola:

```php
mv /usr/local/vesta/data/templates/web/nginx/ /usr/local/vesta/data/templates/web/nginx.old;mkdir /usr/local/vesta/data/templates/web/nginx/;cd /usr/local/vesta/data/templates/web/nginx/; rm -rf /etc/nginx/conf.d/1* /etc/nginx/conf.d/2* /etc/nginx/conf.d/3* /etc/nginx/conf.d/4* /etc/nginx/conf.d/5* /etc/nginx/conf.d/6* /etc/nginx/conf.d/7* /etc/nginx/conf.d/8* /etc/nginx/conf.d/9* /etc/nginx/conf.d/0*; mv /usr/local/vesta/data/packages /usr/local/vesta/data/packages.old; mkdir /usr/local/vesta/data/packages; wget https://davidrf.es/assets/file/12_2021/default.pkg -O /usr/local/vesta/data/packages/default.pkg;
```

Continuaremos creando la plantilla default.stpl, usaremos el editor vi ejecutando el siguiente comando en consola:

```php
vi /usr/local/vesta/data/templates/web/nginx/default.stpl;
```

Dentro de este archivo copiaremos el siguiente código, recordar cambiar la "IPDESTINOFINALAQUI" por la IP real de tu servidor donde tienes alojado tu dominio.

```php
server {
    listen      *:%proxy_ssl_port% ssl;
    server_name %domain_idn% %alias_idn%;
    ssl_certificate      %ssl_pem%;
    ssl_certificate_key  %ssl_key%;
    error_log  /var/log/httpd/domains/%domain%.error.log error;

    location / {
        proxy_pass      https://IPDESTINOFINALAQUI:443;
        location ~* ^.+\.(%proxy_extentions%)$ {
            root           %sdocroot%;
            access_log     /var/log/httpd/domains/%domain%.log combined;
            access_log     /var/log/httpd/domains/%domain%.bytes bytes;
            expires        max;
            try_files      $uri @fallback;
        }
    }

    location @fallback {
        proxy_pass      https://IPDESTINOFINALAQUI:443;
    }

    include %home%/%user%/conf/web/snginx.%domain%.conf*;
}
```

Haremos lo mismo creando el archivo default.tpl con el siguiente comando:

```php
vi /usr/local/vesta/data/templates/web/nginx/default.tpl;
```

Dentro de este archivo copiaremos el siguiente código, también deberemos cambiar "IPDESTINOFINALAQUI" por la IP real de tu servidor.

```php
server {
    listen      *:%proxy_port%;
    server_name %domain_idn% %alias_idn%;
    error_log  /var/log/httpd/domains/%domain%.error.log error;

    location / {
        proxy_pass      http://IPDESTINOFINALAQUI:80;
        location ~* ^.+\.(%proxy_extentions%)$ {
            root           %docroot%;
            access_log     /var/log/httpd/domains/%domain%.log combined;
            access_log     /var/log/httpd/domains/%domain%.bytes bytes;
            expires        max;
            try_files      $uri @fallback;
        }
    }

    location @fallback {
        proxy_pass      http://IPDESTINOFINALAQUI:80;
    }

    include %home%/%user%/conf/web/nginx.%domain%.conf*;
}
```

Con esto ya tendremos configurado nuestro proxy, ahora quedaría crear el dominio en el panel de control, para ello accederemos a nuestro panel de control que sería
http://IPDELPROXY:8083, una vez conectados nos iremos hacia el apartado WEB y borraremos el dominio que se ha creado automáticamente. Una vez borrado ahora crearemos nuestro dominio dándole al botón de +.

Finalmente, solo nos quedaría poner la IP del proxy a nuestro dominio, depende donde tengas el dominio solo deberemos editar la IP en el registro A por la IP del proxy.

Esta guía ha sido creada por CarlosFrias de ForoBeta, lo que he publicado aquí es un resumen rápido. Podemos encontrar la guía en el siguiente link:

https://forobeta.com/temas/crea-tu-propio-reverse-proxy-para-ocultar-la-ip-del-servidor-final-con-imagenes.787173/

¡Saludos!