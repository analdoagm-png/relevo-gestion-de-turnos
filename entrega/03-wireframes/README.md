# 03 · Wireframes

Entregable 3 de 8. Las cuatro vistas viven en Figma, en la página **`03 · Wireframes`**.

> Archivo: `Relevo · Gestión de Turnos` → https://www.figma.com/design/1aZh7f3yjYkD9YV5ju3zRx
>
> Cada wireframe tiene su panel de notas numeradas a la derecha, en el propio lienzo. Este documento explica cómo leerlos; las notas explican cada decisión concreta.

---

## Qué hay aquí

Cuatro vistas de escritorio, en media fidelidad y escala de grises, con veintitrés notas de diseño repartidas entre ellas.

| | Vista | Qué resuelve |
|---|---|---|
| **W1** | Panel | Qué va a fallar y qué exige atención ahora |
| **W2** | Calendario y gestión de turnos | Dónde están los huecos de cobertura a lo largo de la jornada |
| **W3** | Detalle de turno | Quién está asignado, en qué estado, y qué falta |
| **W4** | Panel de asignación | Elegir a quién asignar, con el conflicto ya resuelto de antemano |

Son las cuatro que pide el brief. W3 cubre «detalle de actividad o turno» por el lado del turno, que es donde se toman las decisiones; la actividad aparece como contexto (requisitos, turnos vecinos).

---

## Cómo leerlos

### El gris es una decisión, no una fase pendiente

Los wireframes están en escala de grises **a propósito, y funcionan como una verificación**: si la jerarquía y los tres estados de cobertura se leen sin color, entonces en la fase de alta fidelidad el color queda libre para significar estado, en vez de tener que cargar también con la jerarquía.

Eso obliga a codificar la cobertura por **forma**, que es la codificación que después sobrevive al daltonismo, a la escala de grises y a la impresión — el criterio de accesibilidad que fijamos al elegir la dirección visual.

### El sistema de codificación

Es el mismo en las cuatro vistas. Aprenderlo una vez basta para leerlas todas.

**Cobertura de un turno** — bloques y barras:

| Forma | Significa |
|---|---|
| Relleno completo | Cubierto · el cupo está completo |
| Relleno parcial | Parcialmente cubierto · faltan personas |
| Contorno discontinuo | Sin cubrir · ninguna persona asignada, o plaza libre concreta |

**Estado de una persona** — círculos:

| Forma | Significa |
|---|---|
| Círculo relleno | Confirmada · aceptó la asignación |
| Círculo vacío | Pendiente · avisada, sin respuesta |

**Nunca hay un estado comunicado solo por forma o solo por color.** Siempre va acompañado de texto y número. La forma permite el vistazo; el texto elimina la ambigüedad.

### Los números son discretos, no porcentajes

La cobertura se muestra como «4 de 6» con seis segmentos visibles, no como «67 %». Con cupos que van de 3 a 8 personas, el porcentaje esconde la magnitud real: un 67 % puede ser «falta una persona» o «faltan tres», y esas dos situaciones no se gestionan igual.

### Los datos son consistentes entre vistas

No son datos de relleno. **El mismo turno recorre las cuatro pantallas** — *Montaje de escenario, sábado 14, 08:00–12:00, 4 de 6 personas* — y las personas que aparecen en una vista se comportan de forma coherente en las demás.

Eso permite leer los wireframes como un recorrido y no como cuatro imágenes sueltas. También es lo que hizo aflorar un error real durante la revisión: María Restrepo figuraba a la vez como asignada al turno (W3) y como candidata bloqueada para ese mismo turno (W4), lo cual es imposible. Se corrigió. Los datos coherentes no son decoración: son una herramienta de detección de errores de lógica.

### Los paneles de notas

A la derecha de cada wireframe, con notas numeradas. Explican **por qué** está así, no qué se ve. Un wireframe sin anotar comunica la mitad, y las decisiones que no se justifican se leen como arbitrarias.

---

## Recorrido sugerido

Las vistas están dispuestas verticalmente en el orden del flujo principal del entregable 2. Leerlas en orden reproduce el recorrido real de la coordinación:

1. **W1 · Panel** — detecta que *Punto de hidratación* no tiene a nadie y empieza en 3 h 40 min.
2. **W2 · Calendario** — sitúa el hueco en el contexto de la jornada y comprueba si hay más.
3. **W3 · Detalle de turno** — abre el turno, ve quién está y quién falta.
4. **W4 · Panel de asignación** — asigna, con los candidatos ya clasificados por conflicto.

