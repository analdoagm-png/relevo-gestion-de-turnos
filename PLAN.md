# Plan de ejecución — Prueba Técnica UX/UI

**Reto:** plataforma web de gestión de turnos para +300 voluntarios en un festival cultural de 3 días.
**Tiempo sugerido por el cliente:** 3–5 h. Este plan está timeboxeado a ~5 h.

---

## 1. Lectura del brief: qué se está evaluando realmente

El enunciado lo dice explícitamente: *"No buscamos una solución perfecta ni una gran cantidad de pantallas"*. De los 8 criterios de evaluación, **6 son de proceso y comunicación** y solo 2 son de ejecución visual:

| Criterio | Naturaleza |
|---|---|
| Pensamiento UX y definición del problema | Proceso |
| Arquitectura de información y flujos | Proceso |
| Calidad de la solución propuesta | Ejecución |
| Organización y claridad de la documentación | Proceso |
| Handoff hacia desarrollo | Proceso |
| Uso de componentes y estructura en Figma | Ejecución |
| Uso estratégico de IA | Proceso |
| Comunicación y justificación de decisiones | Proceso |

**Implicación práctica:** el riesgo de fallar la prueba no es "pocas pantallas", es "documentación floja". El presupuesto de tiempo debe cargarse hacia definición, flujos, handoff y decisiones — no hacia pulir píxeles.

### Restricciones no negociables detectadas

1. **Figma es obligatorio.** Aunque uses Lovable/v0/Framer, el entregable 6 exige *además* una versión en Figma con estructura de pantallas, flujos de navegación, componentes y organización general. No es opcional.
2. **Solo se pide UN flujo principal.** El entregable 2 dice "puedes enfocarte en alguno de estos escenarios" — elegir uno y hacerlo bien vale más que tres a medias.
3. **Solo se pide UNA parte del flujo en alta fidelidad** (entregable 4). El resto queda en wireframe.
4. **El uso de IA debe documentarse con sus validaciones** (entregable 8). Esto se registra *mientras* trabajas, no se reconstruye al final.

### Ambigüedades del brief y cómo resolverlas

- No define si los voluntarios usan móvil. Dice "herramienta web" para administrar. **Supuesto a declarar:** admin en desktop (trabajo denso, tablas y calendario), voluntario mobile-first (consulta en campo desde el teléfono). Esto se declara en el entregable 1 y se sostiene en todo el diseño.
- No define quién aprueba los cambios de turno. **Supuesto:** requieren aprobación de un coordinador; no son automáticos.
- No define si existe rol intermedio (coordinador de área). **Supuesto:** sí, con permisos acotados a su actividad. Justifica la escala de 300 voluntarios.
- No define qué pasa con un turno sin cobertura al iniciar el evento. Es un caso borde de oro para el entregable 5.

---

## 2. Decisiones de arranque (recomendadas)

Tomar estas dos decisiones **antes de abrir ninguna herramienta**; condicionan todo lo demás.

### Decisión A — Flujo principal a desarrollar

**Recomendado: "Asignar un voluntario a un turno, con detección de conflicto en el camino."**

Fusiona los escenarios 1 y 3 del brief en un solo recorrido y ataca de frente el dolor #1 del contexto (conflictos de horarios). El escenario 2 (solicitud de cambio de turno) se documenta como flujo secundario en diagrama, sin wireframes propios.

Por qué no los otros: "resolución de conflictos" en aislado obliga a explicar cómo se generó el conflicto; "solicitud de cambio" es el más simple y demuestra menos criterio de producto.

### Decisión B — Herramienta de alta fidelidad

**Recomendado: Figma como fuente de verdad + prototipo navegable en Figma.**

Razón: el entregable 6 exige Figma de todos modos. Duplicar el trabajo en Lovable/v0 consume presupuesto de tiempo que la rúbrica premia más en documentación. Si quieres puntos extra en "uso estratégico de IA", la vía barata es usar IA para *generar contenido y validar* (datos realistas, copy, matriz de casos borde, revisión de accesibilidad), no para producir una segunda implementación.

> Alternativa válida si quieres mostrar músculo de prototipado funcional: construir el flujo de asignación en Lovable/v0 y reflejarlo en Figma después. Cuesta ~45–60 min extra. Solo si te sobra tiempo tras cerrar la documentación.

