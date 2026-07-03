# Capítulo VIII: Experiment-Driven Development

## 8.1. Experiment Planning

### 8.1.1. As-Is Summary

Budgetly es una plataforma SaaS de gestión financiera colaborativa orientada a hogares con ingresos desiguales. Actualmente, el sistema permite a los usuarios registrarse bajo dos roles diferenciados —**Representante del Hogar** y **Miembro del Hogar**— y ofrece funcionalidades de registro de gastos, cálculo proporcional de aportes basado en ingresos, seguimiento de contribuciones y generación de reportes mensuales.

El producto cuenta con una **Landing Page** desplegada en GitHub Pages, una **Web Application** en Vue.js alojada en Firebase Hosting, una **API RESTful** desarrollada en ASP.NET Core y desplegada en Azure, y una **aplicación móvil nativa** construida con Flutter. La arquitectura sigue principios de Domain-Driven Design (DDD) con bounded contexts bien definidos: IAM, Households, Incomes, Bills, Contributions y Settings.

A pesar de estos avances técnicos, el equipo reconoce que **no se han validado empíricamente** las hipótesis centrales del negocio:

- Que los usuarios adoptarán el cálculo proporcional de aportes como mecanismo de distribución justo.
- Que la automatización reduce efectivamente los conflictos financieros dentro del hogar.
- Que la interfaz actual es suficientemente intuitiva para usuarios con baja alfabetización digital.
- Que la funcionalidad de metas de ahorro compartidas incentiva la cooperación económica.

El modelo de monetización freemium no ha sido probado en usuarios reales. Se desconoce si los usuarios estarían dispuestos a pagar por funcionalidades premium como exportación de datos, análisis histórico avanzado o personalización de reglas de distribución.

El pipeline de CI/CD está operativo y permite despliegues continuos, lo que facilita la iteración rápida sobre el producto para ejecutar experimentos controlados en producción.

---

### 8.1.2. Raw Material: Assumptions, Knowledge Gaps, Ideas, Claims

#### Assumptions (Suposiciones del equipo)

| # | Supuesto | Área | Riesgo |
|---|----------|------|--------|
| A1 | Los usuarios prefieren un cálculo automático de aportes basado en ingresos sobre una división igualitaria. | Producto | Alto |
| A2 | La transparencia financiera dentro del hogar reduce los conflictos por dinero. | Negocio | Alto |
| A3 | Los usuarios completarán el onboarding sin necesidad de asistencia técnica. | UX | Alto |
| A4 | Al menos el 50% de los hogares creará una meta de ahorro compartida en los primeros dos meses. | Producto | Medio |
| A5 | El Representante del Hogar liderará activamente la adopción de la app para los demás miembros. | Comportamiento | Alto |
| A6 | Los usuarios están dispuestos a ingresar sus ingresos personales en una aplicación digital. | Privacidad | Alto |
| A7 | Las notificaciones push y correos de recordatorio reducen los pagos atrasados. | Retención | Medio |
| A8 | La versión freemium es suficiente para generar una base de usuarios activos que luego conviertan a premium. | Monetización | Alto |
| A9 | La aplicación móvil (Flutter) tendrá mayor adopción que la web application entre el segmento objetivo. | Canal | Medio |
| A10 | Los usuarios con experiencia en Excel adoptarán Budgetly como reemplazo sin fricciones significativas. | Adopción | Medio |

#### Knowledge Gaps (Brechas de conocimiento)

| # | Brecha | Impacto |
|---|--------|---------|
| KG1 | No sabemos cuántos hogares peruanos urbanos enfrentan desigualdad de ingresos significativa entre sus miembros. | Alto |
| KG2 | Se desconoce cuántos usuarios abandonan el flujo de registro antes de completarlo. | Alto |
| KG3 | No hay datos sobre la frecuencia con la que los usuarios regresan a la app después de su primera visita (Day 7, Day 30 retention). | Alto |
| KG4 | No se ha medido la satisfacción del usuario (NPS, CSAT) tras las primeras semanas de uso. | Alto |
| KG5 | Se desconoce si el modelo de distribución por ingresos genera percepción de equidad o de inequidad invertida en algunos perfiles. | Medio |
| KG6 | No se conoce el ticket promedio que un usuario estaría dispuesto a pagar por el plan premium. | Medio |
| KG7 | No se ha validado qué canal de adquisición (redes sociales, boca a boca, SEO) genera usuarios de mayor calidad. | Medio |
| KG8 | Se desconoce si los representantes del hogar son el perfil correcto para liderar la adopción o si debería orientarse a los miembros. | Alto |

#### Ideas

- Implementar un **modo simulación** en la landing page que permita a los visitantes probar el cálculo proporcional sin registrarse.
- Añadir un **asistente de onboarding** paso a paso que guíe al nuevo usuario desde el registro hasta el primer gasto registrado.
- Incorporar **gamificación** (badges, rachas de pagos puntuales) para incentivar el uso continuo.
- Crear un **tablero de transparencia** donde todos los miembros vean en tiempo real el estado de los aportes.
- Ofrecer una **integración bancaria básica** (lectura de saldo) para reducir la fricción del ingreso manual de datos.
- Desarrollar un **widget de resumen mensual** que pueda compartirse por WhatsApp entre los miembros del hogar.

#### Claims (Afirmaciones a validar)

| # | Afirmación | Fuente |
|---|-----------|--------|
| C1 | "El 70% de los usuarios reportará menos discusiones por dinero en los primeros 3 meses." | Hipótesis 1 del Lean UX |
| C2 | "Los pagos atrasados se reducirán en un 50% con alertas automáticas." | Hipótesis 2 del Lean UX |
| C3 | "El 80% de los usuarios completará el registro sin asistencia técnica." | Hipótesis 3 del Lean UX |
| C4 | "Al menos el 50% de hogares creará una meta de ahorro en los primeros 2 meses." | Hipótesis 4 del Lean UX |
| C5 | "Los usuarios con ingresos dispares perciben la distribución proporcional como más justa." | Entrevistas de needfinding |

---

### 8.1.3. Experiment-Ready Questions

Las siguientes preguntas han sido priorizadas porque pueden ser respondidas mediante experimentos controlados en un plazo razonable y con los recursos disponibles del equipo.

| ID | Pregunta | Tipo |
|----|----------|------|
| ERQ-01 | ¿Qué porcentaje de usuarios que visitan la landing page inician el proceso de registro? | Cuantitativa |
| ERQ-02 | ¿Cuántos usuarios completan el flujo de onboarding (registro → crear/unirse a hogar → primer gasto) sin abandonar? | Cuantitativa |
| ERQ-03 | ¿Los usuarios que reciben recordatorios automáticos registran sus pagos con mayor puntualidad que los que no los reciben? | Comparativa |
| ERQ-04 | ¿La visualización gráfica de aportes proporcionales aumenta la percepción de equidad respecto a mostrar solo montos en texto? | Comparativa (A/B) |
| ERQ-05 | ¿Qué funcionalidades premium generan mayor intención de pago en los usuarios del plan gratuito? | Cuantitativa |
| ERQ-06 | ¿El modo simulación en la landing page aumenta la tasa de registro comparado con no tenerlo? | Comparativa (A/B) |
| ERQ-07 | ¿Los hogares con representante activo tienen mayor tasa de retención a 30 días que los hogares sin representante activo? | Correlacional |
| ERQ-08 | ¿Cuántos días tarda un usuario en registrar su segundo gasto después del primero? | Cuantitativa |

---

### 8.1.4. Question Backlog

El backlog de preguntas está ordenado por impacto en el negocio y viabilidad de ejecución en el corto plazo.

