# Aditum Card & Badge Generator · v4.0

Generador unificado de tarjetas de presentación y gafetes de credencial para el equipo Aditum. App estática HTML/CSS/JavaScript que produce PDFs listos para imprenta. Sin backend, sin build step, sin dependencias en runtime más allá de las librerías incluidas.

## Filosofía de diseño · "Imago Anchor"

El símbolo Aditum (triángulo con gradient navy-amber-teal y partículas) es el activo de marca más distintivo, y el sistema lo trata como protagonista visual, no como decoración secundaria.

**Tarjeta:** imago grande (34mm) como ancla del lado izquierdo. Bloque tipográfico con eje vertical centrado del lado derecho. Reverso navy con logo completo (imago + wordmark) y QR vCard.

**Gafete:** foto del empleado domina la mitad superior. Línea teal y nombre debajo. Logo pequeño en footer. Reverso navy con logo completo y QR grande para escaneo a distancia.

## Sistema visual unificado

Tarjeta y gafete comparten **idioma visual** sin replicar composición:

- **Paleta:** navy `#0A2E5C`, teal-bright `#2A8B9F`, copper `#B8704A`, slate `#5A6B7D`, slate-light `#8895A4`
- **Tipografía:** Barlow Condensed (SemiBold/Medium/Regular) + Open Sans (Regular/SemiBold), ambas OFL embebidas en el PDF
- **Imago Aditum:** mismo asset 1600×1600 RGBA con anti-aliasing real, usado en frente de tarjeta y en reverso de ambos productos
- **Línea teal accent:** elemento visual recurrente (14mm en tarjeta, 16mm en gafete)
- **Etiquetas M./T./E./W.** en teal-bright como signature de los datos de contacto
- **Wordmark reverse blanco sobre navy** en los reversos
- **Web en copper** en todos los reversos, letterspaced uppercase

## Productos generados

### Tarjeta de presentación (90 × 55 mm landscape)

PDF de 2 páginas:

- **Frente:** imago Aditum 34mm como ancla visual izquierda (con todas las partículas, gradient preservado). Bloque tipográfico a la derecha con eje vertical centrado: nombre 16pt Barlow Condensed SemiBold, puesto 9pt slate, línea teal 14×1.2pt, datos de contacto con etiquetas M./T./E./W. en teal-bright + texto en slate. El bloque se centra automáticamente sobre el eje del imago, ajustando su altura según cuántas filas de contacto tenga el usuario (3, 4 o 5 filas).
- **Reverso:** navy sólido con logo completo (imago + ADITUM CONSULTING wordmark blanco) arriba, QR vCard 18×18mm centrado con respiración generosa, web URL copper letterspaced en footer.

### Gafete de credencial (54 × 85.6 mm portrait, CR80)

PDF de 2 páginas:

- **Frente:** foto del empleado 38×50.66mm (proporción 3:4) ocupa la mitad superior, dejando libre la zona del lanyard arriba. Línea teal 16mm debajo de la foto. Nombre 15pt centrado, puesto 9pt slate. Logo completo pequeño (18mm) en el footer.
- **Reverso:** navy sólido con logo completo arriba (debajo de la zona del lanyard), nombre del empleado, QR vCard 28×28mm grande para escaneo a distancia, web copper en footer.
- **Ranura del lanyard:** rectángulo guía 10×3mm centrado a 4.5mm del borde superior, incluido como línea gris fina en ambas páginas para que el impresor sepa dónde perforar.

### Tarjeta de pantalla (vertical, para mostrar en el celular)

PDF de 1 página, formato vertical (≈9:16), pensado para abrirse y mostrarse en la pantalla de un teléfono cuando conoces a alguien. No es para imprimir: no lleva sangrado ni marcas de corte.

- Fondo navy completo, imago Aditum arriba, nombre y puesto centrados, línea teal de acento.
- **QR vCard grande (36mm)** como elemento dominante: la otra persona lo escanea con su cámara y guarda el contacto directo.
- Microcopy "Escanea para añadir contacto" debajo del QR.
- Teléfono principal y correo como referencia visual rápida (el QR ya contiene la vCard completa).
- Web en copper en el footer.

Es la base natural para una futura tarjeta digital con NFC o página web: el QR y el eventual chip apuntarían al mismo destino. Por ahora, es un PDF self-contained que cualquiera puede abrir y mostrar.

## Especificaciones de imprenta

| Parámetro | Tarjeta | Gafete |
|---|---|---|
| Trim final | 90 × 55 mm | 54 × 85.6 mm (CR80) |
| Sangrado | 3 mm cada lado | 3 mm cada lado |
| Canvas total | 96 × 61 mm | 60 × 91.6 mm |
| Resolución | 300 DPI | 300 DPI |
| Marcas de corte | 4 esquinas, 0.5pt | 4 esquinas, 0.5pt |
| Ranura lanyard | — | 10 × 3 mm guía gris |
| Espacio de color | sRGB | sRGB |
| Fuentes | Embebidas en PDF | Embebidas en PDF |
| QR | Vector (no rasterizado) | Vector (no rasterizado) |
| Páginas | 2 (frente/reverso) | 2 (frente/reverso) |