---

## 3. Modelo de dominio (definir antes de wireframear)

Definir esto primero evita rehacer pantallas. 20 minutos aquí ahorran una hora después.

**Entidades:**
- `Actividad / Área` — nombre, responsable, ubicación, descripción, requisitos (habilidades, edad mínima, etc.)
- `Turno` — actividad, fecha, hora inicio/fin, cupo requerido, cupo asignado, estado
- `Voluntario` — datos, disponibilidad declarada, habilidades, historial, límite de horas
- `Asignación` — turno + voluntario + estado (propuesta / confirmada / rechazada / ausente)
- `Solicitud de cambio` — asignación origen, motivo, tipo (cancelar / intercambiar), estado, resolutor

**Estados de turno (semáforo de cobertura):** `Sin cubrir` → `Parcialmente cubierto` → `Cubierto` → `Sobreasignado`. Este es el lenguaje visual central del dashboard y del calendario.

**Estados de asignación:** `Pendiente de confirmación` → `Confirmada` / `Rechazada` → `En conflicto` / `Ausente`.

**Reglas de negocio a declarar** (alimentan directo el entregable 7):
- Un voluntario no puede tener dos turnos solapados → conflicto duro, bloquea.
- Debe respetarse un descanso mínimo entre turnos (ej. 30 min) → conflicto blando, advierte pero permite override con justificación.
- Máximo de horas por voluntario por día / por evento → advertencia al superar.
- Asignación fuera de la disponibilidad declarada → advertencia, no bloqueo.
- Un turno no puede quedar sin responsable a menos de X horas del inicio → alerta escalada.

---

## 4. Plan paso a paso

### Fase 0 — Setup (15 min)
- Crear estructura de carpetas del entregable (ver §5).
- Crear archivo Figma con páginas: `00 Cover` · `01 Research & Definición` · `02 Arquitectura y Flujos` · `03 Wireframes` · `04 UI Alta Fidelidad` · `05 Estados y Casos Borde` · `06 Componentes`.
- **Abrir el log de IA** (`08-decisiones/log-ia.md`) y dejarlo abierto toda la sesión. Cada vez que uses IA: qué pediste, qué devolvió, qué corregiste o descartaste y por qué. Esto es entregable evaluado, no un extra.

### Fase 1 — Definición del problema (40 min) → *Entregable 1*

> **Calibración de método:** no hay acceso a usuarios reales en una prueba técnica. Por tanto no se simulan hallazgos de investigación — eso sería inventar datos. Se trabaja con **proto-personas declaradas como tales**, *jobs to be done* derivados del contexto del brief, y supuestos numerados y explícitos. Se cierra declarando **qué método de investigación usaría para validar cada supuesto** si el proyecto fuera real (ej. entrevistas con 5–8 coordinadores para validar S1; card sorting con 15–30 voluntarios para validar la arquitectura). Eso demuestra criterio de investigación sin fabricar evidencia — y es exactamente lo que el brief pide en "preguntas abiertas".

- **Usuarios:** Administrador/Coordinador general, Coordinador de área, Voluntario. Para cada uno: objetivo, contexto de uso, dispositivo, frustración actual con la hoja de cálculo.
- **Objetivos de producto:** 3–5, medibles. Ej: reducir turnos sin cobertura a <5% al inicio del evento; eliminar solapamientos de horario; que un voluntario sepa su próximo turno en <10 s.
- **Supuestos:** los de §1 más los que surjan. Numerarlos (`S1`, `S2`…) para poder referenciarlos en los otros documentos.
- **Riesgos:** adopción por parte de voluntarios no tecnológicos; conectividad en el recinto; carga de datos inicial de 300 personas; cambios de último minuto sin canal claro.
- **Preguntas abiertas:** 5–7 preguntas reales que le harías al cliente. Esta sección se subestima y es de las que más señal da al evaluador.

**Entregable:** `01-definicion-problema.md` (1–2 páginas).

