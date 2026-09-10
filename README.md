
# Sistema Solar — simulador en el navegador

Simulación del sistema solar en 3D con posiciones astronómicas reales, texturas
generadas por procedimiento y eclipses calculados en el shader. Todo en un solo
archivo HTML, sin instalación ni servidor.


## Archivos

| Archivo | Qué es |
|---|---|
| `sistema-solar-3d.html` | **El simulador completo.** Es el que quieres abrir. |
| `sistema-solar.html` | Versión mínima en canvas 2D (8 planetas, órbitas circulares). Útil si no hay internet. |
| `.respaldo-v1.html` | Copia de la versión anterior del simulador 3D. Puedes borrarla. |

## Cómo se abre

Doble clic en `sistema-solar-3d.html`. Se abre en tu navegador.

La primera vez necesita conexión a internet: descarga la biblioteca **three.js**
desde un CDN (unos 600 kB). A partir de ahí el navegador la guarda en caché y
suele funcionar sin conexión. **Ningún otro recurso viene de fuera**: las
texturas de los planetas, los anillos, la Vía Láctea y los cometas se generan
con ruido procedural dentro de tu propio navegador al arrancar (por eso la
pantalla de carga tarda uno o dos segundos).

Requiere un navegador con WebGL: Chrome, Edge, Firefox o Safari recientes.

## Qué hay dentro

### Astronomía real

- **Posiciones de los planetas** con los elementos keplerianos aproximados de la
  NASA/JPL referidos a J2000 y sus derivadas por siglo (válidos ~1800-2050). La
  ecuación de Kepler se resuelve por Newton-Raphson, con paso limitado para que
  converja también con excentricidades de 0,99 (cometas).
- **La Luna** usa la teoría lunar de baja precisión del *Astronomical Almanac*
  (error < 0,3°), con sus doce términos principales de longitud, siete de latitud
  y cuatro de paralaje. Por eso sus fases, su distancia (perigeo/apogeo) y los
  eclipses caen en su fecha verdadera.
- **Rotaciones** con el periodo y la inclinación del eje de cada cuerpo: Venus
  gira al revés, Urano rueda tumbado 98°, las lunas están acopladas por marea.
- **Cinco planetas enanos**: Ceres, Plutón, Haumea, Makemake y Eris. Los cuatro
  últimos con elementos aproximados (la ficha lo indica).
- **Cuatro cometas** con su paso por el perihelio real: 1P/Halley, 2P/Encke,
  67P/Churyumov-Gerasimenko y C/1995 O1 Hale-Bopp. La cola crece según 1/r² y
  siempre apunta en dirección contraria al Sol, con una parte de iones estrecha
  y otra de polvo más abierta.
- **Cinturones**: 6.000 asteroides con los huecos de Kirkwood, 900 troyanos en
  los puntos L4 y L5 de Júpiter y 4.000 objetos del cinturón de Kuiper, todos con
  su periodo orbital propio según la tercera ley de Kepler.
- **Cuatro sondas** (Voyager 1 y 2, Pioneer 10, New Horizons) por extrapolación
  rectilínea de su rumbo y velocidad actuales. Posición aproximada, y así se dice
  en su ficha.
- **El cielo de fondo es el de verdad**: 60 estrellas con nombre en sus
  coordenadas J2000 convertidas a la eclíptica, cinco constelaciones dibujadas
  (Orión, Osa Mayor, Cruz del Sur, Casiopea y el Cisne) y la banda de la Vía
  Láctea orientada por su polo galáctico real.

### Eclipses y sombras

No son un efecto pintado: cada fragmento de cada superficie traza un rayo hacia
el Sol y comprueba qué cuerpos lo tapan, comparando radios angulares. De ahí
salen la **umbra y la penumbra** de forma natural.

- Pon la fecha en el **12 de agosto de 2026 a las 17:00 UTC**, sigue a la Tierra
  y colócate entre ella y el Sol: verás la sombra de la Luna sobre el Atlántico
  norte. Es el eclipse solar total de ese día.
- En un plenilunio con la Luna cerca de un nodo, la sombra de la Tierra cae
  sobre la Luna: eclipse lunar.
- Acelera el tiempo mirando a Júpiter y verás pasar las sombras de Ío, Europa,
  Ganímedes y Calisto sobre sus nubes.
- Los anillos de Saturno proyectan su sombra sobre el planeta —con la división
  de Cassini incluida— y el planeta proyecta la suya sobre los anillos.

### Aspecto

