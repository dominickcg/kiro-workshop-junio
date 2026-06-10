# Hooks (Automatizaciones del Agente)

Los Agent Hooks son herramientas de automatización que ejecutan acciones predefinidas cuando ocurren eventos específicos en el IDE. Eliminan la necesidad de solicitar manualmente tareas rutinarias y aseguran consistencia en el codebase.

## Concepto

Un hook conecta un **evento del IDE** con una **acción del agente**. Cuando el evento ocurre, la acción se ejecuta automáticamente en segundo plano.

---

## Eventos Disponibles (Triggers)

| Evento | Descripción |
|--------|-------------|
| `fileEdited` | Al guardar un archivo |
| `fileCreated` | Al crear un archivo nuevo |
| `fileDeleted` | Al eliminar un archivo |
| `promptSubmit` | Al enviar un mensaje al agente |
| `agentStop` | Cuando el agente termina una ejecución |
| `preToolUse` | Antes de que el agente use una herramienta |
| `postToolUse` | Después de que el agente usa una herramienta |
| `preTaskExecution` | Antes de ejecutar una tarea de spec |
| `postTaskExecution` | Después de ejecutar una tarea de spec |
| `userTriggered` | Activación manual on-demand |

---

## Acciones Disponibles

### askAgent
Envía un prompt predefinido al agente. Útil para:
- Revisar código generado
- Generar documentación automáticamente
- Validar estándares de seguridad

### runCommand
Ejecuta un comando shell. Útil para:
- Correr linters o formatters
- Ejecutar tests
- Build automáticos

---

## Estructura de un Hook

Los hooks se definen como archivos JSON:

```json
{
  "name": "Lint on Save",
  "version": "1.0.0",
  "description": "Run ESLint when TypeScript files are saved",
  "when": {
    "type": "fileEdited",
    "patterns": ["*.ts", "*.tsx"]
  },
  "then": {
    "type": "runCommand",
    "command": "npm run lint"
  }
}
```

---

## Ejemplos Prácticos

### Linter al guardar archivos TypeScript
```json
{
  "name": "Lint on Save",
  "version": "1.0.0",
  "when": {
    "type": "fileEdited",
    "patterns": ["*.ts", "*.tsx"]
  },
  "then": {
    "type": "runCommand",
    "command": "npm run lint"
  }
}
```

### Revisar operaciones de escritura antes de ejecutarlas
```json
{
  "name": "Review Write Operations",
  "version": "1.0.0",
  "when": {
    "type": "preToolUse",
    "toolTypes": ["write"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Verify this write operation follows our coding standards"
  }
}
```

### Ejecutar tests después de completar una tarea de spec
```json
{
  "name": "Run Tests After Task",
  "version": "1.0.0",
  "when": {
    "type": "postTaskExecution"
  },
  "then": {
    "type": "runCommand",
    "command": "npm run test"
  }
}
```

### Generar documentación al crear archivos
```json
{
  "name": "Auto Document New Files",
  "version": "1.0.0",
  "description": "Generate JSDoc comments for new TypeScript files",
  "when": {
    "type": "fileCreated",
    "patterns": ["src/**/*.ts"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Add comprehensive JSDoc documentation to this new file"
  }
}
```

---

## Cómo Crear un Hook

### Opción 1: Con lenguaje natural
1. Navegar a "Agent Hooks" en el panel de Kiro
2. Clic en `+` → "Ask Kiro to create a hook"
3. Describir el workflow deseado en lenguaje natural
4. Revisar la configuración generada y guardar

### Opción 2: Manualmente
1. Navegar a "Agent Hooks" en el panel de Kiro
2. Clic en `+` → "Manually create a hook"
3. Llenar el formulario (título, descripción, evento, acción, instrucciones)
4. Clic en "Create Hook"

También desde Command Palette: `Ctrl + Shift + P` → "Kiro: Open Kiro Hook UI"

---

## Beneficios para Equipos

- **Calidad de código consistente** — Linters y formatters ejecutados automáticamente
- **Prevención de vulnerabilidades** — Revisiones de seguridad en cada escritura
- **Reducción de overhead manual** — Tareas repetitivas automatizadas
- **Estandarización de procesos** — Todos siguen el mismo workflow
- **Ciclos de desarrollo más rápidos** — Menos tareas manuales entre commits

---

*Fuente: [kiro.dev/docs/hooks](https://kiro.dev/docs/hooks/) — Contenido adaptado de la documentación oficial.*
