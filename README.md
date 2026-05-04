# BWell Local

## Descripcion
`bwell_herramienta_local.html` es una herramienta local para captura clinica y preclasificacion operativa de pacientes en programas BWell. Corre directamente en el navegador, aplica reglas automaticas por formulario y permite generar PDF o CSV sin enviar datos a internet.

Este README consolida:
- Uso rapido
- Privacidad y controles tecnicos
- Resumen ejecutivo
- Reglas implementadas por formulario
- Cambios funcionales aplicados
- Manual de usuario para capacitacion

## Uso rapido
1. Abra `bwell_herramienta_local.html` en Chrome, Edge o Firefox.
2. Seleccione el formulario correspondiente.
3. Complete `Poliza`, `Certificados` y los datos clinicos requeridos.
4. Presione `Es Candidato?` para calcular el resultado.
5. Marque la confirmacion de base legal/consentimiento si va a generar PDF.
6. Use `Crear PDF / imprimir` para guardar el documento.
7. Use `Generar CSV` si necesita una salida tabular local.
8. Use `Limpiar campos` al finalizar.

## Privacidad aplicada
- No hay formulario HTML con submit tradicional.
- No se usa `localStorage` ni `sessionStorage`.
- No se incluye telemetria ni librerias externas.
- La CSP bloquea conexiones de red con `connect-src 'none'` y `form-action 'none'`.
- El JavaScript deshabilita `fetch`, `XMLHttpRequest` y `sendBeacon`.
- El payload minimo opcional no contiene datos personales ni texto clinico libre.
- El bloque de payload y los botones asociados quedaron ocultos para usuarios finales.

## Informe ejecutivo

### Estado general
La herramienta funciona 100% en el navegador. Su objetivo es apoyar la captura clinica, aplicar reglas automaticas visibles y generar evidencia documental local sin exponer informacion a servicios externos.

### Formularios incluidos
- Cardiometabolico
- Gastro
- Articular y espalda

### Cambios funcionales relevantes
- El boton principal de calculo visible es `Es Candidato?`
- `No aplica BWell` se muestra en color naranja `#FF9933`
- Las reglas positivas resaltan opciones en verde `#CCFF99`
- Ese resaltado verde se conserva tambien en impresion/PDF
- En `Articular y espalda`, el calculo exige seleccionar `Paquete BWell`

### Alcance operativo
La herramienta no sustituye el criterio medico. Estandariza la captura y reduce variaciones operativas en la aplicacion de criterios visibles.

### Limitaciones
- Las reglas son deterministicas y dependen de campos seleccionados
- Algunos campos informativos no participan en la logica automatica
- Si cambian los criterios medicos, se debe actualizar el HTML

## Reglas generales
- `Poliza` es obligatoria y numerica
- `Certificados` es obligatorio y numerico
- Para crear PDF se debe confirmar la base legal/consentimiento aplicable
- El calculo se ejecuta con `Es Candidato?`

## Reglas por formulario

## 1. Cardiometabolico

### Candidato BWell
Se clasifica como `Candidato BWell` si se cumple al menos una de estas condiciones:
- Hay al menos un diagnostico en `Diagnosticos confirmados`
- Hay al menos un elemento marcado en `Cardiovasculares`
- En `Endocrino-metabolicos` se marca `Higado graso no alcoholico`, `Hipotiroidismo / Hipertiroidismo` o `Acido urico alto / Gota`
- En `Medicamentos` se marca `Antihipertensivos`, `Hipoglucemiantes orales / Insulina` o `Hipolipemiantes`

Si no se cumple ninguna de esas condiciones, devuelve `No aplica BWell`.

### Diagnosticos mostrados en el resultado
El detalle del resultado incluye lo marcado en:
- Diagnosticos confirmados
- Cardiopatia isquemica
- Insuficiencia cardiaca
- Evento cerebrovascular
- Infarto agudo de miocardio
- Arritmias
- Higado graso no alcoholico
- Acido urico alto / Gota

### Automatismos entre tratamientos y diagnosticos
- `Antihipertensivos` marca automaticamente `Hipertension arterial`
- `Hipolipemiantes` marca automaticamente `Dislipidemia`
- `Hipoglucemiantes orales / Insulina` marca automaticamente:
  - `Diabetes mellitus tipo 1`
  - `Diabetes mellitus tipo 2`
  - `Resistencia a la insulina`

