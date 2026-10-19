# 14 · Galería: `auto-fill`, `auto-fit` y `minmax()`

**Tiempo:** ~30 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| `repeat(auto-fill, 200px)` | `.few-fill` | Crea tantas columnas de 200px **como quepan** |
| `minmax(200px, 1fr)` | `.few-fill` | Mínimo 200px; el sobrante se reparte con `fr` |
| `auto-fill` contra `auto-fit` | `.few-*` | `fill` **conserva** las columnas vacías; `fit` las **colapsa** y estira a los ítems |
| Responsive sin media queries | `.gallery` | El número de columnas cambia solo con el ancho |
| `aspect-ratio` + `object-fit` | `.gallery img` | Todas las fotos del mismo tamaño sin deformarse |
| `min(200px, 100%)` | `.tiny` | Evita el desborde cuando el contenedor es más angosto que el mínimo |
| `span` + `grid-auto-flow: dense` | reto | Foto destacada y relleno de huecos (con su costo en accesibilidad) |

## Checklist

- [ ] Paso 1 · `auto-fill` con 200px fijos deja espacio sobrante a la derecha
- [ ] Paso 2 · Con `minmax(200px, 1fr)` el sobrante se reparte, pero hay **columnas vacías**
- [ ] Paso 3 · `auto-fit` colapsa las vacías y los 2 ítems se estiran
- [ ] Paso 4 · Galería de 6 fotos que cambia de columnas al cambiar el ancho
- [ ] Paso 5 · Fotos uniformes con `aspect-ratio`
- [ ] Paso 6 · `.tiny` sin desborde gracias a `min()`
- [ ] Comparo con el reto del ejercicio 06 (flex wrap)
- [ ] Reto · primera foto al doble y `dense`

## Paso a paso

### Paso 1 · `auto-fill` con pistas fijas (MDN)

```css
.few-fill {
	display: grid;
	grid-template-columns: repeat(auto-fill, 200px);
	gap: 0.5rem;
}
```

**Por qué:** en vez de un número, `auto-fill` le dice al navegador que calcule cuántas columnas de 200px caben, gap incluido.

**Resultado:** los 2 ítems miden 200px y queda espacio vacío a la derecha, porque las columnas son rígidas.

### Paso 2 · Agrega `minmax()`

```css
.few-fill {
	grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
}
```

**Por qué** (MDN): el navegador calcula cuántas veces caben 200px y luego aplica el máximo `1fr` para repartir el resto entre **todas** las columnas creadas.

**Resultado:** los 2 ítems crecen un poco, pero **no llenan la fila**. Activa la insignia `grid` en DevTools: verás **columnas vacías** después del ítem 2. `auto-fill` las creó porque cabían, aunque no haya ítems para ellas.

### Paso 3 · `auto-fit`

```css
.few-fit {
	display: grid;
	grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
	gap: 0.5rem;
}
```

**Resultado:** compara las dos franjas. En `.few-fit`, los 2 ítems **se estiran hasta llenar todo el ancho**.

**Por qué:** `auto-fit` crea las mismas columnas que `auto-fill`, pero luego **colapsa a 0 las que quedan vacías**, y su espacio pasa a las columnas con ítems.

| | `auto-fill` | `auto-fit` |
| --- | --- | --- |
| Columnas vacías | Se conservan | Se colapsan |
| Pocos ítems | Mantienen su tamaño y queda hueco | Se estiran a todo el ancho |
| Muchos ítems | Igual | Igual |

**¿Cuál usar?** Si una galería con 2 fotos debe verse como "2 de muchas" (tamaño consistente), `auto-fill`. Si las 2 deben llenar la fila, `auto-fit`. El tutorial de StudyCode usaba `auto-fit` sin explicar la diferencia.

### Paso 4 · La galería

```css
.gallery {
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
	gap: 1rem;
}
```

**Resultado:** cambia el ancho de la ventana despacio. Las columnas pasan de 4 a 3, 2 y 1 **sin ninguna media query**, y siempre quedan **alineadas en filas y columnas**.

**Compara con el ejercicio 06:** con `flex-wrap`, los ítems de la última línea se estiraban sin alinearse. Aquí la última fila respeta las columnas. Esa es la "cantidad flexible de pistas flexibles" de MDN.

### Paso 5 · Imágenes uniformes

```css
.gallery img {
	width: 100%;
	aspect-ratio: 4 / 3;
	object-fit: cover;
}
```

**Por qué:**
- `aspect-ratio` fija la proporción sin fijar una altura en píxeles, así que funciona en cualquier ancho de columna.
- `object-fit: cover` recorta la foto en vez de deformarla. El tutorial usaba `height: 200px` fijo, que se ve mal en columnas muy anchas o muy angostas.

### Paso 6 · Contenedores muy angostos

```css
.tiny {
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
}
```

**Resultado:** los ítems **se desbordan** del contenedor de 160px, porque el mínimo de 200px es más grande que todo el espacio. Arréglalo:

```css
.tiny,
.gallery {
	grid-template-columns: repeat(auto-fill, minmax(min(200px, 100%), 1fr));
}
```

**Por qué:** `min(200px, 100%)` usa **200px o el 100% del contenedor, lo que sea menor**. En pantallas normales sigue siendo 200px; en un contenedor angosto (un móvil de 320px con padding) baja para no desbordar. Es la versión que te conviene escribir siempre.

## Reto

Haz que la **primera foto** de la galería ocupe **2 columnas y 2 filas**. Luego observa los huecos que quedan con ciertos anchos y prueba `grid-auto-flow: dense`.

<details>
<summary>Ver solución y explicación</summary>

```css
.gallery figure:first-child {
	grid-column: span 2;
	grid-row: span 2;
}

.gallery {
	grid-auto-flow: dense;
}
```

- `span 2` hace que la foto ocupe 2 pistas en cada eje, empiece donde empiece.
- Sin `dense`, el auto-placement nunca vuelve atrás: si un ítem no cabe, salta y deja un **hueco**.
- Con `dense`, los ítems pequeños posteriores **rellenan los huecos anteriores**.

**Cuidado:** `dense` cambia el orden visual respecto al HTML. Es el mismo problema de accesibilidad del ejercicio 13. En una galería de fotos suele ser aceptable; en una lista con orden lógico (pasos, ranking), no.
</details>

## Para recordar

> `repeat(auto-fill, minmax(min(200px, 100%), 1fr))` crea una galería responsive sin media queries. `auto-fit` colapsa las columnas vacías y estira los ítems; `auto-fill` las conserva.
