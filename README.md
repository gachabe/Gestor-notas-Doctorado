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

## 2. Instalación (una vez por PC)

1. **Una distribución de TeX**
   - Windows: MiKTeX o TeX Live. Si la compilación falla por `latexmk`, hace falta Perl (MiKTeX no lo trae).
   - macOS: MacTeX.
   - Linux (Debian/Ubuntu):
     ```
     sudo apt install texlive-full latexmk
     ```
2. **VS Code**, y dentro la extensión **LaTeX Workshop** (autor: James Yu). Se instala desde la pestaña de extensiones (`Ctrl+Shift+X`).
3. **Git**, y una cuenta de GitHub.

Para trabajar: `Archivo > Abrir carpeta` y abre la carpeta del proyecto.
Abre cualquier `.tex`, y al **guardar** (`Ctrl+S`) se compila `vista.tex` automáticamente, aunque estés en otro fichero (lo indica la primera línea `% !TEX root = ...` de cada uno).
El PDF se abre en una pestaña con `Ctrl+Alt+V`. Arrastra esa pestaña a la derecha para tenerla al lado del editor.

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
2. Pon como primera línea:
   ```latex
   % !TEX root = ../vista.tex
   ```
3. En `vista.tex`, añade en la sección 1:
   ```latex
   \cargar{notas/fca}
   ```

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