**Nota para el impresor:** PDF en sRGB. Convertir a CMYK con perfil FOGRA39 o equivalente al imprimir. Marcas de corte ya incluidas. En el gafete, el rectángulo punteado superior indica la ranura del lanyard.

## Foto del empleado (solo gafete)

El usuario sube su foto (JPG o PNG, idealmente vertical o cuadrada, mínimo 300×400px). La app la **recorta automáticamente** a proporción 3:4 portrait, la convierte a JPEG a 600×800 (2x retina) y la embebe en el PDF. Si el usuario no sube foto, el gafete usa el imago Aditum como fallback institucional.

La foto solo se guarda en memoria del navegador durante la sesión. Al cerrar la pestaña, se borra.

## Despliegue en GitHub Pages

1. Crear repositorio (recomendado: `aditum-consulting/cards-and-badges`).
2. Subir el contenido completo al repo.
3. Settings → Pages → Source: Deploy from a branch. Branch: `main`. Folder: `/ (root)`.
4. A los dos minutos: `https://[org].github.io/[repo]/`.

## Uso por el equipo

1. Abrir la URL pública.
2. Elegir **producto**: Tarjeta o Gafete.
3. Elegir **idioma**: ES o EN.
4. Llenar datos. Si el producto es Gafete, subir foto.
5. Click en `Descargar PDF para imprenta`.
6. Mandar el PDF al impresor.

Nombre del archivo: `aditum_tarjeta_[Nombre]_[es|en]_imprenta.pdf` o `aditum_gafete_[Nombre]_[es|en]_imprenta.pdf`.

## Puestos predefinidos

| Valor | ES | EN |
|---|---|---|
| `ba` | Analista de Negocios | Business Analyst |
| `ba-sr` | Analista de Negocios Sr. | Senior Business Analyst |
| `consultant` | Consultor | Consultant |
| `consultant-sr` | Consultor Sr. | Senior Consultant |
| `manager` | Gerente | Manager |
| `manager-sr` | Gerente Sr. | Senior Manager |
| `custom` | personalizado por el usuario | personalizado por el usuario |

## Estructura del repo

```
.
├── index.html                       App completa
├── assets/
│   ├── logo.png                     Logo navy wordmark
│   ├── logo_white.png               Logo wordmark blanco (preview)
│   ├── logo_print_white.png         Logo blanco hi-res para PDF
│   ├── imago.png                    Imago 160×160 app header
│   ├── imago_print.png              Imago 800×800 para PDF
│   ├── icon-mobile.png              Iconos smartphone, handset, sobre, globo
│   ├── icon-phone.png               (48×48 RGBA teal-deep)
│   ├── icon-mail.png
│   ├── icon-web.png
│   └── fonts/
│       ├── BarlowCondensed-SemiBold.ttf
│       ├── BarlowCondensed-Medium.ttf
│       ├── BarlowCondensed-Regular.ttf
│       ├── OpenSans-Regular.ttf
│       └── OpenSans-SemiBold.ttf
├── lib/
│   ├── qrcode.js                    Generador QR
│   ├── pdf-lib.min.js               Generador PDF
│   └── fontkit.min.js               Embedding de TTF en PDF
└── README.md
```

## QR vCard 3.0

Ambos productos llevan QR del reverso que codifica una vCard 3.0 completa:

```
BEGIN:VCARD
VERSION:3.0
FN:[Nombre]
ORG:Aditum Consulting
TITLE:[Puesto]
TEL;TYPE=CELL,VOICE:[Celular]
TEL;TYPE=CELL,VOICE:[Celular alterno]
TEL;TYPE=WORK,VOICE:[Teléfono fijo]
EMAIL;TYPE=WORK:[Correo]
URL:https://www.aditumconsulting.com
URL;TYPE=LinkedIn:[LinkedIn]
END:VCARD
```

QR generado como **vector** (no rasterizado) — escala perfectamente a cualquier resolución de imprenta. El QR del gafete es ~2× más grande que el de la tarjeta (28mm vs 15mm) porque la distancia de escaneo del gafete es mayor.

## Privacidad

LinkedIn solo en el QR, no visible en la cara impresa. La foto del gafete solo se procesa client-side, nunca sale del navegador del usuario.

## Licencia

Código de la app: uso interno Aditum.
qrcode-generator: MIT, Kazuhiko Arase.
pdf-lib: MIT, Andrew Dillon.
fontkit: MIT.
Barlow Condensed: SIL Open Font License 1.1.
Open Sans: SIL Open Font License 1.1, Google Fonts.
