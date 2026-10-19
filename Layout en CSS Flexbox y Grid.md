# Layout en CSS: Flexbox y Grid

Oct 16, 2026 · @Cristian S

## La idea en una línea

Flexbox acomoda elementos en **un eje** (fila o columna); grid los acomoda en **filas y columnas a la vez**. Se aprenden los dos y se combinan.

&#91;embedded content: flexbox en row y column · grid y sus líneas\]

En `column` los ejes se voltean: `justify-content` pasa a trabajar en vertical y `align-items` en horizontal.

## Flexbox: una dimensión

`display: flex` convierte al elemento en contenedor y a sus **hijos directos** en ítems. Los nietos no cambian.

- **Eje principal**: lo define `flex-direction` (`row` por defecto, sigue la dirección de escritura). `justify-content` reparte sobre él.
- **Eje cruzado**: perpendicular al principal. `align-items` alinea cada ítem sobre él.
- En `column` los ejes se voltean: `justify-content` pasa a vertical.

| Propiedad | Va en | Qué hace |
| --- | --- | --- |
| `flex-direction` | contenedor | Define el eje principal: `row` o `column` |
| `flex-wrap` | contenedor | Deja saltar de línea (`wrap`); por defecto `nowrap` |
| `flex-flow` | contenedor | Atajo de `flex-direction` + `flex-wrap` |
| `justify-content` | contenedor | Reparte en el eje principal |
| `align-items` | contenedor | Alinea en el eje cruzado |
| `align-content` | contenedor | Alinea las líneas cuando hay varias (`wrap`) |
| `gap` | contenedor | Espacio entre ítems |
| `flex-grow` | ítem | Cuánto crece si sobra espacio |
| `flex-shrink` | ítem | Cuánto se encoge si falta espacio |
| `flex-basis` | ítem | Tamaño inicial sobre el eje principal |
| `flex` | ítem | Atajo de grow + shrink + basis |

**Por defecto**, con solo `display: flex`: `row`, `nowrap`, `align-items: stretch` y cada ítem con `flex: 0 1 auto` (no crece, sí se encoge).

```css
.centrar {
  display: flex;
  justify-content: center; /* eje principal */
  align-items: center;     /* eje cruzado */
}
```

## Grid: dos dimensiones

`display: grid` define filas **y** columnas a la vez. El contenedor marca las pistas y cada hijo se acomoda en ellas.

- Grid numera las **líneas** entre pistas, no las celdas: 3 columnas = 4 líneas.
- `grid-column: 2 / 4` va de la línea 2 a la 4, o sea ocupa las columnas 2 y 3.
- `grid-area` en orden: fila-inicio / col-inicio / fila-fin / col-fin.
- `fr` = una fracción del espacio libre.

| Propiedad | Va en | Qué hace |
| --- | --- | --- |
| `grid-template-columns` / `-rows` | contenedor | Número y tamaño de columnas / filas |
| `grid-template-areas` | contenedor | Dibuja el layout con nombres de áreas |
| `gap`, `row-gap`, `column-gap` | contenedor | Espacio entre pistas |
| `grid-auto-rows` / `-columns` | contenedor | Tamaño de las pistas que se crean solas |
| `grid-auto-flow` | contenedor | Cómo se acomodan los ítems sin posición |
| `grid-column` / `grid-row` | ítem | De qué línea a qué línea va |
| `grid-area` | ítem | Las cuatro líneas en una, o un nombre de `grid-template-areas` |

Galería que se reacomoda sola según el ancho, sin media queries:

```css
.galeria {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
}
```

## ¿Cuál uso?

**Flexbox** cuando acomodas cosas en una dirección y el contenido manda; **grid** cuando el layout manda en filas y columnas. Se combinan: grid para la página, flexbox dentro de cada pieza.

|  | Flexbox | Grid |
| --- | --- | --- |
| Dimensiones | 1: fila **o** columna | 2: filas **y** columnas |
| Quién manda | El contenido empuja el tamaño | El contenedor define las pistas |
| Uso típico | Navbar, botones, centrar, tarjetas en fila | Layout de página, galerías, dashboards |
| Superponer | No está pensado para eso | Sí, poniendo ítems en la misma celda |

## Trampas comunes

1. **`flex` no significa "misma anchura".** Es atajo de grow + shrink + basis. `flex: 1` (= `1 1 0%`) en todos da anchos iguales, salvo que un contenido no se pueda partir; `min-width: 0` en el ítem lo arregla.
2. **`flex-grow`, `flex-shrink` y `flex-basis` van en el ítem**, no en el contenedor.
3. **`flex-basis` es altura en `column`**, porque sigue al eje principal.
4. **Esperar que `display: flex` afecte a los nietos.** Solo los hijos directos son ítems.
5. **Contar celdas en vez de líneas en grid.** `span 2` ocupa dos pistas; `2 / 4` va de línea a línea.
6. **Nombres viejos:** `grid-gap`, `grid-row-gap` y `grid-column-gap` hoy son `gap`, `row-gap` y `column-gap`, y `gap` también funciona en flexbox.
