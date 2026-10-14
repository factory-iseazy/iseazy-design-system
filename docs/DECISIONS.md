# Registro de decisiones del design system

Decisiones que no se deducen del propio `DESIGN.md`: por qué se eligió una opción
y no otra. Se anota en el mismo commit en que se toma. Formato: fecha, decisión,
contexto, alternativas descartadas, dónde vive.

---

## 2026-10-14 — Modo app operativa (§14) como sección propia, no como notas sueltas

**Decisión**: crear §14 con la misma estructura que §13 (test, overrides,
interacción, checklist) en lugar de repartir los valores densos por §4.7, §5.7 y
§5.14.

**Contexto**: §13.1 enviaba las apps de "crear, editar, tramitar" al modo web, cuyos
defaults vienen de la web pública; la primera tool de registro de horas salió "con
demasiado aire" y hubo que corregirla a mano. Issue #1.

**Descartado**: notas de densidad en cada sección (no hay punto de entrada; el test
de §13.1 seguiría mandando al sitio equivocado).

**Vive en**: DESIGN.md §14, README (plantilla de CLAUDE.md, prompt de auditoría).

## 2026-10-14 — `.table-compact` se reutiliza; solo se añade `.field-input-sm`

**Decisión**: el modo operativo usa `.table-compact` (ya existía) y añade un único
tamaño de campo nuevo, `.field-input-sm` de 36 px, igual a `btn-sm`. El botón de
icono sigue siendo `size-9` (36 px); **no** se añade un tamaño de 32 px.

**Contexto**: la propuesta original traía `.table.densa` (idéntico a
`.table-compact`) y un botón de icono de 32 px fuera de la escala de §5.6.4.

**Descartado**: tamaño de icono de 32 px (rompe la escala y baja del objetivo táctil
mínimo sin ganar nada en escritorio).

## 2026-10-14 — Nombres de clase en inglés y genéricos

**Decisión**: toda clase nueva se nombra en inglés y por lo que es, no por el caso
de uso: `table-editable`, `cell-modified`, `row-removed`, `save-bar`,
`choice-pill`, `icon-tile`, `badge-category`.

**Contexto**: las propuestas llegaron con `.rejilla`, `.finde`, `.quitada`,
`.barra-guardado`, `.opcion-tipo`. El sistema nombra en inglés y §5.5.7 prohíbe
nombrar por caso de uso.

## 2026-10-14 — Celda modificada con fondo, no con borde

**Decisión**: en `table-editable`, "modificada" se marca con fondo
`brand-magenta-soft`; el borde magenta queda reservado al foco.

**Contexto**: la propuesta usaba borde magenta para modificada, que es el mismo
cue que el foco (§5.7.2). Además §5.7.5 ya fijaba "sin borde en idle" para campos
en tabla.

## 2026-10-14 — Columna no laborable en `gray-100`, no `gray-50`

**Decisión**: `col-off` usa `gray-100`.

**Contexto**: `gray-50` es el color del hover de fila y del `thead` (§5.14.2); una
columna en `gray-50` parecería una fila en hover.

## 2026-10-14 — "Cambios sin guardar" es `warning`, no `destructive`

**Decisión**: la `save-bar` con cambios usa `badge-warning` y borde `warning/50`.

**Contexto**: la propuesta mezclaba borde warning con badge destructive. Según
§5.8.1 `warning` es "stale" y `destructive` es "error"; un cambio pendiente no es
un error. El argumento "en rojo se ve más" se resuelve con la posición (barra
sticky al pie) y no con el color de error.

## 2026-10-14 — Feedback inline de éxito: 6 s (frente a 4 s del toast)

**Decisión**: el resultado de guardar se muestra en la propia `save-bar` 6 s si es
éxito y hasta resolverse si es error. El toast de §5.10.2 sigue en 4 s.

**Contexto**: probado en la tool de registro de horas: el éxito como alerta arriba
no se veía. El feedback inline no tiene animación de entrada que llame la atención,
así que necesita más tiempo en pantalla.

## 2026-10-14 — Diálogo de tres opciones apilado en columna

**Decisión**: "Guardar y continuar" (primary) / "Quedarme aquí" (ghost) /
"Descartar y continuar" (ghost con texto destructive), en columna.

**Descartado**: tres botones en fila (no caben en `dialog-sm` y el ojo no distingue
el primario); `btn-destructive` sólido para descartar (competiría con el primario).

