# ROADSTER CLICKER 🚗🍪

Un **cookie clicker** en el que, en lugar de una galleta, pulsas un **Tesla Roadster**.

Todo el juego vive en un único archivo (`index.html`): sin dependencias, sin build step,
sin frameworks. Solo HTML + CSS + SVG + Canvas 2D + WebAudio.

## Jugar

```sh
python -m http.server 8123 --directory .
# abre http://localhost:8123
```

O simplemente abre `index.html` en el navegador (funciona también con `file://`).

## Qué hay dentro

| Mecánica | Detalle |
|---|---|
| **Clic en el coche** | Cada pulsación da cookies. El Roadster rebota, tiembla la pantalla, escupe llamas y las ruedas giran. |
| **18 mejoras** | Tres ramas: producción (`Cargador` → `Singularidad`), potencia de clic (`Botón de arranque` → `Modo cohete`) y galleta dorada. |
| **Compra x1 / x10 / MÁX** | El precio sube ×1,15 por cada unidad comprada. |
| **Galleta dorada** | Aparece sola cada ~1 min. 50 % de probabilidad de **frenesí x7** (15 s), 50 % de un montón de cookies. Dura 12 s y se va sola. |
| **12 logros** | Con aviso emergente y panel propio. |
| **Prestigio orbital** | Con 1.000.000 de cookies lanzas el Roadster en el Falcon Heavy (animación de despegue) y gana **créditos espaciales**: +2 % permanente de producción y de clic por crédito. |
| **Guardado** | `localStorage` con autoguardado cada 6 s, producción offline al volver (máx. 8 h) y exportar la partida al portapapeles. |
| **Sonido** | Sintetizado con WebAudio: rugido eléctrico al pulsar, blips de compra, arpegio de la dorada y whoosh del lanzamiento. Tecla `M` para silenciar. |

## Detalles técnicos

- El Roadster es **SVG inline** (carrocería, cristal, alerón, faros y ruedas de radios). Las ruedas
  giran según la producción y el escape escupe llamas al pulsar.
- El fondo es un **canvas** con campo de estrellas y resplandor rojo del taller.
- La lógica corre con `setInterval` (100 ms), no con `requestAnimationFrame`, para que la producción
  siga activa con la pestaña en segundo plano. El `rAF` solo dibuja el cielo.
- Layout responsive: en móvil la tienda baja debajo del escenario y ambos hacen scroll propio.

## Despliegue

Es un sitio estático. En Vercel basta con importar el repo (o desplegar a mano):

```sh
deno run -A npm:vercel@latest deploy --prod --yes
```
