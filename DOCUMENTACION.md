# Documentacion — Hoja de Ruta Python Colombia

Guia completa para entender, mantener y actualizar la landing page.

---

## Estructura de archivos

```
HOJA_RUTA_PYTHON_COLOMBIA/
├── index.html        contenido y estructura de la pagina
├── styles.css        estilos visuales (dark/light automatico)
├── app.js            logica interactiva
└── DOCUMENTACION.md  este archivo
```

No hay build step, frameworks ni dependencias locales.
Para verla: abrir index.html directamente en el navegador.
Para el Playground Python: requiere conexion a internet (carga Pyodide desde CDN).

---

## Como correr localmente

Opcion 1 — abrir directo:
  Doble click en index.html. Funciona en Chrome, Edge y Firefox.

Opcion 2 — servidor local (recomendado para evitar restricciones CORS):
  python -m http.server 8000
  Luego abrir http://localhost:8000 en el navegador.

---

## Estructura de index.html

El archivo tiene esta organizacion de arriba hacia abajo:

  sidebar-fab
    Boton flotante que aparece solo en pantallas pequeñas (movil).
    Al tocarlo abre el menu lateral.

  sidebar-reopen
    Pestana pegada al borde izquierdo.
    Aparece en desktop cuando el sidebar esta cerrado.
    Al hacer click lo reabre.

  sidebar-overlay
    Fondo semitransparente que aparece detras del sidebar en movil.
    Al tocarlo cierra el menu.

  aside#sidebar
    Barra lateral de navegacion fija.
    Contiene los links a cada seccion, sub-links a Fase 3 y Fase 4,
    y la barra de progreso del checklist personal.

  div.layout
    Envuelve todo el contenido principal.
    Cuando el sidebar esta abierto en desktop, este div se desplaza
    hacia la derecha usando padding-left igual al ancho del sidebar.

  header.hero
    Cabecera con el titulo y descripcion general de la ruta.

  main
    Contiene todas las secciones de contenido en este orden:

    #principios
      5 tarjetas con los principios pedagogicos de la ruta
      y el resultado esperado al finalizar.

    #ruta
      Lista de 6 fases con su numero, nombre, descripcion,
      metadatos (horas, semanas, herramienta) y badge de estado.
      Badges posibles: Fase previa / Estas aca / Proximo / Por definir.
      Las fases 5 y 6 estan como placeholder hasta que se definan.

    #modulos
      Dos acordeones expandibles: Fase 3 y Fase 4.
      Cada uno contiene la ficha completa del modulo:
        - Tabla de datos clave (objetivo, duracion, herramienta, libro base)
        - Temas y subtemas con horas
        - Cronograma semana a semana
        - Proyectos de aplicacion
        - Reto final / Proyecto integrador
        - Tabla de distribucion de horas

    #py5tabla
      Tabla de equivalencias entre Processing (camelCase) y py5 (snake_case).
      Dividida en 4 categorias: Dibujo, Transformaciones, Color y Animacion.
      Es de consulta diaria durante la Fase 3.

    #repos
      Cards de los repositorios del track.
      Fase 3: disponible (enlace pendiente de agregar).
      Fase 4: proximo.
      Fases 5 y 6: por definir.

    #recursos
      4 grupos de recursos:
        - Libros base (Fase 3 y Fase 4)
        - Libros complementarios
        - Recursos de practica (Real Python, Exercism, Python Tutor, CS50P)
        - Documentacion oficial de Processing

    #docs-oficiales
      Tabla con 22 URLs de documentacion oficial para la Fase 4.
      Dividida en dos grupos:
        - Tutorial y libreria estandar (csv, json, os, pathlib, shutil, etc.)
        - Estandares y herramientas (PEP 8, PEP 257, Git, VS Code, etc.)

    #practica
      6 tarjetas clicables que abren plataformas de practica en nueva pestana.

    #hitos
      6 tarjetas con los hitos clave del programa.
      Las de Fase 5 y 6 son placeholders.

    #checklist
      Checklist personal de 70 items divididos en 6 bloques.
      El progreso se guarda en el navegador (localStorage).
      Bloque 0: Gestion del Entorno         (7 items)
      Bloque 1: Listas y Diccionarios        (17 items)
      Bloque 2: Persistencia y Archivos      (14 items)
      Bloque 3: Automatizacion de Procesos   (12 items)
      Bloque 4: Modularidad y Organizacion   (13 items)
      Bloque 5: Integracion Final            (7 items)

    #playground
      Editor de Python interactivo.
      Motor: Pyodide (Python compilado a WebAssembly, corre en el navegador).
      Sin backend ni instalacion.
      Atajos: Ctrl+Enter ejecuta, Tab inserta 4 espacios.

  footer
    Nota sobre como actualizar la pagina.

