# Unidad 1: Linux y bash


## Línea de comando

* El código son las instrucciones escritas **para una computadora** en un **lenguaje de cómputo**, donde "palabra" es un **comando**.
* Paso por paso hasta una solución.
* El código puede ser muy largo y formar un programa (**software**) entero o de una sóla línea para realizar una única operación.
* Escribir código para que lo ejecuten las computadoras y **comentado** para seres humanos.

El código se alimenta a una computadora a través de la **Terminal**. Debe ser un ícono parecido a este. En Ubuntu debe estar por default en tu dock. Si no lo encuentras tanto en Mac como en Ubuntu prueba buscar "Terminal" o "Console".

`$` significa que la terminal está corriendo con un interpretador Shell o Bash y por un usuario sin mayores privilegios. Si termina en `#` significa que la estás corriendo como **root** que es un "súper usuario" con permisos para desconfigurarlo todo, ten cuidado.

En la Terminal no existe el *point and click*. El que funcione como una Línea de Comando significa que tienes que darle comandos de qué hacer línea por línea.

Por ejemplo:

`date` nos responde la fecha actual

`echo algo` nos responde el texto "algo". También lo puedes utilizar con más de una palabra.

`echo hello world`

Podemos utilizar la Terminal para entrar y hacer todo nuestro trabajo en un servidor remoto (o sea una computadora que está en otro lugar) a través dle comando `ssh` (secure shell). Para este curso, escribe en tu Terminal:

```
ssh -Y bioinfo1@genoma.med.uchile.cl
```

Y luego pon la contraseña que te daremos en clase.

Notarás que tu nombre de usuario y el nombre del equipo cambiaron ¿por qué?

```
Last login: Sun Jul 14 17:07:34 2019 from pc-60-181-46-190.cm.vtr.net
bioinfo1@genoma1:~$
```

No nos detendremos mucho a ver los comandos básicos de bash, pero te recomiendo estos cursos en línea (gratis) si necesito practicarlo:

* [Learn the Command Line de CodeAcademy](https://www.codecademy.com/learn/learn-the-command-line).

* [Juego en el bosque de bash](http://web.mit.edu/mprat/Public/web/Terminus/Web/main.html).


## Práctica: Funciones básicas de navegación y manejo de archivos con bash

1. Determina cuál es la ruta al working directory donde te encuentras al hacer el log in con `pwd`

2. Enlista los archivos y directorios presentes, de modo tal que podamos ver su tamaño en un formato entendible para humanos con `ls -lh`

3. Crea un directorio con la primera letra de tu nombre seguido de tu apellido con `mkdir tunombredeusuario`

4. Entra a dicho directorio con `cd`


#### Notas sobre el uso de `cd`

* **Absolute path** que es dar la ruta (dirección) completa **desde root** hasta el directorio que queremos. Ejemplo:

 `bioinfo1@genoma2:~$ cd /home/bioinfo1/amastretta`

* **Relative path** que es dar la ruta **desde donde estamos** hasta el directorio que queremos. Ejemplo:

`bioinfo1@genoma2:~$ cd amastretta`

* Para moverse un directorio arriba se utiliza `../` (cuantas veces sea necesario según los niveles que se quiera subirse)

* Para no moverse sino solo referise al directorio actual, se usa: `./`


5. Estando **dentro** de tu directorio de usuario, utilizando una ruta relativa y `cp` **copia** el archivo `tangananica.txt` que está un nivel arriba a tu directorio.

6. Revisa el contenido de `tangananica.txt` con `less`, `cat` o `head`.

7. Cuénta cuántas palabras hay en `tangananica.txt` con `wc`.

8. Agrega una línea nueva a `tangananica.txt` con `echo` y `>>`.
