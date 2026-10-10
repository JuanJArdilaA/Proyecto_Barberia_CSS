# Barbería El Bigote Elegante - Proyecto HTML y CSS

## 1. El Proyecto
**Negocio:** Barbería "El Bigote Elegante".
**Público objetivo:** Hombres del barrio que buscan servicios de barbería clásica, cortes de cabello y arreglo de barba.
**Necesidad del cliente:** El negocio solo se comunicaba por WhatsApp y los clientes preguntaban constantemente por los precios y la ubicación. El cliente necesitaba un sitio web sencillo, confiable, que funcionara perfecto en celulares, mostrara sus servicios y permitiera agendar reservas fácilmente.

## 2. Estructura
El proyecto consta de dos páginas:
- `index.html`: Página principal con los servicios destacados, información útil y ubicación.
- `contacto.html`: Página de reservas con la tabla de precios detallada y un formulario interactivo.

**Uso de semántica:**
- `<header>` y `<nav>`: Para el encabezado principal y el menú de navegación, facilitando la accesibilidad.
- `<main>`: Envuelve el contenido principal y único de cada página.
- `<section>`: Para dividir temáticamente el contenido (ej. `#servicios`, `.testimonios`).
- `<article>`: Utilizado para cada tarjeta de servicio, ya que un servicio por sí solo tiene sentido independiente.
- `<aside>`: Usado para la "Información extra" (cómo prepararse para la visita) porque es contenido tangencial al principal.
- `<footer>`: Para los derechos de autor y redes sociales.

## 3. Sistema Visual
- **Colores:** Se definió una paleta basada en variables CSS (`:root`).
  - `--color-fondo: #f4f4f4`: Gris muy claro para no cansar la vista.
  - `--color-acento: #b38b59`: Un tono dorado vintage que transmite la elegancia de una barbería clásica.
  - `--color-cabecera: rgba(0, 0, 0, 0.8)`: Negro con canal alfa (80% de opacidad) que permite leer el texto claramente cuando se mezcla con el gradiente de fondo.
- **Fuentes:** `--fuente-principal: 'Arial', sans-serif`. Se eligió una fuente de palo seco por su excelente legibilidad en pantallas pequeñas (celulares).
- **Espaciado:** Uso de `rem` (ej. `1.5rem`) para mantener las proporciones si el usuario cambia el tamaño de fuente de su navegador.

## 4. Especificidad
A continuación, se documentan 3 conflictos reales resueltos en el archivo `styles.css`:

### Conflicto 1: El precio destacado en la tarjeta de promoción
Reglas que compiten por el color del texto del precio:
- `.tarjeta p` -> Especificidad: **(0, 1, 1)** [1 clase, 1 etiqueta]
- `.tarjeta.tarjeta--promo .precio` -> Especificidad: **(0, 3, 0)** [3 clases: .tarjeta, .tarjeta--promo, .precio]

