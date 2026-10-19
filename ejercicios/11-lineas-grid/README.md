# 11 · Colocar ítems por líneas

**Tiempo:** ~30 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| Grid numera **líneas** | `.board` | 4 columnas = 5 líneas verticales (1 a 5) |
| `grid-column: inicio / fin` | `.a` | De qué línea a qué línea va el ítem |
| Líneas negativas | `.b` | `-1` es siempre la última línea del grid explícito |
| `grid-row` | `.c` | Lo mismo en el eje de las filas |
| `span n` | `.d` | "Ocupa n pistas" desde donde le toque |
| `grid-area: f1 / c1 / f2 / c2` | `.e` | Las cuatro líneas en una sola propiedad |
| Auto-placement | `.f` | Los ítems sin posición rellenan los huecos libres en orden |

## Checklist

- [ ] Paso 1 · Tablero de 4 columnas con DevTools mostrando las líneas
- [ ] Paso 2 · `1 / 3` ocupa **2** columnas, y sé por qué
- [ ] Paso 3 · `-1` llega al final sin contar
- [ ] Paso 4 · `grid-row: 2 / 4`
- [ ] Paso 5 · `span 2` contra `2 / 4`
- [ ] Paso 6 · `grid-area` con el orden correcto (fila, columna, fila, columna)
- [ ] Observo cómo `.f` encuentra su lugar sola
- [ ] Reto · revista

## Paso a paso

### Paso 1 · El tablero

```css
.board {
	display: grid;
	grid-template-columns: repeat(4, 1fr);
	grid-auto-rows: 80px;
	gap: 0.5rem;
}
```

**Resultado:** a–d en la primera fila y e–f en la segunda. Activa la insignia `grid` en DevTools con **"Show line numbers"**. Verás las líneas verticales numeradas del 1 al 5.

### Paso 2 · De línea a línea

```css
.a {
	grid-column: 1 / 3;
}
```

**Resultado:** `a` ocupa **2 columnas** (la 1 y la 2).

**Por qué:** `1 / 3` no significa "columnas 1 a 3". Significa "de la **línea** 1 a la **línea** 3", y entre esas dos líneas hay dos pistas. Es la trampa 5 de tus apuntes: contar celdas en vez de líneas.

### Paso 3 · Líneas negativas

```css
.b {
	grid-column: 3 / -1;
}
```

**Resultado:** `b` va de la línea 3 hasta el final.

**Por qué:** `-1` es siempre la **última línea** del grid explícito. Si mañana cambias a 6 columnas, `b` sigue llegando al borde sin que recalcules nada. `1 / -1` es "todo el ancho".

### Paso 4 · Filas

```css
.c {
	grid-row: 2 / 4;
}
```

**Resultado:** `c` ocupa **dos filas** de alto. Como no le diste columna, el auto-placement la puso en el primer hueco libre.

### Paso 5 · `span`

```css
.d {
	grid-column: span 2;
}
```

**Por qué:** `span 2` no dice **dónde** empieza, solo **cuántas pistas** ocupa. El auto-placement busca el primer lugar donde quepan 2 columnas seguidas.

| Escritura | Significado |
| --- | --- |
| `2 / 4` | Empieza en la línea 2 y termina en la 4: posición fija |
| `span 2` | Ocupa 2 pistas donde caiga |
| `2 / span 2` | Empieza en la línea 2 y ocupa 2 pistas (= `2 / 4`) |

### Paso 6 · `grid-area` con cuatro líneas

```css
.e {
	grid-area: 3 / 2 / 4 / 5;
	/* fila-inicio / col-inicio / fila-fin / col-fin */
}
```

**Por qué:** el orden es **primero los dos inicios y luego los dos finales**, fila antes que columna. No es "arriba, derecha, abajo, izquierda" como `margin`. Regla mental: *"fila, columna, fila, columna"*.

**Resultado:** `e` ocupa la fila 3 de la columna 2 a la 4.

**Mira `.f`:** no le diste posición. El auto-placement la coloca en el primer hueco libre recorriendo fila por fila. Así funcionan los ítems "sin dueño" en grid.

## Reto

Reproduce el plano de `.magazine` con **4 columnas × 3 filas de 100px**, usando solo colocación por líneas.

<details>
<summary>Ver solución</summary>

```css
.magazine {
	display: grid;
	grid-template-columns: repeat(4, 1fr);
	grid-template-rows: repeat(3, 100px);
	gap: 0.5rem;
}

.hero  { grid-area: 1 / 1 / 3 / 3; }
.side1 { grid-area: 1 / 3 / 2 / 4; }
.side2 { grid-area: 1 / 4 / 2 / 5; }
.wide  { grid-column: 3 / -1; grid-row: 2; }
.s1    { grid-area: 3 / 1 / 4 / 2; }
.s2    { grid-area: 3 / 2 / 4 / 3; }
.s3    { grid-column: 3 / -1; grid-row: 3; }
```

`grid-row: 2` sin `/` equivale a `2 / 3` (ocupa una pista). Con tantos nombres, ¿no sería más fácil **dibujar** el plano directamente en el CSS? Eso hace `grid-template-areas`, que ves en el ejercicio 12.
</details>

## Para recordar

> Cuenta **líneas**, no celdas: `1 / 3` ocupa 2 pistas, `-1` es el final y `span n` dice cuántas pistas sin decir dónde. `grid-area` sigue el orden fila, columna, fila, columna.