### Resaltado verde
Se pintan en verde `#CCFF99` estas opciones cuando estan marcadas:
- Cardiopatia isquemica
- Insuficiencia cardiaca
- Evento cerebrovascular
- Infarto agudo de miocardio
- Arritmias
- Higado graso no alcoholico
- Acido urico alto / Gota
- Hipertension arterial
- Diabetes mellitus tipo 2
- Sindrome metabolico
- Diabetes mellitus tipo 1
- Dislipidemia
- Resistencia a la insulina

## 2. Gastro

### No ingreso
Si se marca cualquier signo de alarma, el resultado es:
- `NO debe ingresar al programa BWell Gastro y se debera referir urgente a gastroenterologia`

Los signos de alarma son:
- Perdida de peso involuntaria
- Dolor nocturno
- Vomitos persistentes
- Anemia ferropenica
- Sangrados digestivos altos o bajos
- Sospecha de lesion maligna
- Seguimiento de ulcera gastrica/duodenal no cicatrizada
- Sospecha de complicaciones por reflujo: estenosis, Barrett
- Sospecha de enfermedad intestinal inflamatoria

### Candidato BWell
Si no hay signos de alarma, se clasifica como `Candidato BWell` cuando se activa al menos una de estas reglas:
- `Dolor abdominal` en `Ocasional`, `Frecuente` o `Diario`
- `Distension abdominal o gases` en `Ocasional`, `Frecuente` o `Diario`
- `Acidez o reflujo` en `Ocasional`, `Frecuente` o `Diario`
- Alguna opcion en `Cambios en el habito intestinal`
- `Estres empeora` en `Si` u `Ocasional`
- `Actualmente toma medicamentos` en `Si`
- `Frecuencia de medicamentos` en `Todos los dias` o `A veces durante la semana`
- `Sus sintomas interfieren con su trabajo o actividades diarias` en `Mucho`
- `Evita ciertos alimentos, actividades o eventos sociales por sus sintomas` en `Si`
- `Criterio final de ingreso al programa` en `Si`

Si no hay signos de alarma y no se activa ninguna regla, devuelve `Sin criterio automatico registrado`.

### Resaltado verde
Se pintan en verde `#CCFF99` estas condiciones cuando estan marcadas:
- Dolor abdominal: `Ocasional`, `Frecuente`, `Diario`
- Distension abdominal o gases: `Ocasional`, `Frecuente`, `Diario`
- Acidez o reflujo: `Ocasional`, `Frecuente`, `Diario`
- Cambios en el habito intestinal: cualquier opcion
- Estres empeora: `Si`, `Ocasional`
- Actualmente toma medicamentos: `Si`
- Frecuencia de medicamentos: `Todos los dias`, `A veces durante la semana`
- Interfiere con actividades diarias: `Mucho`
- Evita alimentos o actividades: `Si`
- Criterio final de ingreso: `Si`

## 3. Articular y espalda

### No ingreso
Si se marca cualquier bandera roja neurologica, el resultado es:
- `NO debe ingresar al programa BWELL CLINICA ARTICULAR Y ESPALDA`

Banderas rojas:
- Deficit motor
- Alteraciones sensitivas
- Compromiso neurologico

### Candidato BWell
Si no hay banderas rojas, se clasifica como `Candidato BWell` cuando se marca al menos una opcion en:
- `Diagnosticos osteoarticulares o de columna confirmados`
- `Osteoarticulares / neuromusculoesqueleticos`

Si no hay banderas rojas y no se marca ninguna de esas opciones, devuelve `Sin criterio automatico registrado`.

### Validacion adicional
Para calcular en este formulario es obligatorio seleccionar una opcion en `Paquete BWell`.

Si falta esa seleccion, aparece:
- `Debe seleccionar una opcion en Paquete BWell para continuar.`

