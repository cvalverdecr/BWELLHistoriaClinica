# Guia BWell Local

## Objetivo del documento
Este documento resume el comportamiento actual de la herramienta local `bwell_herramienta_local.html` para uso operativo, validacion funcional y capacitacion de usuarios.

Incluye:
- Resumen ejecutivo
- Reglas implementadas por formulario
- Cambios funcionales relevantes aplicados
- Manual de usuario para la capacitacion

## Informe ejecutivo

### 1. Estado general de la herramienta
La herramienta funciona 100% en el navegador, sin enviar informacion a internet y sin dependencias externas. Su objetivo es apoyar la captura clinica, aplicar reglas automaticas de candidatura y permitir generar PDF o CSV de manera local.

### 2. Formularios disponibles
La version actual incluye tres formularios operativos:
- Cardiometabolico
- Gastro
- Articular y espalda

### 3. Decisiones funcionales relevantes
- El boton principal visible para calcular reglas es `Es Candidato?`
- El resultado `No aplica BWell` se muestra en color naranja `#FF9933`
- El bloque de payload y los botones `Copiar payload minimo` y `Descargar payload minimo` quedaron ocultos para usuarios
- Se agregaron resaltados visuales en verde `#CCFF99` para opciones que activan reglas de candidatura en los tres formularios
- Ese mismo resaltado verde se conserva tambien en impresion/PDF
- En `Articular y espalda`, el calculo requiere seleccionar un `Paquete BWell`

### 4. Alcance operativo
La herramienta no reemplaza el criterio medico. Automatiza reglas visibles y consistentes para apoyar:
- Preclasificacion del paciente
- Estandarizacion de captura
- Reduccion de errores operativos
- Generacion local de evidencia documental

### 5. Riesgos o limitaciones actuales
- Las reglas actuales dependen de selecciones puntuales, no de analisis clinico complejo
- Algunos campos informativos no participan en la logica automatica
- Si cambian los criterios medicos, el HTML debe actualizarse para reflejar la nueva politica

## Reglas por formulario

### Reglas generales para todos los formularios
- `Poliza` es obligatoria y debe ser numerica
- `Certificados` es obligatorio y debe ser numerico
- Para crear PDF se debe marcar el consentimiento o base legal aplicable
- El calculo se ejecuta con el boton `Es Candidato?`

## 1. Cardiometabolico

### Regla de candidatura
El formulario devuelve `Candidato BWell` si se cumple al menos una de estas condiciones:
- Hay al menos un diagnostico marcado en `Diagnosticos confirmados`
- Hay al menos un antecedente marcado en `Cardiovasculares`
- En `Endocrino-metabolicos` se marca `Higado graso no alcoholico`, `Hipotiroidismo / Hipertiroidismo` o `Acido urico alto / Gota`
- En `Medicamentos` se marca `Antihipertensivos`, `Hipoglucemiantes orales / Insulina` o `Hipolipemiantes`

Si no se cumple ninguna condicion anterior, devuelve `No aplica BWell`.

### Diagnostico mostrado en el resultado
Cuando se calcula el resultado, el detalle de diagnosticos muestra lo marcado en:
- `Diagnosticos confirmados`
- `Cardiopatia isquemica`
- `Insuficiencia cardiaca`
- `Evento cerebrovascular`
- `Infarto agudo de miocardio`
- `Arritmias`
- `Higado graso no alcoholico`
- `Acido urico alto / Gota`

### Reglas automaticas entre tratamientos y diagnosticos
Si en `Tratamientos actuales` se marcan ciertos medicamentos, se marcan automaticamente estos diagnosticos:
- `Antihipertensivos` marca `Hipertension arterial`
- `Hipolipemiantes` marca `Dislipidemia`
- `Hipoglucemiantes orales / Insulina` marca `Diabetes mellitus tipo 1`, `Diabetes mellitus tipo 2` y `Resistencia a la insulina`

### Resaltado verde en pantalla y PDF
Se pintan en verde `#CCFF99` cuando estan marcadas estas opciones:
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

### Regla de no ingreso
Si se marca al menos un signo de alarma, el resultado es:
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

### Regla de candidatura
Si no hay signos de alarma, el formulario devuelve `Candidato BWell` cuando se cumple al menos una de estas reglas:
- `Dolor abdominal` en `Ocasional`, `Frecuente` o `Diario`
- `Distension abdominal o gases` en `Ocasional`, `Frecuente` o `Diario`
- `Acidez o reflujo` en `Ocasional`, `Frecuente` o `Diario`
- Alguna opcion marcada en `Cambios en el habito intestinal`
- `Estres empeora` en `Si` u `Ocasional`
- `Actualmente toma medicamentos` en `Si`
- `Frecuencia de medicamentos` en `Todos los dias` o `A veces durante la semana`
- `Sus sintomas interfieren con su trabajo o actividades diarias` en `Mucho`
- `Evita ciertos alimentos, actividades o eventos sociales por sus sintomas` en `Si`
- `Criterio final de ingreso al programa` en `Si`

Si no hay signos de alarma y ninguna regla se activa, el resultado es `Sin criterio automatico registrado`.

