# Aditum Signature Generator · v2.0

Generador de firmas de correo institucionales para el equipo Aditum. App estática en HTML/CSS/JavaScript puro, sin backend, sin build step, sin dependencias externas en runtime.

## Qué genera

Una firma de correo en HTML lista para pegar en Outlook, Gmail o Apple Mail. La firma v2 tiene tres bloques verticales:

1. **Banner superior:** logotipo completo Aditum Consulting (298×55px) alineado a la izquierda.
2. **Cuerpo a dos columnas:** a la izquierda, nombre, puesto, mini-regla teal, datos de contacto (M./E./W.); a la derecha, código QR 90×90 con microetiqueta bilingüe debajo.
3. **Disclaimer institucional:** ancho completo, adaptado al idioma seleccionado, cubriendo Aditum Consulting Group S.A. de C.V. y Aditum Consulting LLC.

Una regla horizontal teal-deep de ancho completo separa el banner del cuerpo. Una mini-regla teal corta (60px) separa el bloque nombre/puesto del bloque de contacto. Total: 460px de ancho, ~280px de alto.

## Características

- **Bilingüe ES/EN:** toggle único que cambia puesto, etiquetas del formulario, microetiqueta del QR y disclaimer.
- **Puestos predefinidos** para Business Analyst, Consultant y Manager (regular y senior), más opción personalizada con campos ES/EN independientes.
- **Tres campos de teléfono opcionales:** celular principal, celular alterno, teléfono fijo. Solo aparecen en la firma los que tienen datos llenados.
- **Iconos de contacto** discretos en color teal-deep (smartphone, handset, sobre, globo) en lugar de etiquetas de texto.
- **QR vCard 3.0** que codifica nombre, organización, puesto, todos los teléfonos llenados, correo, sitio web y LinkedIn opcional.
- **LinkedIn no visible:** se incluye solo en el QR para evitar exposición en cadenas de correo reenviadas.
- **Microetiqueta del QR** debajo del código, adaptada al idioma seleccionado (ES o EN).
- **Disclaimer monolingüe** según idioma activo, ambas entidades legales nombradas en cada versión.

## Despliegue en GitHub Pages

1. Crear repositorio en GitHub, por ejemplo `aditum-consulting/signatures`.
2. Subir el contenido de esta carpeta al repositorio (drag-and-drop o `git push`).
3. En el repo: Settings → Pages → Source: Deploy from a branch. Branch: `main`. Folder: `/ (root)`. Save.
4. Esperar uno o dos minutos. La app queda en `https://[org].github.io/[repo]/`.

La URL del logotipo está hard-codeada en el bloque `CONFIG` al inicio del `<script>` de `index.html`, apuntando a `https://aditum-consulting.github.io/firmas/firmas/assets/logo.png`. No se deriva de `window.location` a propósito: esa URL queda horneada dentro de cada firma que el equipo pega en su cliente de correo, así que si se calculara desde el origen actual, abrir el generador desde `file://`, un fork o `localhost` produciría firmas que nacen rotas.

### Regla de estabilidad de assets

`assets/logo.png` sostiene firmas de correo **ya repartidas y en uso**, con una vida útil de años. En consecuencia:

- No renombrar, mover ni borrar `assets/logo.png`.
- No renombrar el repositorio ni la organización, y no pasar el repo a privado: apaga GitHub Pages y rompe retroactivamente las firmas de todo el equipo.
- Para rediseñar la firma, publicar el asset nuevo en una ruta nueva y **dejar la vieja en su lugar**.

El generador incluye una sonda que verifica la URL del logo al cargar. Si deja de responder, aparece una barra de advertencia antes de que nadie copie una firma rota.

