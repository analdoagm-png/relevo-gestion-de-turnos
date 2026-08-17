# 07 · Documentación para desarrollo

Entregable 7 de 8. Handoff funcional de **Relevo**.

> Diseño: `Relevo · Gestión de Turnos` → https://www.figma.com/design/1aZh7f3yjYkD9YV5ju3zRx
>
> Los valores de este documento están **extraídos del archivo**, no transcritos de memoria. Si el archivo cambia, este documento queda desactualizado: la fuente de verdad de los tokens son las variables de Figma.

**Stack no especificado por el cliente.** La documentación es agnóstica; los nombres de variable CSS son los que ya están grabados en cada token de Figma, así que Dev Mode devuelve directamente el nombre y no un hexadecimal suelto.

---

## 1. Funcionalidades principales

Numeradas para poder referenciarlas desde tickets. El orden refleja dependencia, no prioridad de negocio.

### Base

| | Funcionalidad | Notas |
|---|---|---|
| **F1** | Gestión del evento y sus jornadas | Fechas fijas, un solo huso horario (S9). El selector de jornada es global y persiste entre secciones |
| **F2** | Alta y edición de actividades | Nombre, zona, responsable, descripción y **requisitos** (F3) |
| **F3** | Requisitos por actividad | Condicionan quién puede ser asignado. Ver RN5 |
| **F4** | Alta y edición de turnos | Actividad, fecha, hora inicio y fin, cupo requerido |
| **F5** | Importación de voluntarios desde hoja de cálculo | No es función avanzada: es requisito de arranque (R5). Sin ella no hay producto usable |
| **F6** | Disponibilidad declarada por voluntario | Franjas por jornada. Insumo de F8 |

### Asignación

| | Funcionalidad | Notas |
|---|---|---|
| **F7** | Asignación de voluntarios a turnos | Admite asignación múltiple en una sola operación |
| **F8** | **Evaluación previa de candidatos** | El núcleo del producto. Ver §4 |
| **F9** | Detección de conflictos | Duros y blandos. Ver RN1–RN6 |
| **F10** | Asignación con excepción justificada | Solo sobre conflicto blando. El motivo se persiste en el historial |
| **F11** | Retirada de una asignación | Libera la plaza y dispara recálculo (F13) |

### Seguimiento

| | Funcionalidad | Notas |
|---|---|---|
| **F12** | Confirmación de asistencia por el voluntario | Asignada ≠ confirmada. Ver §3 |
| **F13** | Cálculo de cobertura por turno, actividad y jornada | Recalcula ante cualquier cambio de asignación, cupo u horario |
| **F14** | Panel de turnos críticos | Ordenado por urgencia: tiempo restante × hueco. Ver RN9 |
| **F15** | Solicitud de cambio de turno | Cancelar o intercambiar. Motivo obligatorio (RN10) |
| **F16** | Resolución de solicitudes | Con auto-aprobación por impacto. Ver RN11 |
| **F17** | Historial y auditoría por turno | Registro append-only |
| **F18** | Notificaciones al voluntariado | Se apoya en el canal existente de la organización (P7 abierta) |

### Vista de voluntariado

| | Funcionalidad | Notas |
|---|---|---|
| **F19** | Próximo turno | Pantalla de inicio. Debe resolverse en una sola petición |
| **F20** | Listado de asignaciones propias | |
| **F21** | Declaración de disponibilidad | Alimenta F6 |

---

## 2. Modelo de datos

```
Evento 1───n Jornada
Evento 1───n Actividad
Actividad 1───n Turno
Actividad 1───n Requisito
Turno 1───n Asignación
Voluntario 1───n Asignación
Voluntario 1───n FranjaDisponibilidad
Voluntario 1───n Habilidad
Asignación 1───n SolicitudCambio
Turno 1───n EntradaHistorial
```

### Campos que condicionan la lógica

