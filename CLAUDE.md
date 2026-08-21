# PatoCreando — Guía de Diseño y Arquitectura

Landing de captación de PatoCreando: sistemas web de alta conversión para creadores
e infoproductores. El posicionamiento, el ICP y el embudo están en
[BUSINESS_STRATEGY.md](BUSINESS_STRATEGY.md) — leelo antes de tocar copy o CTAs.

Actuá como Director de Diseño UI/UX Senior y Arquitecto Frontend: interfaces de
ultra-alta gama (Apple, Vercel, Linear, Stripe) con foco obsesivo en conversión y
en el rendimiento dentro del navegador in-app de Instagram/TikTok.

---

## 1. Arquitectura

**Un solo archivo, sin build, sin dependencias.**

```
index.html            todo: markup + <style> inline + <script> inline
assets/videos/        hero-studio-loop.mp4 (1.4 MB, 1276x720, 12 s, sin audio)
assets/images/        hero-studio-loop-poster.jpg (41 KB, = frame 0 del clip)
BUSINESS_STRATEGY.md  ICP, PUV, embudo, tono de marca
CLAUDE.md             este archivo
```

No hay `package.json`, ni bundler, ni framework, ni CSS externo. **Es deliberado.**
El presupuesto es <0.6 s en 4G dentro de un WebView: un request HTML que ya trae
todo pintable gana contra cualquier cadena de `<link>` + JS. Antes de proponer
Tailwind, React, Vite o un CDN, medí qué le suma y qué le cuesta al presupuesto.

Lo único externo es Plus Jakarta Sans desde Google Fonts, cargada sin bloquear
(`preload as=style` + `onload` que la promueve a stylesheet, con `<noscript>` de
respaldo). No agregues más orígenes externos.

### Orden interno del `<style>`

1. Tokens · 2. Reset · 3. Tipografía · 4. Layout · 5. Componentes base
6. Secciones · 7. Motion · 8. Responsive

Respetá el orden y el índice del comentario de cabecera. Varias reglas dependen
del orden de fuente (misma especificidad, gana la última) — está anotado donde
pasa.

### Módulos del `<script>`

Un único IIFE al final del `<body>`, numerado:

1. Flujo WhatsApp centralizado
2. Calculadora de fuga de tráfico
3. Reveal on scroll
4. Header hairline + barra CTA móvil
5. Tilt 3D con foco de luz en la tarjeta de cierre
6. Video de fondo del hero
7. Detalles

---

## 2. Dirección de arte

**Negro cálido, no OLED puro.** El `#000` absoluto endurece los bordes y aplana el
contraste de las superficies de cristal. El fondo es `#161517`.

| Rol | Token | Valor |
|---|---|---|
| Fondo | `--bg` | `#161517` |
| Fondo profundo | `--bg-deep` | `#101011` |
| Superficie | `--surface-1` | `#1c1b1e` |
| Panel claro | `--panel-light` | `#e4e4e6` |
| Texto | `--fg` | `#f4f4f4` |
| Texto secundario | `--fg-secondary` | `#cdcdcd` |
| **Acento único** | `--accent` | `#38bdf8` (Ice Blue) |
| Positivo / negativo | `--positive` / `--negative` | `#34d399` / `#f87171` |
| Hairline | `--line` | `rgba(255,255,255,0.12)` |

Reglas duras:

- **Un solo acento.** Ice Blue es el único color de la página. Si algo necesita
  destacarse, se resuelve con valor, tamaño o espacio — no con un color nuevo.
- Cero neón saturado, cero ruido cromático.
- `--panel-light` es el beat de contraste que rompe la monotonía oscura
  (`.section-light`). Usalo con cuentagotas: pierde el efecto si se repite.
- Radios generosos (`--r-sm` 12px → `--r-xl` 34px). Es el ajuste que más mueve la
  percepción de precio.
- Glassmorphism vía tres capas: `--glass-sheen` (reflejo cenital en el borde
  superior) + `--glass-bg` (tinte translúcido) + `backdrop-filter`.
- `--mist`: niebla helada proyectada **hacia adentro** (`inset`). Un `box-shadow`
  externo tiñe el fondo; uno inset lee como vapor contenido.
- Curvas Bézier tipo Linear: `--ease-out: cubic-bezier(0.16, 1, 0.3, 1)`.

Tipografía: Plus Jakarta Sans, escalas con `clamp()`. El titular del hero tiene
techo en ~52px a propósito — más grande compite con la marca.

---

## 3. Rendimiento — el requisito que manda

