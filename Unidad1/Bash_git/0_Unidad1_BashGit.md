# Unidad 1: Bash y github


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


## Práctica: Funciones básicas de navegación y manejo de archivos con bash

No nos detendremos mucho a ver los comandos básicos de bash, pero te recomiendo estos cursos en línea (gratis) si necesitas practicarlo:

* [Learn the Command Line de CodeAcademy](https://www.codecademy.com/learn/learn-the-command-line).

* [Juego en el bosque de bash](http://web.mit.edu/mprat/Public/web/Terminus/Web/main.html).

Ejercicios:

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


## git y github

[`git`](https://git-scm.com/) es un programa que sirve para llevar el control de versiones de un proyecto informático. Puedes entenderlo como una mezcla de control de cambios de word con el versionamiento de documentos de GoogleDrive para saber qué cambios hiciste a un script, por qué y cuándo, de manera que es más fácil mantener el orden, "volver al pasado" y trabajar en paralelo con colegas.

Como introducción a `git` recomiendo revisar los principales conceptos y el [flujo de trabajo de Github leyendo esta documentación](https://guides.github.com/introduction/flow/).

Y es muy buena idea leer [An Intro to Git and GitHub for Beginners (Tutorial) de Meghan Nelson](https://product.hubspot.com/blog/git-and-github-tutorial-for-beginners).

Otra documentación útil:

* [Learn Git Branching](https://learngitbranching.js.org/) para volverse chidos manejando el ramerío.

* [A successful Git branching model de VincentDriessen](http://nvie.com/posts/a-successful-git-branching-model/). Excelente.


## Github
Es un repositorio de código que:

* Utiliza `git` para llevar un sistema de **control de versiones**,
* Tiene una interfase Web pública
* Permite escribir/revisar código en equipo
* Su símbolo es un gatopulpo.

![](Octocat.png)


El repositorio de este curso está alojado en GitHub.

Lo primero que hay que hacer es este tutorial: [Hello-world Github Guide](https://guides.github.com/activities/hello-world/) para aprender a crear un repo en Github y utilizar su versión web.


### Los términos más importantes

+ **Repositorio**: Se usa para organizar un proyecto. Puede contener imágenes, código, etc. Es recomendable incluir un README.

+ **Fork**: Se crea un fork cuando el repositorio es copiado de la cuenta de un miembro de Github a la de otro.

+ **Branch**: El repositorio tiene una rama o branch principal llamada `master`, que es la "original". Se pueden crear otras ramas dentro del mismo repositorio en las cuales se pueden hacer modificaciones sin afectar el código original. Es el equivalente tener un archivo original `Tesis` y ponerle `Tesis_comentariosAsesora1` y `Tesis_comentariosAsesor2` a los archivos con los comentarios de tus asesores, mismos que eventualmente volverás a integrar en un archivo final (pero `git` lo hace todo más hermoso y organizado).

+ **Commit**: Equivale a guardar los cambios **en git** que no es lo mismo que en el archivo. ¡Ojo! Los cambios se guardan en la branch donde trabajas. Puedes acompañar el commit de un mensaje corto para especificar qué cambios hiciste. Esto es mucho mejor que tener nombres de archivos larguísimos tratando de explicar qué versión son (e.g. `Tesis_final_comentariosAMY_DP_rev22oct2017_comentariosFran_revEnero2018_FINAL_BUENO_corrected_2.doc`).

+ **push:** para enviar los commits locales al repo online.

Piensa en `push` para enviar y `pull` para recibir.

+ **Pull request**: Si se quieren agregar las modificaciones en la branch `master`, se envía una solicitud al propietario original. Es decir tú no haces `push`, le pides al propietario que haga `pull`.

+ **Merge**: Una vez que el propietario del repositorio ha revisado y aceptado los cambios, fusiona las ramas.

### Los comandos principales de `git`

1. `git clone` bajar un repositorio de Github a tu compu

2.  `git status` dentro del directorio de tu repo para ver si hay cambios.

3.  `git diff nombrearchivo` para ver las modificaciones que se hicieron a un archivo desde el último commit.

4. `git add nombrearchivo` (para un archivo) o `git add *` (para todos los archivos) para agregar los archivos **que queremos incluir en un commit**. Como el equivalente a "adjuntarlos" en un correo que te enviarías por correo.

5.  `git commit -m "mensaje corto explicando qué contiene el commit"`. Como el contenido de un correo donde te explicarías a tí mismx qué cambios hiciste que ameritan guardar la versión ("commit").

5. `git push origin master` para enviar tus commits al repositorio de Github

#### `git clone`
Te permite copiar un repositorio que ya existe. Cada versión de cada archivo de la historia del proyecto es descargado cuando lo ejecutas. La dirección del repo que quieres clonar puedes conseguirla en el botón verde que dice "Clone or Download" en la página principal del repo en Github.

**Ojo con dónde corres `git clone`, pues tu working directory será el lugar a donde "se baje" el repo que estás clonando.**

**Ejercicio** Clona el repositorio del curso **dentro** de tu directorio.

```{bash}
$ git clone https://github.com/u-genoma/wksp2019.git
Initialized empty Git repository in /home/bioinfo1/amastretta/wksp2019/.git/
remote: Enumerating objects: 44, done.
remote: Counting objects: 100% (44/44), done.
remote: Compressing objects: 100% (34/34), done.
remote: Total 44 (delta 8), reused 29 (delta 3), pack-reused 0
Unpacking objects: 100% (44/44), done.
```


Para poder hacer los siguientes comandos debemos estar en el directorio del repo. Es decir lo que acabamos de bajar. Así que `cd wksp2019`.

#### `git status`
Es para saber en qué branch estas trabajando y si tienes archivos que te falte "guardar" (commit). Por ejemplo, si lo haces cuando acabas de clonar un repositorio, debe verse algo así:

```{bash}
$ git status                           
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working tree clean
```

#### `git add`
Te permite agregar un archivo que no existía en el repositorio o prepara las modificaciones a archivos existentes. Esto no lo "guarda" (commit), solo hace que "lo sigas". Si modificas un archivo es necesario que vuelvas a dar `add`.

```{bash}
$ touch ejemplo.txt
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	ejemplo.txt

nothing added to commit but untracked files present (use "git add" to track)
$ git add ejemplo.txt
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#	new file:   ejemplo.txt
#
```


#### `git commit`
Confirma y agrega los cambios a la branch en la que estas trabajando. Utiliza la flag `-m` para escribirun mensaje breve. Si no lo haces se abrirá un editor de texto donde puedes describir brevemente el cambio que hiciste. Si tu editor es Vim, puedes guardar y salir con `:wq`.

```
$ git commit -m "agregar archivo ejemplo"
[master 79fce15] agregar archivo ejemplo
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 ejemplo.txt
```

#### `git diff`
Para ver los cambios que se hicieron a un archivo.

```{bash}
$ echo "el mundo es bello" > ejemplo.txt
$ cat ejemplo.txt
el mundo es bello
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   ejemplo.txt

no changes added to commit (use "git add" and/or "git commit -a")
$ git diff ejemplo.txt
diff --git a/ejemplo.txt b/ejemplo.txt
index e69de29..0dc4fee 100644
--- a/ejemplo.txt
+++ b/ejemplo.txt
@@ -0,0 +1 @@
+el mundo es bello
```



#### `git rm`
Si quieres borrar un archivo **que ya había formado parte de un commit** no sólo de tu compu sino del sistema de versiones de git, lo mejor es NO utilizar `rm`, sino `git rm`. Ejemplo:

```
$ touch ejemplo2.txt
$ git add ejemplo2.txt
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   ejemplo2.txt
$ git add ejemplo2.txt
$ git commit -m "added ejemplo2"
$ git rm ejemplo2.txt
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	deleted:    ejemplo2.txt

```

#### `git push`

Una vez que quieres integrar tus cambios a una rama, este comando te permite fusionar ramas. Debes decirle el origen (rama donde hiciste los commits) y el destino (por ejemplo master u otra rama).

**Ojo** uds no podrán hacer `push` porque no son propietarios de este repo. Para ello deberían hacer primero un `pull request`. Más adelante haremos ejercicios de esto.


```
$ git push origin master
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 285 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/AliciaMstt/Repo_chocolate.git
   72129b3..79fce15  master -> master
```

Nota: puedes agregar la flag `-u` para establecer `origin master` (o lo que sea) como el default y solo tener que hacer `git push` en un futuro.


#### `git pull`
Actualiza la copia del repositorio local con respecto a la rama remota.

```
$ git pull    
Already up-to-date.
```

Pero ojo, antes de andar con `pull` por la vida [checa las bondades de `git fetch`:

#### `git fetch`

Si vas a trabajar con repos de otras personas problablemente no quieras hacer un `merge` en automático (que es lo que hace `pull` tras bambalinas) con tu repo local, sino que solo quieras jalar los cambios que hayan hecho otros. Por ejemplo los archivos que agregue a este repo sin que borre lo que tu hayas hecho en tu versión. [Para evitar posibles problemas asociados a esto se recomienda usar `fetch`]((https://help.github.com/articles/fetching-a-remote/)).

[Otra referencia de fetch vs pull](https://longair.net/blog/2009/04/16/git-fetch-and-merge/)

Voy a hacer unos cambios en el archivo `ejemplo.txt` desde el editor de texto de Github y comitearlo (sí, espanglish del chido) online.


### Configurando nuestro git local con Github

(Lo siguiente no lo haremos en clase pues todxs estamos utilizando el servidor con un mismo usuario).

Para poder vincular tu `git` con tu cuenta de Github necesitas  **asociar tu dirección de correo electrónico principal de Github con tu git local**. Además puedes cambiar tu nombre de usuario, pero lo que realmente te vincula con Github es tu correo.

Para cambiar tu correo necesitas seguir cualquiera de estos dos métodos:

1) Correr `$ git config --global --edit`

Lo cual abrirá una pantalla de `vim`. Edita tu nombre de usuario y cuenta de correo. Para poder "escribir en vim" presiona `I` (de insertar) donde quieras comenzar a escribir. Recuerda, para guardar y salir, tecla Esc y luego `:wq`.

2) Correr:

`$ git config --global user.email "email@example.com"`

`$ git config user.name "Mi_nombre"`

Donde el texto entre comillas son tus datos.

Comrpueba tu dirección es la correcta con:

`$ git config user.email`

Debe mostrarse tu dirección correcta.

[Referencia de lo anterior](https://help.github.com/articles/setting-your-commit-email-address-in-git/)


## Comodines o el uso de `*` `?` `[]` `{}`

Volvamos a ver el contenido de Maiz dentro del directorio de prácticas:

```
$ cd Unidad1/Prac_bash/Maiz
$ ls 	
nuevos_final.bim	nuevos_final.log
nuevos_final.bed	nuevos_final.fam
```

**Ejercicio**: Necesitamos crear más archivos .bed y .fam para los ejemplos de abajo. Queremos qué se llamen `ejemplo_final.bed` y `ejemplo_final.fam`. ¿Cómo hacerlo?


El resultado del ejercicio anterior debe ser:

```
$ ls
ejemplo_final.bed	nuevos_final.bed	nuevos_final.log
ejemplo_final.fam	nuevos_final.bim
nuevos_final.fam
```

Fácilmente podemos ver que hay varios archivos, y que hay dos que terminan en .bed y dos que terminan en .fam.


¿Y si tuviéramos 1,000 archivos con las terminaciones .bed, .fam, bim pero con diferente prefijo? ¿Cómo contarlos y ver cuántos .bed hay?

### `*`

"Comodín" o *wildcard*. Cualquier texto (uno o más caracteres) a partir (derecha o izquierda) de dónde se ponga el `*`.

Ejemplo:

```
$ ls *.bed
ejemplo_final.bed	nuevos_final.bed
$ ls nuevos*
nuevos_final.bed	nuevos_final.fam
nuevos_final.bim	nuevos_final.log

```


### `?`
Parecido a `*` pero para un sólo carácter.

```
$ ls *.b??
ejemplo_final.bed	nuevos_final.bed	nuevos_final.bim
```

### `[]`

Para agrupar términos de búsqueda. Por ejemplo letras [a-z]

```
$ ls [a-z]*.bed
ejemplo_final.bed	nuevos_final.bed
```

Hay más comodines, puedes explorarlos [aquí](http://tldp.org/LDP/GNU-Linux-Tools-Summary/html/x11655.htm).


## Loops con bash

Los *for loops* permiten **repetir** una serie de comandos con diferentes *variables de una lista*.


Sintaxis:

```
for i in list; do
 command1
 command2
 ..
done
```

"i" puede leerse como "el elemento i de la lista". Y  la lista no es más que el conjunto total de las variables que queremos.

Ejemplo:

```
$ for i in adenina citosina guanina timina; do
> echo "La $i es una base nitrogenada"
> done

La adenina es una base nitrogenada
La citosina es una base nitrogenada
La guanina es una base nitrogenada
La timina es una base nitrogenada
```


**Observaciones importantes:**

* Los elementos de la lista NO se separan por comas (en otros lenguajes sí).
* Para referirnos al "elemento i" dentro de los comandos debemos usar como prefijo el símbolo `$`.
* No tienes que escribir el `>` antes de `echo` y de `done`, los pongo solo para mostrar que eso aparece en la terminal hasta que terminemos de meter los comandos que formarán parte del loop. De hecho `done` sirve para decir "ok, aquí termina el loop". En los ejemplos de abajo ya no lo pondré.

Otro ejemplo:

```
for perro in labrador "pastor mesoamericano" xolo; do
echo Mi mejor amigo es un $perro; done

Mi mejor amigo es un labrador
Mi mejor amigo es un pastor mesoamericano
Mi mejor amigo es un xolo
```

**Preguntas**

1) ¿Cuándo debo usar comillas en la lista de elementos?

2) ¿Qué hace `;`?

Y un ejemplo más:

```
for i in {1..100};do
mkdir directorio$i; done
```

Lo cual hará 100 directorios, llamados directorio1, directorio2 y así.

**Pregunta** ¿De qué manera sencilla borrarías esos 100 directorios que acabas de crear?

Más información de for loops: Aquí presenté la sintaxis más usada, pero hay otros métodos para escribir loops que hacen lo mismo. Y también pueden hacerse más complejos agregando "ifs".
Puedes consultar esta y más info de for loops en [esta guía con ejemplos y varios formatos](http://www.thegeekstuff.com/2011/07/bash-for-loop-examples/).


### Definir variables

Los for loops utilizan *variables* definidas por el usuario, es decir "i" y "perro" en los ejemplos anteriores. Sin embargo, también pueden crearse variables **afuera** de un for loop, y usarlas para lo que queramos.

Ejemplo:

```
$ varx=2
$ $varx
-bash: 2: command not found

```

**Observaciones importantes**
* NO debe haber espacios entre el símbolo = y la variable o su valor.
* El nombre de la variable puede ser cualquier cosa que queramos **MENOS** el nombre de un comando que exista.

Las variables se pueden usar para acortar algo que escribamos muy seguido (como un path o un nombre de archivo largos) y conjuntar con otras variables dentro de un loop.

Ejemplo:

```
$ maullido=miau
$ for i in gato gatito gatón; do
> echo El $i hace $maullido
> done
El gato hace miau
El gatito hace miau
El gatón hace miau
```
