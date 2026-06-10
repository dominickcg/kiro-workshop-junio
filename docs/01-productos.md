# Productos de Kiro: IDE, CLI y Web

Kiro se presenta en tres superficies complementarias, cada una diseñada para un contexto de uso diferente pero con el mismo motor agéntico por debajo.

## Kiro IDE

El producto principal. Un entorno de desarrollo de escritorio basado en la arquitectura de VS Code, pero rediseñado para trabajar con agentes de IA.

**Características clave:**
- Chat multimodal con soporte para imágenes, documentos PDF/DOCX, y referencias a archivos
- Spec-driven development integrado (requirements → design → tasks)
- Agent hooks para automatización
- Steering files para contexto persistente
- Soporte nativo de MCP y Powers
- Compatibilidad con extensiones Open VSX, temas y configuraciones de VS Code
- Code diffs en tiempo real mientras el agente escribe
- Generación automática de mensajes de commit
- Diagnósticos inteligentes integrando extensiones del IDE
- Checkpoints para revertir cambios del agente

**Plataformas soportadas:** Windows, macOS, Linux

**Descarga:** [kiro.dev/downloads](https://kiro.dev/downloads/)

---

## Kiro CLI

Acceso completo al agente de Kiro desde la terminal, diseñado para desarrolladores que prefieren vivir en la línea de comandos.

**Características clave:**
- Misma capacidad agéntica que el IDE
- Funciona en terminal local o por SSH
- Soporte para los mismos modelos de IA
- Ideal para: build features, automatización de workflows, análisis de errores, trazado de bugs
- Interfaz interactiva con comandos slash (`/plan`, `/agent`, `/context`, etc.)
- Agente planificador dedicado para descomponer ideas en planes de implementación

**Instalación en Windows:**
```powershell
irm 'https://cli.kiro.dev/install.ps1' | iex
```

**Casos de uso:**
- Desarrollo sobre SSH en servidores remotos
- Automatización en pipelines CI/CD
- Sesiones de codificación extensas desde la terminal
- Ambientes headless sin interfaz gráfica

**Documentación:** [kiro.dev/cli](https://kiro.dev/cli/)

---

## Kiro Web (Preview)

Agente de desarrollo en el navegador, accesible desde [app.kiro.dev](https://app.kiro.dev).

**Características clave:**
- Desarrollo conversacional sin instalación local
- Escritura de código colaborativa con el agente
- Apertura de Pull Requests directamente desde la interfaz web
- Soporte para Powers y MCP
- Sandbox aislado para ejecución de código

**Casos de uso:**
- Prototipos rápidos sin configurar ambiente local
- Revisiones de código desde cualquier dispositivo
- Onboarding de nuevos miembros del equipo
- Experimentación con nuevas tecnologías

**Documentación:** [kiro.dev/docs/web](https://kiro.dev/docs/web/)

---

## Comparativa Rápida

| Característica | IDE | CLI | Web |
|---------------|-----|-----|-----|
| Specs | ✓ | ✓ | — |
| Steering | ✓ | ✓ | — |
| Hooks | ✓ | — | — |
| MCP | ✓ | ✓ | ✓ (Powers) |
| Multimodal (imágenes) | ✓ | ✓ | ✓ |
| Extensiones/Plugins | ✓ | — | — |
| Sin instalación | — | — | ✓ |
| SSH/Remote | — | ✓ | — |

---

*Fuente: [kiro.dev](https://kiro.dev/) — Contenido adaptado de la documentación oficial.*
