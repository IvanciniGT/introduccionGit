


suma                        C4 -> C6 -> C8   
                           /          /   \
desa    C1 -> C2 -> C3 -> C4 -> C7 --+---- C8  --> C9
                           \                \   /
    resta                   C4 -> C5  -------> C9

C8? Fusion de camibos de suma y desa
C9? Fusion de cambios de resta y desa

[]: # Comandos de Git

# Fusiones de cambios entre ramas

- Merge tradicional: Genero un nuevo commit (foto completa del proyecto, que incluya los cambios de todos los commits que fusiono)
  Me pongo en la rama en la que quiero generar el commit de fusión de cambios.
    Y ejecuto git merge RAMA_DE_LA_QUIERO_TRAER_LOS_CAMBIOS
- Merge de tipo Fast-Forward: No genera un nuevo commit, copia el commit de la rama que quiero traer los cambios.
  Me pongo en la rama en la que quiero traer los cambios.
    Y ejecuto git merge RAMA_DE_LA_QUIERO_TRAER_LOS_CAMBIOS --ff-only
- Rebase: ESTO ES MUY ESPECIAL. Se aplicaría cuando hubiera que hacer un merge tradicional

git log --oneline --all --graph
*   0eddaba (resta) Integro los dambios que hay en desa
|\  
| *   b8cd733 (suma) Nos tremos los camibos de desa
| |\  
| | * 66490f0 modificado el readme de nuestro liberprete
| * | ffc75c5 Suma
| |/  
* / 0c0bff4 (HEAD -> desa) Resta
|/  
* cb1f0a3 listo
* 5cfdd9a Commit tercero
* 5b4fb4d Segundo commit
* c64db02 Primer commit






        suma                      C7 -> C6'
                                 /       \
desa    C1 -> C2 -> C3 -> C4 -> C7 ------ C6'--------- C5'
                                            \         /
                        resta                C6' -> C5'

* 2bdb1cd (HEAD -> desa, resta) Resta
* 020afc3 (suma) Suma
* fb18526 readme.md
* cb1f0a3 listo
* 5cfdd9a Commit tercero
* 5b4fb4d Segundo commit
* c64db02 Primer commit

Los rebase, el problema que tienen es que reescriben los commits... para acumular cambios por delante.
Como haya ya distibuido esos commit anteriores C5... y ahora quiera subir a un remoto en su lugar el C5', me dice que no puedo hacerlo porque los commits no son los mismos. Puedo forzarlo... 

En general los uso solo para ramas locales, que no distribuyo... O solo para ramas de feature en las que trabajamos 2 ti@s...
Y cuando quiero integrar los cambios de desa... que es cada 5 días

                                                                                                                --squash
desa --------C1 -> C2 -> C3 -> C7 -> C8 -> C9 ---+---> C11 -> C12 --+----C11 ------------------------------------>  C9
                                                                            \                                     /
                                                    la-ivan-rama            C11 -> C4 -> C5 -> C6 -> C7 -> C8 -> C9


merge octopus
stashes