## 2026-10-14 — `icon-tile` (icono sobre su tinte) acotado a métricas y listados

**Decisión**: se admite el icono con fondo tintado al 12 % como `icon-tile`
(§5.8.4) para secciones de métricas operativas y listados densos. `category-card`
mantiene "solo stroke, sin fondo" (§5.4.3).

**Contexto**: las secciones de métricas salían en gris y magenta ("sosas"). La regla
de §5.4.3 se escribió para iconos grandes (32-40 px) de card de marketing, donde el
tinte sobra; a 20 px el tinte es lo que da presencia. Issue #3.

## 2026-10-14 — §2.4 regla 1 reescrita: prohibido para estado, permitido para categoría

**Decisión**: un decorativo no codifica estado (OK/error), pero sí identidad de
categoría con mapeo fijo.

**Contexto**: la redacción anterior ("no asignar significado fijo") contradecía
§2.3 regla 2 (mapeo fijo por categoría). La intención siempre fue la de estado.

## 2026-10-14 — Variable `--c` como mecanismo único de color de categoría

**Decisión**: las clases `.c-*` (una por token de §2.3 y §2.4) fijan `--c`; los
componentes de categoría (`badge-category`, `row-category`,
`choice-pill-category`, `icon-tile`) consumen `hsl(var(--c))` y sus tintes.

**Descartado**: una variante de clase por color y componente (N×M clases) y
colores por índice/hash (prohibido por §2.3 regla 4).

## 2026-10-14 — `choice-pills` es una variante de `filter-toolbar`, no un componente

**Decisión**: selección única con radios nativos = la pill de §5.4.14 envolviendo
un `<input type="radio">`. Activa en magenta sólido (como la pill de filtro) salvo
que lleve color de categoría, en cuyo caso se tinta con `--c`.

**Contexto**: la propuesta la basaba en `tag` (§5.8.2, que es cerrable). Issue #5.

## 2026-10-14 — Escala de sombras alineada con lo implementado

**Decisión**: §4.4 pasa a 4 niveles: ninguna / `shadow-sm` / `shadow-md`
(dropdowns, popovers, tooltips, calendario, barras sticky) / `shadow-lg`
(modales, drawers, toasts). El modal de §5.11 pasa de `shadow-md` a `shadow-lg`.

**Contexto**: §4.4 decía `shadow` para dropdowns, pero §5.15, §5.16 y §5.9 ya
usaban `shadow-md`, y el playground también. Se alinea el doc con la
implementación, no al revés. Issue #2.

## 2026-10-14 — §12.6: CSS de componentes transcrito del playground

**Decisión**: §12 incluye el CSS de badge, tag, alert, toast, dialog, table,
dropdown, popover y tabs, copiado del `<style>` del playground, más las piezas
nuevas. El playground sigue siendo la referencia viva; si cambia uno, cambia el
otro en el mismo commit.

**Contexto**: §5.11 remitía a §12 para HTML estático y §12 no lo tenía. Tools sin
React (Jinja2) lo reinventaban. Issue #2.

## 2026-10-14 — Doble eje permitido con siete condiciones (criterio Datawrapper)

**Decisión**: §13.3.6 pasa de prohibición a permiso condicionado: medidas
relacionadas por construcción; barras + línea (nunca dos líneas); ambos ejes
desde cero; unidad y marca de color por eje; leyenda con el eje de cada serie;
proporción entre ejes fija en código (no cambia al filtrar); descripción de una
línea. §13.4 añade doble eje, paneles con eje X compartido y ratio. Issue #6.

**Contexto**: el problema del doble eje (alineación arbitraria de escalas) es real
y no tiene arreglo técnico, pero no es una cuestión cerrada: Datawrapper se negó
años y acabó permitiéndolo con ejes desde cero por demanda de clientes
financieros; las terminales financieras lo usan a diario. Nuestros dashboards los
lee gente que viene de Excel. Prohibirlo sin más no evita el problema (se hará en
Excel) y genera fricción.

**Descartado**: mantener la prohibición y ofrecer solo paneles con eje X
compartido (sigue documentado como alternativa cuando no se cumplen las
condiciones).

**Fuera del repo**: el kit compartido de dashboards (cuando se extraiga de la tool
de ingreso predictivo) debe implementar `DualAxis` con las condiciones impuestas
por diseño: sin opción de quitar el cero ni de poner dos líneas.