| Prioridad | ID | Pregunta | Impacto | Esfuerzo | Estado |
|-----------|-----|----------|---------|----------|--------|
| 1 | ERQ-02 | ¿Cuántos usuarios completan el onboarding sin abandonar? | Alto | Bajo | Listo para experimentar |
| 2 | ERQ-01 | ¿Qué % de visitantes inicia el registro? | Alto | Bajo | Listo para experimentar |
| 3 | ERQ-03 | ¿Los recordatorios mejoran la puntualidad de pagos? | Alto | Medio | Listo para experimentar |
| 4 | ERQ-04 | ¿Los gráficos mejoran la percepción de equidad? | Alto | Medio | En diseño |
| 5 | ERQ-06 | ¿El simulador en landing page aumenta el registro? | Alto | Medio | En diseño |
| 6 | ERQ-07 | ¿El representante activo impacta la retención? | Alto | Alto | Pendiente |
| 7 | ERQ-08 | ¿Cuántos días hasta el segundo gasto? | Medio | Bajo | Listo para experimentar |
| 8 | ERQ-05 | ¿Qué features premium generan más intención de pago? | Medio | Medio | Pendiente |

---

### 8.1.5. Experiment Cards

---

**Experiment Card #1**

| Campo | Detalle |
|-------|---------|
| **ID** | EXP-01 |
| **Nombre** | Tasa de completación del onboarding |
| **Pregunta asociada** | ERQ-02 |
| **Hipótesis** | Creemos que al menos el 80% de los usuarios que inician el registro completarán el flujo de onboarding (registro → hogar → primer gasto) si el proceso se presenta en pasos claros y secuenciales. |
| **Método** | Análisis de embudo (funnel analysis) sobre eventos de navegación registrados en la aplicación. |
| **Segmento** | Nuevos usuarios registrados en los primeros 30 días de la campaña. |
| **Métricas** | Tasa de completación por paso del embudo; tasa de abandono por pantalla. |
| **Criterio de éxito** | ≥ 80% de usuarios completan el paso de creación/unión a hogar dentro de las 24 horas del registro. |
| **Duración** | 4 semanas |
| **Resultado esperado** | Identificar el paso de mayor abandono para priorizar mejoras de UX. |

---

**Experiment Card #2**

| Campo | Detalle |
|-------|---------|
| **ID** | EXP-02 |
| **Nombre** | Impacto de recordatorios automáticos en puntualidad de pagos |
| **Pregunta asociada** | ERQ-03 |
| **Hipótesis** | Creemos que los miembros que reciben recordatorios push 48 horas antes de la fecha límite de aporte registrarán su pago a tiempo con una tasa al menos 30 puntos porcentuales mayor que los que no reciben recordatorio. |
| **Método** | Prueba A/B: Grupo A recibe notificación push + email a 48h de la fecha límite; Grupo B no recibe ningún recordatorio. |
| **Segmento** | Miembros del hogar con al menos un aporte pendiente en el mes en curso. |
| **Métricas** | % de aportes registrados antes de la fecha límite por grupo; tiempo promedio desde el recordatorio hasta el registro del pago. |
| **Criterio de éxito** | El Grupo A presenta una tasa de pagos a tiempo ≥ 50% mayor que el Grupo B. |
| **Duración** | 6 semanas (2 ciclos de pago mensual) |
| **Resultado esperado** | Validar la utilidad de las notificaciones automáticas y determinar el momento óptimo de envío. |

---

**Experiment Card #3**

| Campo | Detalle |
|-------|---------|
| **ID** | EXP-03 |
| **Nombre** | A/B Test: Gráfico vs. texto en panel de aportes |
| **Pregunta asociada** | ERQ-04 |
| **Hipótesis** | Creemos que mostrar la distribución de aportes mediante un gráfico circular interactivo aumentará la percepción de equidad del sistema en al menos un 20% respecto a mostrar solo montos en texto plano. |
| **Método** | Prueba A/B: Variante A muestra los aportes en tabla de texto; Variante B muestra un gráfico circular con porcentajes y montos. Encuesta de percepción de equidad al final de la semana. |
| **Segmento** | Miembros del hogar con al menos 2 semanas de uso activo. |
| **Métricas** | Puntuación de percepción de equidad (escala Likert 1-5); tiempo en pantalla del panel de aportes; número de clics en el panel. |
| **Criterio de éxito** | La Variante B obtiene una puntuación media de percepción de equidad ≥ 4.0/5.0, al menos 20% mayor que la Variante A. |
| **Duración** | 3 semanas |
| **Resultado esperado** | Confirmar que la visualización gráfica es más efectiva para comunicar equidad y justificar su priorización en el roadmap. |

---

**Experiment Card #4**

| Campo | Detalle |
|-------|---------|
| **ID** | EXP-04 |
| **Nombre** | Simulador interactivo en landing page |
| **Pregunta asociada** | ERQ-06 |
| **Hipótesis** | Creemos que añadir un simulador de distribución de gastos en la landing page aumentará la tasa de conversión de visitante a registro en al menos un 15%. |
| **Método** | Prueba A/B: 50% de los visitantes ve la landing page actual (sin simulador); 50% ve la versión con el simulador interactivo. Se mide el CTR del botón "Registrarse". |
| **Segmento** | Visitantes nuevos de la landing page sin cuenta previa. |
| **Métricas** | Tasa de conversión (clics en "Registrarse" / visitantes únicos); tiempo en página; tasa de rebote. |
| **Criterio de éxito** | La variante con simulador presenta una tasa de conversión al menos un 15% superior a la variante de control. |
| **Duración** | 4 semanas |
| **Resultado esperado** | Validar si la experiencia interactiva previa al registro reduce la fricción de adopción y justifica el esfuerzo de desarrollo del simulador. |

---
## 8.2. Experiment Design

### 8.2.1. Hypotheses

Las hipótesis de experimentación de Budgetly se derivan directamente de las Lean UX Hypothesis Statements definidas en el capítulo I, traducidas ahora a un formato estructurado para su validación empírica.

---

**Hipótesis 1 – Adopción del cálculo proporcional**

> Creemos que **automatizar la asignación de contribuciones según los ingresos individuales** aumentará la equidad percibida y reducirá los conflictos financieros dentro del hogar.  
> Sabremos que hemos tenido éxito cuando el **70% de los usuarios activos** reporte una disminución en las discusiones por dinero, medido mediante encuesta in-app al tercer mes de uso.

---

**Hipótesis 2 – Efectividad de recordatorios automáticos**

> Creemos que **enviar recordatorios automáticos 48 horas antes de la fecha límite de pago** reducirá los aportes atrasados en el hogar.  
> Sabremos que hemos tenido éxito cuando el **número de contribuciones registradas fuera de plazo se reduzca en un 50%** respecto al mes anterior al activar los recordatorios, medido sobre un grupo de al menos 50 hogares activos.

---

**Hipótesis 3 – Onboarding sin fricción**

> Creemos que **una interfaz intuitiva y un flujo de onboarding paso a paso** incrementará la tasa de adopción incluso entre usuarios con poca experiencia digital.  
> Sabremos que hemos tenido éxito cuando el **80% de los nuevos usuarios complete el registro y configure su primer hogar dentro de las 24 horas** de haberse registrado, sin necesidad de contactar al soporte.

---

**Hipótesis 4 – Metas de ahorro compartido**

> Creemos que **introducir metas de ahorro compartidas visibles para todos los miembros** fomentará la cooperación económica y el compromiso financiero dentro del hogar.  
> Sabremos que hemos tenido éxito cuando **al menos el 40% de los hogares activos cree y mantenga activa al menos una meta de ahorro** durante los primeros 60 días de uso.

---

**Hipótesis 5 – Conversión freemium a premium**

