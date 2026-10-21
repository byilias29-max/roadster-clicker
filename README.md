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
| **Ruleta de la fortuna 🎡** | Aparece sola cada 2-4 min. Gira con frenada real (5 vueltas), va sonando por cada sector que pasa y cae en un premio: **cookies**, **frenesí x7**, **cookies x2 durante 20 s**, **una mejora gratis**, **+1 crédito espacial** o **JACKPOT** (150× la producción + frenesí). Cae confeti y suena la victoria. |
| **Combo de clics** | Si pulsas en ráfaga, el clic sube hasta **x2** (5 % por clic, tope de 20) y se ve en la barra inferior con la barrita que se vacía. El sonido del clic sube de tono con el combo. |
| **Hitos** | Al cruzar 1.000, 10.000, 100.000, 1 M… cookies salta un cartel de celebración con confeti. |
| **22 mejoras** | Cuatro ramas: producción (`Cargador` → `Singularidad`), **cursores automáticos** (`Cursor de Tesla` → `Ejército de cursores`), potencia de clic (`Botón de arranque` → `Modo cohete`) y galleta dorada. |
| **Cursores automáticos** | Son punteros con el **logo de Tesla** que se mueven por la pantalla y pulsan el coche solos. Cada clic automático vale lo mismo que tu clic: el juego avanza sin que toques nada. |
| **Cada mejora enseña lo suyo** | La fila muestra únicamente lo que aporta *esa* mejora (no la suma de todas) y, encima del precio, **lo que da la compra pendiente** (`+0,1 cookies/s`, `+0,5 clic/s`, `x2 al clic`…) en grande y con color. |
| **Dopamina al comprar** | La fila destella, el precio se ilumina, sale una **onda expansiva**, flota el texto de lo que has ganado y suena un tintineo de monedas. |
| **El dinero tiene valor** | El contador grande muestra las cookies y su equivalencia en euros (`≈ 12,34 €`), con rebote suave al subir. |
| **Barra de progreso** | En la parte inferior ves la siguiente mejora que te falta y cuánto llevas (`268 / 400`). |
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
- El fondo es un **canvas**: noche oscura con estrellas que derivan en diagonal hacia abajo-izquierda
  y auroras de color vivo (rojo Tesla y cian). Encima, dentro del escenario, hay una capa (`#bgmove`)
  de **cuadrados con esquinas redondeadas en damasco** —los grises marcados y los negros flojos— que
  se desliza muy lentamente en esa misma diagonal: el patrón es periódico y el desplazamiento es
  exactamente un periodo, así que el bucle **no produce ningún salto**.
  Los degradados se cachean y solo se rehacen al redimensionar la ventana.
- **Rendimiento**: la lógica corre con `setInterval` (100 ms), no con `requestAnimationFrame`, para que la
  producción siga activa con la pestaña en segundo plano. El `rAF` (60 fps) mueve los cursores, gira la
  ruleta, redibuja el fondo a ~40 fps y actualiza el contador. El DOM solo se escribe **cuando el texto
  cambia**, los cursores se mueven con `transform` (GPU) y el pulso del clic usa la Web Animations API
  para no provocar reflows. Coste medido: ~0,011 ms por frame y ~0,007 ms por tick.
- La ruleta gira **en el bucle de render** (no con una transición CSS) para poder sonar cada vez que pasa
  por un sector y frenarla con una curva propia.
- **Cursor personalizado**: puntero con el logo de Tesla (SVG en `data:`), aplicado a todo lo interactivo.
- Las medidas del escenario y del coche se cachean y se recalculan con un `ResizeObserver`
  (no `getBoundingClientRect` en cada frame).
- El tiempo se mide con el reloj real (`Date.now()`), no con el contador de frames: si el navegador
  estrangula una pestaña oculta, la producción sigue contando (tope de 8 h, igual que el offline).
- La producción offline se cobra **sin frenesí** y el frenesí no sobrevive a la recarga.
- Layout responsive: en móvil la tienda baja debajo del escenario y ambos hacen scroll propio.
- Accesible: navegación completa por teclado, foco visible, el modal es un diálogo real
  (Escape lo cierra), se puede hacer zoom y con `prefers-reduced-motion` las animaciones se calmán.
- `window.RC` es un enganche de depuración (estado y funciones forzadas) para tests; no afecta al juego.

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
