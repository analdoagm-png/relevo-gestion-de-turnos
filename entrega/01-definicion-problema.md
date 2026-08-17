# 01 · Definición del problema

**Relevo** — Gestión de turnos para voluntarios en un festival cultural de tres días.
Entregable 1 de 8. Prueba técnica UX/UI.

---

## Nota de método

No hay acceso a usuarios reales en el marco de esta prueba. Por eso **no se presentan hallazgos de investigación**: inventar citas o datos de campo sería fabricar evidencia, y una definición sostenida sobre evidencia falsa es peor que una sostenida sobre supuestos declarados.

Lo que sigue se construye con tres materiales, cada uno etiquetado:

- **Proto-personas** derivadas del contexto del brief, marcadas como tales.
- **Supuestos numerados** (`S1`–`S10`), referenciables desde el resto de los entregables.
- **Un plan de validación** que indica con qué método se comprobaría cada supuesto si el proyecto fuera real.

Esa última parte es la que convierte un documento de supuestos en un documento accionable: declara no solo qué asumimos, sino cómo dejaríamos de asumirlo.

---

## 1. El problema, en una frase

Una organización coordina más de 300 voluntarios en tres jornadas usando hojas de cálculo y mensajería, dos herramientas que **no conocen el concepto de tiempo ni el de persona**. No pueden detectar que alguien está en dos sitios a la vez, ni decir qué turno quedó descubierto cuando alguien canceló.

El resultado no es solo ineficiencia administrativa: es que **el fallo se descubre cuando ya ocurrió** — cuando el puesto está vacío y el festival está abierto al público.

Los cuatro síntomas que enumera el brief son consecuencias de esa única causa:

| Síntoma del brief | Causa raíz |
|---|---|
| Conflictos de horarios | Nada valida el solapamiento en el momento de asignar |
| Dificultad para asignar recursos | La disponibilidad y las habilidades no son consultables |
| Cambios de último momento | El canal de cambios es mensajería: sin estado, sin trazabilidad |
| Falta de visibilidad de cobertura | No existe una vista que agregue el estado de todos los turnos |

**Implicación de diseño:** el producto no es un calendario. Es un sistema que hace visible el riesgo de descobertura *antes* de que se materialice. Todo lo demás es soporte de eso.

---

## 2. Usuarios involucrados

> Proto-personas. Construidas desde el contexto del brief, no desde entrevistas.

### Coordinación general · "Marta"

Responsable de que las tres jornadas estén cubiertas. Arma la parrilla completa semanas antes y la sostiene durante el evento.

- **Contexto de uso:** escritorio en oficina durante la planificación; portátil en la carpa de producción durante el festival, con interrupciones constantes.
- **Trabajo a resolver:** *"Cuando reviso el estado del evento, quiero ver de inmediato qué va a fallar, para poder actuar antes de que ocurra."*
- **Frustración actual:** la hoja de cálculo no le avisa de nada. Se entera de un hueco cuando alguien no aparece.
- **Volumen que maneja:** ~300 voluntarios, decenas de actividades, tres jornadas. **No puede revisar turno por turno.**

### Coordinación de área · "Diego"

Responsable de una actividad concreta — acreditaciones, escenario, hidratación. Conoce a su gente por nombre.

- **Contexto de uso:** escritorio antes del evento; móvil o tablet en campo, de pie, con una mano.
- **Trabajo a resolver:** *"Cuando alguien me falla, quiero encontrar un reemplazo que pueda de verdad, sin tener que preguntar uno por uno."*
- **Frustración actual:** no ve la disponibilidad global. Pide gente por WhatsApp y se cruza con otros coordinadores pidiendo a las mismas personas.
- **Tensión de diseño:** necesita autonomía para resolver lo suyo, pero sus decisiones afectan la cobertura global. Sus permisos deben ser acotados sin volverlo dependiente.

### Voluntariado · "Sofía"

Dona su tiempo. No es personal contratado: **su relación con la herramienta es voluntaria y su tolerancia a la fricción es baja.**

- **Contexto de uso:** móvil, en la calle, con conectividad irregular y prisa.
- **Trabajo a resolver:** *"Cuando llego al festival, quiero saber sin dudas dónde tengo que estar ahora, para no llegar tarde ni al sitio equivocado."*
- **Frustración actual:** la información llega en mensajes sueltos. Nunca sabe cuál es la versión vigente ni a quién avisar si le surge algo.
- **Consecuencia de diseño:** para este perfil, **la pantalla principal no es un calendario: es la respuesta a "¿qué me toca ahora?"**. El calendario es la vista secundaria.

---

## 3. Objetivos de producto

Formulados como metas medibles. Son **objetivos a validar con la organización**, no compromisos verificados — el valor de escribirlos así es que hacen falsable el diseño.

