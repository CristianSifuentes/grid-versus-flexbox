# 06 · `flex-wrap`, `gap` y `align-content`

**Tiempo:** ~25 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| `flex-wrap: nowrap` (por defecto) | `.veggies` | Todo en una línea: si no cabe, se encoge o se desborda |
| `flex-wrap: wrap` | `.veggies` | Los ítems saltan a una nueva línea cuando no caben |
| `gap` contra `margin` | `.veggies` | `gap` pone espacio **solo entre** ítems, no en los bordes |
| `flex: 1 1 auto` | `.veggies li` | Cada línea se llena estirando sus ítems |
| Cada línea es independiente | `.veggies`, reto | Las líneas reparten su espacio por separado, así que **no hay columnas alineadas** |
| `align-content` | `.lines` | Reparte el espacio entre **líneas** (solo con `wrap`) |
| `flex-flow` | `.lines` | Atajo de `flex-direction` + `flex-wrap` |

## Checklist

- [ ] Paso 1 · Veo el desborde con `nowrap`
- [ ] Paso 2 · `wrap` resuelve el desborde
- [ ] Paso 3 · Cambio `margin` por `gap` y noto la diferencia en los bordes
- [ ] Paso 4 · `flex: 1 1 auto` llena las líneas
- [ ] Observo que las líneas no forman columnas
- [ ] Paso 5 · Distingo `align-content` de `align-items`
- [ ] Reto · cinco ítems de MDN y la conclusión: aquí convendría grid

## Paso a paso

### Paso 1 · Solo `display: flex`

```css
.veggies {
	display: flex;
}
```

**Resultado:** las diez verduras se meten en **una sola línea**. Se encogen hasta donde les permite la palabra más larga y luego **se salen** del recuadro punteado.

**Por qué:** `flex-wrap` vale `nowrap` por defecto, y `flex-shrink: 1` no puede encoger una palabra por debajo de su ancho mínimo (`min-width: auto`).

### Paso 2 · Permitir el salto de línea

```css
.veggies {
	flex-wrap: wrap;
}
```

**Resultado:** las verduras se reparten en varias filas dentro de los 450px. Este es el ejercicio "Flexbox 3" de MDN resuelto.

### Paso 3 · `gap` en vez de `margin`

Cambia el `margin: 0.5em` del punto de partida por `margin: 0` y agrega:

```css
.veggies {
	gap: 0.5em;
}
```

**Por qué:**
- `margin` también deja espacio contra los **bordes** del contenedor, y los márgenes de ítems vecinos se **suman** (0.5 + 0.5 = 1em entre ítems).
- `gap` pone exactamente 0.5em **entre** ítems y entre líneas, nada en los bordes.

Es el error #3 del artículo de Lukman Faiz.

**Resultado:** las verduras quedan pegadas al borde del contenedor y el espacio entre ellas es uniforme.

### Paso 4 · Llenar cada línea

```css
.veggies li {
	flex: 1 1 auto;
}
```

**Por qué:** `grow: 1` reparte el sobrante de **cada línea** entre los ítems de esa línea, y `basis: auto` parte del ancho del texto.

**Resultado:** cada fila llega de borde a borde. Ahora **observa con atención** que las cajas de una fila **no están alineadas** con las de la fila de arriba, porque cada fila tiene distintas palabras.

> **Concepto clave (MDN):** cuando los ítems hacen `wrap`, **cada línea se comporta como un contenedor flex nuevo**. Flexbox es unidimensional: controla la fila, no las columnas. Si necesitas alinear filas **y** columnas, necesitas grid.

### Paso 5 · `align-content`

```css
.lines {
	display: flex;
	flex-flow: row wrap; /* atajo de flex-direction + flex-wrap */
	gap: 0.5rem;
	align-content: flex-start;
}
```

Prueba también `center`, `space-between` y `stretch`.

**Por qué:** hay dos niveles de alineación vertical:
- `align-items` alinea **cada ítem dentro de su línea**.
- `align-content` alinea **las líneas dentro del contenedor**. Solo actúa cuando hay varias líneas (`wrap`) y sobra altura.

**Resultado:**
- `flex-start`: las dos filas suben.
- `space-between`: una fila queda arriba y la otra abajo.
- `stretch` (el valor por defecto): las filas se reparten la altura.

Quita `flex-wrap` y `align-content` deja de tener efecto.

## Reto

Copia el primer ejemplo de MDN ("Relación de Grid Layout con otros métodos") en `.mdn-wrapper`: flex con wrap y cada ítem `flex: 1 1 200px`. Haz la ventana más angosta y más ancha. ¿Qué pasa con la última fila?

<details>
<summary>Ver solución y explicación</summary>

```css
.mdn-wrapper {
	display: flex;
	flex-wrap: wrap;
	gap: 0.5rem;
}

.mdn-wrapper > .box {
	flex: 1 1 200px;
}
```

Con cierto ancho quedan 3 arriba y 2 abajo, y **los 2 de abajo se estiran** para llenar su línea. No quedan alineados bajo las columnas de arriba.

Si para arreglarlo le pones `max-width: 33%` a los ítems, estás **quitándole la flexibilidad** a flexbox. MDN dice que esa es la señal de que el problema pedía grid. Lo resuelves en el ejercicio 14 con `repeat(auto-fill, minmax(200px, 1fr))`.
</details>

## Para recordar

> `wrap` permite varias líneas, pero **cada línea es independiente**. Usa `gap` para espaciar. `align-content` mueve líneas y `align-items` mueve ítems.
