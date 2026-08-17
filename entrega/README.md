# Relevo · Gestión de turnos para voluntariado

Prueba técnica UX/UI. Plataforma para administrar más de 300 voluntarios en un festival cultural de tres días.

**Índice navegable con estado de cada entregable:** https://claude.ai/code/artifact/76e9c53f-b56b-4dd2-a8bf-3df7ab47b779
**Archivo de diseño:** https://www.figma.com/design/1aZh7f3yjYkD9YV5ju3zRx

---

## Por dónde empezar

Si solo vas a leer dos cosas: [la definición del problema](01-definicion-problema.md) y [las decisiones de diseño](08-decisiones-de-diseno.md). La primera plantea, la segunda cierra.

Si quieres ver el producto: abre el prototipo en Figma, página `04`, flujo **«Flujo principal · coordinación»**.

---

## Los ocho entregables

| | Entregable | Dónde |
|---|---|---|
| 1 | Definición del problema | [`01-definicion-problema.md`](01-definicion-problema.md) |
| 2 | Arquitectura y flujos | [`02-arquitectura/`](02-arquitectura/README.md) · Figma pág. `02` |
| 3 | Wireframes anotados | [`03-wireframes/`](03-wireframes/README.md) · Figma pág. `03` |
| 4 | Diseño de interfaz | [`04-ui-alta-fidelidad/`](04-ui-alta-fidelidad/README.md) · Figma pág. `04` y `06` |
| 5 | Estados y casos borde | [`05-estados-y-casos-borde.md`](05-estados-y-casos-borde.md) · [`05-estados/`](05-estados/) · Figma pág. `05` |
| 6 | Prototipo y entrega en herramienta | [`06-prototipo.md`](06-prototipo.md) · Figma pág. `04` |
| 7 | Documentación para desarrollo | [`07-documentacion-desarrollo.md`](07-documentacion-desarrollo.md) |
| 8 | Decisiones de diseño | [`08-decisiones-de-diseno.md`](08-decisiones-de-diseno.md) |

Bitácora de uso de IA, registrada durante el trabajo: [`log-ia.md`](log-ia.md)
Plan de ejecución inicial: [`../PLAN.md`](../PLAN.md)

---

## El archivo de Figma

```
00 · Portada                    tesis, leyenda de cobertura e índice del archivo
01 · Definición                 proto-personas, objetivos, supuestos críticos, riesgo
02 · Arquitectura y Flujos      sitemap por rol · flujo de asignación · flujo de cambio
03 · Wireframes                 4 vistas · 23 notas de diseño
04 · UI Alta Fidelidad          6 pantallas · prototipo navegable · 23 conexiones
05 · Estados y Casos Borde      4 estados, cada uno como flujo propio
06 · Componentes                21 componentes · 39 variantes · 67 variables
```

Cada componente lleva su regla de uso en la descripción, visible en Dev Mode.

---

## La idea en una frase

Los cuatro síntomas del brief tienen una causa común: las hojas de cálculo y la mensajería no conocen el concepto de tiempo ni el de persona. El fallo de cobertura se descubre cuando ya ocurrió.

**Relevo no es un calendario: es un sistema que hace visible el riesgo de descobertura antes de que se materialice.**