---

## Las cuatro vistas

### W1 · Panel

Ordenado por urgencia, no por cronología. La franja superior de indicadores responde las cuatro preguntas que la hoja de cálculo no puede responder, pero el bloque dominante es **Turnos críticos** — porque el panel existe para decir qué hacer ahora, no para informar.

La urgencia cruza el hueco de cobertura con el tiempo restante hasta el inicio. Un turno al que le falta una persona y empieza en tres horas está por encima de uno al que le faltan tres y empieza mañana.

Las solicitudes de cambio tienen presencia aquí y no solo en su sección propia: es una decisión contra el riesgo del doble canal.

### W2 · Calendario y gestión de turnos

**Timeline por actividad, no cuadrícula semanal.** Las filas son actividades y el eje horizontal son las horas de la jornada. Con esa disposición el hueco de cobertura aparece como un agujero literal en la fila — la forma del gráfico es la forma del problema.

La leyenda vive dentro de la pantalla. Un sistema de codificación que necesita manual externo ya falló.

### W3 · Detalle de turno

Dos ideas sostienen esta vista.

**El estado es por persona, no solo por turno.** «4 de 6» esconde que dos de esas cuatro aún no han confirmado: la cobertura real es más frágil que el número.

**Las plazas libres se dibujan.** No son la ausencia de una fila, son una fila con contorno discontinuo y su propio botón de asignar. La vacante es el objeto sobre el que se actúa.

### W4 · Panel de asignación

La vista que concentra la propuesta. Los candidatos llegan **ya clasificados en tres grupos** — disponibles, con advertencia y bloqueados — porque el sistema los evaluó antes de mostrarlos.

Tres detalles que conviene mirar:

- El motivo del conflicto se escribe en la fila, con el turno y la regla concretos. No un icono que obligue a investigar.
- Los bloqueados se muestran, no se ocultan. Que María desaparezca de la lista solo genera la pregunta «¿y María?».
- Los candidatos con advertencia llevan **«Revisar»** en vez de «Asignar». La acción cambia de nombre porque la consecuencia es distinta: exige justificación.

---

## Qué NO está resuelto aquí, y dónde se resuelve

Declararlo evita que se lea como olvido.

| No está | Dónde aparece |
|---|---|
| Color, tipografía definitiva y tratamiento visual | Entregable 4 · alta fidelidad |
| Estados vacío, de carga, de éxito y de error | Entregable 5 |
| Iconografía y microinteracción | Entregable 4, y solo en la parte llevada a alta fidelidad |
| Comportamiento responsive y puntos de ruptura | Entregable 7 · consideraciones técnicas |
| Espaciado fino y sistema de rejilla | Entregable 4, al definir los tokens |

### Una ausencia que sí conviene señalar

**No hay wireframe de la vista de voluntariado.** Las cuatro vistas que pide el brief son todas de coordinación, así que el entregable está completo según lo solicitado.

Pero el entregable 1 sostiene que el voluntariado tiene la necesidad más distinta de todas — una sola respuesta, en móvil, en la calle — y el entregable 2 convirtió eso en una decisión de arquitectura visible: su inicio es «Mi próximo turno», no un calendario. Dejar esa decisión sin ninguna representación visual la debilita.

**Recomendación:** llevar «Mi próximo turno» directamente a alta fidelidad en el entregable 4, saltándose el wireframe. Es una pantalla simple, el contraste de densidad con la vista de coordinación se convierte en argumento a favor de la propuesta, y demuestra que el sistema de componentes escala a móvil.

---

## Trazabilidad

| Decisión en los wireframes | Origen |
|---|---|
| Panel ordenado por urgencia | E1 · el fallo se descubre cuando ya ocurrió |
| Indicadores superiores | E1 §1 · los cuatro síntomas del brief |
| Solicitudes visibles en el panel | E1 · R1, doble canal |
| Timeline por actividad | E1 · O1, ningún turno descubierto |
| Codificación por forma | Dirección visual · préstamo de la dirección C |
| Estado por persona | E1 · S4, la asignación requiere confirmación |
| Requisitos en el detalle del turno | E1 · S6, pendiente de P6 |
| Tres grupos de candidatos | E2 · evaluar antes de mostrar |
| Bloqueados visibles con motivo | E2 · ninguna rama termina en un muro |
