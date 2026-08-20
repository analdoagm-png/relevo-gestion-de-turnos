# Presentación · Relevo

Deck de 18 slides que recorre las ocho fases de la entrega, en el mismo orden que el índice de entregables.

**Abrir:** `presentacion/index.html` en cualquier navegador. Sin dependencias ni build.

## Navegación

| Acción | Tecla |
|---|---|
| Avanzar | `→` · `↓` · `espacio` |
| Retroceder | `←` · `↑` |
| Primera / última | `Inicio` · `Fin` |
| Editar texto en el navegador | `E` |
| Guardar copia editada | `Ctrl/Cmd + S` en modo edición |

En móvil y tablet funciona el deslizamiento lateral. La URL guarda la slide actual en el hash, así que se puede enlazar una diapositiva concreta (`index.html#10`).

## Estructura

```
presentacion/
├── index.html     deck completo, CSS y JS en línea
└── img/           21 exportaciones del archivo de Figma
```

Las imágenes son rutas relativas: si mueves el HTML, lleva `img/` con él.

## Diseño

Escenario fijo de 1920×1080 escalado uniformemente al viewport — nunca re-maqueta por tamaño de pantalla, ni siquiera en móvil.

El sistema visual usa la **paleta y las tipografías reales de Relevo**: Archivo para titulares, Public Sans para texto, IBM Plex Mono para datos y chrome, azul `#1B4DE4` como única tinta.

Una retícula milimetrada permanece detrás de cada slide y los filetes superior e inferior enmarcan todas. Cero radios de esquina, cero sombras, cero degradados: la profundidad es estructural.

**Excepción deliberada al sistema de dos tintas:** los colores del semáforo de cobertura (verde, ámbar, rojo) aparecen únicamente dentro de indicadores de estado y datos, nunca como decoración. Es la misma regla que gobierna el producto — el acento nunca significa estado, y el estado nunca hace de acento.
