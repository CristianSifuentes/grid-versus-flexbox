# 01 · Contenedor e ítems flex

**Tiempo:** ~15 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| `display: flex` | `.row` | Convierte al elemento en **contenedor** y a sus hijos directos en **ítems** |
| Solo hijos directos | `.box--nested` | Los nietos no cambian: hay que crear otro contenedor flex |
| Valores por defecto | `.row` | `row`, `nowrap`, `align-items: stretch`, ítems con `flex: 0 1 auto` |
| `align-items: stretch` | `.row` | Por eso todas las cajas quedan con la **misma altura** |
| `inline-flex` | `.row--inline` | El contenedor se comporta como inline por fuera y como flex por dentro |

## Checklist

- [ ] Paso 1 · Observo el estado inicial (cajas apiladas)
- [ ] Paso 2 · `display: flex` en `.row`: las cajas quedan en fila y con la misma altura
- [ ] Paso 3 · Los chips no se movieron, así que hago flex a `.box--nested`
- [ ] Paso 4 · Escribo los valores por defecto y compruebo que nada cambia
- [ ] Paso 5 · `inline-flex` contra `flex`
- [ ] Reto · `.tags`
- [ ] Lo verifico en DevTools (insignia `flex`)

## Paso a paso

### Paso 1 · Observa el punto de partida

Abre la página. Las tres cajas se apilan y cada una ocupa todo el ancho. Esto pasa porque un `div` es `display: block`.

### Paso 2 · Crea el contenedor

```css
.row {
	display: flex;
}
```

**Por qué:** `display: flex` no cambia a `.row` en sí. Cambia **cómo se acomodan sus hijos directos**, que pasan a ser ítems flex.

**Resultado:**
- Las cajas quedan en una fila. El eje principal es horizontal porque `flex-direction` vale `row` por defecto.
- Cada caja mide lo que pide su contenido (`flex-basis: auto`) y la caja 2 se encoge para caber (`flex-shrink: 1`).
- **Las tres tienen la misma altura**, aunque la caja 1 tiene muy poco texto. Eso es `align-items: stretch`, el valor por defecto en el eje cruzado.

### Paso 3 · Los nietos no cambian

Los chips `a`, `b` y `c` siguen igual: son **nietos** de `.row`, no hijos directos. Si quieres acomodarlos, la caja 3 tiene que ser contenedor también:

```css
.box--nested {
	display: flex;
	align-items: center;
	gap: 0.5rem;
}
```

**Por qué:** un contenedor flex solo afecta a un nivel. Para acomodar algo dentro de un ítem, ese ítem también tiene que ser contenedor. Esto es el **flex anidado** y lo vas a usar en todas las navbars y tarjetas (ejercicios 08 y 09).

**Resultado:** el texto y los chips quedan en fila dentro de la caja 3, que sigue siendo ítem de `.row`. Un elemento puede ser ítem y contenedor a la vez.

### Paso 4 · Comprueba los valores por defecto

Agrega esto debajo de lo que ya escribiste:

```css
.row {
	flex-direction: row;
	flex-wrap: nowrap;
	justify-content: flex-start;
	align-items: stretch;
}

.row > .box {
	flex: 0 1 auto; /* grow 0 · shrink 1 · basis auto */
}
```

**Por qué:** si nada cambia en pantalla, demuestras que esos son los valores que flexbox ya traía.
- `flex: 0 1 auto` quiere decir que los ítems **no crecen** pero **sí se encogen**.
- Cuando más adelante algo "no ocupe todo el espacio", recuerda que `flex-grow` vale 0.

**Resultado:** nada se mueve. Puedes borrar este bloque cuando lo comprendas.

### Paso 5 · `inline-flex`

```css
.row--inline {
	display: inline-flex;
	gap: 0.5rem;
	vertical-align: middle;
}
```

**Por qué:** `flex` hace que el contenedor sea un bloque que ocupa toda la línea. `inline-flex` hace que fluya con el texto, como una palabra, mientras sus hijos siguen siendo ítems flex.

**Resultado:** las cajas A y B quedan **en medio del párrafo**, entre "Texto antes" y "texto después".

## Reto

Pon en fila las etiquetas de `.tags`, sin que el título `h3` quede en la misma fila.

<details>
<summary>Ver solución</summary>

```css
.tags-list {
	display: flex;
	gap: 0.5rem;
	list-style: none;
}
```

Si aplicas `display: flex` a `.tags`, el `h3` y el `ul` se ponen en fila (son sus hijos), pero los `li` no se mueven, porque son nietos. El contenedor correcto es el **padre directo** de lo que quieres mover.
</details>

## Verifícalo en DevTools

En Chrome o Firefox, abre el inspector y mira el panel *Elements*:
- Junto a `.row` aparece la insignia **`flex`**. Al hacer clic, se dibujan los ítems y el espacio libre.
- En *Computed* puedes ver `flex-grow: 0`, `flex-shrink: 1` y `flex-basis: auto` de cada `.box`.

## Para recordar

> `display: flex` actúa sobre los **hijos directos**. Por defecto los pone en **fila**, sin saltar de línea, **estirados** a la misma altura, sin crecer y encogiéndose si hace falta.
