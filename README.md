# Divide la cuenta

Herramienta web para calcular cuánto paga cada persona al dividir una cuenta, con propina incluida. Es un proyecto demo para mostrar la característica de calidad **Usabilidad** del modelo **ISO/IEC 25010**.


**Estado:** en desarrollo.

---

## Idea

Dividir una cuenta es una tarea cotidiana que suele hacerse con prisa y desde el celular. La idea es que cualquier persona, sin instrucciones, pueda escribir el total, elegir cuántas son y ver el resultado al instante.

## Característica de calidad elegida

**Usabilidad (ISO/IEC 25010)**, con estas subcaracterísticas:

| Subcaracterística | Cómo se aplica en el proyecto |
|---|---|
| Capacidad de reconocimiento de la idoneidad | El título y un texto breve explican para qué sirve la herramienta. |
| Capacidad de aprendizaje | Un solo flujo: total, personas, propina. Los campos traen ejemplos de formato. |
| Operabilidad | Resultado en vivo (sin botón "Calcular"), botones de 44 px o más, uso completo con teclado y foco visible. |
| Protección contra errores del usuario | Los errores explican cómo corregir ("Escribe solo números, mayores que 0. Ejemplo: 45000 o 45,50."). Se limita el número de personas de 1 a 50. |
| Accesibilidad | Etiquetas asociadas a cada campo, mensajes con `role="alert"`, resultado con `aria-live`, texto ajustable, modo de alto contraste y tema claro u oscuro según el sistema. |

## Qué hice para demostrarla

1. **Diseñé la interfaz con criterios de usabilidad:** etiquetas visibles, ejemplos de formato, errores claros, resultado inmediato y tipografía pensada para la legibilidad (Atkinson Hyperlegible).
2. **Agregué opciones de accesibilidad:** botones A− y A+ para el tamaño del texto, interruptor de alto contraste y soporte de modo oscuro.
3. **Incluí un panel de calidad dentro de la misma página** que verifica automáticamente los requisitos de abajo. La página se prueba a sí misma con la pantalla real de quien la usa.

## Requisitos comprobables

| ID | Requisito | Cómo se verifica |
|---|---|---|
| RU-1 | Una persona nueva completa la tarea principal (ingresar el total y ver el resultado) en **30 s o menos**. | El cronómetro inicia en la primera interacción con el formulario y se detiene cuando aparece el primer resultado válido. El panel muestra el tiempo medido. |
| RU-2 | El texto tiene un contraste de **al menos 4,5:1** (WCAG AA). | El panel calcula la razón de contraste de cada elemento de texto contra su fondo y reporta el peor caso. |
| RU-3 | Los botones y campos miden **al menos 44×44 px**. | El panel mide con `getBoundingClientRect()` todos los controles y cuenta los que no cumplen. |
| RU-4 | **Todo campo tiene una etiqueta** asociada. | El panel revisa que cada `input` tenga `label` o `aria-label`. |
| RU-5 | Una entrada inválida produce **un mensaje que explica cómo corregirla**. | El panel prueba `"abc"`, `"-5"` y `0 personas` (deben dar mensaje) y `"45,50"` (debe aceptarse). |

Cada requisito aparece como **Cumple**, **No cumple** o **Pendiente**, con texto y no solo con color.

### Verificación independiente (opcional)

- Contraste y etiquetas: **Lighthouse** (pestaña Accessibility de Chrome DevTools) o la extensión **axe DevTools**.
- Tamaño de objetivos táctiles: DevTools, pestaña Elements, con el inspector de cajas.
- Teclado: ver la prueba manual más abajo.

## Cómo ejecutarlo

No necesita instalación ni dependencias.

1. Descarga o clona este repositorio.
2. Abre `divide-la-cuenta.html` en cualquier navegador moderno (Chrome, Firefox, Edge, Safari).

Opcional, con un servidor local:

```bash
python3 -m http.server 8000
# luego abre http://localhost:8000/divide-la-cuenta.html
```

La fuente Atkinson Hyperlegible se carga desde Google Fonts. Sin internet, la página usa la fuente del sistema y funciona igual.

## Prueba manual sugerida

1. Escribe `45000` en el total y confirma que el resultado aparece sin presionar nada.
2. Escribe `abc` y revisa que aparezca un mensaje que explique cómo corregirlo.
3. Navega solo con el teclado: `Tab` para avanzar, flechas para cambiar la propina, `Enter` o `Espacio` en los botones.
4. Presiona **A+** varias veces y revisa que nada se corte ni se superponga.
5. Activa **Alto contraste** y ejecuta las pruebas del panel.
6. Cambia el tema del sistema a oscuro y recarga la página.

## Estructura

```
.
├── divide-la-cuenta.html   # HTML, CSS y JavaScript en un solo archivo
└── README.md
```

Tecnologías: HTML, CSS y JavaScript sin librerías.

## Cómo funciona (resumen técnico)

- `validateTotal()` y `validatePeople()` son funciones puras que validan la entrada y devuelven el valor numérico o un mensaje de error. Se usan tanto en la interfaz como en las pruebas (RU-5).
- `update()` recalcula todo cada vez que cambia un campo: valida, muestra errores, calcula `total × (1 + propina) ÷ personas` y actualiza el panel de calidad.
- `runChecks()` ejecuta las pruebas RU-1 a RU-5 y `renderTests()` las muestra.
- Los colores son variables CSS (`--bg`, `--text`, `--primary`...) que cambian según el tema claro, oscuro o de alto contraste.

## Limitaciones y próximos pasos

- RU-1 mide el tiempo de quien está usando la página en ese momento, así que no reemplaza una prueba con varias personas.
- La prueba de contraste revisa los colores calculados del texto frente al fondo del contenedor. No analiza imágenes, porque la página no tiene.
- No se probó aún con lectores de pantalla como NVDA, VoiceOver o TalkBack.
- Siguientes pasos: probar con 5 usuarios reales, permitir montos distintos por persona e incluir selector de moneda.

## Uso de IA

Este proyecto se construyó con ayuda de IA (Claude). El código, las decisiones de diseño y los requisitos están explicados arriba para poder defenderlos y modificarlos.