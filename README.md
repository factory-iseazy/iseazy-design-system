# Design system iseazy

Sistema de diseño compartido por las tools internas de Marketing iseazy. Define la paleta, tipografía, componentes y patrones que hacen que cualquier herramienta del equipo se reconozca de un vistazo como isEazy.

- **[DESIGN.md](DESIGN.md)** — fuente única de verdad (tokens, componentes, patrones, reglas).
- **[playground/index.html](playground/index.html)** — preview visual interactivo. Abre el archivo en tu navegador.
- **[assets/](assets/)** — logos isEazy (marca + 6 productos, versión color y blanca).

---

## Para conectar tu tool al design system

Hay dos escenarios distintos. Elige el tuyo:

- **A.** Estás arrancando una **tool nueva** desde cero.
- **B.** Tienes una **tool ya hecha** con pantallas existentes y quieres adaptarlas al branding isEazy.

En ambos casos, el primer paso es el mismo: **instalar el sistema** (crea reglas en tu proyecto, no toca tu código). Después, si es el caso B, hay un segundo mensaje opcional que dispara la auditoría y migración.

### A · Tool nueva (instalación limpia)

Abre tu Claude Code en el proyecto de tu tool y **copia este mensaje**:

```
Instala el design system isEazy en este proyecto. Para hacerlo, hace WebFetch
a https://raw.githubusercontent.com/factory-iseazy/iseazy-design-system/main/README.md
y sigue exactamente las instrucciones que verás en la sección "Si eres Claude Code".
```

Tu Claude Code:

1. Leerá las instrucciones de este README
2. Creará un archivo `CLAUDE.md` en tu proyecto con las reglas del sistema
3. Te confirmará *"Listo, sistema instalado"*

A partir de ese momento, cuando le pidas pantallas, componentes, layouts o cualquier output visual, tu Claude consultará automáticamente el `DESIGN.md` y aplicará las reglas. Tú trabajas como siempre — solo notarás que las pantallas salen alineadas con el branding.

### B · Tool existente (instalación + migración)

Primero **instalas el sistema** con el mismo mensaje del caso A (solo crea el `CLAUDE.md` con las reglas, no toca tu código existente — es seguro).

Cuando te confirme la instalación, **copia este segundo mensaje** al mismo Claude:

```
Adapta las pantallas existentes de esta tool al design system isEazy.

1. Lee el CLAUDE.md del proyecto y sigue sus reglas.
2. Antes de tocar nada, audita el código visual existente (HTML/JSX/CSS):
   - Colores hardcodeados que no estén en §2 del DESIGN.md
   - Componentes hechos a mano que duplican lo que ya hay en §5.6 a §5.24
     (buttons, forms, modales, badges, alerts, etc.)
   - Pantallas que podrían usar un patrón de bloque del §5.4
   - Falta de tool-shell (§5.25) envolviendo la app
   - Tipografía no alineada con la escala nombrada (§3.2)
   - Iconos de otros sets (heroicons, lucide…) en lugar de Tabler (§5.2)
3. Devuélveme un plan en formato lista:
   - Por cada pantalla / componente, qué cambia y qué riesgo tiene
   - Ordenado de menor a mayor riesgo
   - Sin tocar todavía nada
4. Espera mi aprobación. Luego aplica los cambios pantalla por pantalla,
   no todo de golpe.
```

Tu Claude:

1. **Auditará primero**, sin tocar — te dará un plan revisable
2. Esperará tu aprobación
3. **Migrará pantalla por pantalla** con tu validación entre cada una (puedes pararle, ajustar, seguir)

#### Variantes según lo que quieras migrar

- **Una sola pantalla**: cambia *"las pantallas existentes"* por *"el archivo `src/pages/Dashboard.tsx`"* (o el path concreto). Skip la auditoría global.
- **Solo colores**: añade *"Mantén la estructura tal cual, solo sustituye los colores hardcodeados por los tokens del §2"*.
- **Refactor agresivo**: añade *"si encuentras algo que claramente debería usar un patrón del §5.4 en lugar del código actual, propón el refactor"*.

### Qué obtienes en ambos casos