| Entidad | Campo | Nota |
|---|---|---|
| `Turno` | `cupoRequerido` | Entero ≥ 1. Reducirlo por debajo de las asignaciones activas dispara CB12 |
| `Turno` | `inicio`, `fin` | `fin` puede caer en el día siguiente (CB6) |
| `Turno` | `version` | Entero. Base del bloqueo optimista (§8) |
| `Asignación` | `estado` | Ver §3 |
| `Asignación` | `esExcepcion`, `motivoExcepcion` | Se rellenan solo vía F10 |
| `SolicitudCambio` | `tipo` | `cancelar` \| `intercambiar` |
| `SolicitudCambio` | `motivo` | **Obligatorio** (RN10) |
| `SolicitudCambio` | `impactoCobertura` | Calculado al crear, no al resolver. Decide el enrutado (RN11) |

**La cobertura no se persiste, se deriva** de las asignaciones activas del turno. Persistirla introduce una segunda fuente de verdad que se desincroniza — y la desincronización de la cobertura es literalmente el problema que este producto existe para eliminar.

---

## 3. Estados

### Asignación

```
                  ┌──────────────► rechazada
                  │
    pendiente ────┼──────────────► confirmada ────► ausente
        │         │                     │
        │         └──► retirada ◄───────┘
        │                                │
        └────────────────────────────────┘
                  (cambio de horario · RN12)
```

| Estado | Significado | Transiciones válidas |
|---|---|---|
| `pendiente` | Asignada, sin respuesta del voluntario | → `confirmada`, `rechazada`, `retirada` |
| `confirmada` | El voluntario aceptó | → `retirada`, `ausente`, `pendiente` (solo por RN12) |
| `rechazada` | El voluntario declinó | → `retirada` |
| `retirada` | Ya no cuenta para la cobertura | terminal |
| `ausente` | Confirmó y no se presentó | terminal |

**Solo `pendiente` y `confirmada` computan cobertura.** La distinción importa: un turno con el cupo lleno de asignaciones `pendiente` está en riesgo real (CB3), y la interfaz debe poder mostrarlo.

### Cobertura de turno · derivada

| Estado | Condición |
|---|---|
| `sin-cubrir` | 0 asignaciones activas |
| `parcial` | 0 < activas < cupoRequerido |
| `cubierto` | activas = cupoRequerido |
| `sobreasignado` | activas > cupoRequerido (CB7) |

### Solicitud de cambio

`pendiente` → `aprobada` \| `rechazada` \| `auto-aprobada` \| `caducada`

`caducada` cubre CB11: el turno empezó antes de resolverse. No se resuelve sola — pasa a incidencia en vivo.

---

## 4. La regla central · evaluación previa

**F8 es lo que diferencia este producto de una hoja de cálculo.** Al abrir el panel de asignación de un turno, el sistema evalúa a *cada* candidato antes de renderizar y devuelve la lista ya clasificada.

```
evaluarCandidato(voluntario, turno) → {
  grupo: 'disponible' | 'advertencia' | 'bloqueado',
  motivos: [{ regla, severidad, referencia }]
}
```

- `bloqueado` si dispara **al menos una** regla dura
- `advertencia` si dispara al menos una blanda y ninguna dura
- `disponible` si no dispara ninguna

**Los tres grupos se devuelven siempre, incluidos los bloqueados con su motivo.** No filtrar los bloqueados es una decisión de producto: que una persona desaparezca de la lista genera la pregunta «¿y esa persona por qué no está?» y empuja al usuario fuera del sistema.

**Rendimiento:** con 300 voluntarios, la evaluación se hace en servidor y se pagina; no se envían 300 registros al cliente para filtrarlos allí. Objetivo: respuesta por debajo de 300 ms para el conjunto ordenado.

---

## 5. Reglas de negocio

