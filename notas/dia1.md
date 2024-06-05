
# Git

Sistema de control de código fuente: SCM (Source Code Management)
Muchas veces lo llamamos Sistemas de Control de Versiones... por eso de aquel antiguo sistema de control de código fuente llamado CVS: Concurrent Versions System.

Git fue creado por Linus Torvalds en 2005 para el desarrollo del kernel de Linux.

Git es lo que denominamos un sistema de control de código fuente distribuido.

Cada persona tiene en local su repositorio. Los repos que tienen en local los desarrolladores son iguales entre si? NO, lo son... Por lo tanto, un proyecto es la SUMA de todos los repos de los desarrolladores.
No hay un repo central con todo el código + historial del proyecto, como pasa en CVS o en SVN.

Cuando trabajamos con SCM, el concepto clave es el concepto de COMMIT...

## Qué es un commit en git?

Qué contiene un commit? Un commit tiene asociado un ID
Un conjunto de cambios... SI ESTUVIERAMOS EN SVN o  EN CVS SI... En git NO!

Un commit en GIT es un backup (foto) completa del proyecto en un momento dado del tiempo...
Es como si meto mi carpeta de proyecto en un archivo zip... completa!!!!
En git, no se guardan CAMBIOS. 
Si en un momento dado se requieren los cambios (las diferencias entre 2 commits) se calculan bajo demanda.

Y en git, al igual que en otros SCM, los commit llevan una secuencia temporal... un commit va asociado al menos a un commit anterior.

## En que se diferencia GIT de un sistema de Backups?

En git, cosa que no podemos hacer en los sistemas de backup, un proyecto puede ir evolucionando de formas paralelas en en el tiempo. Lo que llamamos RAMAS.

Los commit van asociados a 1 o más branches... Y dentro de los branches es donde mantenemos la secuencia temporal.

Un repo de git es un conjunto de ramas, que contienen commit secuenciales, pero paralelos a los commits de otras ramas.

## Los 3 árboles de GIT

Hay 3 conceptos que manejamos en git... y que debemos tener claros:
- Working dir (carpeta de mi proyecto)
- Área de preparación (staging area)
- Repositorio (donde se guardan los commits)

### Área de staging

El área de staging es el camino para llevar nuestro código a un commit.

Podéis entenderlo como una "carpeta" paralela a la carpeta de trabajo (working dir)

Lo que vamos haciendo es copiar archivos de nuestra carpeta (working dir) al área de preparación.
Una vez tengo algo que me gusta en el área de preparación... lo comparto(lo sello) en un commit.

El commit por tanto es una foto de lo que hay en el área de preparación.

MOMENTO 1 del tiempo
                                        Esto es lo que tengo bajo control de git
                                ------------------------------------------------------------------
    WORKING DIR                 STAGING AREA                REPOSITORIO
    -------------------------- --------------------------- ---------------------------------------
    mi-proyecto/                mi-proyecto/                mi-proyecto/RAMA-A
        Archivo1.txt (v1)  ------> Archivo1.txt (v1) ----->  C1 (Archivo1.txt v1)
        Archivo2.txt (v1)

    $ git add Archivo1.txt
    $ git commit -m "Añadido Archivo1.txt"
    $ git status
        All clean.. No hay cambios. Qué cojones significa esto?
        El comando git status compara (busca diferencias) entre el WORKING DIR y el AREA DE STAGING
                                                        y entre el AREA DE STAGING y el REPOSITORIO


MOMENTO 2 del tiempo

                <------------>               <----------->
    WORKING DIR                 STAGING AREA                REPOSITORIO
    -------------------------- --------------------------- ---------------------------------------
    mi-proyecto/                mi-proyecto/                mi-proyecto/RAMA-A
        Archivo1.txt (v2)  -----> Archivo1.txt (v2)           C1 (Archivo1.txt v1)
        Archivo2.txt (v1)         Archivo2.txt (v1)           C2 (Archivo1.txt v2 + Archivo2.txt v1)

    git add Archivo*
    git status
    git commit -m "Más cositas"


