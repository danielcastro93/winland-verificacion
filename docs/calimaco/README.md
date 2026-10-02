# Entrega técnica · Centro de verificación de Winland

Para el equipo de Calímaco. 1 de octubre de 2026.

Este módulo es un **rediseño de algo que ya existe**, no una construcción desde cero.
La pantalla de hoy (`/privado/perfil#/overview`, pestaña Verificación) ya trae el
bloque de Truora, la rejilla de documentos y el diálogo de carga. La mayor parte de
lo que sigue es reestilizar eso. Lo verdaderamente nuevo son cinco componentes, y
están marcados como tales.

## Qué hay en esta carpeta

| Archivo | Para qué |
|---|---|
| `README.md` | este documento |
| `winland-verificacion.delta.css` | el CSS a sumar |
| `markup-produccion.md` | el markup actual del sitio, capturado con sesión iniciada, contra el que se homologó todo |

Y en la raíz, `index.html`: el prototipo navegable con los 15 estados del flujo,
incluidos los caminos de error. Abre con doble clic, no necesita servidor ni
dependencias externas.

## Convención de nombres

Para que se distinga de un vistazo qué tocar y qué construir:

- **`.clmc-*`** — el componente ya existe en Calímaco. El delta solo ajusta lo
  necesario y se apoya en la base. Son 14.
- **`.wl-*`** — componente nuevo. Hay que construirlo. Son 75, pero casi todos son
  piezas internas de los cinco bloques nuevos.

No se inventó ni un solo nombre `clmc-`: todos salieron de `base.css`,
`components.css` y `profile.css` del sitio.

## Paso 1 · Tokens

Agregar a `/assets/css/client_custom.css`. El resto de la paleta ya la hereda del
que ya existe, así que un cambio de marca se propaga solo.

```css
:root {
	--wl-orange-dark: #c2480c;   /* hover del primario */
	--wl-orange-tint: #fce9dd;   /* fondo suave de marca */
	--wl-ink: #1f1f1f;           /* texto principal */
	--wl-page-bg: #f4f6f7;       /* ≡ fondo de .clmc-profile-menu */
	--wl-line: #e4e7ea;
	--wl-line-soft: #eceef0;
	--wl-text-gray: #5b6470;     /* texto secundario */
	--wl-text-faint: #8a8a8a;    /* texto terciario */
	--wl-success-bg: #e7f5ea;
	--wl-warning-bg: #fff1e5;
	--wl-error-bg: #fbeae8;
	--wl-info-bg: #e9f1fb;
	--wl-radius-lg: 16px;        /* ≡ borde de .privateLayout-content */
}
```

Una decisión que conviene no revertir sin querer: **el texto principal es `#1F1F1F`,
no `--clmc-color-tertiary` (`#000`)**. En bloques largos el negro puro endurece la
lectura. Es intencional.

## Paso 2 · El delta de CSS

Pegar `winland-verificacion.delta.css` al final de `profile.css`, o cargarlo como
hoja propia **después** de `base.css`, `components.css`, `responsive.css` y
`profile.css`. El orden no es negociable: varias reglas asumen la base ya aplicada.

Ejemplo de cómo está escrito el delta, para que se entienda el criterio:

```css
/* .clmc-btn-primary — DELTA.
   Producción ya define fondo, color, alto, min-width, radio, peso, tamaño de
   fuente, transición, line-height y el hover (base.css 467-531). */
.clmc-btn-primary{
  padding:0 40px;cursor:pointer;width:auto;
  display:inline-flex;align-items:center;justify-content:center;gap:6px;
  font-family:inherit;letter-spacing:-0.005em;
}
```

Nada de ahí pisa a Calímaco. En el sitio esas propiedades las aporta MUI sobre un
`<Button>`; en el prototipo son `<button>` planos y hay que darlas a mano. **Al
integrar en React, estas reglas de acomodo probablemente sobran.**

## Paso 3 · Componentes

### Ya existen, solo cambia el estilo

| Clase | Qué cambia |
|---|---|
| `clmc-user-documents-grid` | rejilla y espaciado |
| `clmc-user-document-item` | tipografía, estados y zona accionable |
| `clmc-user-document-icon` | tamaño y color por estado |
| `clmc-upload-document-drop-area` | zona de carga |
| `clmc-upload-documents-grid` · `-actions` · `-format-box` | acomodo del diálogo |
| `clmc-dialog` · `clmc-closeIcon-popup` | encabezado y cierre del diálogo |
| `clmc-btn-primary` · `-primary-white` · `clmc-btn-disabled` | solo acomodo interno |
| `clmc-card` · `clmc-profile-menu` | contenedores |

Un cambio de orden, sin componente nuevo: la rejilla pasa de
**Email · Identidad · Cuenta bancaria · Domicilio** a
**Email · Identidad · Domicilio · Cuenta bancaria**, porque el domicilio ahora es
el paso 2 y la cuenta bancaria el 3.

### Nuevos

| Componente | Qué es |
|---|---|
| `wl-tracker` · `wl-step` | barra de 3 pasos arriba del módulo |
| `wl-status-banner` | franja con el estado de la cuenta |
| `wl-upgrade-card` | tarjeta de mejora del estado pre-verificada |
| `wl-alta-toggle` | acordeón de alta de cliente con firma digital |
| `wl-faq-*` | pestaña de preguntas frecuentes |

