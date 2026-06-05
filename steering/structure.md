---
inclusion: always
---

# Estructura del Proyecto — Dashboard Fintech

## Estructura de directorios

```
demo-kashio/
├── .kiro/
│   ├── specs/                        # Specs de features (requisitos → diseño → tareas)
│   │   └── {feature-name}/
│   │       ├── .config.kiro          # Metadata del spec
│   │       ├── requirements.md
│   │       ├── design.md
│   │       └── tasks.md
│   └── steering/                     # Reglas permanentes del proyecto
│       ├── product-rules.md
│       ├── tech-conventions.md
│       └── project-structure.md
├── docs/                             # Documentación de planificación (no specs)
├── dashboard/                        # Frontend React + Vite + TypeScript
│   └── src/
│       ├── components/               # Componentes de feature (PascalCase)
│       ├── lib/                      # Utilidades, API client, schemas (camelCase)
│       ├── assets/                   # Imágenes y recursos estáticos
│       ├── App.tsx                   # Entry point layout
│       ├── main.tsx                  # Bootstrap React
│       ├── index.css                 # Tailwind + tokens @theme
│       └── test-setup.ts            # Setup de vitest
├── api-gateway/                      # API Gateway Node.js + Express
│   └── src/
│       ├── routes/                   # Un archivo por endpoint
│       │   ├── index.ts             # Barrel re-export de todos los routers
│       │   ├── balance.ts
│       │   ├── kpis.ts
│       │   ├── transactions.ts
│       │   ├── cashflow.ts
│       │   └── health.ts
│       ├── server.ts                # Bootstrap Express + registro de rutas
│       ├── proxy.ts                 # Cliente Axios + proxyGet genérico
│       ├── schemas.ts               # Zod schemas (response + query)
│       └── validation.ts            # Validación pura de query params
├── data-service/                     # Data Service Python + FastAPI
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py                  # FastAPI app + routes (thin handlers)
│   │   ├── models.py                # Pydantic schemas y modelos
│   │   ├── compute.py              # Funciones puras de cálculo
│   │   ├── generator.py            # Generador Faker de transacciones
│   │   └── config.py               # Constantes de configuración
│   └── tests/                       # Tests pytest + hypothesis
│       ├── test_property_compute.py
│       ├── test_property_filter.py
│       └── test_property_generator.py
├── e2e/                              # Tests E2E con Playwright
└── load/                             # Tests de carga con K6
```

## Convenciones de nombrado de archivos

| Capa | Tipo | Formato | Ejemplo |
|------|------|---------|---------|
| Dashboard | Componente | PascalCase.tsx | `BalancePanel.tsx`, `CashFlowChart.tsx` |
| Dashboard | Utilidad | camelCase.ts | `format.ts`, `api.ts`, `schemas.ts` |
| Dashboard | Test unitario | `{Component}.test.tsx` | `BalancePanel.test.tsx` |
| Dashboard | Test property | `{module}.prop.test.ts(x)` | `format.prop.test.ts`, `KpiCard.prop.test.tsx` |
| Gateway | Ruta | camelCase.ts | `balance.ts`, `transactions.ts` |
| Gateway | Test property | `gateway.property.test.ts` | — |
| Gateway | Test integración | `integration.test.ts` | — |
| Data Service | Módulo | snake_case.py | `compute.py`, `models.py` |
| Data Service | Test | `test_property_{module}.py` | `test_property_compute.py` |

## Dónde colocar código nuevo

### Nuevo componente de UI
→ `dashboard/src/components/{NombreComponente}.tsx`

### Nueva utilidad/helper del frontend
→ `dashboard/src/lib/{nombreUtilidad}.ts`

### Nuevo schema Zod (frontend)
→ Agregar al archivo existente `dashboard/src/lib/schemas.ts`

### Nuevo endpoint en el Gateway
1. Crear `api-gateway/src/routes/{nombre}.ts` con Router
2. Exportar en `api-gateway/src/routes/index.ts`
3. Registrar en `api-gateway/src/server.ts`
4. Si necesita schema nuevo → agregar en `api-gateway/src/schemas.ts`

### Nuevo endpoint en el Data Service
1. Agregar handler en `data-service/app/main.py`
2. Si necesita modelo nuevo → agregar en `data-service/app/models.py`
3. Si necesita lógica nueva → agregar función pura en `data-service/app/compute.py`

### Nuevos tests
- Dashboard: co-locados con el archivo fuente (`*.test.tsx` o `*.prop.test.ts`)
- Gateway: co-locados en `src/` (`*.test.ts` o `*.property.test.ts`)
- Data Service: en `data-service/tests/test_*.py`

## Convenciones de testing

### Property-Based Tests (PBT)

Cada archivo de PBT debe incluir:

1. **Header comment** con Feature name, Property number, y Requirements validados
2. **Mínimo 100 iteraciones** (`{ numRuns: 100 }` en fast-check, `@settings(max_examples=100)` en hypothesis)
3. **Formato del comment de feature**:
   ```
   Feature: fintech-dashboard, Property N: {descripción}
   Validates: Requirements X.Y
   ```

### Ubicación de tests

| Servicio | Framework | Ubicación | Naming |
|----------|-----------|-----------|--------|
| Dashboard | Vitest + fast-check + Testing Library | Co-locados en `src/` | `*.test.tsx`, `*.prop.test.tsx` |
| Gateway | Vitest + fast-check + supertest | Co-locados en `src/` | `*.test.ts`, `*.property.test.ts` |
| Data Service | pytest + hypothesis | `tests/` dir separado | `test_property_*.py`, `test_*.py` |

### Comandos de test

```bash
# Dashboard
cd dashboard && npx vitest --run

# API Gateway
cd api-gateway && npx vitest --run

# Data Service
cd data-service && python -m pytest
```

## Convenciones de nuevos specs

- Directorio: `.kiro/specs/{feature-name}/` (kebab-case)
- Archivos requeridos: `requirements.md`, `design.md`, `tasks.md`
- Archivo de config: `.config.kiro` (generado automáticamente)
- Los specs referencian requirements con formato `Requirement N.M`

## Documentación

- Planificación general y documentos de diseño preliminar van en `docs/`
- Los specs formales van en `.kiro/specs/`
- `docs/` es para discusión y planificación PRE-spec
- `.kiro/specs/` es la fuente de verdad para implementación
