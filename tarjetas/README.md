# Aditum Card & Badge Generator · v6.0

Generador unificado de tarjetas de presentación, gafetes de credencial y tarjeta de pantalla para el equipo Aditum. App estática HTML/CSS/JavaScript que produce PDFs listos para imprenta y un PNG para el celular. Sin backend, sin build step, sin dependencias en runtime más allá de las librerías incluidas.

## Filosofía de diseño · caras de función separada

Cada pieza separa **marca** de **información**. La marca va en navy sólido con el lockup Aditum (imago + wordmark) solo, sin texto. La información va en blanco, ordenada en una retícula, con el nombre como protagonista tipográfico.

Las piezas no llevan marca de agua del imago: un imago fantasma detrás del imago del lockup, misma forma y mismo sentido, se lee como doble impresión o registro corrido.

## Sistema visual

- **Paleta:** navy `#0A2E5C`, teal-bright `#2A8B9F`, copper `#B8704A`, slate `#5A6B7D`, slate-light `#8895A4`, mist `#E8F0F2`.
- **Texto chico sobre navy:** tintes mínimos que pasan 4.5:1 (WCAG AA): slate `#8D99A8` y copper `#C58A6B`. Los originales dan 4.42:1 y 3.5:1 sobre navy.
- **Tipografía:** Barlow Condensed (SemiBold/Medium/Regular) + Open Sans (Regular/SemiBold), ambas OFL, embebidas en el PDF.
- **Puesto** siempre en versalitas con tracking. **Web** en copper con tracking en las piezas navy.
- **Tamaño mínimo de texto impreso:** 6 pt. Por debajo, Open Sans se empasta.

## Productos generados

### Tarjeta de presentación (90 × 50 mm landscape)

PDF de 2 páginas:

- **Cara A · marca:** navy sólido con el lockup blanco de 44 mm centrado.
- **Cara B · información:** retícula 2×2 sobre blanco.
  - Arriba a la izquierda, contacto con micro-iconos: celulares, teléfono fijo, correo y web. Solo aparecen los que tienen datos.
  - Arriba a la derecha, lockup a color.
  - Abajo a la izquierda, nombre (18 pt, se reduce hasta 11 pt para nombres largos) y puesto.
  - Abajo a la derecha, QR vCard de 22 mm.
  - La columna derecha se alinea a los **módulos visibles** del QR: el logo mide lo mismo que el QR, sus bordes derechos coinciden, y la base del QR coincide con la línea base del puesto. Sobre blanco el QR no lleva placa: la zona de silencio la dan el margen y el sangrado.

### Gafete de credencial (54 × 85.6 mm portrait, CR80)

PDF de 2 páginas. Los 9 mm superiores quedan libres en ambas caras para la perforación del lanyard.

- **Frente · identidad:** lockup a color arriba, foto de 30 × 40 mm (3:4, misma proporción que el recorte, sin deformar) con marco teal, nombre y puesto debajo.
- **Reverso · contacto:** navy con lockup blanco, nombre y puesto, QR vCard de 30 mm sobre placa blanca, microcopy "Escanea para añadir contacto" y web en copper.

El QR va en el reverso porque ahí cabe grande. En el frente, junto a la foto, solo cabía a 15 mm y quedaba demasiado denso para escanearse. Repetir nombre y puesto atrás cubre el caso más común de un gafete: que se voltee en el lanyard.

### Tarjeta de pantalla (vertical, para mostrar en el celular)

**PNG de 1080×1920** (full-HD vertical, ≈9:16), pensado para abrirse en el teléfono cuando conoces a alguien. Se abre a pantalla completa al instante, se comparte por chat y sirve de fondo de pantalla. No es para imprimir: no lleva sangrado. Hay también una descarga **PDF opcional** (74 × 132 mm) con el botón secundario.

- Fondo navy, imago arriba, nombre y puesto centrados, línea teal de acento.
- **QR vCard grande** como elemento dominante.
- Celular principal y correo como referencia rápida; si el correo es largo, se reduce para caber.
- Web en copper en el pie.

## Especificaciones de imprenta

| Parámetro | Tarjeta | Gafete |
|---|---|---|
| Tamaño final (TrimBox) | 90 × 50 mm | 54 × 85.6 mm (CR80) |
| Sangrado (BleedBox) | 3 mm por lado | 3 mm por lado |
| Página total | 96 × 56 mm | 60 × 91.6 mm |
| Imágenes | ≥ 300 DPI | ≥ 300 DPI |
| Marcas de corte | ninguna dibujada; corte en TrimBox | ninguna dibujada; corte en TrimBox |
| Ranura lanyard | — | la perfora el impresor; 9 mm superiores libres |
| Espacio de color | sRGB | sRGB |
| Fuentes | embebidas | embebidas |
| QR | 22 mm, vector | 30 mm en el reverso, vector |
| Páginas | 2 (A marca / B info) | 2 (frente / reverso) |

**Nota para el impresor:** PDF en sRGB con 3 mm de sangrado; convertir a CMYK con FOGRA39 o equivalente. El tamaño de corte va en el TrimBox del PDF y no lleva marcas dibujadas: se agregan en la imposición. En el gafete, perforar la ranura del lanyard al centro del borde superior con troquel estándar.

**Por qué no hay marcas dibujadas:** la página es exactamente trim + sangrado, así que las marcas no caben fuera del sangrado. Dentro de él, si el corte se corre, asoman como rayitas negras en la orilla de la tarjeta.

