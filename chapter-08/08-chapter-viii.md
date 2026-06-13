# Capítulo VIII: Experiment Design

## 8.1. Experiment Planning

### 8.1.1. As-Is Summary

*Contenido pendiente.*

### 8.1.2. Raw Material: Assumptions, Knowledge Gaps, Ideas, Claims

*Contenido pendiente.*

### 8.1.3. Experiment-Ready Questions

*Contenido pendiente.*

### 8.1.4. Question Backlog

*Contenido pendiente.*

### 8.1.5. Experiment Cards

*Contenido pendiente.*

## 8.2. Experiment Design

### 8.2.1. Hypotheses

*Contenido pendiente.*

### 8.2.2. Domain Business Metrics

*Contenido pendiente.*

### 8.2.3. Measures

*Contenido pendiente.*

### 8.2.4. Conditions

*Contenido pendiente.*

### 8.2.5. Scale Calculations and Decisions

*Contenido pendiente.*

### 8.2.6. Methods Selection

*Contenido pendiente.*

### 8.2.7. Data Analytics: Goals, KPIs and Metrics Selection

*Contenido pendiente.*

### 8.2.8. Web and Mobile Tracking Plan

*Contenido pendiente.*

## 8.3. Experimentation

El experimento de Budgetly busca validar si la estrategia **IncomeBased** mejora la experiencia de reparto de gastos del hogar frente al método manual que describieron los entrevistados del Capítulo II (Excel, acuerdos verbales, división informal).

### Diseño del experimento

**Tipo:** experimento A/B entre sujetos.

**Grupos:**

- **Grupo A (control):** reparto manual con calculadora, papel o Excel, sin usar Budgetly.
- **Grupo B (tratamiento):** Budgetly con ingresos registrados y estrategia IncomeBased activa.

**Escenario de tarea:** un hogar de 3 miembros debe repartir un gasto compartido de **S/ 1 200**. Ingresos mensuales: Miembro 1 = S/ 2 500, Miembro 2 = S/ 4 000, Miembro 3 = S/ 6 000 (total S/ 12 500). Con IncomeBased, el resultado esperado es **S/ 240 / S/ 384 / S/ 576**.

**Parámetros:**

<table border="1">
<tr><th>Parámetro</th><th>Valor</th></tr>
<tr><td>Tamaño muestral (N)</td><td>24 (12 por grupo)</td></tr>
<tr><td>Duración</td><td>2 semanas</td></tr>
<tr><td>Nivel de significancia (α)</td><td>0,05</td></tr>
<tr><td>URL aplicación</td><td>https://budgetly-exp-app.web.app/</td></tr>
</table>

### Hipótesis

**H₀ (nula):** No existe diferencia significativa entre el reparto manual (Grupo A) y Budgetly con IncomeBased (Grupo B) en equidad percibida (VD1), tiempo de resolución (VD2), confianza en el resultado (VD3) ni intención de uso (VD4).

**H₁ (alternativa):** El Grupo B presenta mayor equidad percibida (VD1) y confianza (VD3), menor tiempo de resolución (VD2) y mayor intención de uso (VD4) que el Grupo A, con α = 0,05.

### Matriz de variables

<table border="1">
<tr><th>Tipo</th><th>Variable</th><th>Descripción</th><th>Operacionalización</th></tr>
<tr><td>VI</td><td>Método de reparto</td><td>Manual vs. Budgetly IncomeBased</td><td>Asignación aleatoria A/B</td></tr>
<tr><td>VD1</td><td>Equidad percibida</td><td>Sensación de justicia del reparto</td><td>Likert 1–5 post-tarea</td></tr>
<tr><td>VD2</td><td>Tiempo de resolución</td><td>Segundos hasta declarar montos finales</td><td>Cronómetro del moderador</td></tr>
<tr><td>VD3</td><td>Confianza</td><td>Creencia en corrección de los montos</td><td>Likert 1–5 post-tarea</td></tr>
<tr><td>VD4</td><td>Intención de uso</td><td>Disposición a usar la solución en el hogar</td><td>Likert 1–5 post-tarea</td></tr>
<tr><td>Control</td><td>Escenario financiero</td><td>Mismos montos para todos</td><td>Script único de tarea</td></tr>
<tr><td>Control</td><td>Instrucciones</td><td>Mismo guion de moderador</td><td>Checklist de sesión</td></tr>
<tr><td>Control</td><td>Entorno</td><td>Sesión remota sincrónica</td><td>Google Meet + mismo enlace web</td></tr>
<tr><td>VC-01</td><td>Familiaridad con Excel</td><td>Experiencia previa con hojas de cálculo</td><td>Pregunta pre-tarea; análisis por subgrupo</td></tr>
<tr><td>VC-02</td><td>Nivel de ingresos del hogar</td><td>Contexto socioeconómico del participante</td><td>Estratificación en reclutamiento</td></tr>
<tr><td>VC-03</td><td>Sesgo de novedad</td><td>Entusiasmo por probar una app nueva</td><td>Ítem pre-tarea; mismo guion introductorio en ambos grupos</td></tr>
</table>

