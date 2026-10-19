# 20 · Proyecto final: dashboard (grid macro + flex micro)

**Tiempo:** ~60 min · **Archivo que editas:** `styles.css`

Aquí juntas todo. Antes de escribir cada paso, intenta responder **tú** "¿grid o flex? ¿por qué?" y luego compara con la guía.

## Mapa de decisiones

| Pieza | Herramienta | Por qué | Ejercicio de origen |
| --- | --- | --- | --- |
| `.dashboard` | **Grid** + áreas | Filas **y** columnas: macro layout | 12, 13 |
| `.dash-header` | **Flex** | Una fila, el contenido manda | 08 |
| `.dash-search` | `flex: 1` + `min-width: 0` | Ocupa lo que sobra sin desbordar | 07 |
| `.dash-sidebar` | **Flex** column (móvil: row + wrap) | Lista en una dirección | 02, 06 |
| `.stats` | **Grid** `auto-fit` | Columnas alineadas sin media query | 14 |
| `.stat` | **Flex** column | Apilar etiqueta, valor y tendencia | 09 |
| `.panels` | **Grid** `minmax(0, …)` | Dos columnas proporcionales | 15 |
| `.panel--featured` | **Grid** superposición | Texto encima de la imagen | 17 |
| `.event-list li` | **Flex** + `space-between` | Nombre a la izquierda y fecha a la derecha | 03 |

## Checklist

- [ ] Paso 1 · Variables CSS para gap y anchos
- [ ] Paso 2 · Macro layout móvil (una columna, áreas)
- [ ] Paso 3 · Escritorio: sidebar + main, header y footer a todo el ancho
- [ ] Paso 4 · Header: logo, buscador flexible y usuario
- [ ] Paso 5 · Sidebar: fila con wrap en móvil, columna en escritorio
- [ ] Paso 6 · Stats responsive sin media query
- [ ] Paso 7 · Cada stat en columna con la tendencia abajo
- [ ] Paso 8 · Paneles 2:1 en escritorio
- [ ] Paso 9 · Panel destacado con texto sobre la imagen
- [ ] Paso 10 · Lista de eventos
- [ ] Prueba de teclado: el Tab recorre header → sidebar → main en orden
- [ ] Pruebo en 360px, 768px y 1200px

## Paso a paso

### Paso 1 · Variables CSS

```css
:root {
	--gap: 1rem;
	--sidebar: 220px;
}
```

**Por qué:** el último consejo del tutorial de StudyCode: define el espaciado una vez y úsalo en grid **y** en flex. Si mañana cambias `--gap`, todo el dashboard se ajusta igual.

### Paso 2 · Macro layout (móvil primero)

```css
.dashboard {
	display: grid;
	gap: var(--gap);
	grid-template-columns: minmax(0, 1fr);
	grid-template-areas:
		"header"
		"sidebar"
		"main"
		"footer";
}

.dash-header  { grid-area: header; }
.dash-sidebar { grid-area: sidebar; }
.dash-main    { grid-area: main; }
.dash-footer  { grid-area: footer; }
```

**Por qué:**
- La página es un problema 2D, así que va con grid y áreas con nombre.
- `minmax(0, 1fr)` en vez de `1fr` evita que algo ancho (la imagen, una tabla) estire la columna (ejercicio 15).

### Paso 3 · Escritorio

```css
@media (min-width: 900px) {
	.dashboard {
		grid-template-columns: var(--sidebar) minmax(0, 1fr);
		grid-template-rows: auto 1fr auto;
		grid-template-areas:
			"header  header"
			"sidebar main"
			"footer  footer";
		min-height: 100vh;
	}
}
```

**Por qué:** las filas `auto 1fr auto` se definen **solo aquí**, donde el mapa tiene 3 filas. Así evitas el bug de StudyCode (ejercicio 13). `min-height: 100vh` empuja el footer al fondo de la pantalla.

**Resultado:** con más de 900px, la sidebar queda a la izquierda y el header y el footer cubren todo el ancho.

### Paso 4 · Header

```css
.dash-header {
	display: flex;
	align-items: center;
	gap: var(--gap);
}

.dash-search {
	flex: 1;
	min-width: 0;
}

.dash-user {
	display: flex;
	align-items: center;
	gap: 0.5rem;
}

.avatar {
	display: grid;
	place-items: center;
}
```

