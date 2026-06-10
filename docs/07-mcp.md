# Model Context Protocol (MCP)

MCP es un protocolo que permite a Kiro comunicarse con servidores externos para acceder a herramientas, prompts y recursos especializados. Extiende las capacidades del agente más allá del IDE, conectándolo con documentación, bases de datos, APIs y servicios externos.

## Concepto

MCP actúa como un puente estandarizado entre Kiro y fuentes de datos o herramientas externas. Por ejemplo, el servidor MCP de AWS Documentation permite buscar, leer y obtener recomendaciones de documentación de AWS directamente desde Kiro.

---

## Capacidades de un Servidor MCP

Los servidores MCP pueden exponer tres tipos de recursos a Kiro:

| Tipo | Descripción |
|------|-------------|
| **Tools** | Funciones ejecutables (buscar docs, consultar APIs, manipular datos) |
| **Prompts** | Templates de prompts pre-configurados |
| **Resources** | Templates de recursos para acceso a datos estructurados |

---

## Configuración

Los servidores MCP se configuran mediante archivos `mcp.json`:

### A nivel de workspace
```
.kiro/settings/mcp.json
```

### A nivel global (usuario)
```
~/.kiro/settings/mcp.json
```

### Precedencia de configuración
```
usuario < workspace1 < workspace2 < ...
```
Las configuraciones de workspace más recientes sobreescriben las anteriores.

---

## Ejemplo de Configuración

```json
{
  "mcpServers": {
    "aws-docs": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

### Campos del esquema

| Campo | Descripción |
|-------|-------------|
| `command` | Comando para ejecutar el servidor |
| `args` | Argumentos del comando |
| `env` | Variables de entorno |
| `disabled` | Habilitar/deshabilitar el servidor |
| `autoApprove` | Lista de herramientas que se aprueban automáticamente |

---

## Requisitos de Instalación

Los servidores MCP típicamente usan `uvx` para ejecutarse, que requiere tener instalado `uv` (package manager de Python):

- **Con pip:** `pip install uv`
- **Con homebrew:** `brew install uv`
- **Guía oficial:** [docs.astral.sh/uv/getting-started/installation](https://docs.astral.sh/uv/getting-started/installation/)

Una vez instalado `uvx`, descarga y ejecuta los servidores automáticamente sin instalación adicional por servidor.

---

## Soporte para MCP Remoto

Kiro soporta servidores MCP remotos, permitiendo conectar con servicios cloud sin ejecutar nada localmente.

---

## Gestión de Servidores

- Los servidores se reconectan automáticamente al cambiar la configuración
- Se pueden reconectar sin reiniciar Kiro desde la vista "MCP Servers" en el panel de Kiro
- También disponible desde Command Palette buscando "MCP"

---

## Elicitation (Input del usuario durante ejecución)

Los servidores MCP pueden solicitar input adicional del usuario durante la ejecución de una herramienta, permitiendo flujos interactivos.

---

## MCP en Kiro CLI

El CLI también soporta MCP con la misma configuración. Los servidores definidos en `.kiro/settings/mcp.json` del workspace o a nivel global están disponibles en sesiones CLI.

---

---

*Fuente: [kiro.dev/docs/mcp](https://kiro.dev/docs/mcp/) — Contenido adaptado de la documentación oficial.*
