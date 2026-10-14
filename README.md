# Winland · Módulo de verificación de cuenta

Prototipo funcional del rediseño del panel **Mi cuenta → Verificación**, con la
pestaña de **Preguntas frecuentes**.

Versión navegable: https://danielcastro93.github.io/winland-verificacion/

## Cómo está hecho

HTML, CSS y JavaScript sin framework, sin dependencias y sin build. Todo el
módulo vive en `index.html`: el CSS en su `<style>` y los estados en el
objeto `states` del script. Se abre con doble clic en cualquier navegador.

## Qué incluye

Quince estados que cubren el flujo completo:

| Estado | Pantalla |
|---|---|
| `inicial` | Usuario sin verificar |
| `truora-modal` | Modal de verificación biométrica (consentimiento, canal, enlace, QR) |
| `confirmando` | Regreso de la biometría, en espera de resultado |
| `exito-biometria` | Confirmación de biometría exitosa |
| `validada` | Identidad validada, documentos pendientes |
| `docs-validacion` | Documento enviado, en revisión |
| `pre-verificada` | Identidad y domicilio validados: retiro en sala física |
| `verificada` | Cuenta completa: retiro por depósito directo |
| `rechazado` | Un documento fue rechazado |
| `interrumpida` | Verificación biométrica interrumpida |
| `modal-identidad` | Carga de identificación y selfie |
| `modal-bancaria` | Carga de estado de cuenta |
| `modal-domicilio` | Carga de comprobante de domicilio |
| `modal-firma` | Formato de alta de cliente (firma digital) |
| `faq` | Preguntas frecuentes, por categoría, con guías visuales |

Las guías visuales (infografías) tienen versión de escritorio y de móvil; el
`<picture>` elige la que corresponde por viewport.

## Qué es solo demo y no va a producción

- Los botones **‹ ›** de abajo a la derecha y las flechas **← →** del teclado,
  que recorren los estados (`#stateNav`, `tourGo`, `TOUR_STEPS`).
- Las ligas **"Actualizar estado"**, que aquí avanzan el flujo a mano.
- La carga de archivos: un clic en la fila simula el archivo cargado.
- La validación manual de identidad se muestra resuelta al momento solo para
  poder recorrer el flujo.
- El **header, el footer y el menú inferior** son el marco visual de la demo
  (`assets/emotion.css`, `assets/sitio.css`, `assets/marco.css`). No forman parte
  del módulo. El módulo es el bloque marcado como `MÓDULO ENTREGABLE` en
  `index.html`, dentro de `<main class="wl-contenido">`.

## Estructura

```
index.html                           Prototipo completo, con la guía de integración comentada al inicio
docs/winland-verificacion.delta.css  El CSS del módulo, aislado, listo para cargarse como hoja propia
assets/sitio/                        Logo, fuente, íconos del marco y los 12 íconos de documento
assets/infografias/                  Guías visuales (escritorio y móvil, sufijo -movil)
assets/meta/                         Favicon e imagen de vista previa de la liga
assets/emotion.css, sitio.css, marco.css   Hojas del marco de la demo
```

## Para la integración

- **Clases.** `.wl-*` son componentes nuevos del módulo; su CSS completo está
  en `docs/winland-verificacion.delta.css`. Las clases `.clmc-*` siguen la
  nomenclatura del sitio y aquí solo llevan el ajuste visual del módulo.
- **Tokens.** Al inicio del `<style>`, en `:root`: el bloque A son variables
  `--clmc-*` ya existentes, con respaldo; el bloque B son variables nuevas del
  módulo que hay que dar de alta.
- **Backend.** Los puntos de conexión están marcados con `API:` en el código:
  estado de verificación del usuario, creación de la sesión biométrica, envío
  del enlace por WhatsApp o SMS, QR de la sesión, consulta del estado y carga
  de documentos. El backend debe exponer el nivel alcanzado
  (`none | pre-verified | verified`) para decidir qué estado renderizar.
- **Reglas de negocio.** Tres niveles de verificación; la cuenta bancaria no
  bloquea el retiro, es una mejora del método de cobro; domicilio y cuenta
  bancaria se habilitan al validar la identidad. Están detalladas en el
  comentario de cabecera de `index.html`.
