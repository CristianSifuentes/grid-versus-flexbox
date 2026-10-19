# 12 · `grid-template-areas`: layout de página

**Tiempo:** ~25 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| Grid para el **macro layout** | `.page` | La página tiene filas **y** columnas: un problema de 2D |
| `grid-template-areas` | `.page` | Dibujas el layout con nombres: el código se documenta solo |
| `grid-area: nombre` | cada hijo | Envía un elemento a un área con nombre (no a líneas) |
| Filas `auto 1fr auto` | `.page` | Header y footer miden su contenido y main se lleva el resto |
| `.` celda vacía | `.page` | Deja un hueco a propósito |
| Áreas rectangulares | `.page` | Una forma en L invalida **toda** la declaración |

## Checklist

- [ ] Paso 1 · `200px 1fr 250px` y `auto 1fr auto`
- [ ] Paso 2 · Mapa de 3 filas × 3 columnas
- [ ] Paso 3 · Cada elemento con su `grid-area`
- [ ] Paso 4 · Hueco con `.`
- [ ] Paso 5 · Rompo el rectángulo, veo que todo se cae y lo arreglo
- [ ] Sé por qué `auto` es mejor que `80px` fijo para el header
- [ ] Reto · nav de altura completa

## Paso a paso

### Paso 1 · Pistas

```css
.page {
	display: grid;
	grid-template-columns: 200px 1fr 250px;
	grid-template-rows: auto 1fr auto;
	gap: 1rem;
}
```

**Por qué:**
- **Columnas:** la nav (200px) y la sidebar (250px) son fijas y el contenido es fluido. Es la combinación de `fr` y píxeles de los trucos de StudyCode.
- **Filas:** `auto` para header y footer (miden su contenido) y `1fr` para el centro, que se lleva toda la altura sobrante.

StudyCode usaba `80px 1fr 60px`. El problema de las alturas fijas es que, si el header crece (un logo más alto, texto en dos líneas), el contenido **se desborda**. `auto` se adapta.

**Resultado:** los 5 elementos se reparten en orden por las celdas (auto-placement), todavía sin sentido.

### Paso 2 · Dibuja el mapa

```css
.page {
	grid-template-areas:
		"header header header"
		"nav    main   aside"
		"footer footer footer";
}
```

**Por qué:**
- Cada **cadena** es una fila y cada **palabra** una columna.
- Repetir un nombre lo extiende por varias celdas: `header` ocupa las tres columnas.
- Los espacios extra son solo para alinear visualmente; no importan.

**Resultado:** todavía nada cambia. Nombraste áreas, pero ningún elemento sabe a cuál ir.

### Paso 3 · Asigna cada elemento

```css
.page-header { grid-area: header; }
.page-nav    { grid-area: nav; }
.page-main   { grid-area: main; }
.page-aside  { grid-area: aside; }
.page-footer { grid-area: footer; }
```

**Por qué:** aquí `grid-area` recibe un **nombre**, no cuatro números de línea como en el ejercicio 11. Es la misma propiedad con otra sintaxis.

**Resultado:** el layout clásico: header arriba a todo el ancho, tres columnas en medio y footer abajo. El `main` se estira hasta llenar los 480px.

> Compara con el reto del ejercicio 11: sin nombres tuviste que calcular líneas para cada ítem. Aquí el CSS **es** el dibujo.

### Paso 4 · Celdas vacías

Cambia la última fila del mapa:

```css
.page {
	grid-template-areas:
		"header header header"
		"nav    main   aside"
		".      footer footer";
}
```

**Resultado:** debajo de la nav queda un **hueco**. El punto `.` marca una celda sin nombre. Vuelve a `"footer footer footer"` antes de seguir.

### Paso 5 · La regla del rectángulo

Esta vez **edita el mapa del paso 2**, no escribas uno nuevo debajo. Si agregas una declaración inválida aparte, el navegador la descarta y sigue usando la anterior, así que no verías nada. Conviértelo en esta forma en "L":

```css
.page {
	grid-template-areas:
		"header header header"
		"header main   aside"
		"footer footer footer";
}
```

**Resultado:** el layout **se rompe por completo**. DevTools tacha la propiedad con un aviso de valor inválido, y los `grid-area: header`, `main`, etc. apuntan a áreas que ya no existen, así que los ítems acaban en pistas implícitas raras.

**Por qué:** cada área debe formar un **rectángulo**. Si una sola no lo es, el navegador descarta **toda** la declaración. Las cadenas también deben tener el mismo número de columnas. Vuelve al mapa del paso 2.

## Reto

Construye `.grail`: la **nav ocupa toda la altura** a la izquierda (220px), y el header y el footer cubren solo el resto. Usa `auto 1fr auto` en las filas.

<details>
<summary>Ver solución</summary>

```css
.grail {
	display: grid;
	grid-template-columns: 220px 1fr 200px;
	grid-template-rows: auto 1fr auto;
	grid-template-areas:
		"nav header header"
		"nav main   aside"
		"nav footer footer";
	gap: 1rem;
}

.g-header { grid-area: header; }
.g-nav    { grid-area: nav; }
.g-main   { grid-area: main; }
.g-aside  { grid-area: aside; }
.g-footer { grid-area: footer; }
```

Solo cambió el **dibujo**. El HTML y las asignaciones `grid-area` son idénticos. Esa es la ventaja de las áreas con nombre: rediseñar es redibujar. En el ejercicio 13 lo aprovechas con media queries.
</details>

## Para recordar

> `grid-template-areas` es el plano de la página dibujado en el CSS. Cada área debe ser un rectángulo, `.` deja un hueco y las filas `auto 1fr auto` son mejores que alturas fijas.