| | Regla | Severidad | Origen |
|---|---|---|---|
| **RN1** | Un voluntario no puede tener dos turnos con horarios solapados | **Dura** · bloquea | O2 |
| **RN2** | Debe respetarse un descanso mínimo entre turnos del mismo voluntario | Blanda · advierte | P3 abierta |
| **RN3** | Existe un máximo de horas por voluntario y jornada | Blanda · advierte | P3 abierta |
| **RN4** | Asignar fuera de la disponibilidad declarada | Blanda · advierte | S5 |
| **RN5** | El voluntario debe cumplir los requisitos de la actividad | **Dura** · bloquea | S6 · sujeta a P6 |
| **RN6** | Superar el cupo requerido | Blanda · advierte | CB7 |
| **RN7** | Una asignación nace en estado `pendiente` | — | S4 |
| **RN8** | Solo `pendiente` y `confirmada` computan cobertura | — | §3 |
| **RN9** | Urgencia = f(tiempo restante hasta inicio, hueco de cobertura) | — | Panel |
| **RN10** | El motivo es obligatorio en toda solicitud de cambio | **Dura** · bloquea envío | R1 |
| **RN11** | Una solicitud que no deja el turno por debajo del cupo puede auto-aprobarse | — | R4 · sujeta a P1 |
| **RN12** | Cambiar el horario de un turno **invalida las confirmaciones** | — | CB10 |
| **RN13** | Eliminar una actividad archiva sus asignaciones, no las borra | — | CB9 |

### Duro frente a blando

**No es una distinción técnica, es de producto.**

- **Duro**: continuar produce un imposible físico (RN1) o incumple una regla de la organización (RN5).
- **Blando**: continuar es una decisión legítima que alguien debe poder tomar **y justificar**.

Convertir una blanda en dura parece más seguro y en realidad empuja a la coordinación a resolver por WhatsApp, que es el riesgo estructural del proyecto (R1). **Ante la duda, blanda.**

Los umbrales de RN2 y RN3 son **configurables por evento**, no constantes en código.

---

## 6. Sistema de diseño

### Color · tokens semánticos

Los primitivos (`neutral/*`, `azul/*`, `rojo/*`, `ambar/*`, `verde/*`) tienen alcance vacío en Figma y **no deben usarse directamente en la implementación**. Solo la capa semántica.

| Token CSS | Claro | Oscuro | Uso |
|---|---|---|---|
| `--color-fondo-base` | `#F7F7F8` | `#08090B` | Lienzo de la aplicación |
| `--color-fondo-superficie` | `#FFFFFF` | `#17181C` | Tarjetas, barras, paneles |
| `--color-fondo-elevado` | `#EFEFF2` | `#24252C` | Cabeceras de tabla, esqueletos |
| `--color-fondo-acento-suave` | `#EFF2FE` | `#101C4A` | Fondo del elemento activo |
| `--color-fondo-inverso` | `#0E0E10` | `#F7F7F8` | Fondos de alto contraste |
| `--color-borde-sutil` | `#E6E6E9` | `#24252C` | Divisores, bordes de tarjeta |
| `--color-borde-fuerte` | `#D2D3D8` | `#3A3B44` | Bordes de control |
| `--color-borde-foco` | `#1B4DE4` | `#6E93FF` | Anillo de foco |
| `--color-texto-principal` | `#0E0E10` | `#F7F7F8` | Texto principal |
| `--color-texto-secundario` | `#5F6068` | `#94959D` | Texto de apoyo |
| `--color-texto-terciario` | `#6E6F79` | `#94959D` | Metadatos, marcas de tiempo |
| `--color-texto-inverso` | `#FFFFFF` | `#0E0E10` | Texto sobre fondo inverso |
| `--color-texto-acento` | `#1B4DE4` | `#6E93FF` | Enlaces |
| `--color-accion-primaria` | `#1B4DE4` | `#6E93FF` | Botón primario |
| `--color-accion-primaria-texto` | `#FFFFFF` | `#0E0E10` | Texto del botón primario |
| `--color-cobertura-sin-cubrir` | `#C02A25` | `#F0736C` | Estado sin cubrir |
| `--color-cobertura-sin-cubrir-suave` | `#FBEDEC` | `#3B1210` | Fondo del estado |
| `--color-cobertura-parcial` | `#8A5A00` | `#E0A33C` | Estado parcial |
| `--color-cobertura-parcial-suave` | `#FBF3E4` | `#33260F` | Fondo del estado |
| `--color-cobertura-cubierto` | `#17724A` | `#4FBF8B` | Estado cubierto |
| `--color-cobertura-cubierto-suave` | `#EAF4EF` | `#10301F` | Fondo del estado |

