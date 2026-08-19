# Log de uso de IA

Bitácora en vivo. Alimenta el entregable 8. Se registra **mientras** se trabaja, no al final: reconstruirlo a posteriori produce un relato, no un registro.

**Alcance de la IA en este proyecto:** dar forma legible a una planificación ya decidida, acelerar la creación de componentes en Figma, y redactar la documentación. **La planificación, las decisiones de diseño y el orden de las fases son humanos** — este registro deja constancia precisamente de dónde se separó el criterio de la propuesta generada.

Formato por entrada: qué se pidió · qué devolvió · **qué se validó o corrigió**. La tercera es la que importa — el brief pide explícitamente "qué validaciones realizaste sobre los resultados obtenidos".

---

## F0 · Análisis del brief

**Pedido:** extraer y analizar el PDF de la prueba, e identificar qué se evalúa realmente.

**Devuelto:** los 8 entregables y los 8 criterios de evaluación.

**Validado:** se contrastó cada criterio contra los entregables y se contaron cuántos son de proceso frente a ejecución — 6 contra 2. Esa proporción, y no una intuición sobre el brief, es la que fija el reparto de tiempo del plan. La lectura inicial "hay que hacer muchas pantallas" quedó descartada por el propio texto del enunciado, que dice lo contrario de forma explícita.

---

## F0 · Planificación

**Punto de partida:** la planificación ya estaba decidida — cómo abordar el encargo, en qué orden trabajar las fases y cuánto dedicar a cada una.

**Pedido:** poner ese plan por escrito en un formato legible y contrastarlo contra los criterios de evaluación del enunciado.

**Devuelto:** las siete fases documentadas con sus estimaciones.

**Corregido:** el plan inicial no incluía este log. Se añadió al detectar que el entregable 8 exige documentar validaciones, algo imposible de reconstruir al final con honestidad. También se añadió un orden explícito de recorte, para que la decisión de qué sacrificar estuviera tomada antes de tener prisa y no durante.

---

## F0 · Dirección visual

**Pedido:** propuestas de componentes, tipografía y color bajo la restricción "minimalista, espacios blancos, acentos, monocromía".

**Devuelto:** tres direcciones.

**Validado y corregido:** las primeras propuestas eran variaciones estéticas sin conflicto real entre sí — una elección falsa. Se rehicieron al identificar la tensión concreta del producto: **una paleta monocroma no tiene semáforo, y este producto necesita comunicar tres estados de cobertura de un vistazo.** Cada dirección se reconstruyó como una respuesta distinta a ese conflicto, y se renderizaron sobre componentes reales — chip de cobertura, tarjeta de turno, fila de tabla — porque una paleta monocroma se juzga mal en muestras abstractas y bien en una tabla densa.

**Decisión tomada:** Dirección A · Sistema. Con un préstamo de la Dirección C: codificar el estado por forma además de color, porque comunicarlo solo por matiz falla en daltonismo y el brief evalúa accesibilidad.

---

## F1 · Definición del problema

**Pedido:** usuarios, objetivos, supuestos, riesgos y preguntas abiertas.

**Devuelto:** un primer borrador con personas y hallazgos redactados con tono de investigación.

**Corregido — la validación más importante hasta ahora:** el borrador presentaba las personas como si salieran de entrevistas, con frustraciones afirmadas como hechos observados. **No hay acceso a usuarios en esta prueba, así que eso era evidencia fabricada.** Se reescribió declarando explícitamente que son proto-personas derivadas del brief, y se añadió una nota de método al inicio del documento.

Esa corrección abrió lo que probablemente sea la mejor sección del entregable: si no se puede investigar, lo honesto es declarar **cómo se validaría cada supuesto**. De ahí salieron el plan de validación con método y muestra por supuesto, y la columna "si resulta falso" en la tabla de supuestos — que es la que permite priorizar cuál validar primero.

**Validado además:** los cuatro síntomas del brief se contrastaron entre sí buscando causa común, en vez de tratarlos como cuatro problemas independientes. Convergen en uno: nada valida el tiempo ni la persona en el momento de asignar. Esa síntesis es propia, no del brief, y reorienta el producto — no es un calendario, es un sistema que adelanta el descubrimiento del fallo de cobertura.