### Fase 2 — Arquitectura y flujo (40 min) → *Entregable 2*
- **Sitemap** por rol (admin y voluntario en árboles separados, no mezclados). Máximo 3 niveles de profundidad.
- **Flujo principal:** diagrama del recorrido de asignación con el punto de detección de conflicto explícito, incluyendo las tres ramas: sin conflicto / conflicto blando (override con justificación) / conflicto duro (bloqueo + sugerencia de alternativas).
- **Flujo secundario en diagrama simple:** solicitud de cambio de turno del voluntario y su aprobación.
- Marcar en el diagrama qué pantallas de la Fase 3 corresponden a cada paso — conecta los entregables y facilita la lectura del evaluador.

**Entregable:** diagramas en Figma/FigJam + export a `02-arquitectura/`.

### Fase 3 — Wireframes (60 min) → *Entregable 3*
Las cuatro vistas exigidas, en baja/media fidelidad, grises:
1. **Dashboard principal (admin):** cobertura global por día, turnos críticos sin cubrir, conflictos abiertos, solicitudes de cambio pendientes. Prioridad: qué necesita atención *ahora*.
2. **Calendario / gestión de turnos:** vista temporal con las 3 jornadas. Recomendado: timeline por actividad (filas = actividades, columnas = horas), que es la que hace visible la cobertura y los huecos. Filtros por día, actividad, estado de cobertura.
3. **Detalle de actividad / turno:** requisitos, cupo, lista de asignados con su estado, historial de cambios.
4. **Asignación / modificación de turno:** el panel donde se elige voluntario. Debe mostrar disponibilidad, conflictos y carga horaria *antes* de confirmar — no después.

Anotar cada wireframe con notas numeradas explicando decisiones. Los wireframes anotados valen el doble que los mudos.

### Fase 4 — Interfaz de alta fidelidad (60 min) → *Entregable 4*
- **Alcance recomendado:** calendario/gestión de turnos + panel de asignación con detección de conflicto. Es el corazón del producto y donde se demuestran los cinco criterios que evalúan (claridad, jerarquía, consistencia, accesibilidad, escalabilidad).
- Definir primero tokens: paleta (con semántica de cobertura y estados), escala tipográfica, escala de espaciado, radios.
- **Accesibilidad, explícito:** contraste AA verificado, estados de foco visibles, y — crítico aquí — **el estado de cobertura y el conflicto no pueden comunicarse solo por color**; añadir ícono y texto. Documentarlo como decisión.
- **Escalabilidad, explícito:** cómo se comporta la vista con 300 voluntarios y 3 días (densidad, virtualización, filtros, búsqueda, paginación).
- Construir con componentes reales de Figma (variantes + properties), no con capas sueltas. Esto es criterio de evaluación directo.

### Fase 5 — Estados y casos borde (30 min) → *Entregable 5*
Los cuatro estados obligatorios, aplicados a pantallas concretas (no genéricos):
- **Vacío:** evento sin turnos creados aún → con acción primaria clara.
- **Carga:** skeleton del calendario.
- **Éxito:** confirmación de asignación, con deshacer.
- **Error:** fallo al guardar por edición concurrente.

**Casos borde a documentar** (mínimo 8; es una sección donde destacar barato):
- Voluntario asignado a dos turnos solapados.
- Turno sin cobertura a pocas horas de iniciar.
- Voluntario que no confirma disponibilidad y el turno se acerca.
- Cancelación de un voluntario ya confirmado a última hora.
- Dos coordinadores editando el mismo turno a la vez.
- Turno que cruza medianoche entre dos jornadas.
- Sobreasignación (más voluntarios que cupo).
- Voluntario que no cumple los requisitos de la actividad.
- Actividad eliminada con voluntarios ya asignados.
- Cambio de horario de un turno que ya tenía gente confirmada.

### Fase 6 — Documentación para desarrollo (30 min) → *Entregable 7*
- Funcionalidades principales, en formato de historias o de lista funcional numerada.
- Reglas de negocio (las de §3, ya definidas — aquí solo se formalizan con su comportamiento esperado).
- Estados de cada entidad y transiciones permitidas.
- Consideraciones técnicas: manejo de zonas horarias, concurrencia/bloqueo optimista, notificaciones a voluntarios, permisos por rol, volumen de datos.
- Tabla de casos borde con el comportamiento esperado de cada uno.

