# 08 · Decisiones de diseño

Entregable 8 de 8. Relevo · gestión de turnos para voluntariado.

---

## Cómo abordé el problema

El brief enumera cuatro síntomas: conflictos de horarios, dificultad para asignar recursos, cambios de último momento y falta de visibilidad. En vez de tratarlos como cuatro problemas, busqué qué tienen en común.

Convergen en uno: **las hojas de cálculo y la mensajería no conocen el concepto de tiempo ni el de persona.** No pueden detectar que alguien está en dos sitios a la vez, ni decir qué turno quedó descubierto cuando alguien canceló.

Eso desplaza el problema real. No es ineficiencia administrativa: es que **el fallo se descubre cuando ya ocurrió** — cuando el puesto está vacío y el festival abierto al público.

De ahí sale la definición que orientó todo lo demás:

> Relevo no es un calendario. Es un sistema que hace visible el riesgo de descobertura **antes** de que se materialice.

Todo lo que vino después se juzgó contra esa frase. El panel se ordena por urgencia porque su trabajo es decir qué va a fallar. El calendario es un timeline por actividad porque así el hueco aparece como un agujero en la fila. Y la asignación evalúa antes de mostrar porque validar al confirmar sigue permitiendo crear el problema.

---

## Supuestos

Diez, numerados y declarados en el [entregable 1](01-definicion-problema.md), cada uno con **qué se rompe si resulta falso** — que es la información que decide cuál validar primero.

Tres son críticos:

- **S1** · el voluntariado tiene smartphone y lo usa durante el evento. Si es falso, la mitad del producto no tiene destinatario.
- **S5** · la disponibilidad se declara antes del evento. Si es falso, la asignación pierde su insumo principal y cambia el flujo completo.
- **S10** · los datos de las 300 personas ya existen en una hoja de cálculo. Si es falso, falta un flujo de alta que esta entrega no contempla.

**No simulé investigación.** No hubo acceso a usuarios, así que las personas están declaradas como proto-personas y no hay una sola cita inventada. En su lugar, el entregable 1 cierra con un plan de validación: qué método y qué muestra usaría para dejar de asumir cada cosa. Preferí un documento honesto sobre supuestos que uno convincente sobre evidencia falsa.

---

## Qué prioricé

**El flujo de asignación con detección de conflicto.** Fusioné dos de los tres escenarios que ofrecía el brief, porque separarlos implicaba diseñar un producto que primero deja crear el conflicto y luego ofrece resolverlo — exactamente el comportamiento que estamos reemplazando.

Después: visibilidad de cobertura, confirmación por parte del voluntariado, y solicitudes de cambio con resolución por impacto.

### Qué dejé fuera a propósito

Reclutamiento y alta de voluntarios, control de asistencia en vivo, certificados de horas y comunicación masiva.

Los cuatro son productos adyacentes razonables. Ninguno toca los síntomas del brief, y un producto que intenta cubrirlos en su primera versión llega tarde al festival. Declararlo es parte de la propuesta.

---

## Las decisiones que considero más importantes

### 1 · Evaluar antes de mostrar, no validar al confirmar

El borrador validaba los conflictos al confirmar la asignación. Moverlo a **antes de mostrar los candidatos** es la decisión que define el producto.

Un sistema que valida al confirmar deja que la coordinación pierda tiempo eligiendo a alguien imposible. Al evaluar antes, el panel presenta a los candidatos ya clasificados en disponibles, con advertencia y bloqueados: **el conflicto deja de ser un error y pasa a ser información de decisión.**

### 2 · Dos árboles de navegación, no uno con permisos

Coordinación y voluntariado tienen densidades opuestas: una necesita agregar 300 personas, el otro necesita una sola respuesta. Un árbol compartido obliga a diseñar pantallas que sirvan a ambos, y esas pantallas terminan sirviendo mal a los dos.

La consecuencia visible: el inicio del voluntariado es «Mi próximo turno», no un calendario. Un calendario responde «cómo está el evento» muy bien y «dónde tengo que estar ahora» muy mal.

### 3 · El estado nunca se comunica solo por color

Disco lleno, medio disco, anillo vacío — más color, más texto. Tres señales simultáneas.

No es una capa de accesibilidad añadida al final: **se diseñó primero.** Por eso los wireframes están en gris, como prueba de que la jerarquía se sostiene sin color. Superada esa prueba, el color quedó libre para significar estado en lugar de cargar también con la jerarquía.

### 4 · El estado es por persona, no solo por turno

«4 de 6» esconde que dos de esas cuatro aún no confirmaron. Un turno puede tener el cupo lleno y estar en riesgo real.

Esta decisión se tomó en los wireframes y se **confirmó** al auditar los casos borde: apareció como CB3 de forma independiente. La coherencia quedó comprobada, no supuesta.

### 5 · Ninguna rama termina en un muro

El riesgo estructural del proyecto no es técnico: es que la herramienta se adopte y los cambios se sigan negociando por WhatsApp. Si la fuente de verdad no es única, vuelven todos los síntomas.

