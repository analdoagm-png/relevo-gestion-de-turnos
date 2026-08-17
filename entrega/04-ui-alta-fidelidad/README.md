# 04 · Diseño de interfaz

Entregable 4 de 8, más la parte de componentes del entregable 6.

> Archivo: `Relevo · Gestión de Turnos` → https://www.figma.com/design/1aZh7f3yjYkD9YV5ju3zRx
>
> Pantallas en la página **`04 · UI Alta Fidelidad`** · sistema en **`06 · Componentes`**

---

## Qué se llevó a alta fidelidad

El brief pide **una parte relevante del flujo**. El alcance creció en dos decisiones sucesivas hasta cubrir el recorrido completo:

| | Pantalla | Por qué está |
|---|---|---|
| **P1** | Panel | La puerta de entrada. Donde se detecta qué va a fallar |
| **P2** | Calendario de turnos | Donde se sitúa el hueco. Prueba la escalabilidad: 34 turnos simultáneos |
| **P3** | Detalle de turno | Donde vive el estado por persona y la plaza libre como objeto |
| **P4** | Panel de asignación con detección de conflicto | El corazón de la propuesta. Donde el sistema evalúa antes de mostrar |
| **P5** | Mi próximo turno (móvil) | La otra mitad del producto |

El alcance inicial fueron P2 y P4 — el tramo donde se concentran los cinco criterios de evaluación. Después se añadió P5, porque los entregables 1 y 2 sostienen que el voluntariado tiene la necesidad más distinta de todas y dejar esa decisión sin representación visual la debilitaba. Finalmente se añadieron P1 y P3: el Panel es la puerta de entrada del producto y el Detalle es donde vive la idea de que **el estado es por persona**, y ambas ideas se leen a medias en wireframe.

Las cinco recorren el flujo principal del entregable 2 en orden, dispuestas de izquierda a derecha en el lienzo.

**El contraste de densidad es deliberado.** P2 muestra 34 turnos a la vez; P5 muestra uno. Que ambas salgan del mismo sistema de tokens y componentes es la demostración de escalabilidad — no una afirmación sobre ella.

---

## El sistema

### Tokens · 4 colecciones, 67 variables

| Colección | Variables | Modos | Papel |
|---|---|---|---|
| **Primitivos** | 32 | Valor | Rampas crudas. Alcance vacío: **no aparecen en ningún selector** |
| **Color** | 21 | Claro · Oscuro | Capa semántica. Todo alias a primitivos, nunca valores repetidos |
| **Espaciado** | 9 | Valor | De 2 a 64 px |
| **Radio** | 5 | Valor | De 0 a completo |

Dos decisiones de arquitectura de tokens:

**Los primitivos están ocultos.** Su `scope` es una lista vacía, así que no contaminan los selectores de propiedades. Solo la capa semántica es seleccionable. Es lo que impide que alguien vincule un botón a `azul/500` en lugar de a `accion/primaria`.

**Cada variable lleva su sintaxis de código.** `var(--color-cobertura-sin-cubrir)` y equivalentes. Eso hace que Dev Mode devuelva el nombre de la variable CSS real y no un hex suelto — es parte del handoff, no un adorno.

Los estados de cobertura son tokens semánticos propios (`cobertura/sin-cubrir`, `cobertura/parcial`, `cobertura/cubierto`, más sus variantes suaves), separados del acento. El azul de la marca nunca significa estado, y el estado nunca hace de acento.

### Tipografía · 14 estilos

Escala en cuatro familias de rol: `Display`, `Título`, `Cuerpo`, `Etiqueta`, más `Dato` y `Overline` en monoespaciada.

- **Archivo** para titulares, con tracking negativo. Es una grotesca de proporciones estrechas: rinde en titulares densos sin pedir espacio extra.
- **Public Sans** para cuerpo e interfaz. Neutra y muy legible en tamaños pequeños, que es donde vive casi todo este producto.
- **IBM Plex Mono** para datos: horas, recuentos, etiquetas de sistema. **Las cifras alineadas en columna necesitan ancho fijo** — en una tabla de turnos, horas que no alinean se leen mal.

### Componentes · 21 componentes, 39 variantes

Organizados en tres grupos dentro de la sección **Librería Relevo** de la página `06`.

| Grupo | Componentes |
|---|---|
| **Base** · controles e indicadores | Botón, Chip de cobertura, Elemento de navegación, Chip de evento, Toggle, Pestaña, Grupo de filtro, Requisito |
| **Datos** · piezas que representan turnos y personas | Bloque de turno, Fila de candidato, Tarjeta de estadística, Fila de turno crítico, Fila de conflicto, Fila de solicitud, Fila de persona, Entrada de historial, Fila de turno, Plaza libre |
| **Estructura** · armazón de la aplicación | Banner, Topbar, Sidebar |

Los cuatro con variantes más significativas:

| Componente | Variantes | Propiedades |
|---|---|---|
| **Botón** | Jerarquía (3) × Estado (2) | Etiqueta |
| **Chip de cobertura** | Estado (3) | Etiqueta |
| **Bloque de turno** | Cobertura (3) | Actividad, Horario |
| **Fila de candidato** | Estado (3) | Nombre, Detalle |

