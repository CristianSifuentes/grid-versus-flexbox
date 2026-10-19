# 10 · Primer grid: columnas, filas, `fr`, `gap` y grid implícito

**Tiempo:** ~25 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| `display: grid` | `.grid` | Crea un contenedor de **dos dimensiones**; sin columnas definidas hay una sola |
| `grid-template-columns` | `.grid` | Define cuántas **pistas** verticales hay y cuánto mide cada una |
| Unidad `fr` | `.grid` | Una fracción del espacio **libre** (después de restar lo fijo y los gaps) |
| `repeat()` | `.grid` | Evita escribir `1fr 1fr 1fr` |
| `gap` | `.grid` | Espacio entre pistas, en filas y columnas |
| Grid explícito contra implícito | `.grid` | Las filas que no defines se crean solas, con tamaño `auto` |
| `grid-auto-rows` | `.grid` | Fija el tamaño de las filas implícitas |
| Alineación en 2D | reto | Grid mantiene filas **y** columnas; flex no (ejercicio 06) |

## Checklist

- [ ] Paso 1 · Con solo `display: grid` no cambia nada, y sé por qué
- [ ] Paso 2 · Tres columnas de 200px
- [ ] Paso 3 · Tres columnas con `1fr`
- [ ] Paso 4 · Calculo a mano cuánto mide cada columna de `200px 1fr 2fr`
- [ ] Paso 5 · `gap`
- [ ] Paso 6 · Distingo las filas explícitas de las implícitas
- [ ] Veo las líneas numeradas en DevTools (3 columnas = 4 líneas)
- [ ] Reto · comparo flex contra grid con los mismos 5 ítems

## Paso a paso

### Paso 1 · Solo `display: grid`

```css
.grid {
	display: grid;
}
```

**Resultado:** **nada cambia**. Las cajas siguen apiladas.

**Por qué:** sin `grid-template-columns`, grid tiene **una sola columna**, y cada ítem va a una fila nueva (implícita). A diferencia de flex, grid no hace nada visible hasta que defines pistas.

### Paso 2 · Columnas fijas

```css
.grid {
	grid-template-columns: 200px 200px 200px;
}
```

**Resultado:** tres columnas de 200px exactos, que dejan espacio vacío a la derecha si el contenedor es más ancho. Los 7 ítems se reparten en 3 filas (3 + 3 + 1).

**Por qué:** el **contenedor** define las pistas y los ítems se acomodan en ellas en orden (auto-placement). Esto es "layout primero": tú defines la estructura y los ítems entran en ella.

### Paso 3 · La unidad `fr`

```css
.grid {
	grid-template-columns: repeat(3, 1fr);
}
```

**Por qué:** `1fr` es "una fracción del espacio libre". Con tres `1fr`, cada columna recibe un tercio. `repeat(3, 1fr)` es lo mismo que `1fr 1fr 1fr`.

**Resultado:** tres columnas iguales que llenan el ancho y crecen o se encogen con la ventana.

### Paso 4 · Mezclar unidades

```css
.grid {
	grid-template-columns: 200px 1fr 2fr;
}
```

**La cuenta:** si el contenedor mide 800px (sin gap):
1. Primero se resta lo fijo: 800 − 200 = 600px libres.
2. Ese espacio libre se reparte en 3 partes (1 + 2): 200px por fracción.
3. Columnas: **200px · 200px · 400px**.

**Resultado:** la primera columna es rígida y las otras dos fluidas, en proporción 1:2. Deja escrito `repeat(3, 1fr)` antes de seguir.

### Paso 5 · `gap`

```css
.grid {
	gap: 1rem;
}
```

**Por qué:**
- `gap` crea canaletas entre pistas, no en los bordes, igual que en flex.
- `grid-gap` es el nombre viejo (lo usa la guía de MDN); hoy se escribe `gap`.
- Puedes separar los dos ejes: `row-gap` y `column-gap`.

**Resultado:** las cajas quedan separadas por 1rem en las dos direcciones.

### Paso 6 · Grid explícito e implícito

```css
.grid {
	grid-template-rows: 100px 100px;
}
```

**Resultado:** las filas 1 y 2 miden 100px. La fila 3, con la caja 7, mide **lo que pida su contenido**.

**Por qué:** solo definiste dos filas. Esas dos forman el **grid explícito**. La tercera la creó grid automáticamente (**grid implícito**) con tamaño `auto`. Para controlar las implícitas:

```css
.grid {
	grid-auto-rows: 100px;
}
```

**Resultado:** ahora la tercera fila también mide 100px. Un patrón muy común es `grid-auto-rows: minmax(100px, auto)`: mínimo 100px, pero crece si el contenido lo necesita.

### Verifícalo en DevTools

Haz clic en la insignia **`grid`** junto a `.grid` en el panel *Elements*. Activa *"Show line numbers"*.
- Verás **4 líneas verticales** (1 a 4) para 3 columnas. Grid numera **líneas**, no celdas.
- También aparecen números negativos (−1 a −4), que cuentan desde el final. Los usarás en el ejercicio 11.

## Reto

`.compare-flex` ya está resuelto con flex (`wrap` + `flex: 1 1 200px`). Resuelve `.compare-grid` con **grid de 3 columnas iguales** y compáralos.

<details>
<summary>Ver solución y explicación</summary>

```css
.compare-grid {
	display: grid;
	grid-template-columns: repeat(3, 1fr);
	gap: 0.5rem;
}
```

- **Flex:** los 2 de abajo se **estiran** para llenar su línea y no se alinean con las columnas de arriba, porque cada línea es independiente.
- **Grid:** los 2 de abajo **respetan las columnas** y queda un **hueco** al final. Grid alinea filas **y** columnas.

Esta es la frase de MDN: *"¿Necesito controlar el diseño únicamente por filas o por columnas? → flexbox. ¿Por filas y por columnas? → grid."*
</details>

## Para recordar

> En grid, el contenedor define las pistas y los ítems entran en ellas. `fr` reparte el espacio libre. Las filas que no defines se crean solas y se controlan con `grid-auto-rows`.
