---
inclusion: always
---

# Convenciones Técnicas — Dashboard Fintech

## Arquitectura General

Tres capas comunicadas por HTTP/REST:

```
Dashboard (React/Vite :5173) → API Gateway (Express :3001) → Data Service (FastAPI :8000)
```

- El frontend NUNCA llama directamente al Data Service.
- El API Gateway es el único punto de entrada REST para el frontend.
- El proxy de desarrollo en Vite redirige `/api` al Gateway (`http://localhost:3001`).

## TypeScript (Dashboard + API Gateway)

### Exports

- **Named exports** exclusivamente. No usar `export default` (excepción: `App.tsx` entry point).
- Los schemas Zod se exportan junto con su tipo inferido:
  ```ts
  export const BalanceSchema = z.object({...});
  export type Balance = z.infer<typeof BalanceSchema>;
  ```

### Imports

- Orden: framework/librerías → imports locales (relativos)
- Usar `import type { X }` para imports de solo tipo.
- En el API Gateway, todos los imports locales usan extensión `.js` (NodeNext resolution).

### State Management (Dashboard)

- **No usar librerías de estado global** (Redux, Zustand, etc.). Cada componente maneja su propio fetch state con hooks.
- Patron de fetch state con unión discriminada:
  ```ts
  type FetchState =
    | { status: "idle" }
    | { status: "loading" }
    | { status: "success"; data: T }
    | { status: "error"; message: string }
  ```
- Siempre usar `useCallback` para funciones de carga y `useEffect` para disparar la carga inicial.

### Validación

- **Defensa en profundidad**: Zod valida en cada frontera (Dashboard al recibir, Gateway al recibir de upstream, Gateway al recibir query params).
- Toda respuesta de API se valida con `schema.safeParse()` antes de usarla.
- Un fallo de parse se trata como error (nunca renderizar datos no validados).

### API Client (Dashboard)

- Usar `fetchWithValidation()` de `lib/api.ts` para toda petición al backend.
- Timeout de 10 segundos por request (AbortController).
- Errores normalizados con la clase `ApiError` (kinds: `network`, `timeout`, `http`, `parse`).

## Python (Data Service)

### Estilo

- Type hints en **todas** las firmas de funciones y tipos de retorno.
- Docstrings estilo Google (descripción + detalles con guiones).
- Módulos con docstring de nivel superior explicando propósito.
- Funciones privadas prefijadas con `_` (ej: `_random_date_last_30_days()`).
- Enums: usar patrón `class TxType(str, Enum)` para serialización JSON automática.

### Imports (Python)

Orden con línea en blanco entre grupos:
1. stdlib (`datetime`, `math`, `typing`, `uuid`)
2. terceros (`fastapi`, `pydantic`, `faker`, `hypothesis`)
3. locales (`from .models import ...`, `from .compute import ...`)

### Computación

- Toda lógica financiera vive en `compute.py` como **funciones puras** (sin side effects, sin acceso a estado global).
- Los handlers de FastAPI son thin wrappers que llaman a las funciones puras con el dataset in-memory.
- El dataset (`_transactions`) se genera una vez en el evento `startup` y NO muta durante la ejecución.

## API Gateway (Express)

### Rutas

- Un archivo por endpoint en `src/routes/` (ej: `balance.ts`, `transactions.ts`).
- Cada archivo exporta un Router con nombre: `export { router as balanceRouter }`.
- Barrel export en `src/routes/index.ts`.
- El health route se registra PRIMERO en `server.ts` (precedencia sobre proxy routes).

### Proxy Pattern

```ts
await proxyGet(req, res, {
  upstreamPath: "/balance",
  schema: BalanceSchema,
  endpoint: "/api/balance",
});
```

### Error Mapping

| Condición | HTTP | Body |
|-----------|------|------|
| Query param inválido | 400 | `{ error, parameter }` |
| Data Service inalcanzable/timeout | 503 | `{ error: "Data Service unavailable" }` |
| Respuesta upstream inválida (Zod fail) | 502 | `{ error, endpoint }` |
| Error inesperado | 500 | `{ error: "Internal gateway error" }` |

### Validación de queries

- Función pura `validateTransactionQuery()` en `validation.ts`.
- Se ejecuta ANTES del proxy (no gastar round-trip en queries inválidos).
- Retorna `ValidationResult` (discriminated union: success/failure con parámetro ofensor).

## Styling (Dashboard)

- **Tailwind v4** con utility classes directamente en JSX.
- Tokens de diseño definidos en `index.css` con `@theme { ... }`.
- NO usar CSS modules ni styled-components.
- Componentes de Shadcn/ui disponibles pero se escriben componentes custom con Tailwind cuando son simples.
- Cards: `rounded-lg bg-white p-6 shadow-sm`.
- Botones primarios: `rounded-md bg-primary px-4 py-2 text-sm font-medium text-white hover:bg-primary-hover`.

## Dependencias clave (versiones)

| Paquete | Dashboard | API Gateway | Data Service |
|---------|-----------|-------------|--------------|
| React | 19.x | — | — |
| Vite | 8.x | — | — |
| Tailwind | 4.x | — | — |
| TypeScript | 6.x | 5.x | — |
| Zod | 4.x | 3.x | — |
| Express | — | 4.x | — |
| Axios | — | 1.x | — |
| fast-check | 4.x | 3.x | — |
| Vitest | 4.x | 2.x | — |
| FastAPI | — | — | ≥0.110 |
| Pydantic | — | — | ≥2.6 |
| Python | — | — | ≥3.11 |
| hypothesis | — | — | ≥6.98 |

## Puertos de desarrollo

- Dashboard: `http://localhost:5173` (Vite dev server)
- API Gateway: `http://localhost:3001`
- Data Service: `http://localhost:8000`
