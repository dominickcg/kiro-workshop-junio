# Steering (Archivos de Dirección)

Steering da a Kiro conocimiento persistente sobre tu workspace a través de archivos Markdown. En lugar de explicar tus convenciones en cada chat, los archivos de steering aseguran que Kiro siga consistentemente tus patrones, librerías y estándares establecidos.

## Beneficios Clave

- **Generación de código consistente** — Cada componente, endpoint o test sigue los patrones del equipo
- **Reducción de repetición** — No es necesario explicar los estándares del workspace en cada conversación
- **Alineación del equipo** — Todos los desarrolladores trabajan con los mismos estándares
- **Conocimiento escalable** — Documentación que crece con el codebase

---

## Alcance de los Archivos

### Workspace Steering
- Ubicación: `.kiro/steering/` en la raíz del proyecto
- Aplica solo a ese workspace específico
- Para patrones, librerías y estándares del proyecto

### Global Steering
- Ubicación: `~/.kiro/steering/`
- Aplica a todos los workspaces
- Para convenciones universales del desarrollador o equipo

### Team Steering
- Archivos globales distribuidos por MDM o Group Policies
- O descargados desde un repositorio central a `~/.kiro/steering/`
- Para estándares organizacionales

> En caso de conflicto, el workspace steering tiene prioridad sobre el global.

---

## Archivos Fundacionales

Kiro puede generar automáticamente tres archivos base:

| Archivo | Propósito |
|---------|-----------|
| `product.md` | Propósito del producto, usuarios objetivo, funcionalidades clave, objetivos de negocio |
| `tech.md` | Frameworks, librerías, herramientas de desarrollo, restricciones técnicas |
| `structure.md` | Organización de archivos, convenciones de nombres, patrones de import, decisiones arquitectónicas |

Estos se incluyen en cada interacción por defecto y forman la baseline del entendimiento de Kiro sobre el proyecto.

---

## Modos de Inclusión

### Always (por defecto)
```yaml
---
inclusion: always
---
```
Se carga en cada interacción. Para estándares core que deben influir en toda generación de código.

### FileMatch (condicional)
```yaml
---
inclusion: fileMatch
fileMatchPattern: "components/**/*.tsx"
---
```
Se incluye solo al trabajar con archivos que coinciden con el patrón. Mantiene el contexto relevante y reduce ruido.

Patrones comunes:
- `"*.tsx"` — Componentes React
- `"app/api/**/*"` — API routes
- `"**/*.test.*"` — Archivos de test
- `["**/*.ts", "**/*.tsx"]` — Todo TypeScript

### Manual
```yaml
---
inclusion: manual
---
```
Disponible on-demand referenciándolo con `#nombre-del-archivo` en el chat. También aparece como slash command con `/`.

### Auto
```yaml
---
inclusion: auto
name: api-design
description: REST API design patterns and conventions. Use when creating or modifying API endpoints.
---
```
Se incluye automáticamente cuando el request coincide con la descripción. Similar a Skills.

---

## Referencias a Archivos

Vincular archivos vivos del workspace para mantener el steering actualizado:

```markdown
#[[file:api/openapi.yaml]]
#[[file:components/ui/button.tsx]]
#[[file:.env.example]]
```

---

## Soporte para AGENTS.md

Kiro soporta el estándar AGENTS.md. Estos archivos se colocan en la raíz del workspace o en `~/.kiro/steering/` y se incluyen siempre automáticamente (no soportan modos de inclusión).

---

## Estrategias Comunes

| Archivo | Contenido |
|---------|-----------|
| `api-standards.md` | Convenciones REST, formatos de error, autenticación, versionado |
| `testing-standards.md` | Patrones de unit test, mocking, cobertura esperada |
| `code-conventions.md` | Naming, organización de archivos, import ordering |
| `security-policies.md` | Autenticación, validación de datos, sanitización |
| `deployment-workflow.md` | Procedimientos de build, configuración de ambientes, CI/CD |

---

## Mejores Prácticas

1. **Un dominio por archivo** — API design, testing, o deployment procedures
2. **Nombres claros** — `api-rest-conventions.md`, `testing-unit-patterns.md`
3. **Incluir contexto** — Explicar el "por qué" de las decisiones, no solo el "qué"
4. **Proveer ejemplos** — Snippets y comparaciones before/after
5. **Seguridad** — Nunca incluir API keys, passwords o datos sensibles
6. **Mantener regularmente** — Revisar en sprint planning y cambios de arquitectura

---

*Fuente: [kiro.dev/docs/steering](https://kiro.dev/docs/steering/) — Contenido adaptado de la documentación oficial.*
