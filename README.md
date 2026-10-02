# Winland — Módulo de verificación de cuenta (KYC) · Propuesta UX/UI

Prototipo funcional del rediseño del panel **Mi cuenta → Verificación** de winland.com.mx.

## Cómo verlo

Abrir `index.html` en un navegador. No necesita servidor ni conexión: no quedan
dependencias externas.

- Botones **‹ ›** (abajo a la derecha) o flechas **← →** del teclado: recorren los
  15 estados del flujo en orden, incluidos los caminos de error.
- Toda la interfaz del módulo responde al clic: flujo de Truora, modales de carga,
  acordeones y preguntas frecuentes.
- Responsive real: conviene probarlo también en vista móvil.

## Estructura

```
index.html                 Prototipo completo (15 estados) + guía de integración comentada
chrome.css                 Pegamento entre el chrome real del sitio y el módulo
assets/produccion/         Assets extraídos del sitio en producción (logo, fuente, íconos)
assets/infografias/        Guías visuales de Marketing, usadas en modales y FAQ
assets/meta/               Favicon e imagen para compartir la liga del prototipo
assets/*.css               Hojas reales del header, footer y menú inferior
docs/OBSERVACIONES.md      Hallazgos y pendientes reportados al cliente
docs/calimaco/             Entrega técnica para el equipo de desarrollo
_backup/                   Versiones previas y archivos fuera de uso (no se versiona)
```

## Qué va en la carpeta para Calímaco

Todo lo que está versionado en este repositorio, y nada más. Fuera de la entrega
quedan `_backup/` (casinos, versiones previas, íconos originales, assets sin uso),
`_pres/` (material de presentación), `.claude/` (herramientas locales) y cualquier
archivo de sistema. Para armarla limpia:

```
git archive --format=zip -o winland-verificacion.zip HEAD
```

## Para desarrollo

Todo lo que necesita Calímaco está en **[`docs/calimaco/README.md`](docs/calimaco/README.md)**:
tokens a dar de alta, el delta de CSS, el mapa de qué componentes ya existen y
cuáles son nuevos, y las reglas de negocio que debe exponer el backend.

Dos convenciones que conviene tener presentes al leer el código:

- **`.clmc-*`** — el componente ya existe en Calímaco; lo que está aquí es solo el delta.
- **`.wl-*`** — componente nuevo, hay que construirlo.

Los puntos de conexión a backend están marcados con `API:` en los comentarios.

## Alcance

Por indicación del cliente (1 de octubre de 2026) este entregable cubre
**verificación y preguntas frecuentes**. La propuesta de sección pública de casinos
quedó descartada y está archivada en `_backup/`.