La pestaña de preguntas frecuentes **no existe hoy**. Las subpestañas actuales del
perfil son Cuenta · Verificación · Contraseña; esta sería una cuarta, y es lo único
de la entrega que necesita una ruta nueva.

Su contenido debe quedar **administrable desde el backoffice**: las preguntas cambian
seguido y Winland quiere editarlas sin pedir despliegue. El componente ya está hecho
para no asumir cuántas preguntas hay ni qué tan largas son. Hoy son 24 en 6
categorías, y cinco respuestas abren una infografía, así que el campo de respuesta
necesita admitir una imagen asociada, no solo texto.

## Paso 4 · Lo que el backend tiene que exponer

El flujo tiene **tres niveles, no dos**. De aquí sale qué pantalla se pinta y qué
métodos de cobro se ofrecen:

| Nivel | Condición | Qué puede hacer |
|---|---|---|
| Sin verificar | nada aprobado | no retira |
| **Pre-verificada** | **identidad aprobada Y domicilio aprobado** | cobra **en sala física** |
| Verificada | lo anterior + cuenta bancaria aprobada | cobra por **depósito directo (SPEI)** |

La condición de pre-verificada es **identidad + domicilio**. La cuenta bancaria **no
bloquea** el retiro: es una mejora del método de cobro, y así se comunica en la
interfaz (paso punteado y tarjeta de mejora, nunca candado de error).

Hace falta un campo con el nivel alcanzado, por ejemplo `none | pre-verified | verified`.

Otra regla ya implementada en el prototipo: mientras la biometría de Truora está en
proceso, **la carga manual de identidad queda deshabilitada** unos minutos, para que
no entren documentos duplicados. El domicilio y el estado de cuenta siguen abiertos.

## Capa visual v2

Al final del delta hay un bloque marcado `V2 · MODERNIZACIÓN VISUAL`. Reestiliza
sin tocar contenido, con un criterio único: la caja se reserva para lo que es una
unidad accionable y lo demás se resuelve con listas y divisores.

Lo que conviene saber al integrar:

- **Filas de documento.** Dejan de ser tarjetas y pasan a lista con divisores. Las
  validadas llevan una palomita al final (`.wl-row-ok`). Producción ya pinta una ahí
  con el `CheckIcon` relleno de MUI; para que case con la familia nueva conviene
  sustituirlo por el SVG de línea del prototipo (`I.check`).
- **Iconografía.** Una sola familia de línea: 24×24, trazo 1.8, puntas y uniones
  redondeadas, `stroke="currentColor"`. Las únicas excepciones son el logo de
  WhatsApp, el de Truora y el disco de éxito del pop-up biométrico.
- **Tracker.** En móvil es una barra de tres segmentos con la etiqueta del paso que
  pide atención; en escritorio, la misma barra con las tres etiquetas debajo. Es el
  mismo markup en ambos casos.
- **Requisitos de cada documento.** Antes eran una línea separada por puntos; ahora
  son una lista (`.wl-req-line ul > li`). Mismo texto.
- **Zona de carga.** Borde sólido de 1px en lugar del punteado.

## Puntos abiertos

1. **RFC.** El diálogo de identidad en producción lo lista como tercer bloque de
   requisitos, pero el prototipo no lo incluye y **se decidió dejarlo así** (1 de
   octubre de 2026): el cliente revisó y autorizó este diseño sin señalar la falta, y
   hay cuentas verificadas sin RFC cargado. Queda como posible incorporación
   posterior. Si se retoma, hay que definir primero si es obligatorio para completar
   la identidad, porque de eso depende la habilitación del botón de envío.
2. **Íconos de documento: hay que subir los doce archivos.** En
   `/static/images/icons/verificacion/` el servidor hoy solo responde a los cuatro
   `*_verified.svg`. Los `*_pending.svg` devuelven **404**, lo que probablemente deja
   un ícono roto a cualquier usuario con un documento en revisión. Conviene revisarlo
   aparte de este entregable, porque ya está en producción.

   Como esa carpeta hay que tocarla de todos modos, los doce se **redibujaron** en la
   familia de línea del módulo, con los **mismos nombres de archivo**: `email_`,
   `identify_`, `bank_` y `other_`, cada uno en `_verified` (`#2E7D32`), `_pending`
   (`#E85B10`) y `_denied` (`#D32F2F`, ≡ `--clmc-bg-color-error`, que cubre la clase
   `clmc-user-document-icon-denied` que ya existía en el CSS sin archivo). Basta
   reemplazarlos; ningún componente cambia. Producción ya los dibuja a 22×22 y los
   nuevos son vectoriales a 24×24, así que escalan sin ajuste.

   Los doce están en `assets/produccion/`. Los originales de producción quedaron
   respaldados fuera del repositorio.
3. **Infografías.** Las que trae el prototipo son las vigentes. Winland tiene
   pendiente enviar una versión corregida.
4. **Firma digital.** El botón del acordeón de alta de cliente está maquetado, pero
   el proveedor y el flujo de firma siguen sin definirse.
