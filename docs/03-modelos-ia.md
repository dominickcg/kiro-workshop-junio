# Modelos de IA Disponibles

Kiro ofrece acceso a modelos de IA frontier y open weight, permitiendo elegir el modelo adecuado para cada tarea o dejar que Kiro decida automáticamente con el modo Auto.

## Tabla Comparativa

| Modelo | Contexto | Costo Relativo | Mejor Para |
|--------|----------|----------------|------------|
| Claude Opus 4.8 | 1M tokens | 2.2x | Máxima honestidad, auto-verificación |
| Claude Opus 4.7 | 1M tokens | 2.2x | Pensamiento adaptativo, visión de alta resolución |
| Claude Opus 4.6 | 1M tokens | 2.2x | Sesiones largas, debugging, code review |
| Claude Opus 4.5 | 200K tokens | 2.2x | Arquitectura cross-system, precisión single-shot |
| Claude Sonnet 4.6 | 1M tokens | 1.3x | Casi-Opus a costo de Sonnet |
| Claude Sonnet 4.5 | 200K tokens | 1.3x | Coding agéntico autónomo extendido |
| Claude Sonnet 4.0 | 200K tokens | 1.3x | Baseline predecible, sin capas de routing |
| Auto (recomendado) | — | 1.0x | Desarrollo general, balancea calidad y costo |
| Claude Haiku 4.5 | 200K tokens | 0.4x | Iteraciones rápidas, ahorro de créditos |
| MiniMax M2.5 | 200K tokens | 0.25x | Resultados frontier a bajo costo |
| GLM-5 | 200K tokens | 0.5x | Trabajo agéntico a escala de repo |
| DeepSeek 3.2 | 128K tokens | 0.25x | Workflows agénticos a bajo costo |
| MiniMax M2.1 | 200K tokens | 0.15x | Programación multilenguaje |
| Qwen3 Coder Next | 256K tokens | 0.05x | Sesiones largas con presupuesto mínimo |

> El costo es relativo a Auto (1.0x baseline). Una tarea de 10 créditos en Auto costaría 22 en Opus, 4 en Haiku o 0.5 en Qwen3 Coder Next.

---

## Cómo Elegir el Modelo Correcto

### Para desarrollo general → Auto
Rutea cada tarea al modelo óptimo automáticamente. La mejor relación calidad/costo sin pensar.

### Para problemas complejos → Opus
Planifica con mayor profundidad, considera edge cases, se auto-corrige. Mantiene el foco en sesiones largas.

### Para iteraciones rápidas → Haiku
Inteligencia near-frontier a una fracción del costo. Ideal para fixes rápidos y sub-agentes.

### Para presupuesto ajustado → Qwen3 Coder Next / DeepSeek 3.2
Modelos open weight con buen rendimiento en coding a costos mínimos.

---

## Diferencias de Comportamiento Entre Modelos

**Profundidad de planificación:** Los modelos Opus planifican más a fondo antes de actuar. Sonnet y Haiku son más directos y comienzan a ejecutar antes.

**Pensamiento adaptativo (Opus 4.7+):** Escala automáticamente la profundidad del razonamiento según la complejidad de la tarea. Preguntas simples obtienen respuestas rápidas; problemas arquitectónicos complejos obtienen análisis más profundo.

**Auto-corrección:** Todos los Opus detectan sus propios errores. Opus 4.8 va más lejos: señala activamente incertidumbres en lugar de afirmar progreso con confianza falsa.

**Resistencia de sesión:** Para tareas long-running (como trabajar un spec completo), Opus mantiene mejor el foco. Haiku y Sonnet funcionan mejor para interacciones cortas y enfocadas.

**Nivel de iniciativa:** Opus tiende a tomar más iniciativa y hacer cambios más amplios. Sonnet es más conservador y se ciñe a lo solicitado.

---

## Ciclo de Vida de los Modelos

| Etapa | Descripción |
|-------|-------------|
| Experimental | Disponible para testing temprano, puede cambiar. Disponibilidad regional limitada. |
| Active | Completamente soportado y recomendado para uso en producción. |

---

## Cómo Cambiar de Modelo

Usar el dropdown de modelo en la interfaz del chat. La selección aplica a todos los mensajes subsiguientes en la conversación.

---

## Disponibilidad Regional

Los modelos están disponibles en `us-east-1` y `eu-central-1`. Algunos modelos experimentales solo están en `us-east-1`. La disponibilidad puede variar por país o región según los requerimientos del proveedor.

---

*Fuente: [kiro.dev/docs/models](https://kiro.dev/docs/chat/model-selection/) — Contenido adaptado de la documentación oficial.*
