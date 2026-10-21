# ROADSTER CLICKER

Un **cookie clicker** en el que, en lugar de una galleta, pulsas un **Tesla Roadster**.

Todo el juego vive en un único archivo (`index.html`): sin dependencias, sin build step,
sin frameworks. Solo HTML + CSS + SVG + Canvas 2D + WebAudio.

## Jugar

```sh
python -m http.server 8123 --directory .
# abre http://localhost:8123
```

O simplemente abre `index.html` en el navegador (funciona también con `file://`).

En línea: <https://byilias29-max.github.io/roadster-clicker/>

## Qué hay dentro

| Mecánica | Detalle |
|---|---|
| **Clic en el coche** | Cada pulsación da cookies. El Roadster rebota, tiembla la pantalla, escupe llamas y las ruedas giran. |
| **18 mejoras** | Tres ramas: producción (`Cargador` → `Singularidad`), potencia de clic (`Botón de arranque` → `Modo cohete`) y galleta dorada. |
| **Compra x1 / x10 / MÁX** | El precio sube ×1,15 por cada unidad comprada. |
| **Galleta dorada** | Aparece sola cada ~1 min. 50 % de probabilidad de **frenesí x7** (15 s), 50 % de un montón de cookies. Dura 12 s y se va sola. Se puede coger con el ratón, con el dedo o con el teclado (Intro / Espacio). |
| **12 logros** | Con aviso emergente y panel propio. |
| **Prestigio orbital** | Con 1.000.000 de cookies lanzas el Roadster en el Falcon Heavy (animación de despegue) y gana **créditos espaciales**: +2 % permanente de producción y de clic por crédito. |
| **Guardado** | `localStorage` con autoguardado cada 6 s, producción offline al volver (máx. 8 h) y **exportar / importar** la partida con una cadena base64. |
| **Sonido** | Sintetizado con WebAudio: rugido eléctrico al pulsar, blips de compra, arpegio de la dorada y whoosh del lanzamiento. Tecla `M` para silenciar. |

## Guardar, exportar e importar

- **GUARDAR** escribe la partida en `localStorage` (clave `roadster-clicker-v1`).
  El autoguardado también salta cada 6 s y al cerrar o esconder la pestaña.
- **EXPORTAR** copia la partida al portapapeles como una cadena **base64** (texto UTF-8).
  Si el navegador no deja copiar, se muestra en un cuadro de texto para que la copies a mano.
- **IMPORTAR** hace lo contrario: pegas la cadena y la partida se restaura (cookies, mejoras,
  créditos, logros y tiempo de juego). La cadena se valida antes de aplicarse: si no es una
  partida válida o es de otra versión, no se toca nada.
- **REINICIAR** borra la partida **definitivamente** (cookies, mejoras, créditos y logros)
  y limpia la clave `roadster-clicker-v1` del navegador. No hay vuelta atrás: exporta antes
  si quieres conservarla.

> Si borras los datos del navegador o juegas en otro dispositivo, usa EXPORTAR/IMPORTAR
> para llevar la partida contigo.

## Detalles técnicos

- El Roadster es **SVG inline** (carrocería, cristal, alerón, faros y ruedas de radios). Las ruedas
  giran según la producción y el escape escupe llamas al pulsar.
- El fondo es un **canvas** con campo de estrellas y resplandor rojo del taller. Los degradados
  se cachean y solo se rehacen al redimensionar la ventana.
- La lógica corre con `setInterval` (100 ms), no con `requestAnimationFrame`, para que la producción
  siga activa con la pestaña en segundo plano. El `rAF` solo dibuja el cielo.
- El tiempo se mide con el reloj real (`Date.now()`), no con el contador de frames: si el navegador
  estrangula una pestaña oculta, la producción sigue contando (tope de 8 h, igual que el offline).
- La producción offline se cobra **sin frenesí** y el frenesí no sobrevive a la recarga.
- Layout responsive: en móvil la tienda baja debajo del escenario y ambos hacen scroll propio.
- Accesible: navegación completa por teclado, foco visible, el modal es un diálogo real
  (Escape lo cierra) y se puede hacer zoom en el navegador.

## Despliegue

Es un sitio estático (un solo `index.html`).

**GitHub Pages** (ya activo): el repositorio publica la rama `main` en
`https://byilias29-max.github.io/roadster-clicker/`. No hace falta ningún workflow.

**Vercel** (opcional): basta con importar el repo, o desplegar a mano:

```sh
deno run -A npm:vercel@latest deploy --prod --yes
```

El `vercel.json` incluye las cabeceras de seguridad (HSTS, `X-Frame-Options`,
`Permissions-Policy`, `nosniff` y `Referrer-Policy`) y la caché de estáticos.

## Licencia

MIT — ver [LICENSE](LICENSE).
