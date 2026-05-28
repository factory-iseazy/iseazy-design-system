# iseazy-design-system

Sistema de diseño compartido por las tools internas de Marketing iseazy (core `bbdd-salesforce-sync` + tools de compañeros derivadas del `iseazy-tool-starter`).

**Fuente única de verdad**: [DESIGN.md](DESIGN.md). Cualquier cambio de paleta, tipografía, voz o nomenclatura va aquí — los repos consumidores leen este fichero al inicio de cada sesión de Claude Code.

## Cómo se consume

### Opción A — Prompt al inicio de la sesión (sin tocar CLAUDE.md)

En cualquier repo, lo primero que le dices a Claude Code:

> Antes de cualquier output visual (UI, charts, copy, Streamlit), descarga `https://raw.githubusercontent.com/factory-iseazy/iseazy-design-system/main/DESIGN.md` y aplícalo como fuente única para colores, tipografía, voz y nomenclatura.

### Opción B — Incrustado en `CLAUDE.md` del repo consumidor

Añade este bloque al `CLAUDE.md` del repo:

```markdown
## Sistema de diseño

Para cualquier output visual (HTML/CSS, React, Streamlit, charts, copy, mockups), antes de empezar fetchea el DESIGN.md canónico:

    https://raw.githubusercontent.com/factory-iseazy/iseazy-design-system/main/DESIGN.md

Es la fuente única de paleta (#EB1C79 magenta, #3B189B púrpura), tipografía (Work Sans), labels de países y product lines, formato numérico y voz. No inventes valores; si necesitas algo que no está documentado, pregunta a Hugo.
```

### Cuándo usar cuál

- **A (prompt al inicio)**: scripts ad-hoc, notebooks, tools experimentales, exploración.
- **B (CLAUDE.md)**: cuando quieres que Claude lo aplique automáticamente en cada sesión sin que nadie tenga que recordarlo. Recomendado para tools que ya están en uso real.

## Versionado

`main` es la versión vigente. Si necesitas inmutabilidad (e.g. snapshot reproducible para un release de la app), usa la URL fija por commit SHA:

```
https://raw.githubusercontent.com/factory-iseazy/iseazy-design-system/<sha>/DESIGN.md
```

## Qué entra aquí y qué no

**Entra**: paleta de marca, tipografía, voz, escala tipográfica, componentes UI canónicos, nomenclatura de países, product lines, stages, formato numérico, locale.

**NO entra**: URLs internas, métricas de negocio concretas, nombres de empleados o sales_owners, IDs de Salesforce, IPs, secretos, OAuth, schemas de BBDD (esos viven en el repo core `bbdd-salesforce-sync`).
