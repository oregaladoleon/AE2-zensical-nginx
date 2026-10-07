# AE2-zensical-nginx
> Autor: Óscar Regalado León  
> Fecha: Octubre 2026 

Creación de un repositorio en Git y Github. Simulación de un flujo de trabajo entre dos usuarios, aplicación de: (fork, issue, rama, PR, conflicto, etiqueta y release.

## Índice

- [Entorno e instalación](#entorno-e-instalacion)
- [Configuración](#configuracion)
- [Comprobación](#comprobacion)
- [Problemas encontrados y solución](#problemas-encontrados-y-solucion)
- [Repositorio remoto](#repositorio-remoto)

## Entorno e instalación

En primer lugar accedemos al directorio del módulo ~dpl/ y comprobamos lo contenedores creados:
~~~bash
~/dpl$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED       STATUS                     PORTS     NAMES
f2e4f1d3a079   ubuntu:24.04   "/bin/bash"   2 weeks ago   Exited (137) 2 weeks ago             dpl-cliente
b8c336859c05   ubuntu:24.04   "/bin/bash"   2 weeks ago   Exited (137) 2 weeks ago             dpl-lab
~~~
Levantamos el contenedor de trabajo:
~~~bash
~/dpl$ sudo docker start dpl-lab
dpl-lab
~~~
Comprobamos el estado:
~~~bash
~/dpl$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED       STATUS         PORTS                                                                                                                           NAMES
b8c336859c05   ubuntu:24.04   "/bin/bash"   2 weeks ago   Up 6 seconds   0.0.0.0:80->80/tcp, [::]:80->80/tcp, 0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp, 0.0.0.0:8080->8080/tcp, [::]:8080->8080/tcp   dpl-lab
~~~
Creamos un nuevo repositorio en GitHub:
~~~bash
~/dpl$ gh repo create AE2-zensical-nginx --public --add-readme --license mit
✓ Created repository oregaladoleon/AE2-zensical-nginx on GitHub
  https://github.com/oregaladoleon/AE2-zensical-nginx
~~~
Clonamos el repositorio en nuestro equipo local y creamos un directorio dentro de ~dpl/ llamado AE2/:
~~~bash
~/dpl$ git clone git@github.com:oregaladoleon/AE2-zensical-nginx.git AE2
Clonando en 'AE2'...
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Recibiendo objetos: 100% (4/4), listo.
~~~
Creamos el fichero **.gitignore** dentro del directorio de trabajo AE2/:
~~~bash
~/dpl/AE2$ touch .gitignore
~/dpl/AE2$ nano .gitignore 
~~~
El fichero lo creamos con las siguientes restricciones:
~~~bash
.venv/
__pycache__/
site/
.zensical/
.mkdocs/
*.pyc
.DS_Store
.vscode/
EOF
~~~
Preparamos las modificaciones y las añadimos al repositorio con el **primer commit**
~~~bash
~/dpl/AE2$ git add .gitignore
~/dpl/AE2$ git commit -m "Inicialización repositorio Git y GitHub" -m "Se crea el repositorio AE2-zensical-nginx en GitHub, se clona en local y se añade el fichero .gitignore"
[main 7fb657f] Inicialización repositorio Git y GitHub
 1 file changed, 9 insertions(+)
 create mode 100644 .gitignore
~/dpl/AE2$ git push origin main
Enumerando objetos: 4, listo.
Contando objetos: 100% (4/4), listo.
Compresión delta usando hasta 6 hilos
Comprimiendo objetos: 100% (3/3), listo.
Escribiendo objetos: 100% (3/3), 479 bytes | 119.00 KiB/s, listo.
Total 3 (delta 0), reusados 0 (delta 0), pack-reusados 0
To github.com:oregaladoleon/AE2-zensical-nginx.git
   8011fe0..7fb657f  main -> main
~~~