> Creemos que **ofrecer un plan premium con reportes avanzados, exportación PDF y personalización de reglas** generará una intención de pago real en usuarios que ya usan activamente el plan gratuito.  
> Sabremos que hemos tenido éxito cuando **al menos el 10% de los usuarios activos del plan gratuito con más de 30 días de uso** indiquen intención de contratar el plan premium en una encuesta de valoración de funcionalidades.

---

### 8.2.2. Domain Business Metrics

Las métricas de negocio de Budgetly se organizan en tres dimensiones clave: **Adquisición**, **Activación y Retención**, y **Monetización**.

#### Adquisición

| Métrica | Descripción | Meta inicial |
|---------|-------------|-------------|
| Visitantes únicos a la landing page | Número de usuarios nuevos que acceden a la landing page por semana. | 500/semana |
| Tasa de conversión landing → registro | % de visitantes que completan el registro. | ≥ 10% |
| Costo de adquisición por canal | Estimación del costo por usuario registrado según canal (orgánico, redes, boca a boca). | Referencial |

#### Activación y Retención

| Métrica | Descripción | Meta inicial |
|---------|-------------|-------------|
| Tasa de onboarding completado | % de usuarios registrados que configuran su hogar y registran su primer gasto. | ≥ 70% |
| Retención Day 7 | % de usuarios que regresan a la app en los 7 días siguientes al registro. | ≥ 40% |
| Retención Day 30 | % de usuarios que regresan a la app en los 30 días siguientes al registro. | ≥ 25% |
| Hogares activos mensuales (MAH) | Número de hogares con al menos un gasto registrado en el mes. | 50 en el primer mes |
| Aportes registrados a tiempo | % de contribuciones registradas antes de la fecha límite. | ≥ 65% |

#### Monetización

| Métrica | Descripción | Meta inicial |
|---------|-------------|-------------|
| Tasa de conversión freemium → premium | % de usuarios gratuitos que contratan el plan premium. | ≥ 5% en 3 meses |
| Ingreso mensual recurrente (MRR) | Suma de suscripciones premium activas por mes. | Referencial (fase beta) |
| Intención de pago por funcionalidad | % de usuarios que seleccionan una funcionalidad premium como "muy importante" en encuesta. | ≥ 30% por feature |

---

### 8.2.3. Measures

Las siguientes mediciones se aplicarán para evaluar el progreso de cada experimento:

#### Medidas cuantitativas

| Medida | Descripción | Fuente de datos |
|--------|-------------|-----------------|
| Tasa de completación de onboarding | % de usuarios que completan todos los pasos del flujo de alta. | Eventos de navegación (Firebase Analytics) |
| Tasa de abandono por pantalla | % de usuarios que abandonan el flujo en cada paso específico. | Funnel en Firebase / Mixpanel |
| Tiempo hasta primer gasto registrado | Días transcurridos entre el registro y el primer gasto creado. | Base de datos (tabla `bills`) |
| % de aportes registrados a tiempo | Contribuciones pagadas antes de `fecha_limite` / total de contribuciones. | Base de datos (tabla `member_contributions`) |
| Frecuencia de uso semanal | Promedio de sesiones por usuario activo por semana. | Firebase Analytics |
| CTR en botón de registro (landing page) | Clics en CTA / visitantes únicos de la landing. | Google Analytics / Firebase |

#### Medidas cualitativas

| Medida | Descripción | Instrumento |
|--------|-------------|-------------|
| Percepción de equidad | Grado en que el usuario considera justa la distribución de aportes. | Encuesta Likert in-app (1-5) |
| Satisfacción general (CSAT) | Valoración de la experiencia general con la aplicación. | Encuesta post-sesión |
| Net Promoter Score (NPS) | Probabilidad de recomendar Budgetly a otros. | Encuesta mensual in-app |
| Percepción de facilidad de uso | Grado de facilidad percibida al completar tareas clave. | System Usability Scale (SUS) |
| Intención de pago premium | Disposición declarada a pagar por funcionalidades avanzadas. | Encuesta de valoración de features |

---

### 8.2.4. Conditions

Cada experimento se ejecuta bajo condiciones controladas para garantizar la validez de los resultados:

#### Condiciones generales

- Todos los experimentos se ejecutan sobre usuarios reales del entorno de producción (no staging).
- Los usuarios asignados a grupos de control y tratamiento deben ser mutuamente excluyentes.
- La asignación a grupos A/B se realizará de forma aleatoria mediante un hash del `user_id` para garantizar distribución uniforme.
- Los experimentos no se superpondrán sobre el mismo segmento de usuarios simultáneamente para evitar efectos de interferencia.

#### Condiciones por experimento

| Experimento | Grupo Control | Grupo Tratamiento | Variable controlada |
|-------------|---------------|-------------------|---------------------|
| EXP-01 (Onboarding) | Flujo actual sin cambios | Flujo con barra de progreso y tips contextuales | Solo se modifica la UI del flujo de alta |
| EXP-02 (Recordatorios) | Sin notificación de recordatorio | Notificación push + email a 48h del vencimiento | Mismo perfil de hogar y monto de aporte |
| EXP-03 (Gráfico vs. texto) | Panel en texto plano (tabla) | Panel con gráfico circular interactivo | Mismos datos de aportes para ambos grupos |
| EXP-04 (Simulador landing) | Landing page actual | Landing page con simulador interactivo | Mismo tráfico de entrada por canal |

#### Criterios de exclusión

- Usuarios registrados hace menos de 48 horas (no tienen suficiente historial).
- Hogares con un solo miembro (no aplica la lógica de distribución de aportes).
- Usuarios que ya estén participando en otro experimento activo.

---

### 8.2.5. Scale Calculations and Decisions

Para determinar el tamaño de muestra necesario y la duración de cada experimento, se aplican cálculos básicos de significancia estadística.

#### Parámetros generales

- **Nivel de confianza:** 95% (α = 0.05)
- **Potencia estadística:** 80% (β = 0.20)
- **Efecto mínimo detectable (MDE):** 10-15% de mejora relativa según el experimento

#### Cálculo de muestra para EXP-02 (Recordatorios)

- Tasa de pagos a tiempo en grupo control estimada: **50%**
- Mejora mínima esperada: **+20 puntos porcentuales** (hasta 70%)
- Tamaño de muestra requerido por grupo: **~95 usuarios** (calculado con fórmula de proporción binomial)
- Total requerido: **~190 usuarios activos con aportes pendientes**
- Estimación de tiempo: **6 semanas** asumiendo ~35 nuevos usuarios activos por semana

#### Cálculo de muestra para EXP-04 (Simulador en landing)

- Tasa de conversión actual estimada: **8%**
- Mejora mínima esperada: **+15% relativo** (hasta ~9.2%)
- Tamaño de muestra requerido por grupo: **~2,800 visitantes**
- Total requerido: **~5,600 visitantes únicos**
- Estimación de tiempo: **4-6 semanas** con estrategia de difusión en redes sociales y comunidades financieras

#### Decisiones de escala

| Experimento | Muestra mínima | Duración estimada | Decisión si MDE no se alcanza |
|-------------|----------------|-------------------|-------------------------------|
| EXP-01 | 150 nuevos usuarios | 4 semanas | Revisar el paso de mayor abandono y rediseñar |
| EXP-02 | 190 usuarios activos | 6 semanas | Probar con recordatorio a 24h en lugar de 48h |
| EXP-03 | 100 usuarios activos | 3 semanas | Mantener versión de texto y deprioritizar gráfico |
| EXP-04 | 5,600 visitantes | 4-6 semanas | Evaluar inversión en pauta pagada para acelerar |

---

### 8.2.6. Methods Selection

La selección de métodos combina técnicas cuantitativas y cualitativas para obtener una visión integral del comportamiento del usuario.

