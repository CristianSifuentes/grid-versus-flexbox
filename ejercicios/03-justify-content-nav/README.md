# 03 · `justify-content`: repartir el espacio libre

**Tiempo:** ~20 min · **Archivo que editas:** `styles.css`

## Conceptos que implementas

| Concepto | Dónde | Por qué |
| --- | --- | --- |
| Espacio libre | `.strip` | `justify-content` solo actúa si sobra espacio en el eje principal |
| `flex-start` / `center` / `flex-end` | franjas 1 a 3 | Juntan los ítems al inicio, al centro o al final |
| `space-between` / `around` / `evenly` | franjas 4 a 6 | Reparten el espacio libre *entre* y *alrededor* de los ítems |
| Menú horizontal | `.mdn-nav ul` | Ejercicio "Flexbox 1" de MDN: fila + espacio igual |
| `margin: auto` en flex | `.site-nav` | Un margen `auto` **absorbe todo el espacio libre** de ese lado |

## Checklist

- [ ] Paso 1 · Las franjas son contenedores flex
- [ ] Pasos 2 a 6 · Aplico cada valor y describo la diferencia con mis palabras
- [ ] Distingo `space-around` de `space-evenly`
- [ ] Paso 7 · Menú de MDN resuelto
- [ ] Reto · logo a la izquierda y enlaces a la derecha con `margin-right: auto`

## Paso a paso

### Paso 1 · Las franjas

```css
.strip {
	display: flex;
	gap: 0.5rem;
}
```

**Resultado:** todas las franjas se ven iguales, con las cajas a la izquierda. `flex-start` es el valor por defecto.

### Pasos 2 a 6 · Un valor por franja

```css
.strip--center  { justify-content: center; }
.strip--end     { justify-content: flex-end; }
.strip--between { justify-content: space-between; }
.strip--around  { justify-content: space-around; }
.strip--evenly  { justify-content: space-evenly; }
```

**Por qué:** cada valor decide qué hacer con el **espacio libre** (el ancho del contenedor menos lo que miden los ítems):

| Valor | Qué hace con el espacio libre |
| --- | --- |
| `flex-start` | Lo deja todo al final |
| `center` | La mitad a cada lado del grupo |
| `flex-end` | Lo deja todo al inicio |
| `space-between` | Solo **entre** ítems; los extremos quedan pegados al borde |
| `space-around` | Cada ítem lleva la misma cantidad a cada lado, así que el borde recibe **la mitad** del espacio que hay entre dos ítems |
| `space-evenly` | Todos los huecos son **idénticos**, incluidos los bordes |

**Resultado:** compara las franjas 5 y 6. En `around`, el hueco del borde es la mitad del hueco del centro; en `evenly` son iguales.

> **Prueba:** haz la ventana muy angosta. Cuando ya no sobra espacio, las seis franjas se ven iguales: `justify-content` no tiene nada que repartir.

### Paso 7 · Menú de MDN (Flexbox 1)

```css
.mdn-nav ul {
	display: flex;
	justify-content: space-between;
}
```

**Por qué:**
- El contenedor es el `ul`, porque los `li` son sus hijos directos (ejercicio 01).
- `space-between` reparte el sobrante entre los enlaces y deja Home y Contact Us pegados a los bordes.

**Resultado:** cuatro botones en fila, el primero al borde izquierdo, el último al derecho y el mismo espacio entre ellos.

## Reto

En `.site-nav`, el logo debe quedar a la izquierda y los tres enlaces **juntos** a la derecha. No puedes agregar divs.

<details>
<summary>Ver solución</summary>

```css
.site-nav {
	display: flex;
	align-items: center;
	gap: 1rem;
}

.site-nav .logo {
	margin-right: auto;
}
```

`space-between` no sirve aquí: separaría también los enlaces entre sí. En un contenedor flex, un margen `auto` absorbe **todo** el espacio libre de su lado, así que `margin-right: auto` en el logo empuja al resto hacia la derecha. Es la herramienta para "este a un lado, el resto al otro".
</details>

## Para recordar

> `justify-content` reparte el **espacio sobrante** en el eje principal. Para separar **un** ítem del resto, usa `margin: auto` en ese ítem.
