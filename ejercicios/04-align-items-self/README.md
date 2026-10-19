# 04 · `align-items` y `align-self`: el eje cruzado

**Tiempo:** ~20 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| `align-items: stretch` | franja 1 | Valor por defecto: los ítems llenan el eje cruzado (columnas de igual altura) |
| `flex-start` / `center` / `flex-end` | franjas 2 a 4 | Cada ítem se alinea al inicio, al centro o al final del eje cruzado |
| `baseline` | franja 5 | Alinea la **primera línea de texto** de cada ítem, sin importar el tamaño de letra |
| `align-self` | `.box1`, `.box2` | Sobrescribe `align-items` para **un solo** ítem |
| Box Alignment | todo | Estas propiedades vienen de una especificación compartida con grid (ejercicio 16) |

## Checklist

- [ ] Paso 1 · Las franjas son contenedores flex
- [ ] Paso 2 · Comparo los cinco valores
- [ ] Explico por qué `baseline` se ve "desordenado" pero alinea el texto
- [ ] Paso 3 · Compruebo que `stretch` no actúa si el ítem tiene altura fija
- [ ] Paso 4 · Ejemplo de MDN: `align-self` sobrescribe al contenedor
- [ ] Reto · columnas de igual altura y una centrada

## Paso a paso

### Paso 1 · Las franjas

```css
.cross {
	display: flex;
	gap: 0.5rem;
}
```

**Resultado:** las cinco franjas se ven igual, con las cajas estiradas a 140px. Ese es el `stretch` por defecto.

### Paso 2 · Un valor por franja

```css
.cross--start    { align-items: flex-start; }
.cross--center   { align-items: center; }
.cross--end      { align-items: flex-end; }
.cross--baseline { align-items: baseline; }
```

**Por qué:** el eje cruzado de una fila es vertical, así que `align-items` decide dónde se coloca cada ítem en la altura disponible.

**Resultado:**
- `stretch` estira todas las cajas a lo alto de la franja.
- `flex-start`, `center` y `flex-end` mantienen cada caja **a su tamaño natural**, arriba, al centro o abajo.
- `baseline` parece desordenado, pero mira el texto: los "1", "2" y "3" descansan sobre **la misma línea imaginaria**, aunque el "2" es más grande. Úsalo para alinear un título grande con un texto chico en la misma fila.

### Paso 3 · La trampa de `stretch`

```css
.cross--stretch .box:first-child {
	height: 50px;
}
```

**Por qué:** `stretch` solo estira ítems con tamaño `auto` en el eje cruzado. Si les fijas `height`, ganas tú.

**Resultado:** en la primera franja, la caja 1 queda en 50px y las otras siguen estiradas. Si alguna vez "las columnas no quedan de la misma altura", busca un `height` fijo.

### Paso 4 · `align-self` (ejemplo de MDN)

```css
.mdn-wrapper {
	display: flex;
	align-items: flex-end;
	gap: 0.5rem;
}

.box1 {
	align-self: stretch;
}

.box2 {
	align-self: flex-start;
}
```

**Por qué:** `align-items` en el contenedor fija el valor para **todos**, y `align-self` en un ítem lo sobrescribe solo para él.

**Resultado:** Three queda abajo (`flex-end`), One ocupa toda la altura y Two queda arriba.

> En el ejercicio 16 harás lo mismo con grid. Allí se escribe `start` y `end`, sin `flex-`, porque en grid cada ítem se alinea **dentro de su celda**.

## Reto

En `.columns`, las tres columnas deben tener **la misma altura**, aunque su texto sea distinto. La tercera debe quedar **centrada en vertical** y no estirarse.

<details>
<summary>Ver solución</summary>

```css
.columns {
	display: flex;
	gap: 1rem;
}

.col-centered {
	align-self: center;
}
```

Para la misma altura no escribiste nada: `stretch` ya viene por defecto. Antes de flexbox esto requería hacks (tablas, `padding-bottom` gigante). La tercera columna se sale con `align-self`.
</details>

## Para recordar

> `align-items` alinea a todos en el eje cruzado y `align-self` a uno solo. `stretch` es el valor por defecto y no actúa si el ítem tiene altura fija.