#### Métodos cuantitativos

| Método | Aplicación en Budgetly |
|--------|----------------------|
| **Análisis de embudo (Funnel Analysis)** | Identificar dónde abandonan los usuarios en el onboarding. Implementado con Firebase Analytics + eventos personalizados. |
| **Prueba A/B** | Comparar variantes de interfaz (gráfico vs. texto, landing con/sin simulador). Asignación aleatoria por hash de `user_id`. |
| **Análisis de cohortes** | Medir la retención Day 7 y Day 30 por cohorte de registro (semana de alta). |
| **Análisis de correlación** | Evaluar si la actividad del Representante del Hogar correlaciona con la retención de los miembros. |
| **Métricas de eventos (Event Tracking)** | Registrar acciones clave: registro, creación de hogar, primer gasto, primer aporte registrado, uso de gráficos, apertura de notificaciones. |

#### Métodos cualitativos

| Método | Aplicación en Budgetly |
|--------|----------------------|
| **Encuestas in-app** | Medir NPS, CSAT, percepción de equidad y satisfacción con el onboarding. Activadas por evento (primer gasto, día 7, día 30). |
| **Pruebas de usabilidad (moderadas)** | Sesiones de 30-45 minutos con 5-8 usuarios nuevos que completan tareas clave (registrarse, crear hogar, registrar gasto, verificar aportes). |
| **Entrevistas de seguimiento** | Entrevistas breves (15 min) con usuarios que abandonaron el onboarding para entender las barreras. |
| **Análisis de mapas de calor (Heatmaps)** | Identificar zonas de interacción y fricción en la landing page y en el dashboard principal mediante Hotjar o herramienta equivalente. |

#### Criterio de selección

Los métodos cuantitativos se priorizan para validar hipótesis con criterios numéricos claros (EXP-01, EXP-02, EXP-04). Los métodos cualitativos complementan cuando los datos cuantitativos muestran anomalías o cuando se necesita comprender el "por qué" detrás del comportamiento observado (especialmente en casos de alta tasa de abandono).

---

### 8.2.7. Data Analytics: Goals, KPIs and Metrics Selection

#### Goals (Objetivos de análisis)

| Goal | Descripción | Vinculado a |
|------|-------------|-------------|
| G1 | Maximizar la tasa de activación de nuevos usuarios | Hipótesis 3 |
| G2 | Reducir los pagos atrasados entre miembros del hogar | Hipótesis 2 |
| G3 | Aumentar la percepción de equidad en la distribución de gastos | Hipótesis 1 |
| G4 | Incrementar la retención mensual de hogares activos | Hipótesis general de producto |
| G5 | Generar intención de conversión al plan premium | Hipótesis 5 |

#### KPIs y Métricas por objetivo

**G1 – Activación**

| KPI | Métrica asociada | Herramienta |
|-----|-----------------|-------------|
| Tasa de completación de onboarding | % usuarios que completan registro → hogar → primer gasto | Firebase Analytics (funnel) |
| Tiempo hasta activación | Horas desde registro hasta primer gasto | Base de datos (`bills.created_at` vs `users.created_at`) |
| Tasa de abandono por paso | % de usuarios que abandonan en cada pantalla del flujo | Firebase + eventos personalizados |

**G2 – Reducción de pagos atrasados**

| KPI | Métrica asociada | Herramienta |
|-----|-----------------|-------------|
| % aportes registrados a tiempo | `member_contributions` con `pagado_en` ≤ `contributions.fecha_limite` | Base de datos (query SQL) |
| Tasa de apertura de notificaciones | % de notificaciones push abiertas vs. enviadas | Firebase Cloud Messaging |
| Tiempo desde recordatorio a pago | Horas entre envío de notificación y registro del pago | Logs de notificaciones + BD |

**G3 – Percepción de equidad**

| KPI | Métrica asociada | Herramienta |
|-----|-----------------|-------------|
| Puntuación de equidad percibida | Escala Likert 1-5 en encuesta in-app | Encuesta in-app (formulario nativo) |
| NPS (Net Promoter Score) | Escala 0-10 en encuesta mensual | Encuesta in-app mensual |
| Tiempo en pantalla de aportes | Segundos en el panel de distribución de aportes | Firebase Analytics |

**G4 – Retención**

| KPI | Métrica asociada | Herramienta |
|-----|-----------------|-------------|
| Retención Day 7 | % usuarios con sesión en días 6-8 post-registro | Análisis de cohortes (Firebase) |
| Retención Day 30 | % usuarios con sesión en días 28-32 post-registro | Análisis de cohortes (Firebase) |
| Hogares Activos Mensuales (MAH) | Hogares con ≥1 gasto registrado en el mes | Base de datos |
| Frecuencia de sesiones semanales | Promedio de sesiones por usuario activo por semana | Firebase Analytics |

**G5 – Intención de conversión premium**

| KPI | Métrica asociada | Herramienta |
|-----|-----------------|-------------|
| Intención de pago declarada | % usuarios que seleccionan "muy importante" en encuesta de features premium | Encuesta in-app |
| Clics en "Ver plan premium" | CTR en botón de upgrade desde la UI | Firebase Analytics (evento `premium_view_click`) |
| Funcionalidades más valoradas | Ranking de features premium por puntuación media | Encuesta in-app |

---

### 8.2.8. Web and Mobile Tracking Plan

El plan de tracking define todos los eventos que deben instrumentarse en la **Web Application (Vue.js)** y la **Mobile Application (Flutter)** para alimentar los experimentos y métricas definidos.

#### Convenciones de nomenclatura de eventos

Se utiliza la convención `snake_case` con prefijo de módulo: `{modulo}_{accion}_{objeto}`.

Ejemplo: `auth_completed_registration`, `household_created_home`, `expense_registered_bill`.

---

#### Eventos de Autenticación (IAM)

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `auth_viewed_landing` | Usuario visita la landing page | `source`, `device`, `timestamp` |
| `auth_clicked_register_cta` | Usuario hace clic en el botón de registro | `cta_location` (hero/navbar/footer), `variant` (A/B) |
| `auth_started_registration` | Usuario inicia el formulario de registro | `role_selected` (representante/miembro) |
| `auth_completed_registration` | Usuario completa el registro exitosamente | `role`, `method` (email), `timestamp` |
| `auth_failed_registration` | Registro fallido | `error_type`, `step_failed` |
| `auth_logged_in` | Inicio de sesión exitoso | `role`, `device_type` |
| `auth_logged_out` | Cierre de sesión | `session_duration_minutes` |

---

#### Eventos de Onboarding

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `onboarding_viewed_step` | Usuario ve un paso del onboarding | `step_number`, `step_name` |
| `onboarding_completed_step` | Usuario completa un paso | `step_number`, `step_name`, `time_spent_seconds` |
| `onboarding_abandoned_step` | Usuario abandona en un paso | `step_number`, `step_name`, `time_spent_seconds` |
| `onboarding_completed_full` | Usuario completa todo el flujo de alta | `total_time_minutes`, `device_type` |

---

#### Eventos de Hogares (Households)

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `household_created_home` | Representante crea un hogar | `household_id`, `currency` |
| `household_joined_home` | Miembro se une a un hogar | `household_id`, `method` (ID/invitación) |
| `household_invited_member` | Representante envía invitación | `household_id`, `invite_method` |
| `household_accepted_invitation` | Miembro acepta una invitación | `household_id`, `time_to_accept_hours` |
| `household_viewed_members` | Usuario consulta la lista de miembros | `household_id`, `member_count` |

---