Migrar a un dominio propio (`aditumconsulting.com`) es el siguiente paso natural y elimina la dependencia de GitHub. No es urgente: al configurar un dominio custom, GitHub redirige las URLs `github.io` con 301, así que las firmas ya repartidas siguen cargando.

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
│   ├── imago.png        Imago Aditum 160×160 RGBA, usado en el header de la app
│   ├── icon-mobile.png  Icono smartphone, 48×48 RGBA, displayed 14×14
│   ├── icon-phone.png   Icono handset (fijo), 48×48 RGBA, displayed 14×14
│   ├── icon-mail.png    Icono sobre, 48×48 RGBA, displayed 14×14
│   └── icon-web.png     Icono globo, 48×48 RGBA, displayed 14×14
├── lib/
│   └── qrcode.js        qrcode-generator v1.4.4 (MIT, Kazuhiko Arase)
└── README.md            Este archivo
```

Los cuatro `icon-*.png` viven además **copiados en base64** dentro de la constante `ICONS` de `index.html`. Los `.png` son la fuente de verdad, pero editarlos no cambia nada por sí solo: hay que re-inyectar el base64. Desde `firmas/`:

```bash
node -e 'const fs=require("fs");const m={mobile:"icon-mobile.png",phone:"icon-phone.png",mail:"icon-mail.png",web:"icon-web.png"};let s=fs.readFileSync("index.html","utf8");for(const[k,f]of Object.entries(m)){const d="data:image/png;base64,"+fs.readFileSync("assets/"+f).toString("base64");s=s.replace(new RegExp("(\\n\\s*"+k+": )\x27data:image/png;base64,[^\x27]*\x27"),"$1\x27"+d+"\x27")}fs.writeFileSync("index.html",s)'
```

## Sistema de diseño · v2

| Elemento | Tipografía | Tamaño | Color |
|---|---|---|---|
| Logotipo banner | — (asset PNG) | 298×55px | gradient navy/teal/copper |
| Regla horizontal larga | — | 1px × 460px | teal-deep `#1A6B7A` |
| Nombre | Bahnschrift SemiBold | 14pt | navy `#0A2E5C` |
| Puesto | Bahnschrift Condensed | 10.5pt | slate `#5A6B7D` |
| Mini-regla | — | 1px × 60px | teal-deep `#1A6B7A` |
| Iconos de contacto | — (asset PNG) | 12×12px | teal-deep `#1A6B7A` |
| Datos de contacto | Segoe UI | 9.5pt | slate `#5A6B7D` |
| Código QR | — (generado) | 130×130px | navy/blanco |
| Microetiqueta QR | Bahnschrift Condensed | 7pt | slate-light `#8895A4` |
| Disclaimer | Segoe UI | 7.5pt | slate-light `#8895A4` |

Stack de fuentes con fallback robusto: `'Bahnschrift', 'Arial Narrow', Arial` para Bahnschrift Condensed; `'Segoe UI', Arial` para Segoe UI; `'Consolas', 'Courier New'` para Consolas. Bahnschrift es nativa en todo Windows; en macOS cae a Segoe UI o Arial preservando jerarquía.

## Compatibilidad de clientes de correo

Probado en:

- Outlook 365 desktop (Windows): tabla raíz con estilos inline, sin clases CSS, sin `border-radius`.
- Outlook web: idem.
- Gmail web y móvil: render nativo correcto.
- Apple Mail (macOS, iOS): import directo del `.html` descargado.

### Dependencias remotas de la firma

La firma emitida contiene **una sola** imagen remota: el logotipo. Todo lo demás va embebido en base64 (QR ~2KB, y los cuatro iconos de contacto, de 0.3 a 2KB cada uno). El logotipo se queda por URL porque Outlook desktop a veces descarta imágenes inline base64 grandes; los assets chicos tienen compatibilidad estable, así que embeberlos reduce de cinco descargas remotas a una.

El `<img>` del logotipo lleva estilos de tipografía y color además de los de tamaño. Eso aplica al texto alternativo: si el cliente bloquea la imagen, la firma degrada a un wordmark "Aditum Consulting" en navy en vez de a un icono roto.

### Pegar en Outlook desktop embebe el logotipo

Al pegar la firma como HTML enriquecido en el editor de firmas de Outlook desktop, Outlook descarga las imágenes remotas y guarda una copia local en `%APPDATA%\Microsoft\Signatures\<nombre>_files\`. A partir de ese momento esa firma ya no depende de la URL. Por eso conviene pegar en el editor de firmas y no guardar el `.html` suelto. En Gmail y Outlook web el logotipo permanece remoto.

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
