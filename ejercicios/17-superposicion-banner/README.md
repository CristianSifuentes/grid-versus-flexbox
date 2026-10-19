# 17 · Superposición: varios ítems en la misma celda

**Tiempo:** ~25 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| Superponer con grid | `.banner` | Varios ítems en la **misma** celda se apilan. Flex no puede hacerlo (tabla de tus apuntes) |
| `grid-area: 1 / 1` | hijos de `.banner` | Fila 1, columna 1 para todos |
| `-self` para colocar | `.banner-text`, `.banner-badge` | Cada capa se alinea dentro de la celda compartida |
| Orden de apilado | `.banner` | Sin `z-index`, el último en el HTML queda encima |
| `z-index` sin `position` | `.banner-img` | Funciona en ítems grid y flex sin `position: relative` |
| Altura según el contenido | `.banner` | A diferencia de `position: absolute`, las capas **siguen en el flujo** |

## Checklist

- [ ] Paso 1 · Los tres hijos se apilan en la celda `1 / 1`
- [ ] Paso 2 · La imagen llena la celda con `object-fit`
- [ ] Paso 3 · Texto abajo a la izquierda e insignia arriba a la derecha
- [ ] Paso 4 · Cambio el orden de capas con `z-index`
- [ ] Paso 5 · Misma idea con `grid-template-areas: "stack"`
- [ ] Explico por qué esto es mejor que `position: absolute` para un banner
- [ ] Reto · tarjetas de conciertos

## Paso a paso

### Paso 1 · Todos a la misma celda

```css
.banner {
	display: grid;
}

.banner > * {
	grid-area: 1 / 1;
}
```

**Por qué:** `grid-area: 1 / 1` significa fila 1, columna 1. Cuando varios ítems reciben la **misma** posición, grid **no los separa**: los pinta uno encima del otro.

**Resultado:** imagen, texto e insignia apilados en la esquina superior izquierda. El texto queda sobre la foto.

### Paso 2 · La imagen como fondo

Primero prueba lo intuitivo:

```css
.banner {
	min-height: 360px;
}

.banner-img {
	width: 100%;
	height: 100%;
	object-fit: cover;
}
```

**Resultado:** el banner mide **más de 1500px** de alto.

**Por qué:** `banner.jpg` es vertical (687 × 1030). Mientras grid calcula el tamaño de la fila, el `height: 100%` del ítem todavía no se puede resolver (depende de la fila que se está calculando), así que la imagen aporta su **altura natural**, escalada a todo el ancho. Corrígelo:

```css
.banner-img {
	height: 0;          /* no aporta altura a la fila */
	min-height: 100%;   /* y luego llena la celda ya calculada */
}
```

**Resultado:** el banner mide 360px (o más, si el texto lo necesita) y la foto lo llena recortándose sin deformarse. Ahora la altura la deciden el `min-height` y el texto, no la foto. Es un truco que se usa mucho con imágenes de fondo en grid.

### Paso 3 · Colocar las capas

```css
.banner-text {
	align-self: end;     /* abajo */
	justify-self: stretch;
}

.banner-badge {
	align-self: start;   /* arriba */
	justify-self: end;   /* derecha */
}
```

**Por qué:** cada capa es un ítem de la misma celda y la alineas con `-self` (ejercicio 16). No hay coordenadas ni `top: 20px`.

**Resultado:** el texto, con su degradado, cubre la parte de abajo de la foto, y la insignia amarilla queda en la esquina superior derecha.

### Paso 4 · Orden de apilado

Sin `z-index`, el que aparece **después en el HTML** queda arriba. Prueba esto:

```css
.banner-img {
	z-index: 1;
}
```

**Resultado:** la foto **tapa** el texto y la insignia.

**Por qué:** en ítems grid (y flex), `z-index` funciona **sin `position: relative`**. Borra la regla o ponle `z-index: 1` también al texto y a la insignia, con valores mayores, para devolverlos arriba.

### Paso 5 · Variante con área con nombre

En vez de `grid-area: 1 / 1`, puedes nombrar la celda:

```css
.banner {
	grid-template-areas: "stack";
}

.banner > * {
	grid-area: stack;
}
```

**Resultado:** lo mismo, pero más legible cuando otros lo lean ("todos van a la pila").

### ¿Por qué no `position: absolute`?

Escribe un texto mucho más largo en `.banner-text` (en el HTML) y achica la ventana:
- **Con grid**, el banner **crece** para contener el texto, porque las capas siguen en el flujo y la celda mide lo que mida su ítem más alto.
- **Con `position: absolute`**, el texto saldría del flujo: el banner no lo "vería" y el texto se desbordaría por abajo (ejercicio 18).

## Reto

En `.concerts`, crea una rejilla responsive (ejercicio 14) y pon el nombre y la fecha **encima** de cada foto, **abajo a la izquierda**. Es el diseño de las tarjetas del proyecto principal (`index.html` de la raíz).

<details>
<summary>Ver solución</summary>

```css
.concerts {
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(min(240px, 100%), 1fr));
	gap: 1rem;
}

.concert {
	display: grid;
}

.concert > * {
	grid-area: 1 / 1;
}

.concert img {
	width: 100%;
	aspect-ratio: 4 / 3;
	object-fit: cover;
}

.concert-info {
	align-self: end;
}
```

Fíjate que el `alt=""` vacío en esas imágenes es intencional: el texto de la tarjeta ya describe el concierto, y la foto es decorativa.
</details>

## Para recordar

> Si varios ítems reciben la misma celda (`grid-area: 1 / 1`), se apilan. Cada capa se coloca con `-self`, y el banner sigue creciendo con su contenido, que es lo que `position: absolute` no logra.
