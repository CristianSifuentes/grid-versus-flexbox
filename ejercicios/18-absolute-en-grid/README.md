# 18 · `position: absolute` dentro de un grid

**Tiempo:** ~30 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| Bloque contenedor | `.wrapper` | Un ítem absoluto se posiciona respecto al ancestro **posicionado** más cercano |
| Grid area como bloque contenedor | `.box3` | Si el contenedor es `relative` **y** el ítem tiene líneas, los offsets se miden desde **su área** |
| Sale del flujo | `.box3` | El auto-placement **ignora** su espacio y no se crean filas por su culpa |
| Sin `position: relative` | `.wrapper` | El bloque contenedor pasa al viewport (o al ancestro posicionado) |
| Área con `position: relative` | `.area` | Un hijo absoluto se mide desde esa área |

## Checklist

- [ ] Paso 1 · Grid de 4 columnas
- [ ] Paso 2 · `.box3` en las líneas 2/4 y 1/3
- [ ] Paso 3 · Sin `relative`, el ítem se va lejos (bloque contenedor = viewport)
- [ ] Paso 4 · Con `relative`, se mide desde **su área** y los demás ocupan su lugar
- [ ] Paso 5 · Sin líneas, se mide desde todo el contenedor
- [ ] Paso 6 · Hijo absoluto dentro de un área `relative`
- [ ] Reto · contador sobre la esquina

## Paso a paso

### Paso 1 · El grid

```css
.wrapper {
	display: grid;
	grid-template-columns: repeat(4, 1fr);
	grid-auto-rows: 200px;
	gap: 20px;
}
```

(MDN escribe `grid-gap: 20px`, que es el nombre viejo.)

### Paso 2 · Colocar `.box3`

```css
.box3 {
	grid-column: 2 / 4;
	grid-row: 1 / 3;
}
```

**Resultado:** Three ocupa las columnas 2 y 3 y **dos filas**. One, Two y Four se acomodan alrededor, y aparece una segunda fila porque Three la necesita.

### Paso 3 · `position: absolute` sin contenedor posicionado

```css
.box3 {
	position: absolute;
	top: 40px;
	left: 40px;
}
```

**Resultado:** Three **sale volando** a la esquina superior izquierda de **la página**, encima del título.

**Por qué:** un elemento absoluto se posiciona respecto al **ancestro posicionado más cercano** (cualquier `position` distinta de `static`). Ningún ancestro lo está, así que usa el bloque contenedor inicial (el viewport). Es el caso "el grid container como parent" de MDN.

Fíjate también en que **One, Two y Four se reacomodaron** y ocupan el espacio de Three. Al salir del flujo, el auto-placement ya no lo ve.

### Paso 4 · El contenedor como bloque contenedor

```css
.wrapper {
	position: relative;
}
```

**Resultado:** Three vuelve al grid, **desplazado 40px** desde la esquina de **su área** (línea de columna 2 y línea de fila 1), no desde la esquina del contenedor.

**Por qué:** si el grid es el bloque contenedor **y** el ítem absoluto tiene posición de líneas, MDN dice que *"sus offsets se calculan a partir de los bordes del área en la que está colocado"*. Mira dos cosas más:
- One, Two y Four **siguen ocupando** el lugar de Three. Se superponen, porque sigue fuera del flujo.
- **No hay segunda fila** de 200px por Three: un ítem absoluto no crea pistas.

### Paso 5 · Sin líneas

Comenta el `grid-column` y el `grid-row` de `.box3` del paso 2.

**Resultado:** Three se posiciona 40px desde la esquina del **contenedor entero** (su borde de padding) y su tamaño depende de su contenido.

**Por qué:** sin líneas asignadas, el bloque contenedor es todo el grid. Vuelve a activarlas.

### Paso 6 · Demo B: el área como padre

```css
.wrapper-b {
	display: grid;
	grid-template-columns: repeat(4, 1fr);
	grid-auto-rows: 200px;
	gap: 20px;
}

.area {
	grid-column: 2 / 4;
	grid-row: 1 / 3;
	position: relative;
}

.abspos {
	position: absolute;
	top: 40px;
	left: 40px;
}
```

**Por qué:** ahora el que está en el grid es `.area`, que sigue **en el flujo** (ocupa su espacio y crea su fila). El absoluto es su **hijo**, y como `.area` es `relative`, se mide desde ella.

**Resultado:** One, Two y Four respetan a Three, y el recuadro blanco flota 40px dentro de Three. Es el patrón que más vas a usar.

## Reto

En `.notifications`, haz una rejilla de 3 columnas. Cada contador `.count` debe quedar sobre la **esquina superior derecha** de su tarjeta, **saliéndose 8px** del borde.

<details>
<summary>Ver solución</summary>

```css
.notifications {
	display: grid;
	grid-template-columns: repeat(3, 1fr);
	gap: 1.5rem;
	padding: 16px;
}

.notif {
	position: relative;
}

.count {
	position: absolute;
	top: -8px;
	right: -8px;
}
```

Aquí `position: absolute` **sí** es la herramienta correcta: el contador es decoración que no debe afectar el tamaño de la tarjeta. Compáralo con el ejercicio 17, donde el texto **sí** debía empujar la altura del banner y por eso usaste grid.
</details>

## Para recordar

> Un absoluto en grid se mide desde **su área** si el contenedor es `relative` y el ítem tiene líneas, y desde todo el contenedor si no tiene líneas. Sale del flujo: no ocupa celda ni crea filas.
