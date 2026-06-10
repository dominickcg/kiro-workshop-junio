# Planes y Billing

Kiro utiliza un sistema basado en créditos que permite a los usuarios elegir el plan adecuado según su volumen de uso y necesidades de modelos.

## Planes Individuales

| Plan | Precio | Créditos/mes | Modelos Incluidos | Overage |
|------|--------|-------------|-------------------|---------|
| **Free** | $0/mes | 50 | Open weight* + Claude Sonnet 4.5 | — |
| **Pro** | $20/mes | 1,000 | Todos (premium**) | $0.04/crédito |
| **Pro+** | $40/mes | 2,000 | Todos (premium**) | $0.04/crédito |
| **Power** | $200/mes | 10,000 | Todos (premium**) | $0.04/crédito |

> *Free Tier con social login o AWS Builder ID: acceso a Claude Sonnet 4.5, Qwen3 Coder Next, DeepSeek v3.2 y MiniMax 2.1.

> **Planes pagos: acceso a modelos premium incluyendo Auto, Claude Sonnet 4.6 y Claude Opus 4.6.

### Bono de registro
$20 acreditados hacia tu suscripción cuando actualizas por primera vez a un plan pago usando social login o Builder ID.

---

## Sistema de Créditos

### ¿Qué es un crédito?

Un crédito es la unidad de consumo de Kiro. Cada interacción con el agente consume una cantidad de créditos proporcional al modelo usado y la complejidad de la tarea.

### Multiplicadores de costo por modelo

El costo de cada tarea se calcula relativo a Auto (1.0x baseline):

| Modelo | Multiplicador |
|--------|--------------|
| Claude Opus 4.8 / 4.7 / 4.6 / 4.5 | 2.2x |
| Claude Sonnet 4.6 / 4.5 / 4.0 | 1.3x |
| Auto (baseline) | 1.0x |
| GLM-5 | 0.5x |
| Claude Haiku 4.5 | 0.4x |
| DeepSeek 3.2 / MiniMax M2.5 | 0.25x |
| MiniMax M2.1 | 0.15x |
| Qwen3 Coder Next | 0.05x |

**Ejemplo práctico:** Una tarea que cuesta 10 créditos en Auto, costaría:
- 22 créditos en Opus
- 13 créditos en Sonnet
- 4 créditos en Haiku
- 0.5 créditos en Qwen3 Coder Next

### Visibilidad del consumo

Kiro muestra el consumo de créditos por prompt en tiempo real, permitiendo control constante del gasto.

### Seguimiento

El uso de créditos se puede rastrear desde la configuración de la cuenta en [app.kiro.dev/settings/account](https://app.kiro.dev/settings/account).

---

## Overage (Excedente)

En planes pagos, si se agotan los créditos mensuales, se puede continuar usando Kiro con cargo de **$0.04 por crédito adicional** (pay-per-use). No hay interrupción del servicio.

---

## Créditos No Usados

Los créditos mensuales **no se acumulan**. Si no usas todos tus créditos en un mes, no se transfieren al siguiente.

---

## Uso Compartido

La suscripción individual no se puede compartir con un equipo. Para equipos, Kiro ofrece planes Enterprise con facturación centralizada, SSO, analytics de uso y controles de seguridad empresarial.

---

## Con Qué Herramientas Se Puede Usar

La suscripción de Kiro cubre el uso en las tres superficies:
- **Kiro IDE** (escritorio)
- **Kiro CLI** (terminal)
- **Kiro Web** (navegador, preview incluido sin cargo extra)

---

## Planes Enterprise

Para equipos y organizaciones:
- Facturación centralizada
- Single Sign-On (SSO)
- Analytics de uso
- Controles de seguridad empresarial

Contacto: [pages.awscloud.com/kiro-contact-us](https://pages.awscloud.com/kiro-contact-us.html)

---

## Estrategias de Optimización de Créditos

1. **Usar Auto para la mayoría del trabajo** — Optimiza calidad y costo automáticamente
2. **Reservar Opus para problemas complejos** — Donde la profundidad de análisis justifica el costo
3. **Usar Haiku o open weight para iteraciones rápidas** — Fixes simples, exploraciones, sub-tareas
4. **Considerar el plan según modelo principal:**
   - Si usas principalmente Auto/Sonnet → Pro suele ser suficiente
   - Si usas frecuentemente Opus → Pro+ o Power para más créditos

---

## Disponibilidad Regional

Los planes individuales están disponibles en la mayoría de países y regiones. Consultar la documentación oficial para la lista completa de países soportados.

---

*Fuente: [kiro.dev/pricing](https://kiro.dev/pricing/) — Contenido adaptado de la documentación oficial.*
