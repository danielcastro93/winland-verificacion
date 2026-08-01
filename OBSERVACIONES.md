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
- **El paso 3 deja de ser un requisito bloqueante** y se comunica como *mejora del método de cobro*, no como candado: paso con borde punteado (`.step.invite`), fila con la leyenda "Opcional · para recibir por depósito directo" y una tarjeta de invitación con el copy del cliente ("Sube tu información bancaria y olvídate de las filas").
- **Beneficio UX del cambio:** entrega una recompensa a mitad del recorrido (ya puedes cobrar) en lugar de exigir los tres documentos antes de cualquier retiro; y convierte el último paso en un incentivo, más fácil de vender que una obligación.

### 8.1 🔴 CONTRADICCIÓN A RESOLVER ANTES DE IMPLEMENTAR
El requerimiento recibido contiene **dos afirmaciones que se contradicen entre sí**, y de cuál sea la correcta depende **qué documento habilita el retiro en sala**:

| | Dice | Implicaría |
|---|---|---|
| **La justificación** | *"si el cliente solo cuenta con identidad verificada **y cuenta bancaria**, sus retiros únicamente se podrán realizar en salas físicas"* | Pre-verificada = identidad + **cuenta bancaria** |
| **El mensaje propuesto** | *"¡Tu cuenta ya está pre-verificada! … ¿Prefieres depósito directo? **Sube tu información bancaria**"* | Pre-verificada = identidad + **domicilio** (el banco aún falta) |

**Lectura aplicada en el prototipo:** pre-verificada = identidad + comprobante de **domicilio**. Es la única compatible con el mensaje que ellos mismos redactaron y con el cambio de orden solicitado (domicilio a paso 2).

**Riesgo si no se aclara:** implementar la regla al revés dejaría sin poder retirar a usuarios que hoy sí pueden, o al contrario. **Confirmar con el equipo antes de desarrollo.**

### 8.2 Hueco de UX: ¿a qué sala física acude el usuario?
El nuevo estado le dice al usuario que puede cobrar en salas físicas, pero **no le dice en cuál ni dónde**. Es la fricción más visible del modelo nuevo: se le pide un traslado sin darle la información para hacerlo.

**Recomendación:** agregar en la pantalla de cuenta pre-verificada un enlace a **sucursales/salas** (mapa o listado con horarios) y replicarlo en la FAQ de retiros. Si existe un localizador en el sitio, basta con enlazarlo.

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

## 10. Propuesta: página pública de salas (cierra el hueco del nuevo flujo + SEO local)

El modelo de cuenta pre-verificada le dice al usuario *"cobra en nuestras salas físicas"* **sin decirle en cuál ni dónde**. Se propone una página pública de salas, enlazada desde la pantalla de cuenta pre-verificada, desde la FAQ de retiros y desde el footer.

### 10.1 ¿No duplica el sitio corporativo (winlandcasino.com)?
Se diferencia por **a quién le habla**:
- **winlandcasino.com** es el sitio del *grupo*: quiénes somos, líneas de negocio, "soluciones para casinos". Le habla al mercado, al inversionista y al socio comercial; las sucursales aparecen como prueba de presencia.
- **winland.com.mx** le habla al *jugador*: dónde está el casino más cercano, cómo se ve, cómo llegar y qué puede resolver ahí (incluido cobrar un premio).

**Contenido y audiencia distintos ⇒ no compiten en buscadores, se complementan**, y pueden enlazarse entre sí.

**Además hay un argumento de conversión:** mandar a otro dominio a un usuario que está dentro de su cuenta, con un retiro pendiente, es una fuga de contexto justo en el momento de mayor intención.

### 10.1.1 Enfoque de la sección
La sección se llama **"Nuestros casinos"**, no "cobra tus premios": la mayoría de quien llegue busca **ubicaciones**, no cobrar. El cobro de premios queda como bloque informativo al final. Deliberadamente **no** se listan amenidades por sala (qué hay en cada casino): sería contenido a inventar o a mantener, y las fotografías comunican mejor la experiencia. Si más adelante operación quiere detallarlo, cada casino puede tener su propia página (lo que además multiplica las entradas orgánicas).

**Fotografías:** se usan las de las fachadas publicadas en winlandcasino.com, optimizadas para web. Confirmar con marketing que son las vigentes y que pueden usarse en winland.com.mx.

### 10.2 Valor SEO
- Son solo **5 ubicaciones**: un objetivo perfectamente alcanzable para dominar búsquedas locales de alta intención ("casino en Monterrey", "casino en Chihuahua", "salas de apuestas cerca de mí").
- Con marcado `LocalBusiness` (schema.org) cada sala puede aparecer en el **paquete local de Google Maps**, que es donde se decide a dónde ir.
- Lo que vive **detrás del login no es indexable**: por eso la página debe ser pública y el módulo de verificación solo enlazarla. (Criterio inverso al de la FAQ, que se definió privada a propósito.)
- Recomendación adicional: una página por sucursal (`/salas/monterrey`) multiplica las entradas orgánicas.

### 10.3 ⚠️ Hallazgo: dos salas no se llaman Winland
De las 5 sucursales, **La Paz y Hermosillo operan bajo la marca Fortune**. Si el flujo dice "cobra en nuestras salas Winland", un usuario de esas ciudades puede creer que no aplica para él.

**Acción:** el prototipo ya lo aclara explícitamente (badge de marca en cada sala + nota al pie). **Confirmar con marketing** si ambas marcas aceptan el cobro de retiros del casino online.

### 10.4 Implementación del mapa: por qué Leaflet y no Google Maps
El prototipo usa **Leaflet + tiles de OpenStreetMap/CARTO**:
- **Sin API key ni cuota.** Google Maps exige clave de facturación y cobra por carga de mapa; en una página pública con tráfico de SEO ese costo escala solo.
- **Open source y sin dependencia de proveedor**: los tiles se pueden cambiar (claro, oscuro, satélite) sin tocar la lógica.
- **Pines a medida** con la marca de cada sala (naranja Winland / negro Fortune), imposible de lograr igual con un iframe de Google Maps.
- Selección **sincronizada en ambos sentidos**: al tocar un pin se resalta su tarjeta y viceversa; el botón "Ver todo México" reencuadra las 5 salas.
- Si el usuario no tiene conexión al CDN, la página **sigue siendo usable**: el directorio con direcciones y los enlaces "Cómo llegar" no dependen del mapa.

Para producción, Calímaco puede conservar Leaflet tal cual (es la opción recomendada) o migrar a Google Maps si el sitio ya tiene contrato con esa API.

### 10.5 Pendientes de operación
- **Horarios de caja** de cada sala (el prototipo los marca como "por confirmar").
- **Requisitos exactos para cobrar**: se asumió identificación oficial + folio del retiro. Validar con operación y Legal.
- **Montos máximos** por cobro en sala, si aplican.
- **Coordenadas exactas** de cada sala: las del prototipo son aproximadas a nivel calle a partir de las direcciones publicadas en winlandcasino.com.
