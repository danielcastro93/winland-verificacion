# Winland — Módulo de Verificación de Cuenta (KYC) · Propuesta UX/UI

Prototipo funcional del rediseño del panel **Mi cuenta → Verificación** de winland.com.mx.

## Cómo verlo
Abrir `index.html` en un navegador (o servirlo con cualquier estático, ej. `python3 -m http.server`).
- Botones **‹ ›** (abajo-derecha) o flechas **← →** del teclado: recorren los 13 estados del flujo en orden.
- Toda la interfaz del módulo es funcional al clic (flujo Truora, modales de carga, FAQ, etc.).
- Responsive real: probar también en vista móvil (DevTools).

## Estructura
```
index.html            Prototipo completo (13 estados) + guía de integración comentada
assets/produccion/    Assets extraídos del sitio en producción (logo, iconos, fuente, CSS de referencia)
assets/infografias/   Guías visuales de Marketing usadas en modales y FAQ
docs/OBSERVACIONES.md Hallazgos y pendientes reportados al cliente
```

## Notas para desarrollo (Calímaco)
La guía completa de integración está comentada al inicio de `index.html`: mapeo de design-tokens a `clmc-*`,
qué se implementa y qué es solo réplica visual (header/footer), y los puntos de conexión a backend marcados con `API:`.
