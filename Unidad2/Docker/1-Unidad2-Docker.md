# Unidad 2: Docker

A veces no basta con tener los scripts, tener una versión distinta de un software puede llevar a resultados diferentes.

![Fig2_Beaulieu.png](Fig2_Beaulieu.png)

**Figura tomada de [Beaulieu-Jones et al Greene (2017)]((https://www.nature.com/articles/nbt.3780)
)**


Las pipelines bioinformáticas ocupan muchos programas distintos que constantemente están renovándose. La probabilidad de que tengamos una versión de algo distinta a la que corrieron los autores originales es muy alta.

Además, instalar software especializado puede no ser trivial, y al instalar un programa bioinformático es común "romper" las dependencias de otro programa o incluso tu sistema operativo. Por eso a la hora de **tener que** hacer una actualización nos dan mido los tiburones:

![https://imgs.xkcd.com/comics/success.png](https://imgs.xkcd.com/comics/success.png)

Una solución a ambos problemas es utilizar un sistema de **contenedores de sofware, como Docker**. 

![https://upload.wikimedia.org/wikipedia/commons/7/79/Docker_(container_engine)_logo.png](https://upload.wikimedia.org/wikipedia/commons/7/79/Docker_(container_engine)_logo.png)

Docker permite poner dentro de un contenedor un software (o varios) junto con todo lo que necesitan para correr: su sistema de archivos, código, herramientas del sistema, librerías, etc, cualquier cosa que normalmente podamos instalarle a un sistema operativo.

Terminología básica:

* Un **contenedor** es una versión de Linux reducida a sus componentes más básicos. 

* Una **imagen** es el software que cargamos en un contenedor. 

* Un **dockerfile** es un script que describe (e instala) el software que pondremos en una imagen, pero esto no incluye sólo el programa en sí, sino también cualquier detalle de la configuración del ambiente y hasta los comandos que queremos corra.

Esto nos permite que un programa corra de manera idéntica sin importar el sistema operativo original del equipo, y hace que la instalación sea independiente de la instalación de otro software.

Los contenedores son **la nueva forma de hacer aplicaciones**.

![Why containers](why_containers.png)

**Figura tomada de la documentación de [Kubernetes](https://kubernetes.io/)**, que es otra forma de hacer contenedores. Su documentación tiene una [muy buena intro a por qué utilizar contendores](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/#why-containers).

En el siguiente link puedes encontrar las instrucciones y primeros pasos para utilizar Docker.

[Instalación en Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce)

[Instalación en Mac](https://docs.docker.com/docker-for-mac/install/)

[Primeros pasos de Docker](https://docs.docker.com/get-started/).

Para probar que Docker está funcionando correctamente deberás correr `docker run hello-world` y obtener:

```
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
78445dd45222: Pull complete
Digest: sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://cloud.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/

```

#### Funcionamiento básico de Docker

La sintaxis de docker es: `docker OPTIONS`. 

Las opciones más importanes de Docker son:

* `pull` una imagen (solo la primera vez)
* `run` la imagen dentro de un contenedor (para crearlo, solo la primera vez)
*  `exit` para salir del contendor
*  `stop` para detener un contenedor 
*  `restart` para reactivar un contenedor
*  `exec` para entrar a un contenedor activo
*  `rm` borrar un contenedor (debes `stop` primero).
*   `rmi` borrar una imagen. 

Veámoslo con un ejemplo:


1) Bajar (*pullear*) una imagen con `pull`. Por ejemplo la última versión de ubuntu: 

```
$ docker pull ubuntu #Baja la última versión de ubuntu 
Using default tag: latest
latest: Pulling from library/ubuntu

5a132a7e7af1: Pull complete 
fd2731e4c50c: Pull complete 
28a2f68d1120: Pull complete 
a3ed95caeb02: Pull complete 
Digest: sha256:4e85ebe01d056b43955250bbac22bdb8734271122e3c78d21e55ee235fc6802d
Status: Downloaded newer image for ubuntu:latest
```
 
Aquí por default bajó la última, pero también hubieramos podido especificar qué versión de ubuntu queríamos, así:  `docker pull ubuntu:14.04`
 
Para revisar hayamos bajado la imagen deseada:

```
$ docker images #Enlista imagenes ya bajadas
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              07c86167cdc4        11 days ago         188 MB
hello-world         latest              690ed74de00f        5 months ago        960 B
docker/whalesay     latest              6b362a9f73eb        9 months ago        247 MB
```    

2) Cargamos la imagen dentro de un contenedor con `run`. Voilá, estamos dentro de un Ubuntu, específicamente dentro de un **contenedor** corriendo Ubuntu.
      
```
$ docker run -it ubuntu:14.04 bash
root@8f36b66384cb:/# 
root@8f36b66384cb:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@8f36b66384cb:/# mkdir Prueba
root@8f36b66384cb:/# ls
Prueba 
```


**Pregunta**: ¿Qué significa el `#` en vez del `$`?

Escribe algunos comandos. Cuando termines `exit` para salir.

Vamos a ver qué contenedores tenemos:

```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS                NAMES
8f36b66384cb        ubuntu              "bash"              5 minutes ago       Exited (0) 3 minutes ago                         eager_brattain
3dd0d21a78b9        ubuntu              "bash"              10 minutes ago      Up 10 minutes                                    gifted_hypatia
e3591a0c4077        ubuntu              "bash"              14 minutes ago      Exited (0) 13 minutes ago                        keen_clarke
1dab978ee569        hello-world         "/hello"            16 minutes ago      Exited (0) 16 minutes ago           
```

Nuestro contenedor es el último. Para volver a entrar a él utilizamos primero `start` y su ID (el texto alfanumérico es el ID del container) y luego `exec`:

```
$ docker start 8f36b66384cb 
8f36b66384cb 
```

Ahora aparecerá en la lista de contenedores activos (`docker ps` SIN `-a`).

```
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                NAMES
8f36b66384cb        ubuntu              "bash"              8 minutes ago       Up 42 seconds                            eager_brattain

```

Y por lo tanto podemos volver a entrar a el con `exec`

```
$ docker exec -it 8f36b66384cb  bash
root@8f36b66384cb:/# 
```

Nota que los cambios que hayas realizado dentro del contenedor continúan existiendo.

```
root@8f36b66384cb:/# ls
Prueba  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```


Si nos salimos (`exit`) y luego queremos detenerlo por completo:

```
$ docker stop a5864268eadd 
```

Si quieres que tu contenedor corra en el background desde un principio, puedes utilizar el flab `-d`.

```
$ docker run -dit ubuntu bash
e236ce647ea0c6513531f44f9880bc3b4c0005f2442dd16ca10125bd305ec31b
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                NAMES
e236ce647ea0        ubuntu              "bash"              12 seconds ago      Up 10 seconds                            hopeful_goodall

```


#### Borrar contenedores e imágenes

Si quieres borrar contenedores o imágenes (son espacio en disco):

* Borrar un contenedor: Primero deterlo con `docker stop CONTAINER_ID` y luego borrarlo con `docker rm CONTAINER_ID`

* Borrar una imagen: `docker rmi -f IMAGE_ID`


#### "Conectar" un contenedor con un directorio dentro del OS nativo, aka "montar un volumen":

Ejemplo con una imagen de Ubuntu.

Corre la imagen de ubuntu dentro de un contenedor, pero **montando un volumen**, es decir un directorio en tu equipo que podrá ser accedido por el contenedor:

```
docker run --rm  -v [RUTA_ABSOLUTA]:/nombredirContenedor -it ubuntu bash
```

Desglozando el comando anterior:

`-v` es la bandera para indicar que queremos que monte un volumen 

`[RUTA_ABSOLUTA]` es la ruta **absoluta** (así que cambiala por la ruta de tu equipo) al directorio de tu equipo a donde quieres darle acceso al contenedor. Para no hacer un chorizo, te recomiendo crear primero por fuera una variable, por ejemplo así:

Asumiendo que estás en `Unidad2/Docker/Prac_docker`.

```
$ pwd
Unidad2/Docker/Prac_docker
$ midirectorio=$(pwd)
$ docker run --rm  -v $midirectorio:/nombredirContenedor -it ubuntu bash

```

 

`:/nombredirContenedor` es el nombre del directorio como quremos que aparezca dentro de nuestro contenedor. El `:/` es para indicar que lo que sigue es el nombre.

4) Explora el volumen que montaste, prueba hacer un archivo. Nota que puedes acceder a el desde tu explorador, es decir todo lo que suceda en ese directorio puedes verlo/modificarlo desde dentro y fuera del contenedor. 

```
root@dd4667e94adb:/# ls

```

## Dockerhub 

Dockerhub es un repositorio parecido a Github, pero especializado en dockerfiles. Cuando utilizas el comando `docker pull` el pull se hace desde Dockerhub. 


#### Dockerfiles

Recordemos que los contenedores de docker son creados a partir de una **imagen**. Dicha imagen puede ser básica, es decir el OS en su forma más simple, pero también puede incluir un software o conjunto de softwares ya instalados y listos para correr, y hasta los comandos que queremos que corra.

Lo anterior se hace a través de un **dockerfile**, es decir un script que describe (e instala) el software que pondremos en una imagen y además incluye cualquier detalle de la configuración del ambiente y hasta los comandos a correr.

Es decir un dockerfile nos permite construir y compartir una imagen especializada en correr el proceso que deseemos.


**Ejemplo de uso de Dockerhub**

Cualquiera puede abrir una cuenta en Dockerhub, esto permite que podamos subir nuestras propias recetas (dockerfiles) y que otros las jalen con `docker pull`.

Para preparar esta clase 

Commit a docker con el nombre del contendor editado (rstudio_21 en este ejemplo) y el nuevo nombre que queremos en nuestra cuenta de Dockerhub, incluir la versión después de `:`.

```
docker commit rstudio_21 aliciamstt/rstudio_wksp2019:v1
```

Si revisamos con `docker images` ahora deberá haber una imagen con el nombre que le dimos en el comando de arriba.

Hacer log in a docker con las credenciales de nuestra cuenta en Dockerhub:


```
docker login
```

Hacemos el push de la imagen:

```
docker push aliciamstt/rstudio_wksp2019:v1
```



### Biocontainers

Veamos un dockerfile como [este](https://github.com/BioContainers/containers/blob/master/fastxtools/0.0.14/Dockerfile) que instala el programa FastXTools en un contenedor.

El dockerfile del ejercicio anterior vive en el [github de Biocontainers](https://github.com/Biocontainers/). Y además en  [DockerHub de Biocontainers](https://hub.docker.com/u/biocontainers/), por lo que podemos hacer un `pull` desde ahí.

Como verás, **[Biocontainers](http://biocontainers.pro/)** es una comunidad que se dedica a crear imágenes para instalar ese software que tanto necesitas. Checa sus [contenedores](https://github.com/Biocontainers/containers).

La imagen base de Biocontainers se llama `biocontainers` y es un ubuntu más varias herramientas básicas de bioinformática. Checa su [dockerfile aquí](https://github.com/BioContainers/containers/blob/master/biocontainers/Dockerfile).

Vamos a bajar la imagen y correr un contenedor:

```
$ docker pull biocontainers/biocontainers
$ docker run -it biocontainers/biocontainers /bin/bash
biodocker@4c1831e1f7f7:/data$
$ curl   
curl: try 'curl --help' or 'curl --manual' for more information

```

**Observaciones y preguntas**:

* En vez de ser root (´#´ al inicio de la línea de comando) como es el default de docker, somos un usuario normal y estamos en un directorio llamado `data`. ¿Con qué líneas del dockerfile se realizó esto?


* La línea 100 del dockerfile dice `VOLUME ["/data", "/config"]` ¿Qué significa esto?

**Ejercicio** ¿Cómo puedo utilizar `docker run` para que el volumen `data` corresponda a un directorio en mi computadora? 

#### Imagenes de sofware bioinformáico

Pero además del contendor básico biocontainers tenemos contenedores con ese sofware que siempre deseamos instalar. Por ejemplo con FastX-Tools y otro con vcftools. Vamos a hacer un `pull`:

```
$ docker pull biocontainers/fastxtools:0.0.14
$ docker pull biocontainers/vcftools:0.1.15 :0.1.15
```

Yo puedo entrar a estos contenedores con `-it /bin/bash` como lo hemos hecho antes, pero también puedo utilizarlo para **solo** correr el programa con un comando concreto. Por ejemplo, mostrar la ayuda:

```
$ docker run -rm biocontainers/vcftools:0.1.15 vcftools -help

VCFtools (0.1.14)
© Adam Auton and Anthony Marcketta 2009

Process Variant Call Format files

For a list of options, please go to:
	https://vcftools.github.io/man_latest.html

Alternatively, a man page is available, type:
	man vcftools

Questions, comments, and suggestions should be emailed to:
	vcftools-help@lists.sourceforge.net

```

o en FastX-tools:

```
$ docker run -rm biocontainers/fastxtools:0.0.14 fastq_to_fasta -h
usage: fastq_to_fasta [-h] [-r] [-n] [-v] [-z] [-i INFILE] [-o OUTFILE]
Part of FASTX Toolkit 0.0.14 by A. Gordon (assafgordon@gmail.com)

   [-h]         = This helpful help screen.
   [-r]         = Rename sequence identifiers to numbers.
   [-n]         = keep sequences with unknown (N) nucleotides.
                  Default is to discard such sequences.
   [-v]         = Verbose - report number of sequences.
                  If [-o] is specified,  report will be printed to STDOUT.
                  If [-o] is not specified (and output goes to STDOUT),
                  report will be printed to STDERR.
   [-z]         = Compress output with GZIP.
   [-i INFILE]  = FASTA/Q input file. default is STDIN.
   [-o OUTFILE] = FASTA output file. default is STDOUT.

```

**Ejercicio**: ve a la página [http://biocontainers.pro/docs/101/running-example/](http://biocontainers.pro/docs/101/running-example/) y lee el ejemplo de cómo usar `blast`. 


### Tip: borrar automáticamente un contenedor cuando acaba de correr

Cada vez que utilizamos `docker run` se **crea** un contendor nuevo. Por ejemplo si corrieras los `docker run` de fastxtools y vcftools de arriba sin el flag `--rm` obtendrías:

```
$ docker ps -a
CONTAINER ID        IMAGE                      COMMAND               CREATED             STATUS                      PORTS               NAMES
5750fafe938a        biocontainers/vcftools     "vcftools -help"      2 seconds ago       Exited (0) 3 seconds ago                        practical_yonath
4845c81d1dc6        biocontainers/fastxtools   "fastq_to_fasta -h"   17 seconds ago      Exited (1) 17 seconds ago                       elastic_villani

```

Esto ocupa espacio en disco y nos llena de contenedores a los que no volveremos a entrar. La solución: borrar un contenedor al salir. 

Esto se hace con el flag `--rm` para que se borre al salir. Además se puede agregar una de estas dos opciones: 

1) Indicandole al contenedor que se salga al terminar de correr, agregando `-c exit` al comandos que queremos que corra. Ejemplo:

```
$ docker run --rm biocontainers/fastxtools:0.0.14 fastq_to_fasta -h -c exit
usage: fastq_to_fasta [-h] [-r] [-n] [-v] [-z] [-i INFILE] [-o OUTFILE]
Part of FASTX Toolkit 0.0.14 by A. Gordon (assafgordon@gmail.com)

   [-h]         = This helpful help screen.
   [-r]         = Rename sequence identifiers to numbers.
   [-n]         = keep sequences with unknown (N) nucleotides.
                  Default is to discard such sequences.
   [-v]         = Verbose - report number of sequences.
                  If [-o] is specified,  report will be printed to STDOUT.
                  If [-o] is not specified (and output goes to STDOUT),
                  report will be printed to STDERR.
   [-z]         = Compress output with GZIP.
   [-i INFILE]  = FASTA/Q input file. default is STDIN.
   [-o OUTFILE] = FASTA output file. default is STDOUT.
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

2) El flag `-c`  en realidad sirve para pedirle que corra más de un comando dentro del mismo contenedor (unidos por ejemplo con `|`, `;`, etc) Si corres el contenedor con `bash` y los comandos deseados entre "" automáticamente se saldrá (sin tenerle que decir `exit`) al terminar de correr todos los comandos. Ejemplo:

```
$ docker run --rm biocontainers/fastxtools:0.0.14 bash -c "fastq_to_fasta -h ; echo hola mundo"
usage: fastq_to_fasta [-h] [-r] [-n] [-v] [-z] [-i INFILE] [-o OUTFILE]
Part of FASTX Toolkit 0.0.14 by A. Gordon (assafgordon@gmail.com)

   [-h]         = This helpful help screen.
   [-r]         = Rename sequence identifiers to numbers.
   [-n]         = keep sequences with unknown (N) nucleotides.
                  Default is to discard such sequences.
   [-v]         = Verbose - report number of sequences.
                  If [-o] is specified,  report will be printed to STDOUT.
                  If [-o] is not specified (and output goes to STDOUT),
                  report will be printed to STDERR.
   [-z]         = Compress output with GZIP.
   [-i INFILE]  = FASTA/Q input file. default is STDIN.
   [-o OUTFILE] = FASTA output file. default is STDOUT.

hola mundo
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

Si te quedan dudas sobre Docker y cómo aplicarlo a Bionformática revisa esta excelente [sección de ayuda de Biocontainers](http://biocontainers.pro/docs/101/intro/).



### Repaso y conceptos clave

* Utilidad de docker: 

1 ) Máxima reproducibilidad, al igualar OS y versiones de un software.

2) No romper tu equipo/otroprograma tratando de instalar algo


* Docker permite correr un programa en un *contenedor* a partir de una *imagen* que podemos `pull` desde un repositorio online, como Biocontainers o DockerHub.

* Dentro de un contenedor está el OS, las librerías y otros requisitos que requiera un programa y el programa mismo.

* Para "conectar" un contenedor de Docker con el mundo necesitas **montar un volumen**: `docker run -v [RutaABSOLUTAaldirectorioDeseado:/nombrevolumen] [imagen] [comandos a correr].` 

* Formas para correr un software bioinformático usando Docker:

1) **Entrando a un contenedor con todo lo que queremos**: suponiendo que en un contenedor vayas instalando todo el sofware que necesitas para un análisis dado. Los comandos base que necesitas para esto serían: 

`docker pull [IMAGEN]` para bajar la imagen base donde trabajaras

 `docker run -v [-v [RutaABSOLUTAaldirectorioDeseado:/nombrevolumen]] -it [IMAGEN] bash` para crear y correr el contenedor con la imagen de forma interactiva ("entrando") y con un volumen montado a un directorio de nuestra compu donde queramos escribir/leer datos. Ejemplo: `docker run -v /Users/ticatla/hubiC/Science/Teaching/Mx/BioinfInvgRepro/BioinfinvRepro/Unidad4/Prac_Uni4/DatosContenedor1:/DatosContenedorEjercicioClase -it ubuntu bash`
 
  OJO: cada vez que haces `docker run` se **crea** un contenedor **distinto** a partir de la misma imagen. Para **no llenarte de contenedores** utiliza `docker run --rm`.
 
 `docker restart IDCONTAINER` para prender de nuevo el contenedor, NO `docker pull` de nuevo.
 
 `docker exec -it IDCONTAINER bash` para volver a entrar al contenedor. 
 
 
 `docker rm` para borrar un contenedor que ya no quieras. Debes `docker stop` si está corriendo.
 
 
 
 
 2) **Utilizando un contenedor por proceso** (Recomendado) suponiendo que el sofware que utilizas ya vive en una imagen, por ejemplo de Biocontainers. Los comandos base que utilizarías para esto serían:
 
 `docker pull [biocontainers/IMAGEN]` para bajar la imagen de cada sofware que utilizarás, por ejemplo de Biocontainers.
 
 `docker run --rm -v [RutaABSOLUTAaldirectorioDeseado:/data]  [biocontainers/IMAGEN] [COMANDOS del sofware en cuestión] -c exit` para correr el contenedor de una imagen de biocontainers con los comandos específicos de un software dado, con volumen montado a un directorio de nuestra compu donde queramos escribir/leer datos y de tal forma que el contenedor se borre automáticamente al terminar el proceso. Ejemplo:
 
`docker run --rm -v /Users/ticatla/hubiC/Science/Teaching/Mx/BioinfInvgRepro/BioinfinvRepro/Unidad4/Prac_Uni4/DatosContenedor1/datos:/data biocontainers/fastxtools:0.0.14 bash -c "fastx_trimmer -f 1 -l 70 -i human_Illumina_dataset.fastq -v | fastq_quality_filter -q 20 -p 90 -o clean_human_data.fastq -v ; exit"`


(Estos datos ejemplo vienen de [Galaxy Data Libraries](https://usegalaxy.org/library/list#folders/F5bee13e9f312df25/datasets/99fa250d93e003f7) y son de libre uso)

**RECOMENDACIONES**


* Declara una variable con la ruta absoluta al inicio de tu script y luego utilizala dentro de tu línea de `docker run -v`. 

**Ejercicio**: corre el ejemplo anterior (el de limpiar `human_Illumina_dataset.fastq`) utilizando una variable declarada antes de `docker run...` para ajustando la ruta absoluta a tu equipo. 