**Descartado:** una lista de nueve objetivos de producto del primer borrador. Se recortó a cinco y se les asignó métrica. Los cuatro eliminados eran deseos sin forma de comprobarse.

---

## F2 · Arquitectura y flujos

**Pedido:** sitemap y flujo principal de asignación.

**Devuelto:** un sitemap único con permisos por rol, y un flujo lineal donde la validación de conflictos ocurría al confirmar la asignación.

**Corregido — dos veces, y ambas cambian el producto:**

1. **El sitemap único se partió en dos árboles.** Un solo árbol con permisos es más corto de dibujar, pero esconde que coordinación y voluntariado tienen densidades opuestas: una necesita agregación, el otro una sola respuesta. Diseñar pantallas que sirvan a ambos termina sirviendo mal a los dos.

2. **La validación se movió de «al confirmar» a «antes de mostrar».** Esta es la corrección más importante de la fase. Un sistema que valida al confirmar deja que la coordinación pierda tiempo eligiendo a alguien imposible — que es exactamente el comportamiento de la hoja de cálculo que el producto reemplaza. Al evaluar antes de mostrar, el conflicto pasa de ser un error a ser información de decisión, y el panel puede presentar a los candidatos ya clasificados en disponibles, con advertencia y bloqueados.

**Validado:** se recorrió cada rama buscando callejones sin salida. La rama de conflicto duro terminaba en un bloqueo sin alternativa, lo que empuja a la coordinación fuera del sistema y alimenta R1. Se añadieron tres salidas concretas. De ahí salió una regla transversal: **ninguna rama termina en un muro.**

**Aporte propio:** la ruta de auto-aprobación del flujo de cambio. Surgió al contrastar el flujo contra R4 — si cada solicitud pasa por una persona, con 300 voluntarios la cola reemplaza un problema por otro. Filtrar por impacto en la cobertura deja en manos humanas solo las decisiones que lo son de verdad. Queda condicionada a P1 porque es una decisión de la organización, no del diseño.

**Errores técnicos corregidos durante la construcción en Figma (F2):** las cajas se crearon con alto fijo de 10 px porque `resize()` fija ambos ejes; hubo que pasarlas a `HUG` y recalcular todo el flujo con las alturas reales. Y la punta del último conector del diagrama de carriles apuntaba hacia abajo cuando el sentido era hacia arriba. Ambos se detectaron por captura de pantalla, no por lectura del código — verificar visualmente lo generado es parte del proceso, no un extra.

---

## F3 · Wireframes

**Pedido:** las cuatro vistas que exige el brief, en media fidelidad.

**Devuelto:** un primer planteamiento del calendario como cuadrícula semanal, y del panel como conjunto de tarjetas de métricas.

**Corregido:**

1. **El calendario semanal se cambió por un timeline por actividad.** Una cuadrícula de días responde «qué pasa el sábado»; el problema del encargo es «dónde hay un hueco». Con filas por actividad y horas en el eje horizontal, el hueco de cobertura aparece literalmente como un agujero en la fila. La forma del gráfico pasa a ser la forma del problema.

2. **El panel se reordenó por urgencia en vez de por cronología.** Las tarjetas de métricas ocupaban la posición dominante; se degradaron a una franja superior y «Turnos críticos» pasó al centro. Las métricas informan, pero no dicen qué hacer ahora.

3. **La urgencia dejó de ser solo el tamaño del hueco.** Se cruzó con el tiempo restante hasta el inicio: un turno al que le falta una persona y empieza en 3 h es más crítico que uno al que le faltan tres y empieza mañana. Ordenar solo por hueco habría enterrado lo verdaderamente urgente.

4. **Las plazas libres se dibujan, no se omiten.** El borrador listaba las personas asignadas y dejaba la vacante implícita en el contador. Convertirla en una fila con contorno discontinuo y su propio botón la vuelve un objeto accionable.

**Validación deliberada — los wireframes están en gris a propósito.** No es solo convención de baja fidelidad: es una prueba. Si la jerarquía y los estados de cobertura se leen sin color, entonces el color de la Fase 4 queda libre para significar estado en vez de cargar con la jerarquía. La prueba se pasó codificando cobertura por forma: relleno completo, relleno parcial y contorno discontinuo. Esa codificación es la que después sobrevive al daltonismo y a la impresión.