## Foto del empleado (solo gafete)

El usuario sube su foto (JPG o PNG, idealmente vertical, mínimo 300×400 px). La app la **recorta automáticamente** a 3:4, la convierte a JPEG de 600×800 y la embebe en el PDF a 30 × 40 mm (≈ 500 DPI). Sin foto, el marco muestra el imago Aditum en vez de una caja vacía.

La foto solo vive en la memoria del navegador durante la sesión. No se guarda con el perfil.

## Despliegue

La app vive en este repo, en la carpeta `tarjetas/`, servida por GitHub Pages en `https://aditum-consulting.github.io/firmas/tarjetas/`.

## Uso por el equipo

1. Abrir la URL pública.
2. Elegir **producto**: Tarjeta, Gafete o Pantalla.
3. Elegir **idioma**: ES o EN.
4. Llenar datos. Si el producto es Gafete, subir foto.
5. Click en `Descargar PDF para imprenta` (o `Descargar PNG para celular` en Pantalla).
6. Mandar el PDF al impresor. **Antes de imprimir un lote, escanear el QR con un celular** desde la pantalla o una prueba impresa.

Nombre del archivo: `aditum_tarjeta_[Nombre]_[es|en]_imprenta.pdf`, `aditum_gafete_[Nombre]_[es|en]_imprenta.pdf`, `aditum_pantalla_[Nombre]_[es|en].png`. Los acentos se transliteran (`María` → `Maria`).

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

## Estructura

```
tarjetas/
├── index.html                       App completa
├── assets/
│   ├── logo.png                     Lockup navy (preview)
│   ├── logo_white.png               Lockup blanco (preview)
│   ├── logo_print_color.png         Lockup a color hi-res para PDF (cara B, frente del gafete)
│   ├── logo_print_white.png         Lockup blanco hi-res para PDF (piezas navy)
│   ├── logo_print.png               Lockup hi-res (sin uso en el PDF actual)
│   ├── imago.png                    Imago 160×160, header y preview
│   ├── imago_print.png              Imago 1600×1600 para PDF y PNG
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

Todos los productos llevan un QR que codifica la vCard completa:

```
BEGIN:VCARD
VERSION:3.0
FN:[Nombre completo]
N:[Apellidos];[Nombres];;;
ORG:Aditum Consulting
TITLE:[Puesto]
TEL;TYPE=CELL:[Celular]
TEL;TYPE=CELL:[Celular alterno]
TEL;TYPE=WORK:[Teléfono fijo]
EMAIL;TYPE=WORK:[Correo]
URL:https://www.aditumconsulting.com
URL;TYPE=LinkedIn:[LinkedIn]
END:VCARD
```

`N` separa apellidos de nombres con una heurística para nombres hispanos: los dos últimos apellidos forman el apellido, y las partículas (de, del, la…) se pegan al apellido que les sigue. Solo afecta cómo ordena el teléfono el contacto; el nombre que se ve es `FN`.

### Densidad del QR

Una vCard completa pesa entre 300 y 450 bytes, lo que da un QR de 61 a 73 módulos. Cada byte de más agranda el QR y encoge sus módulos. Por eso:

- La vCard va **ligera**: sin `;VOICE` ni campos redundantes.
- La corrección de errores es **L**. En impresión limpia rinde más tener ~8 módulos menos (módulos más grandes) que la redundancia extra de M.
- El QR se dibuja como **un solo trazo vectorial**, no como un rectángulo por módulo. Los rectángulos sueltos dejan hilos blancos entre módulos al rasterizar, y eso confunde a los lectores.

| Pieza | Tamaño | Módulo (caso típico) | Módulo (caso extremo) |
|---|---|---|---|
| Tarjeta | 22 mm | 0.36 mm | 0.30 mm |
| Gafete (reverso) | 30 mm | 0.49 mm | 0.41 mm |
| Pantalla | 36 mm | 0.59 mm | 0.49 mm |

El caso extremo es un nombre de cuatro palabras, tres teléfonos con extensión, un correo de 47 caracteres y LinkedIn. Para impresión se suele recomendar un módulo de 0.3 mm o más.

### Por qué la vCard va dentro del QR y no en una URL

Un QR que apunte a una URL con la vCard alojada bajaría a unos 30 módulos. Se descartó por cuatro razones:

1. **Privacidad.** El repo es público. Alojar las vCards ahí publica los celulares de todo el equipo en un directorio navegable e indexable.
2. **Autoservicio.** El generador corre en el navegador y no puede escribir al repo. Cada tarjeta nueva requeriría un commit.
3. **Experiencia.** En Android, un `.vcf` por URL se descarga como archivo; una vCard embebida abre directo "Añadir contacto". Además, la versión por URL necesita datos móviles al escanear.
4. **Permanencia.** Una tarjeta impresa no se actualiza. Si la URL se cae, todas las tarjetas repartidas quedan con un QR muerto.

Con los tamaños de arriba, la vCard embebida ya queda en módulos de 0.3 mm o más, así que el hosting no hace falta.

## Privacidad

LinkedIn solo va en el QR, no visible en la cara impresa. La foto del gafete solo se procesa en el navegador y nunca sale de él.

## Licencia

Código de la app: uso interno Aditum.
qrcode-generator: MIT, Kazuhiko Arase.
pdf-lib: MIT, Andrew Dillon.
fontkit: MIT.
Barlow Condensed: SIL Open Font License 1.1.
Open Sans: SIL Open Font License 1.1, Google Fonts.