### Resaltado verde
Se pintan en verde `#CCFF99` estas opciones cuando estan marcadas:
- Lumbalgia mecanica / cronica
- Cervicalgia
- Dorsalgia
- Hernia discal cervical / dorsal / lumbar
- Radiculopatia
- Artrosis
- Artritis inflamatoria
- Lesion meniscal / condropatia
- Tendinopatias
- Bursitis
- Sindrome miofascial
- Escoliosis
- Cirugias previas de columna o articulaciones
- Lesiones deportivas previas
- Fracturas previas

## Cambios de interfaz aplicados
- Boton principal cambiado de `Calcular resultado` a `Es Candidato?`
- `No aplica BWell` mostrado en naranja `#FF9933`
- Payload oculto al usuario
- Botones `Copiar payload minimo` y `Descargar payload minimo` ocultos
- Resaltados verdes visibles tambien en PDF/impresion

## Manual de usuario

### Objetivo del usuario final
Completar el formulario, validar si la persona es candidata segun las reglas vigentes y generar el PDF o CSV localmente.

### Flujo recomendado
1. Abrir `bwell_herramienta_local.html`
2. Seleccionar el tab correcto
3. Completar `Poliza`, `Certificados` y `Datos generales`
4. Completar la informacion clinica
5. Verificar si hay campos resaltados en verde
6. En `Articular y espalda`, seleccionar un `Paquete BWell`
7. Presionar `Es Candidato?`
8. Revisar el resultado mostrado
9. Marcar consentimiento/base legal si va a imprimir
10. Presionar `Crear PDF / imprimir`
11. Usar `Generar CSV` si aplica
12. Presionar `Limpiar campos` al terminar

### Interpretacion de resultados
- `Candidato BWell`: se activo al menos una regla positiva
- `No aplica BWell`: no se activo ninguna regla positiva
- `NO debe ingresar...`: se activo una regla de exclusion o referencia

### Reglas practicas para capacitacion
- No avanzar sin `Poliza` y `Certificados`
- No imprimir sin consentimiento marcado
- En `Articular y espalda`, no avanzar sin `Paquete BWell`
- El verde identifica criterios positivos
- El naranja identifica `No aplica BWell`

### Preguntas frecuentes

#### Por que no me deja calcular?
- Falta `Poliza`
- Falta `Certificados`
- Alguno de esos campos no es numerico
- En `Articular y espalda`, falta `Paquete BWell`

#### Por que no me deja crear el PDF?
- Falta marcar la confirmacion de consentimiento/base legal aplicable

#### Por que algunas opciones se ponen verdes?
- Porque forman parte de reglas positivas de candidatura

#### Por que no veo el payload?
- Porque fue ocultado para uso operativo de usuarios finales

## Guion sugerido para capacitacion

### Apertura
- Explicar que la herramienta trabaja localmente y no transmite informacion
- Aclarar que es apoyo operativo y no reemplaza criterio medico

### Demostracion
1. Abrir el HTML
2. Explicar los tabs
3. Hacer un ejemplo de Cardiometabolico
4. Mostrar los resaltados verdes
5. Ejecutar `Es Candidato?`
6. Generar PDF
7. Mostrar un caso Gastro con signo de alarma
8. Mostrar un caso Articular y espalda con la validacion de `Paquete BWell`

### Cierre
- Reforzar que las reglas dependen de la version actual del HTML
- Recordar que cualquier cambio medico requiere actualizacion tecnica

## Macros del Excel revisadas
- Crear PDF: exportaba la hoja activa como PDF y sugeria nombre con hoja + paciente + fecha/hora
- Limpiar campos: limpiaba celdas no bloqueadas y respetaba formulas
- Construir diagnostico/criterio:
  - Cardiometabolico: lista diagnosticos confirmados y calcula candidato segun reglas de la hoja
  - Gastro: signos de alarma impiden ingreso y derivan a gastroenterologia; sintomas, tratamiento y calidad de vida activan candidato
  - Clinica Articular y Espalda: banderas neurologicas impiden ingreso; diagnosticos y antecedentes seleccionados activan candidato

## Nota legal operativa
Esta herramienta no sustituye revision legal. Al capturar datos de salud, valide con su responsable de privacidad/compliance la base legal, consentimiento informado, aviso de privacidad, custodia del PDF generado, retencion, acceso, auditoria y eliminacion segura.
