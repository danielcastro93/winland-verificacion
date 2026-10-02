# Observaciones para el cliente — Módulo de Verificación Winland
*(Para incluir como sección final de la presentación. Última actualización: 30-jul-2026)*

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

## 7. Lógica del flujo: bloqueo secuencial y vías de identidad (decisión de diseño)
Cómo se comporta la propuesta, para que el equipo tenga la regla de negocio explícita:

- **La identidad es el ancla del proceso.** Cuenta bancaria y domicilio se validan *contra* la identidad confirmada (el nombre del titular debe coincidir). Por eso ambos pasos permanecen **bloqueados** —con candado y la leyenda "Disponible después de validar tu identidad"— hasta que el Paso 1 quede validado. Se desbloquean automáticamente al validarse la identidad.
  - *Por qué se bloquea y no es mala UX:* evita validar documentos en el vacío, evita trabajo tirado si la identidad se rechaza, y deja un solo "siguiente paso" claro (menor carga cognitiva y menor abandono).
- **Dos vías para validar identidad, ambas respetan el bloqueo:**
  - **Truora (biométrica):** síncrona (~5 min). La pantalla de resultado dice "Identidad validada con Truora".
  - **Manual (carga de INE + selfie):** por el acordeón de alternativas. La pantalla de resultado dice "Identidad validada manualmente". El prototipo adapta banner, tracker, fila de identidad y los fondos de los modales según la vía usada.
- **Recomendación UX para la vía manual (a validar con el equipo):** la revisión manual **no es instantánea** (la revisa una persona). Para no obligar al usuario a adivinar cuándo volver, agregar un **aviso por correo cuando la identidad manual quede aprobada**, invitándolo a regresar a subir cuenta bancaria y domicilio. En el prototipo la vía manual se muestra resuelta al momento solo para poder demostrar el flujo completo.
  - *Alternativa fase 2:* permitir subir los 3 documentos en una sola sesión sin importar el orden y aplicar el bloqueo solo al **resultado** (activar retiros), validando en backend en el orden que corresponda. Reduce sesiones múltiples a costa de más lógica de backend.
- **Cierre de modales:** cerrar un modal de documentos (identidad/banco/domicilio) regresa a la pantalla previa; **solo** cerrar el modal biométrico de Truora lleva a "Verificación interrumpida", porque ahí sí se abandonó un proceso en curso.

## 8. Nuevo modelo de niveles de verificación (feedback del cliente, jul-2026)

El flujo pasa de dos niveles (verificado / no verificado) a **tres**, y el orden de los pasos cambia:

| Nivel | Documentos | Qué habilita |
|---|---|---|
| Sin verificar | — | No puede retirar |
| **Pre-verificada** | Identidad + **Comprobante de domicilio** | Retiros **cobrados en salas físicas** |
| Verificada | + **Cuenta bancaria** | Retiros por **depósito directo** (SPEI) |

- **Orden nuevo de pasos:** 1 Identidad · 2 Comprobante de domicilio · 3 Cuenta bancaria.
- **El paso 3 deja de ser un requisito bloqueante** y se comunica como *mejora del método de cobro*, no como candado: paso 3 en naranja atenuado (`.wl-step.invite`) con la leyenda "Cobra sin ir a sala" y una tarjeta de invitación con el copy del cliente ("Sube tu información bancaria y olvídate de las filas").
- **Beneficio UX del cambio:** entrega una recompensa a mitad del recorrido (ya puedes cobrar) en lugar de exigir los tres documentos antes de cualquier retiro; y convierte el último paso en un incentivo, más fácil de vender que una obligación.

### 8.1 Definición del disparador de "pre-verificada" (CERRADO)

El requerimiento original admitía dos lecturas y no coincidían entre sí: la
justificación hablaba de identidad **y cuenta bancaria**, mientras que el mensaje
redactado por el propio cliente decía *"sube tu información bancaria"*, lo que
implica que el banco todavía no está.

**Resuelto el 1 de octubre de 2026.** Queda la lectura aplicada en el prototipo:

> **pre-verificada = identidad aprobada + comprobante de domicilio aprobado**

Es la única compatible con el mensaje del cliente y con el cambio de orden que
pidieron (domicilio a paso 2). Esta es la condición que debe programarse.

### 8.2 Hueco de UX asumido por el cliente

