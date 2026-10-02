# Markup real de producción — módulo de verificación

Capturado el 1 de octubre de 2026 desde `winland.com.mx/privado/perfil#/overview`,
pestaña **Verificación**, con sesión iniciada. Los datos personales están sustituidos.

Este archivo es la referencia contra la que se homologan las clases del prototipo.
No se inventó ningún nombre: todo lo que aparece aquí existe hoy en el sitio.

---

## Stack confirmado

| Capa | Qué es |
|---|---|
| Framework | Next.js (Pages Router). Confirmado por `__NEXT_DATA__`, `#__next` y `/_next/static/chunks/` |
| UI | React con MUI v5 y Emotion (`css-*` generadas en runtime) |
| CSS de plataforma | `/assets/css/base.css`, `components.css`, `responsive.css`, `profile.css` — encabezado `CALIMACO CSS FRAMEWORK` |
| CSS de marca | `/assets/css/client_custom.css` — solo tokens `--clmc-*` |

Las hojas de `/assets/css/` se sirven desde el dominio de Winland y son CSS plano,
editable. Esa es la superficie por la que entra nuestro diseño.

---

## Jerarquía de la pantalla

```
.privateLayout-content
├── .clmc-profile-menu                     menú lateral (Mi cuenta · Historial · Bonos · Juego responsable)
├── .MuiTabs-root.clmc-tabs                subpestañas (Cuenta · Verificación · Contraseña)
└── .clmc-p-large
    └── .MuiCard-root.clmc-card#userDocuments
        └── .MuiCardContent-root.verification-documents-card-content
            ├── .clmc-bg-tertiary…         bloque Truora
            ├── .text-line-background      separador "o verifica manualmente"
            ├── .clmc-card-content-buttons
            │   └── .clmc-user-documents-grid
            │       └── .clmc-user-document-item ×4
            └── .clmc-user-document-item   bloque "Verificación por correo electrónico"
```

### Bloque de Truora

```html
<div class="clmc-bg-tertiary clmc-p-medium clmc-border-radius-small clmc-mb-large">
  <div class="clmc-flex clmc-row clmc-gap-medium">
    <div class="clmc-btn-primary clmc-btn-icon clmc-flex clmc-align-center clmc-justify-center">
      <img class="clmc-p-xsmall" alt="Truora" src="/static/images/icons/truora-icon-black.svg">
    </div>
    <div class="clmc-flex clmc-column clmc-align-start clmc-justify-center">
      <p class="clmc-recommended-title-whatsapp clmc-color-secondary clmc-text-small clmc-text-500 clmc-text-bold clmc-mb-small">Recomendado</p>
      <p class="clmc-text-bold clmc-color-secondary clmc-mb-small">Verificación rápida por Truora</p>
      <p class="clmc-text-xsmall clmc-color-secondary">Proceso guiado en minutos</p>
    </div>
  </div>
  <button class="… clmc-btn-primary-white clmc-w100 clmc-mt-medium">Iniciar verificación</button>
</div>
```

### Tarjeta de documento (el patrón que más se repite)

```html
<button type="button"
        class="clmc-user-document-item clmc-user-document-button clmc-user-document-clickable
               clmc-flex clmc-row clmc-align-center clmc-justify-between
               clmc-p-medium clmc-border-radius-small"
        aria-disabled="false" tabindex="0">
  <div class="clmc-flex clmc-row clmc-align-center clmc-gap-small">
    <div class="clmc-mr-small clmc-ml-small clmc-user-document-icon clmc-color-success">
      <img class="clmc-user-document-icon-img clmc-user-document-icon-approved"
           alt="Identidad" src="/static/images/icons/verificacion/identify_verified.svg">
    </div>
    <div class="clmc-flex clmc-column">
      <p class="clmc-text-bold clmc-color-tertiary clmc-mb-small">Identidad</p>
      <p class="clmc-text-xsmall clmc-color-success">3 documentos subidos</p>
    </div>
  </div>
  <svg class="MuiSvgIcon-root clmc-color-success" viewBox="0 0 24 24"><path d="M9 16.17 …"/></svg>
</button>
```

Variantes del estado, ya existentes en `components.css`:

| Clase | Estado |
|---|---|
| `clmc-user-document-icon-approved` | validado |
| `clmc-user-document-icon-pending` | pendiente |
| `clmc-user-document-icon-denied` | rechazado |
| `clmc-user-document-clickable` | la tarjeta abre el diálogo |
| `clmc-user-document-static` | la tarjeta no es accionable (caso de Email) |

Íconos en `/static/images/icons/verificacion/`:
`email_*`, `identify_*`, `bank_*`, `other_*`, cada uno en `_pending` y `_verified`.
Hoy **no existe** un `*_denied`, aunque la clase de ícono rechazado sí está en el CSS.

### Diálogo de carga de documentos

```
.MuiDialog-root.clmc-dialog
└── .MuiDialog-paper.MuiDialog-paperWidthSm
    ├── .MuiDialogTitle-root
    │   └── .clmc-flex.clmc-row.clmc-align-center.clmc-justify-between
    │       ├── p.clmc-text-500.clmc-text-large.clmc-mb-medium   título
    │       └── p.clmc-closeIcon-popup                           cerrar
    └── .MuiDialogContent-root
        └── .clmc-upload-documents-form
            └── form.clmc-w100
                ├── p.clmc-upload-documents-intro
                ├── .clmc-upload-documents-info-box  ×N   bloques de requisitos
                │   └── ul.clmc-upload-documents-list      lista de documentos válidos
                ├── .MuiFormControl-root.clmc-select-label  selector de tipo de documento
                ├── .MuiAlert-root.MuiAlert-colorWarning    aviso de bloqueo
                └── .clmc-upload-documents-actions.clmc-upload-documents-actions-floating
                    └── button.clmc-btn-primary.clmc-w100
```

Clases de la zona de carga (en `profile.css`, no visibles con la cuenta ya validada):
`clmc-upload-document-drop-area`, `-drop-placeholder`, `-drop-icon`, `-selected`,
`-success-icon`, `-filename`, `-status-card`, `-status-icon`, `-status-filename`,
`-status-label`, `-status-label-validating`, más `clmc-upload-documents-grid`,
`-format-box`, `-format-icon`, `-validation-alert` y `clmc-upload-documents-submit-enabled`.

---

## Hallazgos que afectan al diseño

1. **El diálogo de identidad pide RFC.** Está como tercer bloque de requisitos, junto
   con selfie y documento de identidad. El prototipo no lo contemplaba porque era una
   de las dudas abiertas. Queda confirmado: sí aplica.

2. **Hay un selector de tipo de documento** ("Por INE") dentro del diálogo de identidad.
   El prototipo ya lo tiene, y coincide.

3. **El orden actual de la rejilla es** Email · Identidad · Cuenta bancaria · Domicilio.
   El flujo nuevo lo cambia a Email · Identidad · Domicilio · Cuenta bancaria. Es un
   reordenamiento del arreglo, no un componente nuevo.

4. **No existe pestaña de preguntas frecuentes** en el perfil. Las subpestañas reales
   son Cuenta · Verificación · Contraseña. La pestaña de FAQ del prototipo es nueva y
   hay que darla de alta.

5. **El bloque de Truora ya está construido** con el badge "Recomendado" y el botón de
   ancho completo. Nuestra propuesta ajusta tipografía y espaciado, no la estructura.

6. **No hay nada de estados intermedios de cuenta** en el markup actual: ni tracker de
   pasos, ni estado pre-verificada, ni acordeón de alta de cliente. Esos tres sí son
   componentes nuevos.
