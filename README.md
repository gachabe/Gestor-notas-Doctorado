# Cuaderno de notas del doctorado

Guía de uso. Aquí no se apuntan tareas: los pendientes viven en `diario.tex`.

## 1. Qué hay y para qué sirve

```
notas-doctorado/
├── notacion.sty   ← tus macros de notación + todos los comandos del sistema
├── notas/         ← tus notas, un fichero por tema (fca.tex, dl.tex...)
├── diario.tex     ← diario y pendientes, una entrada por sesión
├── vista.tex      ← lo que quieres ver hoy (este es el que se compila)
├── refs.bib       ← bibliografía
└── .vscode/       ← configuración de VS Code (compila al guardar)
```

| Pieza | Qué haces con ella |
|---|---|
| `notas/*.tex` | Escribes aquí cada nota: definición, lema, idea... |
| `vista.tex` | Eliges qué notas se muestran hoy. Es lo único que se compila. |
| `diario.tex` | Una entrada por sesión; ahí van los pendientes. |
| `notacion.sty` | Añades tus macros (`\N`, `\Pot`...) y, si quieres, tipos de nota. |

La idea: cada nota se **escribe una vez** y se **recuerda** donde haga falta. Si la corriges, se corrige en todas partes.

## 2. Instalar y usar LaTeX en VS Code (una vez por PC)

Repite esto en cada PC (casa y despacho).

### 2.1 Instalar TeX (el compilador)

**Windows**: instala **TeX Live** (trae Perl, que `latexmk` necesita). Descarga `install-tl-windows.exe` desde https://tug.org/texlive/windows.html y ejecútalo con la instalación completa (ocupa varios GB y tarda). Alternativa: MiKTeX (https://miktex.org), pero entonces tienes que instalar Perl aparte (por ejemplo Strawberry Perl).

**macOS**: instala MacTeX desde https://tug.org/mactex/ o con Homebrew:

```
brew install --cask mactex-no-gui
```

**Linux (Debian/Ubuntu)**:

```
sudo apt install texlive-full latexmk
```

Cuando termine, **cierra y vuelve a abrir** la terminal (o reinicia el PC) y comprueba que existen estos comandos:

```
latexmk -v
pdflatex --version
```

Si dice que no los reconoce, TeX no está en el PATH: reinicia, y en Windows comprueba que la carpeta `bin` de TeX Live está en el PATH (el instalador lo hace solo).

Si instalaste una versión reducida (BasicTeX, MiKTeX) y falla por un paquete que falta, instálalo con:

```
sudo tlmgr install babel-spanish environ
```

### 2.2 Instalar VS Code y la extensión

1. Descarga VS Code: https://code.visualstudio.com
2. Abre VS Code, pulsa `Ctrl+Shift+X` (extensiones), busca **LaTeX Workshop** (autor: James Yu) y pulsa *Install*. O desde la terminal:

```
code --install-extension James-Yu.latex-workshop
```

### 2.3 Primer uso

1. `Archivo > Abrir carpeta` y elige la carpeta del proyecto (`notas-doctorado`).
2. Abre `vista.tex`.
3. Guarda con `Ctrl+S`. Se compila solo (la primera vez tarda más). Abajo, en la barra de estado, verás que está compilando.
4. Abre el PDF con `Ctrl+Alt+V`. Se abre en una pestaña: arrástrala a la derecha para tenerla al lado del código.
5. Prueba: abre `notas/ejemplo.tex`, cambia una palabra y guarda. Se compila esa nota sola y su PDF se abre con `Ctrl+Alt+V` (apartado 4.1).

Para compilar a mano: `Ctrl+Alt+B`.

### 2.4 Escribir LaTeX en VS Code

- **Autocompletado**: al escribir `\` salen sugerencias de comandos (`Ctrl+Espacio` las abre). Los entornos propios de este proyecto (`lema`, `idea`...) puede que no los sugiera; escríbelos a mano.
- **Errores**: panel *Problems* (`Ctrl+Shift+M`). Indica fichero y línea. El log completo está en `Ver > Salida`, eligiendo «LaTeX Compiler» en el desplegable.
- **Saltar entre código y PDF**: `Ctrl+clic` en el PDF te lleva a esa línea del código; `Ctrl+Alt+J` te lleva del código al PDF.
- **Terminal integrada**: `Ctrl+ñ` (o `Ctrl+\``) para ejecutar los comandos de git sin salir de VS Code.

