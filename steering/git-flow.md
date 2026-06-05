---
inclusion: always
---

# Git Flow — Dashboard Fintech

## Modelo de ramas

```
main ← release/* ← develop ← feature/* | bugfix/*
main ← hotfix/*
```

| Rama | Propósito | Origen | Destino |
|------|-----------|--------|---------|
| `main` | Producción estable | — | Solo recibe merges de `release/*` y `hotfix/*` |
| `develop` | Integración continua | `main` (inicial) | Recibe merges de `feature/*` y `bugfix/*` |
| `feature/<ticket>-<slug>` | Nuevas funcionalidades | `develop` | → `develop` (vía PR) |
| `bugfix/<ticket>-<slug>` | Correcciones a develop | `develop` | → `develop` (vía PR) |
| `release/<version>` | Preparación de release | `develop` | → `main` + back-merge a `develop` |
| `hotfix/<ticket>-<slug>` | Fixes urgentes a producción | `main` | → `main` + back-merge a `develop` |

## Naming de ramas

- `<ticket>` = identificador corto (ej: `KASH-42`, `001`, o nombre del spec)
- `<slug>` = descripción kebab-case (ej: `transaction-detail-modal`)
- `<version>` = semver (ej: `1.0.0`, `1.1.0`)

Ejemplos:
```
feature/KASH-01-transaction-detail-modal
bugfix/KASH-15-fix-pagination-overflow
release/1.1.0
hotfix/KASH-20-fix-critical-balance-error
```

## Conventional Commits (obligatorio)

Todos los mensajes de commit DEBEN seguir el formato:

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### Types permitidos

| Type | Uso |
|------|-----|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de bug |
| `docs` | Cambios en documentación |
| `style` | Formato, whitespace (no cambia lógica) |
| `refactor` | Reestructuración sin cambio funcional |
| `test` | Añadir o modificar tests |
| `chore` | Tareas de mantenimiento, config, deps |
| `ci` | Cambios en CI/CD |
| `perf` | Mejoras de rendimiento |

### Scopes válidos

| Scope | Capa |
|-------|------|
| `dashboard` | Frontend React |
| `gateway` | API Gateway Express |
| `data-service` | Data Service FastAPI |
| `e2e` | Tests Playwright |
| `load` | Tests K6 |
| `spec` | Documentos de spec |
| `steering` | Documentos de steering |
| `deps` | Dependencias |

### Ejemplos

```
feat(dashboard): add transaction detail modal
fix(gateway): handle timeout on cashflow endpoint
test(data-service): add property test for export function
docs(spec): create dashboard-enhancements requirements
chore(deps): update fast-check to 4.9.0
```

## Flujo de merge y aprobación

### Feature / Bugfix → develop

1. Crear rama desde `develop`
2. Desarrollar y hacer commits con Conventional Commits
3. Abrir Pull Request hacia `develop`
4. **Gates obligatorios** (en orden):
   - ✅ QA — Tests pasan (unit, property, integration)
   - ✅ Security — No secrets, no vulnerabilidades conocidas
   - ✅ Reviewer — Aprobación humana de code review
5. Merge solo con aprobación humana explícita

### Release → main

1. Crear rama `release/<version>` desde `develop`
2. Solo fixes menores y bumps de versión en esta rama
3. Abrir Pull Request hacia `main`
4. **Gate obligatorio**:
   - ✅ Reviewer en modo **auditoría completa** (revisa TODOS los cambios del release)
5. Merge solo con aprobación humana explícita
6. Back-merge a `develop` después del merge a `main`

### Hotfix → main

1. Crear rama `hotfix/<ticket>-<slug>` desde `main`
2. Fix mínimo y dirigido
3. Abrir PR hacia `main`
4. **Gates**: QA + Security + Reviewer
5. Merge solo con aprobación humana explícita
6. Back-merge a `develop` después del merge a `main`

## Reglas para agentes (CRÍTICO)

- **NUNCA** ejecutar `git push` sin aprobación humana explícita.
- **NUNCA** ejecutar merge (fast-forward, squash, o rebase) sin aprobación humana.
- **NUNCA** hacer force push (`git push --force` / `git push -f`).
- **NUNCA** hacer commit directamente a `main` o `develop`.
- Siempre trabajar en ramas `feature/*` o `bugfix/*`.
- Antes de crear un commit, informar al usuario qué archivos se incluirán.
- Antes de push, informar al usuario la rama destino y pedir confirmación.
- Si el usuario no ha aprobado, NO proceder con la operación git.

## Configuración inicial

La rama por defecto es `main`. Al iniciar el proyecto:

```bash
git checkout -b main        # rama de producción
git checkout -b develop     # rama de integración
```

Toda feature nueva se crea desde `develop`:

```bash
git checkout develop
git checkout -b feature/<ticket>-<slug>
```