**Mitigación de variables de confusión:**

- **VC-01:** registrar nivel de uso de Excel; si hay desbalance entre grupos, reportar análisis de sensibilidad.
- **VC-02:** balancear reclutamiento entre perfiles similares a los segmentos del Cap. II.
- **VC-03:** no enfatizar que Budgetly es un producto "nuevo"; medir el sesgo antes de la tarea y contrastarlo con VD4.

### Procedimiento

1. Reclutar 24 participantes y asignarlos aleatoriamente a A o B.
2. Aplicar cuestionario previo (VC-01, VC-02, VC-03).
3. Explicar la tarea con guion estándar, sin revelar la hipótesis.
4. Ejecutar la tarea y cronometrar hasta que el participante declare los montos finales.
5. Aplicar encuesta post-tarea (VD1–VD4).
6. Registrar datos y contrastar H₀ vs. H₁ con prueba t independiente (α = 0,05).

### Sustento en evidencia previa

- **Capítulo II:** la mayoría de entrevistados reportó desacuerdos en pagos y necesidad de una herramienta que organice los gastos del hogar.
- **Capítulo VI:** pruebas unitarias y BDD de IncomeBased confirman el reparto S/ 240 / S/ 384 / S/ 576 para el escenario del experimento.
- **Capítulo VII:** el despliegue en Firebase permite usar la misma versión de la app en todas las sesiones del Grupo B.

Las historias de usuario y el backlog To-Be que materializan el tratamiento del Grupo B se documentan en **8.3.1** y **8.3.2**.

## 8.3.1. To-Be User Stories

Historias de usuario del estado objetivo (To-Be), alineadas al experimento de la sección 8.3 y al producto Budgetly desplegado.

---

### US-TB-01 — Registrar ingresos de miembros del hogar

**Épica:** EP03 — Panel del Miembro del Hogar  
**Historia:** Como representante del hogar, quiero registrar el ingreso mensual de cada miembro para que Budgetly calcule contribuciones proporcionales automáticamente.  
**Valor de negocio:** Sin ingresos registrados no se puede activar IncomeBased; es prerrequisito del Grupo B en el experimento.  
**Relación Cap. III:** extiende US11 (Ingresar ingresos personales).

**Criterios INVEST:**

<table border="1">
<tr><th>Letra</th><th>Criterio</th><th>Cumplimiento</th></tr>
<tr><td>I</td><td>Independiente</td><td>Se implementa sin US-TB-02; solo requiere miembros del hogar creados</td></tr>
<tr><td>N</td><td>Negociable</td><td>El formulario puede ser modal o página; el dato obligatorio es monto &gt; 0</td></tr>
<tr><td>V</td><td>Valiosa</td><td>Habilita reparto justo según ingresos (María Fernanda, Ronald — Cap. II)</td></tr>
<tr><td>E</td><td>Estimable</td><td>5 story points; similar a US11 del Cap. III</td></tr>
<tr><td>S</td><td>Pequeña</td><td>Formulario + validación + persistencia en API en un sprint</td></tr>
<tr><td>T</td><td>Testeable</td><td>BDD con ruta feliz, triste y alternativa</td></tr>
</table>

**Escenarios BDD:**

**Ruta feliz — Registrar ingreso por primera vez**

- **Dado** que soy representante y existe el miembro "Ana" sin ingreso registrado  
- **Cuando** ingreso S/ 2 500 en "Ingreso mensual" y guardo  
- **Entonces** el sistema muestra "Ingreso registrado" y Ana aparece con S/ 2 500 en la lista de miembros

**Ruta triste — Monto inválido**

- **Dado** que estoy en el formulario de ingreso de un miembro  
- **Cuando** ingreso "-100" o dejo el campo vacío y guardo  
- **Entonces** el sistema muestra error "Ingrese un monto válido mayor a cero" y no guarda

