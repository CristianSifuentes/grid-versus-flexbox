# 08 · Navbar real: flex anidado

**Tiempo:** ~25 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| Flex para 1D | `.navbar` | Una navbar es una fila: el caso clásico de flexbox |
| `justify-content: space-between` | `.navbar` | Logo, enlaces y botón repartidos a lo ancho |
| `align-items: center` | `.navbar` | Elementos de distinta altura centrados en vertical |
| Flex anidado | `.nav-links` | Los enlaces son **nietos** de `.navbar`: necesitan su propio contenedor |
| `margin-left: auto` | `.nav-links` | Alternativa a `space-between` para agrupar a la derecha |
| `flex-wrap` + `gap` | `.navbar` | Adaptación natural a pantallas angostas, sin media query |
| `inline-flex` | reto | Icono + texto dentro de un botón |

## Checklist

- [ ] Paso 1 · `.navbar` flex con `space-between` y `center`
- [ ] Paso 2 · `.nav-links` flex con `gap` (porque solo afecta a los hijos directos)
- [ ] Paso 3 · Pruebo la variante `margin-left: auto`
- [ ] Paso 4 · En 360px la navbar hace wrap y no desborda
- [ ] Reto · icono alineado con `inline-flex`

## Paso a paso

### Paso 1 · El contenedor exterior

```css
.navbar {
	display: flex;
	justify-content: space-between; /* reparte a lo ancho */
	align-items: center;            /* centra en vertical */
}
```

**Por qué:** `.navbar` tiene **tres** hijos directos: `.logo`, `.nav-links` y `.cta`.
- `space-between` pone el logo al inicio, el botón al final y los enlaces en medio.
- `align-items: center` iguala la línea vertical, aunque el logo y el botón tienen alturas distintas.

**Resultado:** tres bloques en fila, pero **los cuatro enlaces siguen pegados entre sí**. `.nav-links` todavía es un bloque normal.

### Paso 2 · El contenedor interior

```css
.nav-links {
	display: flex;
	gap: 2rem;
}
```

**Por qué:** los enlaces son hijos de `.nav-links`, no de `.navbar`. Es tu trampa 4: `display: flex` no afecta a los nietos. `.nav-links` es **ítem** de `.navbar` y **contenedor** de los enlaces a la vez.

**Resultado:** los enlaces quedan separados 2rem entre sí.

### Paso 3 · Variante: agrupar a la derecha

Comenta el `justify-content: space-between` del paso 1 y agrega:

```css
.navbar {
	gap: 2rem;
}

.nav-links {
	margin-left: auto;
}
```

**Por qué:** el margen `auto` absorbe todo el espacio libre a la izquierda de `.nav-links` y empuja los enlaces y el botón hacia la derecha (reto del ejercicio 03).

**Resultado:** el logo queda a la izquierda y los enlaces y el botón **juntos** a la derecha. Es el diseño más común en sitios reales. Quédate con la variante que prefieras.

### Paso 4 · Pantalla angosta

Mira la demo de 360px: todo se aprieta o se desborda. Es una de las fallas del tutorial de StudyCode, que no tenía plan para móvil. Agrega:

```css
.navbar {
	flex-wrap: wrap;
	row-gap: 0.75rem;
}

.nav-links {
	flex-wrap: wrap;
	column-gap: 1rem;
}
```

**Por qué:** con `wrap`, cuando no caben, los ítems bajan a otra línea en vez de desbordarse. Pasa en cualquier ancho, porque reacciona al espacio disponible y no a una media query.

**Resultado:** en 360px, el logo y el botón comparten la primera línea y los enlaces bajan a otra. Con el ancho completo, todo sigue en una fila.

## Reto

`.icon-btn` tiene un SVG y un texto. Haz que el icono quede **centrado en vertical** con el texto y con un poco de espacio entre ambos, y que el botón siga siendo inline (no debe ocupar toda la línea).

<details>
<summary>Ver solución</summary>

```css
.icon-btn {
	display: inline-flex;
	align-items: center;
	gap: 0.5rem;
}
```

- `inline-flex` mantiene el botón inline por fuera.
- `align-items: center` corrige el desfase natural entre un SVG y la línea base del texto.

Este patrón aparece en casi cualquier interfaz.
</details>

## Para recordar

> Una navbar es flex dentro de flex: el exterior reparte los bloques y el interior separa los enlaces. Con `flex-wrap`, la navbar se adapta a pantallas angostas sin media queries.
