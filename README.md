# Git Lab comandos

# 1.INICIALIZAR REPOSITORIO Y CONFIGURAR EL USUARIO
Creamos la carpeta local donde vamos a hacer el repositorio y
accedemos. Para esto usamos los comandos de Windows mkdir y cd.

mkdir git-lab-comandos-ordaz
cd git-lab-comandos-ordaz

Ahora ejecutamos el comando init de git el cual convierte un
directorio vacío en un repositorio Git, creando la carpeta .git con su
estructura interna. 
git init

Ahora, configuramos el usuario usando tu
nombre y tu correo de preferencia.
git config user.name "Daniel Ordaz Motos"
git config user.email "daniel27sep.DOM@gmail.com"

# 1.1 PRIMER COMMIT
Antes de hacer el commit en si, podemos crear el archivo README.md para que
aparezca en la pagina del repositorio posteriormente, para esto usamos echo y luego
git add para añadirlo al Staging Area.
- Tras eso, hacemos el commit al Repositorio Local con el comando
git commit el cual toma el contenido staged y lo convierte en un
commit nuevo en la historia del repositorio.

echo #Git Lab Comandos > README.md
git add README.md
git commit -m "Primer commit: inicialización del repositorio"

# 2. CICLO BÁSICO
Para comenzar con el ciclo básico creamos un archivo, lo stageamos y posteriormente hacemos un
commit descriptivo para este:
echo "Linea 1" < archivo.txt
git add archivo.txt
git commit -m "Agregar archivo.txt"

Posteriormente editamos el archivo que hemos añadido antes y ahora al hacer un git diff para ver
las diferencias entre el grupo de trabajo y el stage:
echo "Linea 2" >> archivo.txt
git diff
git add archivo.txt
git diff --cached

Con --cached solo nos muestra los archivos que esten en el stage con lo cual hace falta hacer git add anteriormente.

Finalmente, hacemos el commit de nuevo y ahora usamos git log para ver la información de los
últimos commits, además usamos los parámetros –oneline para mostrar una versión resumida:
git commit -m "Agregar linea 2 en archivo.txt"
git log --oneline

# 3. RAMAS
Para empezar, vamos a crear una rama en este caso “feature” y vamos a hacer switch para
movernos a ella:
git branch feature
git switch feature

Ahora, para simular un cambio de desarrollo, vamos a crear un nuevo archivo llamado feature.txt, lo
stageamos y hacemos el commit:
echo "Nueva funcionalidad" > feature.txt
git add feature.txt
git commit -m "Agregar el archivo en rama feature"

Finalmente volvemos a la rama master:
git switch master

# 4. MERGE CON CONFLICTO

Ahora en la rama main vamos a crear un fichero y añadirlo a un commit para simular un conflicto
con merge:
echo "Hola desde main" > conflicto.txt
git add conflicto.txt
git commit -m "Linea desde main"

A continuación, cambiamos a la rama feature de nuevo y aquí hacemos otro commit con un nuevo
archivo llamado conflicto.txt en este caso:
git switch feature
echo "Hola desde feature" > conflicto.txt
add conflictogit commit -m "Linea desde feature"

Cambiamos a master de nuevo, y aquí podemos ver el conflicto de merge:
git switch master
git merge master

Ahora en la rama main resolvemos manualmente el conflicto añadiendo conflicto.txt y haciendo
commit:
git add conflicto.txt
git commit -m "Resolver conflicto entre main y feature"

# 5. REBASE INTERACTIVO
Ahora vamos a probar el modo interactivo del rebase, primero vamos a cambiar el orden de los
commits, para ellos usamos git rebase -i HEAD~3 para los 3 últimos commits y veremos una pantalla:
git rebase -i HEAD~3

Despues debemos presionar la I en nuestro teclado para que abajo ponga “INSERT”, luego,
movemos las líneas de orden simplemente cogiendo la linea de un commit y cambiandola de lugar, después, le damos al esc para salir del modo
“INSERT” y escribimos :wq para salir y guardar, tras esto, podemos usar el siguiente comando para
ver el cambio de orden efectivo:
git log --oneline para ver el cambio de orden

Para poner commits en squash repetimos el mismo proceso pero antes de los commits donde pone "pick" escribimos squash dejando solo uno en pick.
Esto va a squashear los commits que quiere decir que se unen los commits.
Al salir del rebase interactivo nos pedira un nombre para el nuevo commit squasheado.