**Ruta alternativa — Actualizar ingreso existente**

- **Dado** que el miembro "Ana" ya tiene ingreso S/ 2 500 registrado  
- **Cuando** cambio el monto a S/ 3 000 y guardo  
- **Entonces** el sistema actualiza a S/ 3 000 y recalcula las contribuciones pendientes según IncomeBased

---

### US-TB-02 — Configurar estrategia IncomeBased en el hogar

**Épica:** EP02 — Panel del Representante del Hogar  
**Historia:** Como representante, quiero seleccionar la estrategia IncomeBased para que los gastos compartidos se repartan según el porcentaje de ingreso de cada miembro.  
**Valor de negocio:** Es el tratamiento central del experimento (Grupo B).  
**Relación Cap. III:** extiende US08 (Ajustar porcentajes de aportes).

**Criterios INVEST:**

<table border="1">
<tr><th>Letra</th><th>Criterio</th><th>Cumplimiento</th></tr>
<tr><td>I</td><td>Independiente</td><td>Depende de US-TB-01; no requiere gastos registrados aún</td></tr>
<tr><td>N</td><td>Negociable</td><td>Puede mostrarse como selector en configuración del hogar</td></tr>
<tr><td>V</td><td>Valiosa</td><td>Automatiza el reparto proporcional que hoy hacen manualmente en Excel</td></tr>
<tr><td>E</td><td>Estimable</td><td>3 story points; lógica IncomeBased ya probada en Cap. VI</td></tr>
<tr><td>S</td><td>Pequeña</td><td>Selector + llamada API + confirmación visual</td></tr>
<tr><td>T</td><td>Testeable</td><td>BDD verifica activación exitosa y bloqueo sin ingresos</td></tr>
</table>

**Escenarios BDD:**

**Ruta feliz — Activar IncomeBased**

- **Dado** que todos los miembros tienen ingreso registrado  
- **Cuando** selecciono "Reparto según ingresos (IncomeBased)" y confirmo  
- **Entonces** el hogar queda en modo IncomeBased y se muestra el porcentaje de cada miembro

**Ruta triste — Activar sin ingresos completos**

- **Dado** que al menos un miembro no tiene ingreso registrado  
- **Cuando** intento activar IncomeBased  
- **Entonces** el sistema bloquea la acción y muestra "Registre el ingreso de todos los miembros"

**Ruta alternativa — Cambiar de manual a IncomeBased con gastos existentes**

- **Dado** que el hogar tenía reparto manual y ya existe un gasto compartido de S/ 1 200  
- **Cuando** activo IncomeBased  
- **Entonces** el sistema recalcula las contribuciones del gasto pendiente según los ingresos actuales sin borrar el historial

---

### US-TB-03 — Visualizar desglose de contribución por gasto

**Épica:** EP04 — Gestión de Gastos Compartidos  
**Historia:** Como miembro del hogar, quiero ver cuánto me corresponde pagar de cada gasto compartido para entender el reparto sin recalcular manualmente.  
**Valor de negocio:** Soporta VD1 (equidad percibida) y VD3 (confianza) del experimento.  
**Relación Cap. III:** extiende US12 (Ver monto a pagar) y US16 (Registrar nuevo gasto).

**Criterios INVEST:**

<table border="1">
<tr><th>Letra</th><th>Criterio</th><th>Cumplimiento</th></tr>
<tr><td>I</td><td>Independiente</td><td>Requiere al menos un gasto creado; puede probarse con datos de prueba</td></tr>
<tr><td>N</td><td>Negociable</td><td>Tabla o tarjetas; lo esencial es el monto por miembro</td></tr>
<tr><td>V</td><td>Valiosa</td><td>Resuelve la confusión sobre quién pagó qué (Abraham, María Fernanda — Cap. II)</td></tr>
<tr><td>E</td><td>Estimable</td><td>5 story points</td></tr>
<tr><td>S</td><td>Pequeña</td><td>Vista de detalle de contribuciones en el frontend</td></tr>
<tr><td>T</td><td>Testeable</td><td>Comparar montos con pruebas unitarias IncomeBased del Cap. VI</td></tr>
</table>

**Escenarios BDD:**

**Ruta feliz — Ver desglose de gasto compartido**