---

## Estructura de styles.css

El archivo empieza con tokens de color en :root.
Cambiar un valor ahi lo propaga a toda la pagina automaticamente.

Tokens principales:
  --bg            fondo principal
  --bg-2          fondo de tarjetas y tablas
  --bg-3          fondo de encabezados y hover
  --border        borde principal
  --border-soft   borde suave entre filas
  --text          texto principal
  --text-2        texto secundario
  --text-3        texto terciario / placeholders
  --accent        color de acento (verde bosque en light, verde claro en dark)
  --accent-2      acento hover
  --accent-light  fondo muy claro del acento (para highlights)
  --accent-inv    color del texto sobre fondo de acento
  --code-bg       fondo de bloques de codigo
  --code-text     color del texto en codigo inline
  --sidebar-w     ancho del sidebar (224px)

El dark mode es automatico via:
  @media (prefers-color-scheme: dark)
No requiere boton ni logica en JavaScript.

Bloques del CSS en orden:
  TOKENS          variables de color, espaciado y tipografia
  RESET           limpieza de margenes y box-sizing
  LAYOUT          sidebar + padding del contenido
  SIDEBAR         barra lateral, header, nav, progreso, overlay, FAB, reopen
  CONTENIDO       container, hero, sections
  PRINCIPIOS      grid de tarjetas de principios
  PHASES          lista de fases con numeros circulares y badges
  MODULE CARDS    acordeones expandibles
  TABLA PY5       grid de tablas de equivalencias
  REPOS           grid de cards de repositorios
  RESOURCES       lista de recursos y libros
  DOCS OFICIALES  tablas de documentacion oficial
  PRACTICA        cards clicables de plataformas
  MILESTONES      grid de hitos
  CHECKLIST       items con checkbox y barra de progreso
  PLAYGROUND      editor y panel de salida
  BOTONES HERRAMIENTA  tool-btn y phase-meta-link
  FOOTER          pie de pagina
  SCROLLBAR       estilo del scrollbar webkit
  RESPONSIVE      ajustes para pantallas pequeñas

---

## Estructura de app.js