Presupuesto: **<0.6 s de carga, 60 fps de scroll, en gama media dentro del
navegador in-app.** Cuando un efecto y el presupuesto chocan, **gana el
presupuesto** y el efecto degrada.

Patrones ya aplicados, seguí la misma línea:

- **`content-visibility: auto`** en `.section.deferred`: lo que está bajo el fold
  no se pinta hasta acercarse al viewport.
- **Animar solo `transform` y `opacity`.** Lo resuelve el compositor en GPU sin
  repintar. Animar `background-position` o los stops de un gradiente fuerza un
  repaint de pantalla completa por frame: eso es lo que mata el scroll en
  Instagram.
- **`backdrop-filter` con radio reducido abajo de 720px.** El desenfoque escala
  con radio × área y se recalcula por frame. El look de cristal lo sostienen el
  tinte y el hairline, no el radio del blur.
- **`@supports not (backdrop-filter)`**: degrada a superficie sólida en vez de
  quedar transparente con el texto ilegible (in-app viejos de Android).
- **Assets pesados no se descargan en móvil.** Ver §5.

---

## 4. Accesibilidad y progressive enhancement

- **`prefers-reduced-motion: reduce`** está soportado de verdad, no es un gesto:
  anula animaciones y transiciones, congela la atmósfera en una composición
  estable, revela el contenido diferido y desactiva el video del hero.
- **El HTML funciona sin JS.** Los `href` de WhatsApp ya son válidos en el markup;
  el JS solo los enriquece. Cualquier cosa que el JS oculte tiene que estar
  gateada tras la clase `.js` (se marca inline en el `<head>`, antes del primer
  paint) para que sin JS se pinte visible.
- `skip-link`, `aria-labelledby` por sección, `aria-live` en el resultado de la
  calculadora, `aria-hidden` en todo lo decorativo.
- Nada interactivo depende de hover: el sitio se consume mayormente en touch.

---

## 5. Hero con video de fondo

Capas, de atrás hacia adelante: `.hero-media` (poster de respaldo en `::before`)
→ `.hero-video` → `.hero-overlay` (degradé) → `.hero-vignette` → `.hero > .container`.

- **La intensidad se calibra con cuatro tokens en `:root`, no editando las
  reglas:** `--hero-video-opacity` (0.60), `--hero-video-filter`,
  `--hero-overlay-strength` (0.30) y `--hero-vignette-max` (0.84). Los consumen el
  clip, el poster de respaldo y la viñeta a la vez, así que se tunean en vivo desde
  DevTools y los tres estados del hero quedan coherentes solos.
- **`--hero-overlay-strength` es el lever dominante, no la opacidad.** El velo
  plano estaba en 0.60 y era lo que aplastaba el clip: bajarlo a 0.30 duplicó la
  presencia del video a opacidad constante, con un costo de contraste
  despreciable. Si el video se ve apagado, mirá acá antes que la opacidad.
- Arriba el overlay va casi opaco (0.92) porque **ahí el clip ya es negro**
  (luminancia 2-30 sobre 255): no se pierde imagen y el header gana fondo estable.
- El clip entra desaturado: a brillo pleno los azules del estudio compiten con
  `--accent`, que es el único color de la página.

### Cómo se eligieron esos valores

Se compositaron tres frames del clip (t=2/5/8 s) aplicando filtro + opacidad +
overlay + viñeta, y se calculó **contraste WCAG contra el color real de cada
bloque de texto** del hero. Resultado con los valores actuales:

| Zona | Contraste |
|---|---|
| microcopy (13px gris) | **5.83:1** ← la que primero se rompe |
| subtítulo | 6.4:1 |
| titular | ~9:1 |
| CTA | ~8:1 |
| proof | ~18:1 |

AA pide 4.5:1. El margen extra es deliberado: el fondo se mueve, y texto sobre
video pide más aire que sobre un fondo fijo.

**Si tocás cualquiera de los cuatro tokens, la zona que primero cae es el
microcopy.** Verificá ahí antes que en el titular, que tiene el doble de margen.
Datos medidos: `brightness` a 0.85 lo tira abajo de 5:1; opacidad 0.80+ rompe AA.

Un "pocket" oscuro detrás del bloque de texto se probó y **se descartó**: bajaba
la presencia del video un 20% y mejoraba el contraste 0.03. No compensa.

El clip ya tiene movimiento de cámara propio y creciente (diferencia entre frames
de 1.9 a 5.5 sobre el final), así que **no le agregues un zoom lento tipo Ken
Burns**: es redundante y marea.
- **Fade-in de 1.2 s** al evento `loadeddata`; el contenido entra 450 ms después,
  nunca junto con el video.