- Paleta de color isEazy (magenta, púrpura, accents de producto, decorative, grises, semantic states)
- Tipografía Work Sans con escala nombrada
- 14 patrones de bloque listos (cards, banners, stats, tables, etc.)
- Componentes base: buttons, forms, badges, modales, dropdowns, etc. (~30 piezas)
- Tool shell obligatorio (header con logo isEazy + toggle dark/light)
- Logos servidos desde este repo vía URL raw — cero copia local

### Actualizaciones

Cuando este repo se actualiza (nueva paleta, nuevo patrón, fix), tu Claude lo cogerá en la siguiente sesión automáticamente — el sistema lee siempre la última versión publicada en `main`. No tienes que hacer nada.

---

## Si eres Claude Code leyendo esto

**Te están pidiendo instalar el design system isEazy en un proyecto.** Sigue estos pasos literalmente, en orden:

### Paso 1 — Verifica el contexto

Confirma que estás en el proyecto del usuario (no en el repo `iseazy-design-system`). Si estás en este repo, dile al usuario "ya estás en el repo del design system, ¿quieres que te ayude con el sistema o instalarlo en otro proyecto?".

### Paso 2 — Crea `CLAUDE.md` en el proyecto del usuario

En la **raíz del proyecto activo**, crea (o actualiza si ya existe) el archivo `CLAUDE.md` añadiendo este bloque al final, separado del contenido existente:

```markdown
## Design system iseazy

Este proyecto consume el design system isEazy. La fuente única de verdad
para todas las decisiones visuales vive en:

  https://raw.githubusercontent.com/factory-iseazy/iseazy-design-system/main/DESIGN.md

### Antes de cualquier output visual

Antes de elegir colores, tipografía, layout, componentes o cualquier cosa
que afecte cómo se ve la tool, haz `WebFetch` al DESIGN.md con un `prompt`
que extraiga SOLO la sección que necesitas. No descargues las 2500 líneas
enteras cada vez. Mapeo:

| Necesitas | WebFetch con prompt |
|---|---|
| Color (tokens, brand, accents, deco, grey) | "Extrae §2 (Tokens de color)" |
| Modo oscuro | "Extrae §2.8 (Modo oscuro)" |
| Tipografía (escala, stacks) | "Extrae §3 (Tipografía)" |
| Layout (spacing, radius, shadows, motion, focus) | "Extrae §4 (Layout y sistema visual)" |
| Icono → consultar Tabler | "Extrae §5.2 (Iconografía)" |
| Charts | "Extrae §5.3 (Gráficos)" |
| Patrón de pantalla (hero, cards, stats, etc.) | "Extrae §5.4 (Patrones de bloque)" |
| Anti-patrones | "Extrae §5.5" |
| Botones | "Extrae §5.6" |
| Forms (inputs, validación) | "Extrae §5.7" |
| Badges, tags, avatars | "Extrae §5.8" |
| Tooltips | "Extrae §5.9" |
| Alerts y toasts | "Extrae §5.10" |
| Modales/dialogs | "Extrae §5.11" |
| Empty states | "Extrae §5.12" |
| Loading (spinner, skeleton, progress) | "Extrae §5.13" |
| Tablas y pagination | "Extrae §5.14" |
| Dropdowns, popovers, drawer, tabs internas, stepper, date, combobox, breadcrumbs, slider, file upload | "Extrae §5.15 a §5.24" |
| Tool shell (wrapper de la pantalla) | "Extrae §5.25" |
| CSS classes copy-paste (.t-*, .btn-*, .field-*, etc.) | "Extrae §12 (Utilidades CSS extra)" |

### Reglas duras (no negociables)

1. **No inventar tokens de color** fuera de §2. Solo las variables CSS
   definidas allí.
2. **No renombrar patrones** por su caso de uso (nada de `post-card`,
   `client-card`, `experiment-card` — usa los nombres genéricos de §5.4).
3. **Anti-patrones a evitar**: §5.5. Sidebar permanente con avatares
   circulares + emojis, headings con gradiente de texto, sombras
   `shadow-2xl`, etc.
4. **Toda pantalla debe estar envuelta en el `tool-shell`** de §5.25:
   header con logo isEazy + nombre de la tool + toggle dark/light.
   No renderizar contenido fuera del shell.
5. Si el sistema necesita un cambio que no encaja con lo que hay,
   **NO modifiques tokens o componentes localmente**. Dile al usuario:
   "Esto requiere una decisión sobre el design system. Abre un issue en
   https://github.com/factory-iseazy/iseazy-design-system/issues
   describiendo el caso de uso." Sigue trabajando con lo más cercano
   del sistema actual hasta que se resuelva.

### Logos isEazy (vía URL raw, sin copia local)

Base: `https://raw.githubusercontent.com/factory-iseazy/iseazy-design-system/main/assets`