### Fase 7 — Decisiones de diseño y empaquetado (25 min) → *Entregables 6 y 8*
- Redactar `08-decisiones-de-diseno.md` (1–2 páginas): cómo abordaste el problema, supuestos, qué priorizaste **y qué dejaste fuera a propósito**, decisiones más importantes con su justificación, y el uso de IA con las validaciones aplicadas (destilado del log de la Fase 0).
- Prototipo navegable en Figma conectando el flujo principal.
- Limpiar el archivo Figma: nombres de capas, portada, orden de páginas, leyenda de navegación.
- Verificar permisos de compartir del link.
- Checklist final contra los 8 entregables (§6).

---

## 4b. Rol y skills por fase

Trabajo esta prueba en dos sombreros: **UX/UI Designer** (fases 1–3, 5) y **Design Engineer** (fases 4, 6). Cada fase se ejecuta cargando la skill que la gobierna, no de memoria:

| Fase | Sombrero | Skill que la dirige |
|---|---|---|
| 1 · Definición del problema | UX Designer | `design:user-research` |
| 2 · Arquitectura y flujos | UX Designer | `better-layout` (jerarquía y orden de lectura de la IA) |
| 3 · Wireframes | UX Designer | `better-layout` + `design:design-critique` (autocrítica antes de cerrar) |
| 4 · UI alta fidelidad | Design Engineer | `anthropic-skills:frontend-design` + `better-colors` + `better-typography` + `better-ui` |
| 4 · Verificación de accesibilidad | Design Engineer | `design:accessibility-review` + `better-accessibility` |
| 4/5 · Componentes y tokens en Figma | Design Engineer | `design:design-system` |
| 5 · Estados y copy de esos estados | UX Designer | `design:ux-copy` + `better-writing` |
| 6 · Documentación para desarrollo | Design Engineer | `design:design-handoff` |
| 7 · Revisión final del conjunto | Ambos | `better-interface` (pasada holística) |

Las skills se cargan al entrar en cada fase, no todas de golpe: cargarlas por adelantado diluye la instrucción que importa en el momento de ejecutar.

---

## 5. Estructura de entrega propuesta

```
entrega/
├── README.md                      ← índice con links a todo + link a Figma
├── 01-definicion-problema.md
├── 02-arquitectura/
│   ├── sitemap.png
│   ├── flujo-asignacion.png
│   └── flujo-cambio-turno.png
├── 03-wireframes/                 ← exports anotados
├── 04-ui-alta-fidelidad/
├── 05-estados-y-casos-borde.md
├── 07-documentacion-desarrollo.md
└── 08-decisiones-de-diseno.md
```

El `README.md` con el índice es barato y mueve directo el criterio *"organización y claridad de la documentación"*. El evaluador debe poder navegar todo sin preguntarte nada.

---

## 6. Checklist final

- [ ] 1. Definición del problema: usuarios, objetivos, supuestos, riesgos, preguntas abiertas
- [ ] 2. Sitemap + flujo principal (+ flujo secundario)
- [ ] 3. Wireframes: dashboard, calendario, detalle, asignación
- [ ] 4. Una parte del flujo en alta fidelidad
- [ ] 5. Estados vacío / carga / éxito / error + casos borde
- [ ] 6. Link a Figma con estructura, flujos, componentes y organización
- [ ] 7. Documentación funcional para desarrollo
- [ ] 8. Decisiones de diseño **incluyendo uso de IA y sus validaciones**

---

## 7. Presupuesto de tiempo

| Fase | Tiempo | Acumulado |
|---|---|---|
| 0 · Setup | 15 min | 0:15 |
| 1 · Definición del problema | 40 min | 0:55 |
| 2 · Arquitectura y flujos | 40 min | 1:35 |
| 3 · Wireframes | 60 min | 2:35 |
| 4 · UI alta fidelidad | 60 min | 3:35 |
| 5 · Estados y casos borde | 30 min | 4:05 |
| 6 · Documentación de desarrollo | 30 min | 4:35 |
| 7 · Decisiones y empaquetado | 25 min | 5:00 |

**Si el tiempo se acorta**, recortar en este orden: (1) el flujo secundario de cambio de turno queda solo en diagrama, (2) la alta fidelidad se limita al panel de asignación sin el calendario completo, (3) los wireframes de detalle de actividad se dejan más esquemáticos. **Nunca recortar** las fases 1, 6 y 7 — son las que concentran la rúbrica.