El estado pre-verificada le dice al usuario que puede cobrar en salas físicas, pero
no le dice en cuál ni dónde. Se propuso resolverlo con una sección de sucursales
enlazada desde ese mensaje.

**El cliente decidió descartarlo** (1 de octubre de 2026) y dejar la frase como texto
plano, sin enlace. Queda registrado como fricción conocida del modelo, no como
pendiente nuestro.

### 8.3 Decisión de diseño a validar
La cuenta bancaria (paso 3) **se desbloquea al validar la identidad**, igual que el domicilio: la numeración 2/3 comunica prioridad, no un candado adicional. Bloquearla hasta tener el domicilio retrasaría justo el objetivo de negocio (que el usuario registre su banco y deje de cobrar en sala).

## 9. Formato de alta de cliente (menú colapsable)

Se integró como **módulo paralelo al KYC**, no como un cuarto paso del tracker: acordeón propio debajo del de verificación manual.

- **Nace bloqueado** ("Formato de alta de cliente · próximamente"), con candado. Se puede **expandir** para entender de qué se trata —así no queda una caja muerta— pero su botón está deshabilitado.
- Al **habilitarlo desde Back Office**, el acordeón pierde el candado y su botón abre el modal introductorio de firma, cuyo CTA entrega el control al proveedor de firma digital.
- En el prototipo son navegables **ambos estados** (bloqueado y activo) para poder mostrar cómo se verá al activarlo.

**Definiciones pendientes con el equipo:**
1. **Proveedor de firma digital**: ¿es un servicio externo (tipo Mifiel/DocuSign), pantalla propia o pestaña nueva? Define si el handoff es modal, redirección o ventana emergente.
2. **¿Es obligatorio?** Se asumió que **no** bloquea retiros ni forma parte de los tres pasos. Si llegara a ser requisito, tendría que integrarse al tracker y replantearse el flujo.
3. **Criterio de activación**: ¿se habilita para todos los usuarios o solo para los que Back Office marque?
4. **Estado posterior a la firma**: qué se muestra cuando el formato ya está firmado (acuse descargable, sello de "firmado", etc.).

## 10. Sección pública de salas (DESCARTADA)

Se propuso una sección pública de casinos con mapa y SEO local, para cerrar el hueco
descrito en 8.2. **El cliente la descartó el 1 de octubre de 2026**: el alcance se
acotó a verificación y preguntas frecuentes.

El trabajo quedó archivado en `_backup/` por si se retoma. Fuera del prototipo, de la
entrega y del repositorio.

## 11. Preguntas frecuentes autorizadas (1 de octubre de 2026)

El cliente envió el listado autorizado y reemplaza por completo al que se había
redactado en la propuesta. Se integró **tal cual**, respetando sus seis categorías y
su orden; lo único que se agregó es el acceso a la infografía en las cinco preguntas
donde ya existe una.

Se corrigieron acentos faltantes del original ("credito", "atencion", "deposito",
"podras"). Es corrección mecánica, no de contenido.

Tres cosas quedan anotadas, sin acción de nuestra parte porque el contenido viene
autorizado:

1. **El listado se contradice con el flujo nuevo.** En *"¿Cómo puedo retirar?"* pide
   *"contar con su verificación completa"*, pero el estado pre-verificada existe
   justamente para permitir el cobro sin verificación completa. Dos filas arriba,
   *"¿Cuáles son las opciones de retiro?"* sí incluye *"directamente en punto
   físico"*, que es lo que habilita pre-verificada.

2. **Los comprobantes de domicilio no coinciden con la infografía vigente.** El texto
   autorizado dice *agua, luz o teléfono*; la infografía dice *luz (CFE), gas natural
   y teléfono/internet*. El agua aparece en uno y no en el otro; el gas, al revés.

3. **Sigue el error de la infografía de domicilio**: "Credencial del Seguro Social"
   aparece repetida en la columna de no aceptables (ver 1.1). Las infografías
   corregidas no llegaron con el listado.

**Administración del contenido:** el cliente pidió que las preguntas sean editables
desde el backoffice, porque cambian con frecuencia. Lo ve directamente Andrés Arango
con Calímaco. El componente ya está construido sin asumir cuántas preguntas hay ni
qué tan largas son; lo que sí hay que considerar es que el campo de respuesta debe
admitir una imagen asociada, no solo texto plano.
