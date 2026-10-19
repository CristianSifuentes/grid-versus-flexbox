# 02 · Eje principal, eje cruzado y `flex-direction`

**Tiempo:** ~15 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| Eje principal | `.axis` | Lo define `flex-direction`. `justify-content` reparte sobre él |
| Eje cruzado | `.axis` | Perpendicular al principal. `align-items` alinea sobre él |
| `flex-direction: column` | `.axis` | Los ejes **se voltean**: `justify-content` pasa a ser vertical |
| `row-reverse` | `.axis--reverse` | Cambia el orden **visual**, pero no el del HTML (accesibilidad) |
| Dirección de escritura | `.writing` | `row` sigue al idioma: en `rtl` empieza por la derecha |

## Checklist

- [ ] Paso 1 · `justify-content: center` centra en horizontal
- [ ] Paso 2 · `align-items: center` centra en vertical
- [ ] Paso 3 · Con `column`, las mismas dos propiedades cambian de dirección
- [ ] Paso 4 · `row-reverse`: navego con Tab y el foco va 1, 2, 3
- [ ] Paso 5 · `rtl` invierte el inicio del eje principal
- [ ] Reto · esquina inferior derecha en columna

## Paso a paso

### Paso 1 · Eje principal

```css
.axis {
	display: flex;
	justify-content: center;
}
```

**Por qué:** `justify-content` reparte el espacio libre **sobre el eje principal**. En `row`, el eje principal es horizontal.

**Resultado:** las tres cajas se juntan en el centro horizontal y siguen pegadas arriba.

### Paso 2 · Eje cruzado

```css
.axis {
	align-items: center;
}
```

**Por qué:** `align-items` alinea cada ítem en el **eje cruzado**, que en `row` es el vertical.

**Resultado:** las cajas quedan centradas en los dos ejes. Además dejan de estirarse: con `stretch` llenaban los 240px de alto.

### Paso 3 · Voltea los ejes

```css
.axis {
	flex-direction: column;
}
```

**Por qué:** ahora el eje principal es vertical.
- `justify-content: center` centra **en vertical**.
- `align-items: center` centra **en horizontal**.

No cambiaste ninguna de las dos propiedades; cambió el eje sobre el que trabajan.

**Resultado:** las cajas se apilan (1, 2, 3) y siguen centradas. Prueba cambiar `justify-content` a `flex-end`: las cajas bajan, no se van a la derecha.

> **Regla mental:** `justify` = eje principal, `align` = eje cruzado. Pregúntate siempre primero *"¿cuál es mi eje principal?"*

### Paso 4 · `row-reverse` y el orden del HTML

```css
.axis--reverse {
	display: flex;
	flex-direction: row-reverse;
	gap: 0.5rem;
}
```

**Por qué:** `row-reverse` mueve el **inicio** del eje principal al otro extremo. Los ítems se pintan 3, 2, 1, empezando por la derecha.

**Resultado y prueba de accesibilidad:** haz clic en el texto encima de la demo y pulsa **Tab** varias veces. El foco recorre **1, 2, 3**, el orden del HTML, que visualmente va de derecha a izquierda. A una persona que navega con teclado le parece que el foco salta "al revés".

> **Regla:** usa `-reverse` (y `order`) solo cuando el orden visual no cambia el sentido. El orden lógico se decide en el HTML.

### Paso 5 · Dirección de escritura

```css
.writing {
	display: flex;
	gap: 0.5rem;
}
```

**Por qué:** `row` no significa "de izquierda a derecha". Significa "en la dirección en que se escribe el texto". Por eso las propiedades usan `start` y `end`, no `left` y `right`.

**Resultado:** el contenedor `ltr` va 1, 2, 3 desde la izquierda y el `rtl` va 1, 2, 3 desde la derecha, con el mismo CSS.

## Reto

En `.corner`, apila A y B en **columna** y llévalas a la **esquina inferior derecha**.

<details>
<summary>Ver solución</summary>

```css
.corner {
	display: flex;
	flex-direction: column;
	justify-content: flex-end; /* principal = vertical → abajo */
	align-items: flex-end;     /* cruzado = horizontal → derecha */
	gap: 0.5rem;
}
```

Si intentaste `justify-content` para mandarlas a la derecha, caíste en el volteo de ejes: en `column`, la horizontal la controla `align-items`.
</details>

## Verifícalo en DevTools

Con la insignia `flex` activada, el inspector dibuja la dirección del eje principal con una flecha. Cambia `flex-direction` desde el panel *Styles* y mira cómo gira.

## Para recordar

> `justify-content` = eje principal; `align-items` = eje cruzado. `flex-direction` decide cuál es cuál, y en `column` se voltean.
