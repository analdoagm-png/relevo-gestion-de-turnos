# 05 · Estados y casos especiales

Entregable 5 de 8. Las cuatro pantallas de estado viven en Figma, en la página **`05 · Estados y Casos Borde`**.

> Archivo: `Relevo · Gestión de Turnos` → https://www.figma.com/design/1aZh7f3yjYkD9YV5ju3zRx

Construidas con el mismo estándar que la página 04: shell de aplicación en auto-layout, instancias de componentes reales (`Elemento de navegación`, `Botón`, `Chip de cobertura`), estilos de texto aplicados y rellenos vinculados a variables.

---

## Los cuatro estados

Aplicados a pantallas concretas, no a plantillas genéricas. Un estado vacío ilustrado sobre una pantalla inventada no demuestra nada.

### E1 · Vacío · Jornada sin turnos creados

**Cuándo aparece:** al abrir una jornada del evento antes de montar la parrilla.

La decisión que importa: **el estado vacío ofrece una salida del folio en blanco, no solo un botón de crear.** La acción secundaria es «Duplicar la parrilla del viernes», porque las actividades de un festival se repiten entre jornadas y montar la segunda desde cero es trabajo tirado. La tercera vía —importar desde hoja de cálculo— se menciona en voz baja porque conecta con el riesgo de carga inicial (R5).

Un estado vacío que solo dice «no hay nada aquí» traslada el problema al usuario. Este propone tres caminos ordenados por probabilidad de uso.

### E2 · Carga · Calendario cargando

**Cuándo aparece:** durante la carga de la parrilla de una jornada.

**El esqueleto reproduce la estructura final**, no es un bloque gris genérico: filas por actividad, etiquetas a la izquierda y bloques a distintas posiciones horizontales. Cuando llegan los datos, nada salta de sitio.

La cabecera mantiene su texto real y añade el estado en la esquina. **El encabezado de página, la barra de filtros y el marco no se esqueletizan**: son estructura, no datos, y hacerlos parpadear comunica una incertidumbre que no existe.

### E3 · Éxito · Asignación confirmada

**Cuándo aparece:** al completar una asignación desde el panel.

Tres decisiones:

- **«Deshacer» está en el mensaje**, no en un menú. Una asignación equivocada durante el evento hay que revertirla en segundos.
- **El mensaje dice la verdad completa:** la persona quedó asignada *y* queda pendiente de que confirme. Celebrar un éxito a medias construye una falsa sensación de cobertura, que es exactamente el problema de origen.
- **La cobertura se recalcula a la vista** — pasa a «5 de 6» y la nota aclara que el turno sigue en la lista de críticos. El éxito de una acción no equivale al éxito del objetivo.

El bloque inferior mantiene abierto el camino a la plaza restante, para no obligar a repetir el recorrido completo.

### E4 · Error · Edición concurrente

**Cuándo aparece:** al guardar un turno que otra persona modificó mientras tanto.

Se eligió este error, y no un fallo de red, porque **es el que revela una decisión de producto** en vez de una de infraestructura.

El mensaje sigue tres reglas:

1. **Dice qué pasó y quién lo causó**, con marca temporal: «Diego Salas cambió este turno hace 12 segundos». Un «error al guardar» genérico obliga a investigar.
2. **Tranquiliza sobre lo que no se perdió** antes de pedir una decisión. «Tu cambio no se perdió.»
3. **Ofrece tres salidas concretas**, no un botón de reintentar: comparar, aplicar encima, o descartar.

Debajo, la comparación campo a campo de las dos versiones con las diferencias marcadas. **El sistema nunca sobrescribe en silencio.** La nota al pie explica el bloqueo optimista y remite al entregable 7.

Sin apologías, sin «¡Ups!», sin culpar al usuario.

---

## Casos borde

Detectados a lo largo del diseño. La columna de comportamiento es la que alimenta el handoff del entregable 7.

