# TKDFormsVR 🥋

Entrenador de **poomsae (formas) de taekwondo en realidad virtual**, pensado para
practicarse con un Meta Quest. Un instructor virtual tipo sombra demuestra cada
movimiento frente a ti y la app te narra paso a paso qué hacer — diseñado para
que alguien que **nunca ha visto la forma** pueda seguirla.

**Demo en vivo:** https://jedealbag.github.io/TKDFormsVR/

## Probarlo

1. Abre la URL de arriba en el **navegador del Meta Quest**.
2. Pulsa **Entrar a VR** (acepta el permiso de realidad virtual).
3. Mira al frente al entrar: la app calibra tu mirada inicial y coloca el
   inicio de la forma y al instructor **delante de ti**.
4. Sigue al instructor sombra y las instrucciones en pantalla.

> ⚠️ **Nota técnica importante:** WebXR exige que la página corra como documento
> de nivel superior. Si se incrusta en un iframe de otro origen sin
> `allow="xr-spatial-tracking"`, el visor nunca muestra el permiso de VR.
> Por eso se publica en GitHub Pages (HTTPS directo, sin marcos), no en
> páginas que embe ban el contenido.

## Estado actual

- ✅ Forma completa: **Taegeuk Il Jang** (18 movimientos)
- ✅ Instructor sombra en espejo (demuestra brazos, posturas y patadas)
- ✅ Narración paso a paso: giro (izq/der), pie que va al frente, técnica
- ✅ Calibración de mirada inicial
- ✅ Modos: aprender / pasada con cadencia; forma completa o por mitades
- ✅ Patadas confirmadas con gatillo (sin sensores en tobillos)
- ✅ Retroalimentación háptica en controles

## Estructura del proyecto

```
TKDFormsVR/
├── index.html   # Toda la app (un solo archivo autocontenido, ~36 KB)
└── README.md    # Este documento
```

Todo vive en **`index.html`** a propósito: un solo archivo facilita
publicarlo en cualquier hosting estático (GitHub Pages, Netlify, etc.) sin
proceso de build.

## Stack técnico

| Pieza | Detalle |
|---|---|
| Render 3D | [Three.js 0.160.0](https://threejs.org) vía CDN (`unpkg.com`) |
| VR | WebXR (`navigator.xr`, sesión `immersive-vr`) |
| Lenguaje | JavaScript + HTML/CSS en un solo archivo |
| Audio | `AudioContext` para tonos/guías |
| Háptica | `GamepadHapticActuator` de los controles |
| Sin build | No hay bundler, dependencias ni backend |

## Datos de la forma

La secuencia vive en el arreglo `MOVES` dentro de `index.html`. Cada paso es
un objeto así:

```js
{
  n: 1,                          // número de movimiento
  dir: -90,                      // giro respecto al paso anterior (grados)
  side: 'left',                  // lado que ejecuta la técnica
  kind: 'low',                   // tipo: 'low' | 'punch' | 'high' | 'inside' | 'kick'
  travel: 'GIRA 90° A LA IZQUIERDA', // instrucción de desplazamiento (HUD)
  leadSide: 'left',              // lado del pie que va al frente
  lead: 'PIE IZQUIERDO AL FRENTE',   // instrucción de pie (HUD)
  tech: 'Bloqueo bajo izquierdo',     // nombre de la técnica
  stance: 'Ap sogi izquierdo'         // postura
}
```

Para **agregar otra forma** (p. ej. Taegeuk Yi Jang) basta con definir un nuevo
arreglo con este mismo esquema y un selector en el menú inicial.

## Cómo funciona por dentro

- **Instructor sombra:** figura humanoide procedural (construida con
  primitivas de Three.js) que se anima por código: interpola posiciones de
  brazos/piernas según el `kind` y `side` de cada paso, y rota para marcar
  los giros (`dir`).
- **Narración:** al cambiar de paso se muestra `travel` + `lead` + `tech` en
  grande y permanece en el HUD durante la ejecución.
- **Calibración inicial:** al iniciar la sesión XR se lee la orientación real
  del visor y se rota la escena para que el paso 1 quede al frente.
- **Detección VR:** `navigator.xr.isSessionSupported('immersive-vr')` decide
  si se muestra el botón "Entrar a VR".

## Cómo contribuir

Ideas abiertas, ordenadas por impacto:

1. **Mejores animaciones** — el instructor actual es procedural y simple.
   Se puede reemplazar por un modelo con rig (GLTF) y animaciones reales de
   taekwondo, o refinar las interpolaciones actuales.
2. **Mejores visuales** — entorno del dojang, iluminación, partículas en
   impactos, modelos de uniforme (dobok).
3. **Narración por voz** — hoy las instrucciones son texto en el HUD;
   `speechSynthesis` (Web Speech API) podría leerlas en español.
4. **Más formas** — Taegeuk 2–8, palgwe, o formas de otros estilos usando el
   esquema `MOVES`.
5. **Evaluación** — comparar la pose del usuario (vía controles/cámara) con
   la del instructor y dar puntuación.
6. **Modo espejo real** — usar la cámara del Quest (passthrough) es
   experimental; hoy el fondo es virtual.

Para proponer cambios: abre un *issue* o manda un *pull request* contra `main`.

## Publicación

El sitio se sirve con **GitHub Pages** desde la rama `main`, carpeta `/(root)`:
*Settings → Pages → Deploy from a branch*. Cada push a `main` se publica
solo en 1–2 minutos.

## Licencia

Pendiente de definir.
