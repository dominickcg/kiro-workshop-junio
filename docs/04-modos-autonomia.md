# Modos de Autonomía

Kiro ofrece dos modos de operación que determinan cuánto control ejerce el desarrollador sobre las acciones del agente. Ambos están disponibles en sesiones Vibe (chat conversacional) y Spec.

## Autopilot (por defecto)

El agente trabaja de forma autónoma de principio a fin. Puede crear archivos, modificar código en múltiples ubicaciones, ejecutar comandos y tomar decisiones arquitectónicas sin pedir aprobación en cada paso.

**Controles disponibles:**
- **Ver todos los cambios** — Lista completa de modificaciones en vista diff (qué se agregó, modificó o eliminó)
- **Revertir todo** — Restaura los archivos a su estado anterior localmente
- **Interrumpir ejecución** — Detiene al agente a mitad de ejecución para retomar control manual
- **Checkpoints** — Puntos de restauración que revierten tanto cambios de archivos como adiciones de contexto

**Cuándo usar Autopilot:**
- Usuarios experimentados con Kiro
- Tareas repetitivas o bien definidas
- Proyectos donde se quiere avanzar rápido
- Tareas que abarcan múltiples archivos o requieren varios pasos

---

## Supervised (Supervisado)

El agente se detiene después de cada turno que contenga ediciones de archivos. Los cambios se presentan como "hunks" individuales (grupos lógicos de líneas relacionadas), dando control granular sobre cada modificación.

**Controles disponibles:**
- **Revisión por archivo** — Revisar y aceptar/rechazar cambios archivo por archivo
- **Revisión por hunk** — Cada grupo de cambios se puede:
  - **Aceptar** — Aplicar ese hunk específico
  - **Rechazar** — Descartar ese hunk manteniendo otros
  - **Chat inline** — Iniciar una conversación sobre ese hunk para pedir ajustes
- **Aprobación selectiva** — Aceptar algunos hunks y rechazar otros dentro del mismo archivo
- **Accept All / Reject All** — Aplicar o revertir todos los cambios pendientes de una vez

**Cuándo usar Supervised:**
- Nuevos usuarios familiarizándose con Kiro
- Codebases críticas o sensibles
- Para aprender cómo Kiro aborda los problemas
- Cuando se quiere revisar cuidadosamente cada cambio
- Trabajando con código desconocido o sistemas complejos

---

## Alternar Entre Modos

Se puede cambiar entre Autopilot y Supervised en cualquier momento usando el switch en la interfaz del chat. Esto permite usar el nivel de control apropiado para diferentes tareas dentro de la misma sesión.

---

## Seguridad en Cada Modo

Ambos modos mantienen seguridad enterprise-grade:
- El código no se usa para entrenar modelos
- Control sobre qué datos se comparten
- Archivos sensibles pueden protegerse de modificaciones del agente

La diferencia está en el **momento** del control: Autopilot confía en revisión post-ejecución; Supervised enforce revisión pre-aplicación.

---

---

*Fuente: [kiro.dev/docs/chat/autopilot](https://kiro.dev/docs/chat/autopilot/) — Contenido adaptado de la documentación oficial.*
