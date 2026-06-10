# Spec-Driven Development

El desarrollo guiado por especificaciones es la funcionalidad diferenciadora de Kiro. Transforma una idea expresada en lenguaje natural en un plan de implementación estructurado y rastreable, antes de escribir una sola línea de código.

## Concepto

A diferencia del "vibe coding" donde se le pide al agente que genere código directamente, el spec-driven development formaliza el proceso de diseño e implementación en tres artefactos que actúan como fuente de verdad del proyecto.

## Los Tres Artefactos

Cada spec genera tres archivos almacenados en `.kiro/specs/{nombre-feature}/`:

### 1. requirements.md (o bugfix.md)

Captura los requerimientos como user stories con criterios de aceptación en notación EARS (Easy Approach to Requirements Syntax).

**Contenido típico:**
- User stories: "Como [rol], quiero [funcionalidad], para que [beneficio]"
- Criterios de aceptación precisos y verificables
- Glosario de términos del dominio
- Propiedades de correctitud formales

### 2. design.md

Documenta la arquitectura técnica y el enfoque de implementación.

**Contenido típico:**
- Arquitectura del sistema y diseño de componentes
- Diagramas de secuencia y flujo de datos
- Modelos de datos
- Estrategia de manejo de errores
- Estrategia de testing

### 3. tasks.md

Proporciona un plan de implementación con tareas discretas y rastreables.

**Contenido típico:**
- Tareas con descripción clara y resultado esperado
- Dependencias entre tareas
- Sub-tareas cuando una tarea es compleja
- Estado en tiempo real (not started, in progress, completed)

---

## Flujos de Trabajo

### Feature Specs

Para construir funcionalidades nuevas. Tres variantes:

**Requirements-First:**
1. Se recopilan y documentan los requerimientos
2. Se diseña la arquitectura técnica basada en los requerimientos
3. Se generan las tareas de implementación

**Design-First:**
1. Se documenta el diseño técnico
2. Se derivan los requerimientos formales del diseño
3. Se generan las tareas de implementación

**Quick Plan:**
- Genera los tres artefactos en una sola pasada sin puertas de aprobación intermedias
- Hace preguntas clarificadoras al inicio y luego produce todo el plan

### Bugfix Specs

Para corregir errores de forma quirúrgica y prevenir regresiones:
1. Se analiza el bug (comportamiento actual vs esperado vs no afectado)
2. Se diseña la corrección con mínimo impacto
3. Se generan las tareas de corrección y validación

---

## Ejecución de Tareas

### Ejecución Individual
Ejecuta una tarea a la vez con supervisión paso a paso.

### Run All Tasks (Ejecución Paralela por Waves)

Kiro analiza las dependencias entre tareas y construye un grafo de ejecución:

- **Wave 1** — Todas las tareas sin dependencias. Se ejecutan concurrentemente.
- **Wave 2** — Tareas cuyas dependencias se satisficieron en Wave 1. Se ejecutan concurrentemente.
- **Wave N** — Continúa hasta completar todas las tareas.

Las waves se ejecutan secuencialmente; las tareas dentro de una wave se ejecutan concurrentemente. Esto reduce significativamente el tiempo de ejecución para la mayoría de specs.

---

## Cuándo Usar Specs vs Vibe

| Situación | Recomendación |
|-----------|---------------|
| Features complejas con planificación estructurada | Spec |
| Bugfixes donde las regresiones son costosas | Spec |
| Colaboración en equipo que requiere documentación | Spec |
| Requerimientos o diseño que necesitan iteración | Spec |
| Prototipado rápido y exploratorio | Vibe |
| Código sin objetivos claros aún | Vibe |

---

## Cómo Iniciar un Spec

1. Desde el panel de Kiro, clic en `+` bajo Specs (o seleccionar "Spec" en el chat)
2. Kiro pregunta si es una Feature o un Bug
3. Para Features: describir la funcionalidad y elegir el flujo (Requirements-First o Design-First)
4. Para Bugs: describir el bug
5. Iterar en cada fase hasta la implementación

---

*Fuente: [kiro.dev/docs/specs](https://kiro.dev/docs/specs/) — Contenido adaptado de la documentación oficial.*