#### Eventos de Gastos (Bills & Expenses)

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `expense_viewed_list` | Usuario ve la lista de gastos | `household_id`, `filter_applied` |
| `expense_started_registration` | Usuario abre formulario de nuevo gasto | `household_id` |
| `expense_completed_registration` | Usuario registra un gasto exitosamente | `household_id`, `amount`, `category`, `has_attachment` |
| `expense_abandoned_registration` | Usuario abandona el formulario | `step_abandoned`, `time_spent_seconds` |
| `expense_viewed_charts` | Usuario accede a la vista de gráficos | `chart_type`, `period_selected` |
| `expense_downloaded_report` | Usuario descarga un reporte PDF | `period`, `report_type` |

---

#### Eventos de Contribuciones (Contributions)

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `contribution_viewed_my_amount` | Miembro consulta su monto a pagar | `household_id`, `amount`, `days_to_deadline` |
| `contribution_registered_payment` | Miembro registra un pago | `household_id`, `amount`, `method`, `on_time` (bool) |
| `contribution_viewed_history` | Usuario consulta historial de pagos | `period_selected`, `payment_count` |
| `contribution_viewed_distribution` | Usuario ve la distribución de aportes | `view_type` (texto/gráfico), `variant` |

---

#### Eventos de Notificaciones y Recordatorios

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `notification_sent_reminder` | Sistema envía recordatorio de pago | `channel` (push/email), `hours_to_deadline` |
| `notification_opened_reminder` | Usuario abre el recordatorio | `channel`, `time_to_open_minutes` |
| `notification_payment_registered_after_reminder` | Pago registrado tras recordatorio | `time_after_reminder_hours` |
| `notification_configured_preferences` | Usuario ajusta sus preferencias de notificación | `channels_enabled`, `frequency` |

---

#### Eventos de Encuestas y NPS

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `survey_viewed_nps` | Usuario ve la encuesta NPS | `trigger` (day7/day30/monthly) |
| `survey_completed_nps` | Usuario responde la encuesta NPS | `score`, `comment_provided` |
| `survey_viewed_equity` | Usuario ve la encuesta de equidad percibida | `trigger_context` |
| `survey_completed_equity` | Usuario responde la encuesta de equidad | `equity_score` (1-5) |
| `survey_viewed_premium_features` | Usuario ve la encuesta de features premium | — |
| `survey_completed_premium_features` | Usuario responde la encuesta de features | `top_feature_selected`, `payment_intent` |

---

#### Implementación técnica

| Plataforma | Herramienta de tracking | SDK |
|------------|------------------------|-----|
| Web Application (Vue.js) | Firebase Analytics | `firebase/analytics` JS SDK |
| Mobile Application (Flutter) | Firebase Analytics | `firebase_analytics` Flutter plugin |
| Landing Page | Google Analytics 4 + Firebase | `gtag.js` |
| Backend (ASP.NET Core) | Logs estructurados (Application Insights / Azure Monitor) | `Microsoft.ApplicationInsights` |

#### Consideraciones de privacidad

- Todos los eventos de tracking son **anónimos o seudonimizados**; no se almacena PII (información de identificación personal) en los payloads de eventos.
- El `user_id` utilizado en el tracking es un identificador interno de la plataforma, no vinculado a datos personales en los sistemas de analítica.
- Se presentará al usuario un aviso de cookies y tracking en el primer acceso, con opción de opt-out para analítica no esencial, en cumplimiento con la política de privacidad declarada en el Acuerdo de Servicio SaaS (sección 5.2.4).
- El plan de tracking será revisado y actualizado con cada nuevo experimento para asegurar que las métricas recopiladas son las mínimas necesarias para la toma de decisiones.

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

Tras la ejecución del experimento descrito en la sección 8.3 (reparto manual vs. Budgetly con estrategia *IncomeBased*), se recopilaron los datos de los 24 participantes (12 por grupo) durante las 2 semanas establecidas. A continuación, se presenta el análisis estadístico de las variables dependientes definidas en la matriz de variables, junto con la interpretación de los hallazgos.

#### A. Resultados por variable dependiente

| Variable | Grupo A (Manual) M (SD) | Grupo B (Budgetly IncomeBased) M (SD) | t (gl=22) | p-valor | Decisión (α = 0,05) |
|---|---|---|---|---|---|
| VD1 – Equidad percibida (1–5) | 3.1 (0.9) | 4.3 (0.6) | 3.85 | 0.0009 | Se rechaza H₀ → soporta H₁ |
| VD2 – Tiempo de resolución (segundos) | 245 (58) | 97 (22) | 8.10 | < 0.001 | Se rechaza H₀ → soporta H₁ |
| VD3 – Confianza en el resultado (1–5) | 3.4 (0.8) | 4.5 (0.5) | 4.02 | 0.0006 | Se rechaza H₀ → soporta H₁ |
| VD4 – Intención de uso (1–5) | 3.0 (1.0) | 4.1 (0.7) | 3.15 | 0.0047 | Se rechaza H₀ → soporta H₁ |

En las cuatro variables dependientes se observaron diferencias estadísticamente significativas a favor del Grupo B (Budgetly con *IncomeBased*), por lo que se rechaza la hipótesis nula (H₀) planteada en 8.3 y se acepta la hipótesis alternativa (H₁): el reparto automatizado mediante *IncomeBased* genera mayor equidad percibida, mayor confianza y mayor intención de uso, además de reducir significativamente el tiempo de resolución de la tarea frente al método manual.

#### B. Análisis de variables de control (VC-01, VC-02, VC-03)

| Variable de control | Hallazgo | Implicancia |
|---|---|---|
| VC-01 – Familiaridad con Excel | Distribución balanceada entre grupos (7/12 en Grupo A y 6/12 en Grupo B con alta familiaridad). Dentro del Grupo A, los participantes con alta familiaridad obtuvieron VD1 ligeramente mayor (3.4) que los de baja familiaridad (2.8), pero ambos subgrupos permanecen por debajo del promedio del Grupo B (4.3). | La familiaridad con Excel no explica la diferencia observada entre grupos; el efecto de *IncomeBased* se mantiene independientemente del nivel de experiencia previa con hojas de cálculo. |
| VC-02 – Nivel de ingresos del hogar | El reclutamiento logró una distribución equivalente por cuartil de ingreso entre ambos grupos. | Se descarta que el nivel socioeconómico del participante sea un factor de confusión en los resultados. |
| VC-03 – Sesgo de novedad | Puntuación promedio pre-tarea similar entre grupos (Grupo A = 3.6; Grupo B = 3.8; diferencia no significativa, p = 0.41). | El entusiasmo por probar una aplicación nueva no explica por sí solo la mejora observada en VD4 (intención de uso); el efecto es atribuible principalmente al mecanismo de cálculo proporcional.|

#### C. Interpretación de los resultados

1. **Validación de la Hipótesis 1 (Lean UX) y de C5 (Claim del Cap. VIII).** Los resultados respaldan empíricamente la afirmación de que los usuarios con ingresos dispares perciben la distribución proporcional como más justa que el reparto manual, confirmando el *Claim* C5 registrado en la sección 8.1.2.
2. **Reducción del tiempo de resolución (VD2).** La reducción de ~60% en el tiempo de resolución (de 245s a 97s) constituye la señal más contundente del experimento y sustenta el valor de negocio de automatizar el cálculo, en línea con las fricciones identificadas en las entrevistas del Capítulo II (Abraham, Renzo, Ronald).
3. **Limitación identificada.** El experimento comparó el método manual contra la experiencia completa de Budgetly (cálculo + interfaz visual), por lo que no es posible aislar cuánto del efecto proviene específicamente del cálculo *IncomeBased* y cuánto de la presentación visual del panel de contribuciones. Esta limitación mantiene vigente y **eleva la prioridad** de EXP-03 (Gráfico vs. texto), definido en 8.1.5, como próximo experimento a ejecutar dentro de la propia aplicación.
4. **Nuevas brechas de conocimiento (Knowledge Gaps) identificadas durante la sesión:**

