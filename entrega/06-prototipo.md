# 06 · Prototipo y entrega en herramienta de diseño

Entregable 6 de 8.

> Archivo: `Relevo · Gestión de Turnos` → https://www.figma.com/design/1aZh7f3yjYkD9YV5ju3zRx

---

## Lo que pide el entregable, punto por punto

| Requisito del brief | Dónde se cubre |
|---|---|
| Estructura de pantallas | Páginas `03`, `04` y `05`, en orden de flujo de izquierda a derecha |
| Flujos de navegación | Prototipo navegable en la página `04` · 23 conexiones · también los diagramas de la página `02` |
| Componentes utilizados | Página `06 · Componentes` · 21 componentes, 39 variantes, todos con descripción |
| Organización general | 7 páginas numeradas, una por fase del proceso |

---

## Organización del archivo

```
00 · Portada                    tesis, leyenda de cobertura e índice del archivo
01 · Definición                 proto-personas, objetivos, supuestos críticos y riesgo estructural
02 · Arquitectura y Flujos      sitemap + flujo principal + flujo de cambio
03 · Wireframes                 4 vistas + 23 notas de diseño
04 · UI Alta Fidelidad          6 pantallas · prototipo navegable
05 · Estados y Casos Borde      4 estados, cada uno como flujo propio
06 · Componentes                Librería Relevo (21 componentes) + documentación
```

Las páginas están numeradas por fase del proceso, no por tipo de artefacto. Quien revise puede recorrerlas en orden y ver cómo evolucionó la solución.

---

## El prototipo

### Dos flujos de entrada

| Flujo | Punto de partida |
|---|---|
| **Flujo principal · coordinación** | `P1 · Panel` |
| **Vista de voluntariado** | `P5 · Mi próximo turno` |

Son flujos separados a propósito: son dos productos con densidades opuestas, tal como se estableció en la arquitectura de información. La vista de voluntariado es de una sola pantalla, así que no tiene navegación interna — es correcto, no un cabo suelto.

### El recorrido que reproduce el flujo principal

```
P1 · Panel
  └─ turno crítico ──────────────► P3 · Detalle de turno
  └─ botón «Asignar» ────────────► P4 · Panel de asignación
  └─ nav «Turnos» ───────────────► P2 · Calendario

P2 · Calendario
  └─ bloque de turno ────────────► P3 · Detalle de turno

P3 · Detalle de turno
  └─ «Asignar voluntario» ───────► P4 · Panel de asignación

P4 · Panel de asignación
  └─ candidato disponible ───────► P6 · Éxito
  └─ «Cancelar» / cerrar ────────► P3 · Detalle de turno

P6 · Éxito
  └─ «Asignar la plaza restante» ► P4 · Panel de asignación
  └─ «Volver al calendario» ─────► P2 · Calendario
```

**23 conexiones, cero destinos rotos, ninguna pantalla sin salida.** Verificado recorriendo el grafo, no haciendo clic a mano.

### Detalles de interacción

- **Solo los candidatos disponibles completan la asignación.** Los que tienen advertencia y los bloqueados no llevan a ninguna parte, y eso es deliberado: en el producto real exigen justificación o directamente no pueden asignarse. Un prototipo donde los tres grupos se comportan igual anularía la propiedad que el diseño intenta demostrar.
- **La fila del turno crítico y su botón llevan a sitios distintos**: la fila abre el detalle, el botón abre directamente la asignación. Es la jerarquía real de la pantalla —consultar frente a actuar— y en el prototipo funciona porque el nodo más interno gana el clic.
- **La apertura del panel de asignación usa `Smart Animate`**; el resto de la navegación usa disolución breve. La diferencia es intencional: un panel lateral se desliza, una pantalla se sustituye.

### Los cuatro estados, cada uno como flujo propio

En la página `05`, cada estado es un punto de partida independiente. Así se pueden presentar uno a uno sin tener que provocarlos desde el flujo, que es como se revisan en una sesión de handoff.

---

## Una limitación de Figma que condicionó el montaje

**Los prototipos de Figma no admiten navegación entre páginas.** Al intentar enlazar el panel de asignación (página 04) con el estado de éxito (página 05), la API lo rechaza:

> `destinations must be a different top-level frame on the same page`

Se comprobó ejecutándolo, no asumiéndolo.

**Consecuencia:** el estado de éxito existe dos veces. En la página `05` como uno de los cuatro estados documentados, y en la `04` como `P6`, para que el flujo de asignación llegue a su conclusión. Son la misma pantalla con dos propósitos: documentar y demostrar.

La alternativa era dejar el botón principal del prototipo sin destino. Un prototipo cuya acción central no hace nada es exactamente lo que un evaluador va a pulsar primero.

---

## Sobre un prototipo en código

Se evaluó complementar la entrega con una implementación real desplegada. **No sustituye nada**: el brief exige la versión en Figma de forma explícita («adicionalmente, entrega una versión en Figma»), así que un desarrollo en código es puramente aditivo.

El argumento a favor es concreto y acotado: hay una propiedad del diseño que una pantalla estática no puede transmitir —**que el sistema evalúa a los candidatos antes de mostrarlos**— y que solo se entiende usándola. En un frame parece una lista con colores; interactuando, se entiende que el conflicto dejó de ser un error y pasó a ser información de decisión.

Si se aborda, el alcance correcto es **una sola pantalla, el panel de asignación**, consumiendo los mismos nombres de variable CSS que ya están grabados en cada token de Figma. Eso convierte el deploy en evidencia de que el sistema funciona, en lugar de una maqueta paralela que puede divergir.

Y debe declararse como prototipo con datos simulados en la propia página: uno que aparenta ser producto genera preguntas equivocadas en la revisión.