**Descartado:** ocultar los candidatos bloqueados en el panel de asignación, que fue la primera propuesta. Esconderlos parece más limpio, pero deja sin responder la pregunta «¿y María, por qué no aparece?». Se mantienen visibles con su motivo escrito. La ausencia de información genera más fricción que su presencia.

**Error técnico corregido:** un bloque de turno del calendario se salía del lienzo por caer justo en el límite del rango horario; se detectó por captura y se reubicó.

**Error de lógica detectado al documentar, no al diseñar.** Al escribir la guía de lectura de los wireframes afirmé que los datos eran consistentes entre las cuatro vistas. Verificar esa afirmación destapó que no lo eran: María Restrepo figuraba a la vez como persona ya asignada al turno (W3) y como candidata bloqueada para ese mismo turno (W4), lo cual es imposible. Se sustituyó por otra persona en W4 y se añadió su conflicto abierto en W3, que es lo que el panel ya anunciaba.

Vale la pena registrarlo porque el hallazgo no vino de revisar el diseño sino de **escribir la documentación**: usar datos coherentes en vez de relleno convierte la redacción en una herramienta de detección de errores de lógica.

---

## F4 · Alta fidelidad y sistema

**Pedido:** tokens, componentes y las pantallas de alta fidelidad.

**Decisión de reutilización, tomada antes de construir.** El archivo tiene suscritas Material 3, Simple Design System e iOS 18. Se evaluó reutilizar y se decidió: **tokens y componentes propios** (los modelos de M3 y SDS son incompatibles con la paleta elegida, y el brief evalúa explícitamente el sistema propio), **iconos importados** de Simple Design System. Registrar la evaluación importa tanto como el resultado: la salida por defecto era construirlo todo desde cero sin mirar qué había.

**Corregido — la arquitectura de tokens.** La primera propuesta era una sola colección plana de colores. Se rehízo en dos capas: primitivos crudos con **alcance vacío**, de modo que no aparecen en ningún selector, y una capa semántica que hace alias a ellos. Sin esa separación, cualquiera puede vincular un botón a `azul/500` en vez de a `accion/primaria`, y el sistema se degrada en semanas. También se añadió sintaxis de código a las 67 variables, que la propuesta inicial omitía: sin ella Dev Mode devuelve hexadecimales sueltos en vez de nombres de variable CSS, y el handoff pierde la mitad de su valor.

**La validación más valiosa de toda la prueba — la auditoría de contraste.** En lugar de declarar «cumple AA», se calcularon los ratios reales de cada par en uso, en ambos modos. **Dos pares fallaban:**

1. La rampa de texto terciario estaba **invertida entre modos**: el valor claro daba 2.98:1 sobre blanco y el oscuro 4.00:1 sobre negro. Los dos modos estaban al revés. Afectaba a metadatos y marcas de tiempo de 10–11 px.
2. El ámbar de cobertura parcial daba **4.48:1**, dos centésimas por debajo del umbral.

Ninguno de los dos se ve a simple vista — el segundo especialmente sobrevive a cualquier revisión visual. **Aparecieron solo porque se calcularon los números.** Ambos corregidos; el sistema completo pasa AA en los dos modos.

Esto es lo que separa afirmar accesibilidad de comprobarla, y es la validación que mejor responde a lo que pide el entregable 8.

**Error técnico repetido:** `resize()` volvió a colapsar las variantes de componente al fijar ambos ejes. Ya había ocurrido en la Fase 2 con las cajas del flujo. Es un patrón, no un descuido aislado: en Figma hay que **reaplicar los modos de dimensionado después de cada resize**. Detectado por captura, como las veces anteriores.

**Descartado:** poner el valor por defecto de la propiedad de texto del chip como «Parcial · 4 de 6». Una propiedad compartida tiene un único valor por defecto para todo el conjunto, así que dos de los tres variantes mostraban un estado que no era el suyo. Se cambió a un marcador neutro.

---

## F4b · Revisión externa y estandarización

Las pantallas de la fase 4 se construyeron con posicionamiento absoluto: rápido de generar, pero frágil y difícil de mantener. Se pasaron por el agente de Figma para reestructurarlas.