| | Objetivo | Métrica |
|---|---|---|
| **O1** | Ningún turno llega descubierto al inicio de su jornada | Turnos sin cobertura al abrir jornada < 5% |
| **O2** | Eliminar los solapamientos de horario | 0 asignaciones solapadas activas en el sistema |
| **O3** | El voluntario sabe qué le toca sin preguntar | Identificar el próximo turno en < 10 s desde abrir la app |
| **O4** | Los cambios dejan de vivir en mensajería | Solicitud de cambio resuelta en < 2 h de mediana |
| **O5** | Armar la parrilla deja de ser un proyecto en sí mismo | Parrilla inicial de 3 jornadas montada en < 1 jornada de trabajo |

O1 y O2 son los objetivos primarios: atacan directamente los dos primeros síntomas del brief. O3, O4 y O5 sostienen a los anteriores — sin adopción del voluntariado (O3) y sin un canal de cambios con estado (O4), la cobertura se degrada aunque la parrilla inicial sea perfecta.

---

## 4. Supuestos

Numerados para poder referenciarlos desde los demás entregables. Cada uno indica **qué se rompe si resulta falso** — que es la información que decide cuál validar primero.

| | Supuesto | Si resulta falso… |
|---|---|---|
| **S1** | El voluntariado tiene smartphone con datos y lo usa durante el evento | Cae toda la vista de voluntario; habría que volver a un modelo de impresión y avisos por SMS |
| **S2** | La coordinación trabaja en escritorio para planificar | El calendario denso deja de ser viable; habría que rediseñar la gestión para móvil |
| **S3** | Existe un rol intermedio de coordinación de área con permisos acotados | El modelo de permisos se simplifica a dos roles, pero la coordinación general se vuelve cuello de botella con 300 personas |
| **S4** | Los cambios de turno requieren aprobación humana; no son automáticos | El flujo de cambio se simplifica mucho, pero aparece riesgo de descobertura por intercambios no supervisados |
| **S5** | El voluntariado declara su disponibilidad antes del evento | La asignación pierde su insumo principal y pasa a ser por invitación y confirmación |
| **S6** | Las actividades tienen requisitos (habilidades, certificaciones, edad mínima) que condicionan quién puede cubrirlas | Desaparece una regla de validación, pero el producto se vuelve más simple |
| **S7** | Una persona puede cubrir turnos en varias actividades a lo largo del evento | La asignación se vuelve trivial y el conflicto de horarios casi desaparece |
| **S8** | La conectividad en el recinto es irregular | Deja de hacer falta tolerancia a desconexión; se simplifica la arquitectura |
| **S9** | El evento ocurre en un único huso horario, con fechas fijas conocidas de antemano | Aparece manejo de husos y el modelo de datos se complica notablemente |
| **S10** | Los datos de las 300 personas ya existen en una hoja de cálculo | Hace falta diseñar además el flujo de alta y registro individual, que hoy queda fuera de alcance |

**Los tres críticos son S1, S5 y S10.** Si S1 es falso, la mitad del producto no tiene destinatario. Si S5 es falso, cambia el flujo principal completo. Si S10 es falso, falta un flujo de alta que esta entrega no contempla.

---

## 5. Riesgos identificados

| | Riesgo | Por qué importa | Mitigación desde el diseño |
|---|---|---|---|
| **R1** | **Doble canal.** La herramienta se adopta pero los cambios se siguen negociando por WhatsApp | Es el riesgo que **mata el producto**: si la fuente de verdad no es única, vuelven todos los síntomas originales | Que solicitar un cambio en la herramienta sea más rápido que escribir el mensaje. Notificar al voluntariado por su canal habitual, pero resolver siempre dentro |
| **R2** | **Adopción desigual.** Parte del voluntariado no es tecnológico | Datos incompletos degradan la confianza en la cobertura mostrada | Vista de voluntario sin registro complejo, con enlace directo y lectura sin fricción. Que la coordinación pueda actuar en nombre de alguien |
| **R3** | **Conectividad en recinto** (S8) | Consultar el turno propio es justo lo que ocurre en el peor punto de cobertura | Vista de voluntario ligera y cacheada; indicar antigüedad del dato en lugar de fallar en silencio |
| **R4** | **Cuello de botella en aprobaciones.** Todo cambio pasa por una persona | Con 300 voluntarios y cambios de último minuto, la cola se vuelve el nuevo problema | Delegar aprobación en coordinación de área (S3); aprobación en un toque desde móvil; auto-aprobar los intercambios que no generan conflicto ni descobertura |
| **R5** | **Carga inicial de datos** (S10) | Migrar 300 registros a mano es una barrera de adopción antes de empezar | Contemplar importación desde hoja de cálculo como parte del arranque, no como función avanzada |
| **R6** | **Datos personales de 300 personas** | Contactos, disponibilidad y a veces datos sensibles como certificaciones médicas | Visibilidad por rol; la coordinación de área solo ve a su gente. Declarar retención de datos posterior al evento |

