# 15 · Tamaños de pista: `fr`, `minmax(0, 1fr)`, `auto` y min/max-content

**Tiempo:** ~30 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| Hero con grid | `.hero` | Texto e imagen lado a lado (ejemplo del artículo de Lukman Faiz) |
| `1fr` = `minmax(auto, 1fr)` | `.hero` | El mínimo **auto** impide encoger por debajo del contenido |
| `minmax(0, 1fr)` | `.hero` | La versión grid del `min-width: 0` de flexbox |
| `auto` | `.sizes` | Se ajusta al contenido y puede estirarse si sobra espacio |
| `min-content` | `.sizes` | Lo más angosto posible: el ancho de la palabra más larga |
| `max-content` | `.sizes` | Todo el texto en una línea, sin partir |
| `fit-content(n)` | `.sizes` | Como `max-content`, pero sin pasar de `n` |
| Responsive sin media query | `.hero` | `auto-fit` + `minmax(min(…, 100%), 1fr)` apila las columnas cuando no caben |

## Checklist

- [ ] Paso 1 · Hero con `1fr 1fr`
- [ ] Paso 2 · Veo que las columnas **no son iguales**, y sé por qué
- [ ] Paso 3 · Lo arreglo con `minmax(0, 1fr)` + `overflow-wrap`
- [ ] Paso 4 · Comparo los 4 tamaños intrínsecos
- [ ] Paso 5 · El hero se apila en móvil **sin media query**
- [ ] Reto · imagen a la izquierda sin tocar el HTML

## Paso a paso

### Paso 1 · El hero del artículo

```css
.hero {
	display: grid;
	grid-template-columns: 1fr 1fr;
	gap: 2rem;
	align-items: center;
}
```

**Por qué:** dos columnas iguales (texto e imagen) con espacio entre ellas, y `align-items: center` para centrar el texto en vertical frente a la imagen. El mismo `align-items` que en flex (ejercicio 16).

### Paso 2 · El problema

Mira el resultado con atención. La columna del texto es **más ancha** que la de la imagen, o el hero se desborda en pantallas medianas.

**Por qué:** la URL larga no se puede partir. Y `1fr` **no** es "la mitad del espacio": es `minmax(auto, 1fr)`. El mínimo `auto` impide que la columna sea más angosta que su contenido más largo. Es la **misma trampa** que `flex: 1` con `min-width: auto` (ejercicio 07, paso 6).

### Paso 3 · El arreglo

```css
.hero {
	grid-template-columns: repeat(2, minmax(0, 1fr));
}

.hero-text {
	overflow-wrap: anywhere;
}
```

**Por qué:**
- `minmax(0, 1fr)` pone el mínimo en 0, así que las columnas sí pueden encogerse y quedan **exactamente iguales**.
- `overflow-wrap: anywhere` deja que la URL se parta para no salirse de su columna.

**Resultado:** dos columnas idénticas y la URL partida en varias líneas.

> **Regla práctica:** si quieres columnas **garantizadas iguales**, usa `minmax(0, 1fr)`. Si está bien que el contenido las empuje, `1fr` basta.

### Paso 4 · Laboratorio de tamaños

```css
.sizes {
	display: grid;
	grid-template-columns: auto min-content max-content fit-content(150px);
	gap: 0.5rem;
}
```

**Resultado** (columna por columna):

| Tamaño | Qué ves | Uso típico |
| --- | --- | --- |
| `auto` | Se estira para llenar el espacio sobrante | Columna "lo que quede" |
| `min-content` | Muy angosta: una palabra por línea | Etiquetas, iconos |
| `max-content` | Todo en una sola línea, sin partir | Botones que no deben romperse |
| `fit-content(150px)` | Crece con el texto hasta 150px y luego parte líneas | Columnas con tope |

Achica la ventana: `max-content` **nunca** cede, y por eso puede desbordar.

### Paso 5 · Hero responsive sin media query

Reemplaza las columnas del paso 3:

```css
.hero {
	grid-template-columns: repeat(auto-fit, minmax(min(300px, 100%), 1fr));
}
```

**Por qué:** es la técnica de la galería (ejercicio 14) aplicada a un layout de 2 columnas.
- Si caben 2 columnas de al menos 300px, quedan lado a lado.
- Si no caben, `auto-fit` deja **una sola** y la imagen baja debajo del texto.
- `min(300px, 100%)` evita el desborde en pantallas muy angostas.

**Resultado:** achica la ventana: el hero pasa de 2 columnas a 1 sin ningún `@media`.

## Reto

Pon la **imagen a la izquierda** y el texto a la derecha **sin tocar el HTML**. Debe seguir funcionando cuando el hero se apila en una columna (paso 5). ¿Es aceptable para la accesibilidad?

<details>
<summary>Ver solución</summary>

```css
.hero-img {
	order: -1;
}
```

`order` también funciona en grid: el auto-placement coloca primero los ítems con `order` menor. Con una columna, la imagen queda **arriba** del texto.

No uses `grid-column: 1` aquí: cuando `auto-fit` deja una sola columna, el texto pediría una columna 2 que ya no existe y se crearía una columna implícita.

**Accesibilidad:** aquí es aceptable, porque la imagen no es interactiva ni cambia el sentido de la lectura. Si fuera un enlace o un botón, el orden del Tab no coincidiría con lo que se ve (ejercicios 09 y 13).
</details>

## Para recordar

> `1fr` es en realidad `minmax(auto, 1fr)`: el contenido largo puede ensanchar la columna. Para columnas garantizadas iguales, usa `minmax(0, 1fr)`.
