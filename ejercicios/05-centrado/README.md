# 05 · Centrado perfecto: tres formas

**Tiempo:** ~15 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| `justify-content` + `align-items` | `.stage--flex` | Un eje cada una: juntas centran en 2D |
| `place-items: center` | `.stage--grid` | Atajo de `align-items` + `justify-items`: centra cada ítem **en su celda** |
| `margin: auto` en flex/grid | `.stage--margin` | Los márgenes `auto` absorben el espacio libre en los dos ejes |
| Varios hijos | segunda demo | Flex los pone **en fila**; grid los **apila** en filas implícitas |
| `position` + `inset` | reto | Capa que cubre a su contenedor posicionado |

## Checklist

- [ ] Paso 1 · Centro con flexbox (2 propiedades)
- [ ] Paso 2 · Centro con grid (1 propiedad)
- [ ] Paso 3 · Centro con `margin: auto`
- [ ] Paso 4 · Explico por qué flex y grid se ven distintos con 2 hijos
- [ ] Reto · modal centrado

## Paso a paso

### Paso 1 · Flexbox

```css
.stage--flex {
	display: flex;
	justify-content: center; /* eje principal (horizontal) */
	align-items: center;     /* eje cruzado (vertical) */
}
```

**Por qué:** cada propiedad centra en su eje. Es el bloque de código de tu archivo de apuntes (`.centrar`).

**Resultado:** la caja queda en el centro exacto del recuadro.

### Paso 2 · Grid

```css
.stage--grid {
	display: grid;
	place-items: center;
}
```

**Por qué:**
- `display: grid` crea una sola celda que llena el contenedor.
- `place-items: center` es el atajo de `align-items: center` + `justify-items: center`, y centra al ítem **dentro de su celda**.

**Resultado:** el mismo centrado, con una sola propiedad.

> El tutorial de StudyCode decía que usar grid para centrar es un error. **No lo es**: es igual de válido. La regla "flex para 1D, grid para 2D" aplica al *layout*, no a centrar una sola cosa.

### Paso 3 · `margin: auto`

```css
.stage--margin {
	display: flex;
}

.stage--margin > .box {
	margin: auto;
}
```

**Por qué:**
- En un bloque normal, `margin: auto` solo centra en horizontal.
- Dentro de un contenedor flex (o grid), los márgenes `auto` **absorben el espacio libre en los dos ejes**.

**Resultado:** centrado otra vez. Es la misma idea del reto del ejercicio 03, aplicada en las cuatro direcciones.

### Paso 4 · Con dos hijos

Mira la segunda demo. Ya tiene el CSS de los pasos 1 y 2 porque comparte clases. Agrega un espacio entre las cajas:

```css
.stage--flex,
.stage--grid {
	gap: 0.5rem;
}
```

**Resultado:**
- **Flex:** A y B quedan **lado a lado** en el centro. Es un eje: los ítems se ponen en fila y el grupo se centra.
- **Grid:** A y B quedan **una encima de otra**. No definiste columnas, así que grid crea filas implícitas, una por ítem, y centra cada ítem en su fila.

**Por qué importa:** "centrar" no es lo mismo con varios hijos. Si los quieres en fila, flex. Si los quieres apilados y centrados, grid (o flex con `column`).

## Reto

`.overlay` debe cubrir **toda** la `.screen` (con el fondo oscuro) y el `.dialog` debe quedar centrado encima.

<details>
<summary>Ver solución</summary>

```css
.screen {
	position: relative;
}

.overlay {
	position: absolute;
	inset: 0; /* top, right, bottom y left en 0 */
	display: grid;
	place-items: center;
}
```

- `position: absolute` + `inset: 0` estira la capa a los bordes del ancestro posicionado (`.screen`).
- Grid centra el diálogo.

En un modal real usarías `position: fixed` para cubrir toda la ventana.
</details>

## Para recordar

> Para centrar algo, cualquiera de las tres formas es válida. Con **varios** hijos, flex los pone en fila y grid los apila.