| ID | Brecha de conocimiento identificada | Origen |
|---|---|---|
| KG9 | Se desconoce si el efecto de *IncomeBased* sobre la equidad percibida es proporcional a la disparidad de ingresos del hogar (a mayor disparidad, ¿mayor beneficio percibido?). | Observación cualitativa durante la ejecución del experimento (comentarios de participantes con ingresos similares entre sí). |
| KG10 | La etiqueta "Ingreso mensual" generó dudas en 3 de 12 participantes del Grupo B sobre si debían declarar ingreso bruto o neto, lo que retrasó el registro de datos. | Observación del moderador durante la sesión; consistente con el hallazgo #5 de la auditoría de UX recibida (sección 6.4.2.3). |

### 8.4.2. Re-scored and Re-prioritized Question Backlog

A partir de los resultados obtenidos, se actualiza el backlog de preguntas presentado originalmente en la sección 8.1.4, ajustando prioridad, impacto y estado en función de la evidencia recolectada, e incorporando las preguntas emergentes (KG9, KG10).

| Prioridad | ID | Pregunta | Impacto | Esfuerzo | Estado actualizado | Justificación del re-scoring |
|---|---|---|---|---|---|---|
| 1 | ERQ-04 | ¿La visualización gráfica de aportes proporcionales aumenta la percepción de equidad respecto a mostrar solo montos en texto? | Alto | Medio | **Reabierta / priorizada** | El experimento de 8.3 no permitió aislar el efecto de la UI del efecto del cálculo; se requiere ahora un A/B específico dentro de la app (EXP-03) para separar ambos factores. |
| 2 | ERQ-NUEVA (KG10) | ¿La aclaración del término "Ingreso mensual" (bruto/neto) reduce el tiempo y los errores de registro de ingresos? | Alto | Bajo | **Nueva – Listo para experimentar** | Hallazgo directo de la sesión de moderación; bajo esfuerzo de implementación (cambio de copy) y alto impacto en la calidad del dato de entrada. |
| 3 | ERQ-NUEVA (KG9) | ¿El beneficio percibido de *IncomeBased* es mayor en hogares con alta disparidad de ingresos que en hogares con ingresos similares? | Medio-Alto | Medio | **Nueva – En diseño** | Permite segmentar el mensaje de valor de la propuesta y priorizar el segmento con mayor sensibilidad al problema (Cap. I, Segmento objetivo 1). |
| 4 | ERQ-02 | ¿Cuántos usuarios completan el flujo de onboarding (registro → hogar → primer gasto) sin abandonar? | Alto | Bajo | **Priorizada (sube)** | El registro de ingresos (prerrequisito de *IncomeBased*, US-TB-01) añade un paso adicional al onboarding; es necesario medir su impacto real en la tasa de completación en producción, no solo en entorno moderado. |
| 5 | ERQ-01 | ¿Qué porcentaje de usuarios que visitan la landing page inician el proceso de registro? | Alto | Bajo | Listo para experimentar | Sin cambios; se mantiene como línea base de adquisición. |
| 6 | ERQ-03 | ¿Los usuarios que reciben recordatorios automáticos registran sus pagos con mayor puntualidad que los que no los reciben? | Alto | Medio | Listo para experimentar | Sin cambios; pendiente de ejecución por restricciones de tiempo del ciclo académico. |
| 7 | ERQ-06 | ¿El modo simulación en la landing page aumenta la tasa de registro comparado con no tenerlo? | Alto | Medio | En diseño | Baja ligeramente en prioridad frente a ERQ-04 y las preguntas emergentes, dado el tamaño de muestra elevado (~5,600 visitantes) requerido y los recursos limitados del equipo. |
| 8 | ERQ-07 | ¿Los hogares con representante activo tienen mayor tasa de retención a 30 días que los hogares sin representante activo? | Alto | Alto | Pendiente | Sin cambios; requiere mayor volumen de hogares activos en producción del que se dispone actualmente. |
| 9 | ERQ-08 | ¿Cuántos días tarda un usuario en registrar su segundo gasto después del primero? | Medio | Bajo | Listo para experimentar | Sin cambios. |
| 10 | ERQ-05 | ¿Qué funcionalidades premium generan mayor intención de pago en los usuarios del plan gratuito? | Medio | Medio | Pendiente | Se mantiene en la parte inferior del backlog; la evidencia de este ciclo no aporta información nueva sobre monetización. |

**Decisiones derivadas del re-scoring:**
- Se promueve ERQ-04 al primer lugar del backlog, a ejecutarse como EXP-03 en el siguiente ciclo, dentro del propio flujo de la aplicación (no en un escenario moderado).
- Se incorpora una mejora de bajo esfuerzo y alto impacto (aclaración del rótulo "Ingreso mensual") directamente al Product Backlog (ítem PB-TB-017, ver sección 8.5.1) sin necesidad de un experimento formal adicional, dado que la evidencia cualitativa ya es suficiente para justificar el cambio.
- Se mantiene en observación la relación entre disparidad de ingresos y beneficio percibido (KG9) como candidato a un análisis de segmentación sobre datos ya recolectados, antes de invertir en un nuevo experimento controlado.

---

## 8.5. Continuous Learning

### 8.5.1. Shareback Session Artifacts: Learning Workflow

Al finalizar el ciclo de experimentación, el equipo Equilibria realizó una **sesión de shareback** (intercambio de aprendizajes) con el objetivo de comunicar los resultados obtenidos, alinear al equipo sobre las decisiones a tomar y actualizar los artefactos de producto (Product Backlog, Question Backlog) en función de la evidencia recolectada. Esta sección documenta el flujo de aprendizaje continuo aplicado y los artefactos generados durante dicha sesión.

#### A. Flujo de aprendizaje continuo (Learning Workflow)

El flujo seguido por el equipo para transformar los resultados del experimento en decisiones accionables fue el siguiente:

1. **Recolección de evidencia:** consolidación de los datos cuantitativos (VD1–VD4) y las observaciones cualitativas del moderador durante las sesiones del experimento (sección 8.4.1).
2. **Síntesis en Insight Cards:** cada hallazgo relevante se documentó en una tarjeta de aprendizaje estandarizada (ver formato en el punto B), asignándole un nivel de confianza y una acción recomendada.
3. **Sesión de Shareback:** reunión de 45 minutos con todo el equipo, en la que se presentaron los resultados estadísticos, se discutieron las Insight Cards y se validaron las acciones propuestas.
4. **Actualización de artefactos:** con base en los acuerdos de la sesión, se actualizó el Question Backlog (sección 8.4.2) y se incorporaron nuevos ítems al Product Backlog To-Be (sección 8.3.2).
5. **Registro de decisiones:** cada decisión tomada (promover, descartar, seguir investigando) quedó documentada en un log de decisiones, trazable al experimento que la originó.
6. **Definición del siguiente ciclo:** se seleccionó el siguiente experimento a ejecutar (EXP-03) y se re-priorizó el backlog de preguntas para el próximo sprint de experimentación.

<p align="center">
  <img src="https://i.imgur.com/6jjIx2a.png" alt="Commits made by the members of the team in the first progress"/>
</p>

#### B. Formato de Insight Card

| Campo | Descripción |
|---|---|
| ID | Identificador único de la tarjeta de aprendizaje |
| Experimento de origen | Referencia al experimento o pregunta que generó el hallazgo |
| Enunciado del aprendizaje | Afirmación breve y clara sobre lo aprendido |
| Nivel de confianza | Alto / Medio / Bajo, según robustez estadística y tamaño de muestra |
| Evidencia de soporte | Datos, cita o métrica que sustenta el enunciado |
| Acción recomendada | Qué debe hacer el equipo con este aprendizaje |
| Responsable | Integrante encargado de dar seguimiento a la acción |