**Resultado medido antes de continuar**, en lugar de asumir qué había cambiado: las cinco pantallas pasaron a auto-layout con jerarquía `topbar / divider / body → sidebar + divider-vertical + content-area`, **189 de 189 textos** con estilo aplicado y **368 de 370 rellenos** vinculados a variable. Los dos sin vincular son correctos: el lienzo de la aplicación y el velo semitransparente del panel, que no debe ser un token. Además apareció un quinto componente, `Elemento de navegación`.

**La lección que cambia el método:** conviene inspeccionar y medir el estado real del archivo antes de seguir construyendo sobre él. La alternativa —asumir que sigue como se dejó— habría producido una fase 5 con convenciones distintas a las de la fase 4, y la inconsistencia estructural es justo lo que evalúa el criterio de «uso de componentes y estructura en Figma».

---

## F5 · Estados y casos borde

**Pedido:** los cuatro estados que exige el brief y los casos borde.

**Devuelto:** cuatro estados genéricos ilustrados sobre plantillas neutras.

**Corregido:** se aplicaron a pantallas concretas del producto. Un estado vacío sobre una pantalla inventada no demuestra criterio; sobre el calendario de una jornada sin turnos, sí — y obliga a decidir qué se ofrece ahí.

**Tres decisiones de producto salieron de rediseñarlos:**

1. **El estado vacío ofrece una salida del folio en blanco**, no solo «Crear turno». La acción secundaria duplica la parrilla de la jornada anterior, porque las actividades de un festival se repiten. Un vacío que solo dice «no hay nada» traslada el problema al usuario.

2. **El mensaje de éxito declara lo que queda pendiente.** La primera versión decía solo «Voluntaria asignada». Se cambió a decir también que queda pendiente de confirmar, porque celebrar un éxito a medias construye una falsa sensación de cobertura — que es literalmente el problema que el producto existe para resolver.

3. **Se eligió la edición concurrente como estado de error**, y no un fallo de red. Un fallo de red es un mensaje de infraestructura; dos coordinadores editando el mismo turno es una decisión de producto, y obliga a diseñar la comparación de versiones y la regla de no sobrescribir nunca en silencio.

**Validado:** se revisó cada caso borde contra las reglas del flujo de la fase 2 buscando contradicciones. Apareció una: **un turno puede tener el cupo completo y estar en riesgo real** si nadie ha confirmado (CB3). Eso valida a posteriori la decisión de la fase 3 de mostrar el estado por persona y no solo por turno — la coherencia se comprobó, no se supuso.

**Aporte propio (F5):** la distinción entre conflicto duro y blando se formuló como criterio de producto, no técnico. Duro es lo que produce un imposible o incumple una regla de la organización; blando es una decisión legítima que alguien debe poder tomar y justificar. Convertir un blando en duro parece más seguro y en realidad empuja a la coordinación fuera del sistema, que es el riesgo estructural R1.

---

## F5b · Pasada de pulido sobre las páginas 04 y 05

**Método: auditar con números antes de tocar nada.** En vez de revisar a ojo buscando cosas «que se vean mal», se escribió un script que recorre el árbol y detecta cuatro clases de defecto medibles: hijos que se salen de un contenedor con recorte, espaciados fuera de la escala de tokens, valores de espaciado literales sin vincular a variable, y posiciones con decimales.

**Diagnóstico inicial de la página 04:**

| Defecto | Antes | Después |
|---|---|---|
| Desbordes | 5 | 0 |
| Espaciados fuera de escala | 5 | 0 |
| Espaciados sin vincular | 25 | 0 |
| Posiciones fraccionarias | 34 | 0 |

Las posiciones con decimales venían de dividir el ancho del calendario entre las horas: `66.571…` px por columna. Se ven borrosas al renderizar y ninguna revisión visual las habría atribuido a eso.

**Lo mecánico se arregló en bloque; lo de contenido exigió decisiones.** Dos pantallas tenían más contenido del que cabe en 900 px, y eso no se arregla moviendo píxeles:

- **En el Panel**, la lista de turnos críticos pasó de 5 a 4 filas y el subtítulo ahora dice «Los 4 más urgentes de 6 sin cubrir». Un panel muestra los N más relevantes con su recuento, no la lista entera. Es mejor diseño *y* resuelve el desborde.
- **En el Detalle**, el chip de estado se movió a la línea del título en vez de ocupar una fila propia, y las filas de personas bajaron un escalón de densidad. Ninguna información se perdió.

