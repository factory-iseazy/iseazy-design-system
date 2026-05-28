# DESIGN.md — Sistema de diseño Marketing iseazy

Documento canónico del sistema de diseño compartido por el core (`bbdd-salesforce-sync`) y las tools de compañeros (`iseazy-tool-starter` y derivados). Inspirado en la web pública [iseazy.com](https://www.iseazy.com/es/) — los tokens de color y la tipografía se han extraído literalmente del CSS servido por esa página (mayo 2026).

Cuando cambie la web de iseazy y queramos arrastrar la tool al nuevo branding, este es el único fichero a actualizar. El template `iseazy-tool-starter/DESIGN.md` debe quedar como copia exacta de este (`make sync-design`, si existe).

---

## 1. Identidad de marca

### 1.1 Producto

- **Nombre del producto core**: *Marketing Dashboard iseazy*.
- **One-liner** (extraído del [README](../README.md)): "Dashboard interno que sustituye el Excel mensual de Inbound de iseazy. Sincroniza directamente con Salesforce y mantiene una réplica local de Leads y Opportunities, con dashboards KPI por país."
- **Tools derivadas**: *Tu tool de Marketing iseazy* (template). Cada compañero le da nombre concreto al crear su repo (`tool-cohorts`, `tool-alerts`, etc.).
- **Eslogan de la familia isEazy** (web pública, no usar en el dashboard interno como reclamo, sí como respaldo en footer): "E-LEARNING MADE EASY".

### 1.2 Voz

Reglas tomadas del copy real de iseazy.com (`/es/`) y de las pantallas ya escritas (`HomePage.tsx`, `Layout.tsx`, `StatusBadge.tsx`):

| Atributo | Cómo se aplica |
|---|---|
| **Idioma por defecto** | Español de España. La web tiene `/es/`, `/en/`, `/br/`. La app interna está en `es-ES` (números, fechas, mes). Mismo enfoque aquí: español primero, inglés solo si una métrica o término técnico no tiene traducción canónica (`MQL`, `pipeline`, `won`). |
| **Tono** | Directo, profesional, sin marketing-speak. Una idea por frase. Ej. real del Home: *"Lanza una sincronización con Salesforce manual o consulta el histórico. Próximamente se ejecutará automáticamente cada noche."* |
| **Promesa simplicidad** | El claim que cruza toda la web isEazy es "made easy" / "todo en uno". En la tool interna se traduce a: una acción visible por pantalla, cero opciones decorativas. |
| **Estados sin jerga** | `pending → "En cola"`, `running → "Sincronizando"`, `succeeded → "Completado"`, `failed → "Error"`, `aborted_stale → "Abortado"` (ya implementado en `StatusBadge.tsx`). |
| **Mensajes vacíos** | Frases cortas, en cursiva muted. Ej. existente: *"Aún no se ha ejecutado ninguna."* |
| **Tiempos relativos** | `nunca`, `hace <1 min`, `hace 5 min`, `hace 2h`, `hace 3d` (ver `HomePage.tsx:7-17`). |

No usar emojis en UI ni en copy de error. Sí en mensajes de chat de Claude Code dentro de las tools si el compañero lo prefiere, pero **no en componentes renderizados**.

---

## 2. Tokens de color

Origen: `curl https://www.iseazy.com/es/` → `grep -oE '#[0-9a-fA-F]{3,6}'` → ordenado por frecuencia. Los valores siguientes son los que aparecen explícitamente en `background-color` / `color` / `border-color` del HTML público, no derivados.

### 2.1 Brand primario

| Token | HSL | Hex | Uso |
|---|---|---|---|
| `--brand-magenta` | `329 84% 51%` | `#EB1C79` | **Primary action**. Botones principales, links activos, badges de marca. 150 ocurrencias en CSS de iseazy.com — es *el* color de marca. |
| `--brand-purple` | `260 73% 35%` | `#3B189B` | Secondary brand. Fondos de sección hero, headers densos, badges informativos. |
| `--brand-ink` | `0 0% 20%` | `#333333` | Texto cuerpo. |
| `--brand-bg-soft` | `0 0% 96%` | `#F4F4F4` | Fondo de sección alternativo. |
| `--brand-divider` | `0 0% 92%` | `#EBEBEB` | Bordes y separadores. |

### 2.2 Variables CSS (modo claro)

Sustituyen el block actual de `frontend/src/styles/globals.css:6-25`. Se mantiene el contrato shadcn/ui (las mismas variables, distintos valores) para no reescribir componentes.

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

  --muted: 0 0% 96%;                  /* #F4F4F4 — brand-bg-soft */
  --muted-foreground: 0 0% 40%;       /* #666666 */

  --accent: 196 96% 49%;              /* #03A0F4 — info accent (presente en iseazy.com) */
  --accent-foreground: 0 0% 100%;

  --destructive: 354 70% 54%;         /* #DC3545 — usado en .text-destructive del CSS de iseazy */
  --destructive-foreground: 0 0% 100%;

  --success: 152 69% 31%;             /* #198754 */
  --warning: 45 100% 51%;             /* #FFC107 */

  --border: 0 0% 92%;                 /* #EBEBEB */
  --input: 0 0% 92%;
  --ring: 329 84% 51%;                /* matches --primary */

  --radius: 0.5rem;
}
```

Notas:

- `--ring` se enlaza al primary magenta para que el foco sea visible y consistente con la marca, no con el slate genérico de shadcn por defecto.
- `--success`, `--warning` no formaban parte de la paleta shadcn base; se añaden como tokens propios para reemplazar los `bg-blue-500/15` y `bg-green-500/15` hardcodeados en `StatusBadge.tsx:13-17`.
- **Modo oscuro**: pendiente. Mientras no exista una guía pública oscura de iseazy.com, dejar `.dark` con los valores que ya hay y no exponerlo en UI. Cuando se aborde, mantener `#EB1C79` como primary (alto contraste sobre fondos oscuros) y bajar `#3B189B` hacia `~280 70% 65%`.