- **Dado** un gasto "Alquiler" de S/ 1 200 con IncomeBased activo e ingresos S/ 2 500 / S/ 4 000 / S/ 6 000  
- **Cuando** abro el detalle del gasto  
- **Entonces** veo S/ 240, S/ 384 y S/ 576 para cada miembro y la suma total S/ 1 200

**Ruta triste — Gasto sin estrategia definida**

- **Dado** un gasto creado antes de configurar la estrategia de reparto  
- **Cuando** abro el detalle  
- **Entonces** el sistema muestra "Configure la estrategia de reparto del hogar"

**Ruta alternativa — Filtrar desglose por miembro**

- **Dado** que soy miembro "Luis" y existen varios gastos compartidos  
- **Cuando** filtro contribuciones por "Mis pendientes"  
- **Entonces** solo veo los montos que me corresponden, ordenados por fecha

---

### US-TB-04 — Marcar contribución como pagada

**Épica:** EP04 — Gestión de Gastos Compartidos  
**Historia:** Como representante, quiero marcar cuando un miembro pagó su parte para llevar trazabilidad y evitar desacuerdos por pagos duplicados u olvidados.  
**Valor de negocio:** Cierra el ciclo de trazabilidad que los entrevistados pidieron en el Cap. II.  
**Relación Cap. III:** complementa US14 (Ver historial de pagos).

**Criterios INVEST:**

<table border="1">
<tr><th>Letra</th><th>Criterio</th><th>Cumplimiento</th></tr>
<tr><td>I</td><td>Independiente</td><td>Requiere contribución generada (US-TB-03); no depende de pagos en línea</td></tr>
<tr><td>N</td><td>Negociable</td><td>Checkbox o botón "Marcar pagado"; sin integración bancaria en el MVP</td></tr>
<tr><td>V</td><td>Valiosa</td><td>Reduce conflictos por "quién pagó qué"</td></tr>
<tr><td>E</td><td>Estimable</td><td>3 story points</td></tr>
<tr><td>S</td><td>Pequeña</td><td>Actualización de estado en API + reflejo en UI</td></tr>
<tr><td>T</td><td>Testeable</td><td>Estado Pendiente → Pagado verificable en API y UI</td></tr>
</table>

**Escenarios BDD:**

**Ruta feliz — Marcar pago completo**

- **Dado** que la contribución de "Ana" por S/ 240 está pendiente  
- **Cuando** marco la contribución como "Pagada"  
- **Entonces** el estado cambia a Pagada y el dashboard muestra el avance del gasto

**Ruta triste — Marcar sin permisos**

- **Dado** que soy miembro regular sin rol de representante  
- **Cuando** intento marcar la contribución de otro miembro  
- **Entonces** el sistema deniega la acción y muestra "Solo el representante puede confirmar pagos"

**Ruta alternativa — Revertir marcado por error**

- **Dado** que una contribución fue marcada como Pagada por error  
- **Cuando** el representante selecciona "Revertir a pendiente" y confirma  
- **Entonces** el estado vuelve a Pendiente y el progreso del gasto se actualiza

---

**Resumen de historias To-Be:**

<table border="1">
<tr><th>ID</th><th>Título</th><th>Épica</th><th>Puntos</th><th>US Cap. III</th></tr>
<tr><td>US-TB-01</td><td>Registrar ingresos</td><td>EP03</td><td>5</td><td>US11</td></tr>
<tr><td>US-TB-02</td><td>Activar IncomeBased</td><td>EP02</td><td>3</td><td>US08</td></tr>
<tr><td>US-TB-03</td><td>Ver desglose</td><td>EP04</td><td>5</td><td>US12, US16</td></tr>
<tr><td>US-TB-04</td><td>Marcar pagado</td><td>EP04</td><td>3</td><td>US14</td></tr>
</table>

## 8.3.2. To-Be Product Backlog

Ítems del backlog To-Be vinculados a las historias de 8.3.1 y al experimento de 8.3. Incluye ítems **Funcional**, **Operación** y **Test**.

