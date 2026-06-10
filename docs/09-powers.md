# Powers

Los Powers dan al agente de Kiro acceso instantáneo a conocimiento especializado para cualquier tecnología. Empaquetan herramientas, workflows y mejores prácticas en un formato que Kiro puede activar on-demand según el contexto de la conversación.

## El Problema: Sobrecarga de Contexto

**Sin contexto de framework, los agentes adivinan.** Tu agente puede llamar APIs de Stripe, pero ¿sabe usar idempotent keys? Puede consultar Neon, pero ¿entiende connection pooling para serverless?

**Con demasiado contexto, los agentes se ralentizan.** Conectar 5 servidores MCP carga 100+ definiciones de herramientas antes de escribir una línea de código. Cinco servidores pueden consumir 50,000+ tokens (40% de la ventana de contexto) antes del primer prompt.

---

## Cómo Funcionan

En lugar de cargar todas las herramientas MCP de una vez, los Powers se activan dinámicamente basándose en keywords de la conversación.

Cuando inicias una tarea, Kiro:
1. Lee la descripción de la tarea
2. Evalúa los Powers instalados contra la tarea
3. Carga solo los Powers relevantes en el contexto

**Ejemplo:** Instalas el Power de Stripe. Cuando mencionas "payment" o "checkout", el Power se activa cargando las herramientas MCP de Stripe y su POWER.md en contexto. Cuando te mueves a trabajo de base de datos, el Power de Supabase se activa y Stripe se desactiva.

---

## Anatomía de un Power

Un Power es un bundle unificado que incluye:

| Componente | Descripción |
|-----------|-------------|
| `POWER.md` | Steering que indica al agente qué herramientas MCP tiene y cuándo usarlas |
| MCP server config | Herramientas y detalles de conexión del servidor MCP |
| Steering/hooks | Tareas automatizadas en eventos del IDE o via slash commands (opcional) |

---

---

## Ecosistema de Powers

### Partners de lanzamiento
- **Datadog** — Monitoring y observabilidad
- **Dynatrace** — Performance y AIOps
- **Figma** — Diseño UI conectado a código
- **Neon** — PostgreSQL serverless
- **Netlify** — Deployment y hosting
- **Postman** — Testing de APIs
- **Supabase** — Backend-as-a-Service
- **Stripe** — Pagos y billing
- **Strands SDK** — Construcción de agentes AI
- **AWS Aurora** — Base de datos relacional

### Fuentes de Powers
- Browse curado en [kiro.dev/powers](https://kiro.dev/powers)
- Instalación desde GitHub URLs
- Creación y compartición de Powers propios

---

## Instalación

**One-click:** Navegar a kiro.dev/powers o al panel de Powers en el IDE, encontrar el Power deseado y hacer clic en "Install". Se registra automáticamente sin archivos JSON manuales ni configuración por línea de comandos.

**Desde GitHub:** Instalar Powers de la comunidad proporcionando la URL del repositorio.

---

## Crear un Power Propio

Los Powers son extensibles. Se pueden crear Powers personalizados que empaqueten:
- Herramientas MCP internas de la empresa
- Workflows y best practices del equipo
- Integraciones con servicios propietarios

Y compartirlos con la comunidad o dentro de la organización.

---

## Comparativa: Skills vs Powers

| Aspecto | Skills | Powers |
|---------|--------|--------|
| Estándar | Abierto (agentskills.io) | Específico de Kiro |
| Activación | On-demand por descripción del skill | Dinámico por keywords en la conversación |
| Contenido | Instrucciones, scripts, templates, docs | MCP tools + POWER.md + hooks opcionales |
| Herramientas externas | No incluye (solo instrucciones) | Sí (servidores MCP integrados) |
| Portabilidad | Alta (compatible con otros AI tools) | Solo Kiro |
| Instalación | Importar desde GitHub o carpeta local | One-click desde kiro.dev/powers |
| Mejor para | Workflows reutilizables y compartibles | Integraciones con servicios externos + guidance |

---

*Fuente: [kiro.dev/docs/powers](https://kiro.dev/docs/powers/) — Contenido adaptado de la documentación oficial.*
