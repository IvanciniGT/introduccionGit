# Crear repo en local 
$ git init


# Configurar el repo

Las configuraciones de git se dan a 2 niveles:
- A nivel de cada proyecto (repositorio)
- A nivel de usuario en el PC

$ git config -l # Listar configuraciones
$ git config --global user.name "Nombre Apellido"   # Configurar nombre
$ git config --global user.email "ivan@yo.com"      # Configurar email

$ git config --global init.defaultbranch master
Tradicionalmente se ha usado el nombre master para la primera rama.
Hoy en día, master ha caído en desuso (WOKE) y se recomienda usar main.
    master/slave tiene connotaciones racistas/esclavistas -> main/worker

Una rama en git no se crea hasta que en ella pongo un commit!

$ git switch -c desa # Me crea la rama desa y me cambio a ella

$ git add Archivo1.txt
$ git commit -m 'Primer commit'
    c64db028a0fad836d5680a733ff3976b645c857c   <<< ID del commit
    Ese ID del commit es una huella digital (hash) de todo lo que contiene el commit.. del backup
    Git puede usar ese ID para asegurarse que nace intencionadamente o no haya tocado los archivos guardados en el repo.

$ git log --oneline

# Diferencias entre versiones de los archivos

$ git diff Archivo1.txt
Me muestra las diferencia entre el archivo guardado en WD y el guardado en AS

Los cambios se controla a nivel de línea de código

$ git diff --staged Archivo1.txt
Me muestra las diferencias entre el archivo guardado en AS y el guardado en el último commit

$ git diff COMMIT_ID Archivo1.txt
Me da las diferencias entre el Commit y lo que hay en WD

$ git diff COMMIT_ID1..COMMIT_ID2 -- Archivo1.txt
Me da las diferencias entre el Commit y lo que hay en WD

# PathSpecs de git

Rutas con comodines que puedo usar en git.

$ git add *.txt     # Añade todos los archivos .txt
$ git add *         # Añade todos los archivo dentro de la carpeta en la que estoy... y sus subcarpetas QUE NO SEAN OCULTOS
$ git add .         # Añade todos los archivo dentro de la carpeta en la que estoy... y sus subcarpetas INDEPENDIENTEMENTE DE SI SON OCULTOS O NO
$ git add :/        # Añade todos los archivos INDEPENDIENTEMENTE DE SI SON OCULTOS O NO desde la raíz del proyecto hacia abajo


# VARIABLE HEAD

Head es una variable en git... que apunta al commit que en ese momento estoy visualizando en mi WD.

Por defecto, la variable HEAD apunta siempre al último commit que haya realizado en la rama en la que me encuentro.

HEAD^ apunta al commit anterior al commit en el que estoy posicionado
HEAD^^ apunta al commit anterior al commit anterior al commit en el que estoy posicionado
HEAD^^^ apunta al commit anterior al commit anterior al commit anterior al commit en el que estoy posicionado

Claro... si quiero ir muy para atrás... demasiados picos... y tengo otra sintaxis:
HEAD~ apunta al commit anterior al commit en el que estoy posicionado
HEAD~2 
HEAD~30

Esto lo puedo cambiar... git checkout.
    Lo usaremos para viajar en el tiempo

    git checkout HEAD^

    Al hacer esto, el repo se queda en esta dettach (Head desasociado). Eso significa que desde este commit
    no puedo hacer un nuevo commit en esta rama... porque ya hay uno detrás.

    Si quiero, puedo hacer modificaciones.. pero no guardarlas en un commit de esta rama... las puedo guardar en otra rama

    desa       C1 -> C2
                ^
              HEAD


        WD                              AS                          REPO

        Archivo1.txt                    Archivo1.txt                   C2(Archivo1.txt, Archivo2.txt(v1))
        Archivo2.txt (v2)               Archivo2.txt(v1)



# Lo que llevamos:

- Crear un repo: git init
- Configurar repo: git config [--global] prop valor
- Ver la configuración: git config -l
- Crear y cambiar a una rama: git switch -c nombreRama
- Ver las ramas del repo: git branch
- Añadir cosas al AS: git add pathspec
- Eliminar cosas del AS: 
    git rm --cached pathspec            Para archivos nuevos
    git restore --staged pathspec       Para archivos modificados
