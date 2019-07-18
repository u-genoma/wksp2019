# Unidad 1: Linux y bash

UNIX es un *sistema operativo* (OS en inglés) es decir el conjunto de programas que controlan las funciones básicas de una compu (agendar tareas, ejecutar aplicaciones y controlar cosas como teclados y micrófonos). UNIX fue desarrollado en la década de 1960 y sigue vivo. No cualquier tecnología puede decir eso. Hay muchos systemas UNIX, los más conocidos son Sun Solaris, GNU/Linux y MacOS X, también los teléfonos Android están basados en UNIX.

Los sistemas UNIX se caracterizan por tener un sistema de archivos unifocado y un lenguaje de **línea de comando** (lo veremos abajo) que combinados pueden hacer operaciones muy complejas. Esto los hace muy poderosos y flexibles, por eso muchos desarrolladores eligen esta familia de OS para hacer software en ciencia. En UNIX [una palabra vale más que mil clicks](http://matt.might.net/articles/basic-unix/).

Los sistemas UNIX también pueden tener una interfaz gráfica, como en Mac, Ubuntu y Biolinux, que son una forma de Linux.  Sin embargo, además de la interfaz gráfica una también puede seguir utilizando la poderosa línea de comando. Windows está construído de otra forma por completo, y por eso es mucho más difícil traducir los programas a ese OS.

Los sistemas UNIX están formados por el *kernel*, los *programas* y la *terminal*, también llamada *consola* o *shell*. El kernel determina el tiempo y memoria que podrá usar cada programa, además de que maneja el sistema de archivos y la comunicación en respuesta a llamadas al sistema. Un programa es un conjunto de comandos que el kernel puede ejecutar. Cuando un programa está siendo ejecutado ("está corriendo") se llama proceso. A través de la consola es que podemos interactuar con el kernel y decirle qué hacer.


## Línea de comando

* El código son las instrucciones escritas **para una computadora** en un **lenguaje de cómputo**, donde "palabra" es un **comando**.
* Paso por paso hasta una solución.
* El código puede ser muy largo y formar un programa (**software**) entero o de una sóla línea para realizar una única operación.
* Escribir código para que lo ejecuten las computadoras y **comentado** para seres humanos.

El código se alimenta a una computadora a través de la **Terminal**. Debe ser un ícono parecido a este. En Ubuntu debe estar por default en tu dock. Si no lo encuentras tanto en Mac como en Ubuntu prueba buscar "Terminal" o "Console".

![](terminalicon.png)

Tu terminal abierta debe verse parecida a esta:

![TerminalEjemplo](01Terminal.png)

La tercera línea dice el **nombre del equipo** (Alicas-MacBook-Pro en este caso), el **directorio** donde estoy (`~` significa "home directory", lo veremos más adelante) y el **nombre del usuario** con quién estoy en la terminal (en mi caso ticatla).

Notarás que la tercera línea termina en `$`

`$` significa que la terminal está corriendo con un interpretador Shell o Bash y por un usuario sin mayores privilegios. Si termina en `#` significa que la estás corriendo como **root** que es un "súper usuario" con permisos para desconfigurarlo todo, ten cuidado.

En la Terminal no existe el *point and click*. El que funcione como una Línea de Comando significa que tienes que darle comandos de qué hacer línea por línea.

Por ejemplo:

`date` nos responde la fecha actual

`echo algo` nos responde el texto "algo". También lo puedes utilizar con más de una palabra.

`echo hello world`


¿Qué pasa si intentas correr el comando  `algo`?:

La terminal dirá:

`-bash: algo: command not found`

Que quiere decir que no existe el comando `algo`.

De igual forma si corres `$ date` (incluyendo el `$`) te dirá que no existe el comando `$`, aunque `date` sí sea un comando.

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

**Pregunta:** ¿Por qué?

A continuación veremos una introdcción a los comandos del día a día de bash, pero si quieres seguir practicando te recomiendo estos cursos en línea (gratis):

* [Learn the Command Line de CodeAcademy](https://www.codecademy.com/learn/learn-the-command-line).

* [Juego en el bosque de bash](http://web.mit.edu/mprat/Public/web/Terminus/Web/main.html).


## Funciones básicas de navegación y manejo de archivos con bash

#### `pwd`

`pwd` nos da el directorio en donde estamos (viene de print **working directory**).

El directorio de trabajo es **dónde estamos**, equivalente a tener una ventana abierta del explorador en una carpeta determinada. Al menos que le indiques lo contrario, todo archivo que se genere como parte de la ejecución de un programa se guardará aquí. También este será el lugar donde cualquier programa/script buscará los archivos que le pidas, y NO los encontrará si no están exactamente ahí. Claro que es posible decirle que busque en otro directorio, lo veremos adelante.  

El working directory de base es la carpeta "home" del usuario en una computadora de escritorio, y en el caso del servidor donde estamos trabajando es la siguiente carpeta compartida:

```
bioinfo1@genoma1:~$ pwd
/shared/bioinfo1
```

### `ls`

Enlista los archivos y directorios presentes en un directorio.

Nota que los enlista en orden alfabético.

Para tener más info de los archivos:

`ls -l` brinda la misma lista, pero con datos sobre:
si es un directorio (d) o un archivo (-), permisos (si es sólo lectura, editable, etc y por quién, detalles más adelante), número de links al archivo, qué usuario es el dueño, a qué grupo pertenece dicho usuario, tamaño en bytes, fecha-hora en que se modificó y el nombre del directorio o archivo.


También se pueden ordenar por fecha (y hora, aunque no se vea) de modificación, por ejemplo:

```
$ ls -lt
total 5216
-rwx------  1 ticatla  staff     1825  6 May  2015 nuevos_final.log
-rwx------  1 ticatla  staff     3604  6 May  2015 nuevos_final.fam
-rwx------  1 ticatla  staff  1551105  6 May  2015 nuevos_final.bed
-rwx------  1 ticatla  staff  1109078  6 May  2015 nuevos_final.bim
```


`man ls` abre el manual de `ls` (**o de cualquier otro comando**), donde vienen muchas más opciones para usar este comando.

**Tip**: presiona "q" para salir de la pantalla de `man`.


### `cd`

`cd` viene de **change directory** y sirve para movernos a otro directorio.

Entra al directorio del curso

```
cd WorkshopUgenoma2019
```

**Pregunta:** ¿Qué pasa con el texto antes del nombre de usuario?

Como se explicó antes, el texto antes de `$` nos indica el nombre del equipo, el directorio actual y el nombre del usuario. El directorio actual cambió de "ticatla" (home) a "Desktop".

La navegación se puede hacer de directorio en directorio, pero también de otras maneras:


Para esto hay varias opciones:

#### Moverse hacia adelante/abajo (i.e. adentro de subdirectorios):

* **Absolute path** que es dar la ruta (dirección) completa **desde root** hasta el directorio que queremos.

* **Relative path** que es dar la ruta **desde donde estamos** hasta el directorio que queremos.

Ejemplo de ruta absoluta:

```
Alicias-MacBook-Pro:~ ticatla$ cd /Users/ticatla/Desktop/BioinfInvRepro/Unidad1/Prac_Uni1
Alicias-MacBook-Pro:Prac_Uni1 ticatla$

```

Ejemplo de ruta relativa:

```
Alicias-MacBook-Pro:Prac_Uni1 ticatla$ cd ~
Alicias-MacBook-Pro:~ ticatla$ cd Desktop/BioinfInvRepro/Unidad1/Prac_Uni1

Alicias-MacBook-Pro:Prac_Uni1 ticatla$
```


#### Moverse para atrás (hacia el directorio raíz):

Igual puede ser con rutas absolutas o relativas, pero utilizando `..` que representa **parent directory**, es decir el directorio arriba (o atrás, como le entiendas mejor):

`cd ..` por lo tanto te lleva a un directorio arriba de donde estés.

```
cd ..
ls
```

### `mkdir`

Crea un directorio.

```
$ mkdir Prueba
$ ls
```

Nota: Se puede combinar con paths absolutos o relativos para crearlo en un directorio diferente al WD.


**Ejercicio:** ¿Qué pasa si intentas crear un directorio que ya existe? ¿Para qué sirve el flag `-p`?

**Ejercicio**  Cera un directorio con la primera letra de tu nombre segudio de tu apellido **dentro** de `WorkshopUgenoma2019`.

### `cp`

Copia un archivo o directorio de lugar A a lugar B.

**Ejerciio** copia el direcotiro `wksp2019` a tu carpeta

```
cp wksp2019 tudirectorio
```

### `mv`


Mueve un archivo o directorio de lugar A a lugar B. Equivalente a "cut-paste".

```
$ mv ../Prueba ../amastretta
$ ls ../
$ ls ../amastretta
```


### `rm`

Borra (**AGUAS al usar esto**) archivos o directorios.

```
$ rm -r Prueba
```

**Pregunta:** ¿Por qué utilizar `-r`?

**Pregunta:** ¿Si borras un directorio se borra su contenido?



### Crear archivos desde la terminal

Es posible crear archivos de texto directamente desde la terminal utilizando programas como `vi` y `nano` o el comando `touch`.


Touch solo crea un archivo sin contenido. Ejemplo :

```
$ touch prueba
$ ls
$ rm prueba
```

Con `Nano` o `Vim` podemos crear archivos y escribir dentro de ellos.

```
$ nano ejemplonano.txt
```

Y abrirá una nueva ventana, donde pueden escribir así:

![nanoej1](nanoej1.png)

Al guardar con `^O` (ctrl + O) pedirá un nombre de archivo. Dirá "ejemplonano.txt" porque fue el nombre que dimos al correr `nano`, pero se puede cambiar.

Una vez fuera de nano podemos verlo en la Terminal y volverlo a abrir si queremos:

```
$ ls
ejemplonano.txt		
$ nano ejemplonano.txt
```

`vim` o `vi` funciona muy parecido. Solo que tienes que aprenderte (o revisar [en un acordeón](https://vim.rtorr.com/)) los shortcuts del teclado. Hay gente que vive solo con vim.



## Funciones básicas de exploración de archivos con bash

### `less`

Nos permite ver el archivo una línea (flecha abajo) o página a la vez (barra espaciadora). Para salir: `q`

Dentro de `less` podemos escribir `/` y luego texto, mismo que será buscando dentro del archivo.

```
$ cd wksp2019/Unidad1/Prac_bash/Maiz
$ less nuevos_final.fam
```

**Ejercicio**: En el archivo que estamos viendo hay unas muestras de teocintles cuyos nombres empiezan con "teos". ¿En qué líneas del documento están?


### `head`

Muestra las primeras líneas de un archivo (default 10).

```
$ head nuevos_final.fam
1 maiz_3 0 0 0 -9
2 maiz_68 0 0 0 -9
3 maiz_91 0 0 0 -9
4 maiz_39 0 0 0 -9
5 maiz_12 0 0 0 -9
6 maiz_41 0 0 0 -9
7 maiz_35 0 0 0 -9
8 maiz_58 0 0 0 -9
9 maiz_51 0 0 0 -9
10 maiz_82 0 0 0 -9
```

### `tail`

Muestra las últimas líneas de un archivo.

**Pregunta:** ¿Para qué podría ser útil ver las últimas líneas de un archivo?

### `wc`
Brinda el número de líneas, el número de palabras y el número de caracteres del archivo.

```
$ wc nuevos_final.fam
     165     990    3604 nuevos_final.fam
```

### `cat`

Viene de *Concatenate*. Sirve para unir uno detrás de otro varios archivos, o para imprimir todo el contendio de un archivo a la consola.

```
$ cat nuevos_final.fam *log
1 maiz_3 0 0 0 -9
2 maiz_68 0 0 0 -9
3 maiz_91 0 0 0 -9
4 maiz_39 0 0 0 -9
5 maiz_12 0 0 0 -9
6 maiz_41 0 0 0 -9
7 maiz_35 0 0 0 -9
8 maiz_58 0 0 0 -9
9 maiz_51 0 0 0 -9
10 maiz_82 0 0 0 -9
11 maiz_67 0 0 0 -9
12 maiz_93 0 0 0 -9
13 maiz_21 0 0 0 -9
14 maiz_6 0 0 0 -9
15 maiz_101 0 0 0 -9
16 maiz_27 0 0 0 -9
17 maiz_43 0 0 0 -9
18 maiz_1 0 0 0 -9
19 maiz_33 0 0 0 -9
20 maiz_100 0 0 0 -9
21 maiz_24 0 0 0 -9
22 maiz_103 0 0 0 -9
23 maiz_72 0 0 0 -9
24 maiz_10 0 0 0 -9
25 maiz_28 0 0 0 -9
26 maiz_49 0 0 0 -9
27 maiz_56 0 0 0 -9
28 maiz_66 0 0 0 -9
29 maiz_52 0 0 0 -9
30 maiz_47 0 0 0 -9
31 maiz_80 0 0 0 -9
32 maiz_65 0 0 0 -9
33 maiz_94 0 0 0 -9
34 maiz_36 0 0 0 -9
35 maiz_26 0 0 0 -9
36 maiz_105 0 0 0 -9
37 maiz_30 0 0 0 -9
38 maiz_16 0 0 0 -9
39 maiz_42 0 0 0 -9
40 maiz_4 0 0 0 -9
41 maiz_31 0 0 0 -9
42 maiz_17 0 0 0 -9
43 maiz_46 0 0 0 -9
44 maiz_5 0 0 0 -9
45 maiz_32 0 0 0 -9
46 maiz_19 0 0 0 -9
47 maiz_50 0 0 0 -9
48 maiz_8 0 0 0 -9
49 maiz_34 0 0 0 -9
50 maiz_23 0 0 0 -9
51 maiz_54 0 0 0 -9
52 maiz_14 0 0 0 -9
53 maiz_37 0 0 0 -9
54 maiz_25 0 0 0 -9
55 maiz_55 0 0 0 -9
56 maiz_40 0 0 0 -9
57 maiz_29 0 0 0 -9
58 maiz_60 0 0 0 -9
59 maiz_44 0 0 0 -9
60 maiz_74 0 0 0 -9
61 maiz_89 0 0 0 -9
62 maiz_64 0 0 0 -9
63 maiz_83 0 0 0 -9
64 maiz_75 0 0 0 -9
65 maiz_92 0 0 0 -9
66 maiz_69 0 0 0 -9
67 maiz_84 0 0 0 -9
68 maiz_76 0 0 0 -9
69 maiz_97 0 0 0 -9
70 maiz_70 0 0 0 -9
71 maiz_85 0 0 0 -9
72 maiz_77 0 0 0 -9
73 maiz_71 0 0 0 -9
74 maiz_86 0 0 0 -9
75 maiz_78 0 0 0 -9
76 maiz_102 0 0 0 -9
77 maiz_73 0 0 0 -9
78 maiz_88 0 0 0 -9
79 maiz_79 0 0 0 -9
80 maiz_106 0 0 0 -9
81 maiz_119 0 0 0 -9
82 maiz_148 0 0 0 -9
83 maiz_108 0 0 0 -9
84 maiz_120 0 0 0 -9
85 maiz_151 0 0 0 -9
86 maiz_134 0 0 0 -9
87 maiz_123 0 0 0 -9
88 maiz_153 0 0 0 -9
89 maiz_135 0 0 0 -9
90 maiz_124 0 0 0 -9
91 maiz_184 0 0 0 -9
92 maiz_140 0 0 0 -9
93 maiz_125 0 0 0 -9
94 maiz_141 0 0 0 -9
95 maiz_131 0 0 0 -9
96 maiz_189 0 0 0 -9
97 maiz_57 0 0 0 -9
98 maiz_126 0 0 0 -9
99 maiz_113 0 0 0 -9
100 maiz_138 0 0 0 -9
101 maiz_63 0 0 0 -9
102 maiz_127 0 0 0 -9
103 maiz_114 0 0 0 -9
104 maiz_139 0 0 0 -9
105 maiz_99 0 0 0 -9
106 maiz_129 0 0 0 -9
107 maiz_116 0 0 0 -9
108 maiz_142 0 0 0 -9
109 maiz_110 0 0 0 -9
110 maiz_132 0 0 0 -9
111 maiz_118 0 0 0 -9
112 maiz_144 0 0 0 -9
113 maiz_133 0 0 0 -9
114 maiz_121 0 0 0 -9
115 maiz_111 0 0 0 -9
116 maiz_137 0 0 0 -9
117 maiz_109 0 0 0 -9
118 maiz_146 0 0 0 -9
119 maiz_164 0 0 0 -9
120 maiz_157 0 0 0 -9
121 maiz_171 0 0 0 -9
122 maiz_149 0 0 0 -9
123 maiz_165 0 0 0 -9
124 maiz_159 0 0 0 -9
125 maiz_172 0 0 0 -9
126 maiz_150 0 0 0 -9
127 maiz_166 0 0 0 -9
128 maiz_160 0 0 0 -9
129 maiz_173 0 0 0 -9
130 maiz_152 0 0 0 -9
131 maiz_167 0 0 0 -9
132 maiz_161 0 0 0 -9
133 maiz_174 0 0 0 -9
134 maiz_154 0 0 0 -9
135 maiz_169 0 0 0 -9
136 maiz_162 0 0 0 -9
137 maiz_176 0 0 0 -9
138 maiz_156 0 0 0 -9
139 maiz_170 0 0 0 -9
140 maiz_163 0 0 0 -9
141 maiz_177 0 0 0 -9
142 maiz_178 0 0 0 -9
143 maiz_192 0 0 0 -9
144 maiz_185 0 0 0 -9
145 maiz_201 0 0 0 -9
146 maiz_179 0 0 0 -9
147 maiz_193 0 0 0 -9
148 maiz_186 0 0 0 -9
149 maiz_202 0 0 0 -9
150 maiz_180 0 0 0 -9
151 maiz_195 0 0 0 -9
152 maiz_187 0 0 0 -9
153 maiz_181 0 0 0 -9
155 maiz_197 0 0 0 -9
156 maiz_188 0 0 0 -9
157 maiz_182 0 0 0 -9
158 maiz_198 0 0 0 -9
159 maiz_190 0 0 0 -9
160 maiz_200 0 0 0 -9
161 maiz_183 0 0 0 -9
162 maiz_191 0 0 0 -9
163 teos_96 0 0 0 -9
164 teos_203 0 0 0 -9
163 teos_911 0 0 0 -9
164 teos_9107 0 0 0 -9

@----------------------------------------------------------@
|        PLINK!       |     v1.07      |   10/Aug/2009     |
|----------------------------------------------------------|
|  (C) 2009 Shaun Purcell, GNU General Public License, v2  |
|----------------------------------------------------------|
|  For documentation, citation & bug-report instructions:  |
|        http://pngu.mgh.harvard.edu/purcell/plink/        |
@----------------------------------------------------------@

Web-based version check ( --noweb to skip )
Recent cached web-check found... OK, v1.07 is current

+++ PLINK 1.9 is now available! See above website for details +++

Writing this text to log file [ nuevos_final.log ]
Analysis started: Wed May 06 12:19:25 2015

Options in effect:
	--file nuevos_final
	--out nuevos_final
	--recodeA

36931 (of 36931) markers to be included from [ nuevos_final.map ]
Warning, found 165 individuals with ambiguous sex codes
Writing list of these individuals to [ nuevos_final.nosex ]
165 individuals read from [ nuevos_final.ped ]
0 individuals with nonmissing phenotypes
Assuming a disease phenotype (1=unaff, 2=aff, 0=miss)
Missing phenotype value is also -9
0 cases, 0 controls and 165 missing
0 males, 0 females, and 165 of unspecified sex
Before frequency and genotyping pruning, there are 36931 SNPs
165 founders and 0 non-founders found
Total genotyping rate in remaining individuals is 0.990151
0 SNPs failed missingness test ( GENO > 1 )
0 SNPs failed frequency test ( MAF < 0 )
After frequency and genotyping pruning, there are 36931 SNPs
After filtering, 0 cases, 0 controls and 165 missing
After filtering, 0 males, 0 females, and 165 of unspecified sex
Writing recoded file to [ nuevos_final.raw ]

Analysis finished: Wed May 06 12:19:30 2015
```

Es decir, básicamente es como copiar-pegar un archivo al final de otro.


**Ejercicio** ¿Cómo concatenar tres o más archivos a la vez?


**Pregunta:** ¿Y si quisiéramos tener el resultado en un archivo nuevo?


## Redirección con bash

**Pregunta** ¿Qué son el Standar output y el Standar input?

###  `>` y `>>`

Redirige el Standar output (*stdout*) a un archivo en vez de imprimirlo en pantalla.

Estando en `Prac_bash/Maiz`:

```
$ cat nuevos_final.fam *log > catejemplo.txt
$ ls
catejemplo.txt		ejemplonano.txt		nuevos_final.fam
ejemplo_final.bed	nuevos_final.bed	nuevos_final.log
ejemplo_final.fam	nuevos_final.bim
$ head catejemplo.txt
1 maiz_3 0 0 0 -9
2 maiz_68 0 0 0 -9
3 maiz_91 0 0 0 -9
4 maiz_39 0 0 0 -9
5 maiz_12 0 0 0 -9
6 maiz_41 0 0 0 -9
7 maiz_35 0 0 0 -9
8 maiz_58 0 0 0 -9
9 maiz_51 0 0 0 -9
10 maiz_82 0 0 0 -9
$ tail catejemplo.txt
Total genotyping rate in remaining individuals is 0.990151
0 SNPs failed missingness test ( GENO > 1 )
0 SNPs failed frequency test ( MAF < 0 )
After frequency and genotyping pruning, there are 36931 SNPs
After filtering, 0 cases, 0 controls and 165 missing
After filtering, 0 males, 0 females, and 165 of unspecified sex
Writing recoded file to [ nuevos_final.raw ]

Analysis finished: Wed May 06 12:19:30 2015
```

Nota que si el archivo catejemplo.txt ya existe será borrado por el comando anterior. Si no deseas que esto ocurra sino que el nuevo contenido se agregue al final de un archivo ya existente entonces usa  `>>`. Así:

```
$ cat nuevos_final.fam *log >> catejemplo.txt
```

**Ejercicio** utiliza `sed` para sustituir "Solanum lycopersicum" del archivo `Tomates/tomates.fasta` por "jitomate" y guarda el output en un nuevo archivo llamado "edited_tomates.fasta"


### `|`
Toma el stdout de un comando y lo convierte en el input de otro (*Pipes* the strout).

![Mariopipe](https://i.ytimg.com/vi/uMCCxuGIGtw/hqdefault.jpg)


Ejemplo:

```
$ ls
catejemplo.txt		ejemplonano.txt		nuevos_final.fam
ejemplo_final.bed	nuevos_final.bed	nuevos_final.log
ejemplo_final.fam	nuevos_final.bim
$ ls | wc -l
       8
```  

Otro ejemplo (no se muestra el stdout pues es demasiado largo)

```
cat *.fam | more
```


Más detalles y otras formas de redireccionar (que ocupan algunos programas) las puedes encontrar aquí [https://www.tutorialspoint.com/unix/unix-io-redirections.htm](https://www.tutorialspoint.com/unix/unix-io-redirections.htm)


**Ejercico** Primero explora qué hace el comando `history`. ¿Cómo lo combinarías con `grep` para buscar cuáles líneas hemos corrido con `cat`?



## curl y wget

`curl` Sirve para bajar archivos de internet a la computadora.

Sintaxis:

    curl [opciones] [direccionURLdelarchivo]


Ejemplo, podemos bajar sencuencias de ADN de GeneBank! ([instrucciones de cómo construir la url aquí](http://www.ncbi.nlm.nih.gov/books/NBK25499/#chapter4.EFetch))


```
$ curl -s "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=nucleotide&rettype=fasta&id=AB080944.1"
>AB080944.1 Pinus patula chloroplast matK gene for maturase, complete cds
ATGGATGAGTTCCATAGATGCGGAAAGGAAGATAGCTTTTGGCAACAATGCTTTTTATATCCACTCTTTT
TTCAGGAAGATCTTTACGCAATTTCTCATGATCATTATTTGGATGTATCAAGTTCCTCCAGACCGATGGA
ACATTTAAGTTCCAATGATCAATTAAGTTTCCTAACTGTAAAACGTTTGATTGGTCAAATACGTCAACAA
AATCATTCAATTGTTTTATTCGTGAATTGCGATCCAAATCCATTAGCTGATCGCAAGAAGAGTTTCTATT
CTGAATCGGTACTAGAAGCACTTACATTGGTCCTGGAAGTTCCGTTCTCTATATGGTCAAAATATTCTGT
GGAAGGGATGAATGAATCGAAGAGTTTCCGGTCGATCCATTCAATATTTCCCTTCTTAGAGGATAAATTC
CCGCATTCAAATTCTATATTAGATGCACGAATACCCTATTCTATTCATCCGGAAATTTTGGTTCGAACCT
TTCGTCGCTGGATCCGAGATGCTCCCTCCTTGCACCCATTACGATCTGTTCTCTATGAATATAGAAATAG
TCCAGATAATTTACAAAGATCAATTATTGTCGTCCCAAGAGTAAATACGAGATTCTTCCTGTTCCTGTGG
AATTATTATGTCTGTGAATGCGAATCCATTTTATTTTCCCGTCTTAAACGATCCTCTCATTCACGATCGT
TGACTCATGGATCTTTCCCTCAGCGAACTCATTTTCATCGAAAGATAAAACATATTATCATATTTTCTCG
TCGAAATTCACTGAAAAGTATCTGGTCGTTGAAGGATCCTAAAATTCACTATGTTAGATATGGCGAAAGA
CCTATTATAGCTATAAAGGGTGCTCATCTCCTAGTTAAAAAATGTAGATATTATCTTCTAATTTTTCGGC
AATTTTATTTCCATCTTTGGTCCGAACCGTATAGGGTCTGTTCTCATCAATTATCCAAGAATTGTTCTTC
TTCTCCAGGTTATTTTTTGAGGGTTCGGATGAACCCTATTTTGGTCAGAACCAAAATGCTCGATGAGTTA
TTCATCGCCGATCTTATTACCGATGAAATTGATCCAATAGTTCCGATTGTACCAATAATTGGATTATTGG
CTACAGAAAAATTCTGTGACATATCAGGGCGGCCAATTAGTAAATTGTCTTGGACCAGTCTAACAGATGA
TGATATCCTCGATCGATTCGATCAAATTTGGAGAAATCTTTTTCATTACTACAGTGGATCCTTTGATCGA
GATGGTTTATATCGTATAAAGTATATACTTTCATTATCATGTGCTAAAACTTTAGCCTGTAAACATAAAA
GTACGATACGTGTAGTTCGGAAGGAATTAGGTCCAGAACTCTTTAAAAAATCGTTTTCAAAAGAACGAGA
ATTTTATTCTCTGCGCTTTTCATCAAAAGCGGCGGCCCGTTCGCAGAGAGAACGAATTTGGCATTCAGAT
ATTTCCCAGATAAATCCCCTAGCTAATTCCTGGCAAAAGATACAGGATCTGAAAATAGAAAACTTATTTG
ACCAATGAAATGCTCTTTGAGTAATTGCCTCGATTCAGAATCATTTTTATTTTTCTATCCGAGAACTAAA
ATGATTAGGAAATAGATACATTACATGGGGAAAGCCGTGTGCAATGAGAAT
```

**Ejercicio:** corre de nuevo `curl` como en el ejemplo anterior, pero guarda el resultado en un archivo llamado `Ppatula_matk.fasta`


`wget` hace algo parecido a `curl`, pero lo salva a un archivo directamente, en vez de solo imprimirlo a la pantalla.


```
wget "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=nucleotide&rettype=fasta&id=AB080944.1"
```

En bioinformática `curl` y `wget` se utilizan para transferir desde archivos FASTA de secuencias individuales de GeneBank hasta genomas completos.

* Excelente explicación más profunda de `curl` y `wget` en el Capítulo 6 de Buffalo V (2015) Bioinformatics data skills.


### Comodines o el uso de `*` `?` `[]` `{}`

Volvamos a ver el contenido de Maiz:

```
$ cd Prac_bash/Maiz
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


## Scripts

Podemos insertar el código en una Terminal y ésta lo ejecutará, sin embargo, realizar análisis bioinformáticos complejos de esta forma acarrea dos problemas serios:

1. **No recordaremos** exactamente qué comando con qué parámetros utilizamos, o para qué hicimos cierto paso.
2. **No podremos compartir** nuestra pipeline con otras personas.
3. **No podremos repetir** rápidamente los análisis con otros datos o cambiando algún paso.

En otras palabras, si haces los análisis de tu trabajo en la terminal sin tenerlos en un script es como platicar la introducción de tu tesis sin haberla escrito nunca. Considera el correr comandos en la terminal como una **prueba** y ya que todo funcione, pon todos los comandos juntos en **uno más scripts documentados** y deja que corra el análisis de principio a fin solito (veremos adelante cómo).Un script es:

* un **archivo de texto plano** (¡¡NO WORD!!)
* permanente,
* repetible,
* anotado,
* compartible

En otras palabras, un script es una recopilación por escrito de las instrucciones que queremos que la computadora corra, de modo que al tener esas instrucciones cualquiera pueda repetir el análisis tal cual se hizo.

El script consta de dos tipos de texto:

**1.** El **código** (comandos) que queremos que se ejecute, en el órden que queremos que lo ejecute.

Es decir lo mismo que escribiríamos en la Terminal para hacer un análisis, pero guardado en un archivo de texto que tiene todos los comandos juntos y que podemos abrir para **repetir** o **compartir** el análisis.

**2.** Comentarios escritos **para un ser humano** en un **lenguaje de humanos**, dígase no solo en español, sino que nos permita entender qué hace el código, qué tipo de información requiere y cualquier otra cosa que una persona cualquiera necesite para poder utilizar el código del script de forma correcta.


Para que la computadora distinga entre el código y los comentarios para humanos se utiliza el símbolo `#`. Todo el texto a la *derecha* del símbolo `#` será ignorado por la computadora, aunque sí "se imprima" en la Consola.

Por ejemplo, el texto siguiente es un estracto de un script para correr Admixture:


```{bash}
#### Admixture

## For Juniperus
mkdir -p ../genetic/JmINGP/out.noreplicates/popstructure
cd ../genetic/JmINGP/out.noreplicates/popstructure

# recode plink to needed formats
cp ../batch_1.plink.* ./
plink --file batch_1.plink --maf 0.05 --geno .2 --make-bed --out batch_1.plink --noweb --allow-no-sex

# run admixture using multithreaded mode, fixed random seed and corss-validation procedure to choose the correct value
for K in 1 2 3 4 5 6 7 8 9 10 11 12 13;
do ../../../../bin/admixture --cv batch_1.plink.bed  $K -j4  -s 21 | tee log${K}.out; done

# Check CV
grep -h CV log*.out

# back to bin
cd ../../../../bin

```

### Cómo escribir un script

Escribir un script es escribir en un **editor de texto** los comandos para resolver un problema, de preferencia comentando cada paso.

Una buena forma de escribir un script es:

1. Escribir el algoritmo, es decir los pasos que queremos hacer.
2. Marcar dichos pasos como comentarios (recuerda el uso de `#` para indicar que el texto a su derecha es un comentario, no un comando).
3. Escribir el código para hacer cada paso debajo del comentario correspondiente.

Ejemplo:

* Algoritmo para guardar secuencias de *Chiropterotriton*

```
Definir secuencias a bajar desde NCBI
Crear directorio para guardar datos
Bajar datos al directorio deseado
Revisar secuencias
Fin
```

* Algoritmo + código para bajar secuencias de *Chiropterotriton*:

```
## Este script baja 3 secuencias de Chiropterotriton de NCBI
# Crear directorio para guardar datos
mkdir Chiropt

# Bajar datos de NCBI
curl -s "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=nucleotide&rettype=fasta&id=937202862,937202860,937202858" > Chiropt/ranas.fasta

# Revisar qué secuencias se bajaron
grep ">" Chiropt/ranas.fasta
```

**Observación**: otra ventaja de los scripts es que nos permiten tener en un solo documento *varios* comandos que se utilizaron para hacer algo, es decir, conforme se complican los análisis necesitamos más de una línea de comando para realizarlos.

También, para análisis muy complejos o tardados, podemos tener un script y "mandarlo a correr" a un servidor.

Editores de texto recomendados:

* Mac y Linux (y hasta Windows): [Atom](https://atom.io/)
* Linux: [Gedit](http://sourceforge.net/projects/gedit/)


**NOTA IMPORTANTE**: el workingdirectory de un script siempre es el directorio donde está guardado dicho script. Entonces, es importante que si tu script va a manejar directorios (`cd` a algún lugar) lo planees todo con **rutas relativas** empezando en el directorio donde guardarás el script. ¿Dónde es un buen lugar para guardar el script? Lo veremos con detalle en otra unidad, pero en resumen el mejor lugar es en el mismo directorio que los datos, o en uno muy cercano.

El ejemplo que vimos antes, abierto en un editor de texto se ve así:

![egScript_noShebang.png](egScript_noShebang.png)

La terminación `.sh` indica que es un archivo Shell (es decir el interpretador de la Terminal, equivalente a decir Bash, recordemos la Unidad 1). Y al darle esta terminación de archivo, o señalar el tipo de lenguaje en el menú de opciones, el editor de texto nos ayuda a leer el código coloreando los comandos, los flags, las variables y los comentarios.

En este momento ya podemos correr nuestro script. Sólo hay que ir a la Terminal, `cd` al directorio donde esté nuestro script y utilizar el comando `bash`:

```
$ bash getsecsNCBI.sh
>gi|937202862|gb|KT820711.1| Chiropterotriton sp. SMR-2015b voucher MVZ:Herp:269665 cytochrome b (cytb) gene, partial cds; mitochondrial
>gi|937202860|gb|KT820710.1| Chiropterotriton sp. SMR-2015a voucher IBH:28182 cytochrome b (cytb) gene, partial cds; mitochondrial
>gi|937202858|gb|KT820709.1| Chiropterotriton sp. SMR-2015a voucher IBH:28178 cytochrome b (cytb) gene, partial cds; mitochondrial

```

`bash` es un comando que a su vez ejecuta comandos de un stdinput o de un archivo, en este caso nuestro script.


Podemos volver un script un **ejecutable** con lo cual correrá como cualquier otro comando. De hecho, los comandos que hemos estado utilizando son scritps ejecutables que vivien en algún lugar de la computadora. Sin embargo no cubriremos aquí cómo volver ejecutable un script.