**Por qué:**
- Es una fila con 3 piezas, así que flex.
- El buscador se lleva el espacio sobrante con `flex: 1`, y `min-width: 0` le permite encogerse en móvil (ejercicio 07).
- `.dash-user` es flex anidado (ejercicio 08).
- Las iniciales del avatar se centran con `place-items` (ejercicio 05).

### Paso 5 · Sidebar

```css
.dash-sidebar {
	display: flex;
	flex-wrap: wrap;
	gap: 0.25rem;
}

@media (min-width: 900px) {
	.dash-sidebar {
		flex-direction: column;
		flex-wrap: nowrap;
	}
}
```

**Por qué:** en móvil, los enlaces van en fila y saltan de línea si no caben, como una barra de pestañas. En escritorio, la misma lista va en columna. Solo cambia el eje (ejercicio 02).

### Paso 6 · Stats

```css
.stats {
	display: grid;
	grid-template-columns: repeat(auto-fit, minmax(min(180px, 100%), 1fr));
	gap: var(--gap);
}
```

**Por qué:** 4, 2 o 1 columnas según el espacio, **sin media query**. Usamos `auto-fit` porque, si algún día hay solo 2 stats, queremos que llenen la fila (ejercicio 14).

### Paso 7 · Cada stat

```css
.stat {
	display: flex;
	flex-direction: column;
	gap: 0.25rem;
}

.stat-trend {
	margin-top: auto;
}
```

**Por qué:** grid estira todas las stats de una fila a la misma altura, y `margin-top: auto` alinea las tendencias abajo, aunque una etiqueta ocupe dos líneas (ejercicio 09).

### Paso 8 · Paneles

```css
.panels {
	display: grid;
	gap: var(--gap);
	margin-top: var(--gap);
}

@media (min-width: 1100px) {
	.panels {
		grid-template-columns: minmax(0, 2fr) minmax(0, 1fr);
	}
}
```

**Por qué:**
- Este paso sí usa media query: una proporción 2:1 solo tiene sentido con espacio. En pantallas menores queda una columna.
- `minmax(0, …)` impide que la imagen grande rompa la proporción (ejercicio 15).

### Paso 9 · Panel destacado

```css
.panel--featured {
	display: grid;
	min-height: 260px;
}

.panel--featured > * {
	grid-area: 1 / 1;
}

.panel--featured img {
	width: 100%;
	height: 0;
	min-height: 100%;
	object-fit: cover;
}

.panel-caption {
	align-self: end;
	display: flex;
	justify-content: space-between;
	align-items: center;
	flex-wrap: wrap;
	gap: var(--gap);
}
```

**Por qué:**
- Superposición con grid y el truco `height: 0; min-height: 100%` para que la foto vertical no infle el panel (ejercicio 17).
- Dentro del pie, flex pone el título a un lado y el botón al otro.
- Grid y flex se anidan en las dos direcciones: un ítem grid puede ser contenedor flex y viceversa.

### Paso 10 · Lista de eventos

```css
.event-list li {
	display: flex;
	justify-content: space-between;
	align-items: center;
	gap: 0.5rem;
}
```

**Por qué:** cada fila tiene dos piezas, una a cada lado, así que `space-between` (ejercicio 03).

### Prueba final

1. Cambia el ancho a **360px**, **768px** y **1200px** (modo responsive de DevTools). Nada debe desbordar horizontalmente.
2. Haz clic en la barra de direcciones y recorre con **Tab**: buscador → enlaces de la sidebar → botón "Ver detalles". El orden visual y el del DOM coinciden porque no reordenaste nada.
3. Abre DevTools y mira las insignias `grid` y `flex`. Cuenta cuántos contenedores de cada tipo usaste y por qué.

## Reto extra: el proyecto de conciertos

Abre el `index.html` y el `styles.css` de **la raíz del repo** (el proyecto original "Flexbox vs CSS Grid"). Con lo aprendido:

- [ ] `.container`: grid con las tarjetas y el banner lado a lado (`minmax(0, 1fr) 300px`), en una columna en móvil
- [ ] `.concerts-container`: grid `auto-fill` + `minmax(min(220px, 100%), 1fr)`
- [ ] `.card .texts`: flex en columna con `justify-content: flex-end`, para que el texto quede abajo de cada foto
- [ ] `.banner`: flex en columna con el botón empujado al fondo (`margin-top: auto`)

Antes de cada decisión, escribe en un comentario del CSS: **"grid/flex porque…"**.

## Para recordar

> **Grid para la estructura** (página, rejillas) y **flex para los componentes** (barras, tarjetas, filas). Se anidan sin problema; la clave es saber qué dimensión controla cada pieza.
