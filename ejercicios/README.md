# Ejercicios · Flexbox y Grid

20 prácticas progresivas para dominar el layout en CSS **escribiendo tú el código**.

## Cómo trabajar cada ejercicio

1. Arranca el servidor desde la raíz del repo: `npm run ejercicios` (se abre este índice en el navegador).
2. Abre la carpeta del ejercicio. Cada una tiene:
   - `index.html`: el HTML ya está hecho; **no necesitas tocarlo** (salvo que la guía lo pida).
   - `styles.css`: casi vacío, con un comentario por paso. **Aquí escribes tú.**
   - `README.md`: la guía con los conceptos, el **por qué** de cada línea, el resultado esperado y el checklist.
3. Escribe un paso, guarda, mira el navegador (se recarga solo) y compara con el **Resultado** de la guía.
4. Marca el checklist del README de ese ejercicio (`- [x]`).
5. Haz el **Reto** sin mirar la solución; está escondida en un `<details>`.
6. Abre DevTools y activa las insignias `flex` / `grid`: son tu mejor profesor.

> El CSS de `assets/base.css` solo da colores y bordes punteados a los contenedores (`.lab`). **No contiene flexbox ni grid**: todo el layout lo escribes tú.

## Progreso

### Parte 1 · Flexbox (una dimensión)

- [ ] [01 · Contenedor e ítems flex](01-contenedor-flex/README.md): `display: flex`, hijos directos, valores por defecto, `inline-flex`
- [ ] [02 · Ejes y flex-direction](02-ejes-flex-direction/README.md): eje principal y cruzado, `column` voltea los ejes, `-reverse`, `rtl`
- [ ] [03 · justify-content y navegación](03-justify-content-nav/README.md): 6 valores, menú de MDN, `margin: auto`
- [ ] [04 · align-items y align-self](04-align-items-self/README.md): `stretch`, `baseline`, columnas de igual altura
- [ ] [05 · Centrado perfecto](05-centrado/README.md): flex, `place-items`, `margin: auto`, modal
- [ ] [06 · flex-wrap, gap y align-content](06-wrap-gap-align-content/README.md): líneas independientes, `gap` contra `margin`
- [ ] [07 · grow, shrink y basis](07-grow-shrink-basis/README.md): `flex: 1`, `flex: 2`, `min-width: 0`
- [ ] [08 · Navbar con flex anidado](08-navbar-flex-anidado/README.md): flex dentro de flex, wrap sin media query
- [ ] [09 · Tarjetas en columna y order](09-tarjeta-column-order/README.md): pie abajo, `flex-basis` como altura, `order` y Tab

### Parte 2 · Grid (dos dimensiones)

- [ ] [10 · Primer grid](10-primer-grid/README.md): pistas, `fr`, `gap`, grid implícito
- [ ] [11 · Colocar por líneas](11-lineas-grid/README.md): líneas, `span`, `-1`, `grid-area`
- [ ] [12 · grid-template-areas](12-template-areas/README.md): layout de página con nombres
- [ ] [13 · Responsive con áreas](13-responsive-areas/README.md): mobile first, el bug de las filas, accesibilidad
- [ ] [14 · Galería auto-fill / auto-fit](14-galeria-auto-fit/README.md): `minmax`, `min()`, `dense`
- [ ] [15 · Tamaños de pista y hero](15-tamanos-pista-hero/README.md): `minmax(0, 1fr)`, min/max-content
- [ ] [16 · Box Alignment en grid](16-alineacion-grid/README.md): `-items`, `-self`, `-content`
- [ ] [17 · Superposición](17-superposicion-banner/README.md): misma celda, `z-index`, banner

### Parte 3 · Integración

- [ ] [18 · position: absolute en grid](18-absolute-en-grid/README.md): bloque contenedor y flujo
- [ ] [19 · display: contents y subgrid](19-contents-subgrid/README.md): nietos en el grid
- [ ] [20 · Proyecto final: dashboard](20-dashboard-final/README.md): grid macro + flex micro (+ reto sobre el proyecto de conciertos)

## Mapa de conceptos → ejercicio

| Concepto | Ejercicios |
| --- | --- |
| Contenedor e ítems, hijos directos | 01, 08, 19 |
| Eje principal y cruzado | 02, 03, 04 |
| `justify-content` | 03, 08, 16 |
| `align-items` / `align-self` | 04, 16 |
| Centrado | 05, 16 |
| `flex-wrap`, `align-content` | 06, 08 |
| `gap` contra `margin` | 06, 08, 10 |
| `flex-grow` / `shrink` / `basis` | 07, 09, 20 |
| `min-width: 0` / `minmax(0, 1fr)` | 07, 15, 20 |
| `margin: auto` en flex | 03, 05, 08, 09 |
| Pistas, `fr`, grid implícito | 10 |
| Líneas, `span`, `grid-area` | 11, 17 |
| `grid-template-areas` | 12, 13, 20 |
| Responsive sin media queries | 08, 14, 15, 20 |
| `auto-fill` contra `auto-fit` | 14 |
| Superposición | 16, 17, 20 |
| `position: absolute` | 05, 18 |
| `display: contents`, `subgrid` | 19 |
| **Accesibilidad del orden** | 02, 09, 13, 14, 15 |
| Grid macro + flex micro | 09, 17, 20 |