MOMENTO 3 del tiempo

                <------------>               <----------->
    WORKING DIR                 STAGING AREA                REPOSITORIO
    -------------------------- --------------------------- ---------------------------------------
    mi-proyecto/                mi-proyecto/                mi-proyecto/RAMA-A
        Archivo1.txt (v3)  --->   Archivo1.txt (v3)           C1 (Archivo1.txt v1)
        Archivo3.txt (v1)         Archivo2.txt (v1)           C2 (Archivo1.txt v2 + Archivo2.txt v1)
                                  Archivo3.txt (v1) 
    git status?
        Entre el working y el staging: 
            - Archivo1 tiene cambios no controlados aún por git
            - Archivo2 ha sido borrado del proyecto... y lo he borrado de git? Le he dicho a git que en el siguiente commit no lo incluya? NO
            - Archivo3: Nuevo archivo... git no lo conoce
        Entre el staging y el repositorio:
            No hay cambios en el área de staging pendentientes de ser guardados...
    
            git add Archivo1.txt Archivo3.txt

    git status?
        Entre el WD y el AS:
            - Archivo2 ha sido borrado del proyecto... y lo he borrado de git? Le he dicho a git que en el siguiente commit no lo incluya? NO
        Entre el AS y el REPOSITORIO:
            - Hay cambios el AS aún no guardados en el repo:
                - Archivo1 v3
                - Archivo3 v1

MOMENTO 4 del tiempo

                <------------>               <----------->
    WORKING DIR                 STAGING AREA                REPOSITORIO
    -------------------------- --------------------------- ---------------------------------------
    mi-proyecto/                mi-proyecto/                mi-proyecto/RAMA-A
        Archivo1.txt (v4)  --->   Archivo1.txt (v3)           C1 (Archivo1.txt v1)
        Archivo3.txt (v1)         Archivo2.txt (v1)           C2 (Archivo1.txt v2 + Archivo2.txt v1)
                                  Archivo3.txt (v1) 

    git status?
        Entre el WD y el AS:
            - Archivo2 ha sido borrado del proyecto... y lo he borrado de git? Le he dicho a git que en el siguiente commit no lo incluya? NO
            - El archivo1 tiene cambios aún no controlados por git
        Entre el AS y el REPOSITORIO:
        - Archivo1 v3
        - Archivo3 v1

    git rm --cached Archivo2.txt    # Eliminar el Archivo2.txt del área de staging

                <------------>               <----------->
    WORKING DIR                 STAGING AREA                REPOSITORIO
    -------------------------- --------------------------- ---------------------------------------
    mi-proyecto/                mi-proyecto/                mi-proyecto/RAMA-A
        Archivo1.txt (v4)  --->   Archivo1.txt (v3)           C1 (Archivo1.txt v1)
        Archivo3.txt (v1)         Archivo3.txt (v1)           C2 (Archivo1.txt v2 + Archivo2.txt v1)

## Colaborar con git

Tengo mi proyecto... y quiero empezar a trabajar con otras personas en él... 
Queremos estar compartiendo trabajo

Cuando trabajamos con SVN, CVS, todo el código está en un repo CENTRAL... y trabajo contra el repo CENTRAL.
Diosito! diosito!... que no se caiga... que me quedo sin trabajar (VAYA MIERDOLO!!!!)

                                        REPOSITORIO
                                            rama1: C1 -> C2 -> C3 -> C4 -> C5 -> C7 (*)
                                        mi-proyecto:
                                         |
                        Servidor de alojamiento de repositorios REMOTOS de git: remoto1
                                         |
    -+-----------------------------------+----------------------------------+---- red de computadoras
     |                                                                      |
   IvanPC                                                                MenchuPC
     |                                                                      |                     
   mi-proyecto:                                                    mi-proyecto:
    Mi carpeta de trabajo (working dir)                               Menchu carpeta de trabajo (working dir)
    Mi área de staging                                                Menchu área de staging                 
    Mi repositorio                                                    Menchu Repositorio:
        rama1: C1 -> C2 -> C3 -> C4 -> C6 ---> C7           remoto1/rama1: C1 -> C2 -> C3 -> C4 -> C5 -> C7 (*)
                    |                     ^    ^ \                                |      ^           
                    v                     |    |  \                               v      |
        remoto1/rama1: C1 -> C2 -> C3 -> C4 -> C5 -> C7 (*)   rama1: C1 -> C2 -> C3 -> C4 -> C5 -> C7

Con git podemos copiar ramas de un repo local a un repo REMOTO.
    git push
Ivan quiere actualizarse con los cambios que ha hecho Menchu:
    git fetch

Los commits los muevo d una rama a otra mediante operaciones de fusión de cambios:
- git merge
  - Tradicionales
  - Fast-forward: Copio un Commit a otra rama... siempre y cuando tengan compatibilidad (el commit padre que sea común.)
- git rebase

git pull = git fetch + (git merge | git rebase)

## Servidor de alojamiento de repositorios REMOTOS de git

Son herramientas tipo: Gitlab, Github, Bitbucket, Apache HTTPD, Carpeta compartida en red...
Un repo remoto es una carpeta asociada a un proyecto, en la que solo tengo:
- REPOSITORIO
- No hay working dir
- No hay área de staging