- El `.hero-overlay` además funde el borde inferior con el fondo: sin eso se ve el
  corte recto del `<video>` sobre el hairline de la sección siguiente.
- **El clip es un ping-pong y por eso el fundido del loop está APAGADO.** El
  archivo son 6 s hacia adelante y los mismos 6 s en reversa, así que su último
  frame es idéntico al primero (diferencia medida: 0.4/255). En un loop sin
  costura el fundido no tapa nada — al revés, mete un bajón de opacidad donde no
  hay corte y termina señalando la costura invisible. El código sigue ahí y se
  reactiva agregándole `data-loop-fade` al `<video>`, para el día que entre un
  clip que corte de verdad. Si lo reactivás: el umbral de salida es 0.6 s y no
  0.4 s, porque `timeupdate` dispara cada ~250 ms y con 0.4 s la transición se
  corta a mitad de camino, que es justo el salto que se quería tapar.
- El poster es **el frame 0 del clip**, que en un ping-pong es también el último.
  Así el arranque del video no produce ningún salto respecto del poster.
- **El poster de respaldo lleva el mismo filtro y la misma opacidad que el video**,
  y se apaga (`.hero-media.is-video-active::before`) cuando el clip sí va a
  reproducirse. Si no, el fondo ya estaría al 32% antes de tiempo y el fade-in no
  se vería. Así el hero pesa igual en todos los estados.
- **El `src` vive en `data-src`** y el JS lo asigna solo si corresponde. El mp4
  **no se pide** cuando: viewport ≤768px, `prefers-reduced-motion`, o
  `navigator.connection.saveData`. En esos casos queda el poster de 31 KB.
  1.7 MB de clip decorativo contra <0.6 s en el in-app de Instagram no cierra, y
  encima se paga con datos del visitante.
- Red de seguridad de 2.2 s y handler de `error`: si el video no carga (404,
  códec, WebView raro), el titular no puede quedarse invisible esperando un
  evento que no llega.
- **Las tres condiciones se guardan como `MediaQueryList`, no como booleanos.**
  Cambian en caliente: maximizar la ventana cruza el corte de 768px y activar las
  animaciones del sistema apaga `reduced-motion`. Leídas una sola vez al cargar,
  el hero se quedaba con el poster para siempre aunque la condición que lo
  bloqueaba ya no existiera. Hay listeners de `change` que reevalúan: arrancan el
  video si se destraba y lo frenan si aparece la condición (respetar
  `reduced-motion` solo al cargar no sería respetarlo).

---

## 6. Conversión

Fricción cero, un solo destino: **WhatsApp**. Sin formularios.

- El número está centralizado en `WA_PHONE` (módulo 1 del script). **Cambialo en
  un solo lugar**, no en los `href`.
- Cada CTA lleva `data-wa` (origen del clic) y `data-wa-intent` (intención). El JS
  arma el mensaje y le suma el resultado de la calculadora, para que el lead
  llegue precalificado.
- La escasez es real: **2 cupos**. No la infles.
- Tono: sofisticada, directa, técnica y ejecutiva. "Zero bullshit". Ver
  BUSINESS_STRATEGY.md §4.

---

## 7. Convenciones de código

- **JavaScript ES5**: `var`, `function`, cero arrow functions, cero `let`/`const`.
  Es por los WebViews viejos de Android, donde no hay transpilación que salve.
  Hoy hay 0 ocurrencias de sintaxis moderna — mantenelo así.
- **Los comentarios explican el porqué, no el qué.** El código dice lo que hace;
  el comentario dice qué decisión hay detrás, qué se probó y qué costo se evitó.
  Ese es el estándar del archivo: si tu comentario parafrasea la línea de abajo,
  sobra.
- Español rioplatense en comentarios y copy.
- Listeners de scroll/pointer con `{ passive: true }`.
- Clases de estado con prefijo `is-` (`is-ready`, `is-revealed`, `is-tilting`).

---

## 8. Antes de dar un cambio por terminado

1. ¿Se sostiene el presupuesto de <0.6 s y 60 fps en gama media?
2. ¿Funciona con `prefers-reduced-motion` activo?
3. ¿Funciona sin JS?
4. ¿Funciona en móvil sin descargar nada pesado de más?
5. ¿Sigue habiendo un solo acento?
6. ¿Los comentarios nuevos explican decisiones, no sintaxis?

Verificá en navegador, no de memoria. Si algo no lo pudiste probar, decilo.
