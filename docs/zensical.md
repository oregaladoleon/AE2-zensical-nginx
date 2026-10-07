# Zensical y generación de la web estática.

Explicación de la herramienta utilizada para elaborar la documentación interactiva.

## Inicialización del entorno.
Se ha utilizado **uv** para gestionar la instalación local de Zensical:

## Procedimiento realizado.
Procedemos a inicializar **uv** y comprobamos que se crea el directorio correspondiente ~dpl/AE2/src/
~~~bash
~/dpl/AE2$ uv --version
uv 0.12.18 (x86_64-unknown-linux-gnu)

~/dpl/AE2$ uv init
Initialized project `ae2`

~/dpl/AE2$ ll
total 36
drwxrwxr-x 4 oscar oscar 4096 oct  7 14:57 ./
drwxrwxr-x 9 oscar oscar 4096 oct  7 13:14 ../
drwxrwxr-x 8 oscar oscar 4096 oct  7 14:52 .git/
-rw-rw-r-- 1 oscar oscar   75 oct  7 13:15 .gitignore
-rw-rw-r-- 1 oscar oscar 1063 oct  7 13:14 LICENSE
-rw-rw-r-- 1 oscar oscar  350 oct  7 14:57 pyproject.toml
-rw-rw-r-- 1 oscar oscar    5 oct  7 14:57 .python-version
-rw-rw-r-- 1 oscar oscar 3325 oct  7 13:30 README.md
drwxrwxr-x 3 oscar oscar 4096 oct  7 14:57 src/
~~~
Ejecutamos el **tercer commit**:
~~~bash
/dpl/AE2$ git add .

/dpl/AE2$ git commit -m "Inicializamos uv" -m "Ejecutamos *uv init* para crear el directorio del proyecto llamado src/ae2/"
[main 78e37a0] Inicializamos uv
 3 files changed, 20 insertions(+)
 create mode 100644 .python-version
 create mode 100644 pyproject.toml
 create mode 100644 src/ae2/__init__.py

~/dpl/AE2$ git push origin main
Enumerando objetos: 8, listo.
Contando objetos: 100% (8/8), listo.
Compresión delta usando hasta 6 hilos
Comprimiendo objetos: 100% (3/3), listo.
Escribiendo objetos: 100% (7/7), 887 bytes | 221.00 KiB/s, listo.
Total 7 (delta 0), reusados 0 (delta 0), pack-reusados 0
To github.com:oregaladoleon/AE2-zensical-nginx.git
   d190bf9..78e37a0  main -> main
~~~
A continuación, instalamos las dependencias de **Zensical** y se crea el entorno virtual de trabajo **.venv**
~~~bash
~/dpl/AE2$ uv add --dev zensical
Using CPython 3.12.3 interpreter at: /usr/bin/python3.12
Creating virtual environment at: .venv
Resolved 12 packages in 1.62s
      Built ae2 @ file:///home/oscar/dpl/AE2                                                                            Prepared 5 packages in 5.21s
Installed 12 packages in 270ms
 + ae2==0.1.0 (from file:///home/oscar/dpl/AE2)
 + click==8.5.0
 + deepmerge==3.0.1
 + jinja2==3.1.6
 + markdown==3.11
 + markupsafe==3.0.4
 + pathspec==1.1.1
 + pygments==2.21.0
 + pymdown-extensions==12.1
 + pyyaml==6.0.3
 + tomli==2.5.0
 + zensical==0.0.68
~~~
Procedemos a documentar con el **cuarto commit**:
~~~bash
~/dpl/AE2$ git add .

~/dpl/AE2$ git commit -m "Instalación de dependencias Zensical" -m "Se instalan todas las dependencias necesarias y se crea el entorno virtual *.venv*"
[main b82a769] Instalación de dependencias Zensical
 2 files changed, 343 insertions(+)
 create mode 100644 uv.lock

~/dpl/AE2$ git push origin main
Enumerando objetos: 6, listo.
Contando objetos: 100% (6/6), listo.
Compresión delta usando hasta 6 hilos
Comprimiendo objetos: 100% (4/4), listo.
Escribiendo objetos: 100% (4/4), 23.31 KiB | 11.66 MiB/s, listo.
Total 4 (delta 2), reusados 0 (delta 0), pack-reusados 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To github.com:oregaladoleon/AE2-zensical-nginx.git
   78e37a0..b82a769  main -> main
~~~
Creamos el conjunto de directorios y ficheros base de zensical:
~~~bash
~/dpl/AE2$ uv run zensical new .

~/dpl/AE2$ ll
total 128
drwxrwxr-x 7 oscar oscar  4096 oct  7 15:05 ./
drwxrwxr-x 9 oscar oscar  4096 oct  7 13:14 ../
drwxrwxr-x 2 oscar oscar  4096 oct  7 15:05 docs/
drwxrwxr-x 8 oscar oscar  4096 oct  7 15:04 .git/
drwxrwxr-x 3 oscar oscar  4096 oct  7 15:05 .github/
-rw-rw-r-- 1 oscar oscar    75 oct  7 13:15 .gitignore
-rw-rw-r-- 1 oscar oscar  1063 oct  7 13:14 LICENSE
-rw-rw-r-- 1 oscar oscar   405 oct  7 15:02 pyproject.toml
-rw-rw-r-- 1 oscar oscar     5 oct  7 14:57 .python-version
-rw-rw-r-- 1 oscar oscar  3325 oct  7 13:30 README.md
drwxrwxr-x 3 oscar oscar  4096 oct  7 14:57 src/
-rw-rw-r-- 1 oscar oscar 74072 oct  7 15:02 uv.lock
drwxrwxr-x 4 oscar oscar  4096 oct  7 15:02 .venv/
-rw-rw-r-- 1 oscar oscar  2974 oct  7 15:05 zensical.toml
~~~
Procedemos a documentar con el **quinto commit**:
~~~bash
~/dpl/AE2$ git add .

~/dpl/AE2$ git commit -m "Creación de la estructura base de zensical" -m "Se genera el directorio /docs, se crea el fichero de configuración zensical.toml y un fichero index.md"
[main 5fcba77] Creación de la estructura base de zensical
 4 files changed, 431 insertions(+)
 create mode 100644 .github/workflows/docs.yml
 create mode 100644 docs/index.md
 create mode 100644 docs/markdown.md
 create mode 100644 zensical.toml

~/dpl/AE2$ git push origin main
Enumerando objetos: 10, listo.
Contando objetos: 100% (10/10), listo.
Compresión delta usando hasta 6 hilos
Comprimiendo objetos: 100% (7/7), listo.
Escribiendo objetos: 100% (9/9), 4.14 KiB | 385.00 KiB/s, listo.
Total 9 (delta 1), reusados 0 (delta 0), pack-reusados 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:oregaladoleon/AE2-zensical-nginx.git
   b82a769..5fcba77  main -> main
~~~
Comprobamos que se ejecuta correctamente en local arrancando el servidor de desarrollo:
~~~bash
~/dpl/AE2$ uv run zensical serve
Serving /home/oscar/dpl/AE2/site on http://localhost:8000
Build started
No issues found
^CReceived interrupt, exiting
~~~
Observamos en el navegador que introduciendo http://127.0.0.1:8000 en el navegador se visualiza la plantilla por defecto de zensical.