R1 es el riesgo estructural. Los otros cinco son manejables con decisiones de producto; R1 depende de que la herramienta sea genuinamente más cómoda que el canal que reemplaza, y eso se juega en el diseño de interacción, no en las funcionalidades.

---

## 6. Preguntas abiertas

Lo que preguntaría antes de seguir diseñando, ordenado por cuánto cambiaría el diseño la respuesta.

| | Pregunta | Qué cambia según la respuesta |
|---|---|---|
| **P1** | ¿Quién puede aprobar un cambio de turno: solo la coordinación general, o también la de área? | Define el modelo de permisos y si R4 se materializa |
| **P2** | ¿Cómo se declara hoy la disponibilidad, y cuánta gente la declara realmente? | Valida S5. Si la cobertura de disponibilidad es baja, la asignación debe funcionar sin ella |
| **P3** | ¿Existe un mínimo legal o interno de descanso entre turnos, y un máximo de horas por persona? | Define si el descanso es un conflicto duro que bloquea o uno blando que advierte |
| **P4** | ¿Qué pasa hoy cuando alguien no se presenta? ¿Hay lista de reserva o se cubre improvisando? | Determina si hace falta modelar suplencias, que cambiaría bastante el alcance |
| **P5** | ¿Los turnos pueden cruzar la medianoche entre jornadas? | Afecta directamente el modelo de datos y la vista de calendario |
| **P6** | ¿Las actividades tienen requisitos formales que impidan asignar a alguien, o son preferencias? | Valida S6 y define si el requisito bloquea o solo advierte |
| **P7** | ¿Qué canal usa hoy la organización para avisar al voluntariado, y se puede integrar? | Decide si las notificaciones son internas o se apoyan en el canal existente. Clave para R1 |
| **P8** | ¿El voluntariado puede rechazar una asignación, o se asume aceptada al asignarla? | Cambia el estado inicial de la asignación y todo el flujo de confirmación |

---

## 7. Alcance declarado

Lo que **sí** resuelve esta propuesta: creación de actividades y turnos, asignación con detección de conflictos, visibilidad de cobertura, consulta y confirmación por parte del voluntariado, y solicitud y resolución de cambios de turno.

Lo que queda **deliberadamente fuera**, y por qué:

- **Reclutamiento y alta de voluntarios** — se asume una base ya existente (S10). Es un producto adyacente con su propio flujo.
- **Control de asistencia y fichaje** — cambia el foco de planificación a operación en tiempo real. Es la evolución natural del producto, no su primera versión.
- **Certificados y reconocimiento de horas** — posterior al evento; no toca ninguno de los síntomas del brief.
- **Comunicación masiva** — se apoya en el canal existente de la organización (P7). Competir con él alimentaría R1 en lugar de reducirlo.

Declarar lo que queda fuera es parte de la propuesta: un producto que intenta cubrir los cuatro puntos anteriores en su primera versión llega tarde al festival.

---

## 8. Plan de validación

Si el proyecto fuera real, así se dejaría de asumir. Ordenado por prioridad, no por método.

| Prioridad | Supuesto a validar | Método | Muestra |
|---|---|---|---|
| 1 | S1, S5 · disponibilidad y uso de móvil | Encuesta al voluntariado ya registrado | 100+ respuestas |
| 2 | S3, S4, P1, P4 · roles, aprobaciones y qué se hace hoy ante una ausencia | Entrevistas con la coordinación | 5–8 personas |
| 3 | P3, P6 · reglas de descanso, límites y requisitos por actividad | Entrevista con la dirección de la organización + revisión documental | 1–2 sesiones |
| 4 | Arquitectura de información y vocabulario | Card sorting sobre actividades, turnos y estados | 15–30 participantes |
| 5 | El flujo de asignación diseñado | Test de usabilidad moderado sobre el prototipo | 5–8 coordinadores |
| 6 | El comportamiento real durante el evento | Estudio diario acotado a las tres jornadas | 10–15 voluntarios |

Los dos primeros bloques son los que desbloquean el diseño; los demás lo refinan. Con las respuestas de 1 y 2 se puede empezar a construir con confianza razonable.

---

## Resumen para quien tenga prisa

El problema no es la ausencia de un calendario: es que **el fallo de cobertura se descubre cuando ya ocurrió**. El producto existe para adelantar ese descubrimiento.

Tres usuarios con necesidades divergentes — la coordinación general necesita agregación, la de área necesita autonomía, el voluntariado necesita una sola respuesta clara. El diseño se sostiene sobre diez supuestos declarados, de los cuales S1, S5 y S10 son los críticos, y enfrenta un riesgo estructural (R1, el doble canal) que no se resuelve con funcionalidades sino con que la herramienta sea más cómoda que el WhatsApp al que reemplaza.