- Hacer commit: git commit -m 'mensaje'
- Ver el historial de commits: git log --oneline
- Ver diferencias entre archivos: 
  - git diff pathspec : Muestra diferencias entre WD y AS
  - git diff --staged pathspec : Muestra diferencias entre AS y el último commit
  - git diff commitID pathspec : Muestra diferencias entre commit y WD
  - git diff commitID1..commitID2 -- pathspec : Muestra diferencias entre 2 commits
- Viajar en el tiempo: git checkout commitID
- Restaurar un fichero en mi carpeta (descartando cambios): git restore pathspec


## Qué ocurre si quiero modificar los COMMITS QUE YA TENGO EN EL REPO!

    C1 -> C2 -> C3

Por qué querría hacer eso?
- Porque quiero cambiar el contenido de un commit (había algo mal)
- Me he confundido de rama
- Quiero Mejorar el historial de commits de mi proyecto:
  - Mejorar un mensaje de commit
  - Fusionar commits en 1


Para quiero yo todos esos commits?
- Para poder dar marcha atrás en el tiempo
- Quitar una funcionalidad concreta que se implementó en un commit
- Coger una funcionalidad concreta que se añadió en un commit y llevarla a otra rama

Y para todas estas cosas... que es lo primero que necesito? Lo primero que necesito es tener un historial limpio de commits...
- donde sea capaz de identificar (buscar el ID) del commit que me interesa modificar, restaurar, copiar...


Me piden una libraría que desarrolle:
    - Suma
    - Resta
    - Multiplicación
    - Valor absoluto
    - División

                                Añado función multiplicación
C1 (Suma) -> C2 (Suma+Resta) -> C3 (Suma+Resta+Multiplicación) -> C4 (ValorAbsoluto) -> C5 (División) -> C6 (potencia) -> C7 (división entre cero.)
             Añado función resta                                                        -m "A por el café"               -m "Ahora si que si que la tengo la división"

De entrada: Cambiar un commit que ya he distribuido (lo he subido a un remoto) es hiperpeligroso... Tengo que tenerlo muy claro!

# APAÑAR el último commit

git commit --amend: Me permite modificar el último commit que he hecho en la rama en la que estoy.
    - Cambiar el mensaje del commit
    - Cambiar el contenido
    git commit --amend --no-edit
    git commit --amend -m "Segundo commit"

Hago un código
Lo pruebo, funciona: COMMIT
Refactorización, para dejarlo decente: COMMIT AMEND
Pruebo, funciona: COMMIT si la lio en refactorización: RESTORE

# Me dice el jefe, que el valor absoluto... para otra!!!

## Generar un commit de reversión de cambios

$ git revert COMMIT_ID
    - Genera un nuevo commit que revierte los cambios del commit que le indico
    - No borra el commit original
Generaría en automático un commit C8, que no tiene el cambio que se añadió (que hay que calcularlo bajo demanda) en el commit del valor absoluto.

Se calcula la diferencia entre  C3 (Suma+Resta+Multiplicación) -> C4 (ValorAbsoluto)
    Y se genera lo que llamamos un PATCH (los cambios que hay que hacer para que C3 se convierta en C4)... 
    Y se genera un commit que des-aplica ese PATCH.

    C1(suma)->C2(resta)->C3(multiplicación)->C4(valor absoluto)->C5(división)->C6(potencia)->C7(división entre cero)->C8(quito valor absoluto)

Si quiero aplicar en el futuro de nuevo el valor absoluto... lo que haría sería un cherry-pick del commit C4.
    Que calcularía de nuevo el PATCH que hay que aplicar para que C3 se convierta en C4... y lo aplicaría.

## git reset
    Y ESTA ES COMPLEJA DE ENTENDER

C1 -> C2 -> C3

Git reset BORRA COMMITS DEL REPOSITORIO = PELIGROSO
Funciona de 3 formas diferentes (más que diferentes, INCREMENTALES):
- soft                                  <<<<
- mixed << ESTA ES POR DEFECTO          <<<< CON ESTAS 2, no pierdo datos (código), pierdo historial... pero no código
- hard                                  Con esta pierdo historial y código ***ESTA ES LA REQUETE PELIGROSA***

Con cualquiera de ellas, le paso el ID del commit en el que me quiero quedar (no el que borro).. Se borra todo a partir de ese commit.

    git reset C1... en cualquier de sus opciones (soft, mixed, hard) en el historial (REPO) solo queda C1

    Además, me mueve el HEAD a ese commit: C1