### 2.3 Mapeo a estados de sync (`StatusBadge.tsx`)

| Estado | Clases actuales | Clases nuevas |
|---|---|---|
| `pending` | `bg-muted text-muted-foreground` | igual |
| `running` | `bg-blue-500/15 text-blue-700` | `bg-accent/15 text-accent` |
| `succeeded` | `bg-green-500/15 text-green-700` | `bg-[hsl(var(--success))]/15 text-[hsl(var(--success))]` |
| `failed` | `bg-destructive/15 text-destructive` | igual |
| `aborted_stale` | `bg-yellow-500/15 text-yellow-700` | `bg-[hsl(var(--warning))]/15 text-[hsl(var(--warning))]` |

### 2.4 Streamlit (tools de compañeros)

El template usa Streamlit. Para mantener identidad sin reescribirlo, cada tool debe llevar este `.streamlit/config.toml`:

```toml
[theme]
base = "light"
primaryColor = "#EB1C79"
backgroundColor = "#FFFFFF"
secondaryBackgroundColor = "#F4F4F4"
textColor = "#333333"
font = "sans serif"
```

Si la tool quiere acento púrpura para títulos/secciones, usar `st.markdown` con `color:#3B189B` literal.

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

### 3.2 Escala

Heredada de Tailwind, sin custom scale. Patrones canónicos extraídos del código actual y promovidos a estándar:

| Rol | Clase | Ejemplo en la app |
|---|---|---|
| H1 página | `text-3xl font-bold` | "Marketing Dashboard iseazy" (HomePage) |
| H2 sección de tarjeta | `text-lg font-semibold` | "Sincronización", "Dashboards de KPIs" |
| Eyebrow / sección monitor | `text-sm font-medium uppercase tracking-wide text-muted-foreground` | "Datos vivos en Salesforce" |
| Body | default (`text-base`) | párrafos de descripción |
| Muted explicativo | `text-sm text-muted-foreground` | subtítulos de tarjeta |
| Métrica grande | `text-2xl font-bold tabular-nums` | contadores de leads/opps |
| Label de métrica | `text-xs text-muted-foreground` | "Leads", "Opportunities" |
| Badge / status | `text-xs font-medium` | StatusBadge |

