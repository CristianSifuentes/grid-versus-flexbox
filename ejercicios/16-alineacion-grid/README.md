# 16 · Box Alignment en grid

**Tiempo:** ~30 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| Box Alignment | todo | Las propiedades de alineación salieron de flexbox y hoy son una especificación compartida |
| `justify-items` | `.cells` | Alinea cada ítem **dentro de su celda** en el eje en línea (horizontal) |
| `align-items` | `.cells` | Lo mismo en el eje de bloque (vertical) |
| `place-items` | `.cells` | Atajo: `align-items` `justify-items` |
| `justify-self` / `align-self` | un ítem | Sobrescribe para un solo ítem |
| `start` / `end` | `.mdn-grid` | En grid no se escribe `flex-start` / `flex-end` |
| `justify-content` / `align-content` | `.tracks` | Mueven **las pistas completas** cuando sobra espacio en el contenedor |

> **Diferencia con flexbox:** en flex no existe `justify-items`, porque el eje principal se reparte entre todos los ítems con `justify-content`. En grid cada ítem tiene **su propia celda**, así que puede alinearse en los dos ejes.

## Checklist

- [ ] Paso 1 · Rejilla de 3 × 120px
- [ ] Paso 2 · `justify-items` mueve los ítems en horizontal dentro de sus celdas
- [ ] Paso 3 · `align-items` en vertical
- [ ] Paso 4 · `place-items` y un ítem rebelde con `-self`
- [ ] Paso 5 · Ejemplo de MDN con `start` / `end`
- [ ] Paso 6 · Distingo **-items** (ítems en celdas) de **-content** (pistas en el contenedor)
- [ ] Reto · etiqueta en la esquina

## Paso a paso

### Paso 1 · La rejilla

```css
.cells {
	display: grid;
	grid-template-columns: repeat(3, 1fr);
	grid-auto-rows: 120px;
	gap: 0.5rem;
}
```

**Resultado:** las cajas **llenan** toda su celda. `stretch` es el valor por defecto en los dos ejes.

### Paso 2 · `justify-items`

```css
.cells {
	justify-items: start;
}
```

Prueba también `center`, `end` y `stretch`.

**Resultado:** las cajas se encogen a su contenido y se colocan a la izquierda, al centro o a la derecha **de su propia celda**. Con DevTools (insignia `grid`) ves que las celdas siguen midiendo lo mismo; solo se movió la caja dentro de cada una.

### Paso 3 · `align-items`

```css
.cells {
	align-items: end;
}
```

**Resultado:** las cajas bajan al fondo de cada celda de 120px.

### Paso 4 · `place-items` y `-self`

Reemplaza los pasos 2 y 3 por el atajo:

```css
.cells {
	place-items: center; /* = align-items: center; justify-items: center; */
}

.cells .box:nth-child(5) {
	justify-self: end;
	align-self: start;
}
```

**Por qué:**
- `place-items: center` es lo que usaste para centrar en el ejercicio 05.
- `-self` sobrescribe el valor para un ítem, igual que `align-self` en flex.

**Resultado:** todas centradas, menos la 5, que queda en la esquina superior derecha de su celda.

### Paso 5 · Ejemplo de MDN

```css
.mdn-grid {
	display: grid;
	grid-template-columns: repeat(3, 1fr);
	grid-auto-rows: 200px;
	align-items: end;
}

.mdn-grid .box1 {
	align-self: stretch;
}

.mdn-grid .box2 {
	align-self: start;
}
```

**Por qué:** es el mismo resultado que el ejercicio 04 con flex, pero aquí se escribe `start` / `end`, no `flex-start` / `flex-end`. En grid, MDN dice que *"alineamos los ítems dentro de su grid area"*, que puede ser una celda o varias.

### Paso 6 · Mover las pistas: `-content`

```css
.tracks {
	display: grid;
	grid-template-columns: repeat(3, 100px);
	grid-template-rows: repeat(2, 100px);
	gap: 0.5rem;
	justify-content: space-between;
	align-content: center;
}
```

Prueba también `justify-content: center`, `end` y `space-evenly`.

**Por qué:** las pistas suman 300px de ancho y el contenedor es mucho más ancho, así que **sobra espacio fuera de las pistas**.
- `justify-content` y `align-content` mueven la **rejilla completa** (o reparten espacio entre pistas).
- `-items` y `-self` mueven ítems **dentro** de sus celdas.

**Resultado:** las tres columnas se separan a lo ancho y las dos filas quedan centradas en los 260px.

| Sufijo | Mueve | Dentro de |
| --- | --- | --- |
| `-items` / `-self` | Los ítems | Su celda o área |
| `-content` | Las pistas | El contenedor (solo si sobra espacio) |

> Con pistas `fr` no sobra espacio, porque `fr` lo consume todo, así que `justify-content` no hace nada. Por eso aquí usamos pistas fijas.

## Reto

Convierte `.products` en una rejilla responsive de tarjetas (técnica del ejercicio 14). Dentro de cada `.product`, la etiqueta `.tag` debe quedar en la **esquina superior derecha de la foto**, sin `position: absolute`.

<details>
<summary>Ver solución</summary>

```css
.products {
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(min(200px, 100%), 1fr));
	gap: 1rem;
}

.product {
	display: grid;
}

.product > * {
	grid-area: 1 / 1; /* imagen y etiqueta en la misma celda */
}

.tag {
	justify-self: end;
	align-self: start;
}
```

Imagen y etiqueta comparten la celda `1 / 1`, y la etiqueta se alinea dentro de esa celda con `-self`. Esa es la técnica de **superposición** que profundizas en el ejercicio 17.
</details>

## Para recordar

> `-items` / `-self` mueven ítems dentro de su celda; `-content` mueve las pistas dentro del contenedor. En grid se escribe `start` y `end`, no `flex-start` y `flex-end`.
