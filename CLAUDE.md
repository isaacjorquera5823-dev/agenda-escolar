# Agenda escolar de Daniel y Benja

Contexto traspasado desde un chat de claude.ai (7 de octubre de 2026). Léelo antes de hacer cambios.

## Sobre el usuario
- Isaac, papá de Daniel y Benjamín (Benja). No programa: explícale en español simple, paso a paso, sin jerga.
- Trabaja de forma iterativa: propón opciones, deja que elija y luego implementa.
- Antes de publicar algo (commit/push), cuéntale qué vas a cambiar y pide su visto bueno.

## Qué es
App web para seguir el avance escolar de los dos niños, que estudian en la plataforma BSchool.
- Publicada: https://isaacjorquera5823-dev.github.io/agenda-escolar/
- Repositorio: `isaacjorquera5823-dev/agenda-escolar` (GitHub Pages, rama `main`)
- Todo vive en un solo archivo: `index.html` (HTML + CSS + JS).
- Sincronización en tiempo real entre dispositivos con Firebase Firestore. Las actividades marcadas se guardan en Firestore como una lista de IDs (`checked`), no en el archivo. Cambiar `index.html` no borra el avance.

## Estructura de datos
- `const DATA = [...]` dentro de `index.html`, en una sola línea muy larga. Cada actividad tiene:
  `id`, `c` (hijo: `Daniel` o `Benja`), `g` (curso), `a` (nombre de la actividad), `s` (asignatura), `w` (semana), `f1`/`f2` (fechas de la semana), `e`.
- Semanas: `w = null` significa "1er semestre"; `w = 31…45` son las semanas del 2º semestre (la 39 no se usa); `w = 99` es el grupo "Noviembre", para lo que cae después del 30 de octubre.
- Funciones relacionadas: `wLabel`, `isPastWeek`, `isNowWeek` y la constante `WEEKS` (que excluye el 99). Los colores por asignatura están en un objeto con `{c, soft}`.

## Funciones que ya existen
- **Botón "⭐ Actualizar agenda" (bookmarklet).** Se arrastra a favoritos desde la sección "🔄 Sincronizar avance". Se usa en BSchool, en la página *Mis Clases* (`https://cl.bschool.college/lecciones-por-curso/`):
  - Identifica al niño por el saludo "<nombre>, has completado".
  - Cada asignatura es un `.accordion-item`; las lecciones completadas tienen la clase `btn-outline-success` (verde).
  - Empareja nombres con normalización y similitud difusa, porque BSchool tiene errores de tipeo.
  - **Solo agrega** actividades completadas. Nunca desmarca lo que se marcó a mano. Evita duplicados.
  - La agenda escucha `hashchange`, así que reacciona aunque ya esté abierta.
- **Importación por CSV** como respaldo. Acepta `;` o `,` (Excel con configuración chilena exporta con `;`).
- **Vista "Por semana".**
- **Vista "Por asignatura"**, rediseñada: tarjetas por asignatura con porcentaje y barra; mapa de semanas con cuadritos de colores (completa, a medias, atrasada, futura); detalle agrupado por semana; encabezado fijo con botón "‹ Asignaturas"; interruptor "Solo pendientes"; etiqueta "· atrasada" en rojo.

## Último estado conocido (verificar primero)
1. El último cambio **no se pudo publicar**: GitHub devolvió error 500 al guardar. Incluía:
   - Benja: Artes Visuales/Tecnología (10), Educación Física (4), Taller de Ciencias (3), Lectura complementaria (7), Escritura Creativa (36 historias, una por semana) y 13 lecturas del 1er semestre de Comprensión Lectora y Caligrafía.
   - Daniel: libros de Lectura complementaria corridos un mes (ids 374→sem 36, 375→sem 40, 376→sem 45, 377→Noviembre/99).
   - El grupo "Noviembre" (`w = 99`).
   Revisa con `git log` y buscando `Escritura Creativa` en `index.html` si Isaac lo subió después. Si no está, hay que volver a aplicarlo.
2. Falta confirmar que el bookmarklet funciona con la cuenta de Benja.
3. La agenda estaba pensada hasta el 30 de octubre de 2026. Pregúntale a Isaac si quiere extenderla al resto del año.

## Cómo probar
- Para probar sin tocar Firebase real, se puede reemplazar el import de Firebase por funciones de prueba y abrir el archivo localmente.
- Después de publicar, GitHub Pages tarda 1 a 2 minutos. Abre la agenda con Ctrl + F5 para ver la versión nueva.