**El acento nunca significa estado, y el estado nunca hace de acento.** Son familias separadas a propósito.

### Espaciado y radio

| Token | px | | Token | px |
|---|---|---|---|---|
| `--espaciado-2xs` | 2 | | `--radio-none` | 0 |
| `--espaciado-xs` | 4 | | `--radio-sm` | 3 |
| `--espaciado-sm` | 8 | | `--radio-md` | 4 |
| `--espaciado-md` | 12 | | `--radio-lg` | 8 |
| `--espaciado-lg` | 16 | | `--radio-full` | 999 |
| `--espaciado-xl` | 24 | | | |
| `--espaciado-2xl` | 32 | | | |
| `--espaciado-3xl` | 48 | | | |
| `--espaciado-4xl` | 64 | | | |

**Ningún valor de espaciado fuera de esta escala.** El archivo de Figma está auditado a cero excepciones.

### Tipografía

| Estilo | Familia | Peso | Tamaño | Interlineado | Tracking |
|---|---|---|---|---|---|
| Display/L | Archivo | SemiBold | 34 | 112 % | −3 % |
| Display/M | Archivo | SemiBold | 26 | 115 % | −2.5 % |
| Título/L | Archivo | SemiBold | 20 | 120 % | −2 % |
| Título/M | Archivo | SemiBold | 16 | 125 % | −2 % |
| Título/S | Archivo | SemiBold | 14 | 130 % | −1.5 % |
| Cuerpo/L | Public Sans | Regular | 15 | 155 % | 0 |
| Cuerpo/M | Public Sans | Regular | 13.5 | 155 % | 0 |
| Cuerpo/S | Public Sans | Regular | 12 | 150 % | 0 |
| Etiqueta/M | Public Sans | Medium | 13 | 140 % | 0 |
| Etiqueta/S | Public Sans | Medium | 11.5 | 140 % | 0 |
| Énfasis/M | Public Sans | SemiBold | 13 | 140 % | 0 |
| Dato/M | IBM Plex Mono | Regular | 12 | 140 % | 2 % |
| Dato/S | IBM Plex Mono | Regular | 10.5 | 140 % | 6 % |
| Overline | IBM Plex Mono | Regular | 9.5 | 140 % | 12 % |

**Todo dato numérico usa `Dato/*` en monoespaciada.** Horas y recuentos se alinean en columna; con ancho proporcional se leen mal en tabla. Donde no se pueda usar mono, aplicar `font-variant-numeric: tabular-nums`.

### Componentes · 21 componentes, 39 variantes

Los que tienen variantes o propiedades relevantes para implementación:

| Componente | Variantes | Propiedades |
|---|---|---|
| `Botón` | Jerarquía: Primario \| Secundario \| Fantasma · Estado: Normal \| Deshabilitado | Etiqueta |
| `Chip de cobertura` | Estado: Sin cubrir \| Parcial \| Cubierto | Etiqueta |
| `Bloque de turno` | Cobertura: Sin cubrir \| Parcial \| Cubierto | Actividad, Horario |
| `Fila de candidato` | Estado: Disponible \| Advertencia \| Bloqueado | Nombre, Detalle |
| `Requisito` | Estado: Cumplido \| Pendiente \| Faltante | Texto |
| `Banner` | Estado: Éxito \| Error | Titular, Cuerpo, Acción |
| `Elemento de navegación` | Estado: Activo \| Predeterminado | Etiqueta |
| `Pestaña` | Estado: Activo \| Predeterminado | Etiqueta |
| `Toggle` | Estado: Encendido \| Apagado | — |
| `Topbar` | Variante: Completo \| Compacto | — |