# 6. SQUASH
Para simular una situación realista nos movemos a la rama feature:
git switch feature

Ahora creamos un archivo y si hacemos git stash veremos que nos dice que no hay cambios locales,
esto se debe a que stash por defecto solo guarda los archivos que estén en staging, si queremos
guardar los que no están en el stage debemos usar –u o si no añadir los archivos que queramos, en
mi caso añadiré el archivo creado:
echo "Codigo de implementación" >> implementación.txt
git add implementacion.txt
git stash save "Cambio temporal"

Ahora cambiamos a master y simmulamos un escenario en el que arreglamos un bug y hacemos un
hotfix:
git switch master
echo "Bug corregido >> README.md
git add README.md
git commit -m "HOTFIX: corregido bug en master"

Volvemos a la rama de feature para seguir trabajando en nuestra “implementación” y ahora
recuperamos los cambios aplicando el stash:
git switch feature
git stash list

Ahora ya podemos ver la recuperación de los cambios y con git stash pop simplemente aplicamos el
ultimo stash y lo borramos:
git stash pop

Ya hemos podido recuperar los cambios con el stash pero, ahora las ramas feature y master no se
encuentran en la misma posición, master cuenta un un nuevo hotfix que es el cambio a README.md
y feature cuenta con el archivo implementación.txt del cual master no sabe nada, ahora, para
sincronizar ambas ramas debemos hacer lo siguiente:
git add implementacion.txt
git commit -m "Continuacion de la funcionalidad iniciada antes del hotfix"

Hacemos un commit con la implementación. Después hacemos git merge master, de esta forma nos
traemos los cambios de master sin borrar la implementación, finalmente lo que podemos hacer
cuando en un escenario real terminemos la implementación, seria ir a master y hacer un merge de
feature

# 7. DESHACER

Primero, vamos a crear otra rama para hacer las pruebas, crearemos un cambio a revertir y haremos
el commit correspondiente:

git switch -c deshacer
echo "Cambio que revertiremos" >> revertir.txt
git add revertir.txt
git commit -m "Commit a revertir"

Ahora vamos empezar usando git revert, este comando no elimina otros commits, simplemente crea
un nuevo commit revirtiendo los cambios. Es decir, si usamos git revert Head, vamos a crear un
nuevo commit con los cambios del commit anterior pero no vamos a eliminar ningún commit:
Al poner git revert HEAD, veremos un pantalla que nos indica qeu se va a revertir

Tras escribir :wq en la pantalla que nos aparece veremos esto, ahora podemos poner git log –
oneline para ver que se nos ha creado un nuevo commit después del commit revertido. Tambien
podremos ver que el archivo que hicimos en el anterior commit ya no esta:
git log --oneline

Ahora para el comando git reset hacemos lo mismo, creamos un archivo y hacemos un commit:
echo "Cambio que eliminaremos en reset" >> reset.txt
git add reset.txt
git commit -m "Commit a eliminar"

La diferencia esencial que vamos a ver ahora es que el comando git reset SI elimina commits,
básicamente reinicia la rama al commit indicado, en el caso de –hard no solo mueve el puntero
HEAD al commit indicado si no que además los cambio hechos en los commits posteriores al commit
indicado también se pisan, se borran del directorio local y del área de staging (cosa que podemos
controlar cambiando el modo de reset)

Ejecutamos el comando git reset y como podemos ver ya no tenemos el archivo, ni en el directorio
local ni en el área de staging, tampoco podemos ver el commit que hicimos puesto que ha sido
borrado:
git reset --hard HEAD~1

Como podemos ver, volvemos a estar en el commit a revertir que fue revertido que es el resultado
del comando anterior, el commit nuevo que hicimos para el reset ya no esta y no hay rastro de el:
git log --oneline

# 8. RECUPERAR CON REFLOG
Ahora vamos a ejecutar el comando git reflog en el que vamos a ver el commit que hemos perdido
haciendo reset, en mi caso el hash de este es d02f6fe
git reflog
este comando muestra los logs de las referencias del puntero HEAD

Ahora, si ponemos git checkout al hash obtenido, nos aparece este mensaje que nos dice que
estamos en el estado deatached head lo cual quiere decir que no estamos como tal en ninguna
rama, ahora es recomendable crear una nueva rama para guardar permanentemente los cambios
recuperados:
git checkout d02f6fe
git switch -c rama-recuperada