- Marca: `{base}/logos/iseazy/iseazy.svg` + `iseazy-white.svg` (dark mode)
- Productos: `{base}/logos/iseazy/iseazy-{producto}.svg` + `-white.svg`
  donde `{producto}` ∈ {`author`, `factory`, `skills`, `lms`, `engage`, `game`}

Patrón dual obligatorio para soportar light + dark mode automáticamente:

    <img src="{base}/logos/iseazy/iseazy.svg"
         class="block dark:hidden h-7 w-auto" />
    <img src="{base}/logos/iseazy/iseazy-white.svg"
         class="hidden dark:block h-7 w-auto" />

### Stack recomendado

- **React + Tailwind + shadcn/ui + Tabler Icons** para tools React.
- **Streamlit** con el `.streamlit/config.toml` del §2.7 para tools data.
- **HTML estático con Tailwind CDN** para landings o playgrounds.

En todos los casos, copia las variables CSS de §2.2 al `:root` de tu
`globals.css`, configura Tailwind con `darkMode: 'class'`, y usa las
clases CSS copy-paste de §12 si las necesitas (buttons, forms, etc).

### Versión

Conectado al design system en `main` el día de la instalación. El sistema
lee siempre la última versión publicada — si hay actualizaciones cuando
trabajes mañana, se aplican automáticamente.
```

Si el archivo `CLAUDE.md` ya existía con otro contenido (instrucciones del proyecto, etc.), **no lo borres**: añade el bloque del design system al final, separado por una línea en blanco.

### Paso 3 — Confirma al usuario

Cuando hayas creado/actualizado el archivo, dile al usuario exactamente:

> Listo. Design system iseazy instalado en este proyecto. Cuando me pidas pantallas, componentes o cualquier output visual, consultaré automáticamente las reglas del sistema. Si quieres ver visualmente cómo es el sistema, abre el playground en https://github.com/factory-iseazy/iseazy-design-system/blob/main/playground/index.html

Y termina. No hagas nada más en este turno salvo lo anterior — no propongas cambios al proyecto, no toques otros archivos, no instales dependencias. El usuario hará la siguiente petición.

### Si el usuario te pide algo visual en sesiones futuras

Tu `CLAUDE.md` local ya tiene todas las reglas. Léelas, identifica qué sección del `DESIGN.md` necesitas, haz `WebFetch` extractivo con `prompt` apuntando a esa sección, y aplica las reglas a lo que el usuario te pida.

Si en algún momento el `WebFetch` falla (sin red, repo caído), avísale al usuario y trabaja con lo que recuerdes del sistema, pero marcando explícitamente "trabajando sin acceso al design system, valida cuando esté disponible".

---

## Versionado y snapshots

`main` es la versión vigente. Las tools que necesiten reproducibilidad estricta (releases formales) pueden fijar la URL a un commit SHA:

```
https://raw.githubusercontent.com/factory-iseazy/iseazy-design-system/<sha>/DESIGN.md
```

## Qué entra en este repo

- Paleta de marca, tipografía, escala, voz, layout, motion, focus
- Componentes UI canónicos y patrones de bloque
- Logos isEazy (marca + productos)
- Nomenclatura de países, product lines, stages, formato numérico
- Tool shell obligatorio

## Qué NO entra

- URLs internas, métricas de negocio concretas, IDs de Salesforce
- Logos de clientes concretos (los aporta cada tool si los necesita)
- Schemas de BBDD (viven en el repo core `bbdd-salesforce-sync`)
- Secretos, credenciales, OAuth

## Contribuir

Para proponer un cambio en tokens, patrones o componentes: abre un issue en este repo con el caso de uso real que motiva el cambio. Las tools consumidoras no modifican el sistema localmente — siempre PR aquí.
