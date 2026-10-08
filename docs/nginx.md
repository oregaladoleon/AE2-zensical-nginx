# Configuración de nginx dentro del contenedor docker **dpl-lab**.
El objeto de este apartado es la instalación de **nginx** dentro del contenedor docker **dpl-lab** y su configuración. Finalmente, procederemos a comprobar desde fuera del contenedor que **nginx** está lanzado y activo.

## Procedimiento realizado.
Instalamos **nginx** en el contenedor **dpl-lab**:
~~~bash
~/dpl/AE2$ sudo docker exec -it dpl-lab apt update
Get:1 http://security.ubuntu.com/ubuntu noble-security InRelease [126 kB]                              
Get:2 http://security.ubuntu.com/ubuntu noble-security/main amd64 Packages [1323 kB]                   
Get:3 http://security.ubuntu.com/ubuntu noble-security/restricted amd64 Packages [1943 kB]
Get:4 http://security.ubuntu.com/ubuntu noble-security/universe amd64 Packages [1545 kB]
Ign:5 http://archive.ubuntu.com/ubuntu noble InRelease                                                                         
Get:6 http://archive.ubuntu.com/ubuntu noble-updates InRelease [126 kB]
Get:7 http://archive.ubuntu.com/ubuntu noble-backports InRelease [126 kB]
Hit:5 http://archive.ubuntu.com/ubuntu noble InRelease                                                                         
Get:8 http://archive.ubuntu.com/ubuntu noble-updates/restricted amd64 Packages [2138 kB]                                       
Get:9 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 Packages [1701 kB]                                             
Get:10 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 Packages [2165 kB]                                        
Fetched 11.2 MB in 1min 47s (105 kB/s)                                                                                         
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
8 packages can be upgraded. Run 'apt list --upgradable' to see them.

~/dpl/AE2$ sudo docker exec -it dpl-lab apt install -y nginx
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  iproute2 libatm1t64 libbpf1 libcap2-bin libelf1t64 libmnl0 libpam-cap libxtables12 nginx-common
Suggested packages:
  iproute2-doc python3:any fcgiwrap nginx-doc ssl-cert
The following NEW packages will be installed:
  iproute2 libatm1t64 libbpf1 libcap2-bin libelf1t64 libmnl0 libpam-cap libxtables12 nginx nginx-common
0 upgraded, 10 newly installed, 0 to remove and 8 not upgraded.
Need to get 2031 kB of archives.
After this operation, 5808 kB of additional disk space will be used.
~~~
Ahora vamos a crear el directorio donde se alojarán los ficheros de la web en el contenedor **dpl-lab** que ejecutará **nginx**:
~~~bash
~/dpl/AE2$ sudo docker exec dpl-lab mkdir -p /var/www/html/ae2

~/dpl/AE2$ sudo docker cp site/. dpl-lab:/var/www/html/ae2/
Successfully copied 709kB to dpl-lab:/var/www/html/ae2/
~~~
A continuación, creamos el archivo de configuración de **nginx** dentro de **~/etc/nginx/sites-available/ae2** y lo editamos con la configuración pertinente, para tabular las rutas absolutas a la hora de introducir los comandos, preferimos entrar en la terminal del contenedor:
~~~bash
~/dpl/AE2$ sudo docker exec -it dpl-lab bash

root@b8c336859c05:/# touch /etc/nginx/sites-available/ae2

root@b8c336859c05:/# nano /etc/nginx/sites-available/ae2
~~~
Añadimos al fichero **/etc/nginx/sites-available/ae2** la siguiente configuración:
~~~
server {
    listen 80;
    server_name localhost;

    root /var/www/html/ae2;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
~~~
Luego, creamos el enlace simbólico desde **/etc/nginx/sites-enabled/** para activar el sitio web, comprobamos la sintaxis de los ficheros en **nginx** y recargamos la configuración.
~~~bash
root@b8c336859c05:/# ln -sf /etc/nginx/sites-available/ae2 /etc/nginx/sites-enabled/

root@b8c336859c05:/# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

root@b8c336859c05:/# nginx -s reload
2026/10/08 10:30:44 [notice] 60#60: signal process started
2026/10/08 10:30:44 [error] 60#60: invalid PID number "" in "/run/nginx.pid"
~~~
Debido al error final de **invalid PID number**, observamos que **nginx** no se está ejecutando en el contenedor, por lo que tenemos que arrancarlo.
~~~bash
oot@b8c336859c05:/# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   4592  3860 pts/0    Ss+  09:58   0:00 /bin/bash
root          44  0.0  0.0   4592  3940 pts/1    Ss   10:23   0:00 bash
root          61  4.3  0.0   7896  3992 pts/1    R+   10:35   0:00 ps aux

root@b8c336859c05:/# service nginx start
 * Starting nginx nginx                                                                             [ OK ] 

root@b8c336859c05:/# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   4592  3860 pts/0    Ss+  09:58   0:00 /bin/bash
root          44  0.0  0.0   4592  3940 pts/1    Ss   10:23   0:00 bash
root          76  0.0  0.0  11164  1696 ?        Ss   10:36   0:00 nginx: master process /usr/sbin/nginx
www-data      77  0.0  0.0  11548  2956 ?        S    10:36   0:00 nginx: worker process
www-data      78  0.0  0.0  11548  2956 ?        S    10:36   0:00 nginx: worker process
www-data      79  0.0  0.0  11548  2956 ?        S    10:36   0:00 nginx: worker process
www-data      80  0.0  0.0  11548  2956 ?        S    10:36   0:00 nginx: worker process
www-data      81  0.0  0.0  11548  2956 ?        S    10:36   0:00 nginx: worker process
www-data      82  0.0  0.0  11548  2896 ?        S    10:36   0:00 nginx: worker process
root          83 50.0  0.0   7896  4132 pts/1    R+   10:36   0:00 ps aux
~~~
Comprobamos desde fuera del contenedor que **nginx** está lanzado:
~~~bash
~/dpl/AE2$ curl -I http://localhost
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Thu, 08 Oct 2026 10:37:49 GMT
Content-Type: text/html
Content-Length: 18413
Last-Modified: Wed, 07 Oct 2026 20:58:24 GMT
Connection: keep-alive
ETag: "6ac6b270-47ed"
Accept-Ranges: bytes
~~~