| | Caso | Comportamiento esperado |
|---|---|---|
| **CB1** | Un voluntario queda asignado a dos turnos solapados | Conflicto duro: se bloquea en el momento de asignar. Si el solapamiento se genera al **mover** un turno ya existente, se advierte antes de confirmar el cambio y el turno movido queda marcado con conflicto abierto |
| **CB2** | Un turno sigue sin cubrir a pocas horas de empezar | Escala en la lista de críticos por urgencia (tiempo restante × hueco). Por debajo de un umbral configurable, notifica a la coordinación general además de a la de área |
| **CB3** | Un voluntario nunca confirma y el turno se acerca | La cobertura distingue asignado de confirmado. Al acercarse el inicio, las asignaciones sin confirmar cuentan como riesgo y el turno reaparece en críticos aunque el cupo esté completo |
| **CB4** | Cancelación de alguien ya confirmado, a última hora | La plaza vuelve a libre y el turno recalcula cobertura. Si quedan menos de X horas, se marca como incidencia y no como solicitud ordinaria: el flujo de aprobación normal no llega a tiempo |
| **CB5** | Dos coordinadores editan el mismo turno a la vez | Bloqueo optimista. Se detecta el choque al guardar y se muestra la comparación de versiones (E4). Nunca se sobrescribe en silencio |
| **CB6** | Un turno cruza la medianoche entre dos jornadas | El turno pertenece a la jornada en que **empieza**, y aparece como continuación en la siguiente. El calendario lo dibuja partido en las dos vistas con marca de continuidad. Pendiente de confirmar con P5 |
| **CB7** | Se asignan más personas que el cupo requerido | Estado «Sobreasignado», permitido pero visible. Puede ser deliberado —refuerzo en un turno pico— así que se advierte sin bloquear, y no se contabiliza como cobertura extra en las métricas globales |
| **CB8** | Un voluntario no cumple los requisitos de la actividad | Conflicto duro, aparece en el grupo Bloqueados con el requisito concreto escrito. Sujeto a P6: si los requisitos resultan ser preferencias, pasa a conflicto blando |
| **CB9** | Se elimina una actividad con voluntarios ya asignados | No se elimina en silencio. Se pide confirmación indicando cuántas personas quedan liberadas y cuántos turnos desaparecen; las asignaciones se archivan, no se borran, para conservar el historial |
| **CB10** | Se cambia el horario de un turno con gente confirmada | Las confirmaciones **se invalidan** y vuelven a pendiente: alguien confirmó otro horario. Se avisa a las personas afectadas y se revalidan solapamientos y descansos con el nuevo horario |
| **CB11** | Un voluntario solicita cambio de un turno ya empezado | La solicitud ordinaria se cierra. Pasa a incidencia en vivo, dirigida a la coordinación de área, con aviso inmediato |
| **CB12** | El cupo requerido se reduce por debajo de las personas ya asignadas | Se advierte antes de aplicar y se pide indicar a quién se retira. El sistema no elige por su cuenta a quién dar de baja |

### Los tres que más condicionan el diseño

**CB3** es el más traicionero: un turno puede tener el cupo completo y estar en riesgo real. Es lo que justifica que el estado sea **por persona** y no solo por turno, y por qué el detalle distingue «Confirmada» de «Pendiente».

**CB5** es el que obligó a diseñar el estado de error como una decisión de producto y no como un mensaje de sistema.

**CB10** es el que revela que confirmar no es un estado permanente sino un acuerdo sobre unas condiciones concretas. Si cambian las condiciones, el acuerdo caduca.

---

## Validaciones especiales

Reglas que el sistema aplica antes de permitir una acción, ya recogidas en el flujo del entregable 2:

| Validación | Tipo | Origen |
|---|---|---|
| Solapamiento de horario | Duro · bloquea | O2 |
| Requisitos de la actividad | Duro · bloquea | S6 · pendiente P6 |
| Descanso mínimo entre turnos | Blando · advierte | P3 abierta |
| Máximo de horas por jornada | Blando · advierte | P3 abierta |
| Fuera de la disponibilidad declarada | Blando · advierte | S5 |
| Cupo excedido | Blando · advierte | CB7 |
| Motivo en la solicitud de cambio | Duro · campo obligatorio | R1 |

**La distinción duro/blando no es técnica, es de producto.** Un conflicto duro es aquel en el que continuar produce un imposible físico o incumple una regla de la organización. Uno blando es aquel en el que continuar es una decisión legítima que alguien debe poder tomar y justificar. Convertir un blando en duro parece más seguro y en realidad empuja a la coordinación fuera del sistema, que es el riesgo estructural R1.

---

## Trazabilidad

| Estado o caso | Origen |
|---|---|
| Vacío con opción de duplicar | R5 · carga inicial de datos |
| Esqueleto con estructura real | Escalabilidad · evitar saltos de layout |
| Éxito que declara lo pendiente | E1 · falsa sensación de cobertura |
| Deshacer en el propio mensaje | Contexto de evento en vivo |
| Error de edición concurrente | CB5 · dos coordinadores en campo |
| Nunca sobrescribir en silencio | E1 · el fallo se descubre tarde |
| CB2 escalado por urgencia | O1 · ningún turno descubierto |
| CB3 confirmado frente a asignado | S4 · la asignación requiere confirmación |