#### C. Insight Cards generadas en la sesión

**Insight Card #1**

| Campo | Detalle |
|---|---|
| ID | IC-01 |
| Experimento de origen | Experimento IncomeBased vs. Manual (8.3) |
| Enunciado del aprendizaje | El reparto automatizado por ingresos reduce en ~60% el tiempo que toma a un hogar resolver la distribución de un gasto compartido. |
| Nivel de confianza | Alto (p < 0.001, efecto consistente en las 4 variables dependientes) |
| Evidencia de soporte | VD2: 245s (Grupo A) vs. 97s (Grupo B); t(22) = 8.10 |
| Acción recomendada | Usar este resultado como mensaje principal de valor en la landing page y en el simulador propuesto en EXP-04. |
| Responsable | Angelo Solano |

**Insight Card #2**

| Campo | Detalle |
|---|---|
| ID | IC-02 |
| Experimento de origen | Experimento IncomeBased vs. Manual (8.3) |
| Enunciado del aprendizaje | No es posible atribuir con certeza la mejora en equidad percibida solo al cálculo *IncomeBased*; la presentación visual del panel de contribuciones puede estar influyendo. |
| Nivel de confianza | Medio (limitación metodológica identificada, no una medición directa) |
| Evidencia de soporte | Diseño del experimento no aisló la variable de interfaz (texto vs. gráfico); ver 8.4.1-C.3 |
| Acción recomendada | Ejecutar EXP-03 (Gráfico vs. texto) como experimento independiente dentro de la app en producción. |
| Responsable | Camila Huamani |

**Insight Card #3**

| Campo | Detalle |
|---|---|
| ID | IC-03 |
| Experimento de origen | Observación cualitativa durante moderación (8.3) |
| Enunciado del aprendizaje | El rótulo "Ingreso mensual" genera ambigüedad (bruto vs. neto) en una parte relevante de los usuarios, afectando la calidad del dato registrado. |
| Nivel de confianza | Medio (observación cualitativa en 3 de 12 participantes del Grupo B) |
| Evidencia de soporte | Notas de moderador; consistente con hallazgo #5 de la auditoría UX recibida (6.4.2.3). |
| Acción recomendada | Incorporar un texto de ayuda contextual en el campo de ingreso ("Ingresa tu ingreso neto mensual") sin necesidad de un experimento formal adicional. |
| Responsable | Martin Gonzales |

#### D. Decisiones registradas en la sesión de Shareback

| # | Decisión | Tipo | Vinculado a |
|---|---|---|---|
| D1 | Promover EXP-03 (Gráfico vs. texto) al primer lugar del backlog de experimentación. | Promover | IC-02, ERQ-04 |
| D2 | Incorporar mejora de copy en el campo "Ingreso mensual" directamente al Product Backlog, sin experimento previo. | Implementar directo | IC-03, PB-TB-017 |
| D3 | Mantener en observación la relación entre disparidad de ingresos y beneficio percibido (KG9); evaluar con análisis de segmentación antes de invertir en un nuevo experimento. | Seguir investigando (bajo costo) | KG9 |
| D4 | Postergar EXP-01, EXP-02 y EXP-04 para el siguiente ciclo por restricciones de tiempo del proyecto académico, manteniendo sus Experiment Cards vigentes (8.1.5). | Postergar | ERQ-01, ERQ-02, ERQ-06 |

#### E. Artefactos generados por la sesión de Shareback

- **Reporte de resultados del experimento** (sección 8.4.1), compartido con el equipo antes de la sesión.
- **Tres Insight Cards** (IC-01, IC-02, IC-03), documentadas en el punto C.
- **Backlog de preguntas re-priorizado** (sección 8.4.2).
- **Nuevo ítem de Product Backlog** derivado de la sesión:

| ID | Tipo | Descripción | Vinculado a | Prioridad | SP |
|---|---|---|---|---|---|
| PB-TB-017 | Funcional | Agregar texto de ayuda contextual "Ingresa tu ingreso neto mensual" en el formulario de registro de ingresos | US-TB-01 | Alta | 1 |

- **Log de decisiones** (punto D), que queda como registro trazable para auditorías futuras y para el siguiente ciclo de experimentación del equipo.

## 8.6. To-Be Software Platform Pre-launch

### 8.6.1. About-the-Product Intro Video

*Contenido pendiente.*


## Matriz de Evaluación Ética y de Impacto

**Proyecto:** Budgetly (App de gestión financiera colaborativa del hogar, con cálculo proporcional de aportes según ingresos)

La matriz permite demostrar la capacidad del equipo de reconocer sus responsabilidades éticas y profesionales, y emitir juicios informados considerando el impacto de la solución de ingeniería de software (Student Outcome 4). Se busca evitar el "sentido mercenario de la ingeniería" (donde solo se busca lograr un fin contratado sin cuestionarse el fin en sí mismo) y evidenciar un pensamiento crítico y reflexivo sobre las implicancias de Budgetly.

---

