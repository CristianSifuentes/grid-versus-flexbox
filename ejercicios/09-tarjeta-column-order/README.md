# 09 · Tarjetas: flex en columna, pie abajo y `order`

**Tiempo:** ~25 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| `stretch` entre tarjetas | `.cards` | Las tarjetas de una fila tienen la misma altura |
| `flex-direction: column` | `.card` | Apila imagen, título, texto y pie sobre el eje vertical |
| `flex-basis` = altura en `column` | `.card img` | Tu trampa 3: la base sigue al eje principal |
| `margin-top: auto` | `.card-footer` | Empuja el pie hasta abajo y alinea los pies de todas las tarjetas |
| `space-between` contra `margin: auto` | `.card` | `space-between` reparte entre **todos**; `auto` actúa solo en un punto |
| `order` y accesibilidad | `.buy` | Cambia el orden visual, **no** el del Tab ni el del lector de pantalla |

## Checklist

- [ ] Paso 1 · Tarjetas en fila con la misma altura
- [ ] Paso 2 · Contenido de la tarjeta en columna
- [ ] Paso 3 · `flex-basis: 160px` en la imagen fija su **altura**
- [ ] Paso 4 · Los pies quedan alineados abajo
- [ ] Paso 5 · Comparo con `space-between` y entiendo por qué no conviene
- [ ] Paso 6 · Pruebo `order` con el teclado y lo deshago
- [ ] Reto · pie en fila: precio a la izquierda, botones a la derecha

## Paso a paso

### Paso 1 · Las tarjetas en fila

```css
.cards {
	display: flex;
	gap: 1rem;
}

.card {
	flex: 1;
	min-width: 0;
}
```

**Resultado:** tres tarjetas del mismo ancho y **la misma altura**, aunque la segunda tiene más texto. Es el `stretch` que viste en el ejercicio 04.

### Paso 2 · Cada tarjeta en columna

```css
.card {
	display: flex;
	flex-direction: column;
	gap: 0.5rem;
}
```

**Por qué:** la tarjeta es ítem de `.cards` y contenedor de su contenido a la vez. En `column`, el eje principal es vertical.

**Resultado:** a simple vista cambia poco, porque ya se apilaban como bloques, pero ahora `gap` las separa y tienes control sobre el eje vertical.

### Paso 3 · `flex-basis` es altura en columna

```css
.card img {
	flex: 0 0 160px;
}
```

**Por qué:** `flex-basis` mide **sobre el eje principal**. En `row` es ancho y en `column` es **alto**. Es tu trampa 3. Con `object-fit: cover` (ya en el punto de partida), la foto se recorta sin deformarse.

**Resultado:** las tres imágenes miden 160px de alto exactos.

### Paso 4 · Pie abajo

Mira los pies: el de la tarjeta 2 está más abajo que los otros porque su texto es más largo. Arréglalo:

```css
.card-footer {
	margin-top: auto;
}
```

**Por qué:** las tarjetas ya miden lo mismo (paso 1). El margen `auto` absorbe el espacio libre **encima** del pie y lo empuja hasta el fondo de cada tarjeta.

**Resultado:** **los tres pies quedan alineados** en la misma línea horizontal.

### Paso 5 · Compara con `space-between`

Comenta el `margin-top: auto` y prueba esto:

```css
.card {
	justify-content: space-between;
}
```

**Resultado:** el pie también baja, pero el título y el texto **se separan** de la imagen. `space-between` reparte el espacio libre entre **todos** los ítems.

**Conclusión:** para empujar **un** elemento, `margin: auto` es más preciso. El artículo de Lukman Faiz y el tutorial de StudyCode usaban `space-between` porque sus tarjetas tenían solo dos bloques. Borra este paso y vuelve a activar el `margin-top: auto`.

### Paso 6 · `order` y accesibilidad

Pon el botón "Comprar" visualmente **después** de "Detalles":

```css
.card-footer {
	display: flex;
	flex-direction: column;
	gap: 0.5rem;
}

.buy {
	order: 1; /* todos tienen order: 0; el mayor va al final */
}
```

Haz clic arriba de las tarjetas y pulsa **Tab** varias veces.

**Resultado:** visualmente se ve "Detalles" y luego "Comprar", pero el foco salta **Comprar 1 → Detalles 1**: va hacia arriba y luego hacia abajo.

**Por qué:** `order` (como `row-reverse`, `grid-area` o `grid-auto-flow: dense`) solo cambia la pintura. El teclado y los lectores de pantalla siguen el **HTML**.

**Regla:** si el orden importa, cámbialo en el HTML. Ahora **borra el `order: 1`**.

## Reto

Haz que `.card-footer` sea una **fila**: precio a la izquierda y los dos enlaces a la derecha, todos centrados en vertical. Reemplaza el `flex-direction: column` del paso 6.

<details>
<summary>Ver solución</summary>

```css
.card-footer {
	display: flex;
	flex-direction: row;
	flex-wrap: wrap;
	align-items: center;
	gap: 0.75rem;
}

.price {
	margin-right: auto;
}
```

Es flex anidado en tres niveles: `.cards` → `.card` → `.card-footer`. Cada nivel resuelve una sola dimensión. Por eso el artículo de Faiz lo llama **micro layout**.
</details>

## Para recordar

> Si la tarjeta está en `column`, `margin-top: auto` empuja el pie hacia abajo. `flex-basis` mide alto en columna. `order` no cambia el orden del Tab.
