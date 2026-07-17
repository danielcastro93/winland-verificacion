# Observaciones para el cliente — Módulo de Verificación Winland
*(Para incluir como sección final de la presentación. Última actualización: 16-jul-2026)*

## 1. Errores de contenido en las infografías (requieren corrección de Marketing)

### 1.1 Infografía "Comprobantes de domicilio" — error crítico
- **"Credencial del Seguro Social" aparece DUPLICADA** en la columna de "No aceptables" (dos veces el mismo elemento).
- Además es un elemento **fuera de contexto**: la credencial del IMSS es un "no aceptable" de *identificación*, no de *comprobante de domicilio*. Parece error de copy-paste de la infografía de identificación.
- Los "no aceptables" relevantes para domicilio serían: predial, contrato de arrendamiento, escrituras del catastro (que sí aparecen en versiones anteriores).
- **Estado en el prototipo**: la imagen se integró tal cual para mostrar el diseño; en cuanto se corrija, solo se sobrescribe el archivo (`infografias/guia-domicilio-tipos.jpg`) sin tocar código.

### 1.2 Inconsistencia: ¿"Agua" es comprobante válido o no?
- **Producción hoy** lista: "Agua, Predial, Internet, Luz".
- **La infografía nueva** lista: Luz (CFE), Gas natural, Teléfono/Internet — *sin Agua*.
- Confirmar con Legal/Operación la lista definitiva.

### 1.3 Inconsistencia: Predial (arrastrada de la revisión anterior)
- **Producción y el documento del proyecto** aceptan "Predial" como comprobante.
- **Las infografías** lo marcan explícitamente como NO aceptable ("no es un servicio mensual").
- Tercera fuente en conflicto sobre la misma lista → definir una sola fuente de verdad con Legal.

### 1.4 Encabezados inconsistentes en el set de infografías
- Unas dicen "Guía rápida de documentación" y otras "Guía rápida de requisitos de identificación" — incluso la de domicilio/estados de cuenta usa el encabezado de "identificación".
- Corregir al re-exportar (mismo lote que 1.1).

## 2. RFC en el modal de identidad de producción
- El modal actual de producción incluye un bloque de **RFC** ("el formato debe de ser PDF") como parte de la verificación de identidad.
- El rediseño contempla: frontal + reverso + selfie. **Confirmar si RFC sigue siendo requisito**; si sí, se agrega su zona de carga al modal.

## 3. Solicitud a Marketing: versiones móviles de las infografías
- Las 6 infografías actuales son formato panorámico/póster (~1081×906 px), pensadas para escritorio.
- En móvil (viewport ≤430px) el texto interno queda ilegible (~5-6px). El prototipo lo resuelve mostrando una tarjeta "Ver guía" que abre visor con zoom, pero la solución ideal es una **versión vertical apilada**:
  - **Layout**: "Aceptables" arriba → "No aceptables" abajo (una sola columna).
  - **Proporción sugerida**: 4:5 u 9:16 (ej. 1080×1350 px o 1080×1920 px).
  - **Tipografía mínima interna**: que el texto más pequeño no baje de ~28px en el archivo de 1080 de ancho (≈11px renderizados en pantalla de 390px).
  - Mantener la misma estandarización del set actual (mismo encabezado, misma estructura, mismos estilos).

## 4. Fuente tipográfica en versión TRIAL (hallazgo técnico, arrastrado)
- Producción carga `DenimINKCollectionVF-TRIAL.ttf` — versión de **prueba** de la fuente Denim INK.
- Riesgo de licenciamiento: confirmar que exista licencia comercial y desplegar el archivo licenciado.

## 5. Recomendación técnica fase 2: validación en el momento de la carga
- El rediseño ataca los rechazos **antes** de la carga (guías, requisitos, ejemplos). El mayor impacto restante está en validar **durante** la carga:
  - **Detección de calidad de imagen al subir**: borrosidad, resolución mínima, documento incompleto/recortado, reflejos — con aviso inmediato ("Esta imagen se ve borrosa, vuelve a tomarla") *antes* de enviar a revisión manual.
  - **Validación de selfie / prueba de vida asistida**: guiar la captura con la cámara (encuadre del rostro + documento a la altura de la barbilla) en lugar de aceptar un archivo libre. Truora ofrece SDKs de captura de documentos y prueba de vida que podrían reutilizarse también para el canal manual.
  - **Motivos de rechazo específicos y accionables**: cuando revisión rechace un documento, devolver la causa exacta (ej. "vigencia vencida") para mostrarla en el estado de rechazo ya maquetado.
- Beneficio esperado: reducir el ciclo subir→esperar→rechazo→re-subir, que es donde más se alarga el "time-to-verification" y más tickets de soporte se generan.

## 6. Comportamiento actual que el rediseño corrige (contexto para la presentación)
- Sin notificación al terminar la biometría con Truora (usuario re-sube documentos ya validados).
- Tile de identidad sin sincronizar en tiempo real → confusión y cargas duplicadas.
- Verificación manual con la misma jerarquía visual que Truora → 6 documentos subidos en el peor caso.
- Muros de texto en modales que los usuarios no leen → rechazos evitables.