Y a partir de aquí es donde vienen las diferencias:
    - soft: SOLAMENTE BORRA COMMITS DEL REPO... pero no toca AREA DE STAGING ni WD
    - mixed: BORRA COMMITS DEL REPO y restaura el AREA DE STAGING a la versión del commit en el que me quedo, pero no toca WD
    - hard: BORRA COMMITS DEL REPO, restaura el AREA DE STAGING y WD a la versión del commit en el que me quedo


MOMENTO 1
    WD:                     AS:                     REPO:
    proyecto/               proyecto/               C1 (Archivo1.txt(v1), Archivo2.txt(v1))
        Archivo1.txt(v1)     Archivo1.txt(v1)       
        Archivo2.txt(v1)     Archivo2.txt(v1)       

MOMENTO 2
    WD:                     AS:                     REPO:
    proyecto/               proyecto/               C1 (Archivo1.txt(v1), Archivo2.txt(v1))
        Archivo1.txt(v2)     Archivo1.txt(v2)       C2 (Archivo1.txt(v2), Archivo2.txt(v1))
        Archivo2.txt(v1)     Archivo2.txt(v1)       

MOMENTO 3
    WD:                     AS:                     REPO:
    proyecto/               proyecto/               C1 (Archivo1.txt(v1), Archivo2.txt(v1))
        Archivo1.txt(v2)     Archivo1.txt(v2)       C2 (Archivo1.txt(v2), Archivo2.txt(v1))
                                                    C3 (Archivo1.txt(v2))

    C1 -> C2 -> C3

    git reset --soft C1
        WD:                     AS:                     REPO:
        proyecto/               proyecto/               C1 (Archivo1.txt(v1), Archivo2.txt(v1))
            Archivo1.txt(v2)     Archivo1.txt(v2)       

    git reset --mixed C1 = git reset C1
    WD:                     AS:                     REPO:
    proyecto/               proyecto/               C1 (Archivo1.txt(v1), Archivo2.txt(v1))
        Archivo1.txt(v2)     Archivo1.txt(v1)
                             Archivo2.txt(v1)

    git reset --hard C1

    WD:                     AS:                     REPO:
    proyecto/               proyecto/               C1 (Archivo1.txt(v1), Archivo2.txt(v1))
        Archivo1.txt(v1)     Archivo1.txt(v1)
        Archivo2.txt(v1)     Archivo2.txt(v1)

    La v2 del Archivo1.txt se ha perdido para siempre ( o no.. que git es muy guay... y siempre me quedará un git reflog...)


Ca(empiezo con la multiplicacion...) -> Cb(continuo con la multiplicacion...) -> Cc (me voy a cenar... y la dejo a medias ) --> Cd (la tengo lista)

git reset --soft Ca
git commit --amend -m "Multiplicación finalizada"


---

module "mi-modulo" {
    exports com.mi.modulo;
    requires com.otro.modulo;
    
}
---

# Arquitecturas de Microservicios: Sistema Animalitos Fermin

Frontal Navegador WEB                   Backend procesos JAVA
    HTML                                  HTML <- JSP

Frontales?
- Navegador web (v1.0.0)
   JS (HTML)
- App Android (v2.0.0)                        datos del animalito (v1.1.0)
- App iOs                       <- json       datos del animalito (v2.0.0)  
- IVR
- Cajeros automáticos
- App desktop Windows (v1.1.0)                alta de animalito   (v1.1.0)
- Alexa, Siri, Google Home

<animalito id="firulais">

{
    "nombre": "Firulais",
    "raza": "Pastor Alemán",
    "edad": 3,
    "foto": "https://www.foto.com/firulais.jpg"
}

{
    "nombre": "Firulais",
    "raza": "Pastor Alemán",
    "edad": 3,
    "multimedia": {
        "foto": "https://www.foto.com/firulais.jpg",
        "foto": "https://www.foto.com/firulais.jpg",
        "video": "https://www.video.com/firulais.mp4",
        "audio": "https://www.audio.com/firulais.mp3"
    
}



----

rebase interactivo: git rebase -i ... pero esto son PALABRAS MAYORES !!!
    - Cambiar el orden de los commits
    - Fusionar commits
    - Dividir commits
    - Modificar commits
    - Renombrar commits
    - Borrar commits