Sin variantes: `Chip de evento`, `Grupo de filtro`, `Tarjeta de estadística`, `Fila de turno crítico`, `Fila de conflicto`, `Fila de solicitud`, `Fila de persona`, `Entrada de historial`, `Fila de turno`, `Plaza libre`, `Sidebar`.

**Cada componente lleva su regla de uso en la descripción de Figma.** Dev Mode las expone; conviene leerlas antes de implementar, porque varias recogen decisiones de producto que no se deducen de la forma.

**Regla de implementación del estado de cobertura.** Cada estado combina **tres señales simultáneas**:

| Estado | Forma del indicador | Color | Texto |
|---|---|---|---|
| Cubierto | Disco lleno | verde | «Cubierto» |
| Parcial | Medio disco | ámbar | «Parcial · X de Y» |
| Sin cubrir | Anillo vacío | rojo | «Sin cubrir» |

En el calendario se añade **contorno discontinuo** para sin cubrir. Nunca implementar el estado solo con color: falla en daltonismo, escala de grises e impresión.

El recuento va siempre en formato **«X de Y»**, nunca en porcentaje. Con cupos de 3 a 8 personas, un 67 % puede ser «falta una» o «faltan tres», y no se gestionan igual.

---

## 7. Comportamiento responsive

Diseñado a 1440 px (coordinación) y 390 px (voluntariado). Lo intermedio se especifica aquí.

| Rango | Comportamiento |
|---|---|
| ≥ 1280 px | Disposición completa: barra lateral fija de 208 px, contenido en dos columnas |
| 1024–1279 px | La segunda columna del panel y del detalle pasa **debajo** de la principal |
| 768–1023 px | Barra lateral colapsa a iconos. El calendario mantiene el timeline con scroll horizontal, **nunca se convierte en lista**: perder el eje temporal elimina la función de la vista |
| < 768 px | Vista de coordinación en modo consulta. La asignación masiva no se ofrece en móvil: es trabajo de precisión sobre alta densidad |
| Voluntariado | Mobile-first siempre, sin variante de escritorio propia |

**El calendario no degrada a lista.** Es la única regla responsive innegociable: el hueco de cobertura se detecta porque el eje horizontal es tiempo.

---

## 8. Consideraciones técnicas

### Concurrencia · bloqueo optimista

Dos coordinadores editando el mismo turno durante el evento es un caso real, no teórico (CB5).

- Cada `Turno` lleva `version`. Toda mutación la envía.
- Si la versión recibida no coincide, el servidor responde **409** con la versión actual completa.
- El cliente muestra la comparación campo a campo (pantalla E4) y ofrece tres salidas: comparar, aplicar encima, descartar.
- **Nunca sobrescribir en silencio.** Si se aplica encima, la versión desplazada se registra en el historial.

### Tiempo y zonas horarias

- Persistir en UTC, presentar en la zona del evento (S9).
- Un turno pertenece a la jornada en que **empieza**. Si cruza medianoche, se presenta como continuación en la siguiente (CB6, pendiente de P5).
- La comparación de solapamiento (RN1) opera sobre instantes absolutos, nunca sobre hora local en texto.

### Conectividad

El recinto tiene cobertura irregular (R3), y la vista de voluntariado se consulta justo ahí.

- F19 debe resolverse en **una sola petición** y cachearse.
- Ante datos obsoletos, mostrar la antigüedad («actualizado hace 2 min») en lugar de fallar en silencio.
- Las acciones del voluntariado (confirmar, solicitar cambio) se encolan y reintentan.

### Permisos

| Rol | Alcance |
|---|---|
| Coordinación general | Todo el evento |
| Coordinación de área | Solo sus actividades y los voluntarios asignados a ellas |
| Voluntariado | Solo sus propias asignaciones |

**La falta de permiso oculta la acción, no la deshabilita.** Un botón deshabilitado comunica «puedes, pero ahora no»; si nunca vas a poder, no debe estar.

### Datos personales

Contactos, disponibilidad y a veces certificaciones médicas de 300 personas (R6). Visibilidad acotada por rol, y política de retención posterior al evento acordada con la organización.

