# Git Essential Training: apuntes del curso

Resumen explicado de los contenidos del curso *Git Essential Training* de LinkedIn Learning. Combina los temas de las dos versiones del curso (la clásica sobre arquitectura de Git y la más reciente con GitHub Codespaces).

## Índice

1. [¿Qué es Git y por qué usarlo?](#1-qué-es-git-y-por-qué-usarlo)
2. [Cómo funciona Git](#2-cómo-funciona-git)
3. [Instalación y configuración](#3-instalación-y-configuración)
4. [Conceptos y arquitectura](#4-conceptos-y-arquitectura)
5. [Primeros pasos: repositorio y primer commit](#5-primeros-pasos-repositorio-y-primer-commit)
6. [Trabajar con un repositorio remoto](#6-trabajar-con-un-repositorio-remoto)
7. [Hacer cambios en los archivos](#7-hacer-cambios-en-los-archivos)
8. [Deshacer cambios y volver atrás](#8-deshacer-cambios-y-volver-atrás)
9. [Ignorar archivos con .gitignore](#9-ignorar-archivos-con-gitignore)
10. [Ramas y fusiones](#10-ramas-y-fusiones)
11. [Buenos mensajes de commit](#11-buenos-mensajes-de-commit)
12. [Salir de problemas comunes](#12-salir-de-problemas-comunes)
13. [Chuleta de comandos](#13-chuleta-de-comandos)

---

## 1. ¿Qué es Git y por qué usarlo?

**Git** es un sistema de control de versiones: un programa que registra los cambios que haces en tus archivos a lo largo del tiempo, de modo que puedes consultar versiones anteriores, compararlas o recuperarlas.

### Control de versiones
Sin control de versiones, la gente acaba con archivos como `informe_final.docx`, `informe_final_v2.docx` y `informe_final_DEFINITIVO.docx`. Git reemplaza eso por un historial ordenado: cada cambio importante queda guardado como una "foto" del proyecto con autor, fecha y descripción.

### Compartir código
Con Git puedes alojar tu proyecto en un servicio como GitHub, GitLab o Bitbucket y compartirlo con otras personas.

### Colaborar
Varias personas pueden trabajar a la vez en el mismo proyecto sin pisarse. Cada una trabaja en su copia y Git ayuda a combinar los cambios después.

### Código abierto
Gran parte del software de código abierto se desarrolla con Git. Saber usarlo te permite contribuir a esos proyectos.

### Un poco de historia
Git lo creó Linus Torvalds en 2005 para gestionar el desarrollo del kernel de Linux, buscando velocidad, simplicidad y soporte para miles de ramas de trabajo en paralelo.

---

## 2. Cómo funciona Git

### Uso local
Git funciona perfectamente en tu ordenador, sin internet y sin cuenta en ningún servicio. El historial se guarda dentro de tu propio proyecto.

### Uso con un proveedor
Servicios como **GitHub** añaden un repositorio remoto en la nube, además de herramientas de colaboración (issues, pull requests, etc.). **Git y GitHub no son lo mismo**: Git es la herramienta, GitHub es un servicio que la aloja.

### Control de versiones distribuido
En un sistema centralizado, el historial vive solo en un servidor. En Git, **cada persona tiene una copia completa del repositorio**, con todo su historial. Esto permite:

- Trabajar sin conexión.
- Operaciones muy rápidas, porque casi todo es local.
- Copias de seguridad naturales: si el servidor se pierde, cualquier clon sirve para recuperarlo.

### Cómo empezar a trabajar con Git
Hay dos puntos de partida:

- **Proyecto nuevo:** `git init` crea un repositorio vacío en tu carpeta.
- **Proyecto existente:** `git clone <url>` descarga una copia de un repositorio remoto.

---

## 3. Instalación y configuración

### Instalar Git
- **Windows:** descargar el instalador desde [git-scm.com](https://git-scm.com) (incluye Git Bash).
- **macOS:** `brew install git`, o instalar las Xcode Command Line Tools con `xcode-select --install`.
- **Linux (Debian/Ubuntu):** `sudo apt install git`. En Fedora: `sudo dnf install git`.

Comprueba la instalación:

```bash
git --version
```

### Clientes gráficos
Git se usa sobre todo desde la terminal, pero existen interfaces gráficas (GitHub Desktop, GitKraken, Sourcetree) y soporte integrado en editores como **Visual Studio Code**.

### Configuración básica
Git necesita saber quién eres, porque tu nombre y email quedan registrados en cada commit:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

Para ver toda la configuración:

```bash
git config --list
```

Niveles de configuración:

| Opción | Alcance |
|---|---|
| `--system` | Todos los usuarios de la máquina |
| `--global` | Tu usuario en esa máquina |
| `--local` | Solo el repositorio actual (valor por defecto) |

Opcional pero recomendable, fijar el nombre de la rama inicial y el editor:

```bash
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
```

### Autocompletado
Git incluye scripts de autocompletado para Bash y Zsh, que te permiten pulsar `Tab` para completar comandos, nombres de ramas, etc.

### Ayuda integrada
```bash
git help <comando>     # manual completo
git <comando> -h       # resumen corto
```
Ejemplo: `git help commit`.

---

## 4. Conceptos y arquitectura

### Los tres árboles
Un repositorio Git gestiona tres "zonas" por las que pasan tus cambios:

| Zona | También llamada | Qué contiene |
|---|---|---|
| **Directorio de trabajo** | Working directory | Los archivos tal como los ves y editas |
| **Área de preparación** | Staging area / index | Los cambios que has marcado para el próximo commit |
| **Repositorio** | Repository / HEAD | El historial de commits ya guardados |

### Flujo de trabajo básico

```
Directorio de trabajo --(git add)--> Staging --(git commit)--> Repositorio
```

1. Editas archivos (directorio de trabajo).
2. Eliges qué cambios incluir con `git add` (staging).
3. Los guardas definitivamente con `git commit` (repositorio).

El área de preparación te permite construir commits limpios, incluyendo solo los cambios relacionados entre sí.

### Hashes (SHA-1)
Cada commit se identifica con un **hash**, una cadena de 40 caracteres hexadecimales como `a1b2c3d4e5f6...`. Se calcula a partir del contenido, así que es único y garantiza la integridad: si algo cambia, el hash cambia. En la práctica basta con usar los primeros 7 caracteres.

> Git está migrando gradualmente a SHA-256, pero SHA-1 sigue siendo el predeterminado.

### El puntero HEAD
**HEAD** apunta al commit en el que estás ahora mismo, normalmente el último de la rama actual. Es el "tú estás aquí" del historial. Cuando haces un commit nuevo, HEAD avanza con él.

---

## 5. Primeros pasos: repositorio y primer commit

### Inicializar un repositorio
```bash
mkdir mi-proyecto
cd mi-proyecto
git init
```

### Dónde guarda Git sus archivos
`git init` crea una carpeta oculta llamada **`.git`**. Contiene todo el historial, la configuración y los objetos del repositorio. Si la borras, el proyecto deja de ser un repositorio Git (pierdes el historial, pero tus archivos permanecen). **No edites su contenido a mano.**

### Primer commit
```bash
echo "# Mi proyecto" > README.md
git add README.md
git commit -m "Añade README inicial"
```

### Ver el historial
```bash
git log                 # historial completo
git log --oneline       # una línea por commit
git log --oneline --graph --all   # con forma de árbol de ramas
```

---

## 6. Trabajar con un repositorio remoto

### Crear un repositorio remoto
Crea un repositorio vacío en GitHub (botón **New repository**).

### Clonar
```bash
git clone https://github.com/usuario/repositorio.git
```
Descarga el proyecto con todo su historial y deja configurado el remoto con el nombre `origin`.

### Crear, preparar, confirmar y subir
```bash
echo "hola" > archivo.txt
git add archivo.txt
git commit -m "Añade archivo.txt"
git push
```

### Conectar un repositorio local con uno remoto
Si empezaste con `git init` y ahora quieres subirlo:

```bash
git remote add origin https://github.com/usuario/repositorio.git
git branch -M main
git push -u origin main
```

`-u` guarda la relación entre tu rama local y la remota, de modo que después basta con `git push` y `git pull`.

### Traer cambios del remoto
```bash
git pull      # descarga y fusiona
git fetch     # solo descarga, sin fusionar
```

### Ver remotos configurados
```bash
git remote -v
```

---

## 7. Hacer cambios en los archivos

### Estado del repositorio
```bash
git status
```
Es el comando que más usarás. Te dice qué archivos están modificados, cuáles están en staging y cuáles no están siendo seguidos.

Estados de un archivo:

- **Untracked:** nuevo, Git aún no lo sigue.
- **Modified:** seguido y cambiado, pero no preparado.
- **Staged:** preparado para el próximo commit.
- **Committed:** guardado en el historial.

### Añadir archivos
```bash
git add archivo.txt      # un archivo
git add carpeta/         # una carpeta
git add .                # todo lo del directorio actual
```

### Editar y ver cambios (`git diff`)
```bash
git diff                 # cambios no preparados (working vs staging)
git diff --staged        # cambios preparados (staging vs último commit)
```
Las líneas con `-` se eliminaron y las líneas con `+` se añadieron.

### Atajo: preparar y confirmar a la vez
```bash
git commit -a -m "Mensaje"
```
Prepara automáticamente los archivos **ya seguidos** que se hayan modificado. **No incluye archivos nuevos.**

### Ver un commit concreto y comparar commits
```bash
git show a1b2c3d             # detalles y cambios de un commit
git diff a1b2c3d e4f5g6h     # diferencias entre dos commits
git diff HEAD~1 HEAD         # último commit contra el anterior
```

### Borrar archivos
```bash
git rm archivo.txt
git commit -m "Elimina archivo.txt"
```
`git rm` borra el archivo y prepara la eliminación. Si lo borras a mano, tendrás que hacer después `git add` o `git rm` para registrarlo.

### Mover y renombrar archivos
```bash
git mv nombre_viejo.txt nombre_nuevo.txt
git commit -m "Renombra archivo"
```
Git no guarda el renombrado como tal: detecta que el contenido es el mismo y lo interpreta como un movimiento.

### Trabajar con carpetas
Git **no registra carpetas vacías**, solo archivos. Para conservar una carpeta vacía, la convención es añadir dentro un archivo `.gitkeep`.

### Historial
```bash
git log --oneline
git log -p               # con los cambios de cada commit
git log --author="Ana"
git log --since="2 weeks ago"
```

---

## 8. Deshacer cambios y volver atrás

Qué comando usar depende de **en qué punto está el cambio**:

| Situación | Comando |
|---|---|
| Descartar cambios en el directorio de trabajo | `git restore archivo.txt` |
| Sacar un archivo de staging (sin perder cambios) | `git restore --staged archivo.txt` |
| Corregir el último commit (mensaje o contenido) | `git commit --amend` |
| Deshacer un commit **ya compartido** creando uno nuevo | `git revert <hash>` |
| Mover la rama a otro commit (reescribe historia) | `git reset` |

> ⚠️ `git restore archivo.txt` **descarta los cambios sin posibilidad de recuperarlos**, porque nunca llegaron a guardarse en Git.

### Mirar atrás en el historial
```bash
git log --oneline
git show <hash>
git checkout <hash>      # ver el proyecto en ese momento (HEAD desacoplado)
git switch -             # volver a la rama anterior
```

### Volver a un estado anterior con `git revert`
```bash
git revert <hash>
```
No borra nada del historial: **crea un commit nuevo** que invierte los cambios del commit indicado. Es la forma segura de deshacer algo que ya has subido al remoto.

### `git reset`: tres modos

| Modo | Mueve HEAD | Staging | Directorio de trabajo |
|---|---|---|---|
| `--soft` | Sí | Se mantiene | Se mantiene |
| `--mixed` (por defecto) | Sí | Se vacía | Se mantiene |
| `--hard` | Sí | Se vacía | **Se pierde todo** |

```bash
git reset --soft HEAD~1    # deshace el commit, conserva todo preparado
git reset --hard HEAD~1    # deshace el commit y borra los cambios
```

> ⚠️ **No uses `reset` sobre commits ya subidos** a un remoto compartido: reescribe la historia y causa problemas a tu equipo. En ese caso usa `revert`.

---

## 9. Ignorar archivos con .gitignore

Hay archivos que no deben ir al repositorio: dependencias (`node_modules/`), archivos generados, credenciales, configuración del editor, logs...

Crea un archivo `.gitignore` en la raíz del proyecto:

```gitignore
# Dependencias
node_modules/

# Variables de entorno y secretos
.env

# Archivos del sistema
.DS_Store
Thumbs.db

# Logs
*.log

# Carpeta de compilación
build/
dist/

# Pero sí incluir este archivo concreto
!importante.log
```

### Reglas de patrones
| Patrón | Significado |
|---|---|
| `*.log` | Cualquier archivo que acabe en `.log` |
| `carpeta/` | Una carpeta y todo su contenido |
| `/archivo.txt` | Solo en la raíz del repositorio |
| `!archivo.txt` | Excepción: **sí** incluir |
| `#` | Comentario |

### Importante
`.gitignore` solo afecta a archivos que **aún no están seguidos**. Si un archivo ya se había subido, hay que dejar de seguirlo:

```bash
git rm --cached archivo.txt        # lo quita del índice pero no del disco
git rm -r --cached carpeta/
```

Consejo: en [github.com/github/gitignore](https://github.com/github/gitignore) hay plantillas para casi cualquier lenguaje o framework.

---

## 10. Ramas y fusiones

### Qué es una rama
Una **rama** es una línea de desarrollo independiente. Te permite probar ideas o desarrollar una función sin tocar el código estable. Técnicamente, es solo un puntero a un commit, por eso crear ramas en Git es instantáneo y barato.

La rama principal suele llamarse `main` (antes, `master`).

### Comandos básicos
```bash
git branch                    # lista las ramas (* marca la actual)
git branch nueva-funcion      # crea una rama
git switch nueva-funcion      # cambia a ella
git switch -c nueva-funcion   # crea y cambia en un solo paso
git branch -d nueva-funcion   # borra una rama ya fusionada
git branch -D nueva-funcion   # fuerza el borrado
```
(`git checkout` hace lo mismo que `switch`, y es el comando más antiguo.)

### Fusionar (merge)
Para integrar los cambios de una rama en otra:

```bash
git switch main
git merge nueva-funcion
```

Tipos de fusión:

- **Fast-forward:** si `main` no ha avanzado desde que creaste la rama, Git simplemente mueve el puntero. No se crea commit de fusión.
- **Merge con commit de fusión (three-way):** si ambas ramas han avanzado, Git crea un commit especial que une los dos historiales.

### Conflictos
Si dos ramas modifican las mismas líneas, Git no sabe cuál elegir y marca un **conflicto**:

```
<<<<<<< HEAD
versión de tu rama actual
=======
versión de la rama que fusionas
>>>>>>> nueva-funcion
```

Para resolverlo:
1. Abre el archivo y decide qué versión (o combinación) se queda.
2. Borra los marcadores `<<<<<<<`, `=======` y `>>>>>>>`.
3. `git add archivo.txt`
4. `git commit` (o `git merge --continue`).

Si te asustas: `git merge --abort` te devuelve al estado anterior al merge.

### Flujo habitual
```bash
git switch -c arreglo-login       # 1. rama nueva
# ...trabajas y haces commits...
git switch main                   # 2. vuelves a main
git pull                          # 3. te actualizas
git merge arreglo-login           # 4. fusionas
git push                          # 5. subes
git branch -d arreglo-login       # 6. limpias
```

---

## 11. Buenos mensajes de commit

Un commit es una unidad de cambio con sentido, y su mensaje debe explicar **qué** y **por qué**, no cómo (el cómo ya lo dice el código).

### Estructura recomendada
```
Resumen corto en una línea (máx. ~50 caracteres)

Descripción más detallada si hace falta, separada por una
línea en blanco y ajustada a ~72 caracteres por línea.
Explica el motivo del cambio y cualquier efecto relevante.
```

### Commits con varias líneas
Si usas `git commit` sin `-m`, se abre tu editor. También puedes pasar varios `-m`:

```bash
git commit -m "Corrige validación del formulario" -m "El email aceptaba espacios al inicio, lo que provocaba errores en el login."
```

### Reglas útiles
- Usa el **modo imperativo**: "Añade validación", no "Añadí validación" ni "Añadiendo validación".
- Sé específico: `Corrige error de redondeo en el total` es mejor que `Arreglos`.
- **Un commit = un cambio lógico.** Evita commits gigantes con mil cosas mezcladas.
- No incluyas secretos ni datos personales.

Ejemplos:

| ❌ Mal | ✅ Bien |
|---|---|
| `cambios` | `Añade botón de logout a la cabecera` |
| `fix` | `Corrige cálculo del IVA en carrito vacío` |
| `asdfgh` | `Actualiza dependencias de seguridad` |

---

## 12. Salir de problemas comunes

| Problema | Solución |
|---|---|
| Me equivoqué en el mensaje del último commit | `git commit --amend -m "Mensaje correcto"` (solo si no lo has subido) |
| Olvidé añadir un archivo al último commit | `git add archivo` y luego `git commit --amend --no-edit` |
| Hice `git add` de un archivo por error | `git restore --staged archivo` |
| Quiero descartar mis cambios locales de un archivo | `git restore archivo` |
| Quiero deshacer el último commit pero conservar los cambios | `git reset --soft HEAD~1` |
| Subí algo que no debía | `git revert <hash>` (y si era un secreto, **cámbialo**: sigue en el historial) |
| Estoy en medio de un merge conflictivo y quiero salir | `git merge --abort` |
| Git rechaza el `push` ("rejected", "non-fast-forward") | Haz `git pull` primero para integrar los cambios remotos |
| Estoy en "detached HEAD" | `git switch -c nombre-rama` para conservar lo hecho, o `git switch main` para salir |
| Hice commit en la rama equivocada | Crea una rama donde estás (`git branch correcta`), y luego mueve la anterior con `git reset --hard <hash>` |
| He borrado algo y creo que lo perdí | `git reflog` muestra todos los movimientos de HEAD, incluidos los "perdidos" |

### El salvavidas: `git reflog`
```bash
git reflog
```
Git registra cada vez que HEAD se mueve. Aunque hayas hecho un `reset --hard`, el commit suele seguir recuperable durante semanas:

```bash
git reset --hard <hash-del-reflog>
```

---

## 13. Chuleta de comandos

### Configuración
```bash
git config --global user.name "Nombre"
git config --global user.email "email"
git config --list
```

### Empezar
```bash
git init
git clone <url>
```

### Ciclo diario
```bash
git status
git add <archivo>        # o: git add .
git commit -m "Mensaje"
git push
git pull
```

### Inspeccionar
```bash
git log --oneline --graph --all
git diff
git diff --staged
git show <hash>
```

### Archivos
```bash
git rm <archivo>
git mv <origen> <destino>
```

### Deshacer
```bash
git restore <archivo>
git restore --staged <archivo>
git revert <hash>
git reset --soft HEAD~1
git commit --amend
```

### Ramas
```bash
git branch
git switch -c <rama>
git switch <rama>
git merge <rama>
git branch -d <rama>
```

### Remotos
```bash
git remote -v
git remote add origin <url>
git push -u origin main
git fetch
```

---

## Recursos para profundizar

- [Pro Git](https://git-scm.com/book/es/v2): el libro oficial y gratuito, disponible en español.
- [Documentación oficial de Git](https://git-scm.com/docs)
- [Learn Git Branching](https://learngitbranching.js.org/?locale=es_ES): práctica visual e interactiva.
- [Plantillas de .gitignore](https://github.com/github/gitignore)
