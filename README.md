# Taller de introducción a `git` y GitHub

En este taller de introducción a `git` y [GitHub][1] aprenderemos los comandos básicos para empezar a trabajar con repositorios de forma local y remota.

# `git`

## Instalación y configuración de `git`

### Instalación de `git`

#### Derivados de Debian, como Ubuntu, Linux Mint, Deepin, AntiX, MX Linux

```
sudo apt-get update
sudo apt-get install git
```

#### Windows

Descargar desde la web oficial: https://git-scm.com/downloads

### Configuración de `git`

Configuramos el nombre y el email que aparecerán en los *commits* que hagamos sobre los repositorios.

```
git config --global user.name "Nombre"
```

```
git config --global user.email "correo@electronico.com"
```

Para comprobar si se han aplicado los cambios podemos ejecutar el siguiente comando para mostrar cuál es la configuración actual de `git`:

```
git config --list
```

## Secciones principales de un repositorio `git`

En un repositorio `git` podemos diferenciar las siguientes secciones:

* *Workspace*
* *Staging area (Index)*
* *Local repository*
* *Remote repository*

![](imagenes/img-00.png)

## Estados de un archivo en `git`

Un archivo puede estar en alguno de los siguientes estados:

* Sin seguimiento (*untracked*)
* Preparado (*staged*)
* Modificado (*modified*)
* Confirmado (*commited*)

El siguiente diagrama muestra en qué sección se puede encontrar cada archivo en función de su estado.

```
+-------------+  +-------------+  +-------------+
|  Workspace  |  |   Staging   |  |    Local    |
|             |  |     Area    |  |  Repository |
+------+------+  +------+------+  +------+------+
       |                |                |
       |                |                |
   Untracked            |                |
       |                |                |
   Modified          Staged          Commited
       |                |                |
       |                |                |
       |                |                |
       +                +                +
```

Para consultar el estado de los archivos usamos el comando:

```
git status
```

**Este comando es muy usado** ya que es fundamental conocer el estado de los archivos de nuestro repositorio.

## Cómo trabajar con un repositorio local

### Creación de un repositorio local

Un repositorio Git es un directorio oculto llamado `.git` que se guarda en el directorio raíz de nuestro proyecto. El directorio `.git` almacena el historial de todos los cambios que se han realizado.

El comando para crear un repositorio `git` es el siguiente:

```
git init
```

Por ejemplo, para crear nuestro primer repositorio podríamos hacer lo siguiente:

```
mkdir taller-git
cd taller-git
git init
```

Si examinamos el contenido del directorio `.git` veremos el siguiente árbol de contenidos:

```
.
└── .git
    ├── HEAD
    ├── config
    ├── description
    ├── hooks
    │   ├── applypatch-msg.sample
    │   ├── commit-msg.sample
    │   ├── post-update.sample
    │   ├── pre-applypatch.sample
    │   ├── pre-commit.sample
    │   ├── pre-push.sample
    │   ├── pre-rebase.sample
    │   ├── pre-receive.sample
    │   ├── prepare-commit-msg.sample
    │   └── update.sample
    ├── info
    │   └── exclude
    ├── objects
    │   ├── info
    │   └── pack
    └── refs
        ├── heads
        └── tags
```
### Comandos básicos para trabajar con un repositorio local

**Paso 1**

En primer lugar comprobaremos en qué estado se encuentran los archivos del repositorio:

```
git status
```

**Paso 2**

Si tenemos archivos en estado ***untracked*** o ***modified*** los añadimos a la ***staging area*** con el siguiente comando:

```
git add <nombre_archivo>
```

El comando anterior nos permite seleccionar cuáles son los archivos que queremos mover a la ***staging area***. Si tenemos varios archivos que queremos mover a la ***staging area*** no es necesario hacerlo uno a uno, podemos usar el siguiente comando para moverlos todos a la vez:

```
git add .
```

**Paso 3**

Una vez que tenemos los archivos en la ***staging area*** tenemos que hacer un ***commit*** para moverlos al repositorio:

```
git commit -m "Breve comentario con los cambios realizados"
```

## Cómo deshacer cambios

### Modificar el texto del último *commit*

```
git commit -m "Modifico el texto del último commit" --amend
```

### Añadir archivos al último *commit*

```
git commit --amend
```

**Ejemplo:**

Suponemos que acabamos de hacer un *commit* en el repositorio pero nos hemos olvidado de añadir un archivo que queremos incluir en ese *commit*. En estos casos podemos utilizar el comando `git commit --amend` para añadir nuevos archivos al último *commit* realizado sobre el repositorio.

A continuación se muestra una posible secuencia de comandos simulando la situación que acabamos de describir.

```
git add archivo.txt
git commit -m "Añadimos el archivo.txt"
git add archivo_olvidado.txt
git commit --amend
```

### Mover un archivo del *staging area* al *workspace*

```
git reset HEAD <archivo>
```

**Ejemplo:**

Suponemos que hemos añadido un archivo llamado `archivo.txt` al *staging area* pero queremos volver a llevarlo al *workspace* para realizar una nueva modificación antes de hacer un *commit* en el repositorio.

El escenario descrito sería el siguiente:

```
+-------------+  +-------------+  +-------------+
|  Workspace  |  |   Staging   |  |    Local    |
|             |  |     Area    |  |  Repository |
+------+------+  +------+------+  +------+------+
       |                |                |
       |                |                |
       |                |                |
       |                |                |
       |            archivo.txt          |
       |                |                |
       |                |                |
       |                |                |
       +                +                +
```

