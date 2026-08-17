# 02 · Arquitectura y flujos

Entregable 2 de 8. Los tres diagramas viven en Figma, en la página **`02 · Arquitectura y Flujos`**.

> Archivo: `Relevo · Gestión de Turnos` → https://www.figma.com/design/1aZh7f3yjYkD9YV5ju3zRx
>
> La exportación a PNG de esta carpeta es un paso manual desde Figma (`Export` sobre cada frame). Los diagramas son la fuente de verdad; este documento explica las decisiones que no se leen en un diagrama.

---

## Sitemap · dos árboles, no uno

**Decisión:** la coordinación y el voluntariado no navegan el mismo producto, así que no comparten árbol.

Un sitemap único con permisos por rol habría sido más corto de dibujar, pero habría escondido la diferencia real: son dos productos con densidades opuestas. La coordinación necesita **agregación** — ver 300 personas y decenas de turnos a la vez. El voluntariado necesita **una sola respuesta**. Meterlos en el mismo árbol obliga a diseñar pantallas que sirvan a ambos, y esas pantallas terminan sirviendo mal a los dos.

Ambos árboles se quedan en **tres niveles**. Con más profundidad, la coordinación pierde de vista dónde está durante el evento, que es justo cuando menos atención tiene disponible.

### Coordinación · escritorio (S2)

Seis secciones de primer nivel: Panel, Turnos, Actividades, Voluntarios, Solicitudes y Configuración.

**Panel** va primero porque el problema de origen es de visibilidad, no de edición. Sus cuatro bloques — cobertura por jornada, turnos críticos, conflictos abiertos y solicitudes pendientes — son las cuatro preguntas que hoy la hoja de cálculo no responde.

**Solicitudes** es una sección propia y no una bandeja dentro de Turnos. Es una decisión deliberada contra R1: si el cambio de turno no tiene un lugar visible en la navegación, la conversación vuelve a WhatsApp.

### Voluntariado · móvil (S1)

Cinco secciones, y la primera es **Mi próximo turno**, no un calendario.

Esta es la decisión de arquitectura más importante del entregable. La proto-persona voluntaria consulta de pie, en la calle, con prisa y con conectividad irregular (S1, R3). Su pregunta no es *"cómo está el evento"* sino *"dónde tengo que estar ahora"*. Un calendario responde la primera pregunta muy bien y la segunda muy mal: obliga a localizar el día, encontrar la franja e interpretar la cuadrícula.

Por eso el calendario existe pero baja a segundo nivel, y el inicio es una respuesta directa. Está anotado en el propio diagrama para que la decisión se lea sin necesitar este documento.

---

## Flujo principal · asignar con detección de conflicto

Fusiona los escenarios 1 y 3 que ofrecía el brief. **La detección de conflicto no es un flujo aparte: ocurre dentro de la asignación.** Tratarlos por separado habría implicado diseñar un producto que primero deja crear el conflicto y luego ofrece resolverlo — exactamente el comportamiento de la hoja de cálculo que estamos reemplazando.

### El paso que define el producto

Antes de mostrar un solo candidato, el sistema evalúa a cada uno contra cinco criterios: disponibilidad declarada (S5), requisitos de la actividad (S6), solapamiento con otros turnos, descanso mínimo y carga horaria acumulada.

El resultado es que **el panel de asignación presenta a los candidatos ya clasificados en tres grupos** — disponibles, con advertencia y bloqueados. El conflicto se ve *antes* de elegir, no después de confirmar.

Esa inversión es toda la propuesta de valor. Un sistema que valida al confirmar sigue permitiendo que la coordinación pierda tiempo eligiendo a alguien imposible; uno que valida antes de mostrar convierte el conflicto en información de decisión.

### Las tres ramas

| Rama | Qué la dispara | Comportamiento |
|---|---|---|
| **Sin conflicto** | El candidato cumple los cinco criterios | Se crea la asignación en «Pendiente de confirmación». Se avisa por el canal habitual de la organización (P7). Éxito con deshacer |
| **Conflicto blando** | Descanso por debajo del mínimo, fuera de la disponibilidad declarada, o supera el máximo de horas | Advierte **qué regla y con qué turno**. Permite asignar igualmente dejando una justificación, que queda en el historial. La asignación se marca como excepción |
| **Conflicto duro** | Solapamiento real de horario, o requisito de actividad no cumplido | Bloquea. Ofrece ver el turno en conflicto, sugerir tres alternativas viables, o liberar la otra asignación |

**Regla de diseño transversal:** ninguna rama termina en un muro. Incluso el conflicto duro devuelve al panel con salidas concretas. Un bloqueo sin alternativa empuja a la coordinación fuera del sistema — de nuevo R1.

**P3 sigue abierta** y decide dónde cae el descanso mínimo: si es una obligación legal es conflicto duro, si es una recomendación interna es blando. El diagrama lo asume blando y lo declara.

### Cierre del bucle

El flujo termina recalculando la cobertura del turno. Si el cupo queda completo pasa a «Cubierto» y desaparece de los turnos críticos del panel — que es donde empezó el recorrido. Ese cierre es lo que convierte el panel en un instrumento vivo y no en un informe.

---

## Flujo secundario · solicitud de cambio

Dibujado en **carriles por rol** — voluntariado, sistema y coordinación — porque el problema de este flujo no son los pasos sino los traspasos. Cada salto entre carriles es un punto donde hoy la información se pierde en la mensajería.

### La decisión de producto: no todo cambio necesita aprobación

El sistema calcula el impacto en la cobertura **antes** de enrutar, y de ahí salen dos rutas:

- **Ruta A · la baja no deja el turno por debajo del cupo.** Puede auto-aprobarse, si la organización lo habilita (P1).
- **Ruta B · la baja sí deja el turno descubierto.** Escala a la coordinación de área, que revisa con el impacto ya calculado en vez de tener que deducirlo.

La Ruta A es una respuesta directa a **R4**, el riesgo de que la coordinación se convierta en cuello de botella. Con 300 voluntarios y cambios de último minuto, hacer pasar cada solicitud por una persona crea una cola que reemplaza un problema por otro. Filtrar por impacto deja en manos humanas solo las decisiones que de verdad lo son.

Está condicionada a P1 porque es una decisión de la organización, no del diseño. El diagrama la muestra como ruta habilitable, no como comportamiento impuesto.

### Por qué el motivo es obligatorio

Es el único campo obligatorio del flujo. Sin él, la coordinación recibe una solicitud que no puede evaluar y tiene que preguntar por otro canal — reintroduciendo el doble canal que el producto existe para eliminar.

---

## Trazabilidad

| Elemento del diagrama | Origen en el entregable 1 |
|---|---|
| Inicio del voluntariado en «Mi próximo turno» | S1 · móvil en campo · R3 · conectividad |
| Evaluación previa de candidatos | O2 · eliminar solapamientos |
| Disponibilidad como criterio | S5 |
| Requisitos de actividad como conflicto duro | S6 · pendiente de P6 |
| Descanso mínimo como conflicto blando | P3 abierta |
| Solicitudes como sección propia | R1 · doble canal |
| Auto-aprobación por impacto | R4 · cuello de botella · P1 abierta |
| Salidas en el conflicto duro | R1 · evitar la fuga fuera del sistema |
| Recálculo de cobertura al cerrar | O1 · ningún turno descubierto |