- Texturas procedurales por ruido fBm: continentes, desiertos y casquetes
  polares en la Tierra, con **capa de nubes independiente**, **luces de ciudad
  que solo se ven en la cara nocturna** y brillo especular únicamente sobre el
  agua; bandas turbulentas y Gran Mancha Roja en Júpiter; Valles Marineris y los
  volcanes de Tharsis en Marte; los mares y cráteres de la Luna; el azufre de
  Ío, las grietas de hielo de Europa, las «rayas de tigre» de Encélado, la bruma
  naranja de Titán, el corazón de Plutón, los puntos brillantes de Ceres.
- Atmósferas con dispersión de Fresnel que enrojece en el terminador.
- Sol con granulación animada, manchas, fáculas, oscurecimiento del limbo,
  corona y destello de seis puntas.
- Postprocesado con *bloom* y mapeo tonal ACES, con exposición y resplandor
  regulables.

## Controles

| Acción | Cómo |
|---|---|
| Girar la cámara | Arrastrar |
| Acercar / alejar | Rueda del ratón, o pellizco en pantalla táctil |
| Seguir un astro | Doble clic sobre él, clic en su nombre, o los botones de abajo |
| Vista general | <kbd>Esc</kbd> |
| Pausar el tiempo | <kbd>Espacio</kbd> |
| Un día atrás / adelante | <kbd>←</kbd> <kbd>→</kbd> (con <kbd>Shift</kbd>, un año) |
| Más o menos velocidad | <kbd>,</kbd> <kbd>.</kbd> |
| Ir al Sol y a los planetas | <kbd>0</kbd> … <kbd>9</kbd> |
| Vista desde la superficie | <kbd>F</kbd> |
| Nombres / órbitas / constelaciones | <kbd>N</kbd> / <kbd>O</kbd> / <kbd>C</kbd> |
| Ocultar la interfaz | <kbd>P</kbd> |
| Ayuda | <kbd>H</kbd> |

En el panel izquierdo, además: fecha y hora exactas, velocidad del tiempo (de una
hora por segundo a casi tres años por segundo, en ambos sentidos), tamaño de los
planetas, escala de distancias, capas que mostrar y ajustes de imagen.

**Vista desde la superficie**: elige un planeta o una luna, cambia la cámara a
«Desde la superficie» y ajusta la latitud. Estarás de pie sobre el astro viendo
salir y ponerse el Sol, con el cielo real de fondo.

## Escalas

Los tamaños reales son invisibles al lado de las distancias reales: a escala, la
Tierra sería medio píxel. Por eso:

- Los **diámetros** están exagerados (regulables con el control «Tamaños»).
- Las **distancias** vienen comprimidas con una raíz cuadrada para que quepan
  Mercurio y Neptuno en la misma pantalla. Cambia «Distancias» a **Real** y verás
  las proporciones verdaderas: el sistema solar interior se convierte en un punto.
- Las **lunas** están más cerca de su planeta de lo que están de verdad, para que
  se vean. Eso hace que las sombras de los eclipses sean mayores de lo real,
  aunque caen en el instante correcto.

## Cosas que no simula

- Perturbaciones entre planetas: cada órbita es una elipse kepleriana con
  derivadas seculares, no una integración de N cuerpos.
- Precesión de los ejes, nutación y corrección por tiempo de luz.
- Las órbitas de las lunas (salvo la Luna) son circulares, con su periodo e
  inclinación correctos pero con la fase de partida arbitraria.
- Los elementos de Haumea, Makemake, Eris y las posiciones de las sondas son
  aproximados.

## Para trastear

Con la consola del navegador abierta (<kbd>F12</kbd>) tienes un objeto `SS`:

```js
SS.fecha(2026, 8, 12, 17);      // ir a una fecha (año, mes, día, hora UTC)
SS.enfocar(SS.PLANETAS[5]);     // seguir a Saturno
SS.camara(26, 1.3, 1.0);        // distancia, inclinación, giro
SS.velocidad(30);               // días por segundo
SS.pausa();                     // alternar
SS.estado();                    // dónde estamos
```

Al arrancar, la consola ejecuta unas comprobaciones de mecánica celeste
(convergencia de Kepler, distancia Tierra-Sol en J2000, tercera ley en los ocho
planetas, rango de la distancia lunar, coincidencia Sol-Luna en el eclipse de
2026 y conversión ecuatorial-eclíptica). Si algo se rompiera al tocar el código,
salta ahí antes que en pantalla.
