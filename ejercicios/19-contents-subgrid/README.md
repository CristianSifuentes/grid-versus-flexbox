# 19 · Nietos en el grid: `display: contents` y `subgrid`

**Tiempo:** ~30 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| Solo hijos directos | `.wrapper` | Los `.nested` son nietos: no son ítems del grid |
| `display: contents` | `.box1` | La caja del elemento **desaparece** y sus hijos "suben" a ser ítems |
| Pierde la caja | `.box1` | Fondo, borde, padding y `grid-column` dejan de aplicarse |
| Problema de alineación | `.plans` | Cada tarjeta tiene sus propias filas, así que no se alinean entre sí |
| `grid-template-rows: subgrid` | `.plan` | La tarjeta **usa las filas del grid padre** y **conserva** su caja |
| `grid-template-columns: subgrid` | reto | Lo mismo en columnas |

> MDN (artículo de 2017) dice que subgrid "se implementará". **Hoy funciona en todos los navegadores principales** (Chrome, Edge, Firefox y Safari, desde 2023).

## Checklist

- [ ] Paso 1 · Grid de MDN: a, b y c son bloques normales dentro de box1
- [ ] Paso 2 · `display: contents`: a, b y c pasan a ser ítems y box1 pierde su fondo
- [ ] Paso 3 · Tarjetas desalineadas sin subgrid
- [ ] Paso 4 · Con `subgrid`, títulos, textos y botones se alinean entre tarjetas
- [ ] Sé cuándo usar cada uno
- [ ] Reto · formulario con columnas compartidas

## Paso a paso

### Paso 1 · El grid de MDN

```css
.wrapper {
	display: grid;
	grid-template-columns: repeat(3, 1fr);
	grid-auto-rows: minmax(100px, auto);
	gap: 0.5rem;
}

.box1 {
	grid-column: 1 / 4;
}
```

**Resultado:** box1 ocupa toda la primera fila y **a, b, c se apilan** dentro de ella como bloques normales. No son ítems del grid porque son **nietos** (tu trampa 4, ahora en grid).

### Paso 2 · `display: contents`

```css
.box1 {
	display: contents;
}
```

**Resultado:**
- a, b y c se reparten en las celdas como **ítems del grid**, cada uno en una columna.
- box1 **pierde su fondo**, porque su caja ya no existe.
- `grid-column: 1 / 4` deja de tener efecto, porque ya no hay caja que colocar.

**Por qué:** según la especificación, *"el elemento no genera ninguna caja, pero sus hijos sí"*. Es como si en el HTML box1 se reemplazara por sus hijos.

**Cuidados:**
- Pierdes fondo, borde, padding y todo lo de la caja.
- Durante años, algunos navegadores **eliminaron la semántica** de elementos como `<ul>`, `<button>` o `<table>` con `display: contents`, y los lectores de pantalla dejaron de anunciarlos. Úsalo en `div` o `span` genéricos.

### Paso 3 · El problema real: tarjetas desalineadas

```css
.plans {
	display: grid;
	grid-template-columns: repeat(auto-fit, minmax(min(220px, 100%), 1fr));
	gap: 1rem;
}

.plan {
	display: grid;
	gap: 0.5rem;
	align-content: start;
}
```

**Resultado:** tres tarjetas de igual altura, pero el texto y el botón de cada una quedan a **distintas alturas**, porque el título "Premium" ocupa dos líneas.

**Por qué:** cada `.plan` tiene **su propio** grid interno, y sus filas no saben nada de las filas de las tarjetas vecinas.

### Paso 4 · `subgrid`

Reemplaza la regla de `.plan` del paso 3 por esta:

```css
.plan {
	display: grid;
	grid-row: span 3;               /* la tarjeta abarca 3 filas del padre */
	grid-template-rows: subgrid;    /* y usa ESAS filas para sus hijos */
	gap: 0.5rem;
}
```

**Resultado:** **todos los títulos**, **todos los textos** y **todos los botones** quedan alineados en la misma línea horizontal entre tarjetas. La fila del título mide lo que necesita el título más largo.

**Por qué:**
- Cada tarjeta ocupa 3 filas del grid `.plans`.
- Con `subgrid`, sus 3 hijos (h3, p, a) se colocan **en esas filas compartidas**. Las filas se dimensionan teniendo en cuenta el contenido de **todas** las tarjetas.
- A diferencia de `display: contents`, **la tarjeta conserva su caja**: fondo, padding y borde redondeado.

| | `display: contents` | `subgrid` |
| --- | --- | --- |
| La caja del padre | Desaparece | Se conserva |
| Fondo, borde, padding | Se pierden | Se mantienen |
| Semántica (lectores de pantalla) | Riesgo en algunos elementos | Sin problema |
| Uso típico | Quitar un `div` envoltorio sin estilo | Alinear contenido entre tarjetas o filas |

## Reto

En `.form`, crea un grid de **2 columnas** (`max-content 1fr`). Cada `.field` debe ocupar las dos columnas y usar `subgrid` para que **todas las etiquetas** queden en la primera columna y **todos los campos** empiecen en la misma línea vertical.

<details>
<summary>Ver solución</summary>

```css
.form {
	display: grid;
	grid-template-columns: max-content 1fr;
	gap: 0.75rem 1rem;
}

.field {
	grid-column: 1 / -1;
	display: grid;
	grid-template-columns: subgrid;
	align-items: center;
}
```

La primera columna mide lo que necesita **"Correo electrónico"**, la etiqueta más larga, y los tres campos arrancan alineados. Sin subgrid, cada `.field` calcularía su propia columna y los inputs quedarían escalonados.
</details>

## Para recordar

> Para que los nietos usen el grid del abuelo, prefiere `subgrid`: conserva la caja y alinea entre tarjetas. `display: contents` borra la caja (y en algunos elementos su semántica).