### Resaltado verde en pantalla y PDF
Se pintan en verde `#CCFF99` cuando se marcan opciones que activan candidatura:
- Dolor abdominal: `Ocasional`, `Frecuente`, `Diario`
- Distension abdominal o gases: `Ocasional`, `Frecuente`, `Diario`
- Acidez o reflujo: `Ocasional`, `Frecuente`, `Diario`
- Cambios en el habito intestinal: cualquier opcion marcada
- Estres empeora: `Si`, `Ocasional`
- Actualmente toma medicamentos: `Si`
- Frecuencia de medicamentos: `Todos los dias`, `A veces durante la semana`
- Interfiere con actividades diarias: `Mucho`
- Evita alimentos o actividades: `Si`
- Criterio final de ingreso: `Si`

## 3. Articular y espalda

### Regla de no ingreso
Si se marca cualquier bandera roja neurologica, el resultado es:
- `NO debe ingresar al programa BWELL CLINICA ARTICULAR Y ESPALDA`

Las banderas rojas que bloquean el ingreso son:
- Deficit motor
- Alteraciones sensitivas
- Compromiso neurologico

### Regla de candidatura
Si no hay banderas rojas, el formulario devuelve `Candidato BWell` cuando se marca al menos una opcion en cualquiera de estos grupos:
- `Diagnosticos osteoarticulares o de columna confirmados`
- `Osteoarticulares / neuromusculoesqueleticos`

Si no hay banderas rojas y no se marca ninguna opcion en esos dos grupos, el resultado es `Sin criterio automatico registrado`.

### Validacion adicional obligatoria
Para poder usar `Es Candidato?` en este formulario, debe estar seleccionado un valor en `Paquete BWell`.

Si no se selecciona, la herramienta muestra:
- `Debe seleccionar una opcion en Paquete BWell para continuar.`

### Resaltado verde en pantalla y PDF
Se pintan en verde `#CCFF99` cuando estan marcadas estas opciones:
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

## Cambios de interfaz ya aplicados
- Boton principal actualizado de `Calcular resultado` a `Es Candidato?`
- Resultado `No aplica BWell` en color naranja `#FF9933`
- Ocultamiento del bloque payload al usuario
- Ocultamiento de los botones `Copiar payload minimo` y `Descargar payload minimo`
- Conservacion de resaltados verdes en PDF/impresion

## Manual de usuario

### Objetivo del usuario final
Completar el formulario correspondiente, validar si la persona es candidata segun las reglas vigentes y generar el PDF o CSV localmente.

### Requisitos previos
- Tener el archivo `bwell_herramienta_local.html`
- Abrirlo en Chrome, Edge o Firefox
- Tener a mano la informacion clinica necesaria del paciente

### Flujo de uso recomendado
1. Abrir `bwell_herramienta_local.html`
2. Seleccionar el tab correspondiente:
   - Cardiometabolico
   - Gastro
   - Articular y espalda
3. Completar primero:
   - Poliza
   - Certificados
   - Datos generales
4. Completar el resto de campos clinicos
5. Revisar si hay opciones resaltadas en verde
6. En `Articular y espalda`, confirmar que `Paquete BWell` tenga una opcion seleccionada
7. Presionar `Es Candidato?`
8. Revisar el resultado automatico mostrado en pantalla
9. Marcar la casilla de consentimiento/base legal si se va a generar PDF
10. Presionar `Crear PDF / imprimir` para guardar el documento
11. Si se requiere exportacion tabular, usar `Generar CSV`
12. Al finalizar, usar `Limpiar campos`

### Interpretacion basica de resultados
- `Candidato BWell`: se activo al menos una regla positiva del formulario
- `No aplica BWell`: no se activo ninguna regla positiva
- `NO debe ingresar...`: se activo una regla de exclusion o referencia

### Reglas practicas para la capacitacion
- No avanzar sin `Poliza` y `Certificados`
- No imprimir sin consentimiento marcado
- En `Articular y espalda`, no avanzar sin `Paquete BWell`
- El color verde identifica criterios positivos activados
- El color naranja identifica que no aplica BWell
- Si aparece un mensaje de exclusion, revisar banderas rojas o signos de alarma

### Preguntas frecuentes para usuarios

#### 1. Por que no me deja calcular?
Las causas mas probables son:
- Falta `Poliza`
- Falta `Certificados`
- Alguno de esos campos no es numerico
- En `Articular y espalda`, falta seleccionar `Paquete BWell`

#### 2. Por que no me deja crear el PDF?
La causa mas probable es que no se marco la confirmacion de consentimiento/base legal aplicable.

#### 3. Por que algunas opciones se ponen verdes?
Porque forman parte de reglas activas de candidatura dentro del formulario.

#### 4. Por que no veo el payload?
Porque fue ocultado para uso operativo de usuarios finales y evitar confusion en la interfaz.

## Guion sugerido para la capacitacion de manana

### Apertura
- Explicar que la herramienta trabaja localmente y no transmite informacion
- Aclarar que es un apoyo operativo, no reemplaza criterio medico

### Demostracion
1. Mostrar como abrir el HTML
2. Explicar la estructura por tabs
3. Llenar un caso ejemplo de Cardiometabolico
4. Mostrar como se activan colores verdes
5. Ejecutar `Es Candidato?`
6. Generar PDF
7. Repetir un ejemplo corto en Gastro con signo de alarma
8. Repetir un ejemplo en Articular y espalda mostrando la validacion de `Paquete BWell`

### Cierre
- Reforzar que los resultados dependen de las reglas implementadas hoy
- Recordar que cualquier ajuste de criterio debe traducirse en una actualizacion del HTML

## Recomendacion final
Usar este documento como referencia base de capacitacion, validacion funcional y control de cambios. Si el equipo medico modifica criterios de ingreso, este archivo debe actualizarse junto con la herramienta.
