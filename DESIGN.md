# DESIGN.md — Sistema de diseño Marketing iseazy

Documento canónico del **sistema de diseño** que consumen todas las tools de Marketing isEazy. Este repo (`iseazy-design-system`) NO es una aplicación: es la fuente de verdad de tokens, patrones y assets. Las tools y dashboards son **consumidores externos** que viven en otros repos.

Inspirado en la web pública [iseazy.com](https://www.iseazy.com/es/) — los tokens de color y la tipografía se han extraído literalmente del CSS oficial servido por esa web (verificación en §9).

Cuando cambie la web de iseazy y queramos arrastrar las tools al nuevo branding, **este es el único fichero a actualizar**. Las tools consumidoras vuelven a sincronizar desde aquí.

---

## Índice

- **§1 Identidad de marca** — [§1.1 Alcance del repo](#11-alcance-del-repo) · [§1.2 Voz](#12-voz)
- **§2 Tokens de color** — [§2.1 Brand](#21-brand-con-significado) · [§2.2 Variables CSS](#22-variables-css-modo-claro) · [§2.3 Category accents](#23-category-accents-productos) · [§2.4 Decorative palette](#24-decorative-palette-resto-de-colores) · [§2.5 Grey scale](#25-grey-scale) · [§2.6 Text colors](#26-text-colors-alias-semánticos) · [§2.7 Streamlit](#27-streamlit-tools-de-compañeros) · [§2.8 Modo oscuro](#28-modo-oscuro)
- **§3 Tipografía** — [§3.1 Carga](#31-carga) · [§3.2 Escala nombrada](#32-escala-tipográfica-nombrada) · [§3.3 Combinaciones canónicas](#33-combinaciones-canónicas) · [§3.4 Cheatsheet](#34-aplicación-rápida-cheatsheet)
- **§4 Layout & sistema visual** — [§4.1 Container & breakpoints](#41-container--breakpoints) · [§4.2 Spacing scale](#42-spacing-scale) · [§4.3 Border-radius scale](#43-border-radius-scale) · [§4.4 Shadows / elevation](#44-shadows--elevation) · [§4.5 Z-index scale](#45-z-index-scale) · [§4.6 Motion](#46-motion) · [§4.7 Focus state global](#47-focus-state-global)
- **§5 Componentes base** — [§5.1 Convenciones](#51-convenciones-de-componente) · [§5.2 Iconografía](#52-iconografía) · [§5.3 Gráficos](#53-gráficos-recharts) · [§5.4 Patrones de bloque (14)](#54-patrones-de-bloque) · [§5.5 Anti-patrones](#55-anti-patrones-qué-no-copiar-de-las-landings-de-ai-slop) · [§5.6 Buttons](#56-buttons) · [§5.7 Forms](#57-forms-inputs-labels-validación) · [§5.8 Badges, tags & avatars](#58-badges-tags--avatars) · [§5.9 Tooltips](#59-tooltips) · [§5.10 Alerts & Toasts](#510-alerts--toasts) · [§5.11 Modales / Dialogs](#511-modales--dialogs) · [§5.12 Empty states](#512-empty-states) · [§5.13 Loading states](#513-loading-states) · [§5.14 Tables & Pagination](#514-tables--pagination) · [§5.15 Dropdown menus](#515-dropdown-menus) · [§5.16 Popovers](#516-popovers) · [§5.17 Drawer / Sheet](#517-drawer--sheet) · [§5.18 Tabs internas](#518-tabs-internas) · [§5.19 Steppers](#519-steppers--wizards) · [§5.20 Date picker](#520-date-picker) · [§5.21 Combobox](#521-combobox--autocomplete) · [§5.22 Breadcrumbs](#522-breadcrumbs) · [§5.23 Slider](#523-slider) · [§5.24 File upload](#524-file-upload) · [§5.25 Tool shell](#525-tool-shell)
- **§6 Datos canónicos** · **§7 Accesibilidad** · **§8 Cómo consumir** · **§9 Verificación** · **§10 Playground** · **§11 Assets** · **§12 Utilidades CSS extra**

---

## 1. Identidad de marca

### 1.1 Alcance del repo

- **Qué es**: `iseazy-design-system` — design system canónico. Contiene `DESIGN.md` (este documento), `assets/` (logos, screenshots, patrones decorativos) y `playground/` (HTML estático de inspección).
- **Qué NO es**: una aplicación, una librería de componentes React, un paquete npm. Aquí no se renderiza producto.
- **Consumidores conocidos**:
  - *Marketing Dashboard iseazy* — dashboard interno con réplica local de Salesforce. Repo separado.
  - *iseazy-tool-starter* — template Streamlit/React para tools de compañeros (`tool-cohorts`, `tool-alerts`, etc.). Repo separado.
  - Cada nueva tool de marketing creada por el equipo.
- **Eslogan de la familia isEazy** (web pública): *"E-LEARNING MADE EASY"*. Disponible para usar en footers/landings de tools si encaja; nunca como reclamo principal de la tool, que tiene su propio propósito.

### 1.2 Voz

Reglas tomadas del copy real de iseazy.com (`/es/`). Aplican a cualquier UI que consuma este sistema.

| Atributo | Cómo se aplica |
|---|---|
| **Idioma por defecto** | Español de España. La web tiene `/es/`, `/en/`, `/br/`. Las tools internas en `es-ES` (números, fechas, mes). Mismo enfoque aquí: español primero, inglés solo si un término técnico no tiene traducción canónica (`MQL`, `pipeline`, `won`). |
| **Tono** | Directo, profesional, sin marketing-speak. Una idea por frase. Ej. real: *"Lanza una sincronización con Salesforce manual o consulta el histórico. Próximamente se ejecutará automáticamente cada noche."* |
| **Promesa simplicidad** | El claim que cruza toda la web isEazy es "made easy" / "todo en uno". En tools internas se traduce a: una acción visible por pantalla, cero opciones decorativas. |
| **Estados sin jerga** | Sync: `pending → "En cola"`, `running → "Sincronizando"`, `succeeded → "Completado"`, `failed → "Error"`, `aborted_stale → "Abortado"`. Trasladar el mismo registro a otros estados (jobs, ingestas, ejecuciones). |
| **Mensajes vacíos** | Frases cortas, en cursiva muted. Ej.: *"Aún no se ha ejecutado ninguna."* |
| **Tiempos relativos** | `nunca`, `hace <1 min`, `hace 5 min`, `hace 2h`, `hace 3d`. Canónico. |

No usar emojis en UI ni en copy de error. Sí en mensajes de chat de Claude Code dentro de las tools si el compañero lo prefiere, pero **no en componentes renderizados**.

---

## 2. Tokens de color

**Origen**: extraídos del CSS oficial del tema de iseazy.com (`https://www.iseazy.com/wp-content/themes/iseazy/style.css`, ~545 KB) + páginas de producto (`/es/author/`, `/es/skills/`, `/es/lms/`, `/es/engage/`, `/es/factory/`, `/es/game/`). Verificación reproducible en §9.

La paleta está dividida en cinco capas, en orden de prioridad de uso:

1. **§2.1 Brand** — los colores que SÍ son marca y tienen significado.
2. **§2.2 Variables CSS (shadcn-contract)** — los tokens semánticos que consumen los componentes.
3. **§2.3 Category accents (productos)** — cada producto isEazy con su color fijo.
4. **§2.4 Decorative palette** — el "resto de colores" usados como adorno en stats, ilustraciones, charts, fondos de gradiente.
5. **§2.5 Grey scale** — la escala neutra para texto, bordes y fondos.

### 2.1 Brand (con significado)

Estos son los colores que **tienen significado**: identifican la marca isEazy o un estado. Si los usas, el lector entiende qué representan. Cualquier desviación rompe la lectura.

Los **roles** están explícitos: `PRIMARY` para magenta, `SECONDARY` para purple. Cuando dudes "¿qué color uso para la acción principal?" → primary. "¿Y para el secundario / hero / fondo enfatizado?" → secondary. No al revés.

#### `PRIMARY` — magenta

Es **el color de isEazy**. Acciones principales (CTAs, botones, links activos), foco visible, brand marks. 179 ocurrencias en el CSS oficial — el color más repetido.

| Token | Hex | HSL | Uso |
|---|---|---|---|
| `--brand-magenta` | `#EB1C79` | `329 84% 51%` | **Default**. Botones, CTAs, links. Equivalente a `--primary`. |
| `--brand-magenta-dark` | `#BC1661` | `333 79% 41%` | Hover/press del primary. |
| `--brand-magenta-deep` | `#670933` | `334 84% 22%` | Fondos magenta profundo (cards premium, banners oscuros con identidad). |
| `--brand-magenta-soft` | `#FDECF4` | `330 75% 96%` | Background sutil de marca (highlight de fila, bg de columna "Más popular" en `comparison-table`). |

#### `SECONDARY` — purple

Acentos no-primary, fondos hero, headers densos, secondary buttons. La regla práctica: si una pantalla necesita un segundo color de marca distinto a magenta sin perder identidad, es purple.

| Token | Hex | HSL | Uso |
|---|---|---|---|
| `--brand-purple` | `#3B189B` | `260 73% 35%` | **Default**. Fondos hero, secondary actions. Equivalente a `--secondary`. |
| `--brand-purple-mid` | `#89239B` | `292 64% 38%` | Acento intermedio púrpura/magenta. Stats, sub-fondos. |
| `--brand-purple-deep` | `#220E58` | `253 72% 20%` | Navy oscuro premium (card "Business" en pricing, banners de contraste alto). |

#### `NEUTRAL` — ink

Texto. Toda la jerarquía neutra del cuerpo y los títulos. Para escala más fina ver §2.5 y §2.6.

| Token | Hex | HSL | Uso |
|---|---|---|---|
| `--brand-ink` | `#333333` | `0 0% 20%` | Texto cuerpo (`text-body`). |
| `--brand-ink-deep` | `#212529` | `210 11% 15%` | Heading enfatizado sobre fondo claro (`text-heading`). |

#### `SEMANTIC` — estados

Colores con significado de estado. No usar para decoración aunque queden bien.

| Token | Hex | HSL | Uso |
|---|---|---|---|
| `--brand-info` | `#03A0F4` | `196 96% 49%` | Azul informativo. Status "running", KPI azul, link secundario. Equivalente a `--accent`. |
| `--brand-success` | `#73C13C` | `93 53% 50%` | Verde lima isEazy. Status "succeeded", confirmaciones, KPI positivo. Sustituye al `#198754` bootstrap genérico. |
| `--brand-destructive` | `#DC3545` | `354 70% 54%` | Error / status "failed". Bootstrap classic, usado tal cual en iseazy. |

> **Reservado para marca**: `PRIMARY`, `SECONDARY` y sus variantes. No usarlos como adorno en gráficos o decoración general — eso es §2.4. Si pintas un blob decorativo en magenta, ese magenta deja de "querer decir marca".

### 2.2 Variables CSS (modo claro)

Contrato shadcn/ui: las mismas variables que cualquier app shadcn, con los valores de iseazy. Las tools consumidoras (Marketing Dashboard, tools de compañeros en React) deben copiar este bloque a su `globals.css`.

```css
:root {
  --background: 0 0% 100%;            /* #FFFFFF */
  --foreground: 0 0% 20%;             /* #333333 — brand-ink */
  --card: 0 0% 100%;
  --card-foreground: 0 0% 20%;

  --primary: 329 84% 51%;             /* #EB1C79 — brand-magenta */
  --primary-foreground: 0 0% 100%;

  --secondary: 260 73% 35%;           /* #3B189B — brand-purple */
  --secondary-foreground: 0 0% 100%;

  --muted: 0 0% 96%;                  /* #F4F4F4 — gray-200 */
  --muted-foreground: 0 0% 32%;       /* #525252 — gray-700, secondary text */

  --accent: 196 96% 49%;              /* #03A0F4 — brand-info */
  --accent-foreground: 0 0% 100%;

  --destructive: 354 70% 54%;         /* #DC3545 — brand-destructive */
  --destructive-foreground: 0 0% 100%;

  --success: 93 53% 50%;              /* #73C13C — brand-success */
  --warning: 45 86% 49%;              /* #DCB01B — deco-gold (warning natural en la paleta isEazy) */

  --border: 0 0% 92%;                 /* #EBEBEB — gray-300 */
  --input: 0 0% 92%;
  --ring: 329 84% 51%;                /* matches --primary */

  --radius: 0.5rem;
}
```

Notas:

- `--ring` se enlaza al primary magenta para que el foco sea visible y consistente con la marca, no con el slate genérico de shadcn.
- `--success` ahora es el verde lima real de iseazy (`#73C13C`), no el bootstrap-green (`#198754`). Si una tool importa este sistema, los status "succeeded" pasan a verde lima — más vivo, más isEazy.
- **Modo oscuro**: pendiente. Mientras no exista una guía pública oscura de iseazy.com, dejar `.dark` con los valores que ya hay y no exponerlo en UI. Cuando se aborde, mantener `#EB1C79` como primary y bajar `#3B189B` hacia `~280 70% 65%`.

### 2.3 Category accents (productos)

Cada línea de producto isEazy tiene su **color fijo**. Esto evita el efecto "todo magenta" que aplana la UI y se alinea con cómo iseazy.com diferencia productos. **Reservado por producto**: nadie pinta una card random con `--accent-skills` porque "queda bonito amarillo".

| Producto | Token | Hex | HSL | Verificación |
|---|---|---|---|---|
| `author` | `--accent-author` | `#EB1C79` | `329 84% 51%` | Confirmado — coincide con primary. |
| `factory` | `--accent-factory` | `#3B189B` | `260 73% 35%` | Confirmado — coincide con secondary. |
| `skills` | `--accent-skills` | `#DCB01B` | `45 86% 49%` | Confirmado en CSS de `/es/skills/` y `/es/game/`. (Era aproximado `#F0B033`, ahora con valor real.) |
| `lms` | `--accent-lms` | `#03A0F4` | `196 96% 49%` | Confirmado — coincide con brand-info. |
| `engage` | `--accent-engage` | `#6538E0` | `255 73% 55%` | Confirmado en CSS público. (Era aproximado `#7C3AED`, ahora con valor real.) |
| `game` | `--accent-game` | `#73C13C` | `93 53% 50%` | Verde lima — confirmado en `/es/game/` como color de identificación. (Antes se aproximó como teal `#00ACAD` por error; corregido.) |

```css
:root {
  --accent-author: 329 84% 51%;   /* #EB1C79 */
  --accent-factory: 260 73% 35%;  /* #3B189B */
  --accent-skills: 45 86% 49%;    /* #DCB01B */
  --accent-lms: 196 96% 49%;      /* #03A0F4 */
  --accent-engage: 255 73% 55%;   /* #6538E0 */
  --accent-game: 93 53% 50%;      /* #73C13C */
}
```

> Game comparte hex con `--brand-success`. Es intencional en la web de iseazy: el verde lima identifica gamificación y al mismo tiempo es el verde "positivo" en stats. Conviven sin conflicto porque rara vez aparecen en el mismo bloque.

Reglas de uso:

1. **Producto isEazy concreto**: su color asignado, sin excepciones.
2. **Categoría no-producto** (industria, sector): elegir un color de §2.4 de forma **estable** — la misma industria mismo color en toda la app. Mapeo fijo en código.
3. **Acción primaria genérica**: magenta (`--primary`).
4. Nunca generar color por hash/índice del array — produce inconsistencia entre vistas.

### 2.4 Decorative palette (resto de colores)

Estos colores aparecen en iseazy.com con frecuencia baja-media y **no tienen significado fijo**. Son la paleta "del resto": stats decorativos, ilustraciones, gradientes auxiliares, blobs, badges secundarios, charts de muchas series.

| Token | Hex | Familia | Notas |
|---|---|---|---|
| `--deco-blue-vivid` | `#000CFF` | azul | Azul saturado, números grandes. Sale en stats "+50 Clients". |
| `--deco-blue` | `#0693E3` | azul | Azul medio. |
| `--deco-sky` | `#03A0F4` | azul | Sky (= `--brand-info`). |
| `--deco-sky-soft` | `#8ED1FC` | azul | Sky pastel. |
| `--deco-violet` | `#6538E0` | violeta | Vibrante. Coincide con `--accent-engage`. |
| `--deco-violet-soft` | `#9B51E0` | violeta | Más claro, decorativo. |
| `--deco-magenta-mid` | `#89239B` | púrpura | Coincide con `--brand-purple-mid`. |
| `--deco-lime` | `#73C13C` | verde | Verde lima (= `--brand-success`, = `--accent-game`). |
| `--deco-mint` | `#7BDCB5` | verde | Menta pastel. |
| `--deco-gold` | `#DCB01B` | amarillo | Coincide con `--accent-skills`. |
| `--deco-amber` | `#FCB900` | amarillo | Cálido. |
| `--deco-amber-soft` | `#FFEF66` | amarillo | Pastel. |
| `--deco-orange` | `#FF6900` | naranja | |
| `--deco-rose` | `#F78DA7` | rosa | Pastel. |
| `--deco-slate` | `#ABB8C3` | gris | Slate decorativo. |

Reglas de uso:

1. **No asignar significado fijo** a un color decorativo. Si tu pantalla usa `--deco-lime` para "OK" sistemáticamente, eso es `--brand-success`, no decorativo.
2. **Variedad sí, caos no**: en un `stat-block` con 4 cifras, usar 4 colores decorativos distintos en orden estable (el mismo render siempre da el mismo color a la misma cifra). No rotar entre renders.
3. **Charts**: cuando una serie tiene N>6 valores, ampliar con esta paleta antes de inventar hex. Orden recomendado: magenta → purple → sky → lime → gold → violet → orange → rose → slate.
4. **Gradientes decorativos**: combinar colores afines en familia (sky+sky-soft, violet+magenta-mid) — no mezclar familias opuestas (lime+rose) salvo intención explícita.

Variables CSS:

```css
:root {
  --deco-blue-vivid: 236 100% 50%;   /* #000CFF */
  --deco-blue: 204 96% 46%;          /* #0693E3 */
  --deco-sky: 196 96% 49%;           /* #03A0F4 */
  --deco-sky-soft: 203 95% 77%;      /* #8ED1FC */
  --deco-violet: 255 73% 55%;        /* #6538E0 */
  --deco-violet-soft: 274 71% 60%;   /* #9B51E0 */
  --deco-magenta-mid: 292 64% 38%;   /* #89239B */
  --deco-lime: 93 53% 50%;           /* #73C13C */
  --deco-mint: 153 56% 67%;          /* #7BDCB5 */
  --deco-gold: 45 86% 49%;           /* #DCB01B */
  --deco-amber: 41 100% 49%;         /* #FCB900 */
  --deco-amber-soft: 55 100% 70%;    /* #FFEF66 */
  --deco-orange: 25 100% 50%;        /* #FF6900 */
  --deco-rose: 346 87% 76%;          /* #F78DA7 */
  --deco-slate: 209 17% 72%;         /* #ABB8C3 */
}
```

### 2.5 Grey scale

Iseazy.com usa una escala neutra amplia (9+ tonos). Documentamos los 10 que aparecen con frecuencia suficiente para canonizarlos. **No inventar grises intermedios**: si necesitas uno que no está aquí, probablemente puedes usar el más cercano.

| Token | Hex | HSL | Uso típico |
|---|---|---|---|
| `--gray-50` | `#FBFBFB` | `0 0% 98%` | Background extra sutil (alternancia de filas en tablas claras). |
| `--gray-100` | `#F8F8F8` | `0 0% 97%` | Background suave (panel auxiliar). |
| `--gray-200` | `#F4F4F4` | `0 0% 96%` | bg-muted oficial (`--muted`). |
| `--gray-300` | `#EBEBEB` | `0 0% 92%` | Borders/dividers (`--border`). |
| `--gray-400` | `#D1D1D1` | `0 0% 82%` | Borders enfatizados (outline cards "Enterprise"). |
| `--gray-500` | `#ACACAC` | `0 0% 67%` | Text disabled, iconos secundarios. |
| `--gray-600` | `#83919B` | `206 9% 56%` | Text secundario tenue. |
| `--gray-700` | `#525252` | `0 0% 32%` | Text secundario fuerte (subtítulos) (`--muted-foreground`). |
| `--gray-800` | `#333333` | `0 0% 20%` | Body text (`--foreground` / `--brand-ink`). |
| `--gray-900` | `#212529` | `210 11% 15%` | Heading dark sobre fondo claro (`--brand-ink-deep`). |

### 2.6 Text colors (alias semánticos)

shadcn nombra los colores de texto como `foreground` y `muted-foreground` — genérico y poco evidente. Encima añadimos una capa de **alias semánticos** que dejan claro qué color usar para qué tipo de texto. Si dudas, usa el alias, no la variable de shadcn directa.

| Alias | Mapeo interno | Uso |
|---|---|---|
| `--text-heading` | `--brand-ink-deep` (#212529) | Títulos h1, h2, h3 sobre fondo claro. Más oscuro que body para enfatizar jerarquía. |
| `--text-body` | `--foreground` (#333333) | Texto cuerpo por defecto. Equivalente a `--brand-ink`. |
| `--text-subtle` | `--gray-700` (#525252) | Sub-headings, descripciones secundarias, metadata. Equivalente a `--muted-foreground` pero con nombre semántico claro. |
| `--text-soft` | `--gray-600` (#83919B) | Captions, footnotes, texto de apoyo de bajo peso. |
| `--text-disabled` | `--gray-500` (#ACACAC) | Inputs/botones deshabilitados, placeholder. |
| `--text-link` | `--primary` (#EB1C79) | Links, CTAs inline, "ver más →". |
| `--text-link-hover` | `--brand-magenta-dark` (#BC1661) | Hover del link. |
| `--text-inverse` | white | Texto sobre fondo oscuro/gradiente (CTA banner, hero photo). |
| `--text-success` | `--brand-success` (#73C13C) | Confirmación, KPI positivo en texto. |
| `--text-warning` | `--warning` (#DCB01B) | Aviso. |
| `--text-destructive` | `--brand-destructive` (#DC3545) | Error en texto. |

> **Por qué `subtle` y no `muted`**: shadcn ya usa `bg-muted` para el background gris (`--muted`). Llamar al texto `text-muted` colisionaría con `bg-muted` (mismo nombre, valores distintos). `text-subtle` deja claro que es texto "atenuado" sin colisionar con el background.

Variables CSS (añadir al `:root` después de las shadcn):

```css
:root {
  --text-heading: var(--brand-ink-deep);
  --text-body: var(--foreground);
  --text-subtle: var(--gray-700);
  --text-soft: var(--gray-600);
  --text-disabled: var(--gray-500);
  --text-link: var(--primary);
  --text-link-hover: var(--brand-magenta-dark);
  --text-inverse: 0 0% 100%;
  --text-success: var(--success);
  --text-warning: var(--warning);
  --text-destructive: var(--destructive);
}
```

Y exponerlos en Tailwind como utilidades en `tailwind.config.{ts,js}`:

```ts
colors: {
  heading: 'hsl(var(--text-heading))',
  body: 'hsl(var(--text-body))',
  subtle: 'hsl(var(--text-subtle))',
  soft: 'hsl(var(--text-soft))',
  disabled: 'hsl(var(--text-disabled))',
  link: {
    DEFAULT: 'hsl(var(--text-link))',
    hover: 'hsl(var(--text-link-hover))',
  },
  // …existing entries (primary, secondary, muted, etc.)
}
```

Permite escribir `text-heading`, `text-body`, `text-subtle`, `text-soft`, `text-link hover:text-link-hover`.

Regla: **NO usar `text-foreground` / `text-muted-foreground` literal en JSX nuevo**. Usar `text-body` y `text-subtle`. Si ves `text-foreground` en código antiguo, sustituir cuando toques esa zona. Reduce ambigüedad y hace los componentes self-documenting.

### 2.7 Streamlit (tools de compañeros)

El template Streamlit no consume CSS variables; necesita hex literal. Cada tool debe llevar este `.streamlit/config.toml`:

```toml
[theme]
base = "light"
primaryColor = "#EB1C79"
backgroundColor = "#FFFFFF"
secondaryBackgroundColor = "#F4F4F4"
textColor = "#333333"
font = "sans serif"
```

Para acentos puntuales (títulos, separadores, badges) en Streamlit, usar `st.markdown` con `color:#3B189B` o cualquier hex del §2.4 literal — no hay variables.

### 2.8 Modo oscuro

iseazy.com no publica una guía oscura oficial. Definimos un modo oscuro **funcional** que mantiene la identidad (magenta primary, purple secondary, verde lima success) y respeta el contrato shadcn (cuando `<html class="dark">` está presente, todas las variables cambian).

Decisiones de diseño:

- **`--primary` (magenta) NO cambia**. #EB1C79 tiene contraste suficiente sobre fondos oscuros (4.6:1) y es el color *de* la marca.
- **`--secondary` (purple) sube de luminosidad** — el #3B189B es demasiado oscuro sobre fondo oscuro. Lo subimos a `~278 70% 68%` para que se lea.
- **Greys se invierten**: lo que era `gray-100` claro pasa a ser `gray-900` oscuro como background; los textos saltan al lado claro de la escala.
- **Card** un nivel por encima del background — superficie elevada que se distingue.
- **Border** muy sutil (`hsl(0 0% 22%)`) para no romper el ambient.

```css
.dark {
  --background: 220 13% 9%;            /* #14161A — fondo principal */
  --foreground: 0 0% 96%;               /* #F4F4F4 — texto cuerpo */
  --card: 220 13% 13%;                  /* #1C1F24 — superficie card */
  --card-foreground: 0 0% 96%;

  --primary: 329 84% 51%;               /* #EB1C79 — sin cambios */
  --primary-foreground: 0 0% 100%;

  --secondary: 278 70% 68%;             /* #B47AE5 — purple subido */
  --secondary-foreground: 220 13% 9%;   /* contraste sobre purple claro */

  --muted: 220 13% 17%;                 /* #24272E — gris suave oscuro */
  --muted-foreground: 0 0% 60%;         /* #999 — secondary text */

  --accent: 196 96% 60%;                /* azul subido */
  --accent-foreground: 220 13% 9%;

  --destructive: 354 70% 60%;           /* rojo subido */
  --destructive-foreground: 0 0% 100%;

  --success: 93 53% 60%;                /* verde lima subido */
  --warning: 45 86% 60%;                /* dorado subido */

  --border: 0 0% 22%;                   /* #383838 — divider sutil */
  --input: 0 0% 22%;
  --ring: 329 84% 51%;                  /* sigue magenta */

  /* §2.1 brand extended — recalculados */
  --brand-magenta-dark: 333 79% 60%;
  --brand-magenta-deep: 334 84% 35%;
  --brand-magenta-soft: 330 30% 15%;    /* magenta-soft pasa a ser fondo magenta tenue oscuro */
  --brand-purple-mid: 292 64% 60%;
  --brand-purple-deep: 253 72% 40%;
  --brand-ink-deep: 0 0% 98%;           /* invertido — ahora el "ink-deep" es el blanco más blanco */

  /* §2.5 grey scale — invertida */
  --gray-50: 0 0% 13%;                  /* lo que era casi blanco ahora es casi negro */
  --gray-100: 0 0% 15%;
  --gray-200: 0 0% 17%;
  --gray-300: 0 0% 22%;
  --gray-400: 0 0% 32%;
  --gray-500: 0 0% 50%;
  --gray-600: 206 9% 60%;
  --gray-700: 0 0% 75%;
  --gray-800: 0 0% 88%;
  --gray-900: 0 0% 96%;

  /* §2.6 text aliases — se ajustan solos vía mapeo a las nuevas variables */
  --text-heading: 0 0% 98%;
  --text-body: 0 0% 96%;
  --text-subtle: 0 0% 75%;
  --text-soft: 0 0% 60%;
  --text-disabled: 0 0% 40%;
  --text-link: 329 84% 60%;             /* magenta ligeramente subido para legibilidad */
  --text-link-hover: 329 84% 70%;
  --text-inverse: 220 13% 9%;
}
```

Activación:

- **Tools React + shadcn**: usar `next-themes` o `localStorage` para alternar `class="dark"` en `<html>`. shadcn ya soporta el contrato.
- **HTML estático**: añadir `class="dark"` al `<html>` y todas las variables saltan.
- **Streamlit**: `[theme] base = "dark"` en `.streamlit/config.toml` con `primaryColor = "#EB1C79"`, `backgroundColor = "#14161A"`, `secondaryBackgroundColor = "#1C1F24"`, `textColor = "#F4F4F4"`.

Reglas que se mantienen en oscuro:

- **Aura de buttons** sigue siendo del color del botón al 16% — sobre fondo oscuro queda más sutil pero coherente.
- **Category accents** (`--accent-author`, etc.) sin cambios — sus colores siguen siendo identidad de producto.
- **Decorative palette** (`--deco-*`) sin cambios — los colores decorativos mantienen su saturación.

> **Cuando NO usar dark mode**: si la tool es marketing-facing (landing pública), default a claro. Dark es para apps internas / dashboards / herramientas largas de uso intensivo donde el contraste alto cansa.

---

## 3. Tipografía

iseazy.com declara explícitamente `font-family: 'Work Sans', sans-serif !important` en su CSS. Lo adoptamos para coherencia visual entre web y dashboards internos.

### 3.1 Carga

Añadir al `<head>` de `frontend/index.html`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link
  href="https://fonts.googleapis.com/css2?family=Work+Sans:wght@400;500;600;700&display=swap"
  rel="stylesheet"
/>
```

Y en `globals.css`:

```css
body {
  font-family: "Work Sans", ui-sans-serif, system-ui, -apple-system, sans-serif;
}
```

Pesos a cargar: **400, 500, 600, 700**. No cargar 300/800/900 — no se usan en la web pública y añaden peso al bundle.

Fallback offline (para entornos sin Google Fonts): `ui-sans-serif, system-ui` — los componentes seguirán siendo legibles aunque pierdan personalidad.

### 3.2 Escala tipográfica nombrada

Cada estilo tiene **nombre canónico**, clases Tailwind fijas y un único uso correcto. Cuando un diseñador o Claude diga "esto es un `h2`" o "esto es un `eyebrow`", todos saben exactamente qué tamaño, peso, color y line-height aplicar.

| Token | Clases Tailwind | Tamaño | Peso | Color | Uso |
|---|---|---|---|---|---|
| `t-display` | `text-5xl md:text-6xl font-semibold tracking-tight leading-[1.1] text-heading` | 48/60 px | 600 | heading | Hero principal de landing. Uno por página máximo. |
| `t-h1` | `text-4xl font-semibold tracking-tight leading-tight text-heading` | 36 px | 600 | heading | Título de página. Uno por página. |
| `t-h2` | `text-3xl md:text-4xl font-semibold tracking-tight leading-tight text-heading` | 30/36 px | 600 | heading | Cabecera de sección (`section-heading` §5.4.1). |
| `t-h3` | `text-2xl md:text-3xl font-semibold tracking-tight leading-snug text-heading` | 24/30 px | 600 | heading | Subsección, título de `split-section`, `showcase-card`. |
| `t-h4` | `text-lg font-semibold leading-snug text-heading` | 18 px | 600 | heading | Título dentro de cards (`category-card`, `media-card`, `accordion-list`). |
| `t-eyebrow` | `text-sm font-medium uppercase tracking-wide text-subtle` | 14 px | 500 | muted | Pre-título sobre h2/h3. Categoría de sección. |
| `t-body-lg` | `text-lg leading-relaxed text-subtle` | 18 px | 400 | muted | Body en landings, descripción bajo `t-h1`/`t-h2` con peso visual. |
| `t-body` | `text-base leading-normal text-body` | 16 px | 400 | body | Body por defecto. |
| `t-body-sm` | `text-sm leading-normal text-subtle` | 14 px | 400 | muted | Body denso dentro de cards, descripciones, metadata. |
| `t-caption` | `text-xs font-medium text-soft` | 12 px | 500 | soft | Labels de métricas, footnotes, badges textuales. |
| `t-quote` | `text-lg leading-relaxed italic text-body` | 18 px | 400 italic | body | Cita en `quote-card`. |
| `t-metric` | `text-5xl md:text-6xl font-semibold tabular-nums leading-none` | 48/60 px | 600 | accent (var) | Número grande en `stat-block`. El color cambia por slot (§2.4). |
| `t-metric-label` | `text-sm text-subtle mt-2 max-w-[18ch]` | 14 px | 400 | muted | Label bajo `t-metric`. |
| `t-link` | `text-sm font-medium text-link hover:text-link-hover` | inherited | 500 | link | Link "ver más →", CTA inline. |
| `t-code` | `font-mono text-sm text-body bg-muted/40 px-1.5 py-0.5 rounded` | 14 px | 400 mono | body | Código inline, nombres de tokens. |

**Reglas que no se discuten**:

- **`tabular-nums` obligatorio** en cualquier cifra comparable visualmente columna a columna (KPIs, tablas, contadores, `comparison-table`). Sin esto los dígitos saltan de anchura entre renders y se ve mal.
- **No mezclar pesos sin necesidad**. La jerarquía la hace el tamaño y el color, no el peso. Casi todos los textos son 400 o 600 — el 500 está reservado a eyebrow, caption y link.
- **El color va con el tamaño**. Heading (h1-h4) → `text-heading`. Body cuerpo → `text-body`. Descripciones secundarias y metadata → `text-subtle`. Footnotes → `text-soft`. Nunca usar `text-foreground` o `text-muted-foreground` literal en JSX nuevo (§2.6).
- **No inventar tamaños intermedios**. Si necesitas algo entre `t-body` y `t-h4`, casi siempre la solución correcta es repensar la jerarquía, no añadir `text-base font-semibold` improvisado.

### 3.3 Combinaciones canónicas

En la práctica los textos no aparecen sueltos: van en **stacks** verticales con espaciado fijo. Estos son los 6 stacks que el sistema reconoce — cada uno tiene su rol y su espacio entre líneas. Cualquier composición de texto en una pantalla debe encajar en uno de ellos.

Convención: cada stack se nombra `s-{rol}` para distinguirlo de los `t-` tipográficos individuales. Los espacios verticales son obligatorios — son lo que hace que dos pantallas distintas "respiren" igual.

#### 3.3.1 `s-page-header` — cabecera de landing

```
t-eyebrow    ───────── mb-3
t-display    ───────── mb-4
t-body-lg    ─────────
```

Hero de landing. Solo aparece **una vez por página** y siempre en el top. Centrado o izquierda según diseño del bloque.

#### 3.3.2 `s-section-header` — cabecera de sección

```
t-eyebrow    ───────── mb-3
t-h2         ───────── mb-3
t-body       ─────────
```

Patrón `section-heading` (§5.4.1). Centrado por defecto. Espacio externo: `mt-16 mb-10` arriba/abajo del stack entero.

#### 3.3.3 `s-subsection-header` — cabecera de bloque interno

```
t-eyebrow    ───────── mb-2
t-h3         ───────── mb-3
t-body       ─────────
```

Para `split-section`, secciones secundarias dentro de una pantalla, cabeceras de `showcase-card`. Alinear a izquierda casi siempre.

#### 3.3.4 `s-card-stack` — contenido de card con icono o media

```
icon/media          ───── mb-2
t-h4                ───── mb-1
t-body-sm           ───── mt-auto
t-link              ─────
```

Patrón interno de `category-card`, `media-card`. El `mt-auto` antes del link asegura que la flecha "→" queda anclada abajo aunque las descripciones varíen de longitud — todas las cards de la misma fila terminan visualmente alineadas.

#### 3.3.5 `s-stat-stack` — métrica con label

```
t-metric            ───── mt-0
t-metric-label      ─────
```

Patrón `stat-block` (§5.4.8). Sin más adornos. El color del `t-metric` rota por slot dentro del bloque (§2.4 decorative palette, orden estable).

#### 3.3.6 `s-quote-stack` — cita con atribución

```
logo (opcional)     ───── mb-6
t-quote             ───── mb-6
avatar + nombre/rol ─────
   ├─ avatar size-12 rounded-full
   ├─ t-body (font-semibold)  ← nombre
   └─ t-body-sm                ← rol
```

Patrón `quote-card` (§5.4.5). El logo arriba ocupa altura fija ~32 px, no escala con la cita. Comilla decorativa absoluta detrás (es decoración, no parte del stack).

---

### 3.4 Aplicación rápida (cheatsheet)

Cuando arranques una pantalla, sigue este árbol de decisión:

1. **¿Tengo un título de página?** → `t-h1` + opcional `t-eyebrow` arriba → `s-page-header` si es landing, plano si es app interna.
2. **¿Voy a dividir en secciones?** → cada cabecera de sección es `s-section-header`.
3. **¿Cada sección tiene sub-bloques con título propio?** → `s-subsection-header`.
4. **¿Tengo cards con icono/media + título corto + descripción + CTA?** → `s-card-stack` dentro de `category-card` o `media-card`.
5. **¿Tengo métricas grandes?** → `s-stat-stack` dentro de `stat-block`.
6. **¿Tengo testimonios o citas?** → `s-quote-stack` dentro de `quote-card`.
7. **¿Body suelto entre bloques?** → `t-body` (denso) o `t-body-lg` (landing-style).

Si una pantalla tiene texto que no encaja en ningún `t-*` ni en ningún `s-*`, parar antes de inventar tamaño nuevo.

---

## 4. Layout & sistema visual

Capas estructurales del sistema. Cualquier tool consumidora debe respetar estos números — son los que hacen que dos pantallas distintas "respiren" y "suenen" igual.

### 4.1 Container & breakpoints

Mobile-first: el estilo base se aplica a mobile y los prefijos `md:`/`lg:` añaden capas hacia arriba.

| Breakpoint | Min width | Tailwind | Cuándo cambia layout |
|---|---|---|---|
| (base) | 0 | — | Mobile portrait — single column, padding 1rem. |
| `sm` | 640 px | `sm:` | Mobile landscape, tablet portrait estrecho — rara vez cambia layout. |
| `md` | 768 px | `md:` | Tablet portrait / iPad — primer split a 2 columnas. |
| `lg` | 1024 px | `lg:` | Desktop — grids 3-4 columnas. |
| `xl` | 1280 px | `xl:` | Desktop ancho — solo para layouts muy específicos. |
| `2xl` | 1400 px | `2xl:` | Container max. **No** seguir creciendo más allá — la línea queda demasiado larga. |

**Container canónico** (Tailwind config):

```ts
container: {
  center: true,
  padding: { DEFAULT: "1rem", md: "2rem" },
  screens: { "2xl": "1400px" },
}
```

Uso: `<main class="container mx-auto px-4 md:px-8 pb-32">…</main>`.

**Cuando NO usar container**: hero banners, marquee-strip y otros bloques full-width sangran al viewport. Para esos, layout fuera del container y el bloque interno con su `max-w-{n} mx-auto`.

### 4.2 Spacing scale

Tailwind base: 1 unidad = 4 px. No inventar valores intermedios (`space-y-7`, `gap-3.5`) — usar los canónicos.

**Stacks verticales** (entre elementos en columna):

| Clase | Píxel | Cuándo |
|---|---|---|
| `space-y-2` | 8 | Items muy ligados (label + input dentro de un field). |
| `space-y-4` | 16 | Items dentro de una card (titulo + descripción + CTA). |
| `space-y-5` | 20 | Fields en un formulario. |
| `space-y-6` | 24 | Sub-bloques dentro de una sección. |
| `space-y-8` | 32 | Cards en una lista vertical. |
| `space-y-10` | 40 | Bloques de feature dentro de una página densa. |
| `space-y-16` | 64 | Secciones grandes en una landing. |
| `space-y-24` | 96 | Regiones de página en marketing — rara vez en apps internas. |

**Gaps horizontales / inline** (entre elementos en fila):

| Clase | Píxel | Cuándo |
|---|---|---|
| `gap-1.5` | 6 | Icono + label muy ajustado. |
| `gap-2` | 8 | Icono + texto en botones, badges. |
| `gap-3` | 12 | Botones agrupados (form footer). |
| `gap-4` | 16 | Pills, chips. |
| `gap-6` | 24 | Cards en grid (`grid gap-6`). |
| `gap-8` | 32 | Columnas de página (sidebar + content). |
| `gap-12` | 48 | Split-section media+texto. |

**Padding canónico**:

| Componente | Padding |
|---|---|
| `category-card`, `media-card` body | `p-6` (24) |
| `quote-card`, `showcase-card` | `p-8` (32) |
| `stat-block`, `cta-banner` | `p-8 md:p-12` (32 → 48) |
| `field-input` | `px-4` (16 horizontal) + altura fija |
| `btn-md` | `px-4 py-3` |

### 4.3 Border-radius scale

| Token | Valor | Tailwind | Cuándo |
|---|---|---|---|
| `radius-sm` | 4 px | `rounded-sm` | Badges pequeños, marcas de check. |
| `radius-md` | 6 px | `rounded-md` | Pills, tags, inputs sm. |
| `radius-lg` | 8 px | `rounded-lg` | **Default**: buttons, cards, inputs, alerts. |
| `radius-xl` | 12 px | `rounded-xl` | Cards grandes, modales. |
| `radius-2xl` | 16 px | `rounded-2xl` | Hero banners, `cta-banner` gradient. |
| `radius-full` | 9999 px | `rounded-full` | Avatars, pills de filtro, switches, indicadores. |

`--radius` en `:root` apunta a `radius-lg` (0.5rem). Los demás se derivan en la config Tailwind (ya documentado en §2.2).

> **Regla**: una pantalla mezcla **máximo 2 niveles de radius adyacentes**. Si tienes una card `rounded-lg`, sus elementos interiores deben ser `rounded-md` o `rounded-lg`. Saltar de `rounded-lg` a `rounded-full` dentro del mismo bloque crea ruido visual.

### 4.4 Shadows / elevation

Niveles de elevación apilables. La web pública apenas usa sombras gruesas; nuestro sistema sigue la misma línea minimalista.

| Nivel | Tailwind | Cuándo |
|---|---|---|
| `elev-0` | (ninguna) | Default. Bloques planos, fondos. |
| `elev-1` | `shadow-sm` | Card interactiva no destacada (`category-card` en hover, fila de tabla hover). |
| `elev-2` | `shadow` | Dropdowns, popovers, menus contextuales. |
| `elev-3` | `shadow-md` | Modales, dialogs. |
| `elev-4` | `shadow-lg` | Toasts flotantes, cards hero destacadas. |

> **"Aura" de buttons/inputs (§5.6, §5.7) NO es elevation**. Es identidad de marca (sombra magenta/purple del propio elemento). Convive con `elev-0` por defecto — un botón primary no tiene `shadow-sm` además del aura.

Anti-patrón (§5.5): nada de `shadow-2xl` o `drop-shadow-2xl` en cards "flotantes" — produce el look "AI slop".

### 4.5 Z-index scale

Sin escala canónica, cada tool inventa números y aparecen bugs de stacking. Esta es la única escala válida:

| Nivel | Tailwind | Capa |
|---|---|---|
| 0 | `z-0` | Contenido base de la página. |
| 10 | `z-10` | Sticky elements (header, sticky tabs, columna sticky en tabla). |
| 20 | `z-20` | Dropdowns, menus contextuales, popovers. |
| 30 | `z-30` | Tooltips. |
| 40 | `z-40` | Backdrop de modales (overlay). |
| 50 | `z-50` | Content de modales / dialogs. |
| 60 | `z-[60]` | Toasts (deben aparecer sobre modales). |
| 70 | `z-[70]` | Command palette / cmd-k (sobre todo). |

> **Regla**: nunca usar `z-index: 9999` o números arbitrarios. Si necesitas algo por encima del nivel 70, replantea — probablemente hay un bug de layering, no un nivel faltante.

### 4.6 Motion

Durations canónicas:

| Token | Duration | Uso |
|---|---|---|
| `duration-fast` | 100 ms | Micro-feedback: cambio de color en hover de un link. |
| `duration-default` | 150 ms | Transiciones de estado (focus, hover de inputs, color de texto). |
| `duration-200` | 200 ms | Botones, cards (hover de border). |
| `duration-base` | 300 ms | Enter/leave de elements pequeños (dropdown, tooltip). |
| `duration-slow` | 500 ms | Enter/leave de elements grandes (modales, drawers). |
| `duration-marquee` | 30 s | Animaciones decorativas continuas (`marquee-strip`). |

Easings canónicas:

| Easing | cubic-bezier | Uso |
|---|---|---|
| `ease-out` | (0, 0, 0.2, 1) | **Default** para enter, focus, hover. Lo más usado. |
| `ease-in` | (0.4, 0, 1, 1) | Leave / exit (el elemento sale más rápido al final). |
| `ease-in-out` | (0.4, 0, 0.2, 1) | Toggles, switches, accordion. |
| `linear` | — | Animaciones continuas decorativas (marquee, spinners). |

`prefers-reduced-motion`:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

Excepciones que se mantienen incluso con reduced-motion: transiciones de estado críticas (focus visible, validación de formularios). Lo decorativo (marquee, hover translate) se desactiva.

### 4.7 Focus state global

**Regla**: cualquier elemento focusable por teclado (a, button, input, select, textarea, [tabindex]) debe mostrar foco visible. Nunca `outline: none` sin reemplazo.

Patrón canónico (ya aplicado en buttons §5.6 e inputs §5.7):

```css
*:focus-visible {
  outline: 2px solid hsl(var(--primary));
  outline-offset: 2px;
  border-radius: inherit; /* Para que el outline siga la curva del elemento */
}
```

O via Tailwind en cada componente:

```
focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-primary focus-visible:ring-offset-2
```

- **`focus-visible`** (no `focus`) → solo se ve cuando el usuario navega con teclado, no al hacer click con mouse.
- **`ring-offset-2`** → separa el ring del elemento, para que se vea claro sobre cualquier fondo.
- **`ring-primary`** → magenta, alineado con la identidad de marca. NO usar el azul de browser por defecto.

Excepciones que NO deben llevar foco visible:

- `<div>` con `tabindex="-1"` programático (foco no usuario).
- Elementos puramente decorativos con `aria-hidden="true"`.

> **Densidad**: la app es B2B interna. Prioridad a densidad y escaneabilidad sobre aire — no copiar el espaciado generoso del hero de la web pública dentro de las pantallas operativas. (Regla heredada del §4 original, sigue aplicando.)

---

## 5. Componentes base

Stack recomendado para tools consumidoras en React: **React 18 + Tailwind + shadcn/ui + Tabler Icons + TanStack Query/Table**. Para charts: **recharts** (§5.3). Para HTML estático / Streamlit: ver §5.2 y §2.6.

No introducir librerías extra sin justificar. Este sistema NO impone framework — Streamlit, HTML estático, Next.js, Vite consumen los mismos tokens y patrones.

### 5.1 Convenciones de componente

Cuando una tool consumidora construye un componente nuevo, aplican estas convenciones por defecto. Vienen de patrones repetidos en la web pública y en las primeras tools internas.

- **Card como link**: bloque envoltorio `<a>`/`<Link>` con `rounded-lg border bg-card p-6 transition-colors hover:border-primary` + flecha `→` que se traslada en hover (`transition-transform group-hover:translate-x-1`). Es el patrón base que reutilizan `category-card`, `media-card` y `showcase-card` (§5.4).
- **Header con navegación horizontal**: nav con items `rounded-md`, item activo `bg-primary text-primary-foreground`. Evitar sidebar permanente (§5.5 anti-patrón 1).
- **Sección de métricas operativas (densa)**: `rounded-lg border bg-card p-6`, grid 2/4 columnas, eyebrow `text-sm uppercase tracking-wide text-subtle`. Distinta de `stat-block` (§5.4.8), que es marketing-style.
- **Status pill**: usar `--accent` (azul info) para "running"/"in-progress", `--success` (verde lima) para "completed", `--destructive` para "failed", `--warning` para "aborted/stale", `--muted` para "pending/queued". Punto pulsante (`animate-pulse`) opcional para estados activos.
- **Progreso indeterminado**: barra `animate-pulse bg-accent` (no `bg-blue-500` genérico).

### 5.2 Iconografía

**Set canónico: Tabler Icons** ([tabler.io/icons](https://tabler.io/icons)). ~5500 iconos outline, estilo consistente y calidad alta. Elegido por amplitud de cobertura — no es estrictamente el set del brandbook isEazy (que no tenemos público) pero es el más cercano y suficiente para todas las pantallas internas y de tooling.

| Entorno | Cómo se usa | Tamaños canónicos |
|---|---|---|
| **React + Tailwind** (Marketing Dashboard, tools React) | `import { IconPencil } from "@tabler/icons-react"` → `<IconPencil size={16} />` | `size={16}` inline en texto, `size={20}` en cabeceras de card, `size={32}` en `category-card` variante compact, `size={40}` en `category-card` variante default. |
| **HTML estático** (playground, landings de tool-starter) | Webfont CDN: `<link href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.24.0/dist/tabler-icons.min.css" rel="stylesheet" />` → `<i class="ti ti-pencil text-base"></i>`. Color via `text-{color}`, tamaño via `text-base` / `text-[32px]` / `text-[40px]`. | Mismas equivalencias que arriba en px. |
| **Streamlit** (tools de compañeros) | Emoji unicode o SVG de Tabler descargado y subido como asset. No instalar el paquete completo en el bundle. | — |

Reglas:

- **No mezclar sets**. Si una pantalla usa Tabler, no metas un icono Heroicons/Lucide/Material "porque queda mejor ese".
- **No usar emojis** como icono dentro de componentes renderizados (§1.2).
- **Stroke**: por defecto el del set (2). No retocar stroke vía CSS para "suavizar" — si el icono se ve cargado, probablemente está sobredimensionado.
- **Color**: el color va al stroke (currentColor en SVG, `color` en font). Para iconos representativos de categoría (en `category-card`, `tab-pills`, `stat-block`), coger color de §2.3 (`text-author`, `text-skills`…). Para iconos inline en texto, heredan `text-foreground` o `text-subtle` según el contexto.
- Si Tabler no tiene el icono que necesitas (raro con 5500), avisar a Hugo antes de meter uno de otro set.

### 5.3 Gráficos (recharts)

Para tools React que usen `recharts`, esta es la paleta canónica de series:

```ts
export const CHART_COLORS = [
  "#EB1C79", // brand-magenta (1ª serie)
  "#3B189B", // brand-purple (2ª serie)
  "#03A0F4", // brand-info / sky
  "#73C13C", // brand-success / lime
  "#DCB01B", // deco-gold (skills)
  "#6538E0", // deco-violet (engage)
  "#FF6900", // deco-orange
  "#F78DA7", // deco-rose
] as const;
```

Reglas:

1. **Una serie** → magenta.
2. **Dos series** → magenta + púrpura.
3. **N series** → seguir el orden de `CHART_COLORS` (familias afines no consecutivas).
4. **Won/lost** → `--success` / `--destructive`, no magenta/púrpura (evitar confundir KPI positivo con marca).
5. **Producto isEazy específico** → usar `--accent-{producto}` del §2.3 sobre el array (ej: serie "Author" → magenta, serie "LMS" → azul, etc.).
6. **Eje y grid** → `hsl(var(--muted-foreground))` con opacidad 0.3 para grid.

### 5.4 Patrones de bloque

> **Meta-principio (leer antes de aplicar cualquier patrón)**
>
> Los patrones siguientes son **primitivos visuales genéricos**, NO clones de la web pública de iseazy. Los nombres y descripciones son intencionalmente abstractos: una `media-card` sirve para un post de blog, un caso de éxito, una ficha de cliente en un CRM, una campaña de marketing o una ficha de país en un dashboard ejecutivo — cualquier contenido cuya identidad visual descanse en una imagen prominente.
>
> **NO renombrar patrones por su caso de uso** ("post-card", "client-card", "campaign-card"). Eso fragmenta el sistema y obliga a duplicar clases y reglas. El nombre canónico es el abstracto.
>
> **Cómo elegir patrón** ante un contenido nuevo:
> 1. Identifica el **shape** del contenido — ¿es item con icono+texto? ¿con foto+texto? ¿cita? ¿métrica? ¿comparativa?
> 2. Busca en la lista de abajo el patrón cuyo shape coincide.
> 3. Si ningún patrón encaja, **parar y consultar con Hugo** antes de añadir uno nuevo. La presión a "crear un patrón nuevo para mi caso" es exactamente lo que rompe sistemas de diseño.
>
> Los **ejemplos de uso** que verás junto a cada patrón son ilustrativos, no exhaustivos. Si tu caso no aparece pero su shape sí, úsalo igual — eso es el principio.
>
> Estos primitivos también son una defensa contra el look "AI slop" (sidebar + cards uniformes redondeadas con borde fino, todo magenta-bordered, todo igual). La variedad real entre patrones — card con icono ≠ card con foto ≠ cita ≠ stat ≠ accordion — es lo que hace que la UI no parezca generada.

Convención por patrón: `Qué es` (en abstracto), `Ejemplos de uso` (variados), `Anatomía` (DOM en orden), `Clases` (Tailwind canónico), `Variantes` (cuando las hay) y `Cuándo NO`.

---

#### 5.4.1 `section-heading`

**Qué es**: cabecera tipográfica para separar bloques verticalmente en una página. Eyebrow opcional, h2 grande, sub opcional.

**Ejemplos de uso**:
- Título "Configuración de sincronización" en una pantalla operativa.
- Cabecera "Resultados por país" antes de un grid de stats.
- Separador "Preguntas frecuentes" antes de un `accordion-list`.
- Título de landing "Descubre las tools de marketing".

**Anatomía**: eyebrow opcional → h2 → sub opcional.

**Clases del `h2`**: `text-3xl md:text-4xl font-semibold tracking-tight text-foreground text-center`. Centrado por defecto; alinear a izquierda solo cuando la sección es panel operativo, no landing-like.

**Espacio**: `mt-16 mb-10` arriba/abajo del bloque. `mb-3` entre eyebrow→h2, `mt-3` entre h2→sub.

**Cuándo NO**: dentro de tarjetas (allí va `h3 text-lg font-semibold` del §3.2). Para títulos de página entera (van fuera del flujo, en el header del layout).

---

#### 5.4.2 `tab-pills`

**Qué es**: selector horizontal de 2-6 opciones con icono opcional + label, donde la activa se marca con underline.

**Ejemplos de uso**:
- Selector de producto isEazy (Author / Factory / Skills…).
- Cambio de vista en dashboard (Tabla / Gráfico / Mapa).
- Filtro de status en lista (Todos / Abiertos / Cerrados).
- Período en KPIs (Hoy / Semana / Mes / Trimestre).

**Anatomía**: contenedor `role="tablist"` → N `<button>` con icono opcional + label → bajo el activo, línea de 2 px.

**Clases del tab**: `flex items-center gap-2 px-4 py-3 text-sm font-medium text-subtle hover:text-foreground transition-colors border-b-2 border-transparent`.

**Activo**: `text-foreground border-b-2 border-{accent}`. El color del underline depende del contexto — si es producto, color del producto (§2.3); si es genérico, `border-primary`.

**Cuándo NO**: para >6 tabs (usar dropdown). Para tabs dentro de una tarjeta (usar `Tabs` de shadcn estándar, más compacto).

---

#### 5.4.3 `category-card`

**Qué es**: card unitaria con icono coloreado + título + descripción + CTA. El icono toma color de la categoría que representa (§2.3 si es producto isEazy, un color de §2.4 asignado de forma estable si es otra categoría).

**Ejemplos de uso**:
- Tarjeta de producto isEazy en una home.
- Item de "tipo de tool" en una landing interna.
- Card de "área de la empresa" (Marketing / Ventas / Producto).
- Card de "tipo de KPI" en un setup.
- Card de "fuente de datos" (Salesforce / HubSpot / GA4) en una configuración.

**Variantes**:
- **`default`**: icono 40 px, descripción larga (50-80 palabras).
- **`compact`**: icono 32 px, descripción corta (max 25 palabras). Para grids densos o cuando la card es navegación, no exhibición.

**Anatomía**: contenedor card → icono → `h4` → `p` → `a` "CTA →".

**Clases del contenedor**: `rounded-lg border bg-card p-6 flex flex-col gap-3 transition-colors hover:border-primary`.

**Icono**: `<i class="ti ti-X text-[40px] text-{accent} mb-2"></i>` (default) o `text-[32px]` (compact). Solo se tinta el stroke; el fondo del icono queda sin color.

**Título**: `text-lg font-semibold text-foreground`.

**CTA**: `mt-auto inline-flex items-center gap-1 text-sm font-medium text-primary group` con flecha `transition-transform group-hover:translate-x-1`.

**Grid contenedor**: `grid gap-6 md:grid-cols-2 lg:grid-cols-4 items-start` (para que cards cortas no se inflen).

**Cuándo NO**: para >12 items (es navegación, no exhibición — usar lista densa o tabla). Para items con foto/screenshot prominente (usar `media-card`).

---

#### 5.4.4 `media-card`

**Qué es**: card con **media visual prominente** (imagen/screenshot/avatar/ilustración) + título + opcional metadata + CTA. La media domina la composición; el texto identifica.

**Ejemplos de uso**:
- Artículo de blog (imagen + título).
- Caso de éxito (foto + cliente + sector).
- Ficha de cliente en CRM (avatar + razón social + última interacción).
- Experimento de marketing (gráfico thumbnail + nombre + estado).
- Ficha de producto interno (screenshot + nombre + tagline).
- País en dashboard ejecutivo (mapa thumb + nombre + KPI principal).

**Variantes**:
- **`blob-top`**: media con clip-path orgánico elíptico. Look editorial.
- **`rect-top`**: media rectangular 16:9 o 4:3, sin recorte. Look operativo/factual.
- **`badge-overlay`**: media + badge superpuesto en `absolute top-3 left-3` (estado, categoría, año…).

**Anatomía**: contenedor card → wrapper media → cuerpo (h4 + metadata opcional + CTA).

**Clases del contenedor**: `flex flex-col rounded-lg border bg-card overflow-hidden transition-colors hover:border-primary`.

**Media wrapper**: `aspect-[4/3] w-full` con `<img class="object-cover w-full h-full">`. Para `blob-top`, añadir `[clip-path:ellipse(80%_75%_at_50%_25%)]` al wrapper.

**Cuerpo**: `flex flex-col gap-3 p-6`.

**Título**: `text-lg font-semibold text-foreground line-clamp-3`.

**Metadata (opcional)**: `text-sm text-subtle` — fecha, sector, estado…

**CTA**: igual que `category-card`.

**Cuándo NO**: cuando no hay media real disponible y se rellenaría con icono/placeholder genérico (usar `category-card`). Para items operativos en listas densas (usar lista plana con thumb `size-16 rounded-md` inline).

---

#### 5.4.5 `quote-card`

**Qué es**: card con cita destacada + autor identificable. La cita domina; el autor da contexto.

**Ejemplos de uso**:
- Cita interna del equipo sobre una decisión o launch.
- Declaración de un PM sobre una decisión documentada.
- Soundbite destacable de un informe.
- Voz del usuario en un research summary.

**Anatomía**: contenedor card → logo/identificador opcional → `<blockquote>` → comilla decorativa gigante absoluta → fila autor (avatar + nombre + rol).

**Clases del contenedor**: `relative overflow-hidden rounded-lg border bg-card p-8`.

**Cita**: `text-lg leading-relaxed text-foreground italic`. Las comillas tipográficas `“ ”` van **en el texto**, no como pseudo-elemento.

**Comilla decorativa**: glifo `”` en `absolute right-6 bottom-6 text-[10rem] leading-none font-serif text-subtle/50 select-none pointer-events-none` + `aria-hidden`.

**Autor**: `flex items-center gap-3 mt-6` con avatar `size-12 rounded-full object-cover`, nombre `font-semibold text-sm`, rol `text-sm text-subtle`.

**Cuándo NO**: feedback corto sin atribución (usar nota plana). Conversaciones multiparte (formato "interview" distinto).

---

#### 5.4.6 `showcase-card`

**Qué es**: card grande "premium" para destacar un elemento principal cuando hay 3-6 items y cada uno merece espacio propio. Diferencias clave vs `category-card`: usa **logo/identidad propia** (no icono lineal), incluye **media real** (screenshot/foto/datos), y lleva **2+ CTAs**.

**Ejemplos de uso**:
- Card de producto isEazy en home (logo color + screenshot + "saber más" + "demo").
- Card de país en dashboard ejecutivo (bandera + KPI principal + sparkline + "ver detalle" + "editar config").
- Card de campaña activa (nombre + thumbnail + métricas + "pausar" + "editar").
- Card de cliente top en CRM (logo + cifras + "ver perfil" + "crear oportunidad").
- Card de informe destacado (portada + título + "leer" + "compartir").

**Anatomía**: contenedor grande → logo/identidad (color propio) → headline → párrafo → media sangrada → fila de 2 CTAs.

**Clases del contenedor**: `rounded-lg border bg-card p-8 md:p-10 grid md:grid-cols-2 gap-8 items-center`.

**Logo**: usa el color de marca/categoría del item — **no** uniformizar a magenta ni a foreground.

**CTAs**: primer CTA primario (`bg-primary text-primary-foreground rounded-md px-4 py-2 text-sm font-semibold`), segundo outlined (`border border-border text-foreground hover:bg-muted rounded-md px-4 py-2 text-sm font-semibold`).

**Cuándo NO**: para >6 items (sobredimensionado — usar `category-card`). Cuando no hay media real (usar `category-card`).

---

#### 5.4.7 `cta-banner`

**Qué es**: banda full-width o ancho-de-container con headline grande + 1-2 CTAs prominentes. Llamada a la acción consolidada, normalmente al final de una página o como "next step".

**Ejemplos de uso**:
- Cierre de landing pidiendo demo.
- Banner "siguiente paso" tras completar un onboarding.
- Promo de feature nueva al tope de un dashboard.
- "Configura tu sync" en un dashboard sin datos aún.
- Cierre de informe pidiendo acción ejecutiva.

**Variantes** (elegir explícitamente):
- **`gradient`**: fondo `from-primary to-secondary` + blobs de luz blanca. Marketing-y. Para landings y pantallas de primer uso.
- **`photo`**: fondo con foto humana cubierta + overlay oscuro para legibilidad. Para landings con tono más cercano.
- **`flat`**: fondo color sólido (`bg-muted` o accent secundario). Para banners operativos dentro del dashboard sin caer en marketing.

**Anatomía**: contenedor → decoración opcional (blobs / foto / overlay) → contenido (h2 + p + CTAs).

**Clases del contenedor (gradient)**: `relative overflow-hidden rounded-2xl bg-gradient-to-br from-primary to-secondary p-8 md:p-12 text-primary-foreground`.

**Clases del contenedor (photo)**: `relative overflow-hidden rounded-2xl p-8 md:p-12 text-primary-foreground` con `<img>` absoluto cubriendo + `<div class="absolute inset-0 bg-black/50">` para overlay.

**Clases del contenedor (flat)**: `rounded-lg bg-muted p-8 text-foreground`.

**Headline**: `text-3xl md:text-4xl font-semibold tracking-tight max-w-2xl`. **NO centrar** — centrado es exclusivo de `section-heading`. Alinear a izquierda.

**CTAs**: si gradient/photo, primero `bg-white text-primary` y segundo `border border-white/60 text-white`. Si flat, primero `bg-primary text-primary-foreground` y segundo outlined estándar.

**Cuándo NO**: como cabecera de página operativa permanente (rompe densidad B2B del §4). Para mensajes condicionales cortos (usar alert/callout, no banner).

---

#### 5.4.8 `stat-block`

**Qué es**: grupo de 2-4 métricas presentadas con número grande + label corto, cada una en un color accent distinto. Los números son la estrella.

**Ejemplos de uso**:
- "+1K empresas / +90 países / +120K proyectos" en landing.
- KPIs ejecutivos al tope de un dashboard ("3.450 leads / 87 opps / 1,2M€").
- Resultados de un experimento ("+18% conversion / -23% bounce / 1,4× LTV").
- Resumen mensual al inicio de un informe.

**Anatomía**: contenedor card `bg-muted` → grid de N stats → cada stat: número grande + label muted → ilustración opcional a la derecha.

**Clases del contenedor**: `rounded-lg bg-muted p-8 md:p-12 grid md:grid-cols-[1fr_auto] gap-8 items-center`.

**Número**: `text-5xl md:text-6xl font-semibold tabular-nums text-{accent}`. Cada slot usa color distinto **en orden estable** — no rotar entre renders.

**Label**: `text-sm text-subtle mt-2 max-w-[18ch]`.

`tabular-nums` obligatorio (§3.2): si los números cambian dinámicamente, no deben saltar de anchura.

**Cuándo NO**: para KPIs operativos densos (esos usan "Sección de métricas" del §5.1, sin ilustración). Es patrón marketing-style: landings, resúmenes, pantallas de primer uso.

---

#### 5.4.9 `comparison-table`

**Qué es**: tabla comparativa de N opciones con filas de features **agrupadas por categoría**. Una columna puede destacarse como "recomendada".

**Ejemplos de uso**:
- Tabla de precios (Free / Pro / Business / Enterprise).
- Comparativa de fuentes de datos (Salesforce / HubSpot / CSV manual).
- Features por permiso de usuario (Viewer / Editor / Admin / Owner).
- Comparativa de modos de ejecución (Manual / Programado / Tiempo real).
- Capacidades de tools internas lado a lado.

**Anatomía**: toggle opcional encima (anual/mensual, ámbito global) → header de columnas → filas agrupadas por subheader de categoría → fila final de CTAs por columna.

**Clases del contenedor**: `rounded-lg border bg-card overflow-hidden`.

**Header de columnas**: `grid grid-cols-[2fr_repeat(N,1fr)] gap-4 p-6 border-b border-border`. Nombre de opción, valor destacado, descripción corta.

**Columna recomendada**: `bg-primary/5` y badge superior `<span class="bg-primary text-primary-foreground text-xs font-semibold px-3 py-1 -mt-3 rounded-full inline-block">Más popular</span>`.

**Subheader de categoría**: `text-xs uppercase tracking-wide text-subtle bg-muted px-6 py-2`. Agrupa 3-7 filas.

**Fila de feature**: `grid grid-cols-[2fr_repeat(N,1fr)] gap-4 px-6 py-3 border-b border-border/50 text-sm`. Marca de "incluido" con `ti-check text-success`, "no incluido" con `ti-x text-subtle/40`, o valor literal ("5 GB", "Ilimitado").

**Cuándo NO**: solo 2 opciones y pocas features (usar lista de bullets comparada). Opciones no comparables linealmente (usar `category-card` grid).

---

#### 5.4.10 `split-section`

**Qué es**: bloque narrativo en 2 columnas (media + texto). En repeticiones consecutivas dentro de la misma página, el lado del media **alterna** para crear ritmo visual.

**Ejemplos de uso**:
- Sección narrativa explicando una feature ("De idea a curso en minutos").
- Página de detalle de producto/tool con N feature-stories.
- Bloque explicativo de un setup mostrando screenshot del flujo.
- Step largo de un onboarding (un step por `split-section`).
- Caso de éxito desarrollado (foto cliente + narrativa al lado).

**Anatomía**: contenedor 2 columnas → un lado media (imagen sangrada, opcional sobre patrón decorativo), otro lado contenido (eyebrow + h3 + párrafo + bullets opcionales + CTA secundario).

**Clases del contenedor**: `grid md:grid-cols-2 gap-12 items-center py-12`. Para alternancia: añadir `md:[&>div:first-child]:order-last` en repeticiones pares (o aplicar `order-last md:order-first` selectivamente).

**Lado contenido**: `space-y-4`. Eyebrow `text-sm uppercase tracking-wide text-subtle`. h3 `text-2xl md:text-3xl font-semibold tracking-tight`. Bullets con micro-icono `<i class="ti ti-check text-primary text-base"></i>`.

**Lado media**: imagen sangrada al borde del container O sobre patrón tramado (`bg-muted` con dot/grid pattern). Sin marco.

**Cuándo NO**: cuando el media no aporta (usar `section-heading` + párrafo). Para descripciones cortas sin imagen real (usar `category-card` grid).

---

#### 5.4.11 `accordion-list`

**Qué es**: lista vertical de items colapsables. Sin cajas, sin shadows: solo border-bottom entre items.

**Ejemplos de uso**:
- FAQs.
- Lista de configuraciones avanzadas en un setup ("Mostrar opciones avanzadas").
- Detalles por país/segmento en un informe (expand para ver desglose).
- Histórico de sincronizaciones (header con fecha/estado, expand para log).
- Release notes agrupadas por versión.
- Errores agrupados por tipo en una página de troubleshooting.

**Anatomía**: contenedor lista → N items `<details>` con header (label + chevron) + body (párrafo, tabla, lista anidada — lo que sea).

**Clases del item**: `group border-b border-border`.

**Header**: `<summary>` o button con `flex items-center justify-between w-full py-5 text-left text-base font-medium text-foreground hover:text-primary transition-colors cursor-pointer list-none`. Chevron `<i class="ti ti-chevron-down text-base text-subtle transition-transform group-open:rotate-180"></i>`.

**Body**: `pb-5 text-sm text-subtle leading-relaxed`. Sin background, sin padding lateral extra.

**Cuándo NO**: lista corta (<4 items) donde todo cabe expandido (usar lista plana). Items con header complejo o múltiples acciones (usar card con expand-collapse propio).

---

#### 5.4.12 `badge-grid`

**Qué es**: grid denso de imágenes/logos/badges pequeños, sin texto bajo cada uno. Densidad alta: 15-20+ visibles a la vez.

**Ejemplos de uso**:
- Wall de certificaciones/premios.
- Familia de productos isEazy (Author / Factory / Skills / LMS / Engage / Game).
- Integraciones soportadas (Salesforce / HubSpot / Slack / GA4…).
- Tecnologías que la tool consume/produce.
- Galería de partners.

**Anatomía**: heading opcional → grid → cada celda con la imagen oficial sin retocar.

**Clases del grid**: `grid grid-cols-2 sm:grid-cols-3 md:grid-cols-5 gap-6 items-center`.

**Celda**: `flex items-center justify-center h-20`. Imagen `max-h-12 w-auto object-contain`.

**Regla**: usar los logos/badges **oficiales** sin uniformizar a gris. Si una versión gris es necesaria (consistencia visual), aplicar `grayscale opacity-70 hover:grayscale-0 hover:opacity-100 transition`.

**Cuándo NO**: para 4-6 items (sobra espacio — usar inline list o `marquee-strip`). Cuando los items necesitan texto bajo (usar `category-card` compact).

---

#### 5.4.13 `marquee-strip`

**Qué es**: banda horizontal con scroll automático continuo de N items pequeños. Movimiento lento, sin controles, decorativo.

**Ejemplos de uso**:
- Familia de productos isEazy rotando en una landing.
- Frases destacadas rotando ("Reduce 40% tiempo / Aumenta 2× engagement / …").
- Integraciones soportadas en banda continua.
- "Ticker" de KPIs decorativo en pantalla resumen.
- Premios o certificaciones rotando.

**Anatomía**: contenedor `overflow-hidden` → track con `animation: marquee Xs linear infinite` → items duplicados para loop seamless.

**Clases del contenedor**: `relative overflow-hidden`. Masks laterales opcionales: `bg-gradient-to-r from-background via-transparent to-background absolute inset-y-0 left-0 right-0 pointer-events-none`.

**Track**: `flex gap-12 animate-marquee whitespace-nowrap`. Keyframes CSS:

```css
@keyframes marquee {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}
.animate-marquee { animation: marquee 30s linear infinite; }
```

**Items**: `grayscale opacity-60 hover:opacity-100 transition` para logos. Tamaño consistente.

**Cuándo NO**: cuando el usuario necesita seleccionar/interactuar con items (usar `badge-grid` o lista). En dashboards operativos (movimiento constante distrae).

---

#### 5.4.14 `filter-toolbar`

**Qué es**: barra de filtros pills sobre un grid filtrable, con 1-2 ejes independientes. La pill activa de cada eje se distingue claramente.

**Ejemplos de uso**:
- Filtro de casos de éxito por industria × producto.
- Filtro de blog/recursos por tipo × tema.
- Leads por país × estado.
- Campañas por canal × período.
- Tools internas por equipo × estado.

**Anatomía**: label opcional "Filtros:" → eje 1: pills inline (Todos + N opciones) → si hay eje 2: segunda fila de pills.

**Clases del contenedor**: `flex flex-col gap-3 py-4 border-b border-border`.

**Fila**: `flex flex-wrap items-center gap-2`. Label opcional antes: `text-sm text-subtle mr-2`.

**Pill inactiva**: `rounded-full border border-border bg-background px-4 py-1.5 text-sm font-medium text-subtle hover:text-foreground hover:border-primary/40 transition-colors`.

**Pill activa**: `rounded-full bg-primary text-primary-foreground px-4 py-1.5 text-sm font-medium`.

**Cuándo NO**: >8 opciones por eje (usar dropdown multi-select). Cuando los ejes se combinan con lógica compleja AND/OR ambigua (usar filtros explícitos con dropdowns + search).

---

### 5.5 Anti-patrones (qué NO copiar de las landings de "AI slop")

Estos patrones aparecen en muchas tools recientes (Claude, Notion AI, etc.) y **deben evitarse** en el sistema iseazy interno y en las tools de compañeros:

1. **Sidebar permanente fija a la izquierda con avatares circulares y emojis**. Si la app necesita navegación lateral, usar `Layout.tsx` actual (header horizontal). Sidebar solo si pasamos de 8 secciones de primer nivel — entonces consultar.
2. **Cards rectangulares idénticas todas magenta-bordered, sin variación**. Una pantalla debe mezclar patrones distintos del §5.4 (`category-card` con icono, `media-card` con foto, `quote-card`, `stat-block`, `accordion-list`…) y los iconos coger color de categoría (§2.3). Si todas las cards de una página son iguales, algo va mal.
3. **Headings con gradiente de texto**. `bg-clip-text text-transparent bg-gradient-to-r…` → no. Los headings son `text-foreground` o `text-primary-foreground` (sobre `cta-banner`). El gradiente va en fondos, no en tipografía.
4. **Sombras de blur grande (`shadow-2xl`, `drop-shadow-2xl`) en cards "flotantes"**. La web pública apenas usa sombra (§4). Mantener `shadow-sm` máximo.
5. **Pastillas con `bg-gradient` magenta→púrpura como decoración general**. El gradiente está reservado a `cta-banner` variante gradient. En tabs, badges y status, fondos planos.
6. **Iconos animados/bouncing al hover** (`animate-bounce`, `hover:rotate-12`). Solo la flecha `→` se traslada (`group-hover:translate-x-1`). El resto, quieto.
7. **Renombrar un patrón por su caso de uso**. Si Claude propone `client-card`, `post-card`, `experiment-card` cuando ya existe `media-card` o `category-card` — no. Ver meta-principio al inicio del §5.4.

Si una pantalla mezcla 3+ de estos anti-patrones, se ve "AI generada" aunque cada elemento aislado sea correcto.

---

### 5.6 Buttons

Sistema canónico de botones extraído del CSS oficial (clases `.cta-1`, `.cta-2`, `.cta-3` de iseazy.com). El elemento distintivo del look isEazy es el **glow ("aura")**: una box-shadow doble en el color del botón al 16% de opacidad, que da la sensación de halo suave. NO es un drop-shadow gris — es magenta o purple del propio botón.

#### 5.6.1 Anatomía base

Todos los botones comparten:

| Propiedad | Valor |
|---|---|
| `font-family` | Work Sans (§3.1) |
| `font-weight` | 500 (medium) |
| `letter-spacing` | normal |
| `text-decoration` | none |
| `border-radius` | 8 px (`rounded-lg`) |
| `transition` | `all 200ms` |
| `display` | `inline-flex items-center justify-center gap-2` |
| `cursor` | `pointer` (idle), `not-allowed` (disabled) |

#### 5.6.2 Tamaños

| Tamaño | Padding | Font-size | Height | Icono |
|---|---|---|---|---|
| `btn-sm` | `px-3 py-2` | 14 px | 36 px | 14 px |
| `btn-md` (default) | `px-4 py-3` | 16 px | 44 px | 16 px |
| `btn-lg` | `px-6 py-4` | 18 px | 52 px | 20 px |

Los iconos van inline antes o después del label con `gap-2`.

#### 5.6.3 Variantes

##### `btn-primary` (sólido magenta, default action)

```html
<button class="btn btn-md btn-primary">Try it free</button>
```

| Estado | Background | Color | Border | Glow |
|---|---|---|---|---|
| idle | `bg-primary` | `text-white` | none | `box-shadow: 0 0 2px 0 hsl(var(--primary)/0.16), 2px 2px 8px 0 hsl(var(--primary)/0.16)` |
| hover | `bg-secondary` (purple) | `text-white` | none | igual |
| focus-visible | `bg-primary` | `text-white` | none | `ring-2 ring-primary ring-offset-2` |
| active | `bg-magenta-dark` | `text-white` | none | igual |
| disabled | `bg-gray-300` | `text-gray-500` | none | `none` |

> **Hover muy isEazy**: el botón primary magenta al hover pasa a **purple**, no a magenta-dark. Es intencional, sale del CSS oficial. Si Claude propone "magenta-dark" como hover del primary, recordarle esto.

##### `btn-secondary` (sólido purple)

```html
<button class="btn btn-md btn-secondary">Saber más</button>
```

| Estado | Background | Color | Glow |
|---|---|---|---|
| idle | `bg-secondary` | `text-white` | aura purple `0 0 2px 0 hsl(var(--secondary)/0.16), 2px 2px 8px 0 hsl(var(--secondary)/0.16)` |
| hover | `bg-primary` (magenta) | `text-white` | igual |
| focus-visible | `bg-secondary` | `text-white` | `ring-2 ring-secondary ring-offset-2` |
| active | `bg-purple-deep` | `text-white` | igual |
| disabled | `bg-gray-300` | `text-gray-500` | `none` |

##### `btn-outline` (outline magenta, fondo blanco)

```html
<button class="btn btn-md btn-outline">Contáctanos</button>
```

| Estado | Background | Color | Border | Glow |
|---|---|---|---|---|
| idle | `bg-white` | `text-primary` | `border border-primary` | aura magenta (igual que primary) |
| hover | `bg-primary` | `text-white` | `border border-primary` | igual |
| focus-visible | `bg-white` | `text-primary` | `border border-primary` | `ring-2 ring-primary ring-offset-2` |
| disabled | `bg-white` | `text-gray-500` | `border border-gray-300` | `none` |

##### `btn-ghost` (sin fondo ni borde, para acciones secundarias)

```html
<button class="btn btn-md btn-ghost">Cancelar</button>
```

| Estado | Background | Color | Glow |
|---|---|---|---|
| idle | `bg-transparent` | `text-body` | `none` |
| hover | `bg-magenta-soft` | `text-primary` | `none` |
| focus-visible | `bg-transparent` | `text-body` | `ring-2 ring-primary ring-offset-2` |
| disabled | `bg-transparent` | `text-disabled` | `none` |

##### `btn-link` (parece link, comporta como botón)

```html
<button class="btn-link">Más detalles →</button>
```

Sin padding, sin background, sin border. `text-link font-medium`. Hover: `text-link-hover` + `underline`. Útil cuando el botón está embebido en flujo de texto y no debe romper la lectura. Es el mismo tratamiento que `t-link` (§3.2).

##### `btn-destructive` (acción destructiva: borrar, descartar)

```html
<button class="btn btn-md btn-destructive">Eliminar</button>
```

Misma anatomía que `btn-primary` pero con `bg-destructive` (#DC3545) y aura rojo. Hover oscurece a `bg-[#A82734]`. **Confirmar siempre** con un diálogo antes de ejecutar la acción.

##### Botones sobre fondo oscuro (cta-banner gradient/photo)

Cuando el botón está dentro de `cta-banner` con fondo oscuro/gradiente (§5.4.7):

- **Primary sobre dark**: `bg-white text-primary hover:bg-white/90` — no usar magenta sobre magenta.
- **Secondary sobre dark**: `border border-white/60 text-white hover:bg-white/10` — outline white.

#### 5.6.4 Iconos en botones

```html
<button class="btn btn-md btn-primary">
  <i class="ti ti-arrow-right text-base"></i>
  Empezar ahora
</button>
```

- Tabler icon antes o después del label, gap-2.
- `text-base` (16 px) para `btn-md`, `text-sm` (14 px) para `btn-sm`, `text-lg` (20 px) para `btn-lg`.
- Botón icon-only: misma altura y ancho, sin padding asimétrico (`size-9` / `size-11` / `size-12`).

#### 5.6.5 Loading state

```html
<button class="btn btn-md btn-primary" disabled>
  <i class="ti ti-loader-2 animate-spin"></i>
  Procesando…
</button>
```

Spinner Tabler `ti-loader-2` con `animate-spin`. Cambia el label a la acción en gerundio ("Procesando…", "Guardando…", "Sincronizando…").

#### 5.6.6 Implementación

##### React + shadcn

Extender el `Button` de shadcn con variants iseazy en `components/ui/button.tsx`:

```ts
const buttonVariants = cva(
  "inline-flex items-center justify-center gap-2 font-medium transition-all rounded-lg disabled:cursor-not-allowed disabled:bg-gray-300 disabled:text-gray-500 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2",
  {
    variants: {
      variant: {
        primary: "bg-primary text-white hover:bg-secondary active:bg-magenta-dark focus-visible:ring-primary shadow-[0_0_2px_0_hsl(var(--primary)/0.16),2px_2px_8px_0_hsl(var(--primary)/0.16)]",
        secondary: "bg-secondary text-white hover:bg-primary active:bg-purple-deep focus-visible:ring-secondary shadow-[0_0_2px_0_hsl(var(--secondary)/0.16),2px_2px_8px_0_hsl(var(--secondary)/0.16)]",
        outline: "bg-white text-primary border border-primary hover:bg-primary hover:text-white focus-visible:ring-primary shadow-[0_0_2px_0_hsl(var(--primary)/0.16),2px_2px_8px_0_hsl(var(--primary)/0.16)]",
        ghost: "bg-transparent text-body hover:bg-magenta-soft hover:text-primary focus-visible:ring-primary",
        link: "text-link hover:text-link-hover hover:underline font-medium p-0 h-auto",
        destructive: "bg-destructive text-white hover:bg-[#A82734] focus-visible:ring-destructive shadow-[0_0_2px_0_hsl(var(--destructive)/0.16),2px_2px_8px_0_hsl(var(--destructive)/0.16)]",
      },
      size: {
        sm: "h-9 px-3 text-sm",
        md: "h-11 px-4 text-base",
        lg: "h-12 px-6 text-lg",
      },
    },
    defaultVariants: { variant: "primary", size: "md" },
  }
);
```

##### HTML estático / Streamlit

En el playground hay clases CSS reales (`.btn`, `.btn-primary`…) en `<style>` listas para copy-paste. Ver §12.

---

### 5.7 Forms (inputs, labels, validación)

Sistema de campos de formulario alineado con iseazy.com (`input` con bg `#F4F4F4`, sin border, focus con borde magenta) pero extendido para apps internas con accesibilidad y validación. Frente al diseño marketing-y de la web, las tools tienen formularios densos — priorizamos legibilidad sobre estilo.

#### 5.7.1 Field anatomy

Un campo siempre va en un **field group** con label arriba, input en medio, helper/error abajo:

```html
<div class="field">
  <label class="field-label" for="email">Email corporativo</label>
  <input class="field-input" type="email" id="email" placeholder="tu@empresa.com" />
  <p class="field-helper">Usaremos este email para enviarte el acceso.</p>
</div>
```

| Parte | Clase | Estilo |
|---|---|---|
| Contenedor | `.field` | `flex flex-col gap-1.5` |
| Label | `.field-label` | `text-sm font-medium text-heading` |
| Input | `.field-input` | ver §5.7.2 |
| Helper | `.field-helper` | `text-xs text-soft` |
| Error | `.field-error` | `text-xs text-destructive flex items-center gap-1` con icono `ti-alert-circle` |
| Label required | `<span class="text-destructive">*</span>` después del texto del label | |

#### 5.7.2 Input base (`.field-input`)

Aplica a `<input type="text|email|tel|number|password|search|url|date">` y a `<textarea>`/`<select>`.

| Estado | Tailwind | Notas |
|---|---|---|
| idle | `h-11 w-full rounded-lg bg-muted border border-border px-4 text-base text-body placeholder:text-disabled transition-colors` | Altura 44 px (cómoda táctil). |
| hover | añade `hover:border-gray-400-iz` | Indica interactividad sin enfocar. |
| focus | añade `focus:outline-none focus:border-primary focus:ring-2 focus:ring-primary/15` | **El "aura" magenta en focus** — mismo principio que botones, opacidad 15%. |
| disabled | `disabled:bg-gray-100-iz disabled:text-disabled disabled:cursor-not-allowed` | |
| error (data-state) | `aria-invalid:border-destructive aria-invalid:focus:ring-destructive/15` | Marcar `aria-invalid="true"` cuando hay error. |
| readonly | `read-only:bg-gray-50-iz read-only:cursor-text` | |

Composición completa de la clase `.field-input` (lista para copy-paste):

```
h-11 w-full rounded-lg bg-muted border border-border px-4 text-base text-body
placeholder:text-disabled
transition-colors
hover:border-gray-400-iz
focus:outline-none focus:border-primary focus:ring-2 focus:ring-primary/15
disabled:bg-gray-100-iz disabled:text-disabled disabled:cursor-not-allowed
aria-invalid:border-destructive aria-invalid:focus:ring-destructive/15
```

#### 5.7.3 Variantes específicas

##### `<textarea>`

Mismo styling que input, sin `h-11` fijo: `min-h-[88px]` (2 líneas), `py-3`, `resize-y`.

##### `<select>` nativo

Igual que input pero con flecha custom:

```html
<select class="field-input pr-10 appearance-none bg-[url('data:image/svg+xml,...')] bg-no-repeat bg-[right_0.75rem_center] bg-[length:1.25em_1.25em]">
  <option>…</option>
</select>
```

La flecha es un SVG Tabler `ti-chevron-down` inline en data-URL con `fill="currentColor"`. Para componente shadcn `<Select>` esto se hace automáticamente.

##### Checkbox (`.checkbox`)

```html
<label class="flex items-center gap-2 cursor-pointer">
  <input type="checkbox" class="checkbox" />
  <span class="text-body">Acepto la política de privacidad</span>
</label>
```

Estilo:

| Estado | Tailwind | Visual |
|---|---|---|
| idle unchecked | `size-5 rounded border border-gray-400-iz bg-white cursor-pointer transition-colors` | 20 × 20 px, esquinas 4 px. |
| hover unchecked | `hover:border-primary` | |
| checked | `checked:bg-primary checked:border-primary` + ícono check blanco renderizado vía `background-image: url('data:image/svg+xml…check…')` | Fondo magenta + check blanco. |
| focus | `focus:ring-2 focus:ring-primary/15 focus:ring-offset-1` | Aura magenta. |
| disabled | `disabled:bg-gray-200-iz disabled:border-gray-300-iz disabled:cursor-not-allowed` | |

> Tamaño extraído del CSS oficial: 20 × 20 px, border-radius 4 px.

##### Radio (`.radio`)

```html
<div role="radiogroup" class="flex flex-col gap-2">
  <label class="flex items-center gap-2 cursor-pointer">
    <input type="radio" name="plan" class="radio" />
    <span class="text-body">Anual (ahorra 20%)</span>
  </label>
  <label class="flex items-center gap-2 cursor-pointer">
    <input type="radio" name="plan" class="radio" />
    <span class="text-body">Mensual</span>
  </label>
</div>
```

Estilo: igual que checkbox pero `rounded-full` y dot interior en lugar de check.

| Estado | Tailwind |
|---|---|
| idle | `size-5 rounded-full border border-gray-400-iz bg-white cursor-pointer` |
| checked | `checked:bg-primary checked:border-primary` + pseudo-elemento interior `size-2 rounded-full bg-white` |
| focus | `focus:ring-2 focus:ring-primary/15 focus:ring-offset-1` |

##### Switch (`.switch`)

Toggle on/off. Para shadcn usar el `<Switch>` de Radix; en HTML estático:

```html
<button role="switch" aria-checked="false" class="switch">
  <span class="switch-thumb"></span>
</button>
```

| Estado | Track | Thumb |
|---|---|---|
| off | `w-11 h-6 rounded-full bg-gray-300-iz transition-colors` | `block size-5 rounded-full bg-white translate-x-0.5` |
| on | `bg-primary` | `translate-x-5` |
| focus | `focus:ring-2 focus:ring-primary/15 focus:ring-offset-2` | |

#### 5.7.4 Form layout

- **Stack vertical**: `space-y-5` entre fields. Más aire que `space-y-4` para que labels y helpers respiren.
- **Grid 2 columnas**: `grid grid-cols-1 md:grid-cols-2 gap-5` para fields cortos (nombre+apellido, ciudad+CP). Mantener label arriba — no labels a la izquierda inline (peor escaneabilidad y peor mobile).
- **CTA del form**: alinear al final con `pt-4 flex justify-end gap-3`. Botón primary (submit) + ghost (cancelar). Si es un wizard, añadir `btn-ghost` "Atrás" a la izquierda.
- **Densidad**: para formularios muy largos (>10 campos) usar `space-y-4` y `text-sm` en labels para ganar verticales. Para formularios cortos críticos (login, sign-up) mantener defaults.

#### 5.7.5 Cuándo NO usar este sistema

- **Tablas editables inline**: usar inputs sin `bg-muted` (`bg-transparent`), sin border en idle, con border solo en focus. La fila ya da contexto.
- **Comandos / búsqueda global**: para `<input type="search">` con palette tipo cmd-k, usar componente shadcn `Command` con estilos por defecto.
- **OTP / códigos de verificación**: usar 6 inputs separados con styling propio (separación entre celdas, monospace). No reutilizar `.field-input`.

---

### 5.8 Badges, tags & avatars

Tres componentes pequeños que usan reglas comunes (forma compacta, color de categoría, texto pequeño) pero distinto significado.

#### 5.8.1 `badge` — etiqueta de estado o categoría

Pieza compacta y rectangular para marcar **estado** o **categoría** de un item. Es el StatusBadge canónico.

| Anatomía | Tailwind |
|---|---|
| contenedor | `inline-flex items-center gap-1 rounded-md px-2 py-0.5 text-xs font-medium` |
| punto opcional | `size-1.5 rounded-full bg-current` (anidado al inicio) |

Variantes por estado:

| Estado | Clase | Cuándo |
|---|---|---|
| `default` | `bg-muted text-subtle` | Neutral, "pending" |
| `info` | `bg-accent/15 text-accent` | "running", "in progress" |
| `success` | `bg-success/15 text-success` | "succeeded", "live", "ok" |
| `warning` | `bg-warning/15 text-warning` | "aborted", "stale" |
| `destructive` | `bg-destructive/15 text-destructive` | "failed", "error" |
| `brand` | `bg-primary/10 text-primary` | "premium", "nuevo" |

Variante con **punto pulsante** (estados activos en tiempo real):

```html
<span class="badge badge-info">
  <span class="size-1.5 rounded-full bg-current animate-pulse"></span>
  Sincronizando
</span>
```

#### 5.8.2 `tag` — etiqueta cerrable de selección

Pieza similar al badge pero **interactiva**: la usa el usuario para marcar selecciones (filtros activos, tags de un post, miembros invitados). Lleva una X para eliminar.

| Anatomía | Tailwind |
|---|---|
| contenedor | `inline-flex items-center gap-1.5 rounded-full bg-muted text-body px-3 py-1 text-sm` |
| close button | `<button class="ti ti-x text-soft hover:text-foreground" aria-label="Eliminar"></button>` |

Variante de color: heredar de `--accent-{producto}` o `--deco-*` si el tag representa categoría.

> **Badge vs tag**: si NO se puede eliminar al hacer click, es `badge`. Si SÍ, es `tag`.

#### 5.8.3 `avatar` — representación visual de persona/entidad

| Tamaño | Clase | Uso |
|---|---|---|
| `avatar-sm` | `size-8 text-xs` | Filas densas, comentarios. |
| `avatar-md` | `size-10 text-sm` | **Default**. Listas, autores. |
| `avatar-lg` | `size-12 text-base` | Quote cards (§5.4.5), perfiles. |
| `avatar-xl` | `size-16 text-lg` | Header de página de perfil. |

Anatomía base:

```html
<div class="avatar avatar-md">
  <img src="..." alt="Hugo Martínez" class="size-full object-cover rounded-full" />
</div>
```

Si no hay imagen, **fallback con iniciales**:

```html
<div class="avatar avatar-md bg-muted text-soft font-medium flex items-center justify-center rounded-full">
  HM
</div>
```

Las iniciales se calculan: 1ª letra del nombre + 1ª del apellido, mayúsculas. Para 1 sola palabra, primeras 2 letras.

**Avatar group (stack)** — para mostrar varios miembros agrupados:

```html
<div class="flex -space-x-2">
  <div class="avatar avatar-md ring-2 ring-background">…</div>
  <div class="avatar avatar-md ring-2 ring-background">…</div>
  <div class="avatar avatar-md ring-2 ring-background bg-muted text-soft text-xs">+5</div>
</div>
```

`-space-x-2` los solapa, `ring-2 ring-background` los separa visualmente con el color de fondo de la página.

### 5.9 Tooltips

Texto corto que aparece al hover (o foco) sobre un elemento. Ideal para botones icon-only o aclaraciones.

| Anatomía | Detalle |
|---|---|
| trigger | El elemento que dispara el tooltip. |
| content | Popup arriba/abajo/lado del trigger. |
| arrow opcional | Triángulo que apunta al trigger. |

**Implementación recomendada**: Radix `Tooltip` (incluido en shadcn). Para HTML estático, usar el atributo `title=""` solo para ayuda muy básica — no soporta texto rico ni estilo.

Clases del content (cuando se implementa custom):

```
z-30 max-w-xs rounded-md bg-foreground text-background text-xs px-3 py-1.5 shadow-md
data-[state=open]:animate-in data-[state=closed]:animate-out
data-[side=top]:slide-in-from-bottom-1
```

- **Color**: `bg-foreground text-background` (oscuro sobre claro), **no** `bg-primary`. El magenta queda fuerte en un tooltip pequeño.
- **z-index**: 30 (§4.5).
- **Duration**: 300 ms enter, 200 ms leave.
- **Delay**: 500 ms antes de mostrar (evita parpadeos al mover el ratón).

**Cuándo NO usar tooltip**:

- Para información esencial que el usuario debe leer sí o sí — usar `field-helper` (§5.7.1) o label expandido.
- En mobile (los tooltips no son hover-able). En mobile, sustituir por un icono `i` que abre un popover al tap.

### 5.10 Alerts & Toasts

Dos formas de notificar al usuario. **Alerts** son **inline** en el flujo del DOM y persisten; **toasts** son **flotantes** y efímeros (auto-dismiss).

#### 5.10.1 Alerts (inline)

Bloque persistente dentro del flujo de la página. Útil para advertencias sobre estado del sistema, errores de un formulario, anuncios de feature.

| Anatomía | Detalle |
|---|---|
| contenedor | `flex gap-3 rounded-lg border p-4` |
| icono | A la izquierda, tamaño 20 px, color del estado. |
| body | título opcional `text-sm font-semibold` + descripción `text-sm text-subtle`. |
| close opcional | `ti-x` arriba a la derecha. |

Variantes:

| Variante | Border | Background | Icon | Color título |
|---|---|---|---|---|
| `info` | `border-accent/30` | `bg-accent/5` | `ti-info-circle text-accent` | `text-accent` |
| `success` | `border-success/30` | `bg-success/5` | `ti-circle-check text-success` | `text-success` |
| `warning` | `border-warning/30` | `bg-warning/10` | `ti-alert-triangle text-warning` | `text-warning` |
| `destructive` | `border-destructive/30` | `bg-destructive/5` | `ti-circle-x text-destructive` | `text-destructive` |

Ejemplo:

```html
<div role="alert" class="alert alert-warning">
  <i class="ti ti-alert-triangle text-[20px] text-warning shrink-0 mt-0.5"></i>
  <div class="flex-1">
    <h4 class="text-sm font-semibold text-warning">Sincronización abortada</h4>
    <p class="text-sm text-subtle mt-1">El proceso lleva 10 minutos sin avanzar. Última actualización hace 5 min.</p>
  </div>
  <button aria-label="Cerrar" class="text-subtle hover:text-body"><i class="ti ti-x"></i></button>
</div>
```

#### 5.10.2 Toasts (flotantes)

Notificación temporal que aparece en la esquina y se autodescarta tras 4-6 segundos. Para feedback de acciones que el usuario acaba de ejecutar.

| Anatomía | Detalle |
|---|---|
| posición | `fixed bottom-4 right-4` (default) o `top-4 right-4`. **Una sola posición consistente por app**. |
| stack | Apilados verticalmente con `gap-2`. |
| z-index | 60 (§4.5). |
| contenedor | `flex items-start gap-3 rounded-lg border bg-card p-4 shadow-lg min-w-[320px] max-w-md` |

Variantes: mismas que alerts (info / success / warning / destructive).

Comportamiento:

- **Duración**: 4 s para info/success, 6 s para warning/destructive.
- **Action opcional**: link "Deshacer" / "Ver detalles" inline con el body, alineado a la derecha.
- **Auto-dismiss pausa en hover** (para que el usuario pueda leer).
- **Close manual** con `ti-x`.

**Implementación recomendada**: Sonner (incluido en shadcn) o Radix Toast. NO implementar a mano.

Ejemplo de uso (React + Sonner):

```tsx
toast.success("Sincronización completada", {
  description: "1.234 leads actualizados desde Salesforce.",
  action: { label: "Ver detalles", onClick: () => navigate("/syncs/latest") },
});
```

**Cuándo NO usar toast**:

- Para errores que requieren acción del usuario para continuar — usar modal o alert.
- Para notificaciones críticas que deben verse sí o sí — pueden pasar desapercibidas.
- Para confirmaciones de acciones destructivas — usar modal antes, toast después.

### 5.11 Modales / Dialogs

Overlay modal sobre el contenido principal. Bloquea la interacción con el resto hasta que el usuario actúe.

| Anatomía | Detalle |
|---|---|
| backdrop | `fixed inset-0 bg-black/50 z-40` |
| content | `fixed left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 z-50 w-full max-w-lg rounded-xl bg-card p-6 shadow-md` |
| header | título `t-h3` + close `ti-x` |
| body | contenido scrollable si excede |
| footer | CTAs alineados a la derecha (`flex justify-end gap-3`): cancelar (`btn-ghost`) + confirmar (`btn-primary`) |

Tamaños:

| Tamaño | max-width | Cuándo |
|---|---|---|
| `dialog-sm` | `max-w-sm` (384 px) | Confirmaciones simples sí/no. |
| `dialog-md` | `max-w-lg` (512 px) | **Default**. Formularios cortos. |
| `dialog-lg` | `max-w-2xl` (672 px) | Formularios largos, configuración. |
| `dialog-xl` | `max-w-4xl` (896 px) | Vistas de detalle, comparativas. |
| `dialog-full` | `max-w-[calc(100vw-4rem)] max-h-[calc(100vh-4rem)]` | Modo "edición a pantalla completa". |

Comportamiento:

- **Close** con: click en backdrop, `ESC`, click en `ti-x`, o action del footer.
- **Foco trap**: el foco no sale del modal mientras esté abierto (Radix Dialog lo hace automático).
- **Scroll lock**: el body de la página queda no-scrollable.
- **Mount animation**: backdrop `fade-in 200ms`, content `scale-in 200ms from 0.95`.
- **Altura natural, NUNCA estirada**: el dialog crece según su contenido. Su `max-h` es `calc(100vh - 4rem)` con `overflow-y: auto` en el body si excede. Si dos dialogs aparecen en un grid (caso playground), el grid contenedor debe llevar `items-start` para que cada uno respete su altura — nunca `items-stretch` (default del grid).
- **Footer siempre al fondo del contenido propio**, no del viewport. Si necesitas footer "sticky" dentro del dialog (formulario muy largo), envolver el body en `flex flex-col` y aplicar `max-h` + `overflow-y-auto` solo al body, dejando header y footer fuera del scroll.

#### Confirm dialog (variante destructiva)

Modal específico para confirmar acciones destructivas (borrar, descartar):

```html
<div class="dialog dialog-sm">
  <i class="ti ti-alert-triangle text-[40px] text-destructive mx-auto block mb-3"></i>
  <h2 class="t-h4 text-center mb-2">¿Eliminar esta sincronización?</h2>
  <p class="t-body-sm text-center mb-6">Esta acción no se puede deshacer. Los 1.234 leads asociados perderán la referencia.</p>
  <div class="flex gap-3">
    <button class="btn btn-md btn-ghost flex-1">Cancelar</button>
    <button class="btn btn-md btn-destructive flex-1">Sí, eliminar</button>
  </div>
</div>
```

- Icono Tabler `ti-alert-triangle` arriba, `text-destructive`.
- Texto centrado.
- Dos CTAs de igual peso (`flex-1`), destructive a la derecha (acción destructiva NO es el default).

**Implementación recomendada**: Radix Dialog (shadcn). Para HTML estático, las clases CSS están en §12.

**Cuándo NO usar modal**:

- Para flujos largos con varios pasos → usar wizard / página dedicada.
- Para listas o búsqueda → usar drawer o popover.
- Para notificaciones que no requieren acción → usar toast o alert.

### 5.12 Empty states

Pantalla / sección sin datos. Es el patrón más fácil de olvidar y el que más distingue una UI cuidada de una rota.

| Anatomía | Detalle |
|---|---|
| ilustración o icono | Centrado, 64-96 px. Icono Tabler outline en `text-soft` por defecto, o ilustración custom si el caso lo merece. |
| título | `t-h4 text-heading` — corto, descriptivo (ej. "Aún no hay sincronizaciones"). |
| descripción | `t-body-sm max-w-md` — explica QUÉ se vería aquí y POR QUÉ está vacío. |
| CTA primario | El siguiente paso (ej. "Lanzar primera sincronización"). |
| CTA secundario opcional | Link a docs (`btn-link`). |

Layout:

```html
<div class="empty-state">
  <i class="ti ti-database-off text-[80px] text-soft mb-4"></i>
  <h3 class="t-h4 mb-2">Aún no hay sincronizaciones</h3>
  <p class="t-body-sm text-subtle max-w-md mb-6">Las sincronizaciones aparecen aquí en cuanto lances la primera. Conecta Salesforce y empezarás a verlas.</p>
  <button class="btn btn-md btn-primary">Conectar Salesforce</button>
  <a href="/docs/sync" class="btn-link mt-3">Ver guía paso a paso</a>
</div>
```

Clases:

```
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: 4rem 2rem;
  gap: 0.5rem;
}
```

Variantes por contexto:

| Contexto | Variante |
|---|---|
| Tabla vacía | Empty state dentro de la fila ocupando full width: "No se encontraron resultados". Icono `ti-search-off` si es por filtros, `ti-database-off` si es por no haber datos aún. |
| Filtrado sin resultados | Texto: "No hay resultados para [criterio]." + CTA "Limpiar filtros". |
| Primera vez (onboarding) | Más generoso, con CTA primario destacado. |
| Error de carga | Texto explicando el error + CTA "Reintentar" (refresh). Ver §5.13 loading. |

**Cuándo NO usar empty state**:

- Listas con menos de 5 items donde "no hay items" es informativo pero no bloqueante — basta con texto inline.
- Cargas iniciales rápidas (<300 ms) — usar skeleton (§5.13), no empty state.

### 5.13 Loading states

Tres formas de comunicar "estamos cargando" según el contexto.

#### 5.13.1 Spinner (acción)

Para acciones del usuario que tardan unos segundos. Va dentro del trigger que disparó la acción.

```html
<button class="btn btn-md btn-primary" disabled>
  <i class="ti ti-loader-2 animate-spin text-base"></i>
  Sincronizando…
</button>
```

- Tabler `ti-loader-2` (o `ti-loader`) + `animate-spin`.
- El label del botón cambia a la acción en **gerundio** ("Sincronizando", "Guardando", "Enviando").
- El botón pasa a `disabled` para evitar dobles clicks.

Spinner standalone (centrado en un bloque vacío durante una espera corta):

```html
<div class="flex items-center justify-center py-12">
  <i class="ti ti-loader-2 animate-spin text-[40px] text-primary"></i>
</div>
```

#### 5.13.2 Skeleton (carga de datos en bloque)

Cuando se carga un bloque completo (tabla, lista, dashboard) y aún no hay datos, mostrar **placeholder gris animado** con la forma del contenido futuro.

Clase base:

```css
.skeleton {
  background: hsl(var(--gray-200));
  border-radius: var(--radius);
  animation: skeleton-pulse 1.5s ease-in-out infinite;
}
@keyframes skeleton-pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}
```

Uso (imitar la forma del contenido):

```html
<!-- card skeleton -->
<div class="rounded-lg border p-6 flex flex-col gap-3">
  <div class="skeleton size-10"></div>          <!-- icono -->
  <div class="skeleton h-5 w-3/4"></div>        <!-- título -->
  <div class="skeleton h-4 w-full"></div>       <!-- desc línea 1 -->
  <div class="skeleton h-4 w-5/6"></div>        <!-- desc línea 2 -->
  <div class="skeleton h-4 w-1/3 mt-auto"></div><!-- cta -->
</div>
```

**Regla**: el skeleton debe tener **aproximadamente las mismas dimensiones** que el contenido real. Si la tarjeta real es de 200 px de alto, el skeleton también — evita "salto de layout" al cargar.

**Cuándo usar skeleton vs spinner**:

- **Skeleton**: cuando vas a mostrar un bloque estructurado (tabla, lista, dashboard). El usuario ya ve la forma.
- **Spinner**: para una acción puntual o cuando la carga es muy corta y un skeleton sería overkill.

#### 5.13.3 Progress bar (progreso determinado o indeterminado)

Para tareas con progreso medible (subir archivo, ejecutar batch) o tareas largas sin ETA (sincronización).

| Tipo | Cuándo |
|---|---|
| `progress-determinate` | Conoces el % completado. |
| `progress-indeterminate` | No conoces el ETA, solo sabes que está corriendo. |

Determinada (con `<progress>` semántico):

```html
<progress class="progress" max="100" value="42"></progress>
<span class="text-xs text-subtle tabular-nums">42% · 320 / 760 leads procesados</span>
```

Clase base:

```css
.progress {
  appearance: none;
  width: 100%;
  height: 0.5rem;
  border-radius: 9999px;
  background: hsl(var(--muted));
  overflow: hidden;
}
.progress::-webkit-progress-bar { background: hsl(var(--muted)); border-radius: 9999px; }
.progress::-webkit-progress-value { background: hsl(var(--primary)); border-radius: 9999px; transition: width 200ms; }
.progress::-moz-progress-bar { background: hsl(var(--primary)); border-radius: 9999px; }
```

Indeterminada (sin valor conocido, barra moviéndose):

```html
<div class="progress-indeterminate" role="progressbar" aria-label="Procesando"></div>
```

```css
.progress-indeterminate {
  position: relative;
  width: 100%;
  height: 0.5rem;
  background: hsl(var(--muted));
  border-radius: 9999px;
  overflow: hidden;
}
.progress-indeterminate::after {
  content: "";
  position: absolute;
  inset: 0;
  width: 40%;
  background: hsl(var(--primary));
  border-radius: 9999px;
  animation: progress-slide 1.5s ease-in-out infinite;
}
@keyframes progress-slide {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(250%); }
}
```

### 5.14 Tables & Pagination

Para datos tabulares densos. Para listas con cards visuales, usar `category-card` o `media-card` (§5.4). La tabla es para escaneabilidad fila a fila.

#### 5.14.1 Estructura base

```html
<div class="overflow-x-auto rounded-lg border">
  <table class="table">
    <thead>
      <tr>
        <th>Lead</th>
        <th>País</th>
        <th>Stage</th>
        <th class="text-right">Pipeline</th>
        <th class="text-right">Última actualización</th>
        <th class="sr-only">Acciones</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>
          <div class="font-medium text-body">Acme Corp</div>
          <div class="text-xs text-soft">contacto@acme.com</div>
        </td>
        <td><span class="badge badge-default">ES</span></td>
        <td><span class="badge badge-info">Abiertas</span></td>
        <td class="tabular-nums text-right">12.500 €</td>
        <td class="text-right text-subtle">hace 2h</td>
        <td><button aria-label="Acciones" class="btn-ghost btn-sm"><i class="ti ti-dots-vertical"></i></button></td>
      </tr>
      <!-- … más filas … -->
    </tbody>
  </table>
</div>
```

#### 5.14.2 Clases base

```css
.table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.875rem;
}

.table thead {
  background: hsl(var(--gray-50));
  border-bottom: 1px solid hsl(var(--border));
}

.table th {
  text-align: left;
  font-weight: 500;
  color: hsl(var(--text-subtle));
  text-transform: uppercase;
  letter-spacing: 0.025em;
  font-size: 0.75rem;
  padding: 0.75rem 1rem;
  white-space: nowrap;
}

.table th[aria-sort]::after {
  content: "";
  display: inline-block;
  margin-left: 0.25rem;
  /* SVG arrow inline indicando sort direction */
}

.table td {
  padding: 0.875rem 1rem;
  border-bottom: 1px solid hsl(var(--border));
  color: hsl(var(--text-body));
}

.table tbody tr:hover { background: hsl(var(--gray-50)); }
.table tbody tr[aria-selected="true"] { background: hsl(var(--brand-magenta-soft)); }
.table tbody tr:last-child td { border-bottom: none; }
```

Modificadores:

- `.table-sticky` → `thead` con `position: sticky; top: 0; z-index: 10;`. Útil en tablas largas con header siempre visible.
- `.table-compact` → padding `0.5rem 0.75rem`, font-size `0.8125rem`. Para tablas con >20 filas visibles.
- `.table-selectable` → primera columna `<th>`/`<td>` con checkbox para selección múltiple.

Reglas:

- **Números a la derecha** siempre (`text-right`) y `tabular-nums` siempre (§3.2).
- **Acciones a la derecha** en la última columna (dropdown `ti-dots-vertical`).
- **Truncar texto largo** con `truncate max-w-{n}` en celdas problemáticas, no envolver.
- **Sort en headers**: `<th aria-sort="ascending|descending|none">`. Toda columna sortable es un `<button>` dentro del `<th>` para que sea focusable.

#### 5.14.3 Pagination

| Anatomía | Detalle |
|---|---|
| contenedor | `flex items-center justify-between gap-4 py-3` |
| info de rango | `text-sm text-subtle` — "Mostrando 1-10 de 247" |
| controles | grupo de buttons numerados + prev/next |

Patrón canónico:

```html
<nav class="pagination" aria-label="Paginación">
  <p class="text-sm text-subtle">Mostrando <span class="tabular-nums">1-10</span> de <span class="tabular-nums">247</span></p>

  <div class="flex items-center gap-1">
    <button class="btn btn-sm btn-ghost" disabled aria-label="Página anterior">
      <i class="ti ti-chevron-left"></i>
    </button>
    <button class="btn btn-sm btn-primary tabular-nums" aria-current="page">1</button>
    <button class="btn btn-sm btn-ghost tabular-nums">2</button>
    <button class="btn btn-sm btn-ghost tabular-nums">3</button>
    <span class="text-subtle px-2">…</span>
    <button class="btn btn-sm btn-ghost tabular-nums">25</button>
    <button class="btn btn-sm btn-ghost" aria-label="Página siguiente">
      <i class="ti ti-chevron-right"></i>
    </button>
  </div>
</nav>
```

- **Página actual**: `btn-primary` (sólido magenta), resto `btn-ghost`.
- **Elipsis** cuando se omiten páginas — `…` plano, no clickable.
- **Mobile**: ocultar números intermedios, dejar solo prev / actual / next.
- **Cantidad por página**: si la tabla la expone, va a la izquierda como `<select>` ("10 por página").

**Cuándo NO usar pagination**:

- Listas con <50 items totales — mostrar todo o usar scroll virtual.
- Datos con scroll infinito esperado (timeline, feed) — usar "Cargar más" o infinite scroll.

---

### 5.15 Dropdown menus

Lista de acciones contextual disparada por un trigger (normalmente botón con `ti-dots-vertical` o un caret). A diferencia del `tab-pills`/`filter-toolbar` que muestran opciones siempre, el dropdown está colapsado por defecto.

| Anatomía | Detalle |
|---|---|
| trigger | botón (icon-only o con caret `ti-chevron-down`) |
| menu | `absolute z-20 min-w-[12rem] mt-1 rounded-md border bg-card p-1 shadow-md` |
| item | `flex items-center gap-2 px-3 py-2 text-sm text-body rounded-sm cursor-pointer hover:bg-muted` |
| item con icono | mismo + `<i class="ti ti-X text-base text-soft">` al inicio |
| item destructive | `text-destructive hover:bg-destructive/10` |
| separator | `<div class="my-1 h-px bg-border">` entre grupos |
| label de grupo | `px-3 py-1.5 text-xs uppercase tracking-wide text-soft` |
| shortcut keyboard | `<kbd class="ml-auto text-xs text-soft font-mono">⌘K</kbd>` al final del item |

Ejemplo (HTML estático con `<details>` para que funcione sin JS — en React usar Radix `DropdownMenu`):

```html
<details class="dropdown relative">
  <summary class="btn btn-sm btn-ghost size-9 p-0 list-none cursor-pointer">
    <i class="ti ti-dots-vertical text-base"></i>
  </summary>
  <div class="dropdown-menu">
    <div class="dropdown-label">Acciones</div>
    <button class="dropdown-item"><i class="ti ti-edit text-base text-soft"></i> Editar</button>
    <button class="dropdown-item"><i class="ti ti-copy text-base text-soft"></i> Duplicar</button>
    <div class="dropdown-separator"></div>
    <button class="dropdown-item dropdown-item-destructive"><i class="ti ti-trash text-base"></i> Eliminar</button>
  </div>
</details>
```

Comportamiento:

- **Close** con: click en item, `ESC`, click fuera del menu.
- **Posicionamiento**: por defecto debajo-izquierda del trigger. Si el menu se sale del viewport, flip a arriba o cambiar lado. Radix lo hace automático; en CSS estático, usar `top: 100%; right: 0;` para anclar a la esquina superior derecha del trigger.
- **Foco con teclado**: ↑↓ navega items, `Enter` ejecuta, `ESC` cierra. Radix lo hace automático.
- **Sub-menús**: `ti-chevron-right` al final del item padre. El sub-menu se abre al lado del item, no debajo.

**Implementación recomendada**: Radix `DropdownMenu` (shadcn lo expone). Para HTML estático, las clases CSS están en §12.

**Cuándo NO usar dropdown**:

- Cuando las opciones son **siempre visibles** (radio, tabs, segmented control).
- Para >12 items (usar Combobox con búsqueda).
- Para selección de un valor en formulario (usar `<select>` nativo o Combobox).

### 5.16 Popovers

Contenedor flotante anclado a un trigger, con contenido rico (texto largo, controles, mini-formularios). A medio camino entre tooltip y modal: no bloquea la página, pero es interactivo.

| Anatomía | Detalle |
|---|---|
| trigger | cualquier elemento (button, link, icon) |
| content | `absolute z-20 w-72 mt-1 rounded-lg border bg-card p-4 shadow-md` |
| arrow opcional | triángulo apuntando al trigger (Radix lo dibuja en SVG) |

Diferencias rápidas:

| | Tooltip | Popover | Modal |
|---|---|---|---|
| Trigger | hover/focus | click | click |
| Contenido | texto corto | rico (form, mini-controles) | rico, bloquea |
| Cierra al | mouseleave | click fuera, ESC | botón explícito, ESC, click backdrop |
| Bloquea página | no | no | sí (foco trap, scroll lock) |

Ejemplo (filtros avanzados anclados a un botón):

```html
<button class="btn btn-md btn-outline">
  <i class="ti ti-filter text-base"></i> Filtros
</button>
<div class="popover" data-state="open">
  <h4 class="t-h4 mb-3">Filtrar leads</h4>
  <div class="field mb-3">
    <label class="field-label">País</label>
    <select class="field-input">…</select>
  </div>
  <div class="field mb-4">
    <label class="field-label">Stage</label>
    <select class="field-input">…</select>
  </div>
  <div class="flex justify-end gap-2">
    <button class="btn btn-sm btn-ghost">Limpiar</button>
    <button class="btn btn-sm btn-primary">Aplicar</button>
  </div>
</div>
```

**Implementación recomendada**: Radix `Popover` (shadcn). NO usar `<details>` aquí — no soporta click-outside ni posicionamiento inteligente.

**Cuándo NO usar popover**:

- Para acciones simples sin sub-controles → dropdown.
- Para formularios largos (>5 campos) → modal.
- Para ayuda contextual de una sola línea → tooltip.

### 5.17 Drawer / Sheet

Panel que entra desde un borde del viewport (lado, arriba, abajo). Útil para filtros complejos, configuración, navegación mobile. Es como un modal pero **lateral**, mantiene más contexto de la pantalla visible.

| Anatomía | Detalle |
|---|---|
| backdrop | `fixed inset-0 bg-black/50 z-40` |
| panel | `fixed top-0 right-0 h-screen w-full max-w-md bg-card z-50 shadow-lg flex flex-col` |
| header | `flex items-center justify-between p-6 border-b border-border` con título + close `ti-x` |
| body | `flex-1 overflow-y-auto p-6` |
| footer | `flex justify-end gap-3 p-6 border-t border-border` con CTAs |

Lados (`side`):

| Side | Clase posición | Animación enter |
|---|---|---|
| `right` (default) | `right-0 top-0 h-screen` | `slide-in-from-right` |
| `left` | `left-0 top-0 h-screen` | `slide-in-from-left` |
| `top` | `top-0 left-0 w-screen h-auto max-h-[80vh]` | `slide-in-from-top` |
| `bottom` | `bottom-0 left-0 w-screen h-auto max-h-[80vh]` | `slide-in-from-bottom` |

Tamaños (`max-w` para left/right, `max-h` para top/bottom):

- `drawer-sm` — 320 px (`max-w-sm`)
- `drawer-md` — 448 px (`max-w-md`) — **default**
- `drawer-lg` — 640 px (`max-w-2xl`)
- `drawer-xl` — 896 px (`max-w-4xl`)

**Implementación recomendada**: Radix `Dialog` con `data-side` (shadcn `Sheet`). Mantiene foco trap y scroll lock como un modal.

**Cuándo NO usar drawer**:

- Para confirmaciones simples sí/no → modal `dialog-sm`.
- Para acciones contextuales de un item → dropdown.
- En desktop ancho, evitar drawer si la información cabría en un sidebar permanente.

### 5.18 Tabs (internas)

Selector tipo "segmented control" para alternar vistas dentro de una card o panel. **Distinto del `tab-pills` (§5.4.2)**, que es marketing-y (underline grande, span de página). Las tabs internas son compactas y operativas.

| Anatomía | Detalle |
|---|---|
| tablist | `inline-flex items-center rounded-lg bg-muted p-1 gap-1` |
| tab | `px-3 py-1.5 rounded-md text-sm font-medium text-subtle hover:text-body transition-colors` |
| tab activa | `bg-card text-body shadow-sm` |
| tabpanel | `mt-4` con el contenido |

Ejemplo:

```html
<div role="tablist" class="tabs-segmented">
  <button role="tab" aria-selected="true" class="tab tab-active">Tabla</button>
  <button role="tab" class="tab">Gráfico</button>
  <button role="tab" class="tab">Mapa</button>
</div>
<div role="tabpanel" class="mt-4">
  <!-- contenido -->
</div>
```

Variantes:

- **`tabs-segmented`** (default arriba) — fondo gris pill, activa con elevation. Para vistas alternativas del mismo dato.
- **`tabs-underline`** — sin fondo, item activo con `border-b-2 border-primary`. Para sub-secciones dentro de una página (distinto de `tab-pills` porque es compacto, sin iconos grandes).

Decisión rápida:

| Caso | Patrón |
|---|---|
| Marketing/landing, seleccionar producto | `tab-pills` (§5.4.2) |
| Cambiar vista dentro de panel operativo | `tabs-segmented` |
| Sub-secciones de una página interna | `tabs-underline` |

**Cuándo NO usar tabs**:

- Para >6 opciones → dropdown o sidebar.
- Si las opciones no son alternativas mutuamente excluyentes — usar checkboxes o filter-toolbar.

### 5.19 Steppers / Wizards

Multi-step para formularios largos o flujos guiados. Muestra progreso y permite a veces volver atrás.

| Anatomía | Detalle |
|---|---|
| contenedor | `flex items-center gap-2` (horizontal) o `flex-col gap-4` (vertical) |
| step | item con: indicador numérico + label opcional + connector |
| indicador | círculo 32 px con número, ✓ (completado) o color |
| connector | línea 1 px entre steps |

Estados de cada step:

| Estado | Indicador | Label | Connector hacia siguiente |
|---|---|---|---|
| `complete` | `bg-success text-white` + icono `ti-check` | `text-body font-medium` | `bg-success` |
| `current` | `bg-primary text-white` + número | `text-heading font-semibold` | `bg-border` |
| `upcoming` | `bg-muted text-soft` + número | `text-soft` | `bg-border` |

Ejemplo (horizontal):

```html
<ol class="stepper">
  <li class="step step-complete">
    <div class="step-indicator"><i class="ti ti-check"></i></div>
    <div class="step-label">Cuenta</div>
  </li>
  <li class="step-connector step-connector-complete"></li>
  <li class="step step-current">
    <div class="step-indicator">2</div>
    <div class="step-label">Detalles</div>
  </li>
  <li class="step-connector"></li>
  <li class="step step-upcoming">
    <div class="step-indicator">3</div>
    <div class="step-label">Confirmación</div>
  </li>
</ol>
```

Comportamiento:

- **Clickable**: solo los steps `complete` (volver atrás) y `current`. Los `upcoming` no son interactivos.
- **Validación**: avanzar al siguiente step solo si el actual es válido. Si no, mostrar errores inline.
- **Navegación CTAs**: footer del wizard con `btn-ghost "Atrás"` (izquierda) + `btn-primary "Siguiente"` o `"Finalizar"` (derecha).

**Cuándo NO usar stepper**:

- Form con ≤4 fields que cabe en una sola pantalla — un solo form, sin steps.
- Flujo sin orden estricto — usar tabs o navegación libre.

### 5.20 Date picker

Selector de fecha (single date, range, o multi). Calendario popover anclado a un input o botón.

| Anatomía | Detalle |
|---|---|
| trigger | input `.field-input` o button con `ti-calendar` |
| popover | calendario en popover (§5.16) |
| header | mes + año + flechas `ti-chevron-left/right` + dropdowns selección rápida |
| grid | 7 columnas (días de semana) × 6 filas máximo |
| day cell | `size-9 rounded-md text-sm` |

Estados de la celda de día:

| Estado | Estilo |
|---|---|
| `default` | `hover:bg-muted text-body` |
| `today` | `text-primary font-semibold` (no fondo, solo texto) |
| `selected` | `bg-primary text-white font-medium` |
| `range-middle` | `bg-magenta-soft text-primary` (solo en range mode) |
| `disabled` | `text-disabled cursor-not-allowed` |
| `outside-month` | `text-soft opacity-50` (días del mes anterior/siguiente visibles) |

Modos:

- **`single`** (default) — una sola fecha.
- **`range`** — fecha inicio + fecha fin, se highlight el rango intermedio en `bg-magenta-soft`.
- **`multiple`** — varias fechas no contiguas seleccionables.

Footer opcional con presets:

```html
<div class="flex gap-1 p-2 border-t border-border">
  <button class="btn btn-sm btn-ghost">Hoy</button>
  <button class="btn btn-sm btn-ghost">Últimos 7 días</button>
  <button class="btn btn-sm btn-ghost">Este mes</button>
</div>
```

**Implementación recomendada**: `react-day-picker` (shadcn `Calendar` y `DatePicker` ya lo usan). Locale `es-ES` para que la semana empiece en lunes y los nombres de mes vengan en español.

**Cuándo NO usar date picker**:

- Para fechas concretas conocidas (DOB) — usar 3 selects (día/mes/año) o input mask `<input type="date">`.
- Para horas — usar time picker separado o combinar con datetime picker.

### 5.21 Combobox / Autocomplete

Input de selección con búsqueda + dropdown de sugerencias. Para listas grandes (>12 items) donde un `<select>` nativo se queda corto.

| Anatomía | Detalle |
|---|---|
| input | `.field-input` con icono `ti-search` o `ti-chevron-down` a la derecha |
| popover | dropdown bajo el input con sugerencias filtradas en tiempo real |
| empty state | "No se encontraron resultados para [query]" |
| selected | en input, mostrar valor seleccionado; con `ti-x` para borrar |

Variantes:

- **Single-select** — un solo valor. Al seleccionar cierra el popover y rellena el input.
- **Multi-select** — varios valores. Los seleccionados se muestran como `tags` (§5.8.2) dentro del input, cerrables uno a uno.
- **Async** — los resultados se cargan desde un backend con debounce de 300ms. Mostrar spinner cuando carga.

Comportamiento clave:

- **Filtrado en cliente** si la lista es <500 items.
- **Foco con teclado**: ↑↓ navega resultados, `Enter` selecciona, `ESC` cierra, `⌘K` (configurable) abre.
- **Highlight de match**: la subcadena que coincide con el query va en `<mark>` o `<span class="text-primary font-medium">`.

**Implementación recomendada**: `cmdk` (Radix) — base de shadcn `Command` y `Combobox`.

**Cuándo NO usar combobox**:

- ≤6 items conocidos → `<select>` nativo o segmented control.
- Cuando la búsqueda devuelve más de "una opción" (artículos, leads, productos) — eso es un **search global**, no un combobox.

### 5.22 Breadcrumbs

Navegación jerárquica horizontal — muestra dónde estoy en la estructura del sitio/app.

| Anatomía | Detalle |
|---|---|
| nav | `<nav aria-label="Breadcrumb">` con `<ol class="flex items-center gap-1.5 text-sm">` |
| item | `<li>` con link `text-soft hover:text-link` |
| separator | `ti-chevron-right text-soft text-sm` entre items |
| current page | último item, **NO link**, `text-body font-medium` |

Ejemplo:

```html
<nav aria-label="Breadcrumb">
  <ol class="flex items-center gap-1.5 text-sm">
    <li><a href="/" class="text-soft hover:text-link">Inicio</a></li>
    <li><i class="ti ti-chevron-right text-soft text-sm" aria-hidden="true"></i></li>
    <li><a href="/sync" class="text-soft hover:text-link">Sincronizaciones</a></li>
    <li><i class="ti ti-chevron-right text-soft text-sm" aria-hidden="true"></i></li>
    <li><span class="text-body font-medium" aria-current="page">#1234</span></li>
  </ol>
</nav>
```

Variantes:

- **Truncado**: cuando la jerarquía es muy profunda (>5 niveles), colapsar los del medio con `…` que abre un dropdown con los intermedios.
- **Sin home explícito**: si la página de inicio es obvia y el primer crumb sería redundante, omitirlo (decisión a nivel de app, no por pantalla).

**Cuándo NO usar breadcrumbs**:

- Apps planas con <2 niveles de profundidad — no aportan info.
- Cuando el usuario llegó por search/link directo y la jerarquía no es relevante a su flujo.
- Como sustituto de back button — son navegación posicional, no histórica.

### 5.23 Slider

Control de rango con thumb arrastrable. Para valores en un rango continuo donde es más intuitivo "arrastrar" que escribir un número.

| Anatomía | Detalle |
|---|---|
| track | `relative h-1.5 w-full rounded-full bg-muted` |
| range (parte rellena) | `absolute h-full rounded-full bg-primary` |
| thumb | `absolute size-5 rounded-full bg-card border-2 border-primary shadow-sm -translate-y-1/2 top-1/2` |
| label opcional | encima del thumb, muestra el valor actual |

Variantes:

- **Single** — un solo thumb, valor entre min y max.
- **Range** — dos thumbs, rango entre dos valores (filtro de precio, fecha…).
- **Steps** — marcadores visuales en valores discretos (`bg-border` puntos sobre el track).

Estados del thumb:

| Estado | Estilo |
|---|---|
| idle | borde primary, fondo card |
| hover | añade `cursor-grab` |
| dragging | `cursor-grabbing` + label visible mostrando valor |
| focus-visible | `ring-2 ring-primary/40` |
| disabled | `border-disabled bg-muted cursor-not-allowed` |

Ejemplo con field group:

```html
<div class="field">
  <div class="flex justify-between">
    <label class="field-label">Días de retención</label>
    <span class="text-sm tabular-nums text-subtle">90 días</span>
  </div>
  <input type="range" class="slider" min="7" max="365" value="90" />
  <div class="flex justify-between text-xs text-soft mt-1">
    <span>7</span>
    <span>365</span>
  </div>
</div>
```

**Implementación recomendada**: Radix `Slider` (shadcn). Soporta single y range con la misma API.

**Cuándo NO usar slider**:

- Valores discretos pequeños (<6) → segmented control o radio.
- Valores precisos que el usuario sabe (ej: edad exacta) → `<input type="number">`.
- Rangos muy amplios (1 a 1.000.000) — la precisión por arrastre es imposible. Combinar con input numérico vinculado.

### 5.24 File upload

Zona para subir archivos por drag-and-drop o selección manual.

| Anatomía | Detalle |
|---|---|
| drop zone | `border-2 border-dashed border-border rounded-lg p-8 text-center cursor-pointer hover:border-primary hover:bg-muted/40 transition-colors` |
| icono | `ti-cloud-upload text-[48px] text-soft mb-3` |
| título | `t-h4 mb-1` — "Arrastra archivos aquí" |
| sub | `t-body-sm` — "o haz click para seleccionar · Max 10 MB · PDF, DOCX, XLSX" |
| input oculto | `<input type="file" class="sr-only">` que el wrapper proxy-clickea |
| file list | lista de archivos subidos debajo del drop zone |

Estados del drop zone:

| Estado | Estilo |
|---|---|
| idle | borde dashed `border-border` |
| hover | `border-primary bg-muted/40` |
| drag-over (file siendo arrastrado encima) | `border-primary border-solid bg-magenta-soft` |
| error | `border-destructive bg-destructive/5` con mensaje inline |

Item de la file list:

```html
<div class="file-item">
  <i class="ti ti-file-text text-[24px] text-soft"></i>
  <div class="flex-1 min-w-0">
    <div class="text-sm font-medium truncate">leads-q1-2026.xlsx</div>
    <div class="text-xs text-soft tabular-nums">2.4 MB · subiendo 78%</div>
    <progress class="progress mt-1" max="100" value="78"></progress>
  </div>
  <button aria-label="Cancelar" class="btn-ghost btn-sm size-8 p-0"><i class="ti ti-x text-base"></i></button>
</div>
```

Tres estados por archivo: `uploading` (con progress), `success` (icono `ti-check text-success`), `error` (icono `ti-alert-circle text-destructive` + mensaje).

**Implementación recomendada**: nativo `<input type="file" multiple>` + librería de upload con XHR/fetch (axios, ky). Para validación cliente (tamaño, tipo, número), check en el `onChange`.

**Cuándo NO usar drop zone**:

- Para subir **una sola imagen** (avatar, logo) — botón "Cambiar imagen" + thumb directo, no necesita zona grande.
- En mobile donde drag-and-drop no aplica — fallback a botón "Seleccionar archivo" prominente.

---

### 5.25 Tool shell

Wrapper visual obligatorio para **toda tool isEazy**. Garantiza que cualquier herramienta del equipo se reconoce de un vistazo como isEazy: mismo header con logo, mismo toggle dark/light, misma estructura de scroll. Es el "uniforme" que une todas las tools entre sí.

> **Implementación de referencia en vivo**: el propio playground de este repo usa el `tool-shell` aplicado. Header con logo + título + toggle dark/light + sidebar opcional. Para ver cómo se ve, abre [`playground/index.html`](playground/index.html) — el shell es lo que envuelve todo lo que ves.

#### 5.25.1 Anatomía

```
┌─────────────────────────────────────────────────────────────┐
│  [iseazy logo] · [Nombre tool]                  [🌗] [user▾]│  ← header (sticky, h-16)
├─────────────────────────────────────────────────────────────┤
│ [sidebar       │                                            │
│  opcional]     │           main content                     │
│                │           (área scrolleable)               │
│                │                                            │
└─────────────────────────────────────────────────────────────┘
```

| Parte | Detalle |
|---|---|
| **Header** | Sticky top, `h-16`, `border-b border-border`. Posición fija incluso al scrollear. |
| **Logo isEazy** | Variante color en light, `-white.svg` en dark (patrón dual `block dark:hidden` / `hidden dark:block`). Tamaño `h-7 w-auto`. |
| **Nombre tool** | A la derecha del logo, separado por `border-l border-border pl-3`. Estilo `text-xs text-subtle` (discreto — el logo manda). |
| **Toggle dark/light** | Botón `btn-sm btn-ghost size-9 p-0` con icono `ti-moon-stars` (light) / `ti-sun` (dark). Aplica `class="dark"` a `<html>` y persiste en `localStorage` con key `iseazy-ds-theme`. |
| **User menu** (opcional) | Avatar + dropdown a la derecha del toggle si la tool tiene multi-usuario. Si no, se omite. |
| **Sidebar** (opcional) | Solo si la tool tiene >4 secciones principales y requiere navegación persistente. Por defecto NO se incluye sidebar — la mayoría de tools internas son single-page. Ver el playground si necesitas sidebar como referencia. |
| **Main** | `min-w-0` para evitar overflow en grids, padding canónico (`px-4 md:px-8 pb-32`). |

#### 5.25.2 Comportamiento

- **Dark mode persistente**: al alternar el toggle, se guarda en `localStorage` (`iseazy-ds-theme`). En sesiones futuras del usuario, se restaura automáticamente.
- **Foco visible**: el toggle (y cualquier botón del header) respeta §4.7 (focus-visible con `ring-2 ring-primary`).
- **Mobile**: si el header se queda corto, el nombre de la tool puede ocultarse con `hidden sm:inline` dejando solo el logo + toggle.

#### 5.25.3 Implementación (HTML/Tailwind)

```html
<header class="border-b border-border bg-background sticky top-0 z-50">
  <div class="px-6 h-16 flex items-center justify-between">
    <div class="flex items-center gap-3">
      <img src="https://raw.githubusercontent.com/factory-iseazy/iseazy-design-system/main/assets/logos/iseazy/iseazy.svg"
           alt="isEazy" class="h-7 w-auto block dark:hidden" />
      <img src="https://raw.githubusercontent.com/factory-iseazy/iseazy-design-system/main/assets/logos/iseazy/iseazy-white.svg"
           alt="isEazy" class="h-7 w-auto hidden dark:block" />
      <span class="text-xs text-subtle border-l border-border pl-3 hidden sm:inline">[NOMBRE TOOL]</span>
    </div>
    <button id="theme-toggle" class="btn btn-sm btn-ghost size-9 p-0" aria-label="Cambiar tema">
      <i class="ti ti-moon-stars text-base"></i>
    </button>
  </div>
</header>

<main class="container mx-auto px-4 md:px-8 pb-32">
  <!-- contenido de la tool -->
</main>
```

Más el JS del toggle (idéntico al playground — ver `playground/index.html` al final del `<body>`).

#### 5.25.4 Reglas

1. **TODA pantalla de una tool isEazy debe estar envuelta en este shell.** No renderizar contenido fuera del header.
2. **El logo NO se cambia** por el logo de la tool. La identidad de la tool va en el texto al lado del logo.
3. **El toggle dark/light NO es opcional** — toda tool lo soporta. Aunque la tool sea de marketing-y prefiera default light, el toggle debe existir.
4. **No añadir más elementos al header** sin justificación: queda denso y rompe la consistencia entre tools.

#### 5.25.5 Cuándo NO usar el shell

- Páginas standalone fuera del flujo principal: login screens, splash, 404, error pages, embed widgets. Estas pueden romper la regla del shell si lo justifica el contexto.
- Modales o vistas full-screen *dentro* de la tool (estos sí están envueltos por el shell de la página detrás).

---

## 6. Datos: nomenclatura canónica

Los siguientes valores **se renderizan tal cual**, sin reformatear. Vienen de la BBDD canónica (`docs/data-schema.md`) y de `templates/iseazy-tool-starter/CLAUDE.md`. Cambiarlos en la UI rompe la trazabilidad con el dato fuente.

### 6.1 Países (`countries.code` → label UI)

| Code | Label |
|---|---|
| `ES` | Spain |
| `US` | USA |
| `MX` | México |
| `BR` | Brazil |
| `CO` | Colombia |
| `GB` | UK |
| `ROW` | Rest of World |
| `ALL` | Todos los países |

Definido en `CountryDashboardPage.tsx:8-17`.

### 6.2 Product lines (`product_lines.code` → label UI)

Familia de productos isEazy. Códigos canónicos en BBDD:

| Code | Label UI sugerido |
|---|---|
| `author` | isEazy Author |
| `factory` | isEazy Factory |
| `skills` | isEazy Skills |
| `lms` | isEazy LMS |
| `engage` | isEazy Engage |
| `game` | isEazy Game |
| `esg` | isEazy ESG |
| `bizpills` | isEazy BizPills |
| `all_in_one` | All-in-one |

Taglines oficiales (cuando se necesite expandir en tooltip o landing):

- Author: *La herramienta de autor con IA nº1 del mercado*
- Factory: *Factoría de contenidos e-learning a medida*
- Skills: *Catálogo de cursos de power skills*
- LMS: *La plataforma de e-learning todo en uno*
- Engage: *App para potenciar la productividad de tus frontline workers*
- Game: *App de gamificación para empresas*

(Verbatim de iseazy.com/es/. Mantener tildes y mayúsculas iniciales como ahí están.)

### 6.3 Stages de oportunidad

`opp_stages.stage_group ∈ { open, won, lost, rejected }`. Mapeo:

| Group | Label UI | Color |
|---|---|---|
| `open` | Abiertas | accent (azul) |
| `won` | Ganadas | success |
| `lost` | Perdidas | destructive |
| `rejected` | Rechazadas | muted |

### 6.4 Formato numérico

- Locale: `es-ES` (`toLocaleString("es-ES")`). Separador de miles `.`, decimal `,`.
- Importes: euros, sin decimales (`maximumFractionDigits: 0`). Símbolo `€` después del número en eje/leyenda; en KPI grandes se rotula la columna como "Importe ganado (€)".
- Ceros: renderizar como `—` (em-dash) cuando el valor es 0 *y* significa "no aplica". Si 0 es un dato real (p. ej. 0 MQLs ese mes), mostrar `0`.
- Fechas relativas: regla del §1.2 (`hace 5 min`, `hace 2h`, `hace 3d`).
- Fechas absolutas: `dd MMM yyyy` en español (`14 may 2026`). Nunca formato US.

---

## 7. Accesibilidad mínima

- Contraste AA sobre todos los pares definidos: `#EB1C79` sobre blanco = 4.6:1 (pasa). `#3B189B` sobre blanco = 10.5:1 (sobra). `#333` sobre blanco = 12.6:1.
- Foco visible siempre (`--ring` magenta). No usar `outline: none` sin reemplazo.
- `aria-label` obligatorio en botones-icono (`lucide-react` sin texto).
- Tablas KPI: `<th scope="col">` y `tabular-nums` para alinear.

---

## 8. Cómo consumir este documento

**Como humano** (Hugo, compañeros):

- Cuando arranques una pantalla nueva: lee §1.2 (voz), §3.2 (escala), §5.1 (componentes base) y **§5.4 (patrones de bloque)**. Lee el **meta-principio del §5.4** antes de elegir patrón: los nombres son genéricos a propósito y un patrón sirve para muchos casos de uso. Identifica el shape del contenido, encuentra el patrón cuyo shape coincide, copia su anatomía. Si nada encaja, parar antes de inventar.
- Cuando dudes si un color tiene significado o es adorno: §2.1 (brand, con significado) vs §2.4 (decorative, sin significado fijo). Si no está en §2, no se usa.
- Cuando una card "se vea sosa": probablemente le falta el color de categoría del producto (§2.3) o un accent decorativo (§2.4), o estás cayendo en un anti-patrón (§5.5).

**Como Claude Code** (dentro de este repo o de una tool):

- Antes de proponer nombres de productos, países, stages o labels: consulta §6. No inventes.
- Antes de elegir un color: usa una variable CSS (`hsl(var(--primary))`, `bg-accent`, `hsl(var(--accent-{producto}))`, `hsl(var(--deco-{nombre}))`, etc.) o un hex de §2.1/§2.3/§2.4/§5.3. Si necesitas un color que no está en este documento, párate y avisa a Hugo.
- Antes de componer una pantalla: revisa §5.4 y nombra qué patrón vas a usar para cada bloque. Si propones algo que no está en §5.4, justifícalo y comprueba que no caes en §5.5.
- En tools Streamlit: aplica §2.6 al `.streamlit/config.toml` desde el primer mensaje útil.
- En copy: §1.2. Español de España, frases cortas, estados de sync con las labels de §1.2.

---

## 9. Verificación

Comandos para reconfirmar la fuente de marca cuando se sospeche que iseazy.com ha cambiado de identidad:

```bash
# Top colores por frecuencia en la home pública
curl -s 'https://www.iseazy.com/es/' -A 'Mozilla/5.0' \
  | grep -oE '#[0-9a-fA-F]{6}|#[0-9a-fA-F]{3}\b' \
  | sort | uniq -c | sort -rn | head -20

# Familia tipográfica declarada
curl -s 'https://www.iseazy.com/es/' -A 'Mozilla/5.0' \
  | grep -oE 'font-family:[^;"}]+' | sort -u
```

Si la salida cambia significativamente (deja de aparecer `#EB1C79` o `Work Sans`), abrir issue y actualizar §2.1 y §3 antes que el código.

---

## 10. Playground

[`playground/index.html`](playground/index.html) renderiza los tokens (§2.1 a §2.5) y los 14 patrones de bloque (§5.4) en un único HTML estático. Sin build, sin dependencias locales — abre con doble click. Carga por CDN: Tailwind, Work Sans (Google Fonts) y Tabler Icons (webfont). Los assets locales (logos, screenshots) viven en `assets/` y se referencian con paths relativos.

Propósito:

- Validar visualmente cuando se modifica un token o se añade un patrón al §5.4.
- Probar variantes rápidas sin tocar el dashboard React.
- Compartir el sistema con compañeros que aún no tienen el dashboard arrancado.
- Verificar visualmente que la paleta de §2 sigue alineada con iseazy.com cuando se sospeche deriva — abrir iseazy.com en una pestaña y el playground en otra, comparar swatches. La verificación reproducible (curl + grep) está en §9.

Regla: cuando se añade o modifica un patrón en §5.4, el playground debe actualizarse en el mismo commit. Si el playground no refleja un patrón, ese patrón **no se considera canónico** aunque esté escrito en el doc — la implementación de referencia vive en el HTML.

Lo que NO es el playground:

- No es una librería de componentes. Es una página de inspección.
- No es la implementación de producción. Cada tool consumidora reimplementa los patrones en su stack (React + shadcn, Streamlit, etc.) leyendo este doc y el HTML del playground como referencia.
- No tiene tests. Si un patrón rompe en el playground, se nota a ojo.

---

## 11. Assets

Los recursos visuales compartidos (logos, screenshots, patrones decorativos) viven en `assets/`. Las tools consumidoras importan desde aquí, no duplican.

### 11.1 Estructura

```
assets/
├── logos/
│   └── iseazy/                  Logo principal isEazy + 6 logos de producto (color + white)
├── screenshots/                 Screenshots reales de producto (vacío por ahora)
└── patterns/
    └── dots.svg                 Tramado decorativo para fondos
```

Ver [`assets/README.md`](assets/README.md) para naming convention y reglas de uso por entorno (HTML estático, React, Streamlit).

> **No hay carpeta de clientes**: este sistema es para distribuir el branding isEazy a las tools internas. Si una tool necesita logos de cliente (caso de uso poco frecuente), los aloja en su propio repo. El componente `badge-grid` (§5.4.12) y `marquee-strip` (§5.4.13) son genéricos — sirven igual para mostrar productos isEazy, integraciones, certificaciones o logos de cliente cuando aplique.

### 11.2 Estado actual

- **`logos/iseazy/`** — **SVGs oficiales** del kit de marca isEazy. Incluye:
  - `iseazy.svg` (logo principal en color) + `iseazy-white.svg` (versión blanca para fondos oscuros).
  - 6 logos de producto: `iseazy-author.svg`, `iseazy-factory.svg`, `iseazy-skills.svg`, `iseazy-lms.svg`, `iseazy-engage.svg`, `iseazy-game.svg`, cada uno con su variante `-white.svg`.
  - ViewBox nativo (ratio ~5.6:1 a ~8:1, son logos horizontales). Se controlan en CSS con `class="h-7 w-auto"` o equivalente — el ancho se calcula automático.
- **`screenshots/`** — vacío. Los screenshots reales de producto se añaden con naming `<producto>-<vista>.png` o `.webp`.

### 11.3 Reglas

- **Formato**: SVG siempre que sea posible. PNG solo para screenshots de producto reales.
- **Tamaño**: SVG sin `width`/`height` fijos — solo `viewBox`. El consumidor controla tamaño en CSS.
- **Color**:
  - **Logos de producto isEazy**: color de marca embebido (cada SVG lleva su accent del §2.3). Para dark mode usar el sufijo `-white.svg`.
  - **Patrones decorativos**: `currentColor` por defecto.
- **Naming**: kebab-case, descriptivo. Productos isEazy: `iseazy-<producto>.svg` y `iseazy-<producto>-white.svg`.
- **Qué NO commitear aquí**: assets de un solo uso, mocks de Figma, exports temporales, screenshots para una pantalla específica de una tool, logos de cliente concretos. Eso vive en la tool consumidora.

### 11.4 Consumo desde tools

| Entorno | Cómo importar |
|---|---|
| **HTML estático** (playground, landings) | `<img src="../assets/logos/iseazy/iseazy.svg" alt="isEazy" class="h-8 w-auto" />` |
| **React + Vite** | Tratar `iseazy-design-system` como submódulo git o paquete monorepo. Importar con `import logo from "@/design-system/assets/logos/iseazy/iseazy.svg"` y montar `<img src={logo} />`. |
| **React + Next.js** | Mover `assets/` a `public/` de la app consumidora vía script de sync, o servirlos desde un CDN. |
| **Streamlit** | `st.image("design-system/assets/logos/iseazy/iseazy.svg", width=120)`. |

### 11.5 Logos vs iconos

No confundir:

- **Logo** (este §11): identifica una entidad concreta (isEazy, un producto). SVG dibujado, no editable.
- **Icono** (§5.2): símbolo genérico de una acción/concepto. Tabler Icons, intercambiables.

Si dudas: un icono se puede sustituir por otro icono parecido sin cambiar el significado. Un logo no.

---

## 12. Utilidades CSS extra (copy-paste)

Pegar este bloque en `globals.css` o en un `<style>` propio. Son utilidades **adicionales a Tailwind/shadcn** que el sistema necesita: tipografía nombrada (§3.2), buttons (§5.6), forms (§5.7), patrones especiales (clip-path blob, dotted background, marquee).

Todas usan las CSS variables definidas en §2.2, §2.3, §2.4, §2.5 y §2.6. Si esas variables no están en el `:root`, esto no pinta.

### 12.1 Tipografía (`.t-*` de §3.2)

```css
.t-display { font-size: 3rem; font-weight: 600; letter-spacing: -0.025em; line-height: 1.1; color: hsl(var(--text-heading)); }
@media (min-width: 768px) { .t-display { font-size: 3.75rem; } }

.t-h1 { font-size: 2.25rem; font-weight: 600; letter-spacing: -0.025em; line-height: 1.25; color: hsl(var(--text-heading)); }

.t-h2 { font-size: 1.875rem; font-weight: 600; letter-spacing: -0.025em; line-height: 1.25; color: hsl(var(--text-heading)); }
@media (min-width: 768px) { .t-h2 { font-size: 2.25rem; } }

.t-h3 { font-size: 1.5rem; font-weight: 600; letter-spacing: -0.025em; line-height: 1.375; color: hsl(var(--text-heading)); }
@media (min-width: 768px) { .t-h3 { font-size: 1.875rem; } }

.t-h4 { font-size: 1.125rem; font-weight: 600; line-height: 1.375; color: hsl(var(--text-heading)); }

.t-eyebrow { font-size: 0.875rem; font-weight: 500; text-transform: uppercase; letter-spacing: 0.05em; color: hsl(var(--text-subtle)); }

.t-body-lg { font-size: 1.125rem; line-height: 1.625; color: hsl(var(--text-subtle)); }
.t-body { font-size: 1rem; line-height: 1.5; color: hsl(var(--text-body)); }
.t-body-sm { font-size: 0.875rem; line-height: 1.5; color: hsl(var(--text-subtle)); }

.t-caption { font-size: 0.75rem; font-weight: 500; color: hsl(var(--text-soft)); }

.t-quote { font-size: 1.125rem; line-height: 1.625; font-style: italic; color: hsl(var(--text-body)); }

.t-metric { font-size: 3rem; font-weight: 600; font-variant-numeric: tabular-nums; line-height: 1; }
@media (min-width: 768px) { .t-metric { font-size: 3.75rem; } }
.t-metric-label { font-size: 0.875rem; color: hsl(var(--text-subtle)); margin-top: 0.5rem; max-width: 18ch; }

.t-link { font-weight: 500; color: hsl(var(--text-link)); text-decoration: none; transition: color 0.15s; }
.t-link:hover { color: hsl(var(--text-link-hover)); }

.t-code { font-family: ui-monospace, SFMono-Regular, monospace; font-size: 0.875rem; color: hsl(var(--text-body)); background: hsl(var(--muted) / 0.6); padding: 0.125rem 0.375rem; border-radius: 0.25rem; }
```

### 12.2 Buttons (`.btn` + variants de §5.6)

```css
/* Base */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  font-family: "Work Sans", sans-serif;
  font-weight: 500;
  text-decoration: none;
  border-radius: 0.5rem;
  transition: all 200ms;
  cursor: pointer;
}
.btn:disabled { cursor: not-allowed; background: hsl(var(--gray-300)); color: hsl(var(--gray-500)); box-shadow: none; border-color: transparent; }
.btn:focus-visible { outline: none; box-shadow: 0 0 0 2px hsl(var(--background)), 0 0 0 4px hsl(var(--primary)); }

/* Sizes */
.btn-sm { height: 2.25rem; padding: 0 0.75rem; font-size: 0.875rem; }
.btn-md { height: 2.75rem; padding: 0 1rem; font-size: 1rem; }
.btn-lg { height: 3rem; padding: 0 1.5rem; font-size: 1.125rem; }

/* Variant: primary (solid magenta) */
.btn-primary {
  background: hsl(var(--primary));
  color: white;
  box-shadow:
    0 0 2px 0 hsl(var(--primary) / 0.16),
    2px 2px 8px 0 hsl(var(--primary) / 0.16);
}
.btn-primary:hover:not(:disabled) { background: hsl(var(--secondary)); }
.btn-primary:active:not(:disabled) { background: hsl(var(--brand-magenta-dark)); }

/* Variant: secondary (solid purple) */
.btn-secondary {
  background: hsl(var(--secondary));
  color: white;
  box-shadow:
    0 0 2px 0 hsl(var(--secondary) / 0.16),
    2px 2px 8px 0 hsl(var(--secondary) / 0.16);
}
.btn-secondary:hover:not(:disabled) { background: hsl(var(--primary)); }
.btn-secondary:active:not(:disabled) { background: hsl(var(--brand-purple-deep)); }

/* Variant: outline (white bg, magenta border) */
.btn-outline {
  background: white;
  color: hsl(var(--primary));
  border: 1px solid hsl(var(--primary));
  box-shadow:
    0 0 2px 0 hsl(var(--primary) / 0.16),
    2px 2px 8px 0 hsl(var(--primary) / 0.16);
}
.btn-outline:hover:not(:disabled) { background: hsl(var(--primary)); color: white; }

/* Variant: ghost (no bg, no border) */
.btn-ghost {
  background: transparent;
  color: hsl(var(--text-body));
}
.btn-ghost:hover:not(:disabled) { background: hsl(var(--brand-magenta-soft)); color: hsl(var(--primary)); }

/* Variant: link (looks like a link) */
.btn-link {
  background: transparent;
  color: hsl(var(--text-link));
  font-weight: 500;
  padding: 0;
  height: auto;
}
.btn-link:hover:not(:disabled) { color: hsl(var(--text-link-hover)); text-decoration: underline; }

/* Variant: destructive */
.btn-destructive {
  background: hsl(var(--destructive));
  color: white;
  box-shadow:
    0 0 2px 0 hsl(var(--destructive) / 0.16),
    2px 2px 8px 0 hsl(var(--destructive) / 0.16);
}
.btn-destructive:hover:not(:disabled) { background: hsl(354 70% 44%); }
```

### 12.3 Forms (`.field` + `.field-input` de §5.7)

```css
/* Field group */
.field { display: flex; flex-direction: column; gap: 0.375rem; }
.field-label { font-size: 0.875rem; font-weight: 500; color: hsl(var(--text-heading)); }
.field-helper { font-size: 0.75rem; color: hsl(var(--text-soft)); margin-top: 0.125rem; }
.field-error { font-size: 0.75rem; color: hsl(var(--destructive)); margin-top: 0.125rem; display: flex; align-items: center; gap: 0.25rem; }

/* Input base — applies to text inputs, textarea, select */
.field-input {
  width: 100%;
  height: 2.75rem;
  padding: 0 1rem;
  font-family: "Work Sans", sans-serif;
  font-size: 1rem;
  color: hsl(var(--text-body));
  background: hsl(var(--muted));
  border: 1px solid hsl(var(--border));
  border-radius: 0.5rem;
  outline: none;
  transition: border-color 150ms, box-shadow 150ms, background-color 150ms;
}
.field-input::placeholder { color: hsl(var(--text-disabled)); }
.field-input:hover:not(:disabled):not(:focus) { border-color: hsl(var(--gray-400)); }
.field-input:focus { border-color: hsl(var(--primary)); box-shadow: 0 0 0 2px hsl(var(--primary) / 0.15); }
.field-input:disabled { background: hsl(var(--gray-100)); color: hsl(var(--text-disabled)); cursor: not-allowed; }
.field-input:read-only { background: hsl(var(--gray-50)); cursor: text; }
.field-input[aria-invalid="true"] { border-color: hsl(var(--destructive)); }
.field-input[aria-invalid="true"]:focus { box-shadow: 0 0 0 2px hsl(var(--destructive) / 0.15); }

/* Textarea — same styling, taller */
textarea.field-input { height: auto; min-height: 5.5rem; padding: 0.75rem 1rem; resize: vertical; line-height: 1.5; }

/* Checkbox */
.checkbox {
  appearance: none;
  width: 1.25rem;
  height: 1.25rem;
  border: 1.5px solid hsl(var(--gray-400));
  border-radius: 0.25rem;
  background: white;
  cursor: pointer;
  transition: all 150ms;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}
.checkbox:hover:not(:disabled) { border-color: hsl(var(--primary)); }
.checkbox:focus-visible { outline: none; box-shadow: 0 0 0 2px white, 0 0 0 4px hsl(var(--primary) / 0.4); }
.checkbox:checked {
  background: hsl(var(--primary));
  border-color: hsl(var(--primary));
  background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16' fill='none' stroke='white' stroke-width='2.5' stroke-linecap='round' stroke-linejoin='round'><polyline points='3 8 7 12 13 4'/></svg>");
  background-size: 0.875rem;
  background-repeat: no-repeat;
  background-position: center;
}
.checkbox:disabled { background: hsl(var(--gray-200)); border-color: hsl(var(--gray-300)); cursor: not-allowed; }

/* Radio */
.radio {
  appearance: none;
  width: 1.25rem;
  height: 1.25rem;
  border: 1.5px solid hsl(var(--gray-400));
  border-radius: 9999px;
  background: white;
  cursor: pointer;
  transition: all 150ms;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}
.radio:hover:not(:disabled) { border-color: hsl(var(--primary)); }
.radio:focus-visible { outline: none; box-shadow: 0 0 0 2px white, 0 0 0 4px hsl(var(--primary) / 0.4); }
.radio:checked { border-color: hsl(var(--primary)); background: hsl(var(--primary)); }
.radio:checked::after { content: ""; display: block; width: 0.5rem; height: 0.5rem; border-radius: 9999px; background: white; }
.radio:disabled { background: hsl(var(--gray-200)); border-color: hsl(var(--gray-300)); cursor: not-allowed; }

/* Switch */
.switch {
  position: relative;
  display: inline-flex;
  align-items: center;
  width: 2.75rem;
  height: 1.5rem;
  border-radius: 9999px;
  background: hsl(var(--gray-300));
  transition: background 150ms;
  cursor: pointer;
  border: none;
  padding: 0;
}
.switch[aria-checked="true"] { background: hsl(var(--primary)); }
.switch:focus-visible { outline: none; box-shadow: 0 0 0 2px white, 0 0 0 4px hsl(var(--primary) / 0.4); }
.switch:disabled { background: hsl(var(--gray-200)); cursor: not-allowed; }
.switch-thumb {
  display: block;
  width: 1.25rem;
  height: 1.25rem;
  border-radius: 9999px;
  background: white;
  transform: translateX(0.125rem);
  transition: transform 150ms;
  box-shadow: 0 1px 2px rgba(0,0,0,0.15);
}
.switch[aria-checked="true"] .switch-thumb { transform: translateX(1.375rem); }
```

### 12.4 Patrones especiales

```css
/* Media-card blob-top (§5.4.4 variant blob-top) */
.post-blob { clip-path: ellipse(80% 75% at 50% 25%); }

/* Split-section dotted pattern (§5.4.10) */
.pattern-dots {
  background-image: url('../assets/patterns/dots.svg');
  background-repeat: repeat;
  color: hsl(var(--gray-600));
}

/* Marquee-strip animation (§5.4.13) */
@keyframes marquee {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}
.animate-marquee { animation: marquee 30s linear infinite; }

/* Hide native details marker (accordion-list §5.4.11) */
details > summary { list-style: none; }
details > summary::-webkit-details-marker { display: none; }
```

### 12.5 Resumen

Si las tools consumidoras prefieren no copiar todo este CSS, las alternativas son:

1. **Crear un paquete npm** (`@iseazy/design-system-css`) que exporte estos estilos y se importe en `globals.css`.
2. **Usarlos como referencia visual** y reimplementar con `@apply` o componentes en su stack (lo más limpio para apps React grandes).
3. **Copy-paste tal cual** (lo más rápido para tools pequeñas y prototipos).

Cualquiera de las tres es válida. La regla: si copias, copia desde **aquí**, no desde el playground — el playground es el preview, el doc es la fuente.