### Rendimiento

- La evaluación de candidatos (F8) se hace en servidor y se pagina.
- El calendario de una jornada carga los turnos de esa jornada, no del evento entero.
- El recálculo de cobertura (F13) es incremental por turno, no un recorrido completo.

---

## 9. Accesibilidad

Requisitos verificados en diseño y que deben sostenerse en implementación.

- **Contraste**: todo el sistema pasa WCAG AA en ambos modos. Ratios verificados en el entregable 4. Dos pares fallaban y se corrigieron; no revertir `texto/terciario` ni `ambar/500`.
- **Estado nunca solo por color**: ver §6.
- **Foco visible**: token `--color-borde-foco`, anillo de 2 px con 2 px de separación. Los componentes de Figma **no tienen aún variante de foco** — es deuda declarada que debe cubrirse en implementación.
- **Orden de foco** en el panel de asignación: buscador → filtro → grupos en orden → acción de cada fila → pie. Los candidatos bloqueados son alcanzables por teclado y su motivo debe anunciarse; **no usar `aria-hidden` sobre ellos**.
- **Anuncios**: el recálculo de cobertura tras asignar debe emitirse en una región `aria-live="polite"`. Es el cambio de estado que el usuario necesita percibir sin verlo.
- **Objetivo táctil** mínimo 44 × 44 px en la vista de voluntariado.
- **Sin dependencia de hover**: toda información revelada al pasar el cursor debe estar disponible en foco y en táctil.

---

## 10. Casos borde

Los doce casos y su comportamiento esperado están en [05-estados-y-casos-borde.md](05-estados-y-casos-borde.md) §Casos borde. Los tres que más condicionan la implementación:

| | Caso | Implicación técnica |
|---|---|---|
| **CB3** | Cupo lleno pero sin confirmar | La cobertura necesita dos lecturas: asignada y confirmada. Un solo contador no basta |
| **CB5** | Edición concurrente | Bloqueo optimista con 409 y comparación de versiones |
| **CB10** | Cambio de horario con gente confirmada | Invalidación en cascada de confirmaciones + revalidación de RN1–RN4 con el nuevo horario |

---

## 11. Orden de implementación sugerido

Por dependencia, no por valor de negocio.

1. **F1, F2, F4** — evento, actividades, turnos. Sin esto no hay nada que asignar.
2. **F5** — importación de voluntarios. Es la barrera de adopción (R5); dejarla para el final significa no poder probar con datos reales.
3. **F13** — cálculo de cobertura. Lo consumen casi todas las vistas.
4. **F7, F8, F9** — asignación con evaluación previa. **El núcleo.** Si solo se implementa una cosa, es esta.
5. **F12, F19, F20** — vista de voluntariado. Sin ella la cobertura mostrada es ficción, porque nadie confirma.
6. **F14** — panel de turnos críticos.
7. **F15, F16** — solicitudes de cambio.
8. **F17, F18** — historial y notificaciones.

**F8 antes que F14.** Es tentador empezar por el panel porque es la pantalla más vistosa, pero el panel sin evaluación previa muestra problemas que nadie puede resolver todavía.

---

## Preguntas que bloquean implementación

Las abiertas del entregable 1 que un equipo necesita resolver antes de escribir código:

| | Pregunta | Bloquea |
|---|---|---|
| **P1** | ¿Quién puede aprobar un cambio: coordinación general, o también de área? | RN11, permisos |
| **P3** | ¿El descanso mínimo es obligación legal o recomendación interna? | Severidad de RN2 |
| **P5** | ¿Los turnos pueden cruzar la medianoche? | Modelo de datos, CB6 |
| **P6** | ¿Los requisitos son formales o preferencias? | Severidad de RN5 |
| **P7** | ¿Qué canal de aviso usa hoy la organización y se puede integrar? | F18 |
| **P8** | ¿El voluntario puede rechazar una asignación? | Estado inicial de `Asignación`, RN7 |