Eso se combate con fricción, no con funcionalidades. De ahí que el conflicto duro ofrezca tres salidas en vez de un bloqueo seco; que las solicitudes de cambio tengan sección propia en la navegación; que una solicitud sin impacto en la cobertura pueda auto-aprobarse en vez de hacer cola; y que la distinción duro/blando se defina como criterio de producto, con la regla **ante la duda, blanda**.

---

## El papel de la IA, y qué validé

**La planificación, las decisiones de diseño y el orden de las fases fueron míos.** Definí cómo abordar el encargo, en qué secuencia trabajar y cuánto dedicar a cada parte antes de empezar; el replanteo del problema, qué priorizar, qué dejar fuera y cada resolución de producto salieron de contrastar opciones contra los riesgos declarados en el entregable 1.

La IA intervino en tres frentes acotados:

| Frente | Qué aportó |
|---|---|
| **Visualización de la planificación** | Poner por escrito y dar forma legible al plan que ya tenía decidido — fases, presupuesto de tiempo, orden de recorte |
| **Creación de componentes** | Construir la librería y las pantallas en Figma vía API — trabajo mecánico y repetitivo |
| **Documentación** | Redactar y mantener consistentes ocho documentos que se referencian entre sí |

En los tres casos lo que hubo que aportar fue criterio: qué construir, en qué orden y bajo qué regla. La bitácora completa está en [log-ia.md](log-ia.md), escrita durante el trabajo y no reconstruida al final.

### El patrón que hay que vigilar

**La generación automática optimiza por parecer completa.** Sus fallos no fueron errores obvios sino salidas plausibles: personas con frustraciones redactadas como hechos observados, valores de token recordados en vez de leídos, estados vacíos genéricos, un prototipo con todos los caminos conectados por igual. Todo se veía terminado.

**Por eso el trabajo de revisión consistió, casi siempre, en decidir qué era realmente cierto.**

### Las validaciones que cambiaron el resultado

**Auditoría de contraste con números.** En vez de declarar «cumple AA», calculé los ratios de cada par en uso, en ambos modos. Dos fallaban: la rampa de texto terciario estaba **invertida entre modos** (2.98:1 sobre blanco) y el ámbar de cobertura parcial daba **4.48:1**, dos centésimas por debajo del umbral. Ninguno se ve mirando. Aparecieron porque calculé.

**Datos coherentes como detector de errores.** Usé los mismos turnos y personas en todas las pantallas en lugar de contenido de relleno. Eso destapó dos contradicciones lógicas: María Restrepo figuraba a la vez como asignada a un turno y como candidata bloqueada para ese mismo turno; y el calendario decía «7 actividades» mostrando 6. La segunda apareció al escribir la documentación, no al revisar el diseño.

**Negarme a fabricar evidencia.** El primer borrador del entregable 1 presentaba las proto-personas como si salieran de entrevistas. No hay acceso a usuarios en una prueba técnica: eso era inventar datos. Reescribirlo con una nota de método abrió la mejor sección del documento — el plan de validación por supuesto.

**Extraer en vez de recordar.** Los tokens del handoff salieron del archivo por script, resolviendo cada alias a su primitivo. No es purismo: redactados de memoria habrían sido justo los dos valores que fallan AA, porque el borrador arrastraba los originales y no los corregidos.

**Medir antes de construir encima.** Cuando el archivo pasó por otra herramienta, inspeccioné su estado real —189 de 189 textos con estilo, 368 de 370 rellenos vinculados— antes de seguir. Suponer que seguía como lo dejé habría producido una fase con convenciones distintas.

**Auditar con criterios medibles, no a ojo.** El pulido final no buscó cosas «que se vieran mal»: un script detectó 5 desbordes, 5 espaciados fuera de escala, 25 valores sin vincular y 34 posiciones con decimales — estas últimas invisibles en cualquier revisión visual. Todo a cero.

### Las decisiones, en cambio, fueron humanas

La reformulación del problema, la inversión de validar-al-confirmar a evaluar-antes-de-mostrar, la auto-aprobación por impacto como respuesta al cuello de botella, la regla de que ninguna rama termina en un muro, el criterio duro/blando como decisión de producto y no técnica, y el orden en que se abordaron las fases.

Todas salieron de contrastar propuestas contra los riesgos declarados en el entregable 1 — un ejercicio de criterio, no de generación. **La IA aceleró la ejecución y amplió la cobertura de la auditoría; lo que se construyó y por qué no salió de ahí.**

---

## Si tuviera más tiempo

Validar S1, S5 y S10 con la organización antes que cualquier otra cosa: los tres pueden invalidar partes enteras del diseño. Después, resolver P3 y P6, que deciden si dos reglas son duras o blandas y por tanto cambian el comportamiento del flujo principal.

En el archivo queda deuda declarada: variantes de foco en los componentes, iconografía real, una pantalla montada en modo oscuro, y los puntos de ruptura intermedios.
