# Aditum Signature Generator · v2.0

Generador de firmas de correo institucionales para el equipo Aditum. App estática en HTML/CSS/JavaScript puro, sin backend, sin build step, sin dependencias externas en runtime.

## Qué genera

Una firma de correo en HTML lista para pegar en Outlook, Gmail o Apple Mail. La firma v2 tiene tres bloques verticales:

1. **Banner superior:** logotipo completo Aditum Consulting (298×55px) alineado a la izquierda.
2. **Cuerpo a dos columnas:** a la izquierda, nombre, puesto, mini-regla teal, datos de contacto (M./E./W.); a la derecha, código QR 90×90 con microetiqueta bilingüe debajo.
3. **Disclaimer institucional:** ancho completo, adaptado al idioma seleccionado, cubriendo Aditum Consulting Group S.A. de C.V. y Aditum Consulting LLC.

Una regla horizontal teal-deep de ancho completo separa el banner del cuerpo. Una mini-regla teal corta (60px) separa el bloque nombre/puesto del bloque de contacto. Total: 460px de ancho, ~280px de alto.

## Características

- **Bilingüe ES/EN:** toggle único que cambia puesto, etiquetas del formulario y disclaimer.
- **Puestos predefinidos** para Business Analyst, Consultant y Manager (regular y senior), más opción personalizada con campos ES/EN independientes.
- **QR vCard 3.0** que codifica nombre, organización, puesto, celular, correo, sitio web y LinkedIn opcional.
- **LinkedIn no visible:** se incluye solo en el QR para evitar exposición en cadenas de correo reenviadas.
- **Microetiqueta del QR** debajo del código, adaptada al idioma seleccionado (ES o EN).
- **Disclaimer monolingüe** según idioma activo, ambas entidades legales nombradas en cada versión.

## Despliegue en GitHub Pages

1. Crear repositorio en GitHub, por ejemplo `aditum-consulting/signatures`.
2. Subir el contenido de esta carpeta al repositorio (drag-and-drop o `git push`).
3. En el repo: Settings → Pages → Source: Deploy from a branch. Branch: `main`. Folder: `/ (root)`. Save.
4. Esperar uno o dos minutos. La app queda en `https://[org].github.io/[repo]/`.

Las URLs del logotipo y del imago se calculan automáticamente desde el origen, así que no hace falta editar código al desplegar. Para hard-codear URLs fijas de producción, editar el bloque `CONFIG` al inicio del `<script>` en `index.html`.

## Uso por el equipo

1. Abrir la URL pública del repo desplegado.
2. Elegir idioma (ES o EN).
3. Llenar nombre, correo, puesto, celular. Opcionalmente LinkedIn.
4. Click en `Copiar HTML`.
5. Pegar en el editor de firmas:
   - **Outlook desktop:** Archivo → Opciones → Correo → Firmas → seleccionar firma o crear nueva → pegar en el editor (modo HTML).
   - **Outlook web:** Configuración → Correo → Redactar y responder → pegar en el cuadro de firma.
   - **Gmail:** Configuración (rueda dentada) → Ver toda la configuración → General → Firma → crear nueva → pegar.
   - **Apple Mail:** Mail → Preferencias → Firmas → crear firma → arrastrar el `.html` descargado.

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

Para añadir más puestos, editar el objeto `TITLES` en `index.html` y agregar la opción en el `<select id="title">`.

## Estructura del repo

```
.
├── index.html           App completa: formulario, preview en vivo, lógica
├── assets/
│   ├── logo.png         Logotipo Aditum Consulting 895×165 RGBA, displayed 298×55
│   └── imago.png        Imago Aditum 160×160 RGBA, usado en el header de la app
├── lib/
│   └── qrcode.js        qrcode-generator v1.4.4 (MIT, Kazuhiko Arase)
└── README.md            Este archivo
```

## Sistema de diseño · v2

| Elemento | Tipografía | Tamaño | Color |
|---|---|---|---|
| Logotipo banner | — (asset PNG) | 298×55px | gradient navy/teal/copper |
| Regla horizontal larga | — | 1px × 460px | teal-deep `#1A6B7A` |
| Nombre | Bahnschrift SemiBold | 16pt | navy `#0A2E5C` |
| Puesto | Bahnschrift Condensed | 11.5pt | slate `#5A6B7D` |
| Mini-regla | — | 1px × 60px | teal-deep `#1A6B7A` |
| Etiquetas M./E./W. | Consolas | 9.5pt | teal-deep `#1A6B7A` |
| Datos de contacto | Segoe UI | 10.5pt | slate `#5A6B7D` |
| Código QR | — (generado) | 90×90px | navy/blanco |
| Microetiqueta QR | Bahnschrift Condensed | 7pt | slate-light `#8895A4` |
| Disclaimer | Segoe UI | 8pt | slate-light `#8895A4` |

Stack de fuentes con fallback robusto: `'Bahnschrift', 'Arial Narrow', Arial` para Bahnschrift Condensed; `'Segoe UI', Arial` para Segoe UI; `'Consolas', 'Courier New'` para Consolas. Bahnschrift es nativa en todo Windows; en macOS cae a Segoe UI o Arial preservando jerarquía.

## Compatibilidad de clientes de correo

Probado en:

- Outlook 365 desktop (Windows): tabla raíz con estilos inline, sin clases CSS, sin `border-radius`.
- Outlook web: idem.
- Gmail web y móvil: render nativo correcto.
- Apple Mail (macOS, iOS): import directo del `.html` descargado.

El logotipo se referencia por URL pública del repo, no como base64, porque Outlook desktop a veces descarta imágenes inline base64 grandes. El QR sí va embebido base64 porque su tamaño es chico (~2KB) y la compatibilidad es estable.

## Privacidad

El LinkedIn que el usuario introduce no aparece visible en el cuerpo de la firma. Solo se incluye dentro del código QR, que el destinatario debe escanear voluntariamente para ver. Esto evita que el LinkedIn quede expuesto en cadenas de correo reenviadas o screenshots.

## Cambios v1 → v2

- Layout reorganizado de dos columnas (imago+QR a la izquierda, texto a la derecha) a banner superior con logotipo completo + cuerpo a dos columnas debajo.
- QR reducido de 100×100 a 90×90, ahora con microetiqueta bilingüe debajo.
- Imago aislado reemplazado por logotipo completo Aditum Consulting (gradient imago + wordmark) en el banner.
- Mini-regla corta (60px) reemplaza la regla completa de columna en el bloque de contacto.
- Ancho total de la firma reducido de 520 a 460px.

## Licencia

Código de la app: uso interno Aditum.
qrcode-generator: MIT, Kazuhiko Arase.