Para mover el archivo `archivo.txt` al *workspace* ejecutamos:

```
git reset HEAD archivo.txt
```

Después del comando anterior el repositorio quedaría así:

```
+-------------+  +-------------+  +-------------+
|  Workspace  |  |   Staging   |  |    Local    |
|             |  |     Area    |  |  Repository |
+------+------+  +------+------+  +------+------+
       |                |                |
       |                |                |
       |                |                |
       |                |                |
   archivo.txt          |                |
   (Modified)           |                |
       |                |                |
       |                |                |
       +                +                +
```

### Deshacer cambios en el *workspace*

```
git ckeckout -- <archivo>
```

**Ejemplo:**

Suponemos que hemos realizado algunos cambios sobre un  archivo llamado `archivo.txt` pero queremos deshacerlos y que el archivo vuelva a tener el contenido con el que se guardó en el último *commit* en el repositorio.

El escenario descrito sería el siguiente:

```
+-------------+  +-------------+  +-------------+
|  Workspace  |  |   Staging   |  |    Local    |
|             |  |     Area    |  |  Repository |
+------+------+  +------+------+  +------+------+
       |                |                |
       |                |                |
       |                |                |
       |                |                |
   archivo.txt          |                |
   (Modified)           |                |
       |                |                |
       |                |                |
       +                +                +
```

Para deshacer los cambios realizados en `archivo.txt` y volver a su estado anterior sería necesario ejecutar:

```
git ckeckout -- archivo.txt
```

## Borrando y moviendo/renombrando archivos

### Borrar un archivo

Para borrar un archivo que ya se encuentra bajo el control de versiones de `git` es necesario utilizar el siguiente comando:

```
git rm <archivo>
```

Vamos a ver los cuatro casos que podemos encontrarnos a la hora de borrar un archivo.

1. Queremos eliminar un archivo que todavía **no ha sido incluido en el repositorio** y se encuentra en la sección `Workspace` con el estado `Untracked`. En este caso no es necesario utilizar ningún comando específico de `git`, lo borraríamos con el comando `rm`.

```
+-------------+  +-------------+  +-------------+
|  Workspace  |  |   Staging   |  |    Local    |
|             |  |     Area    |  |  Repository |
+------+------+  +------+------+  +------+------+
       |                |                |
       |                |                |
       |                |                |
   archivo.txt          |                |
   (Untracked)          |                |
       |                |                |
       |                |                |
       |                |                |
       +                +                +
```

**Ejemplo:**

```
rm archivo.txt
```

2. Queremos eliminar un archivo que **ya está incluido en el repositorio** y se encuentra en la sección `Workspace` con el estado `Modified`.

```
+-------------+  +-------------+  +-------------+
|  Workspace  |  |   Staging   |  |    Local    |
|             |  |     Area    |  |  Repository |
+------+------+  +------+------+  +------+------+
       |                |                |
       |                |                |
       |                |                |
   archivo.txt          |                |
   (Modified)           |                |
       |                |                |
       |                |                |
       |                |                |
       +                +                +
```

3. Queremos eliminar un archivo que **ya está incluido en el repositorio** y se encuentra en la sección `Staging Area` con el estado `Staged`.

```
+-------------+  +-------------+  +-------------+
|  Workspace  |  |   Staging   |  |    Local    |
|             |  |     Area    |  |  Repository |
+------+------+  +------+------+  +------+------+
       |                |                |
       |                |                |
       |                |                |
       |            archivo.txt          |
       |             (Staged)            |
       |                |                |
       |                |                |
       |                |                |
       +                +                +
```

4. Queremos eliminar un archivo que **ya está incluido en el repositorio** y se encuentra en la sección `Local Repository` con el estado `Commited`.

```
+-------------+  +-------------+  +-------------+
|  Workspace  |  |   Staging   |  |    Local    |
|             |  |     Area    |  |  Repository |
+------+------+  +------+------+  +------+------+
       |                |                |
       |                |                |
       |                |                |
       |                |            archivo.txt
       |                |            (Commited)
       |                |                |
       |                |                |
       |                |                |
       +                +                +
```

En los tres últimos casos el archivo que queremos eliminar ya se encuentra bajo el sistema de control de `git`, por este motivo hay que utilizar el comando `git rm` y después habría que hacer un `git commit` para guardar los cambios en el repositorio.

**Ejemplo:**

```
git rm archivo.txt
git commit -m "Se elimina archivo.txt"
```

### Mover/Renombrar archivos

Para mover a otro directorio o renombrar un archivo que ya se encuentra bajo el control de versiones de `git` es necesario utilizar el siguiente comando:

```
git mv <archivo> <nuevo_nombre>
```

A la hora de mover/renombrar archivos nos podemos encontrar los mismos casos que hemos comentado en la sección anterior. Por lo tanto, para mover o renombrar un archivo que todavía **no ha sido incluido en el repositorio** y se encuentra en la sección `Workspace` con el estado `Untracked` no es necesario utilizar ningún comando específico de `git`, lo haríamos con el comando `mv`. Para el resto de casos donde el archivo ya se encuentra bajo el sistema de control de `git`, usaremos el comando `git mv` y después habría que hacer un `git commit` para guardar los cambios en el repositorio.

**Ejemplo:**

```
git mv archivo.txt nuevo_nombre.txt
git commit -m "Se renombra archivo.txt por nuevo_nombre.txt"
```