Todos con auto-layout, propiedades de texto y **relleno, borde, espaciado y radio vinculados a variables** — sin valores fijos en los componentes.

**Los 21 llevan descripción en Figma con su regla de uso.** No es documentación decorativa: la del `Chip de cobertura` explica por qué el estado combina tres señales, la de `Fila de candidato` por qué los bloqueados se muestran en vez de ocultarse, y la de `Plaza libre` por qué la vacante es un objeto y no la ausencia de una fila. Quien lo use en seis meses necesita el porqué, no el qué.

---

## La decisión que atraviesa todo el sistema

**El estado de cobertura nunca se comunica solo por color.** Cada estado combina tres señales:

| Estado | Forma | Color | Texto |
|---|---|---|---|
| Cubierto | Disco lleno | Verde | «Cubierto» |
| Parcial | Medio disco | Ámbar | «Parcial · 4 de 6» |
| Sin cubrir | Anillo vacío | Rojo | «Sin cubrir» |

El medio disco está hecho con `arcData` sobre una elipse, no con un icono importado: es geometría, así que escala sin perder definición.

En el calendario la codificación se refuerza con **contorno discontinuo** para los turnos sin cubrir. Un turno descubierto se detecta en una impresión en blanco y negro.

Esto es lo que hizo posible que los wireframes de la fase anterior funcionaran en gris. La codificación por forma no se añadió al final para cumplir un criterio: se diseñó primero y el color llegó después, encima.

---

## Auditoría de accesibilidad

Se calcularon los contrastes reales de cada par en uso, en ambos modos. **Dos pares fallaban AA y se corrigieron.**

### Fallo 1 · la rampa de texto terciario estaba invertida entre modos

| | Antes | Después |
|---|---|---|
| Terciario sobre superficie (claro) | **2.98:1** ✗ | 4.98:1 ✓ |
| Terciario sobre base (oscuro) | **4.00:1** ✗ | 6.68:1 ✓ |

El valor claro era demasiado claro sobre blanco y el oscuro demasiado oscuro sobre negro: los dos modos estaban al revés. Afectaba a metadatos, marcas de tiempo y etiquetas de sistema — texto de 10 a 11 px, justo donde el contraste importa más. Se intercambiaron los pasos de la rampa.

### Fallo 2 · el ámbar de cobertura parcial se quedaba a 4.48:1

Dos centésimas por debajo del umbral. Es exactamente el tipo de fallo que sobrevive a una revisión visual: a ojo se ve bien. Se oscureció el primitivo `ambar/500` de `#A66A00` a `#8A5A00`, y pasa a **5.93:1**.

### Resto del sistema, verificado

| Par | Ratio |
|---|---|
| Texto principal sobre superficie | 19.28:1 |
| Texto secundario sobre superficie | 6.25:1 |
| Acción primaria con texto inverso | 6.50:1 |
| Texto de acento sobre acento suave | 5.82:1 |
| Sin cubrir sobre superficie | 5.83:1 |
| Cubierto sobre superficie | 5.93:1 |
| Parcial sobre fondo oscuro | 8.99:1 |

**Todo el sistema pasa WCAG AA en ambos modos.** Y el diseño no depende del color: la codificación por forma cubre daltonismo, escala de grises e impresión, que el contraste por sí solo no resuelve.

---

## Los cinco criterios del brief

**Claridad de uso.** El panel de asignación presenta el conflicto con la regla y el turno concretos escritos en la fila. La acción cambia de nombre según la consecuencia: «Asignar» frente a «Revisar».

**Jerarquía visual.** En P5, una sola tarjeta ocupa un tercio del alto porque responde la única pregunta que trae el voluntariado. En P1, «Turnos críticos» domina sobre los indicadores: las métricas informan, pero no dicen qué hacer ahora. En P2 la jerarquía la lleva la posición temporal y la codificación de cobertura, no el tamaño.

**Consistencia.** El mismo `Chip de cobertura` aparece en las cinco pantallas, y en P3 se reutiliza para el estado **por persona**: disco lleno para confirmada, medio disco para pendiente. El lenguaje de formas es el mismo aunque el sujeto cambie de turno a persona. Los mismos tokens gobiernan una pantalla de 1440 px y una de 390 px.

**Accesibilidad.** Auditada con números, no a ojo. Dos fallos encontrados y corregidos. Estado codificado por forma además de color.

**Escalabilidad.** El calendario sostiene 34 turnos en una jornada sin cambiar de sistema. Los tokens tienen modo oscuro desde el principio, no como añadido. Los primitivos ocultos impiden que el sistema se degrade con vinculaciones directas a valores crudos.

---

## Deuda declarada

Lo que un equipo real abordaría a continuación:

- **Iconografía.** Se identificó Simple Design System como fuente (tiene el set Feather completo y está suscrita al archivo), pero no se importó. Las pantallas usan formas geométricas donde iría un icono.
- **Estados de foco.** Los tokens existen (`borde/foco`), pero los componentes no tienen variante de foco. Es requisito de navegación por teclado.
- **Modo oscuro sin pantalla.** Los tokens tienen los dos modos y están verificados por contraste, pero no hay una pantalla montada en oscuro.
- **Rejilla y puntos de ruptura.** El escritorio está resuelto a 1440 px y el móvil a 390 px. Lo intermedio queda para el entregable 7.
