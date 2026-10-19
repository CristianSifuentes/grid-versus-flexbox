# 13 · Responsive mobile-first con áreas (y el orden accesible)

**Tiempo:** ~30 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| Mobile first | `.site` | Los estilos base son para móvil y `min-width` **agrega** columnas al crecer |
| Redibujar áreas | media queries | El HTML y los `grid-area` no cambian, solo el mapa |
| Filas en el media query | `.buggy` | Si cambias el número de filas del mapa, **revisa `grid-template-rows`** |
| Orden visual contra el del DOM | `.site` | Grid mueve la pintura, no el orden del Tab ni el del lector de pantalla |

## Checklist

- [ ] Paso 1 · Base de una columna sin media query
- [ ] Paso 2 · Tablet con 2 columnas
- [ ] Paso 3 · Escritorio con 3 columnas y `auto 1fr auto`
- [ ] Paso 4 · Reproduzco el bug de StudyCode y lo corrijo
- [ ] Paso 5 · Reordeno con áreas y compruebo con Tab que el orden no cambia
- [ ] Puedo explicar la regla de accesibilidad

## Paso a paso

### Paso 1 · Móvil primero

```css
.site {
	display: grid;
	gap: 1rem;
	grid-template-columns: 1fr;
	grid-template-areas:
		"header"
		"nav"
		"main"
		"aside"
		"footer";
}

.site-header { grid-area: header; }
.site-nav    { grid-area: nav; }
.site-main   { grid-area: main; }
.site-aside  { grid-area: aside; }
.site-footer { grid-area: footer; }
```

**Por qué:** en móvil todo va en una columna, que es el caso más simple. Lo escribes sin media query, y cada pantalla más grande **agrega** complejidad con `min-width`. Así el CSS para móvil queda corto y sin sobrescrituras. Es el *mobile first* del artículo de Lukman Faiz.

**Resultado:** cinco bloques apilados en cualquier ancho (todavía).

### Paso 2 · Tablet

```css
@media (min-width: 640px) {
	.site {
		grid-template-columns: 200px 1fr;
		grid-template-areas:
			"header header"
			"nav    main"
			"aside  aside"
			"footer footer";
	}
}
```

**Resultado:** con más de 640px aparece la nav a la izquierda del main y la sidebar baja a ocupar todo el ancho.

### Paso 3 · Escritorio

```css
@media (min-width: 1024px) {
	.site {
		grid-template-columns: 200px 1fr 250px;
		grid-template-rows: auto 1fr auto;
		grid-template-areas:
			"header header header"
			"nav    main   aside"
			"footer footer footer";
	}
}
```

**Por qué:** `grid-template-rows` solo aparece aquí, donde el mapa tiene exactamente 3 filas. En móvil y tablet las filas quedan en `auto`, ajustadas a su contenido.

**Resultado:** el layout de tres columnas del ejercicio 12. Achica la ventana y verás las tres versiones.

### Paso 4 · El bug del tutorial

Haz la ventana **menor de 768px** y mira `.buggy`:
- La **nav** es enorme y se lleva casi toda la altura.
- El header mide 80px y la nav se estira, pero el main, la sidebar y el footer quedan con altura mínima.

**Por qué:** el media query cambió las columnas y el mapa (ahora 5 filas), pero **dejó `grid-template-rows: 80px 1fr 60px`**. Esas tres medidas ahora caen en header (80px), **nav (`1fr`)** y main (60px). Main, sidebar y footer pasan a ser filas implícitas `auto`.

**Arréglalo** en el hueco del paso 4:

```css
@media (max-width: 768px) {
	.buggy {
		grid-template-rows: auto;
	}
}
```

Otra opción, mejor, es lo que hiciste en `.site`: definir las filas solo en el breakpoint donde el mapa tiene 3 filas.

> **Regla:** cada vez que un media query cambia el número de filas o columnas del mapa, revisa también `grid-template-rows` y `-columns`.

### Paso 5 · Reordenar y accesibilidad

En móvil, pon la sidebar **antes** del main **editando el mapa del paso 1** (no lo escribas al final del archivo: una regla posterior sin media query también ganaría en tablet y escritorio, porque los media queries no suman especificidad):

```css
.site {
	grid-template-areas:
		"header"
		"nav"
		"aside"
		"main"
		"footer";
}
```

Haz la ventana angosta, haz clic arriba de la demo y recorre los enlaces con **Tab**.

**Resultado:** visualmente ves `4 · Sidebar` antes de `3 · Main`, pero el foco va **1 → 2 → 3 → 4 → 5**: salta hacia abajo al main y luego **vuelve hacia arriba** a la sidebar.

**Por qué:** el artículo de Faiz decía que grid permite reordenar "sin confundir a los lectores de pantalla". **Es falso:** MDN (*"CSS Grid Layout y Accesibilidad"*) explica que grid, `order` y `row-reverse` cambian **solo** la vista. El teclado y los lectores siguen el HTML.

**Regla:** reordenar con grid está bien cuando el orden no cambia el sentido (una sidebar decorativa, una imagen). Si el usuario debe leer o navegar algo en otro orden, **cambia el HTML**. Devuelve el mapa del paso 1 a su orden original.

## Para recordar

> Mobile first: la base es una columna y `min-width` agrega columnas. Si cambias el mapa, revisa también las filas. Reordenar con áreas cambia lo que se ve, no el orden del Tab.