`tabular-nums` es **obligatorio** en cualquier cifra que se compare visualmente columna a columna (KPIs, tablas, contadores).

---

## 4. Layout, espacio y forma

- **Container**: ya configurado en `tailwind.config.ts:8` como `container mx-auto`, padding `2rem`, `2xl: 1400px`. No tocar.
- **Espaciado vertical entre secciones**: `space-y-8` (HomePage) en páginas, `space-y-4` dentro de tarjetas.
- **Radio**: `--radius: 0.5rem` (`rounded-lg`). Mantener. Solo bajar a `rounded-md`/`rounded-sm` para badges e inputs.
- **Bordes**: 1 px sólido, `border-border`. Hover en tarjetas que actúan como link: `hover:border-primary` (ver HomePage cards).
- **Sombras**: minimalistas. `shadow-sm` para elevar tarjetas con acción explícita; nunca sombras de >4 px de blur. La web pública prácticamente no usa sombra y nosotros tampoco.
- **Densidad**: la app es B2B interna. Prioridad a densidad y escaneabilidad sobre aire — no copiar el espaciado generoso del hero de la web pública dentro de las pantallas operativas.

---

## 5. Componentes base

Stack: React 18 + Tailwind + shadcn/ui + lucide-react + recharts + TanStack Query/Table. No introducir librerías extra sin justificar.

### 5.1 Patrones ya canónicos en el repo

- **Layout con header**: `Layout.tsx`. Nav con `NavItem` (rounded-md, active = bg-primary). **Ahora active queda magenta** automáticamente al cambiar `--primary`.
- **Tarjeta-link**: bloque `<Link>` con `rounded-lg border bg-card p-6 transition-colors hover:border-primary` + flecha `→` que se traslada en hover. Ver `HomePage.tsx:53-82`.
- **Sección de métricas**: `rounded-lg border bg-card p-6` con grid 2/4 columnas y eyebrow uppercase.
- **Status pill**: `StatusBadge` con punto pulsante para estados activos.
- **Barra de progreso indeterminada**: `ProgressBar` con `animate-pulse bg-blue-500`. Sustituir `bg-blue-500` por `bg-accent`.

### 5.2 Iconografía

`lucide-react` (ya instalado). Stroke 1.5, tamaño por defecto `size-4` inline en texto, `size-5` en cabeceras de tarjeta. No mezclar con emojis ni con otros sets.

### 5.3 Gráficos (`recharts`)

Paleta serializada para charts:

```ts
export const CHART_COLORS = [
  "#EB1C79", // brand magenta
  "#3B189B", // brand purple
  "#03A0F4", // accent blue
  "#00ACAD", // teal (presente en iseazy.com)
  "#198754", // success green
  "#FFC107", // warning amber
] as const;
```

Reglas:

1. Una serie → magenta.
2. Dos series → magenta + púrpura.
3. Won/lost → success / destructive, no magenta/púrpura (evitar confundir KPI positivo con marca).
4. Eje y grid en `hsl(var(--muted-foreground))` con opacidad 0.3 para grid.

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

- Cuando arranques una pantalla nueva: lee §1.2 (voz), §3.2 (escala), §5.1 (componentes ya canónicos). Copia el patrón más cercano.
- Cuando dudes si un color es de marca o no: existe en §2 o no se usa.

**Como Claude Code** (dentro de este repo o de una tool):

- Antes de proponer nombres de productos, países, stages o labels: consulta §6. No inventes.
- Antes de elegir un color: usa una variable CSS (`hsl(var(--primary))`, `bg-accent`, etc.) o un hex de §2.1/§5.3. Si necesitas un color que no está en este documento, párate y avisa a Hugo.
- En tools Streamlit: aplica §2.4 al `.streamlit/config.toml` desde el primer mensaje útil.
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