El archivo tiene 5 funciones que se inicializan al cargar el DOM:

  initSidebar()
    Maneja la apertura y cierre del sidebar.
    Desktop (>= 900px):
      - Se abre por defecto al cargar la pagina.
      - El boton X dentro del header lo cierra.
      - Cuando esta cerrado, aparece la pestana sidebar-reopen en el borde
        izquierdo para reabrirlo.
      - Los clicks en links del sidebar NO lo cierran.
    Movil (< 900px):
      - Cerrado por defecto.
      - El FAB flotante lo abre.
      - El overlay y el X lo cierran.
      - Los clicks en links SI lo cierran.
    Al redimensionar la ventana ajusta el comportamiento automaticamente.

  initScrollSpy()
    Usa IntersectionObserver para detectar que seccion esta visible.
    Resalta el link correspondiente en el sidebar con la clase is-active.
    No requiere eventos de scroll, es eficiente en rendimiento.

  initModuleToggles()
    Maneja los acordeones de Fase 3 y Fase 4.
    Cada boton .module-toggle tiene un atributo data-target con el ID
    del contenido a mostrar u ocultar.
    Usa el atributo hidden del HTML para mostrar/ocultar.
    Actualiza aria-expanded para accesibilidad.
    Hace scroll suave al card cuando se abre.

  initChecklist()
    Carga el estado guardado en localStorage al abrir la pagina.
    Cada item usa como clave los primeros 90 caracteres de su texto.
    Al marcar o desmarcar un item lo guarda inmediatamente.
    Actualiza los contadores de cada bloque y la barra del sidebar.
    El boton Reiniciar borra todo el localStorage del checklist
    (con confirmacion previa).

  initPlayground()
    Precarga Pyodide cuando el usuario llega a la seccion #playground
    usando IntersectionObserver (no al cargar la pagina completa).
    Redirige sys.stdout y sys.stderr a un buffer interno para
    capturar la salida del codigo del usuario.
    Al ejecutar: limpia el buffer, corre el codigo, muestra la salida.
    Los errores de Pyodide se filtran para mostrar solo la parte
    relevante del traceback.

---

## Como actualizar el contenido

AGREGAR UNA FASE NUEVA (5 o 6):
  1. En index.html, buscar la fase con badge "Por definir" correspondiente.
  2. Reemplazar el h3 con el nombre real.
  3. Reemplazar el p con la descripcion.
  4. Cambiar el badge a badge--next o badge--active segun corresponda.
  5. Quitar la clase phase--future y su opacidad si ya esta activa.
  6. Agregar la card del repo en la seccion #repos.
  7. Actualizar el hito correspondiente en #hitos.

AGREGAR EL LINK DE UN REPO:
  En index.html, en la seccion #repos, cambiar:
    <span class="repo-link repo-link--disabled">proximamente</span>
  por:
    <a href="URL_DEL_REPO" class="repo-link" target="_blank" rel="noopener">ver repo</a>

CAMBIAR EL LINK DE GOOGLE COLAB:
  En index.html buscar "colab.research.google.com" y reemplazar la URL.

AGREGAR ITEMS AL CHECKLIST:
  1. En index.html, dentro del <ul id="block-bX"> correspondiente, agregar:
       <li><label><input type="checkbox" data-block="bX"> Texto del item.</label></li>
  2. En app.js, en el objeto BLOCK_TOTALS, incrementar el numero del bloque:
       var BLOCK_TOTALS = { b0: 7, b1: 17, ... }
  3. Actualizar la variable TOTAL si cambia el total de items.

CAMBIAR LA PALETA DE COLORES:
  En styles.css, modificar los valores en :root.
  El dark mode se ajusta en el bloque @media (prefers-color-scheme: dark).
  Los tokens --accent, --accent-2 y --accent-light controlan el color verde.

AGREGAR UN VIDEO DE REFERENCIA:
  En index.html, dentro de la ficha del modulo correspondiente,
  agregar una seccion con un iframe de YouTube o un enlace.
  Se puede agregar dentro de module-content despues de los proyectos.

---

## Dependencias externas

Pyodide (Playground Python):
  CDN: https://cdn.jsdelivr.net/pyodide/v0.27.5/full/pyodide.js
  Se carga de forma lazy, solo cuando el usuario llega a la seccion.
  Sin Pyodide el resto de la pagina funciona normalmente.
  Para actualizar la version: cambiar la URL en app.js en la variable PYODIDE_CDN.

Ninguna otra dependencia externa. Sin jQuery, sin frameworks, sin npm.

---

## Compatibilidad

Funciona en Chrome, Edge, Firefox y Safari modernos.
El dark/light mode es automatico segun la preferencia del sistema operativo.
En movil el sidebar se convierte en un drawer con overlay.
El Playground requiere un navegador con soporte de WebAssembly (todos los modernos).