Ahora para no tener esta rama temporal podemos simplemente cambiar a cualquier otra, hacer el
merge y eliminarla, en mi caso volveré a la rama deshacer que es la que use para el ejercicio
anterior aunque esta rama ahora ya que estamos le haremos merge también con feature y la
eliminaremos:
git switch deshacer
git merge rama-recuperada
git branch -d rama-recuperada
(Y asi con las demas ramas hasta tener solo main y feature de nuevo)

# 9. REMOTO
Vamos a enlazar nuestro repositorio local con uno remoto en github que acabo de crear con el
mismo nombre, para ello usamos los siguientes comandos. Primero creamos la referencia al
repositorio remoto con git remote y respues usamos git push con –u para enlazar la rama local con
la remota.
- NOTA: antes de esto he hecho un git switch master y he hecho un merge con feature para aplicar
definitivamente todos los cambios realizados en los ejercicios anteriores:
git remote add origin https://github.com/danielordaz/git-lab-comandos-ordaz
git push -u origin master

Ahora para probar el pull con –rebase vamos a crear un archivo simple en local y a hacer un commit
de el, después para simular un cambio desde otra maquina vamos a cambiar el README.md
después el navegador y luego vamos a hacer el pull con –rebase, lo bueno de esto es que evitamos
el commit extra de merge que github hubiera creado si simplemente hubiéramos usado git pull sin --rebase

Tras simular ambos commits en local y en el navegador simplemente hacemos git pull --rebase y podremos ver que no se ha creado un commit adicional, simplemente se ha traido el commit hecho en el navegador.


# 10. CHERRYPICK Y BISECT
Ahora vamos a probar el Cherry-pick, para ello vamos a crear una nueva rama que vamos a llamar
featureA, ahí haremos un cambio y un commit y después haremos otra rama que vamos a llamar
featureB, ahí para traer el cambio de feature A usaremos Cherry-pick y aplicaremos ese commit sin
hacer un merge:
git switch -c featureA
echo "Ajuste de configuración crucial" >> config_fix.txt
git add config_fix.txt
git commit -m "Hotfix de configuración urgente"

Ahora con git log –oneline obtenemos el hash del commit, en mi caso es: fc23e3d.
Después creamos la rama featureB desde la rama master (para que no estén los cambios
hechos en featureA) y ejecutamos el comando Cherry-pick:
git cherry-pick fc23e3d
Como ponemos ver los cambios del commit se aplican correctamente.


Ahora para probar el bisect, vamos a establecer el commit actual como “bad” y luego un commit
anterior como “Good” con su hash, tras esto, el comando nos va a llevar a diferentes commits entre
estos dos commtis en los que vamos a poder probar si el código o el proyecto funcionaba, si
funcionaba lo podemos poner como “Good” o si no funciona como “bad” hasta que cuando quede un
commit el comando nos dira cual es el commit problematico:
git bisect start
git bisect bad
De esta manera establecemos el commit actual como malo, luego, podemos
hacer un git log --oneline para ver los hashes y buscar el hash de un commit anterior. Después simplemente hacemos el siguiente comando:
git bisect good 2c0a76e
De esta manera el comando nos va mandando a diferentes commits donde
deberiamos ir probando el funcionamiento del proyecto, al finalizar
el comando nos da un resumen del commit culpable asi como el autor y la fecha.

# 11. LIMPIEZA CON CLEAN
Ahora vamos a probar el comando git clean, este nos permite limpiar los cambios que no estén rastreados (en el área de staging).
Primero creamos un archivo cualquiera y ejecutamos el comando git clean con el parámetro –n, de
esta manera se muestra lo qe se va a borrar con el comando, pero no se borra nada:
echo "temporal" > temp.txt
git clean -n

Luego ejecutamos git clean –fd, con este comando eliminamos los archivos no rastreados. Con la f
ponemos el modo forcé el cual es obligatorio con este comando a no ser que modifiquemos la
configuración de git, por otro lado la d elimina también directorios no rastreados:
git clean -fd

# 12. ETIQUETAS
Finalmente, probaremos el comando git tag, este comando es útil para añadir etiquetas a los
commits del repositorio, podemos hacer etiquetas ligeras y anotadas, las ligeras son una simple
nota, las anotadas son como un mini commit que tiene correo, fecha, autor...
git tag -a v1.0.0 -m "Versión 1.0.0 estable - Funcionalidad básica completa"
git tag beta-test-2
git tag
El parámetro –a indica que es una etiqueta anotada y si no ponemos nada será una etiquta ligera,
para ver las etiquetas podemos poner git tag