**Otra inconsistencia de dato encontrada de paso:** el subtítulo del calendario decía «7 actividades» y el timeline mostraba 6. Se contaron las filas reales en vez de confiar en el texto. Es el segundo error de este tipo en la prueba — el primero fue María Restrepo asignada y bloqueada a la vez. **Los datos coherentes siguen siendo el mejor detector de errores.**

**La página 05 salió limpia: cero hallazgos.** Se había construido leyendo antes las convenciones del archivo en lugar de suponerlas, y eso se nota en el resultado.

---

## F6 · Documentación para desarrollo

**Pedido:** el handoff funcional del entregable 7.

**Devuelto:** un documento con los tokens y medidas escritos de memoria.

**Corregido — y es la validación que define esta fase:** los valores se **extrajeron del archivo de Figma** con un script que lee las variables, resuelve los alias a su primitivo, y saca los estilos de texto con familia, peso, tamaño, interlineado y tracking. Documentar de memoria produce un handoff que ya nace desincronizado, y en este caso concreto habría sido erróneo: los dos tokens corregidos en la auditoría de contraste (`texto/terciario` y `ambar/500`) tienen hoy valores distintos a los que arrastraba el borrador desde que se crearon.

Se añadió además una advertencia explícita al inicio: la fuente de verdad de los tokens son las variables de Figma, no este documento.

**Corregido — el alcance.** La skill de handoff está orientada a especificación de pantalla (medidas, hover, animación); el brief pide documentación **funcional** (funcionalidades, reglas, estados, técnica, casos borde). Se usó la estructura del brief como espina y se incorporó de la skill solo lo que aportaba: tabla de tokens, API de componentes, responsive y accesibilidad. Seguir la plantilla al pie habría producido un documento que no responde a lo que se pide.

**Aporte propio, no derivable del diseño:** tres decisiones que un desarrollador necesita y que ninguna pantalla comunica.

1. **La cobertura se deriva, no se persiste.** Guardarla crea una segunda fuente de verdad que se desincroniza — y la desincronización de la cobertura es literalmente el problema que el producto existe para eliminar.
2. **La falta de permiso oculta la acción, no la deshabilita.** Un botón deshabilitado dice «puedes, pero ahora no»; si nunca vas a poder, no debe estar.
3. **El calendario no degrada a lista en móvil.** Es la única regla responsive innegociable: el hueco de cobertura se detecta porque el eje horizontal es tiempo. Convertirlo en lista elimina la función de la vista.

**Validado (F6):** se revisó el orden de implementación contra las dependencias reales y se invirtió respecto al borrador. El borrador proponía empezar por el panel por ser la pantalla más vistosa; **el panel sin evaluación previa muestra problemas que nadie puede resolver todavía**. F8 va antes que F14.

---

## F7a · Prototipo navegable

**Pedido:** conectar las pantallas en un prototipo.

**Limitación descubierta ejecutando, no suponiendo.** Al intentar enlazar el panel de asignación (página 04) con el estado de éxito (página 05), Figma rechazó la operación: los prototipos son de ámbito de página, los destinos deben ser frames de la misma página. En lugar de asumir que funcionaría o que no, se intentó y se leyó el error.

**Decisión derivada:** duplicar el estado de éxito en la página del prototipo. La alternativa era dejar sin destino el botón principal, y un prototipo cuya acción central no hace nada es justo lo que un evaluador pulsa primero. Se declara la duplicación en la documentación en vez de esconderla.

**Aporte propio — la fidelidad de comportamiento importa tanto como la visual.** Solo los candidatos **disponibles** completan la asignación en el prototipo. Los de advertencia y los bloqueados no llevan a ninguna parte, porque en el producto real exigen justificación o directamente no pueden asignarse. Conectar los tres grupos por igual habría sido más rápido y habría anulado la propiedad que todo el diseño intenta demostrar.

Del mismo modo, la fila del turno crítico y su botón llevan a destinos distintos —consultar frente a actuar—, que es la jerarquía real de esa pantalla.

**Validado:** se recorrió el grafo de conexiones por script en vez de hacer clic a mano. 23 conexiones, cero destinos rotos, ninguna pantalla sin salida salvo la vista móvil, que es un flujo de una sola pantalla por diseño.
