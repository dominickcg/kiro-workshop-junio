# Agent Skills

Skills son paquetes portables de instrucciones que siguen el estándar abierto [Agent Skills](https://agentskills.io/). Empaquetan instrucciones, scripts y templates en paquetes reutilizables que Kiro puede activar cuando son relevantes para la tarea actual.

## El Problema que Resuelven

Los agentes de IA son cada vez más capaces, pero a menudo carecen del contexto específico necesario para el trabajo real. Sin conocimiento de los procesos de deployment de tu equipo, los estándares de code review de tu empresa, o el pipeline de análisis de datos de tu proyecto, los agentes adivinan e iteran.

Cargar todo ese contexto de antemano tampoco es práctico: demasiada información abruma al agente, ralentiza respuestas y reduce calidad.

---

## Cómo Funcionan: Progressive Disclosure

1. **Discovery** — Al inicio, Kiro carga solo el nombre y descripción de cada skill
2. **Activation** — Cuando tu request coincide con la descripción del skill, Kiro carga las instrucciones completas
3. **Execution** — Kiro sigue las instrucciones, cargando scripts o archivos de referencia solo cuando es necesario

Esto mantiene el contexto enfocado mientras da acceso a conocimiento especializado extenso on-demand.

---

## Estructura de un Skill

```
my-skill/
├── SKILL.md           # Requerido — instrucciones principales
├── scripts/           # Opcional — código ejecutable
├── references/        # Opcional — documentación de referencia
└── assets/            # Opcional — templates y recursos
```

### Formato de SKILL.md

```markdown
---
name: pr-review
description: Review pull requests for code quality, security issues, and test coverage. Use when reviewing PRs or preparing code for review.
---

## Review process

1. Check for security vulnerabilities
2. Verify error handling
3. Confirm test coverage
4. Review naming and structure
```

### Campos del Frontmatter

| Campo | Requerido | Descripción |
|-------|-----------|-------------|
| `name` | Sí | Debe coincidir con el nombre de la carpeta. Minúsculas, números, guiones (max 64 chars) |
| `description` | Sí | Cuándo usar este skill. Kiro lo compara contra los requests (max 1024 chars) |
| `license` | No | Nombre de licencia o referencia a archivo |
| `compatibility` | No | Requisitos del entorno |
| `metadata` | No | Datos adicionales (autor, versión) |

---

## Alcance

### Workspace Skills
- Ubicación: `.kiro/skills/`
- Aplican solo al workspace actual
- Para workflows específicos del proyecto (deployment, convenciones del equipo)

### Global Skills
- Ubicación: `~/.kiro/skills/`
- Disponibles en todos los workspaces
- Para workflows personales independientes del proyecto (tu checklist de code review)

> En caso de conflicto de nombres, el workspace skill tiene prioridad.

---

## Uso

### Activación automática
Kiro activa skills automáticamente cuando tu request coincide con la descripción del skill.

### Activación manual
Escribir `/` en el input del chat para ver los skills disponibles como slash commands. Seleccionar uno carga las instrucciones completas.

### Gestión
Ver y administrar skills en la sección "Agent Steering & Skills" del panel de Kiro.

---

## Importar Skills

1. Abrir "Agent Steering & Skills" en el panel de Kiro
2. Clic en `+` → "Import a skill"
3. Elegir fuente:
   - **GitHub** — URL pública de un repositorio (apuntando a la carpeta del skill o al SKILL.md)
   - **Local folder** — Desde tu filesystem

Los skills importados se copian al directorio de skills y funcionan inmediatamente.

---

## Skills vs Steering vs Powers

| Aspecto | Skills | Steering | Powers |
|---------|--------|----------|--------|
| Estándar | Abierto (agentskills.io) | Específico de Kiro | Específico de Kiro |
| Carga | On-demand por descripción | Configurable (always/auto/fileMatch/manual) | Dinámico por keywords |
| Puede incluir | Scripts, templates, docs | Markdown, file references | MCP tools + steering + hooks |
| Portabilidad | Alta (cross-tool) | Solo Kiro | Solo Kiro |
| Mejor para | Workflows reutilizables compartibles | Estándares del proyecto | Integraciones con herramientas externas |

---

## Mejores Prácticas

1. **Descripciones precisas** — Incluir keywords específicos: "Review pull requests for security and test coverage" es mejor que "helps with code review"
2. **SKILL.md enfocado** — Poner documentación detallada en `references/`
3. **Scripts para tareas determinísticas** — Validación, generación de archivos, y llamadas API funcionan mejor como scripts que como código generado por LLM
4. **Elegir el alcance correcto** — Global para workflows personales, workspace para procedimientos del equipo

---

*Fuente: [kiro.dev/docs/skills](https://kiro.dev/docs/skills/) — Contenido adaptado de la documentación oficial.*
