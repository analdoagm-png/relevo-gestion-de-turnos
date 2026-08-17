# Relevo

**Gestión de turnos para voluntariado en un festival cultural de tres días.**

Prueba técnica de diseño UX/UI. Una organización sin ánimo de lucro coordina más de 300 voluntarios usando hojas de cálculo y mensajería; esta propuesta cubre el proceso completo de diseño, desde la definición del problema hasta el handoff a desarrollo.

| | |
|---|---|
| **Índice navegable** | https://claude.ai/code/artifact/76e9c53f-b56b-4dd2-a8bf-3df7ab47b779 |
| **Archivo de diseño** | https://www.figma.com/design/1aZh7f3yjYkD9YV5ju3zRx |
| **Documentación** | [`entrega/`](entrega/) |

---

## La idea

Los cuatro síntomas del encargo —conflictos de horarios, dificultad para asignar, cambios de último momento, falta de visibilidad— tienen una causa común: **las hojas de cálculo y la mensajería no conocen el concepto de tiempo ni el de persona.** No pueden detectar que alguien está en dos sitios a la vez, ni decir qué turno quedó descubierto cuando alguien canceló.

El resultado no es ineficiencia administrativa: es que **el fallo se descubre cuando ya ocurrió**, con el puesto vacío y el festival abierto.

> Relevo no es un calendario. Es un sistema que hace visible el riesgo de descobertura antes de que se materialice.

---

## Cómo revisar esto

Elige el recorrido según el tiempo que tengas.

### 5 minutos · la propuesta

1. Abre el **[archivo de Figma](https://www.figma.com/design/1aZh7f3yjYkD9YV5ju3zRx)** en la página `00 · Portada`. Explica la tesis y cómo se lee la codificación de cobertura que usa todo el archivo.
2. Ve a la página `04 · UI Alta Fidelidad` y pulsa **▶ Presentar**, eligiendo el flujo **«Flujo principal · coordinación»**.
3. Recorre: *Panel → turno crítico → Detalle → Asignar voluntario → panel de asignación → elige un candidato disponible → Éxito*.

Fíjate en el panel de asignación: los candidatos llegan clasificados en **disponibles, con advertencia y bloqueados** porque el sistema los evaluó antes de mostrarlos. Solo los disponibles completan la asignación — igual que en el producto real.

### 20 minutos · el razonamiento

1. [`entrega/01-definicion-problema.md`](entrega/01-definicion-problema.md) — el planteamiento, los supuestos numerados y el plan de validación.
2. [`entrega/08-decisiones-de-diseno.md`](entrega/08-decisiones-de-diseno.md) — el cierre: las cinco decisiones que sostienen la propuesta y el uso de IA con sus validaciones.
3. Página `03 · Wireframes` en Figma — cada vista lleva su panel de notas numeradas explicando el porqué.

### Una hora · la entrega completa

Los ocho entregables en orden, desde [`entrega/README.md`](entrega/README.md). El archivo de Figma tiene siete páginas numeradas por fase del proceso, así que se puede recorrer en el mismo orden y ver cómo evolucionó la solución.

---

## Cómo está organizado

```
.
├── README.md                       este archivo
├── PLAN.md                         plan de ejecución inicial, con presupuesto de tiempo
└── entrega/
    ├── README.md                   índice de los ocho entregables
    ├── 01-definicion-problema.md
    ├── 02-arquitectura/            memoria + diagramas exportados
    ├── 03-wireframes/              guía de lectura + 4 vistas + 4 paneles de notas
    ├── 04-ui-alta-fidelidad/       memoria + 5 pantallas
    ├── 05-estados/                 4 estados exportados
    ├── 05-estados-y-casos-borde.md
    ├── 06-prototipo.md
    ├── 07-documentacion-desarrollo.md
    ├── 08-decisiones-de-diseno.md
    └── log-ia.md                   bitácora de uso de IA, escrita durante el trabajo
```

### El archivo de Figma

```
00 · Portada                 tesis, leyenda de cobertura e índice
01 · Definición              proto-personas, objetivos, supuestos críticos, riesgo estructural
02 · Arquitectura y Flujos   sitemap por rol · asignación · cambio de turno
03 · Wireframes              4 vistas · 23 notas de diseño
04 · UI Alta Fidelidad       6 pantallas · prototipo de 23 conexiones · 2 flujos
05 · Estados y Casos Borde   vacío, carga, éxito y error · cada uno como flujo propio
06 · Componentes             Librería Relevo · 21 componentes · 39 variantes
```

**Los formatos no son intercambiables.** La documentación vive en Markdown porque se lee, se versiona y se cita. El diseño vive en Figma porque se recorre. Cada artefacto está donde mejor se revisa, y esta página los enlaza.

---

## Sistema de diseño

67 variables en cuatro colecciones, con modo claro y oscuro. Los primitivos tienen alcance vacío: **no aparecen en ningún selector**, de modo que solo la capa semántica es vinculable. Las 67 llevan su nombre de variable CSS grabado, así que Dev Mode devuelve `var(--color-cobertura-parcial)` y no un hexadecimal suelto.

**Todo el sistema pasa WCAG AA en ambos modos**, verificado calculando los ratios de cada par en uso. Dos pares fallaban y se corrigieron; el detalle está en [`entrega/04-ui-alta-fidelidad/README.md`](entrega/04-ui-alta-fidelidad/README.md).

El estado de cobertura **nunca se comunica solo con color**: combina forma del indicador, color y texto. Esa codificación se diseñó primero, y por eso los wireframes funcionan en escala de grises.

---

## Sobre el uso de IA

Este trabajo se hizo con asistencia intensiva de IA, y la bitácora está en [`entrega/log-ia.md`](entrega/log-ia.md), escrita durante el proceso y no reconstruida al final.

El patrón que revelaron las correcciones: **la IA optimiza por parecer completa.** Sus fallos no fueron errores obvios sino salidas plausibles —personas con frustraciones redactadas como hechos observados, valores de token recordados en lugar de leídos, un prototipo con todos los caminos conectados por igual—. Todo se veía terminado.

Las validaciones que cambiaron el resultado están documentadas con sus hallazgos concretos, incluidos dos fallos de contraste WCAG invisibles a simple vista y dos contradicciones lógicas que aparecieron por usar datos coherentes en lugar de contenido de relleno.

---

## Aviso

Propuesta de diseño, no producto en funcionamiento. Los datos de personas, turnos y actividades son ficticios y coherentes entre sí a propósito: esa coherencia se usó como herramienta de detección de errores.