| Dimensión / Criterio a Evaluar | Identificación de Riesgos e Impactos (Positivos y Negativos) | Evaluación del Impacto (¿A quién afecta y cuál es la magnitud?) | Estrategias de Mitigación y Acciones de Diseño |
|---|---|---|---|
| **1. Salud Pública y Seguridad** | *Negativo:* Las notificaciones y alertas de pagos atrasados (US21, US22) pueden generar ansiedad financiera o intensificar conflictos domésticos preexistentes, especialmente en hogares con dinámicas de control económico entre miembros. La visibilidad del ingreso de cada persona podría ser usada como herramienta de presión o vigilancia dentro de relaciones desiguales de poder. *Positivo:* La transparencia reduce discusiones por dinero y el estrés asociado a la incertidumbre financiera (Hipótesis 1, sección 1.2.2.3). | *Afectados:* Miembros del hogar en situación de vulnerabilidad económica o emocional dentro de relaciones de convivencia (parejas, familias). *Magnitud:* Media — no es un riesgo físico directo, pero puede agravar tensiones ya existentes en hogares con dinámicas de control financiero. | *Acciones:* Diseñar el tono de las notificaciones (US21, US22) como recordatorios neutrales y no acusatorios ("Tienes un pago pendiente" en lugar de "No has pagado tu parte"). Permitir que cada miembro configure la visibilidad de su propio ingreso frente a otros miembros no representantes. Incluir, en la sección de ayuda (EP06), enlaces a recursos de apoyo ante situaciones de violencia económica dentro del hogar. |
| **2. Inclusión y Accesibilidad** | *Negativo:* Budgetly requiere smartphone, conexión a Internet y cierto nivel de alfabetización digital y financiera para completar el registro de ingresos y comprender términos como "aporte proporcional" (identificado como riesgo en la sección 1.2.2.1 y en la auditoría UX, hallazgo 4.7 "Ayuda y documentación"). La auditoría de accesibilidad (6.3.3, sección 5) encontró etiquetas ARIA insuficientes y contraste deficiente en algunos componentes. *Positivo:* Automatiza cálculos financieros que hoy excluyen a quienes no dominan hojas de cálculo (Excel), ampliando el acceso a una gestión financiera justa. | *Afectados:* Personas con baja alfabetización digital, adultos mayores dentro del hogar, usuarios con discapacidad visual o motora, y personas sin acceso a un smartphone de gama media/alta. *Magnitud:* Alta — condiciona directamente quién puede usar la plataforma dentro del segmento objetivo definido en la sección 1.3. | *Acciones:* Incorporar onboarding guiado con lenguaje simple (ver Insight IC-03, sección 8.5.1). Completar las etiquetas ARIA y mejorar el contraste de color siguiendo las recomendaciones de la auditoría UX (6.3.3 y 6.4.2.3). Ofrecer una versión web ligera además de la app móvil para dispositivos de gama baja. Evaluar a futuro un modo de solo lectura por voz para miembros con discapacidad visual. |
| **3. Impacto Social y Cultural** | *Negativo:* Declarar el ingreso personal ante otros miembros del hogar puede chocar con normas culturales donde el dinero es un tema privado, o generar comparaciones que alteren dinámicas de poder dentro de la convivencia (identificado parcialmente en la entrevista de Harri, sección 2.2.3, quien prefiere un método informal antes que uno basado en ingresos). *Positivo:* Fomenta una cultura de corresponsabilidad económica y reduce la carga mental que históricamente recae sobre un solo miembro del hogar (usualmente el "representante"), tal como se documenta en el Empathy Mapping (2.3.4). | *Afectados:* Todos los miembros del hogar, en particular quienes provienen de culturas o familias donde discutir ingresos es tabú, y el "representante del hogar", cuyo rol concentra la responsabilidad de gestión. *Magnitud:* Media — varía significativamente según el perfil cultural y la dinámica de cada hogar. | *Acciones:* Mantener el método `AllocationMethod` configurable (PROPORTIONAL_INCOME, EQUAL, MIXED, CUSTOM — sección 4.9.2) para que los hogares elijan el nivel de transparencia de ingresos que les resulte cómodo, en lugar de imponer un único modelo. Comunicar en el onboarding que declarar ingresos es opcional y reversible. |
| **4. Impacto Económico** | *Negativo:* El modelo freemium (sección 1.2.2.2) podría excluir de las funcionalidades avanzadas (exportación, análisis histórico) a los hogares de menores recursos, que son precisamente el segmento con mayor necesidad de control financiero estricto. El registro de ingresos podría, en teoría, exponer información sensible con implicancias tributarias si se usa indebidamente. *Positivo:* Reduce pérdidas económicas por errores de cálculo manual (ver "Representación de costo promedio de errores en cálculos manuales", sección 1.2.1) y mejora la planificación financiera del hogar. | *Afectados:* Hogares de bajos ingresos que dependen del plan gratuito; usuarios cuya información de ingresos podría filtrarse (ver Dimensión 7). *Magnitud:* Media — el impacto económico directo es positivo para la mayoría, pero el riesgo de exclusión del plan premium y de exposición de datos requiere atención. | *Acciones:* Mantener las funciones esenciales de cálculo proporcional, alertas y reportes básicos en el plan gratuito (ya contemplado en el modelo freemium, 1.2.2.2), reservando para premium solo funciones de valor agregado no esenciales (exportación, personalización avanzada). Cifrar y limitar el acceso a los montos de ingreso (`Income.amount`) exclusivamente a los miembros autorizados del hogar. |
| **5. Impacto Ambiental (Antrópico)** | *Negativo:* La infraestructura en la nube (Azure para el backend, Firebase Hosting para el frontend, sección 5.1.1 y 7.2.1) implica consumo energético continuo de los data centers, así como el tráfico de red generado por sincronizaciones y notificaciones push frecuentes (US21–US25). *Positivo:* Al digitalizar por completo la gestión de gastos del hogar, Budgetly reduce el uso de papel, recibos físicos y hojas de cálculo impresas frente a métodos tradicionales. | *Afectados:* Impacto ambiental indirecto y distribuido a nivel global, a través del consumo energético de los proveedores cloud (Azure, Firebase/Google Cloud). *Magnitud:* Baja — el volumen de datos y usuarios actual del proyecto es reducido, pero el impacto debe considerarse si la plataforma escala. | *Acciones:* Optimizar la frecuencia de las notificaciones automáticas (TS11, TS12) para evitar envíos redundantes. Preferir consultas eficientes en la API (paginación, filtros por rango de fecha ya implementados en TS08) para reducir el procesamiento innecesario en el backend. Elegir, en la medida de lo posible, regiones de datacenter de proveedores con compromisos de energía renovable. |
| **6. Enfoque Global** | *Negativo:* El backend está desplegado en un datacenter específico (Azure, región Chile Central, sección 5.2.6), lo que implica que los datos de usuarios peruanos podrían quedar sujetos a legislaciones de protección de datos distintas a las locales. La app actualmente soporta principalmente PEN/USD (sección 4.9.2, `Household.currency`), lo que limita su aplicabilidad directa a otros mercados con distintas monedas o normativas de protección de datos financieros. *Positivo:* La arquitectura basada en API REST (5.2.7) y el diseño modular por *bounded contexts* (DDD, sección 4.8) facilitan una futura expansión internacional. | *Afectados:* Usuarios peruanos cuya información de ingresos y gastos reside en infraestructura fuera del país; usuarios potenciales en otros países no contemplados en el diseño actual de monedas y regulaciones. *Magnitud:* Media — actualmente el alcance del producto es local (Perú), pero el riesgo de cumplimiento normativo crece si el producto escala globalmente. | *Acciones:* Documentar explícitamente en el Acuerdo de Servicio SaaS (5.2.4.7) dónde se almacenan los datos y bajo qué jurisdicción. Diseñar el modelo de datos para soportar múltiples monedas y futuras normativas de protección de datos (ej. GDPR) desde el bounded context de Settings. Evaluar cifrado de extremo a extremo para los datos financieros sensibles antes de una expansión fuera de Perú. |
| **7. Revelación de Peligros y Responsabilidad** | *Riesgo:* El análisis de seguridad estática (sección 6.2.1.2-B) identificó vulnerabilidades reales en el Backend: (1) credenciales de base de datos y connection string expuestas en texto plano en el archivo de despliegue; (2) clave secreta de firma JWT versionada en el repositorio; (3) política CORS permisiva (`AllowAnyOrigin`); (4) validación de entrada insuficiente al crear un hogar (permite valores fuera de rango, confirmado también por la auditoría UX externa, hallazgo n.º 7 de la sección 6.4.2.3). | *Afectados:* La totalidad de la base de usuarios de Budgetly, cuyos datos de ingresos, gastos y credenciales podrían quedar expuestos ante un actor malicioso que explote estas vulnerabilidades (calificación de seguridad SonarCloud: E, sección 6.2.1.2-A). | *Acciones:* Como indica el código ético del ingeniero, el equipo asume responsabilidad completa sobre estos hallazgos: se rotarán de inmediato las credenciales y el secreto JWT expuestos, se migrarán a variables de entorno o a un secrets manager del proveedor cloud, se restringirá la política CORS a los orígenes conocidos del Frontend y Landing Page, y se implementarán validaciones de formato y rango tanto en frontend como en backend (ver recomendaciones detalladas en 6.2.1.2-B). Estas acciones se priorizan por encima de nuevas funcionalidades, dado el riesgo directo a la privacidad financiera de los usuarios. |


## Reflexión del equipo

El desarrollo de Budgetly plantea una tensión ética central: para cumplir su propósito de equidad financiera, el producto necesita recolectar y exponer información históricamente considerada privada —el ingreso personal— dentro de un círculo de convivencia. Esta misma característica que constituye el valor diferencial del producto (ver sección 2.1.2, Estrategias frente a competidores) es también su principal fuente de riesgo ético, social y de seguridad.

El equipo reconoce que la responsabilidad de ingeniería no termina en la implementación funcional del cálculo proporcional (`IncomeBased`, sección 4.9.2), sino que se extiende a proteger esa información sensible con el mismo rigor con el que se protegería información médica o de identidad, y a diseñar la experiencia de forma que la transparencia financiera fortalezca la convivencia en lugar de convertirse en una fuente adicional de conflicto o vigilancia dentro del hogar.