Git checkout lo que hace es cambiar el HEAD del repo local.
Tenía tanta funcionalidad que hoy en día se ha partido en varios comandos:
- git switch
- git restore
- git checkout








---

# Trabajando con git en local

Operaciones para:

- Modificar el área de STAGING
    git add
    git mv
    git rm
- Operaciones para el repo
    git commit
- Consultando el historial... y buscando diferencias:
    git log
    git diff
    git show
- Consultar el estado del proyecto:
    git status
- Restauraciones
    git checkout
    git revert
    git restore

# Trabajar contra repositorios REMOTOS (MIERCOLES) / Algo de ramas

# Trabajar con ramas (JUEVES)... y alguans cositas.

- Submodulos
- Stashes
- Hooks
- Rebases interactivos


---
En git puedo tener asociado a un proyecto, 10 remotos.

Si tengo un remoto(remoto1) con 5 ramas:
rama1
rama2
rama3
rama4
rama5

Cuando hago clone:
remoto1/rama1
remoto1/rama2
remoto1/rama3
remoto1/rama4
remoto1/rama5
Y para la rama donde esté posicionado el HEAD, se me crea una rama local con el mismo nombre.
rama4

En git habréis visto mucho la palabra ORIGIN. 
ORIGIN es el nombre por defecto que en git se da al remoto desde el que clonamos un proyecto.

git remote:
origin
---

Una rama es una secuencia de commits en un proyecto.

Cuando empiezo un proyecto, abro una rama (una linea de evolución en el tiempo de mi proyecto)

git init                                                      cliente A,C                     cliente B
                                                             v1.0.0    v1.0.1                 v2.0.0
                                                               v                                v 
--- hotfix                                                      C3 ---> C12
                                                               /                            
---> master(main)                                            C3                               C11  
                                                            /                                /
---> release1                                             C3                        C10 -> C11       (cherry pick C12) P.Sistema (End2End)
                                                         /  \                      /         \        v
---> desarrollo, dev, desa, development--> C1 -> C2 -> C3 -> C9 ----------+----> C10 -------> C11 -> C13    ---------------> C11'
                                                         \                 \    / P Integración        \                   /
---> feature R1                                           C3 -> C4 -> C5 --> C10                        \                 /
                                                           \                                             \               /
---> feature R2                                             \                                           C13 -> C6' -> C11'
                                                             \
---> feature R3                                               C3 -> C7 -> C8 P.Unitarias

GITHOOKS: Son scripts que se ejecutan en determinados eventos de git.
    - pre-commit
    - post-commit
    - pre-push
    - post-push
    - pre-rebase
    - post-rebase
    - pre-merge
    - post-merge
    - pre-checkout
    - post-checkout
    - pre-clone
    - post-clone
    - pre-receive
    - post-receive
    - pre-applypatch
    - post-applypatch
    - pre-commit-msg
    - post-commit-msg
    - prepare-commit-msg
    - update
    - pre-auto-gc
    - post-rewrite
    - sendemail-validate
    - fsmonitor-watchman
    - p4-ch

FLOW DE GIT: Git Flow
             Github Flow

C4: SUMA
C5: RESTA
C9: MULTIPLICACION
C10?: Fusión de la resta y la multiplicación
---

# DEV-->OPS

Es una cultura, una filosofía, un movimiento... que defiende la automatización.

## Integración Continua

## Entrega Continua

## Despliegue continuop

## Jenkins

## SonarQube
---

# Versionado de software

1.2.3

            Cuando se incrementan?
1 MAJOR     Breaking changes. Cambios que no respetan RETRO-COMPATIBILIDAD
2 MINOR     Nueva funcionalidad
            Funcionalidad marcada como obsoleta (deprecated)
                + Arreglos de bugs
3 PATCH     Arreglo de bug


---

# Característica principal de una metodología ágil.

Entregar el producto de forma incremental mi cliente.

En la met. tradicionales entregaba al final del proyecto.
    HITO 1: 15-Junio: **R1, R2, R3**
        Qué pasaba si el día 15 no estaba el R3:
        - Se replanificaba el HITO. Vamos con retraso: 20-Junio
    SPRINT 1: 15-Junio: R1, R2, R3
        Qué pasaba si el día 15 no está el R3:
        - Se deja el R3 para el siguiente sprint.
        - Pero el día 15 hay paso a producción. R1, R2

Para conseguir feedback rápido de mi proyecto por parte del cliente!

---

Reglas de la rama master:
- Nunca, bajo pena de que me corten las uñas hipercortas y me metan la mano en un vaso lleno de zumo de limón,
  se hace un commit en la rama master.
- Lo que hay en master se considera APTO para producción