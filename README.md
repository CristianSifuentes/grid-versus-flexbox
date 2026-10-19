# grid-versus-flexbox

Código de tutorial: CSS Grid contra Flexbox, cuándo usar uno u otro. Incluye una guía de referencia y **20 ejercicios paso a paso** en [`ejercicios/`](ejercicios/README.md).

## Ejecutar en local

Requiere [Node.js](https://nodejs.org/).

```bash
npm install
npm start         # proyecto de conciertos → http://127.0.0.1:5500
npm run ejercicios # índice de ejercicios   → http://127.0.0.1:5501/ejercicios/
```

Los dos usan live reload: el navegador se recarga solo al guardar.

También puedes instalar en VS Code la extensión recomendada **Live Server** (`ritwickdey.LiveServer`) y hacer clic en **Go Live** en la barra de estado.

## Estructura

```
index.html, styles.css   Proyecto de conciertos (tarjetas + banner)
img/                     Imágenes compartidas
ejercicios/              20 prácticas: index.html + styles.css (tú escribes) + README.md (guía)
Layout en CSS Flexbox y Grid.md   Apuntes originales
```

## Proyecto de conciertos

Dentro de `.container` se puede aplicar tanto flexbox como grid. Con solo `display: flex;`, los hijos (`.concerts-container` y `.banner`) quedan **en fila** porque, por defecto, `flex-direction` vale `row`, `flex-wrap` vale `nowrap` y `align-items` vale `stretch` (los dos tienen la misma altura). El reto extra del [ejercicio 20](ejercicios/20-dashboard-final/README.md) termina este layout.

---

# Guía: Layout en CSS con Flexbox y Grid

## La idea en una línea

Flexbox acomoda elementos en **un eje** (fila o columna); grid los acomoda en **filas y columnas a la vez**. Se aprenden los dos y se combinan: **grid para la estructura** (macro) y **flexbox para los componentes** (micro).

## Flexbox: una dimensión

`display: flex` convierte al elemento en contenedor y a sus **hijos directos** en ítems. Los nietos no cambian.

- **Eje principal**: lo define `flex-direction` (`row` por defecto, sigue la dirección de escritura). `justify-content` reparte sobre él.
- **Eje cruzado**: perpendicular al principal. `align-items` alinea cada ítem sobre él.
- En `column` los ejes se voltean: `justify-content` pasa a vertical y `align-items` a horizontal.

| Propiedad | Va en | Qué hace |
| --- | --- | --- |
| `flex-direction` | contenedor | Define el eje principal: `row` o `column` (y `-reverse`) |
| `flex-wrap` | contenedor | Deja saltar de línea (`wrap`); por defecto `nowrap` |
| `flex-flow` | contenedor | Atajo de `flex-direction` + `flex-wrap` |
| `justify-content` | contenedor | Reparte el espacio libre en el eje principal |
| `align-items` | contenedor | Alinea en el eje cruzado |
| `align-content` | contenedor | Alinea las **líneas** cuando hay varias (`wrap`) |
| `gap` | contenedor | Espacio entre ítems |
| `flex-grow` | ítem | Cuánto crece si sobra espacio |
| `flex-shrink` | ítem | Cuánto se encoge si falta espacio |
| `flex-basis` | ítem | Tamaño inicial sobre el eje principal |
| `flex` | ítem | Atajo de grow + shrink + basis |
| `align-self` | ítem | Sobrescribe `align-items` para un ítem |
| `order` | ítem | Cambia el orden **visual** (no el del teclado) |

**Por defecto**, con solo `display: flex`: `row`, `nowrap`, `align-items: stretch` y cada ítem con `flex: 0 1 auto` (no crece, sí se encoge).

```css
.centrar {
  display: flex;
  justify-content: center; /* eje principal */
  align-items: center;     /* eje cruzado */
}
```

**Cuando hay `wrap`, cada línea se comporta como un contenedor flex nuevo**: reparte su espacio por su cuenta, y los ítems de una línea **no se alinean** con los de la anterior. Si necesitas alinear filas **y** columnas, eso es trabajo de grid.

**`margin: auto` en un ítem flex** absorbe todo el espacio libre de ese lado. Es la forma de separar *un* ítem del resto (logo a la izquierda, enlaces a la derecha) o de empujar el pie de una tarjeta hasta abajo (`margin-top: auto` en `column`).

## Grid: dos dimensiones

`display: grid` define filas **y** columnas a la vez. El contenedor marca las pistas y cada hijo se acomoda en ellas.

- Grid numera las **líneas** entre pistas, no las celdas: 3 columnas = 4 líneas. `-1` es la última línea.
- `grid-column: 2 / 4` va de la línea 2 a la 4, o sea, ocupa las columnas 2 y 3. `span 2` ocupa dos pistas donde caiga.
- `grid-area` con números sigue el orden fila-inicio / col-inicio / fila-fin / col-fin.
- `fr` = una fracción del espacio libre. Ojo: `1fr` es en realidad `minmax(auto, 1fr)`.
- Las pistas que no defines se crean solas (**grid implícito**) con tamaño `auto`; se controlan con `grid-auto-rows` / `-columns`.

| Propiedad | Va en | Qué hace |
| --- | --- | --- |
| `grid-template-columns` / `-rows` | contenedor | Número y tamaño de columnas / filas |
| `grid-template-areas` | contenedor | Dibuja el layout con nombres de áreas |
| `gap`, `row-gap`, `column-gap` | contenedor | Espacio entre pistas |
| `grid-auto-rows` / `-columns` | contenedor | Tamaño de las pistas que se crean solas |
| `grid-auto-flow` | contenedor | Cómo se acomodan los ítems sin posición (`row`, `column`, `dense`) |
| `justify-items` / `align-items` | contenedor | Alinean cada ítem **dentro de su celda** |
| `justify-content` / `align-content` | contenedor | Mueven **las pistas** cuando sobra espacio |
| `grid-column` / `grid-row` | ítem | De qué línea a qué línea va |
| `grid-area` | ítem | Las cuatro líneas en una, o un nombre de `grid-template-areas` |
| `justify-self` / `align-self` | ítem | Alineación de un solo ítem en su celda |

### Layout de página con áreas

```css
.page {
  display: grid;
  grid-template-columns: 200px 1fr 250px;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header header header"
    "nav    main   aside"
    "footer footer footer";
}
.page > header { grid-area: header; }
/* … */
```

Cada área debe ser un **rectángulo** (si no, se descarta toda la declaración) y `.` deja una celda vacía. Para hacerla responsive, basta con **redibujar el mapa** en un media query. Si cambia el número de filas, **revisa también `grid-template-rows`**.

### Galería que se reacomoda sola, sin media queries

```css
.galeria {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(min(200px, 100%), 1fr));
  gap: 1rem;
}
```

- El navegador calcula cuántas columnas de al menos 200px caben (gap incluido) y reparte el resto con `1fr`.
- `min(200px, 100%)` evita el desborde en contenedores más angostos que 200px.
- **`auto-fill` contra `auto-fit`**: `auto-fill` **conserva** las columnas vacías (pocos ítems mantienen su tamaño); `auto-fit` las **colapsa** y los ítems se estiran a todo el ancho. Con muchos ítems no hay diferencia.

## Box Alignment: lo que comparten

Las propiedades de alineación nacieron en flexbox y hoy son una especificación común (*Box Alignment Level 3*), que también usa grid. Diferencias:

- En grid se escribe `start` / `end`. En flex se ha usado `flex-start` / `flex-end` (los dos aceptan `start` / `end`).
- En grid cada ítem se alinea **dentro de su grid area** (una o varias celdas).
- `justify-items` no existe en flex, porque ahí el eje principal se reparte entre todos con `justify-content`.
- Centrar algo en 2D: `display: flex` + `justify-content` + `align-items`, o `display: grid` + `place-items: center`. Los dos son válidos.

## Superposición, posicionamiento y nietos

- **Superponer:** varios ítems con la misma celda (`grid-area: 1 / 1`) se apilan. Se colocan con `-self` y el orden lo da el HTML o `z-index` (sin necesidad de `position`). El contenedor sigue midiendo su contenido.
- **`position: absolute` en grid:**
  - Si el contenedor tiene `position: relative` y el ítem tiene líneas asignadas, los offsets se miden desde **su grid area**. Sin líneas, desde todo el contenedor.
  - Si ningún ancestro está posicionado, desde el viewport.
  - En todos los casos el ítem **sale del flujo**: el auto-placement ocupa su lugar y no crea pistas.
- **`display: contents`:** la caja del elemento desaparece y sus hijos pasan a ser ítems del grid (o flex) del abuelo. Pierde fondo, borde y padding, y en algunos navegadores ha eliminado la semántica de `<ul>`, `<button>`, etc.
- **`subgrid`:** `grid-template-rows: subgrid` (o `-columns`) hace que un ítem use las pistas del grid padre **conservando su caja**. Es la forma moderna de alinear contenido entre tarjetas. Funciona en todos los navegadores principales desde 2023.

## ¿Cuál uso?

**Flexbox** cuando acomodas cosas en una dirección y el contenido manda (va del contenido hacia afuera); **grid** cuando el layout manda en filas y columnas (va del layout hacia adentro). Si estás quitándole flexibilidad a flexbox (anchos en % para alinear con la fila de arriba), probablemente querías grid.

|  | Flexbox | Grid |
| --- | --- | --- |
| Dimensiones | 1: fila **o** columna | 2: filas **y** columnas |
| Quién manda | El contenido empuja el tamaño | El contenedor define las pistas |
| Responsive natural | `flex-wrap` + `flex-grow` | `auto-fill` / `auto-fit` + `minmax()` |
| Uso típico | Navbar, botones, centrar, contenido de tarjetas | Layout de página, galerías, dashboards, hero |
| Superponer | No está pensado para eso | Sí, poniendo ítems en la misma celda |

## Accesibilidad: el orden visual contra el del DOM

`order`, `row-reverse` / `column-reverse`, la colocación con `grid-area` y `grid-auto-flow: dense` cambian **solo lo que se ve**. El foco del teclado (Tab) y los lectores de pantalla siguen el **orden del HTML**. Esto aplica a grid igual que a flexbox.

**Regla:** el orden lógico va en el HTML. Reordena con CSS solo cuando no cambia el sentido, y prueba siempre navegando con Tab.

## Trampas comunes

1. **`flex` no significa "misma anchura".** Es atajo de grow + shrink + basis. `flex: 1` (= `1 1 0%`) en todos da anchos iguales, salvo que un contenido no se pueda partir; `min-width: 0` en el ítem lo arregla.
2. **`flex-grow`, `flex-shrink` y `flex-basis` van en el ítem**, no en el contenedor.
3. **`flex-basis` es altura en `column`**, porque sigue al eje principal.
4. **Esperar que `display: flex` afecte a los nietos.** Solo los hijos directos son ítems (salvo con `display: contents` o `subgrid`).
5. **Contar celdas en vez de líneas en grid.** `span 2` ocupa dos pistas; `2 / 4` va de línea a línea.
6. **Nombres viejos:** `grid-gap`, `grid-row-gap` y `grid-column-gap` hoy son `gap`, `row-gap` y `column-gap`, y `gap` también funciona en flexbox.
7. **`1fr 1fr` no garantiza columnas iguales.** `1fr` = `minmax(auto, 1fr)`, y un contenido largo ensancha su columna. Usa `minmax(0, 1fr)`; es el `min-width: 0` de grid.
8. **Olvidar `grid-template-rows` en el media query.** Si el mapa pasa de 3 a 5 filas pero las filas siguen en `80px 1fr 60px`, el `1fr` le cae a la fila equivocada.
9. **Creer que reordenar con grid es accesible por defecto.** No lo es: ver la sección de accesibilidad.
10. **Espaciar con `margin` en vez de `gap`.** Los márgenes se suman entre vecinos y dejan espacio en los bordes; `gap` solo separa.
11. **Alturas fijas en filas de layout** (`80px` para el header). Si el contenido crece, se desborda; usa `auto`.

## Fuentes

- Apuntes propios: [`Layout en CSS Flexbox y Grid.md`](Layout%20en%20CSS%20Flexbox%20y%20Grid.md)
- MDN: *Relación de Grid Layout con otros métodos de diseño y posicionamiento*
- MDN: *Test your skills: Flexbox*
- StudyCode Pro: *Flexbox vs Grid: cuándo usar cada uno con ejemplos prácticos* (con correcciones: el bug de las filas responsive, "grid para centrar" no es un error)
- Lukman Faiz: *CSS Grid vs Flexbox: When to Use Each and How to Mix Them Without Pain* (con corrección: grid **no** evita el problema de accesibilidad al reordenar)