### 2.5 Git

1. Instala Git: https://git-scm.com
2. Crea una cuenta en GitHub si no la tienes.
3. Los comandos están en el apartado 10. También puedes usar el panel *Source Control* de VS Code (`Ctrl+Shift+G`).

### 2.6 Si falla la compilación

- **No compila y no hay PDF**: comprueba `latexmk -v` en la terminal. Si no existe, TeX no está instalado o no está en el PATH; reinicia VS Code o el PC.
- **Error que menciona Perl** (Windows): instala Perl (TeX Live ya lo trae).
- **Falta un paquete**: instálalo con `tlmgr` (ver 2.1).
- **Cualquier otro error**: mira el panel *Problems* y el log del apartado 2.4.

## 3. Rutina de una sesión

1. `git pull` (trae lo que hiciste en el otro PC).
2. Abre `vista.tex` y deja en la sección «Hoy» las notas que necesitas.
3. Trabaja. Si algo vale la pena, escríbelo como nota en `notas/` (apartado 4).
4. Al acabar, una entrada en `diario.tex` (apartado 7).
5. `git add .`, `git commit`, `git push` (apartado 10).

## 4. Crear una nota

Dentro de un fichero de `notas/`:

```latex
\begin{lema}[Nombre opcional]{clave-unica}
Enunciado.
\end{lema}
\begin{proof}
Demostración (opcional, va fuera del entorno).
\end{proof}
```

- `[Nombre opcional]` aparece en el título: «Lema (Nombre opcional)». Puedes omitirlo: `\begin{lema}{clave-unica}`.
- `{clave-unica}` es el nombre con el que la recordarás después. Sin espacios, sin acentos y sin `_`. Los guiones valen. No puede repetirse (si se repite, LaTeX avisa con «Clave repetida»).
- La demostración **no** se guarda con la nota: solo se ve al leer el fichero entero (apartado 6).

Tipos disponibles:

| Entorno | Se imprime como |
|---|---|
| `definicion` | Definición |
| `concepto` | Concepto |
| `teorema` | Teorema |
| `proposicion` | Proposición |
| `lema` | Lema |
| `corolario` | Corolario |
| `conjetura` | Conjetura |
| `idea` | Idea |
| `pregunta` | Pregunta |

Los resultados de otros se escriben igual, con `\cite{...}` dentro (apartado 8).

**Fichero de notas nuevo** (por ejemplo `notas/fca.tex`):

1. Créalo en `notas/`.
2. Pon estas líneas **al principio** (cambia `fca` por el nombre del fichero):
   ```latex
   % !TEX root = fca.tex
   \ifdefined\notaenvista\else\documentclass[11pt]{article}\usepackage{../notacion}\begin{document}\def\notasola{}\fi
   ```
3. Y esta línea **al final**:
   ```latex
   \ifdefined\notasola\end{document}\fi
   ```
4. En `vista.tex`, añade en la sección 1:
   ```latex
   \cargar{notas/fca}
   ```

Lo más fácil es copiar `notas/ejemplo.tex`, cambiar el nombre y borrar su contenido dejando esas líneas.

### 4.1 PDF de una nota por separado

Con las líneas anteriores, cada fichero de `notas/` se compila **solo**: abre `notas/fca.tex`, guarda (`Ctrl+S`) y pulsa `Ctrl+Alt+V`. Sale un PDF con todo el fichero, demostraciones incluidas, que se guarda como `notas/fca.pdf` (no se sube a git).

Dos cosas:

- Al compilar una nota sola, un `\recordar` de una nota que está en otro fichero no funciona. Para eso está `vista.tex`.
- Si prefieres que al guardar una nota se compile `vista` en vez de la nota, cambia la primera línea del fichero por `% !TEX root = ../vista.tex`.

Hay un `notas/ejemplo.tex` con una definición, un lema con demostración y una idea. Bórralo cuando tengas los tuyos (y quita su `\cargar` y los `\recordar` de ejemplo en `vista.tex`).

## 5. Ver notas en la vista

En `vista.tex`:

```latex
\cargar{notas/fca}      % carga el fichero en silencio (no imprime nada)
\recordar{clave-unica}  % imprime el enunciado de esa nota
```