**Ganador:** Gana `.tarjeta.tarjeta--promo .precio` por tener 3 clases, lo que sobreescribe el color general del párrafo y lo pinta de rojo (#d9534f).
*(Ver captura en: `capturas/conflicto1.png`)*

### Conflicto 2: Empate resuelto por orden de cascada
Reglas que compiten por el color de fondo de la tarjeta promocional (simulado durante el desarrollo):
- `.tarjeta` -> Especificidad: **(0, 1, 0)** [1 clase]
- `.tarjeta--promo` -> Especificidad: **(0, 1, 0)** [1 clase]

**Ganador:** Gana `.tarjeta--promo`. Como ambas tienen exactamente la misma especificidad (un empate de 0,1,0), el navegador aplica la regla que está escrita **más abajo** en la hoja de estilos. 
*(Ver captura en: `capturas/conflicto2.png`)*

### Conflicto 3: Selector de atributo en el formulario
Reglas que compiten por el ancho (width) y padding de los inputs:
- `input` -> Especificidad: **(0, 0, 1)** [1 etiqueta]
- `.formulario input[type="text"]` -> Especificidad: **(0, 2, 1)** [1 clase, 1 atributo, 1 etiqueta]

**Ganador:** Gana el selector de atributo `.formulario input[type="text"]`, asegurando que solo los inputs del formulario reciban los estilos de la caja, sin afectar otros inputs que pudieran existir fuera.
*(Ver captura en: `capturas/conflicto3.png`)*

## 5. Herencia
- **Propiedad que SÍ se hereda:** `font-family: var(--fuente-principal);` y `color: var(--color-texto);` aplicados a la etiqueta `body`. Esto hace que los `<p>`, `<h1>` y `<li>` adopten la fuente y el color sin tener que declararlo uno por uno.
- **Propiedad que NO se hereda:** `border: 1px solid #ddd;` aplicado a `.tarjeta`. El borde envuelve la tarjeta completa, pero no se hereda a la imagen (`<img>`) ni al título (`<h3>`) que están por dentro de ella.

## 6. Modelo de Cajas
- **Cálculo del ancho total (`.seccion-reservas`):**
  - La caja tiene `max-width: 500px`.
  - Tiene un `padding: 2rem` (aprox 32px por lado) y un `border: 2px`.
  - Como al inicio de la hoja de estilos utilicé el reseteo `* { box-sizing: border-box; }`, el ancho total **sigue siendo exactamente 500px**. El padding y el borde (68px en total) se "comen" el espacio hacia adentro, dejando el área de contenido real en 432px. Si no tuviera `border-box`, la caja mediría 568px de ancho.
- **Márgenes que se solapan (Collapsing margins):**
  Las tarjetas (`.tarjeta`) tienen `margin: 1.5rem auto;`. Cuando en versión móvil se apila una tarjeta debajo de otra, el margen inferior de la primera (1.5rem) y el superior de la segunda (1.5rem) **se solapan** (no suman 3rem). El espacio final entre ellas sigue siendo de 1.5rem.

## 7. Selectores
**Uso de la pseudoclase estructural `:nth-child`:**
En la tabla de precios usé el selector `.tabla-precios tbody tr:nth-child(even)`.
- **¿Qué hermanos cuenta?** Cuenta todos los elementos `<tr>` (filas) que sean hijos directos del `<tbody>`.
- **¿A cuál se aplica?** Al pasarle el parámetro `even` (pares), aplica un color de fondo gris claro (`#f8f8f8`) a la fila 2, fila 4, etc. Esto genera el efecto de "filas cebra" para mejorar la lectura sin necesidad de ponerle una clase a cada fila.

## 8. Responsive Design
**Uso de `@media (min-width: 768px)`:**
El sitio se diseñó con el enfoque "Mobile First". Por defecto, `.tarjeta` tiene `display: block` y toma todo el ancho, lo cual es perfecto para un celular de 360px.
- Cuando la pantalla llega a 768px o más, el `@media` entra en acción y cambia las tarjetas a `display: inline-block` con un `width: 30%`. 
- **¿Por qué?** Porque en pantallas de escritorio sobra mucho espacio a los lados, así que aprovechamos para colocar las tres tarjetas de servicios una al lado de la otra en una misma línea.

## 9. Problemas y Soluciones
1. **Problema con las imágenes de fondo:** Al principio, el texto del encabezado no se leía bien porque se mezclaba con el color sólido o la imagen. 
   **Solución:** Utilicé `background: linear-gradient(...)` combinando un color oscuro sólido con una variable RGBA (`rgba(0, 0, 0, 0.8)`). El canal alfa creó una capa semitransparente que mejoró drásticamente el contraste.
2. **Problema con las tarjetas inline-block:** Al pasar a la vista de escritorio (más de 768px), las tarjetas se desalineaban verticalmente si una tenía más texto que la otra.
   **Solución:** Investigué y le agregué `vertical-align: top;` a `.tarjeta` dentro de la Media Query. Con esto, todas se alinearon perfectamente por la parte superior.

## 10. Capturas
*(Recuerda subir estas imágenes a la carpeta `capturas/` de tu repositorio)*

- **Vista Móvil (360px):**
  ![Vista Móvil Inicio](capturas/movil-inicio.png)
  ![Vista Móvil Contacto](capturas/movil-contacto.png)

- **Vista Escritorio:**
  ![Vista Escritorio Inicio](capturas/escritorio-inicio.png)
  ![Vista Escritorio Contacto](capturas/escritorio-contacto.png)

**Enlace de GitHub Pages (Sitio en Vivo):**
[AQUÍ_VA_TU_LINK_DE_GITHUB_PAGES]