---
inclusion: always
---

# Reglas de Producto — Dashboard Fintech

## Idioma de la UI

- Todos los textos visibles al usuario están en **español** (labels, mensajes de error, placeholders, tooltips, botones).
- Los mensajes de error siguen el patrón: descripción breve del problema + botón "Reintentar" cuando aplica.
- Ejemplos: "Error al cargar datos de saldo", "No se encontraron resultados para los filtros actuales".

## Paleta de colores (tokens)

Usar los tokens de diseño definidos en `dashboard/src/index.css` bajo `@theme`:

| Token | Hex | Uso |
|-------|-----|-----|
| `primary` | `#0666EB` | Botones, links, iconos activos, series de ingreso |
| `primary-hover` | `#1A77F9` | Estados hover de botones |
| `heading` | `#01142F` | Títulos y headings |
| `text-secondary` | `#667085` | Texto descriptivo y labels |
| `bg-base` | `#F2F2F2` | Fondo general de la página |
| `secondary` | `#FC5778` | Alertas, series de gasto, acciones secundarias |

NO usar colores hex directamente en componentes; siempre usar las clases `text-primary`, `bg-bg-base`, `text-heading`, `text-text-secondary`, `text-secondary`, `bg-primary`, `hover:bg-primary-hover`.

## Badges de estado y tipo

Estilos consistentes para badges reutilizables:

- **Status completed**: `bg-green-100 text-green-700` → "completado"
- **Status pending**: `bg-yellow-100 text-yellow-700` → "pendiente"
- **Status failed**: `bg-red-100 text-red-700` → "fallido"
- **Type income**: `bg-green-100 text-green-700` → "ingreso"
- **Type expense**: `bg-red-100 text-red-700` → "gasto"

## Estados de UI por widget

Cada widget del dashboard (BalancePanel, KpiSection, CashFlowChart, TransactionTable) maneja **independientemente** estos estados:

1. **Loading** — Skeleton con `animate-pulse` en grises (`bg-gray-200`)
2. **Error** — Mensaje + botón "Reintentar" que re-ejecuta la petición original
3. **Empty** (solo tabla) — Mensaje "No se encontraron resultados..." distinguible del error
4. **Success** — Datos renderizados con formato

El estado por defecto cuando no es "success" ni "error" es **loading/skeleton** (nunca mostrar datos vacíos o parciales).

## Formato de datos

- **Moneda**: Símbolo + número con 2 decimales y separadores de miles. Mapeo fijo: USD→`$`, PEN→`S/`, EUR→`€`. Usar `formatCurrency()` de `lib/format.ts`.
- **Fechas**: Formato `DD MMM YYYY` (ej: "15 Jan 2025"). Usar `formatDate()` de `lib/format.ts`.
- **Porcentajes**: 1 decimal, indicador visual (↑/↓/→) con color semántico.

## Datos mock

- Toda la data es generada por Faker (Python) al arrancar el Data Service.
- NO hay persistencia en base de datos ni operaciones de escritura.
- El dataset se genera una sola vez al inicio y permanece constante durante la sesión.
- Mínimo 50 transacciones, 60-70% gastos, 30-40% ingresos.

## Responsive

- **Desktop (≥768px)**: KPIs en fila horizontal, chart y tabla lado a lado.
- **Mobile (<768px)**: KPIs apilados verticalmente, tabla con scroll horizontal.
- Breakpoints de Tailwind: `md:` para 768px, `lg:` para 1024px.