<table border="1">
<tr><th>ID</th><th>Tipo</th><th>Descripción</th><th>US To-Be</th><th>Prioridad</th><th>SP</th></tr>
<tr><td>PB-TB-001</td><td>Funcional</td><td>Formulario de ingreso mensual por miembro</td><td>US-TB-01</td><td>Alta</td><td>5</td></tr>
<tr><td>PB-TB-002</td><td>Funcional</td><td>Validación de monto &gt; 0 en ingresos</td><td>US-TB-01</td><td>Alta</td><td>2</td></tr>
<tr><td>PB-TB-003</td><td>Funcional</td><td>Actualizar ingreso existente y recalcular contribuciones</td><td>US-TB-01</td><td>Alta</td><td>3</td></tr>
<tr><td>PB-TB-004</td><td>Funcional</td><td>Selector de estrategia IncomeBased en configuración del hogar</td><td>US-TB-02</td><td>Alta</td><td>3</td></tr>
<tr><td>PB-TB-005</td><td>Funcional</td><td>Bloqueo de IncomeBased si faltan ingresos de algún miembro</td><td>US-TB-02</td><td>Alta</td><td>2</td></tr>
<tr><td>PB-TB-006</td><td>Funcional</td><td>Recalcular contribuciones al cambiar de manual a IncomeBased</td><td>US-TB-02</td><td>Media</td><td>3</td></tr>
<tr><td>PB-TB-007</td><td>Funcional</td><td>Vista de desglose por miembro en detalle de gasto</td><td>US-TB-03</td><td>Alta</td><td>5</td></tr>
<tr><td>PB-TB-008</td><td>Funcional</td><td>Filtro "Mis contribuciones pendientes" por miembro</td><td>US-TB-03</td><td>Media</td><td>3</td></tr>
<tr><td>PB-TB-009</td><td>Funcional</td><td>Marcar contribución como pagada</td><td>US-TB-04</td><td>Alta</td><td>3</td></tr>
<tr><td>PB-TB-010</td><td>Funcional</td><td>Revertir estado pagado → pendiente</td><td>US-TB-04</td><td>Media</td><td>2</td></tr>
<tr><td>PB-TB-011</td><td>Operación</td><td>Guion de moderador para sesiones A/B del experimento</td><td>—</td><td>Alta</td><td>2</td></tr>
<tr><td>PB-TB-012</td><td>Operación</td><td>Plantilla de encuesta post-tarea (VD1–VD4) y pre-tarea (VC-01 a VC-03)</td><td>—</td><td>Alta</td><td>1</td></tr>
<tr><td>PB-TB-013</td><td>Operación</td><td>Ejecución del experimento formal con N = 24 participantes</td><td>—</td><td>Alta</td><td>8</td></tr>
<tr><td>PB-TB-014</td><td>Test</td><td>Escenarios BDD SpecFlow para US-TB-01 a US-TB-04 (feliz, triste, alternativa)</td><td>US-TB-01–04</td><td>Alta</td><td>5</td></tr>
<tr><td>PB-TB-015</td><td>Test</td><td>Pruebas de regresión IncomeBased tras actualización de ingresos</td><td>US-TB-01</td><td>Media</td><td>3</td></tr>
<tr><td>PB-TB-016</td><td>Test</td><td>Validar montos S/ 240 / S/ 384 / S/ 576 del escenario del experimento</td><td>US-TB-03</td><td>Alta</td><td>2</td></tr>
</table>

## 8.3.3. Pipeline-supported, Experiment-Driven To-Be Software Platform Lifecycle

### 8.3.3.1. To-Be Sprint Backlogs

*Contenido pendiente.*

### 8.3.3.2. Implemented To-Be Landing Page Evidence

*Contenido pendiente.*

### 8.3.3.3. Implemented To-Be Frontend-Web Application Evidence

*Contenido pendiente.*

### 8.3.3.4. Implemented To-Be Native-Mobile Application Evidence

*Contenido pendiente.*

### 8.3.3.5. Implemented To-Be RESTful API and/or Serverless Backend Evidence

*Contenido pendiente.*

### 8.3.3.6. Team Collaboration Insights

*Contenido pendiente.*

## 8.3.4. To-Be Validation Interviews

### 8.3.4.1. Diseño de Entrevistas

*Contenido pendiente.*

### 8.3.4.2. Registro de Entrevistas

*Contenido pendiente.*

## 8.4. Experiment Aftermath & Analysis

### 8.4.1. Analysis and Interpretation of Results

*Contenido pendiente.*

### 8.4.2. Re-scored and Re-prioritized Question Backlog

*Contenido pendiente.*

## 8.5. Continuous Learning

### 8.5.1. Shareback Session Artifacts: Learning Workflow

*Contenido pendiente.*

## 8.6. To-Be Software Platform Pre-launch

### 8.6.1. About-the-Product Intro Video

*Contenido pendiente.*
