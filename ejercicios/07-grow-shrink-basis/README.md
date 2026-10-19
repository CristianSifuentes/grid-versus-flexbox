# 07 · `flex-grow`, `flex-shrink` y `flex-basis`

**Tiempo:** ~30 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| `flex-basis` | `.trio li` | Tamaño **inicial** sobre el eje principal, antes de crecer o encogerse |
| `flex-grow` | `.trio li` | Cuánto del espacio **sobrante** se lleva cada ítem |
| `flex-shrink` | `.shrink-lab` | Cuánto cede cada ítem cuando **falta** espacio |
| `flex: 1` = `1 1 0%` | `.trio li` | Base 0, así que se reparte **todo** el espacio y salen anchos iguales |
| `flex: 2` | `li:first-child` | Con base 0, exactamente el doble que los demás |
| `min-width: 0` | `.equal` | Desactiva el mínimo automático (`min-width: auto`) que impide encoger el contenido |
| Van en el **ítem** | todo | Tu trampa 2: grow, shrink y basis no se ponen en el contenedor |

## Checklist

- [ ] Paso 1 · Sin nada, cada columna mide según su texto
- [ ] Paso 2 · `flex-grow: 1` solo **no** iguala anchos, y sé por qué
- [ ] Paso 3 · `flex-basis: 0` sí los iguala
- [ ] Paso 4 · `flex: 2` en la primera: hago la cuenta del 50%
- [ ] Paso 5 · `flex-shrink: 0` protege el tamaño de un ítem
- [ ] Paso 6 · Arreglo la URL larga con `min-width: 0`
- [ ] Reto · sidebar fija + contenido fluido

## Paso a paso

### Paso 1 · Punto de partida

```css
.trio {
	display: flex;
	gap: 0.5rem;
}
```

**Resultado:** la tercera columna, con más texto, se lleva casi todo el ancho y la primera queda angosta.

**Por qué:** cada ítem tiene `flex: 0 1 auto`. La base `auto` es "lo que mide el contenido", y como no crecen, el que tiene más texto gana.

### Paso 2 · Solo `flex-grow`

```css
.trio li {
	flex-grow: 1;
}
```

**Resultado:** cambia poco. Las columnas **siguen siendo distintas**.

**Por qué:** `flex-grow` reparte solo el **sobrante**, lo que queda después de dar a cada ítem su base `auto`. Cada uno recibe la misma porción *extra*, pero partían de tamaños distintos. Si no sobra espacio, no hay nada que repartir.

### Paso 3 · Base cero

```css
.trio li {
	flex-basis: 0;
}
```

**Resultado:** **tres columnas iguales**, sin importar el texto.

**Por qué:** con base 0, *todo* el ancho es sobrante y se reparte 1 : 1 : 1. Esto es exactamente lo que hace el atajo:

```css
.trio li {
	flex: 1; /* = flex-grow: 1; flex-shrink: 1; flex-basis: 0% */
}
```

Reemplaza las dos propiedades de los pasos 2 y 3 por `flex: 1`. El resultado es el mismo.

### Paso 4 · La primera al doble (bonus de MDN)

```css
.trio li:first-child {
	flex: 2;
}
```

**Resultado:** la primera ocupa la mitad del ancho y las otras dos un cuarto cada una.

**La cuenta:** 2 / (2 + 1 + 1) = 2/4 = **50%**. Funciona **exacto** porque la base es 0. Con `flex-grow: 2` y base `auto`, el primero solo recibiría el doble del *sobrante*, no el doble de ancho.

### Paso 5 · `flex-shrink`

```css
.shrink-lab {
	display: flex;
}

.shrink-lab .box {
	flex: 0 1 200px; /* no crece · sí encoge · base 200px */
}

.shrink-lab .keep {
	flex-shrink: 0;
}
```

**Por qué:** las cajas quieren 600px, pero solo hay 420.
- Con `shrink: 1` en todas, las tres cederían lo mismo.
- Con `flex-shrink: 0`, C se niega a encogerse y A y B absorben todo el faltante.

**Resultado:** C mide 200px y A y B quedan más angostas (unos 100px cada una: faltan ~196px y se los reparten entre las dos). Úsalo para logos, iconos o avatares que no deben aplastarse.

### Paso 6 · La trampa: `flex: 1` no siempre da anchos iguales

```css
.equal {
	display: flex;
	gap: 0.5rem;
}

.equal .box {
	flex: 1;
}
```

**Resultado:** la caja con la URL es **mucho más ancha** que las otras, o desborda el contenedor.

**Por qué:** los ítems flex tienen `min-width: auto`, así que **no pueden encogerse por debajo de su contenido mínimo**, y una URL no tiene espacios donde partirse. Es la trampa 1 de tus apuntes. Arréglalo así:

```css
.equal .box {
	min-width: 0;            /* permite encoger por debajo del contenido */
	overflow-wrap: anywhere; /* y que el texto se parta donde haga falta */
}
```

**Resultado:** tres columnas iguales y la URL se parte en varias líneas.

## Reto

En `.app`, el `aside` debe medir **siempre 250px** (ni crecer ni encogerse) y el `main` debe ocupar todo lo demás.

<details>
<summary>Ver solución</summary>

```css
.app {
	display: flex;
	gap: 1rem;
}

.sidebar {
	flex: 0 0 250px; /* rígido */
}

.content {
	flex: 1;
	min-width: 0; /* por si adentro hay contenido largo */
}
```

`flex: 0 0 250px` vuelve rígido al ítem. Sin el `0` de shrink, la sidebar se aplastaría en pantallas angostas. Este layout también se puede hacer con grid (`grid-template-columns: 250px 1fr`), y lo harás en el ejercicio 12.
</details>

## Para recordar

> `flex: 1` = `1 1 0%`: base cero, así que se reparte todo. Si un contenido largo rompe la igualdad, agrega `min-width: 0` al ítem.