- Todos los `\cargar` van **antes** de los `\recordar`.
- Un `\recordar` con una clave que no existe imprime en rojo «No existe la nota». Revisa la clave o que el fichero esté cargado.
- Cambias qué ves tocando solo las líneas `\recordar`.

## 6. Leer un fichero entero

Mientras escribes en un fichero, para comprobar que compila bien (con demostraciones y todo), en `vista.tex` cambia `\cargar` por `\leer`:

```latex
\leer{notas/fca}
```

Al terminar vuelves a `\cargar`. No uses `\cargar` y `\leer` del mismo fichero a la vez (daría «Clave repetida»).

## 7. Diario y pendientes

En `diario.tex`, al final del fichero:

```latex
\dia{2026-10-19}
Lo que he hecho hoy, decisiones y por qué.
\pendiente{lo que me falta por hacer}
```

- `\pendiente{...}` funciona en cualquier fichero cargado (también dentro de una nota o de una demostración): deja el recordatorio justo donde está el hueco.
- Todos los pendientes de los ficheros cargados salen en la lista `\pendientes` de `vista.tex`, con el fichero donde están.
- Para quitar un pendiente, bórralo del fichero.
- `diario.tex` se carga con `\cargar{diario}` (ya está en `vista.tex`).

## 8. Citas

1. Añade la entrada a `refs.bib`.
2. Cita con `\cite{clave}` dentro de una nota.
3. En `vista.tex`, descomenta las dos líneas de «BIBLIOGRAFÍA» (solo cuando ya haya alguna cita; sin citas, BibTeX da error).

La bibliografía incluye todo lo citado en los ficheros cargados, aunque no lo recuerdes.

## 9. Notación y tipos nuevos

- **Macro nueva**: en `notacion.sty`, bajo «TU NOTACIÓN»:
  ```latex
  \newcommand{\cl}{\mathrm{cl}}
  ```
- **Tipo de nota nuevo**: en `notacion.sty`, junto a los demás `\definirtipo`:
  ```latex
  \definirtipo{observacion}{Observación}
  ```
  Antes de esa línea, `\theoremstyle{plain}` (cursiva, para enunciados) o `\theoremstyle{definition}` (texto normal) decide el aspecto.

## 10. Git

**Primera vez** (crea antes en GitHub un repositorio *privado* vacío llamado `notas-doctorado`, sin README ni licencia). En la carpeta del proyecto:

```
git init
git add .
git commit -m "Estructura inicial"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/notas-doctorado.git
git push -u origin main
```

**Segundo PC** (una vez):

```
git clone https://github.com/TU_USUARIO/notas-doctorado.git
```

**Cada sesión**, al empezar:

```
git pull
```

Al terminar:

```
git add .
git commit -m "Notas de hoy"
git push
```

Si te olvidas el `push` y editas el mismo fichero en el otro PC, `git pull` puede avisar de un conflicto: abre el fichero, verás marcas `<<<<<<<` y `>>>>>>>` con las dos versiones, deja la que valga y borra las marcas.

Los PDF y ficheros auxiliares no se suben (`.gitignore`): cada PC compila los suyos.

## 11. Referencia rápida

| Comando | Dónde | Qué hace |
|---|---|---|
| `\begin{tipo}[Nombre]{clave}...\end{tipo}` | notas | Crea una nota |
| `\cargar{notas/fichero}` | `vista.tex` | Carga un fichero en silencio |
| `\leer{notas/fichero}` | `vista.tex` | Imprime el fichero entero |
| `\recordar{clave}` | `vista.tex` | Imprime el enunciado de una nota |
| `\dia{fecha}` | `diario.tex` | Abre una entrada del diario |
| `\pendiente{texto}` | cualquier fichero | Marca un pendiente |
| `\pendientes` | `vista.tex` | Lista todos los pendientes |
| `\definirtipo{entorno}{Título}` | `notacion.sty` | Crea un tipo de nota |

## 12. Si algo falla

- **«No existe la nota»** (en rojo): clave mal escrita o fichero sin `\cargar`.
- **«Clave repetida»** (aviso en el log): dos notas con la misma clave, o un fichero cargado dos veces.
- **No compila**: abre la pestaña *Problems* o el log de LaTeX Workshop; el error indica fichero y línea.
- **Un pendiente sale duplicado**: el fichero se está cargando dos veces.
