Prompt
Quiero construir un dashboard financiero de demostración — una app web que muestra el estado de una cuenta ficticia. Debe verse profesional y funcionar de punta a punta sin base de datos real ni integraciones bancarias.

Lo que muestra el dashboard
Panel de saldo — Balance actual, ingresos y gastos del mes en curso, y variación porcentual vs el mes anterior con indicador visual (↑/↓/→). Si la variación del mes anterior es cero, mostrar "flat" y 0%.

4 tarjetas KPI — Total enviado (suma de gastos), total recibido (suma de ingresos), cantidad de transacciones, y ticket promedio (suma total / cantidad, o 0 si no hay transacciones).

Tabla de transacciones — Columnas: fecha, contraparte, tipo, monto, moneda, estado. Filtros combinables con lógica AND: tipo (income/expense), rango de fechas (inclusivo), búsqueda por contraparte (case-insensitive, desde 1 carácter). Paginación con default 20, máximo 100. La tabla distingue "sin resultados para estos filtros" de "error al cargar".

Gráfico de flujo de caja (Recharts) — Barras diarias de ingresos vs gastos. Selector de periodo: 7, 14, 30 días con botones tab-style. El periodo default es 7 días. Si se cambia de periodo durante una carga, la petición anterior se cancela con AbortController. Los días sin transacciones muestran 0.

Modal de detalle de transacción — Click en una fila abre un diálogo accesible: focus trap, cierre con Escape/backdrop/botón X, aria-modal, aria-labelledby, inert en background, foco restaurado al cerrar (con fallback al contenedor de la tabla si la fila ya no existe). Muestra todos los campos formateados con badges de color.

Toggle tema oscuro/claro — Botón sol/luna en el header. Persiste en localStorage (key "theme"). Fallback a prefers-color-scheme del OS. Si localStorage falla, funciona solo para la sesión. Transición de 150ms.

Exportar CSV — Botón "Exportar CSV" en la tabla. Descarga las transacciones filtradas actuales (limit=100). Header: fecha,contraparte,tipo,monto,moneda,estado. Montos sin símbolo ni separadores (ej: 1234.50). Tipos/estados traducidos a español. Escaping RFC 4180. UTF-8 con BOM. Archivo nombrado transacciones_YYYY-MM-DD.csv. Si no hay datos muestra mensaje, no descarga.

Servicio de datos (Python)
Al arrancar genera las transacciones en memoria (una sola vez, no mutan).
Cada transacción: UUID v4, fecha ISO8601 últimos 30 días, contraparte 1-100 chars, tipo income/expense, monto 0.01–999,999.99 (2 decimales), moneda USD/PEN/EUR, status completed/pending/failed.
Si una transacción generada no pasa validación Pydantic, se descarta y se reintenta.
Endpoints: GET /balance, GET /kpis, GET /transactions (type, startDate, endDate, search, page, limit), GET /cashflow?days=N.
Balance: balance = income - expenses total. Variación = (net_mes_actual - net_mes_anterior) / |net_mes_anterior| * 100, redondeado a 1 decimal.
API Gateway
GET /api/health resuelve localmente, probe al Data Service con timeout de 2s para decidir "ok" vs "degraded". Siempre devuelve 200 con status + uptime.
El health route se registra PRIMERO para garantizar precedencia sobre las rutas proxy.
Endpoints proxy: /api/balance, /api/kpis, /api/transactions, /api/cashflow.
E2E y carga
Playwright: dashboard carga y muestra balance formateado en <10s, filtro tipo muestra solo filas del tipo, búsqueda filtra por contraparte, gráfico tiene al menos una serie renderizada. Tests independientes con timeout de 30s cada uno.
K6: requests concurrentes a /api/transactions, /api/kpis, /api/cashflow. Rampa 1→50 VUs en 10s, sostener 30s, bajar en 5s. Thresholds: p95 < 500ms, error rate < 1%. Reporte con p95, error rate %, y total de requests.