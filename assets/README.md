# assets/

Recursos visuales compartidos por el design system isEazy. Las tools consumidoras los referencian vía URL raw a GitHub (recomendado) o vía submódulo si las hace falta funcionar offline.

## Estructura

```
assets/
├── logos/
│   └── iseazy/                       Logos oficiales isEazy (marca + 6 productos)
│       ├── iseazy.svg                Logo principal (check + wordmark, color)
│       ├── iseazy-white.svg          Versión blanca para fondos oscuros
│       ├── iseazy-author.svg         Producto Author — magenta
│       ├── iseazy-author-white.svg   Author en blanco (dark mode)
│       ├── iseazy-factory.svg        Producto Factory — púrpura
│       ├── iseazy-factory-white.svg
│       ├── iseazy-skills.svg         Producto Skills — dorado
│       ├── iseazy-skills-white.svg
│       ├── iseazy-lms.svg            Producto LMS — azul cielo
│       ├── iseazy-lms-white.svg
│       ├── iseazy-engage.svg         Producto Engage — violeta
│       ├── iseazy-engage-white.svg
│       ├── iseazy-game.svg           Producto Game — verde lima
│       └── iseazy-game-white.svg
├── screenshots/                      Screenshots reales de producto (vacío por ahora)
└── patterns/
    └── dots.svg                      Tramado para fondos decorativos (split-section)
```

> **No hay carpeta de clientes**: este repo distribuye **solo branding isEazy**. Si una tool necesita logos de clientes concretos, los aloja en su propio repo. Los patrones genéricos del design system (`badge-grid` §5.4.12, `marquee-strip` §5.4.13) sirven igual para productos isEazy, integraciones, o logos de cliente cuando aplique.

## Cómo se consumen

### Recomendado: URL raw a GitHub (cero clonado)

```html
<img src="https://raw.githubusercontent.com/factory-iseazy/iseazy-design-system/main/assets/logos/iseazy/iseazy.svg" alt="isEazy" class="h-8 w-auto" />
```

Para dark mode, usar el patrón dual:

```html
<img src=".../iseazy.svg"        class="h-8 w-auto block dark:hidden" />
<img src=".../iseazy-white.svg"  class="h-8 w-auto hidden dark:block" />
```

### Si la tool necesita funcionar offline (poco frecuente)

Añadir este repo como submódulo git y referenciar por path local. Esto requiere `git submodule update` cuando los logos cambien — preferir la URL raw salvo necesidad real.

## Reglas

- **Formato**: SVG siempre que sea posible. PNG solo para screenshots reales.
- **Tamaño**: SVG con viewBox sin `width`/`height` fijos. El tamaño se controla en CSS desde quien consume.
- **Color**: los logos de producto traen su color de marca embebido. Para dark mode existe la versión `-white.svg` de cada uno.
- **Naming**: kebab-case. Productos: `iseazy-<producto>.svg` y `iseazy-<producto>-white.svg`.
- **No commitear aquí**: assets de un solo uso, mocks de Figma, exports temporales, logos de cliente concretos, screenshots de una pantalla específica. Eso vive en la tool consumidora.
